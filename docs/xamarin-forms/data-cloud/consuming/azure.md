---
title: Bir Azure mobil uygulaması kullanma
description: Azure Mobile Apps ile ölçeklenebilir arka uçlarını desteği Mobil kimlik doğrulama, çevrimdışı eşitleme ve anında iletme bildirimleri ile Azure App Service içinde barındırılan uygulamalar geliştirmesine olanak sağlar. Yalnızca bir Node.js arka ucu kullanan Azure mobil uygulamalar için geçerlidir, bu makalede, sorgu, Ekle, Güncelleştir ve Azure Mobile Apps örneği tablosunda depolanan verileri silme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: e2e9e2c05d3f6e467fd47b31af4f53049aab2709
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-an-azure-mobile-app"></a>Bir Azure mobil uygulaması kullanma

_Azure Mobile Apps ile ölçeklenebilir arka uçlarını desteği Mobil kimlik doğrulama, çevrimdışı eşitleme ve anında iletme bildirimleri ile Azure App Service içinde barındırılan uygulamalar geliştirmesine olanak sağlar. Yalnızca bir Node.js arka ucu kullanan Azure mobil uygulamalar için geçerlidir, bu makalede, sorgu, Ekle, Güncelleştir ve Azure Mobile Apps örneği tablosunda depolanan verileri silme açıklanmaktadır._

Xamarin.Forms tarafından kullanılabilecek bir Azure Mobile Apps örneği oluşturma hakkında daha fazla bilgi için bkz: [Xamarin.Forms uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/). Bu yönergeleri uyguladıktan sonra indirilebilir örnek uygulama ayarlayarak Azure Mobile Apps örneği kullanmak üzere yapılandırılabilir `Constants.ApplicationURL` Azure Mobile Apps örneğinin URL. Örnek uygulamayı çalıştırdığınızda, daha sonra bunu Azure Mobile Apps örneğine aşağıdaki ekran görüntüsünde gösterildiği gibi bağlanacak:

![](azure-images/portal.png "Örnek uygulama")

Azure Mobile Apps erişimi olan aracılığıyla [Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), ve Azure Xamarin.Forms örnek uygulamadaki tüm bağlantılar HTTPS üzerinden yapılır.

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="consuming-an-azure-mobile-app-instance"></a>Bir Azure mobil uygulaması örneği kullanma

[Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) sağlar `MobileServiceClient` sınıfı bir Xamarin.Forms uygulaması tarafından Azure Mobile Apps örneğine erişmek için aşağıdaki kod örneğinde gösterildiği gibi kullanılır:

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

Zaman `MobileServiceClient` örneği oluşturulduğunda, Azure Mobile Apps örneği tanımlamak için bir uygulama URL'si belirtilmelidir. Bu değer mobil uygulamada panodan elde edilebilir [Microsoft Azure portalı](https://portal.azure.com/).

Bir başvuru `TodoItem` Azure Mobile Apps örneğinde depolanan tablo gerekir elde edilebilir Bu tablo işlemleri gerçekleştirilmeden önce. Bu çağırarak sağlanır `GetTable` yöntemi `MobileServiceClient` örneği, döndüren bir `IMobileServiceTable<TodoItem>` başvuru.

### <a name="querying-data"></a>Veri sorgulama

Bir tablo içeriğini çağırarak alınabilir `IMobileServiceTable.ToEnumerableAsync` zaman uyumsuz olarak sorgu değerlendirir ve sonuçları döndüren yöntemi. Verileri de olabilir ekleyerek sunucu tarafı filtrelenmiş bir `Where` sorgusunda yan tümcesi. `Where` Yan tümcesi aşağıdaki kod örneğinde gösterildiği gibi bir satır, Tablo sorgusu koşulu filtreleme uygulanır:

```csharp
public async Task<ObservableCollection<TodoItem>> GetTodoItemsAsync (bool syncItems = false)
{
  ...
  IEnumerable<TodoItem> items = await todoTable
              .Where (todoItem => !todoItem.Done)
              .ToEnumerableAsync ();

  return new ObservableCollection<TodoItem> (items);
}
```

Bu sorgunun döndürdüğü tüm öğeleri `TodoItem` özelliği tablo `Done` özelliği eşittir `false`. Sorgu sonuçları sonra yerleştirilir bir `ObservableCollection` görüntülemek için.

### <a name="inserting-data"></a>Veri ekleme

Verileri Azure Mobile Apps örneğinde eklerken, yeni sütunlar otomatik olarak oluşturulacak bu dinamik şema Azure Mobile Apps örneğinde etkin gerektiği gibi tabloda sağlanan. `IMobileServiceTable.InsertAsync` Yöntemi yeni bir veri satırı belirtilen tabloya yerleştirmek için kullanılan aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

Bir ekleme isteği yapılırken bir kimliği Azure Mobile Apps örneğine geçirilen verilerde belirtilmemelidir. Ekleme isteği kimliği içeriyorsa, bir `MobileServiceInvalidOperationException` oluşturulur.

Sonra `InsertAsync` yöntemi tamamlayan, Azure Mobile Apps örnek verileri Kimliğini doldurulur `TodoItem` Xamarin.Forms uygulaması örneği.

### <a name="updating-data"></a>Verileri güncelleştirme

Azure Mobile Apps örnek verileri güncelleştirilirken yeni sütunlar otomatik olarak oluşturulacak bu dinamik şema Azure Mobile Apps örneğinde etkin gerektiği gibi tabloda sağlanan. `IMobileServiceTable.UpdateAsync` Yöntemi mevcut verileri yeni bilgilerle güncelleştirmek için kullanılan aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

Azure Mobile Apps örnek güncelleştirilecek veri bulabilmeniz bir güncelleştirme isteği yapılırken bir kimliği belirtilmelidir. Bu kimlik değeri depolanan `TodoItem.ID` özelliği. Güncelleştirme isteği güncelleştirilmesi olanaksız verileri belirlemek Azure Mobile Apps örneği için bir kimlik içermiyorsa ve böylece bir `MobileServiceInvalidOperationException` oluşturulur.

### <a name="deleting-data"></a>Verileri silme

`IMobileServiceTable.DeleteAsync` Yöntemi bir Azure Mobile Apps tablodan veri silmek için kullanılan aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

Azure mobil uygulaması sinstance silinecek veri bulabilmeniz bir silme isteği yaparken bir kimliği belirtilmelidir. Bu kimlik değeri depolanan `TodoItem.ID` özelliği. Silme isteği kimliği içermiyorsa, silinecek verilerin belirlemek Azure Mobile Apps örneğinin yolu yoktur ve bu nedenle bir `MobileServiceInvalidOperationException` oluşturulur.

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağı açıklanmıştır [Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) sorgu, Ekle, Güncelleştir ve Azure Mobile apps örneği tablosunda depolanan verileri silmek için. SDK sağlar `MobileServiceClient` Azure Mobile Apps örneğine erişmek için bir Xamarin.Forms uygulaması tarafından kullanılan sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoAzure (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Xamarin.Forms uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
