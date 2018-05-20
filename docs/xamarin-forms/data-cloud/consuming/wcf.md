---
title: Bir Windows Communication Foundation (WCF) Web hizmeti kullanma
description: WCF hizmet odaklı uygulamalar oluşturmak için Microsoft'un birleşik çerçevedir. Geliştiricilerin güvenli, güvenilir, işlenen ve birlikte çalışabilir dağıtılmış uygulamalar oluşturmalarını sağlar. Bu makalede, bir Xamarin.Forms uygulaması WCF Basit Nesne Erişim Protokolü (SOAP) hizmetinden kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 23cdc1871511fa75ba2686213d135822ca0fb971
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2018
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Bir Windows Communication Foundation (WCF) Web hizmeti kullanma

_WCF hizmet odaklı uygulamalar oluşturmak için Microsoft'un birleşik çerçevedir. Geliştiricilerin güvenli, güvenilir, işlenen ve birlikte çalışabilir dağıtılmış uygulamalar oluşturmalarını sağlar. Bu makalede, bir Xamarin.Forms uygulaması WCF Basit Nesne Erişim Protokolü (SOAP) hizmetinden kullanma gösterilmektedir._

WCF hizmet aşağıdakiler dahil farklı sözleşmeleri çeşitli ile açıklanmaktadır:

- **Veri sözleşmeleri** – bir ileti içinde içeriği temelini veri yapılarını tanımlar.
- **İleti sözleşmeleri** – mevcut veri sözleşmeleri iletilerden oluşturun.
- **Arıza sözleşmeleri** – özel SOAP hataları belirtilmesine izin verin.
- **Hizmet sözleşmeleri** – Destek Hizmetleri işlemleri belirtin ve iletileri gerekli her işlemi ile etkileşmek için. Bunlar ayrıca her hizmet işlemleri ile ilişkilendirilebilir herhangi özel hata davranışı belirtin.

