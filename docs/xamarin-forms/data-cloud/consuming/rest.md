---
title: Bir RESTful Web hizmetini kullanma
description: "Bir web hizmeti bir uygulamayla tümleştirmek ortak bir senaryodur. Bu makalede, bir Xamarin.Forms uygulaması RESTful web hizmetinden kullanma gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: f8b748ad1b57218d1e8aab11bdc1037cf3cfa14c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-a-restful-web-service"></a>Bir RESTful Web hizmetini kullanma

_Bir web hizmeti bir uygulamayla tümleştirmek ortak bir senaryodur. Bu makalede, bir Xamarin.Forms uygulaması RESTful web hizmetinden kullanma gösterilmektedir._

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

RESTful web Hizmetleri, JSON iletileri genellikle veri istemciye döndürmek için kullanın. JSON veri gönderilirken sonuçlanır üretir compact yüklerini bant genişliği gereksinimlerini sınırlı bir metin tabanlı veri değişim biçimidir. Örnek uygulama açık kaynak kullanan [NewtonSoft JSON.NET kitaplığı](http://www.newtonsoft.com/json) seri hale getirmek ve seri durumdan iletileri.

REST basitliği mobil uygulamalarda web hizmetlerine erişme için birincil yöntem olmasına yardımcı oldu.

REST hizmeti ayarlama yönergeleri örnek uygulama eşlik eden readme dosyasında bulunabilir. Örnek uygulamayı çalıştırdığınızda, ancak, bu veriler, salt okunur erişim sağlayan bir Xamarin barındırılan REST hizmeti aşağıdaki ekran görüntüsünde gösterildiği gibi bağlanır:

![](rest-images/portal.png "Örnek uygulama")

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
>
>ATS kabul dışı kullanmak mümkün değilse **HTTPS** protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Web hizmetini kullanma

REST hizmeti ASP.NET Core kullanarak yazılır ve aşağıdaki işlemleri sağlar:

<table>
  <thead>
    <tr>
      <th>Çalışma</th>
      <th>HTTP yöntemi</th>
      <th>Göreli URI</th>
      <th>Parametreler</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Yapılacaklar öğelerini bir listesini alma</td>
      <td>AL</td>
      <td>/api/todoitems/</td>
      <td></td>
    </tr>
    <tr>
      <td>Yeni bir Yapılacaklar öğesi oluşturma</td>
      <td>YAYINLA</td>
      <td>/api/todoitems/</td>
      <td>Biçimlendirilmiş JSON <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>Güncelleştirme Yapılacaklar öğesi</td>
      <td>PUT</td>
      <td>/api/todoitems/</td>
      <td>Biçimlendirilmiş JSON <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>Yapılacaklar öğesi silme</td>
      <td>DELETE</td>
      <td>/api/todoitems/{id}</td>
      <td></td>
    </tr>
  </tbody>
</table>

URI'ler çoğunluğu dahil `TodoItem` yolunda kimliği. Örneğin, silmek için `TodoItem` özelliği kimliği `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, istemci bir silme isteği gönderir `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`. Örnek uygulama kullanılan veri modeli hakkında daha fazla bilgi için bkz: [modelleme verileri](~/xamarin-forms/data-cloud/walkthrough.md).

Web API çerçevesi bir istek aldığında bir eylem isteği yönlendirir. Yalnızca ortak yöntemleri bu eylemlerdir `TodoItemsController` sınıfı. Çerçeve, aşağıdaki kod örneğinde gösterildiği bir isteğe yanıt olarak çağırmak için hangi eylemini belirlemek için bir yönlendirme tablosu kullanır:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Yönlendirme tablosu bir rota şablonu içerir ve Web API çerçevesi bir HTTP isteği aldığında, yönlendirme tablosundaki rota şablonu karşı URI eşleştirmeye çalışır. Eşleşen bir varsa rota istemci 404 (bulunamadı) hatası alır bulunamıyor. Eşleşen bir rota bulunursa, Web API denetleyici ve eylem şu şekilde seçer:

- Web API denetleyicisi bulmak için değerine "denetleyici" ekler *{controller}* değişkeni.
- Eylem bulmak için Web API at HTTP yöntemi arar ve bir öznitelik olarak aynı HTTP yöntemi ile donatılmış denetleyici eylemleri bakar.
- *{İd}* yer tutucu değişkenin bir eylem parametresinin eşlenmedi.

REST hizmeti temel kimlik doğrulaması kullanır. Daha fazla bilgi için bkz: [bir RESTful web hizmeti kimlik doğrulama](~/xamarin-forms/data-cloud/authentication/rest.md). ASP.NET Web API yönlendirme hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de yönlendirme](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) ASP.NET Web sitesi. ASP.NET Core kullanarak REST hizmeti oluşturma hakkında daha fazla bilgi için bkz: [yerel mobil uygulamalar için arka uç hizmetlerini oluşturma](/aspnet/core/mobile/native-mobile-backend/).

