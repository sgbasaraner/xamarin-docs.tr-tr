---
title: Web hizmetlerine giriş
description: Bu kılavuz, farklı bir web hizmeti teknolojileri tüketen gösterilmiştir. Kapsanan konular REST Hizmetleri, SOAP Hizmetleri ve Windows Communication Foundation Hizmetleri ile iletişim içerir.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ad18382a7143c7b1cc6bbecb3867c042512eb562
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-web-services"></a>Web hizmetlerine giriş

_Bu kılavuz, farklı bir web hizmeti teknolojileri tüketen gösterilmiştir. Kapsanan konular REST Hizmetleri, SOAP Hizmetleri ve Windows Communication Foundation Hizmetleri ile iletişim içerir._

Doğru çalışması için birçok mobil uygulama bulutta bağımlıdır ve bu nedenle web hizmetleri mobil uygulamalarla tümleştirmek ortak bir senaryodur. Xamarin platform farklı bir web hizmeti teknolojileri kullanan destekler ve RESTful, ASMX ve Windows Communication Foundation (WCF) hizmetlerini tüketen-yerleşik ve üçüncü taraf desteği içerir.

Bu makalede aşağıdaki konular ele alınmıştır:

- [REST Hizmetleri](#rest)
- [ASP.Net Web Services (ASMX)](#asmx)
- [WCF hizmetleri](#wcf)

Xamarin.Forms kullanan müşteriler için bu teknolojilerin her kullanarak tam örnekler vardır [Xamarin.Forms Web Hizmetleri](~/xamarin-forms/data-cloud/index.md) belgeleri.

> [!IMPORTANT]
> İOS 9'da, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar.
> ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.

Size ATS kullanmak mümkün değilse çevirin `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).



<a name="rest" />

## <a name="rest"></a>REST

Temsili durum aktarımı (REST) web hizmetleri oluşturmaya yönelik bir mimari stili ' dir. REST istekleri, web sayfaları almak ve veri sunucularına göndermek için web tarayıcıları kullanma aynı HTTP fiillerini kullanarak HTTP üzerinden yapılır. Fiiller şunlardır:

- **Alma** – bu işlem web hizmetinden veri almak için kullanılır.
- **POST** – bu işlem web hizmetini yeni bir veri öğesi oluşturmak için kullanılır.
- **PUT** – bu işlem web hizmetine veri öğesi güncelleştirmek için kullanılır.
- **Düzeltme Eki** – bu işlemi nasıl öğesi değiştirilmemelidir hakkında yönergeler kümesini açıklayarak web hizmetine veri öğesi güncelleştirmek için kullanılır. Bu fiil örnek uygulama kullanılmaz.
- **SİLME** – bu işlem web hizmetine veri öğesi silmek için kullanılır.

Web hizmeti uyması için REST API'leri RESTful API'lerini denir ve kullanılarak tanımlanır:

- Taban URI.
- GET, POST, PUT, PATCH veya silme gibi HTTP yöntemleri.
- JavaScript nesne gösterimi (JSON) gibi veri için bir ortam türü.

REST basitliği mobil uygulamalarda web hizmetlerine erişme için birincil yöntem olmasına yardımcı oldu.

## <a name="consuming-rest-services"></a>REST Hizmetleri kullanma

Bir dizi kitaplıkları ve REST hizmetlerini kullanma için kullanılan sınıfları vardır ve aşağıdaki alt bölümleri bunları tartışın. Bir REST hizmeti kullanma hakkında daha fazla bilgi için bkz: [bir RESTful Web hizmeti kullanan](~/xamarin-forms/data-cloud/consuming/rest.md).

### <a name="httpclient"></a>HttpClient

[Microsoft HTTP istemci kitaplıkları](https://www.nuget.org/packages/Microsoft.Net.Http) sağlar `HttpClient` HTTP üzerinden istekleri alıp göndermek için kullanılan sınıf. HTTP istekleri göndermek ve alıcı HTTP yanıtlarını URI tanımlanan bir kaynaktan için işlevsellik sağlar. Her istek eşzamansız bir işlem olarak gönderilir. Zaman uyumsuz işlemleri hakkında daha fazla bilgi için bkz: [zaman uyumsuz desteğine genel bakış](~/cross-platform/platform/async.md).

`HttpResponseMessage` Sınıfı, bir HTTP isteği yapıldıktan sonra web hizmetinden alınan bir HTTP yanıt iletisini temsil eder. Durum kodu, üstbilgi ve gövde dahil olmak üzere, yanıt hakkında bilgi içerir. `HttpContent` Sınıfı temsil eder HTTP gövdesi ve içerik üstbilgileri gibi `Content-Type` ve `Content-Encoding`. İçeriği herhangi birini kullanarak okunabilir `ReadAs` yöntemleri gibi `ReadAsStringAsync` ve `ReadAsByteArrayAsync`, bağlı veri biçimi.

Hakkında daha fazla bilgi için `HttpClient` sınıfı için bkz: [HTTPClient nesnesi oluşturma](~/xamarin-forms/data-cloud/consuming/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Web Hizmetleri ile çağırma `HTTPWebRequest` içerir:

-  İstek örneği için belirli bir URI oluşturma.
-  İstek örneğinde çeşitli HTTP özelliklerini ayarlama.
-  Alınırken bir `HttpWebResponse` istek.
-  Yanıt verileri okunuyor.

Örneğin, aşağıdaki kodu ABD verileri alır. Ulusal kitaplığı, TIP web hizmeti:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

Yukarıdaki örnekte bir `HttpWebRequest` JSON olarak biçimlendirilmiş verileri döndürecektir. İçinde döndürülen veriler bir `HttpWebResponse`, içinden bir `StreamReader` verileri okumak için elde edilebilir.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

REST hizmetleri kullanan başka bir yaklaşım kullanarak [RestSharp](http://restsharp.org/) kitaplığı. RestSharp sonuçları ham dize içeriği veya seri durumdan çıkarılmış bir C# nesnesi olarak desteği de dahil olmak üzere HTTP isteklerini yalıtır. Örneğin, aşağıdaki kodu ABD isteğinde bulunur. Ulusal kitaplığı, TIP web hizmeti ve alır sonuçları JSON olarak biçimlendirilmiş dize:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` Ham JSON dizesi sürer yöntemdir `RestSharp.RestResponse.Content` özelliği ve bir C# nesnesine dönüştürmek. Web hizmetlerinden döndürülen verileri seri durumdan bu makalenin sonraki bölümlerinde ele alınmıştır.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Sınıf gibi kitaplığı (BCL) Mono Bankası'nda kullanılabilen sınıfları yanı sıra `HttpWebRequest`ve üçüncü taraf C# kitaplıkları RestSharp gibi platforma özgü sınıfları Ayrıca web hizmetlerini tüketen için kullanılabilir. Örneğin, iOS içinde `NSUrlConnection` ve `NSMutableUrlRequest` sınıfları kullanılabilir.

Aşağıdaki kod örneğinde ABD çağırmak nasıl gösterir Ulusal kitaplığı, TIP web hizmeti: iOS sınıflarını kullanma

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

Genellikle, web hizmetleri tüketimi için platforma özgü sınıflar burada yerel kod C# için verilen senaryoları ile sınırlı olması gerekir. Paylaşılan platformlar arası olabilmesi Mümkünse, web hizmeti erişim kodu taşınabilir olmalıdır.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Web hizmetleri çağırmak için başka bir seçenek [hizmet yığını](http://www.servicestack.net/) kitaplığı. Örneğin, aşağıdaki kodu hizmet yığın kullanmayı gösterir `IServiceClient.GetAsync` bir hizmet istek yöntemi:

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> ServiceStack ve RestSharp çağırın ve kullanmak kolaylaştırmak gibi araçlar Hizmetleri REST olsa da, bazen XML veya standart uymuyor JSON tüketmeye Önemsiz olmayan _DataContract_ serileştirme kuralları. Gerekirse, istek çağırma ve açıkça aşağıda ele ServiceStack.Text kitaplığını kullanarak uygun serileştirme tanıtıcı.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>RESTful verinin tüketimi

RESTful web Hizmetleri, JSON iletileri genellikle veri istemciye döndürmek için kullanın. JSON ve azaltılmış bant genişliği gereksinimlerini veri gönderilirken sonuçlanır, üretir compact yükü, bir metin tabanlı veri değişim biçimidir. Bu bölümde, JSON ve düz eski XML (POX) RESTful yanıtları tüketimi için mekanizmaları denenecektir.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Xamarin platform JSON desteği kutunun dışında ile birlikte gelir. Kullanarak bir `JsonObject`, aşağıdaki kod örneğinde gösterildiği gibi sonuçları alınabilir:

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

Ancak, farkında olması önemlidir, `System.Json` araçları veri bütünlüğü belleğe yüklenemiyor.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[NewtonSoft JSON.NET kitaplığı](http://www.newtonsoft.com/json) seri hale getirme ve JSON iletileri seri durumdan çıkarmak için yaygın olarak kullanılan bir kitaplıktır. Aşağıdaki kod örneğinde bir JSON ileti bir C# nesnesi seri durumdan çıkarılacak JSON.NET kullanmayı gösterir:

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack.Text

ServiceStack.Text ServiceStack kitaplığı ile çalışmak üzere tasarlanmış bir JSON seri hale getirme kitaplıktır. Aşağıdaki kod örneği, JSON kullanılarak gösterilmektedir bir `ServiceStack.Text.JsonObject`:

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

Bir XML tabanlı REST web hizmeti kullanan durumunda LINQ-XML XML Ayrıştırma ve bir C# nesne satır içi, doldurmak için aşağıdaki kod örneğinde gösterildiği gibi kullanılabilir:

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>ASP.NET Web Service (ASMX)

ASMX Basit Nesne Erişim Protokolü (SOAP) kullanarak iletileri gönder web hizmetleri oluşturma yeteneği sağlar. SOAP oluşturmak ve web hizmetlerine erişme platformdan bağımsız ve dilden bağımsız bir protokoldür. ASMX hizmet tüketicileri herhangi bir şey platform, nesne modeli ya da hizmet uygulamak için kullanılan programlama dili hakkında bilmeniz gerek yoktur. Bunlar yalnızca SOAP ileti gönderme ve alma nasıl anlamanız gerekir.

Aşağıdaki öğeleri içeren bir XML belgesi bir SOAP iletisi şudur:

- Adlı bir kök öğe *Zarf* XML belgesi bir SOAP iletisi olarak tanımlar.
- İsteğe bağlı bir *üstbilgi* kimlik doğrulama verileri gibi uygulamaya özgü bilgileri içeren öğe. Varsa *üstbilgi* öğesi mevcutsa ilk alt öğesi olmalıdır *Zarf* öğesi.
- Gerekli *gövde* alıcıya SOAP iletisi içeren öğe.
- İsteğe bağlı bir *hata* hata iletileri göstermek için kullanılan öğesi. Varsa *hataya* öğesi mevcutsa, bir alt öğesi olmalıdır. *gövde* öğesi.

SOAP, HTTP, SMTP, TCP ve UDP de dahil olmak üzere birçok aktarım protokolleri çalışabilir. Ancak, bir ASMX hizmeti yalnızca HTTP üzerinden çalışabilir. Xamarin platform HTTP üzerinden standart SOAP 1.1 uygulamaları destekler ve bu standart ASMX hizmet yapılandırması çoğunu destekler.

### <a name="generating-a-proxy"></a>Bir proxy sunucu oluşturma

A *proxy* hizmetine bağlanmak uygulama izin veren bir ASMX hizmeti kullanmak için oluşturulmuş olması gerekir. Proxy yöntemleri ve ilişkili hizmet yapılandırmasını tanımlar Süren hizmeti meta verileri tarafından oluşturulur. Bu meta veriler web hizmeti tarafından oluşturulan Web Hizmetleri Açıklama Dili (WSDL) belgesi olarak sunulur. Mac için Visual Studio veya Visual Studio web hizmeti için bir web başvuru platforma özgü projelerine ekleme kullanarak yerleşik proxy.

Web hizmeti URL'si ya da barındırılan uzak kaynak ya da yerel dosya sistemi kaynak aracılığıyla erişilebilir olabilir `file:///` yol öneki, örneğin:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "Web hizmeti URL'si ya da barındırılan uzak kaynak ya da yerel dosya sistemi kaynak dosya yolu önekini erişilebilir olabilir")](images/add-webreference-dialog.png#lightbox)

Bu projenin Web veya hizmet başvuruları klasöründe proxy oluşturur. Bir proxy oluşturulan beri kod, onu değiştirilmemelidir.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Bir Proxy projeye el ile ekleme

Uyumlu araçlar kullanılarak oluşturulan mevcut bir proxy varsa, bu çıkış, projenizin bir parçası olarak dahil olduğunda tüketilebilir. Mac için Visual Studio'da kullanın **dosyaları Ekle...** proxy eklemek için menü seçeneği'ni kullanın. Ayrıca, bu gerektirir *System.Web.Services.dll* kullanılarak açıkça başvurulacak **Başvuru Ekle...** iletişim kutusu.

### <a name="consuming-the-proxy"></a>Proxy kullanma

Oluşturulan proxy sınıfları zaman uyumsuz programlama modeli (APM) tasarım deseni kullanan web hizmeti tüketimi için yöntemleri sağlar. Bu desen adlı iki yöntem zaman uyumsuz bir işlem uygulanır *BeginOperationName* ve *EndOperationName*, başlar ve zaman uyumsuz işlemi sona erdirmek.

*BeginOperationName* yöntemi zaman uyumsuz işlemi başlatır ve uygulayan bir nesne döndürür `IAsyncResult` arabirimi. Çağırdıktan sonra *BeginOperationName*, bir uygulama zaman uyumsuz işlemi gerçekleştirilirken bir iş parçacığı havuzu iş parçacığı üzerinde yönergeleri çağıran iş parçacığı üzerinde yürütme devam eder.

Her çağrı için *BeginOperationName*, uygulama da çağırmalıdır *EndOperationName* işlemi sonuçları elde etmek için. Dönüş değerini *EndOperationName* zaman uyumlu web hizmeti yöntemi tarafından döndürülen aynı tür. Aşağıdaki kod örneğinde bunun bir örneği gösterilmektedir:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Görev paralel kitaplığı (TPL) aynı zaman uyumsuz işlemleri kapsülleyerek bir APM başlamak son yöntemi çifti kullanma işlemini basitleştirebilir `Task` nesnesi. Bu kapsülleme birden çok aşırı tarafından sağlanan `Task.Factory.FromAsync` yöntemi. Bu yöntem oluşturur bir `Task` yürütmelerinin `TodoService.EndGetTodoItems` yöntemi bir kere `TodoService.BeginGetTodoItems` yöntemi tamamlayan, ile `null` hiçbir veri içine geçirilen gösteren parametre `BeginGetTodoItems` temsilci. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

APM hakkında daha fazla bilgi için bkz: [zaman uyumsuz programlama modeli](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) ve [TPL ve geleneksel .NET Framework zaman uyumsuz Programming](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) konusuna bakın.

ASMX hizmeti kullanma hakkında daha fazla bilgi için bkz: [ASP.NET Web hizmeti (ASMX) kullanan](~/xamarin-forms/data-cloud/consuming/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF hizmet odaklı uygulamalar oluşturmak için Microsoft'un birleşik çerçevedir. Geliştiricilerin güvenli, güvenilir, işlenen ve birlikte çalışabilir dağıtılmış uygulamalar oluşturmalarını sağlar.

WCF hizmet aşağıdakiler dahil farklı sözleşmeleri çeşitli ile açıklanmaktadır:

- **Veri sözleşmeleri** – bir ileti içinde içeriği temelini veri yapılarını tanımlar.
- **İleti sözleşmeleri** – mevcut veri sözleşmeleri iletilerden oluşturun.
- **Arıza sözleşmeleri** – özel SOAP hataları belirtilmesine izin verin.
- **Hizmet sözleşmeleri** – Destek Hizmetleri işlemleri belirtin ve iletileri gerekli her işlemi ile etkileşmek için. Bunlar ayrıca her hizmet işlemleri ile ilişkilendirilebilir herhangi özel hata davranışı belirtin.

ASP.NET Web Hizmetleri (ASMX) ve WCF arasındaki farklar vardır, ancak WCF ASMX sağlar – aynı yetenekleri HTTP üzerinden SOAP iletilerine desteklediğini anlamak önemlidir.

Genel olarak, Xamarin platform Silverlight çalışma zamanı ile birlikte gelen WCF aynı istemci-tarafı alt destekler. Bu en yaygın kodlama ve protokolü uygulamaları WCF içerir — metin olarak kodlanmış SOAP iletileri HTTP üzerinden aktarım protokolünü kullanarak `BasicHttpBinding` sınıfı. Ayrıca, WCF Destek Araçları proxy oluşturmak için bir Windows ortamında yalnızca kullanılabilir kullanılmasını gerektirir.

Web hizmeti ile bir WCF kullanmak için Xamarin platform kullanma hakkında daha fazla bilgi için `BasicHttpBinding` sınıfı için bkz: [WCF ile çalışma Kılavuzu -](walkthrough-working-with-wcf.md).

### <a name="generating-a-proxy"></a>Bir proxy sunucu oluşturma

A *proxy* hizmetine bağlanmak uygulama izin veren bir WCF hizmeti kullanmak için oluşturulmuş olması gerekir. Proxy yöntemleri ve ilişkili hizmet yapılandırmasını tanımlar Süren hizmeti meta verileri tarafından oluşturulur. Bu meta veriler web hizmeti tarafından oluşturulan bir Web Hizmetleri Açıklama Dili (WSDL) belge şeklinde sunulur. Proxy web hizmeti için hizmet başvurusu .NET standart kitaplığına eklemek için Visual Studio 2017 Microsoft WCF Web hizmeti başvuru sağlayıcısını kullanarak oluşturulabilir.

Visual Studio 2017 içinde Microsoft WCF Web hizmeti başvuru sağlayıcısı kullanarak proxy oluşturma alternatif ServiceModel meta veri yardımcı Programracı (svcutil.exe) kullanmaktır. Daha fazla bilgi için bkz: [ServiceModel meta veri yardımcı Programracı (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Proxy'sini yapılandırma

Oluşturulan proxy yapılandırma genellikle alacak iki yapılandırma bağımsız değişkenler (veya bağlı olarak SOAP 1.1/ASMX WCF) başlatma sırasında: `EndpointAddress` ve/veya aşağıdaki örnekte gösterildiği gibi ilişkili bağlama bilgileri:

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

Bir bağlama taşıma, kodlama ve uygulamaları ve Hizmetleri birbirleri ile iletişim kurması gereken protokol ayrıntılarını belirtmek için kullanılır. `BasicHttpBinding` Metin olarak kodlanmış SOAP iletileri HTTP Aktarım Protokolü üzerinden gönderilecek belirtir. Birden çok yayımlanan örnekleri koşuluyla bir uç noktası adresi belirtme WCF Hizmeti farklı örneklerine bağlanmaya uygulama sağlar.

### <a name="consuming-the-proxy"></a>Proxy kullanma

Oluşturulan proxy sınıfları zaman uyumsuz programlama modeli (APM) tasarım deseni kullanan web hizmetleri tüketimi için yöntemleri sağlar. Bu modelinde adlı iki yöntem zaman uyumsuz bir işlem uygulanır *BeginOperationName* ve *EndOperationName*, başlar ve zaman uyumsuz işlemi sona erdirmek.

*BeginOperationName* yöntemi zaman uyumsuz işlemi başlatır ve uygulayan bir nesne döndürür `IAsyncResult` arabirimi. Çağırdıktan sonra *BeginOperationName*, bir uygulama zaman uyumsuz işlemi gerçekleştirilirken bir iş parçacığı havuzu iş parçacığı üzerinde yönergeleri çağıran iş parçacığı üzerinde yürütme devam eder.

Her çağrı için *BeginOperationName*, uygulama da çağırmalıdır *EndOperationName* işlemi sonuçları elde etmek için. Dönüş değerini *EndOperationName* zaman uyumlu web hizmeti yöntemi tarafından döndürülen aynı tür. Aşağıdaki kod örneğinde bunun bir örneği gösterilmektedir:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Görev paralel kitaplığı (TPL) aynı zaman uyumsuz işlemleri kapsülleyerek bir APM başlamak son yöntemi çifti kullanma işlemini basitleştirebilir `Task` nesnesi. Bu kapsülleme birden çok aşırı tarafından sağlanan `Task.Factory.FromAsync` yöntemi. Bu yöntem oluşturur bir `Task` yürütmelerinin `TodoServiceClient.EndGetTodoItems` yöntemi bir kere `TodoServiceClient.BeginGetTodoItems` yöntemi tamamlayan, ile `null` hiçbir veri içine geçirilen gösteren parametre `BeginGetTodoItems` temsilci. Son olarak, değeri `TaskCreationOptions` numaralandırmasını belirtir oluşturulması ve görevlerini yürütülmesi için varsayılan davranış kullanılmalıdır.

APM hakkında daha fazla bilgi için bkz: [zaman uyumsuz programlama modeli](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) ve [TPL ve geleneksel .NET Framework zaman uyumsuz Programming](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) konusuna bakın.

Bir WCF hizmetini kullanma hakkında daha fazla bilgi için bkz: [bir Windows Communication Foundation (WCF) Web hizmeti kullanan](~/xamarin-forms/data-cloud/consuming/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Taşıma güvenliği kullanma

WCF hizmetleri aktarım düzeyi güvenlik iletileri ele geçirilmesini karşı korumak için kullanabilir. Xamarin platform, taşıma düzeyi güvenliği SSL ile kullanan bağlamaları destekler. Ancak, istenmeyen davranışları sonuçları sertifikasını doğrulamak yığın gerekebilir durumlar olabilir. Doğrulama kaydederek kılınabilir bir `ServerCertificateValidationCallback` aşağıdaki kod örneğinde gösterildiği şekilde Hizmeti'ni çağırmadan önce temsilci:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Sunucu tarafı sertifika doğrulaması yoksayılıyor çalışırken aktarım şifreleme tutar. Ancak, bu yaklaşım etkili bir şekilde sertifikayla ilişkili güven sorunları yoksayar ve uygun olmayabilir. Daha fazla bilgi için bkz: [Respectfully kökleri güvenilen kullanarak](http://www.mono-project.com/UsingTrustedRootsRespectfully) üzerinde [mono project.com](http://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>İstemci kimlik bilgileri güvenlik kullanma

WCF hizmetleri kimlik bilgilerini kullanarak kimlik doğrulaması hizmeti istemcileri de gerektirebilir. Xamarin platform SOAP iletisi zarfı içinde kimlik bilgilerini göndermek istemcilerin WS-güvenlik protokolünü desteklemiyor. Ancak, Xamarin platform uygun belirterek sunucuya HTTP temel kimlik doğrulama kimlik bilgileri gönderme olanağı destekler `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Ardından, temel kimlik doğrulama kimlik bilgileri belirtilebilir:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

"0 türüne trampolines dışında çalışan" iletisi alırsanız, yukarıdaki örnekte, 0 yazın trampolines sayısı ekleyerek artırabilirsiniz `–aot “trampolines={number of trampolines}”` yapı bağımsız değişkeni. Daha fazla bilgi için bkz: [sorun giderme](~/ios/troubleshooting/troubleshooting.md#trampolines).

HTTP temel kimlik doğrulaması hakkında daha fazla bilgi için bkz: bağlamında bir REST web hizmeti olsa da, [bir RESTful Web hizmeti kimlik doğrulama](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="summary"></a>Özet

Bu kılavuz, farklı bir web hizmeti teknolojileri kullanma gösterilmektedir. Kapsanan konular REST Hizmetleri, SOAP Hizmetleri ve Windows Communication Foundation Hizmetleri ile iletişim içerir.

## <a name="related-links"></a>İlgili bağlantılar

- [Webservices'a örnek](https://developer.xamarin.com/samples/mobile/WebServices/WebServiceSamples/)
- [Xamarin.Forms Web Hizmetleri](~/xamarin-forms/data-cloud/index.md)
- [ServiceModel meta veri yardımcı Programracı (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](http://msdn.microsoft.com/en-us/library/system.servicemodel.basichttpbinding.aspx)
