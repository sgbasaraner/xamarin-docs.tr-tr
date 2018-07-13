---
title: Uzak verilere erişme
description: Bu bölümde, nasıl hizmetine mobil uygulamayı kapsayıcılı mikro hizmetler verilere erişen açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 009a4025bc9df6f657964b7e97e559635ef0a929
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996171"
---
# <a name="accessing-remote-data"></a>Uzak verilere erişme

Birçok modern web tabanlı çözümlerine işlevselliği için Uzak istemci uygulamaları sağlamak için web sunucuları tarafından barındırılan web hizmetleri. Bir web API'sini bir web hizmetini gösteren işlemleri oluşturur.

İstemci uygulamaları veri veya API kullanıma sunduğu işlemleri nasıl uygulandığını farkında olmadan web API'sini kullanmaya başlatabilmeniz gerekir. Bu, API ve istemci uygulamaları ve web hizmeti arasında alınıp verilen verilerin yapısını hangi veri biçimleri hakkında kabul etmek bir istemci uygulaması ve web hizmeti sağlayan ortak standartları tarafından uyduğundan gerektirir.

## <a name="introduction-to-representational-state-transfer"></a>Temsili durum aktarımı giriş

Temsili durum aktarımı (REST), Hiper medyayı temel alan dağıtılmış sistemler oluşturmaya yönelik bir mimari stilidir. REST modeli birincil avantajı açık standartlar dayanıyor ve model için herhangi bir uygulamaya erişen istemci uygulamaları veya uygulama bağlamayan ' dir. Bu nedenle, bir REST web hizmeti, Microsoft ASP.NET Core MVC kullanarak uygulanabilir ve herhangi bir dil ve HTTP istekleri oluşturabilen ve HTTP yanıtlarını ayrıştırabilen Araç Takımı'nı kullanarak istemci uygulamaları geliştirme.

REST model nesneleri ve hizmetler için kaynak olarak adlandırılan bir ağ üzerinden temsil etmek için gezinme düzenini kullanır. Genellikle REST uygulayan sistemleri, bu kaynaklara erişmek için istekleri iletmek için HTTP protokolünü kullanır. Bu tür sistemlerde bir istemci uygulaması kaynağını tanımlayan bir URI ve bu kaynak üzerinde gerçekleştirilecek bir işlemi belirten bir HTTP yöntemini (örneğin, GET, POST, PUT veya Sil) biçiminde bir istek gönderir. HTTP isteği gövdesinin işlemi gerçekleştirmek için gerekli tüm verileri içerir.

> [!NOTE]
> REST, durum bilgisiz istek modeli tanımlar. Bu nedenle, HTTP istekleri bağımsız olmalıdır ve herhangi bir sırayla gerçekleşebilir.

Bir REST yanıtı isteği yapar standart HTTP durum kodları kullanın. Örneğin, bulunamıyor veya belirtilen kaynak silme başarısız bir istek HTTP durum kodu 404 (bulunamadı) kodunu içeren bir yanıtı döndürmelidir sırada geçerli veri döndüren bir istek HTTP yanıt kodu 200 (Tamam) içermelidir.

RESTful web API'si, bağlı kaynakları kümesini sunar ve bu kaynakları düzenlemek ve bunlar arasında kolayca gitmek bir uygulama temel işlemler sağlar. Bu nedenle, tipik bir RESTful web API'si oluşturan bir URI'leri kullanıma sunduğu veri ve tesis sağlanan kullanım doğrultusunda bu veriler üzerinde çalışması için HTTP tarafından yönlendirilmiş.

Bir HTTP isteği ve karşılık gelen yanıt iletilerini web sunucusundan bir istemci uygulaması tarafından dahil edilen veri biçimler medya türleri olarak bilinen, çeşitli sunulabilir. Bir istemci uygulama bir ileti gövdesinde veri döndüren bir isteği gönderdiğinde, işleyebileceği medya türlerini belirtebilirsiniz `Accept` isteği üstbilgisi. Web sunucusu bu ortam türünü destekliyorsa, birlikte içeren bir yanıt yanıtlayabilir `Content-Type` iletisinin gövdesindeki verilerin biçimi belirten üst bilgisi. Ardından yanıt iletisi ayrıştırma ve sonuçları ileti gövdesinde uygun şekilde yorumlamak için istemci uygulamanın sorumluluğundadır.