> [!NOTE]
> Örnek uygulaması web hizmeti için salt okunur erişim sağlayan Xamarin barındırılan REST hizmeti kullanır. Bu nedenle, oluşturmak, güncelleştirmek ve verileri silme işlemleri uygulamada kullanılan verileri değiştirmez. Ancak, REST hizmeti barındırılabilir bir sürümü kullanılabilir **TodoRESTService** eşlik eden klasör [örnek koduna](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/).
> REST hizmeti kendiniz barındırıyorsanız, tam verir oluşturmak, güncelleştirmek için okuma ve verilere erişim silin.

`HttpClient` Sınıfı, HTTP üzerinden istekleri alıp göndermek için kullanılır. Kaynak tanımlanan bir URİ'den HTTP yanıtını alma ve HTTP istekleri göndermek için işlevsellik sağlar. Her istek eşzamansız bir işlem olarak gönderilir. Zaman uyumsuz işlemleri hakkında daha fazla bilgi için bkz: [zaman uyumsuz desteğine genel bakış](~/cross-platform/platform/async.md).

`HttpResponseMessage` Sınıfı, bir HTTP isteği yapıldıktan sonra web hizmetinden alınan bir HTTP yanıt iletisini temsil eder. Durum kodu, üstbilgiler ve her gövde dahil yanıt hakkında bilgi içerir. `HttpContent` Sınıfı temsil eder HTTP gövdesi ve içerik üstbilgileri gibi `Content-Type` ve `Content-Encoding`. İçeriği herhangi birini kullanarak okunabilir `ReadAs` yöntemleri gibi `ReadAsStringAsync` ve `ReadAsByteArrayAsync`, bağlı veri biçimi.

### <a name="creating-the-httpclient-object"></a>HTTPClient nesnesi oluşturma

`HttpClient` Örneği aşağıdaki kod örneğinde gösterildiği gibi HTTP isteklerini duruma getirmek için uygulamanın gerekli olduğu sürece, nesne için yaşadığı böylece sınıf düzeyinde bildirilen:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
    client.MaxResponseContentBufferSize = 256000;
  }
  ...
}
```

`HttpClient.MaxResponseContentBufferSize` Özelliği, HTTP yanıt iletisi içeriği okurken arabelleğe alınacak bayt sayısını belirtmek için kullanılır. Tamsayı en büyük boyutu bu özelliğin varsayılan boyutudur. Bu nedenle, uygulama web hizmetinden bir yanıt olarak kabul veri miktarını sınırlamak için özellik daha küçük bir değere koruyucu olarak ayarlanır.

### <a name="retrieving-data"></a>Veri alma

`HttpClient.GetAsync` Yöntemi kullanılan URI tarafından belirtilen web hizmeti GET isteği gönderin ve ardından web hizmetinden yanıt almak için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));
  ...
  var response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode) {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

REST hizmeti bir HTTP durum kodu gönderir `HttpResponseMessage.IsSuccessStatusCode` HTTP isteği başarılı veya başarısız olup olmadığını belirtmek için özellik. Bu işlem için REST hizmeti, HTTP durum kodu 200 (Tamam) istek başarılı oldu ve istenen bilgileri yanıtta olduğunu gösteren yanıt gönderir.

HTTP işlemi başarılı olursa yanıt içeriğini görüntülenmek üzere okunur. `HttpResponseMessage.Content` Özelliği, HTTP yanıtının içeriğini temsil eder ve `HttpContent.ReadAsStringAsync` yöntemi bir dizeye zaman uyumsuz HTTP içeriğini yazar. Bu içerik için JSON öğesinden sonra dönüştürülür bir `List` , `TodoItem` örnekleri.

### <a name="creating-data"></a>Veri oluşturma

`HttpClient.PostAsync` Yöntemi URI tarafından belirtilen web hizmeti POST isteğini göndermek için kullanılır ve ardından aşağıdaki kod örneğinde gösterildiği gibi web hizmetinden yanıt almak için:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem) {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully saved.");

  }
  ...
}
```

