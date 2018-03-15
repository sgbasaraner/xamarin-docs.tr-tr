---
title: Bir ASP.NET Web hizmeti (ASMX) kullanma
description: "ASMX Basit Nesne Erişim Protokolü (SOAP) kullanarak iletileri gönder web hizmetleri oluşturma yeteneği sağlar. SOAP oluşturmak ve web hizmetlerine erişme platformdan bağımsız ve dilden bağımsız bir protokoldür. ASMX hizmet tüketicileri herhangi bir şey platform, nesne modeli ya da hizmet uygulamak için kullanılan programlama dili hakkında bilmeniz gerek yoktur. Bunlar yalnızca SOAP ileti gönderme ve alma nasıl anlamanız gerekir. Bu makalede, bir Xamarin.Forms uygulaması ASMX SOAP hizmetinden kullanma gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a095dbbb78ad1517791356ae0b7cbeaa94d1336f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Bir ASP.NET Web hizmeti (ASMX) kullanma

_ASMX Basit Nesne Erişim Protokolü (SOAP) kullanarak iletileri gönder web hizmetleri oluşturma yeteneği sağlar. SOAP oluşturmak ve web hizmetlerine erişme platformdan bağımsız ve dilden bağımsız bir protokoldür. ASMX hizmet tüketicileri herhangi bir şey platform, nesne modeli ya da hizmet uygulamak için kullanılan programlama dili hakkında bilmeniz gerek yoktur. Bunlar yalnızca SOAP ileti gönderme ve alma nasıl anlamanız gerekir. Bu makalede, bir Xamarin.Forms uygulaması ASMX SOAP hizmetinden kullanma gösterilmektedir._

Aşağıdaki öğeleri içeren bir XML belgesi bir SOAP iletisi şudur:

- Adlı bir kök öğe *Zarf* XML belgesi bir SOAP iletisi olarak tanımlar.
- İsteğe bağlı bir *üstbilgi* kimlik doğrulama verileri gibi uygulamaya özgü bilgileri içeren öğe. Varsa *üstbilgi* öğesi mevcutsa ilk alt öğesi olmalıdır *Zarf* öğesi.
- Gerekli *gövde* alıcıya SOAP iletisi içeren öğe.
- İsteğe bağlı bir *hata* hata iletileri göstermek için kullanılan öğesi. Varsa *hataya* öğesi mevcutsa, bir alt öğesi olmalıdır. *gövde* öğesi.

SOAP, HTTP, SMTP, TCP ve UDP de dahil olmak üzere birçok aktarım protokolleri çalışabilir. Ancak, bir ASMX hizmeti yalnızca HTTP üzerinden çalışabilir. Xamarin platform HTTP üzerinden standart SOAP 1.1 uygulamaları destekler ve bu standart ASMX hizmet yapılandırması çoğunu destekler.

ASMX hizmet ayarlama yönergeleri örnek uygulama eşlik eden readme dosyasında bulunabilir. Örnek uygulamayı çalıştırdığınızda, ancak, bu veriler, salt okunur erişim sağlayan bir Xamarin barındırılan ASMX hizmet aşağıdaki ekran görüntüsünde gösterildiği gibi bağlanır:

![](asmx-images/portal.png "Örnek uygulama")

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Web hizmetini kullanma

ASMX hizmeti aşağıdaki işlemleri sağlar:

|Çalışma|Açıklama|Parametreler|
|--- |--- |--- |
|GetTodoItems|Yapılacaklar öğelerini bir listesini alma|
|CreateTodoItem|Yeni bir Yapılacaklar öğesi oluşturma|Bir XML Todoıtem serileştirilmiş|
|EditTodoItem|Güncelleştirme Yapılacaklar öğesi|Bir XML Todoıtem serileştirilmiş|
|DeleteTodoItem|Yapılacaklar öğesi silme|Bir XML Todoıtem serileştirilmiş|