REST hakkında daha fazla bilgi için bkz: [API tasarımı](/azure/architecture/best-practices/api-design/) ve [API uygulaması](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>RESTful API'lerini kullanma

Model-View-ViewModel (MVVM) desenini yanı sıra, etki alanı varlıklarının uygulamada kullanılan desen temsil model öğelerini hizmetine mobil uygulama kullanılır. Hizmetine başvuru uygulaması denetleyici ve depo sınıflarda kabul edin ve çoğu bu modeli nesnelerini döndürür. Bu nedenle, bunlar mobil uygulama ile kapsayıcılı mikro hizmetler arasında aktarılan tüm verileri tutan veri aktarımı nesneleri (Dto) olarak kullanılır. Verileri aktarmak ve bir web hizmetinden veri almak için Dto'lar kullanmanın ana avantajı, tek bir uzak çağrı daha fazla veri aktarımı tarafından yapılması gereken uzak çağrı sayısı uygulama azaltmak ' dir.

### <a name="making-web-requests"></a>Web istekleri yapma

Hizmetine mobil uygulama kullandığı `HttpClient` medya türü olarak kullanılan JSON ile HTTP üzerinden isteğinde bulunmak için sınıf. Bu sınıf, zaman uyumsuz olarak HTTP istekleri göndermek için işlevsellik sağlar ve kaynak belirtilen bir URI'den HTTP yanıtlarını alma. `HttpResponseMessage` Sınıfı, bir HTTP istek yapıldıktan sonra bir REST API'SİNDEN alınan HTTP yanıt iletisini temsil eder. Yanıt durum kodu, üst bilgileri ve her gövde dahil olmak üzere hakkında bilgi içerir. `HttpContent` Sınıfı temsil eder HTTP gövdesini ve içerik üstbilgileri gibi `Content-Type` ve `Content-Encoding`. İçerik herhangi biri kullanılarak okunabilir `ReadAs` yöntemleri gibi `ReadAsStringAsync` ve `ReadAsByteArrayAsync`verilerin biçimi bağlı olarak.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Bir GET isteği yapan

`CatalogService` Sınıfı, veri alma sürecini Kataloğu mikro hizmet yönetmek için kullanılır. İçinde `RegisterDependencies` yönteminde `ViewModelLocator` sınıfı `CatalogService` sınıfı kayıtlı bir tür eşlemesine yönelik olarak `ICatalogService` Autofac bağımlılık ekleme kapsayıcısını türüyle. Ardından, örneği `CatalogViewModel` sınıf oluşturulduğu kabul oluşturucusuna bir `ICatalogService` yazın, hangi Autofac çözümler, örneği döndüren `CatalogService` sınıfı. Bağımlılık ekleme hakkında daha fazla bilgi için bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Şekil 10-1 olarak görüntülemek için katalog mikro hizmet Kataloğu veri okuma sınıfları etkileşimi gösterir `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Veri Kataloğu mikro hizmet alınırken")](accessing-remote-data-images/catalogdata-large.png#lightbox "Kataloğu mikro hizmet veriler alınıyor")

**Şekil 10-1**: Katalog mikro hizmet veriler alınıyor

Zaman `CatalogView` için çıkıldığında `OnInitialize` yönteminde `CatalogViewModel` sınıfı çağrılır. Bu yöntem aşağıdaki kod örneğinde gösterildiği gibi Kataloğu mikro hizmet katalog verilerini alır:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Bu yöntemin çağırdığı `GetCatalogAsync` yöntemi `CatalogService` içine eklenen örneği `CatalogViewModel` Autofac tarafından. Aşağıdaki örnekte gösterildiği kod `GetCatalogAsync` yöntemi:

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

Bu yöntem, istek gönderilir kaynağı tanımlayan URI'yi oluşturur ve kullanır `RequestProvider` sonuçları döndürmeden önce kaynak üzerinde GET HTTP yöntemini çağırmak için sınıf `CatalogViewModel`. `RequestProvider` Sınıfı, bu kaynak üzerinde gerçekleştirilecek bir işlemi belirten bir HTTP yöntemini kaynağını tanımlayan bir URI biçiminde bir istek gönderdiğini işlevsellik içerir ve tüm verileri içeren bir gövdesi gerekli işlemi gerçekleştirmek için. Hakkında nasıl bilgi `RequestProvider` sınıfı içine eklenmiş `CatalogService class`, bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Aşağıdaki örnekte gösterildiği kod `GetAsync` yönteminde `RequestProvider` sınıfı:

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Bu yöntemin çağırdığı `CreateHttpClient` örneğini döndüren bir yöntem `HttpClient` sınıfının uygun üst bilgileri. Ardından depolanıyor yanıt URI'si tarafından tanımlanan kaynak için zaman uyumsuz bir GET isteği gönderdiğinde `HttpResponseMessage` örneği. `HandleResponse` Yöntemi ardından çağrılır, HTTP durum kodu başarılı yanıt içermiyorsa, bir özel durum oluşturur. Yanıt için json'dan dönüştürülmüş bir dize olarak okuma sonra bir `CatalogRoot` nesne ve döndürülen `CatalogService`.

`CreateHttpClient` Yöntemi, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

Bu yöntem yeni bir örneğini oluşturur `HttpClient` sınıfı ve kümelerini `Accept` tarafından yapılan tüm istekleri üstbilgisinin `HttpClient` için örnek `application/json`, JSON kullanılarak Biçimlendirilecek herhangi bir yanıt içeriğini beklediğini belirtir. Ardından, bağımsız değişkeni olarak bir erişim belirteci geçirildiyse `CreateHttpClient` yöntemi eklenir `Authorization` tarafından yapılan tüm istekleri üstbilgisinin `HttpClient` örneği, dizenin önek `Bearer`. Yetkilendirme hakkında daha fazla bilgi için bkz: [yetkilendirme](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Zaman `GetAsync` yönteminde `RequestProvider` sınıf çağrıları `HttpClient.GetAsync`, `Items` yönteminde `CatalogController` Catalog.API projesinde sınıfı çağrıldığında, aşağıdaki kod örneğinde gösterilen:

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

Bu yöntem, EntityFramework kullanarak SQL veritabanı'ndan katalog verileri alır ve başarılı bir HTTP durum kodunu içeren bir yanıt iletisi olarak döndüren ve bir koleksiyon, JSON biçimli `CatalogItem` örnekleri.

#### <a name="making-a-post-request"></a>Bir POST isteği yapma

`BasketService` Sınıfı veri alma yönetme ve güncelleştirme işlemlerini sepet bir mikro hizmet için kullanılır. İçinde `RegisterDependencies` yönteminde `ViewModelLocator` sınıfı `BasketService` sınıfı kayıtlı bir tür eşlemesine yönelik olarak `IBasketService` Autofac bağımlılık ekleme kapsayıcısını türüyle. Ardından, örneği `BasketViewModel` sınıf oluşturulduğu kabul oluşturucusuna bir `IBasketService` yazın, hangi Autofac çözümler, örneği döndüren `BasketService `sınıfı. Bağımlılık ekleme hakkında daha fazla bilgi için bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Şekil 10-2 gösterir, tarafından görüntülenen sepet veri gönderen sınıflar etkileşimi `BasketView`, sepet mikro hizmet için.

[![](accessing-remote-data-images/basketdata.png "Sepet mikro hizmet için veri gönderen")](accessing-remote-data-images/basketdata-large.png#lightbox "sepet mikro hizmet için veri gönderme")

**Şekil 10-2**: Sepet mikro hizmet için veri gönderme

Bir öğe, alışveriş sepetine eklendiğinde `ReCalculateTotalAsync` yönteminde `BasketViewModel` sınıfı çağrılır. Bu yöntem, sepet öğelerinde toplam değerini güncelleştirir ve sepet mikro hizmet, aşağıdaki kod örneğinde gösterildiği gibi sepet verileri gönderir:

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

Bu yöntemin çağırdığı `UpdateBasketAsync` yöntemi `BasketService` içine eklenen örneği `BasketViewModel` Autofac tarafından. Aşağıdaki yöntem gösterildiği `UpdateBasketAsync` yöntemi:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Bu yöntem, istek gönderilir kaynağı tanımlayan URI'yi oluşturur ve kullanır `RequestProvider` sonuçları döndürmeden önce kaynak üzerinde POST HTTP yöntemini çağırmak için sınıf `BasketViewModel`. Kimlik doğrulama işlemi sırasında bir erişim belirteci elde IdentityServer Not sepet mikro hizmet isteklerini korunmasına yetki vermek için gereklidir. Yetkilendirme hakkında daha fazla bilgi için bkz: [yetkilendirme](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Aşağıdaki kod örneği birini gösterir `PostAsync` yöntemleri `RequestProvider` sınıfı:

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Bu yöntemin çağırdığı `CreateHttpClient` örneğini döndüren bir yöntem `HttpClient` sınıfının uygun üst bilgileri. Ardından JSON biçimini ve depolanıyor yanıt gönderilmesini sıralı sepete verilerle URI tarafından tanımlanan kaynak için zaman uyumsuz bir POST isteği gönderir `HttpResponseMessage` örneği. `HandleResponse` Yöntemi ardından çağrılır, HTTP durum kodu başarılı yanıt içermiyorsa, bir özel durum oluşturur. Ardından, yanıt için json'dan dönüştürülmüş bir dize olarak okuma bir `CustomerBasket` nesne ve döndürülen `BasketService`. Hakkında daha fazla bilgi için `CreateHttpClient` yöntemi bkz [bir alma isteği yapmadan](#making_a_get_request).

Zaman `PostAsync` yönteminde `RequestProvider` sınıf çağrıları `HttpClient.PostAsync`, `Post` yönteminde `BasketController` Basket.API projesinde sınıfı çağrıldığında, aşağıdaki kod örneğinde gösterilen:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Bu yöntem bir örneği kullanan `RedisBasketRepository` Redis önbelleğine sepet verileri kalıcı hale getirmek için sınıf ve biçimli bir JSON yanı sıra başarılı bir HTTP durum kodu içeren bir yanıt iletisi olarak döndüren `CustomerBasket` örneği.

#### <a name="making-a-delete-request"></a>Bir silme isteği yapan

Şekil 10-3 gösterir sepet mikro hizmet sepet verileri silebilirsiniz sınıflarının etkileşimler `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Sepet mikro hizmet verileri siliniyor")

**Şekil 10-3**: Sepet mikro hizmet verileri silme

Kullanıma alma işlemi çağrıldığında `CheckoutAsync` yönteminde `CheckoutViewModel` sınıfı çağrılır. Bu yöntem, alışveriş sepeti temizlemeden önce aşağıdaki kod örneğinde gösterildiği gibi yeni bir sıra oluşturur:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Bu yöntemin çağırdığı `ClearBasketAsync` yöntemi `BasketService` içine eklenen örneği `CheckoutViewModel` Autofac tarafından. Aşağıdaki yöntem gösterildiği `ClearBasketAsync` yöntemi:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Bu yöntem, istek gönderilir kaynağı tanımlayan URI'yi oluşturur ve kullanır `RequestProvider` kaynakta DELETE HTTP yöntemini çağırmak için sınıf. Kimlik doğrulama işlemi sırasında bir erişim belirteci elde IdentityServer Not sepet mikro hizmet isteklerini korunmasına yetki vermek için gereklidir. Yetkilendirme hakkında daha fazla bilgi için bkz: [yetkilendirme](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Aşağıdaki örnekte gösterildiği kod `DeleteAsync` yönteminde `RequestProvider` sınıfı:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Bu yöntemin çağırdığı `CreateHttpClient` örneğini döndüren bir yöntem `HttpClient` sınıfının uygun üst bilgileri. Ardından, URI tarafından tanımlanan kaynak için zaman uyumsuz bir silme isteği gönderir. Hakkında daha fazla bilgi için `CreateHttpClient` yöntemi bkz [bir alma isteği yapmadan](#making_a_get_request).

Zaman `DeleteAsync` yönteminde `RequestProvider` sınıf çağrıları `HttpClient.DeleteAsync`, `Delete` yönteminde `BasketController` Basket.API projesinde sınıfı çağrıldığında, aşağıdaki kod örneğinde gösterilen:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Bu yöntem bir örneği kullanan `RedisBasketRepository` Redis Önbelleği'ndeki sepet verileri silmek için sınıf.

## <a name="caching-data"></a>Verileri Önbelleğe Alma

Yakın bir konumdaki hızlı depolama için sık erişilen verileri önbelleğe alarak uygulama performansını geliştirilebilir uygulama. Hızlı depolama yakın uygulamaya özgün kaynaktan bulunduğu sonra önbelleğe alma yanıt önemli ölçüde artırabilir veri alınırken zaman.

Önbelleğe alma en yaygın önbelleğe başvurarak uygulama verileri nerede alır anında okuma önbelleği biçimidir. Veriler önbellekte değilse veri deposundan alınmasına ve önbelleğe eklenir. Uygulamaları anında okuma edilgen önbellek düzeni ile önbelleğe alma uygulayabilirsiniz. Bu düzen, öğe şu anda önbellekte olup olmadığını belirler. Öğeyi önbellekte değilse veri deposundan okuma ve önbelleğe eklenir. Daha fazla bilgi için [edilgen önbellek](/azure/architecture/patterns/cache-aside/) deseni.

> [!TIP]
> Sık okunan ve seyrek değişen verileri önbelleğe alın. Bu veriler, isteğe bağlı bir uygulama tarafından alınan ilk kez önbelleğe eklenebilir. Bu uygulamanın veri deposundan verileri yalnızca bir kez getirmesinin gerekli ve sonraki erişimin önbellek kullanılarak yerine getirilebileceği anlamına gelir.

Uygulama hizmetine başvuru gibi dağıtılan uygulamalar ya da aşağıdaki önbellekleri sağlamalıdır:

-   Birden fazla işlem veya makine tarafından erişilebilen paylaşılan önbellek.
-   Uygulamayı çalıştıran cihaz üzerinde verileri yerel olarak tutulduğu özel bir önbellek.

Uygulamanın bir örneğini çalıştıran bir cihaza veri yerel olarak tutulduğu özel bir önbellek hizmetine mobil uygulamayı kullanır. Hizmetine başvuru uygulama tarafından kullanılan önbellek hakkında daha fazla bilgi için bkz. [.NET mikro Hizmetleri: kapsayıcılı .NET uygulamaları mimarisi](https://aka.ms/microservicesebook).

> [!TIP]
> Önbelleğin her an kaybolabilecek geçici veri deposu olarak düşünün. Veri önbelleği yanı sıra özgün veri deposuna karşılandığından emin olun. Önbellek kullanılamaz duruma gelirse veri kaybetme olasılığını ardından en aza indirilir.

### <a name="managing-data-expiration"></a>Veri süre sonunu yönetme

Önbelleğe alınan verilerin her zaman özgün verilerle tutarlı olmasını beklemek zordur. Özgün veri deposundaki verileri, önbelleğe alınmış verilerin eskimesine yol açabilecek neden önbelleğe sonra değiştirebilirsiniz. Bu nedenle, uygulamaları önbellekteki verilerin olabildiğince güncel olmasını sağlamaya yardımcı olan bir strateji yürütmelidir ancak de algılayabilir ve önbellekteki veriler eskidiğinde ortaya çıkan durumları. En önbelleğe alma mekanizması verileri süresi dolacak şekilde yapılandırılması önbelleğini etkinleştirme ve bu nedenle veri süresi dolmuş olabilir süreyi azaltacak.

> [!TIP]
> Varsayılan sona erme süresini ayarlayabilir önbelleği yapılandırırken zaman. Birçok önbellek, verileri geçersiz kılan ve belirli bir süre boyunca erişilemiyorsa önbellekten kaldırır. sona erme uygulayın. Ancak süre sonunu seçerken dikkatli olunmalıdır. Çok kısa duruma getirildiyse, verilerin çok çabuk dolar ve önbelleği avantajları azalır. Bu çok uzun, eski veri riskleri yaptıysanız. Bu nedenle, sona erme zamanı veriler kullanan uygulamalar için erişim düzeni ile eşleşmelidir.

Önbelleğe alınan verilerin süresini sona erdirir, önbellekten kaldırılmalıdır ve uygulama alınması gerekir verileri özgün veri depolamak ve önbelleğe geri yerleştirin.

Veri için çok uzun bir süre kalmasına izin verilirse bir önbellek dolması da mümkündür. Bu nedenle, önbelleğe yeni öğe eklemek için istekleri olarak bilinen bir işlemde bazı öğeleri kaldırmak için gerekli olabilir *çıkarma*. Önbelleğe alma hizmetleri genellikle verileri en son kullanılan olarak çıkarın. Ancak, en son kullanılan ve ilk-giren ilk çıkar dahil olmak üzere diğer çıkarma ilkeleri vardır. Daha fazla bilgi için [önbelleğe alma Kılavuzu](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Görüntüleri önbelleğe alma

Hizmetine mobil uygulama, önbelleğe alınmasını yararlanan uzaktan ürün görüntüleri kullanır. Bu görüntüleri tarafından görüntülenen [ `Image` ](xref:Xamarin.Forms.Image) denetimi ve `CachedImage` denetimi tarafından sağlanan [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) kitaplığı.

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) denetimi, indirilen görüntülerini önbelleğe almayı destekler. Önbelleğe alma, varsayılan olarak etkindir ve görüntünün yerel olarak 24 saat boyunca saklar. Ayrıca, sona erme saati ile yapılandırılabilir [ `CacheValidity` ](xref:Xamarin.Forms.UriImageSource.CacheValidity) özelliği. Daha fazla bilgi için [yüklenen görüntü önbelleğe alma](~/xamarin-forms/user-interface/images.md#Image_Caching).

FFImageLoading'ın `CachedImage` denetimi, bir Xamarin.Forms ardılı [ `Image` ](xref:Xamarin.Forms.Image) ek işlevsellik sağlayan ek özellikler sağlayan denetimi. Bu işlev arasında hata destekleyen ve görüntü yer tutucuları yüklenirken yapılandırılabilir önbelleğe denetim sağlar. Aşağıdaki kod örneği hizmetine mobil uygulamayı nasıl kullandığını gösteren `CachedImage` denetim `ProductTemplate`, tarafından kullanılan veri şablonu [ `ListView` ](xref:Xamarin.Forms.ListView) denetim `CatalogView`:

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

`CachedImage` Denetim kümeleri `LoadingPlaceholder` ve `ErrorPlaceholder` görüntülerine platforma özgü özellikler. `LoadingPlaceholder` Özelliği tarafından belirtilen görüntünün çalışırken gösterilecek resmi belirtir `Source` özelliği alınır ve `ErrorPlaceholder` özelliği görüntü alınmaya çalışılırken bir hata oluşması halinde gösterilecek resmi belirtir tarafından belirtilen `Source` özelliği.

Adından da anlaşılacağı gibi `CachedImage` denetimi değeri tarafından belirtilen süre için cihazda uzak görüntüleri önbellekleri `CacheDuration` özelliği. Bu özellik değeri açıkça ayarlanmamışsa, varsayılan değer 30 gün içinde uygulanır.

## <a name="increasing-resilience"></a>Artan dayanıklılık

Uzak hizmetler ve kaynaklarla iletişim kuran tüm uygulamaların geçici hatalara karşı duyarlı olması gerekir. Geçici hataların hizmetlerine ağ bağlantısının anlık kaybı, bir hizmet veya Hizmet meşgul olduğunda gerçekleşen zaman aşımları geçici olarak kullanım dışı kalması içerir. Bu hatalar genellikle kendi kendini düzeltir ve uygun bir gecikmeden sonra eylem yinelenirse başarılı olma olasılığı yüksektir.

Öngörülebilir tüm koşullar altında kapsamlı sınanmıştır bile geçici hataların algılanan bir uygulama kalitesini üzerinde muazzam bir etkisi olabilir. Uzak Hizmetleri ile iletişim kuran bir uygulamayı güvenilir bir şekilde işlediğinden emin olmak için aşağıdakilerin tümü yapabilirsiniz olmalıdır:

-   Bunlar ortaya çıktığında ve geçici hataları olup olmadığını belirlemek olduğunda hataları algılayın.
-   Hataya geçici ve işlem yapılan yeniden deneme sayısını izlemek olası olduğunu belirlerse işlemi yeniden deneyin.
-   Yeniden denemeler, başarısız bir girişimden sonra yapılacak girişimleri ve eylemleri arasındaki gecikmeyi belirtir bir uygun bir yeniden deneme stratejisi kullanın.

Bu geçici hata işleme, yeniden deneme düzeni uygulayan kodu uzak bir hizmete erişmeye yönelik tüm girişimleri sarmalama tarafından gerçekleştirilebilir.

### <a name="retry-pattern"></a>Yeniden deneme düzeni

Uygulama uzak bir hizmete istek göndermeye çalıştığında bir hata algılarsa, bunu aşağıdaki şekilde işleyebilir:

-   İşlemi yeniden deneniyor. Uygulama başarısız olan isteği hemen yeniden.
-   Bir gecikmeden sonra işlemi yeniden deneniyor. Uygulama isteği yeniden denemeden önce uygun bir süre için beklemeniz gerekir.
-   İşlemi iptal ediliyor. Uygulama, işlemi iptal edin ve bir özel durum bildirmelidir.

Yeniden deneme stratejisi, uygulamanın iş gereksinimlerini karşılamak üzere ayarlanmalıdır. Örneğin, yeniden deneme sayısı en iyi duruma getirme ve yeniden deneme aralığı denenen işlemi için önemlidir. İşlemi bir kullanıcı etkileşimi bir parçası ise, yeniden deneme aralığı, kısa ve kullanıcıların bir yanıtı bekle yapmaktan kaçınmak için yalnızca birkaç yeniden deneme olmalıdır. İşlemi iptal etme veya iş akışı yeniden başlatılması pahalı ya da zaman harcayan olduğu uzun süre çalışan bir iş akışının parçası ise, girişimler arasında daha uzun süre bekleyin ve daha fazla yeniden deneme için uygundur.

> [!NOTE]
> Bir agresif yeniden deneme stratejisi denemeleri ve çok sayıda yeniden deneme arasındaki en az bir gecikmeyle yakın veya kapasitesini çalıştıran uzak bir hizmete düşebilir. Ayrıca, sürekli başarısız olan bir işlemi gerçekleştirmeye çalışıyor, böyle bir yeniden deneme stratejisi ayrıca uygulamanın yanıt verme hızını etkileyebilir.

İstek bir yeniden deneme sayısı sonra yine başarısız olursa, uygulamanın aynı kaynağa giden diğer istekleri önlemek ve bir hata raporu için iyidir. Ardından, belirli bir süre sonra uygulama bir veya daha fazla isteği başarılı olup olmadıklarını görmek için kaynak yapabilir. Daha fazla bilgi için [devre kesici düzeni](#circuit_breaker_pattern).

> [!TIP]
> Hiçbir zaman sonsuz bir yeniden deneme mekanizması uygular. Sınırlı sayıda yeniden deneme kullanın veya uygulayan [devre kesici](/azure/architecture/patterns/circuit-breaker/) bir hizmet, kurtarılır izin vermek için desen.

Hizmetine mobil uygulaması şu anda yeniden deneme düzeni RESTful web istekleri yaparken uygulamıyor. Ancak, `CachedImage` denetimi tarafından sağlanan [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) kitaplık görüntü yükleme yeniden denenerek geçici hata işleme destekler. Görüntü yükleme başarısız olursa daha fazla girişimi yapılamaz. Deneme sayısı tarafından belirtilen `RetryCount` tarafından belirtilen bir gecikmeden sonra özellik ve yeniden denemeler gerçekleşir `RetryDelay` özelliği. Bu özellik değerlerini olmayan açıkça ayarlarsanız, kendi varsayılan değerlerini uygulanır – 3 `RetryCount` özelliği ve için 250ms `RetryDelay` özelliği. Hakkında daha fazla bilgi için `CachedImage` denetlemek için bkz: [önbelleğe alma görüntüleri](#caching_images).

Hizmetine başvuru uygulaması, yeniden deneme düzeni uygulayın. Daha fazla bilgi için yeniden deneme düzeni ile bir araya getirilebileceğini öğrenin hakkında ayrıntılı bilgi de dahil olmak üzere `HttpClient` sınıfı [.NET mikro Hizmetleri: kapsayıcılı .NET uygulamaları mimarisi](https://aka.ms/microservicesebook).

Yeniden deneme düzeni hakkında daha fazla bilgi için bkz: [yeniden](/azure/architecture/patterns/retry/) deseni.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Devre kesici düzeni

Bazı durumlarda, hataları düzeltmek uzun beklenen olaylar nedeniyle oluşabilir. Bu hataların kısmi bir bağlantı kaybına karşı bir hizmetin tamamen çökmesi arasında değişebilir. Bu gibi durumlarda anlamsız bir uygulamanın, başarılı ve bunun yerine işlemin başarısız olduğunu kabul etmeli ve bu hatayı uygun şekilde işlemek olası bir işlemi yeniden deneyin.

Devre kesici düzeni, bir uygulama hatası giderilmiş olup olmadığını algılamak uygulamayı etkinleştirirken, başarısız olma olasılığı yüksek bir işlemi yürütmek sürekli olarak çalışmasını engelleyebilir.

> [!NOTE]
> Devre kesici düzeninin amacı yeniden deneme düzeni farklıdır. Yeniden deneme düzeni, bir işlem başarılı beklentisi, yeniden denemek bir uygulama sağlar. Devre kesici düzeni, bir uygulama başarısız olma olasılığı yüksek bir işlemi gerçekleştirmesini engeller.

Devre kesici, başarısız olabilecek işlemler için bir proxy görevi görür. Proxy, ortaya çıkan son başarısızlıkların sayısını izlemek ve işlemi devam etmek için veya bir özel durum hemen getirilecek izin verilip verilmeyeceğine karar vermek için bu bilgileri kullanın.

Hizmetine mobil uygulaması şu anda devre kesici düzeni uygulamıyor. Ancak, hizmetine yapar. Daha fazla bilgi için [.NET mikro Hizmetleri: kapsayıcılı .NET uygulamaları mimarisi](https://aka.ms/microservicesebook).

> [!TIP]
> Yeniden deneme ve devre kesici desenleri birleştirin. Uygulama, devre kesici üzerinden işlemi çağırmak için yeniden deneme düzenini kullanarak yeniden deneyin ve devre kesici desenleri birleştirebilirsiniz. Ancak, yeniden deneme mantığının devre kesici tarafından döndürülen tüm özel durumlara duyarlı olması ve bu devre kesici bir hataya geçici olmadığını gösteriyorsa yeniden denemeyi bırakması gerekir.

Devre kesici düzeni hakkında daha fazla bilgi için bkz: [devre kesici](/azure/architecture/patterns/circuit-breaker/) deseni.

## <a name="summary"></a>Özet

Birçok modern web tabanlı çözümlerine işlevselliği için Uzak istemci uygulamaları sağlamak için web sunucuları tarafından barındırılan web hizmetleri. Bir web API'sini bir web hizmetini gösteren işlemleri oluşturur ve istemci uygulamaları veri veya API kullanıma sunduğu işlemleri nasıl uygulandığını farkında olmadan web API'sini kullanmaya başlayabilirsiniz olmalıdır.

Yakın bir konumdaki hızlı depolama için sık erişilen verileri önbelleğe alarak uygulama performansını geliştirilebilir uygulama. Uygulamaları anında okuma edilgen önbellek düzeni ile önbelleğe alma uygulayabilirsiniz. Bu düzen, öğe şu anda önbellekte olup olmadığını belirler. Öğeyi önbellekte değilse veri deposundan okuma ve önbelleğe eklenir.

Web API'leri ile iletişim kurarken, uygulamaların geçici hatalara karşı duyarlı olması gerekir. Geçici hataların hizmetlerine ağ bağlantısının anlık kaybı, bir hizmet veya Hizmet meşgul olduğunda gerçekleşen zaman aşımları geçici olarak kullanım dışı kalması içerir. Bu hatalar genellikle kendi kendini düzeltir ve uygun bir gecikmeden sonra eylem yinelenirse sonra başarılı olma olasılığı yüksektir. Bu nedenle, uygulamaları bir geçici hata işleme mekanizması uygulayan kodu bir web API'sine erişmeye yönelik tüm girişimleri sarmalamalıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