`TodoItem` Örnek web hizmetine göndermek için bir JSON yükü dönüştürülür. Bu yük istekte önce web hizmetine gönderilen HTTP İçerik gövdesinde katıştırılır `PostAsync` yöntemi.

REST hizmeti bir HTTP durum kodu gönderir `HttpResponseMessage.IsSuccessStatusCode` HTTP isteği başarılı veya başarısız olup olmadığını belirtmek için özellik. Bu işlem için ortak yanıtları şunlardır:

- **201 (oluşturuldu)** – yanıt gönderilmeden önce oluşturulan içinde yeni bir kaynak isteği ile sonuçlandı.
- **400 (Hatalı istek)** – istek sunucu tarafından anlaşılamıyor.
- **409 (Çakışma)** – istek sunucu üzerindeki bir çakışma nedeniyle gerçekleştirilebilmesi değil.

### <a name="updating-data"></a>Verileri güncelleştirme

`HttpClient.PutAsync` Yöntemi kullanılan URI tarafından belirtilen web hizmeti PUT isteği gönderin ve ardından web hizmetinden yanıt almak için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```
İşlemi `PutAsync` yöntemi ile aynıdır `PostAsync` veri web hizmeti oluşturmak için kullanılan yöntem. Ancak, web hizmetinden gönderilen olası yanıtların farklı.

REST hizmeti bir HTTP durum kodu gönderir `HttpResponseMessage.IsSuccessStatusCode` HTTP isteği başarılı veya başarısız olup olmadığını belirtmek için özellik. Bu işlem için ortak yanıtları şunlardır:

- **204 (NO içerik)** – istek başarıyla işlendi ve yanıt kasıtlı olarak boştur.
- **400 (Hatalı istek)** – istek sunucu tarafından anlaşılamıyor.
- **404 (bulunamadı)** – istenen kaynak sunucuda yok.

### <a name="deleting-data"></a>Verileri silme

`HttpClient.DeleteAsync` Yöntemi kullanılan URI tarafından belirtilen web hizmeti DELETE isteği gönderin ve ardından web hizmetinden yanıt almak için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/{0}
  var uri = new Uri (string.Format (Constants.RestUrl, id));
  ...
  var response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully deleted.");
  }
  ...
}
```

REST hizmeti bir HTTP durum kodu gönderir `HttpResponseMessage.IsSuccessStatusCode` HTTP isteği başarılı veya başarısız olup olmadığını belirtmek için özellik. Bu işlem için ortak yanıtları şunlardır:

- **204 (NO içerik)** – istek başarıyla işlendi ve yanıt kasıtlı olarak boştur.
- **400 (Hatalı istek)** – istek sunucu tarafından anlaşılamıyor.
- **404 (bulunamadı)** – istenen kaynak sunucuda yok.

## <a name="summary"></a>Özet

Bu makalede bir Xamarin.Forms uygulaması RESTful web hizmetinden kullanma incelenmesi kullanarak `HttpClient` sınıfı. REST basitliği mobil uygulamalarda web hizmetlerine erişme için birincil yöntem olmasına yardımcı oldu.


## <a name="related-links"></a>İlgili bağlantılar

- [Yerel Mobil Uygulamalar için Arka Uç Hizmetleri Oluşturma](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