Uygulamada kullanılan veri modeli hakkında daha fazla bilgi için bkz: [modelleme verileri](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Örnek uygulaması web hizmetine salt okunur erişim sağlayan ASMX Xamarin barındırılan hizmeti kullanır. Bu nedenle, oluşturmak, güncelleştirmek ve verileri silme işlemleri uygulamada kullanılan verileri değiştirmez. Ancak, ASMX hizmet barındırılabilir bir sürümü kullanılabilir **TodoASMXService** eşlik eden örnek uygulama klasöründe. ASMX hizmet verir tam barındırılabilir bu sürümü oluşturmak güncelleştirme okumak ve verilere erişim silin.

A *proxy* hizmetine bağlanmak uygulamanın sağlayan ASMX hizmeti kullanmak için oluşturulmuş olması gerekir. Proxy yöntemleri ve ilişkili hizmet yapılandırmasını tanımlar Süren hizmeti meta verileri tarafından oluşturulur. Bu meta veriler web hizmeti tarafından oluşturulan bir Web Hizmetleri Açıklama Dili (WSDL) belge şeklinde sunulur. Proxy web hizmeti için bir web başvuru platforma özgü projelerine ekleme tarafından oluşturulmuştur.

Oluşturulan proxy sınıfları zaman uyumsuz programlama modeli (APM) tasarım deseni kullanan web hizmeti tüketimi için yöntemleri sağlar. Bu desen adlı iki yöntem zaman uyumsuz bir işlem uygulanır *BeginOperationName* ve *EndOperationName*, başlar ve zaman uyumsuz işlemi sona erdirmek.

*BeginOperationName* yöntemi zaman uyumsuz işlemi başlatır ve uygulayan bir nesne döndürür `IAsyncResult` arabirimi. Çağırdıktan sonra *BeginOperationName*, bir uygulama zaman uyumsuz işlemi gerçekleştirilirken bir iş parçacığı havuzu iş parçacığı üzerinde yönergeleri çağıran iş parçacığı üzerinde yürütme devam eder.

Her çağrı için *BeginOperationName*, uygulama da çağırmalıdır *EndOperationName* işlemi sonuçları elde etmek için. Dönüş değerini *EndOperationName* zaman uyumlu web hizmeti yöntemi tarafından döndürülen aynı tür. Örneğin, `EndGetTodoItems` yöntem koleksiyonu döndürür `TodoItem` örnekleri. *EndOperationName* yöntemi de içeren bir `IAsyncResult` karşılık gelen çağrısı tarafından döndürülen örneğine ayarlanmalıdır parametresi *BeginOperationName* yöntemi.

Görev paralel kitaplığı (TPL) aynı zaman uyumsuz işlemleri kapsülleyerek bir APM başlamak son yöntemi çifti kullanma işlemini basitleştirebilir `Task` nesnesi. Bu kapsülleme birden çok aşırı tarafından sağlanan `TaskFactory.FromAsync` yöntemi.

APM hakkında daha fazla bilgi için bkz: [zaman uyumsuz programlama modeli](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) ve [TPL ve geleneksel .NET Framework zaman uyumsuz Programming](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) konusuna bakın.

### <a name="creating-the-todoservice-object"></a>TodoService nesnesi oluşturma

Oluşturulan proxy sınıfı sağlar `TodoService` ASMX hizmetiyle HTTP üzerinden iletişim kurmak için kullanılan sınıf. Hizmet örneği bir uri'den zaman uyumsuz işlemleri tanımlandığı gibi web hizmeti yöntem çağırma için işlevsellik sağlar. Zaman uyumsuz işlemleri hakkında daha fazla bilgi için bkz: [zaman uyumsuz desteğine genel bakış](~/cross-platform/platform/async.md).

`TodoService` Örneği için aşağıdaki kod örneğinde gösterildiği gibi ASMX hizmeti kullanmak, uygulamanın ihtiyacı sürece nesne yaşadığı böylece sınıf düzeyinde bildirilen:

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

`TodoService` Oluşturucusu ASMX hizmet örneği URL'sini belirten bir isteğe bağlı dize parametresi alır. Birden çok yayımlanan örnekleri koşuluyla bu uygulamanın farklı ASMX hizmet örneklerine bağlanmasına sağlar.

### <a name="creating-data-transfer-objects"></a>Veri aktarımı nesneleri oluşturma

Örnek uygulama kullanır `TodoItem` model verileri için sınıf. Depolamak için bir `TodoItem` öğesi, ilk dönüştürülmesi gerekir oluşturulan proxy web hizmeti `TodoItem` türü. Bu tarafından gerçekleştirilir `ToASMXServiceTodoItem` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Bu yöntem yalnızca yeni bir oluşturur `ASMService.TodoItem` örneği ve her bir özellik aynı özelliğinden ayarlar `TodoItem` örneği.

Web hizmetinden veri alınırken benzer şekilde, bu oluşturulan proxy sunucudan dönüştürülmelidir `TodoItem` için yazın bir `TodoItem` örneği. Bu ile gerçekleştirilir `FromASMXServiceTodoItem` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
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

`TodoService.BeginGetTodoItems` Ve `TodoService.EndGetTodoItems` yöntemleri çağırmak için kullanılan `GetTodoItems` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoService.EndGetTodoItems` yöntemi bir kere `TodoService.BeginGetTodoItems` yöntemi tamamlayan, ile `null` hiçbir veri içine geçirilen gösteren parametre `BeginGetTodoItems` temsilci. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

`TodoService.EndGetTodoItems` Yöntemi bir dizi döndürür `ASMXService.TodoItem` sonra dönüştürülür örnekleri bir `List` , `TodoItem` örneklerini görüntülemek için.

### <a name="creating-data"></a>Veri oluşturma

`TodoService.BeginCreateTodoItem` Ve `TodoService.EndCreateTodoItem` yöntemleri çağırmak için kullanılan `CreateTodoItem` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoService.EndCreateTodoItem` yöntemi bir kere `TodoService.BeginCreateTodoItem` yöntemi tamamlayan, ile `todoItem` parametre olarak geçirilen verilerin `BeginCreateTodoItem` belirtmekiçintemsilci`TodoItem` web hizmeti tarafından oluşturulacak. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

Web hizmeti atar bir `SoapException` oluşturmak başarısız olursa `TodoItem`, uygulama tarafından işlenir.

### <a name="updating-data"></a>Verileri güncelleştirme

`TodoService.BeginEditTodoItem` Ve `TodoService.EndEditTodoItem` yöntemleri çağırmak için kullanılan `EditTodoItem` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoService.EndEditTodoItem` yöntemi bir kere `TodoService.BeginCreateTodoItem` yöntemi tamamlayan, ile `todoItem` parametre olarak geçirilen verilerin `BeginEditTodoItem` belirtmekiçintemsilci`TodoItem` web hizmeti tarafından güncelleştirilecek. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

Web hizmeti atar bir `SoapException` bulun veya güncelleştirmek başarısız olursa `TodoItem`, uygulama tarafından işlenir.

### <a name="deleting-data"></a>Verileri silme

`TodoService.BeginDeleteTodoItem` Ve `TodoService.EndDeleteTodoItem` yöntemleri çağırmak için kullanılan `DeleteTodoItem` web hizmeti tarafından sağlanan işlem. Bu zaman uyumsuz yöntemleri içinde kapsüllenir bir `Task` nesnesi, aşağıdaki kod örneğinde gösterildiği gibi:

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

`Task.Factory.FromAsync` Yöntemi oluşturur bir `Task` yürütmelerinin `TodoService.EndDeleteTodoItem` yöntemi bir kere `TodoService.BeginDeleteTodoItem` yöntemi tamamlayan, ile `id` parametre olarak geçirilen verilerin `BeginDeleteTodoItem` belirtmekiçintemsilci`TodoItem` web hizmeti tarafından silinecek. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

Web hizmeti atar bir `SoapException` bulun veya silmek başarısız olursa `TodoItem`, uygulama tarafından işlenir.

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.Forms uygulaması ASMX web hizmetinden kullanma gösterilmektedir. ASMX SOAP kullanarak HTTP üzerinden ileti göndermek web hizmetleri oluşturma yeteneği sağlar. ASMX hizmet tüketicileri herhangi bir şey platform, nesne modeli ya da hizmet uygulamak için kullanılan programlama dili hakkında bilmeniz gerek yoktur. Bunlar yalnızca SOAP ileti gönderme ve alma nasıl anlamanız gerekir.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoASMX (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