ASP.NET Web Hizmetleri (ASMX) ve WCF arasındaki farklar vardır, ancak WCF ASMX sağlar – aynı yetenekleri HTTP üzerinden SOAP iletilerine desteklediğini anlamak önemlidir. ASMX hizmeti kullanma hakkında daha fazla bilgi için bkz: [kullanan ASP.NET Web Hizmetleri (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

Genel olarak, Xamarin platform Silverlight çalışma zamanı ile birlikte gelen WCF aynı istemci-tarafı alt destekler. Bu en yaygın kodlama ve protokolü uygulamaları WCF içerir — metin olarak kodlanmış SOAP iletileri HTTP üzerinden aktarım protokolünü kullanarak `BasicHttpBinding` sınıfı. Ayrıca, WCF Destek Araçları proxy oluşturmak için bir Windows ortamında yalnızca kullanılabilir kullanılmasını gerektirir.

WCF Hizmeti ayarlama yönergeleri örnek uygulama eşlik eden readme dosyasında bulunabilir. Örnek uygulamayı çalıştırdığınızda, ancak, bu veriler, salt okunur erişim sağlayan bir Xamarin barındırılan bir WCF hizmetini aşağıdaki ekran görüntüsünde gösterildiği gibi bağlanır:

![](wcf-images/portal.png "Örnek uygulama")

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Web hizmetini kullanma

WCF hizmeti aşağıdaki işlemleri sağlar:

|Çalışma|Açıklama|Parametreler|
|--- |--- |--- |
|GetTodoItems|Yapılacaklar öğelerini bir listesini alma|
|CreateTodoItem|Yeni bir Yapılacaklar öğesi oluşturma|Bir XML Todoıtem serileştirilmiş|
|EditTodoItem|Güncelleştirme Yapılacaklar öğesi|Bir XML Todoıtem serileştirilmiş|
|DeleteTodoItem|Yapılacaklar öğesi silme|Bir XML Todoıtem serileştirilmiş|

Uygulamada kullanılan veri modeli hakkında daha fazla bilgi için bkz: [modelleme verileri](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Örnek uygulaması web hizmetine salt okunur erişim sağlayan Xamarin barındırılan bir WCF Hizmeti kullanır. Bu nedenle, oluşturmak, güncelleştirmek ve verileri silme işlemleri uygulamada kullanılan verileri değiştirmez. Ancak, ASMX hizmet barındırılabilir bir sürümü kullanılabilir **TodoWCFService** eşlik eden örnek uygulama klasöründe. WCF hizmet verir tam barındırılabilir bu sürümü oluşturmak güncelleştirme okumak ve verilere erişim silin.

A *proxy* hizmetine bağlanmak uygulama izin veren bir WCF hizmeti kullanmak için oluşturulmuş olması gerekir. Proxy yöntemleri ve ilişkili hizmet yapılandırmasını tanımlar Süren hizmeti meta verileri tarafından oluşturulur. Bu meta veriler web hizmeti tarafından oluşturulan bir Web Hizmetleri Açıklama Dili (WSDL) belge şeklinde sunulur. Proxy web hizmeti için hizmet başvurusu .NET standart kitaplığa eklemek için Visual Studio 2017 Microsoft WCF Web hizmeti başvuru sağlayıcısını kullanarak oluşturulabilir. Visual Studio 2017 içinde Microsoft WCF Web hizmeti başvuru sağlayıcısı kullanarak proxy oluşturma alternatif ServiceModel meta veri yardımcı Programracı (svcutil.exe) kullanmaktır. Daha fazla bilgi için bkz: [ServiceModel meta veri yardımcı Programracı (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Oluşturulan proxy sınıfları zaman uyumsuz programlama modeli (APM) tasarım deseni kullanan web hizmetleri tüketimi için yöntemleri sağlar. Bu modelinde adlı iki yöntem zaman uyumsuz bir işlem uygulanır *BeginOperationName* ve *EndOperationName*, başlar ve zaman uyumsuz işlemi sona erdirmek.

*BeginOperationName* yöntemi zaman uyumsuz işlemi başlatır ve uygulayan bir nesne döndürür `IAsyncResult` arabirimi. Çağırdıktan sonra *BeginOperationName*, bir uygulama zaman uyumsuz işlemi gerçekleştirilirken bir iş parçacığı havuzu iş parçacığı üzerinde yönergeleri çağıran iş parçacığı üzerinde yürütme devam eder.

Her çağrı için *BeginOperationName*, uygulama da çağırmalıdır *EndOperationName* işlemi sonuçları elde etmek için. Dönüş değerini *EndOperationName* zaman uyumlu web hizmeti yöntemi tarafından döndürülen aynı tür. Örneğin, `EndGetTodoItems` yöntem koleksiyonu döndürür `TodoItem` örnekleri. *EndOperationName* yöntemi de içeren bir `IAsyncResult` karşılık gelen çağrısı tarafından döndürülen örneğine ayarlanmalıdır parametresi *BeginOperationName* yöntemi.

Görev paralel kitaplığı (TPL) aynı zaman uyumsuz işlemleri kapsülleyerek bir APM başlamak son yöntemi çifti kullanma işlemini basitleştirebilir `Task` nesnesi. Bu kapsülleme birden çok aşırı tarafından sağlanan `TaskFactory.FromAsync` yöntemi.

APM hakkında daha fazla bilgi için bkz: [zaman uyumsuz programlama modeli](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) ve [TPL ve geleneksel .NET Framework zaman uyumsuz Programming](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) konusuna bakın.

### <a name="creating-the-todoserviceclient-object"></a>TodoServiceClient nesnesi oluşturma

Oluşturulan proxy sınıfı sağlar `TodoServiceClient` HTTP üzerinden WCF Hizmeti ile iletişim kurmak için kullanılan sınıf. Hizmet örneği bir uri'den zaman uyumsuz işlemleri tanımlandığı gibi web hizmeti yöntem çağırma için işlevsellik sağlar. Zaman uyumsuz işlemleri hakkında daha fazla bilgi için bkz: [zaman uyumsuz desteğine genel bakış](~/cross-platform/platform/async.md).

`TodoServiceClient` Örneği için aşağıdaki kod örneğinde gösterildiği gibi WCF hizmeti kullanmak, uygulamanın ihtiyacı sürece nesne yaşadığı böylece sınıf düzeyinde bildirilen:

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient` Örneği, bilgi ve bir uç nokta adresi bağlama ile yapılandırılır. Bir bağlama taşıma, kodlama ve uygulamaları ve Hizmetleri birbirleri ile iletişim kurması gereken protokol ayrıntılarını belirtmek için kullanılır. `BasicHttpBinding` Metin olarak kodlanmış SOAP iletileri HTTP Aktarım Protokolü üzerinden gönderilecek belirtir. Birden çok yayımlanan örnekleri koşuluyla bir uç noktası adresi belirtme WCF Hizmeti farklı örneklerine bağlanmaya uygulama sağlar.

Hizmet başvurusu yapılandırma hakkında daha fazla bilgi için bkz: [hizmet başvurusu yapılandırma](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="creating-data-transfer-objects"></a>Veri aktarımı nesneleri oluşturma

Örnek uygulama kullanır `TodoItem` model verileri için sınıf. Depolamak için bir `TodoItem` öğesi, ilk dönüştürülmesi gerekir oluşturulan proxy web hizmeti `TodoItem` türü. Bu tarafından gerçekleştirilir `ToWCFServiceTodoItem` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Bu yöntem yalnızca yeni bir oluşturur `TodoWCFService.TodoItem` örneği ve her bir özellik aynı özelliğinden ayarlar `TodoItem` örneği.

Web hizmetinden veri alınırken benzer şekilde, bu oluşturulan proxy sunucudan dönüştürülmelidir `TodoItem` için yazın bir `TodoItem` örneği. Bu ile gerçekleştirilir `FromWCFServiceTodoItem` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Bu yöntem, verileri yalnızca oluşturulan proxy sunucudan alır. `TodoItem` yazın ve yeni oluşturulan ayarlar `TodoItem` örneği.

### <a name="retrieving-data"></a>Veri alma

`TodoServiceClient.BeginGetTodoItems` Ve `TodoServiceClient.EndGetTodoItems` yöntemleri çağırmak için kullanılan `GetTodoItems` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoServiceClient.EndGetTodoItems` yöntemi bir kere `TodoServiceClient.BeginGetTodoItems` yöntemi tamamlayan, ile `null` hiçbir veri içine geçirilen gösteren parametre `BeginGetTodoItems` temsilci. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

`TodoServiceClient.EndGetTodoItems` Yöntemi döndürür bir `ObservableCollection` , `TodoWCFService.TodoItem` sonra dönüştürülür örnekleri bir `List` , `TodoItem` örneklerini görüntülemek için.

### <a name="creating-data"></a>Veri oluşturma

`TodoServiceClient.BeginCreateTodoItem` Ve `TodoServiceClient.EndCreateTodoItem` yöntemleri çağırmak için kullanılan `CreateTodoItem` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoServiceClient.EndCreateTodoItem` yöntemi bir kere `TodoServiceClient.BeginCreateTodoItem` yöntemi tamamlayan, ile `todoItem` parametre olarak geçirilen verilerin `BeginCreateTodoItem` belirtmekiçintemsilci`TodoItem` web hizmeti tarafından oluşturulacak. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

Web hizmeti atar bir `FaultException` oluşturmak başarısız olursa `TodoItem`, uygulama tarafından işlenir.

### <a name="updating-data"></a>Verileri güncelleştirme

`TodoServiceClient.BeginEditTodoItem` Ve `TodoServiceClient.EndEditTodoItem` yöntemleri çağırmak için kullanılan `EditTodoItem` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoServiceClient.EndEditTodoItem` yöntemi bir kere `TodoServiceClient.BeginCreateTodoItem` yöntemi tamamlayan, ile `todoItem` parametre olarak geçirilen verilerin `BeginEditTodoItem` belirtmekiçintemsilci`TodoItem` web hizmeti tarafından güncelleştirilecek. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

Web hizmeti atar bir `FaultException` bulun veya güncelleştirmek başarısız olursa `TodoItem`, uygulama tarafından işlenir.

### <a name="deleting-data"></a>Verileri silme

`TodoServiceClient.BeginDeleteTodoItem` Ve `TodoServiceClient.EndDeleteTodoItem` yöntemleri çağırmak için kullanılan `DeleteTodoItem` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoServiceClient.EndDeleteTodoItem` yöntemi bir kere `TodoServiceClient.BeginDeleteTodoItem` yöntemi tamamlayan, ile `id` parametre olarak geçirilen verilerin `BeginDeleteTodoItem` belirtmekiçintemsilci`TodoItem` web hizmeti tarafından silinecek. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

Web hizmeti atar bir `FaultException` bulun veya silmek başarısız olursa `TodoItem`, uygulama tarafından işlenir.

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.Forms uygulaması WCF SOAP hizmetinden kullanma gösterilmektedir. Genel olarak, Xamarin platform Silverlight çalışma zamanı ile birlikte gelen WCF aynı istemci-tarafı alt destekler. Bu en yaygın kodlama ve protokolü uygulamaları WCF içerir — metin olarak kodlanmış SOAP iletileri HTTP üzerinden aktarım protokolünü kullanarak `BasicHttpBinding` sınıfı. Ayrıca, WCF Destek Araçları proxy oluşturmak için bir Windows ortamında yalnızca kullanılabilir kullanılmasını gerektirir.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoWCF (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
