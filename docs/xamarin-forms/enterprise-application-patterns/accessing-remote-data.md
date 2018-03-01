---
title: "Uzak verilere erişme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: a219a5ed4045bff639f29fd49ef5288139140135
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="accessing-remote-data"></a>Uzak verilere erişme

Birçok modern web tabanlı çözüm olun işlevselliği için Uzak istemci uygulamaları sağlamak için web sunucuları tarafından barındırılan bir web Hizmetleri kullanın. Bir web hizmeti sunan operations web API'si oluşturur.

İstemci uygulamaları, verileri veya API sunan işlemlerini nasıl uygulandığını bilmeden web API'si verecek olmalıdır. Bu API hangi veri biçimleri ve istemci uygulamaları ve web hizmeti arasında alınıp verilerin yapısını kabul etmesi bir istemci uygulaması ve web hizmeti etkinleştirmek ortak standartları tarafından uyduğundan gerektirir.

## <a name="introduction-to-representational-state-transfer"></a>Temsili durum aktarımı giriş

Temsili durum aktarımı (REST) tabanlı iletilir üzerinde dağıtılmış sistemler oluşturmak için bir mimari stili ' dir. Bir birincil modelin avantajı, REST açık standartlar temelinde ve model veya herhangi bir belirli uygulaması erişim istemci uygulamaları uygulanmasına yönelik bağlama değil ' dir. Bu nedenle, Microsoft ASP.NET Core MVC kullanarak bir REST web hizmeti uygulanabilir ve herhangi bir dil ve HTTP istekleri oluşturmak ve HTTP yanıtlarını ayrıştırma araç takımı kullanarak istemci uygulamaları geliştirme.

REST modeli gezinme düzenini kaynaklar olarak başvurulan bir ağ üzerinden nesne ve hizmetlerini göstermek için kullanır. REST genellikle uygulama sistemlerinde, bu kaynakları erişim istekleri iletmek için HTTP protokolünü kullanır. Bu tür sistemlerinde, istemci uygulaması bir kaynağı tanımlayan bir URI ve bu kaynak üzerinde gerçekleştirilecek işlemi gösteren bir HTTP yöntemini (örneğin, GET, POST, PUT veya Sil) biçiminde bir istek gönderir. HTTP isteğinin gövdesi işlemi gerçekleştirmek için gerekli tüm verileri içerir.

> [!NOTE]
> REST durum bilgisiz isteği modelini tanımlar. Bu nedenle, HTTP isteklerini bağımsız olmalıdır ve herhangi bir sırada ortaya çıkabilir.

Bir REST yanıttan isteği yapar standart HTTP durum kodları kullanın. Örneğin, geçerli veri döndüren bir istek bulmak veya belirtilen kaynağa silmek için başarısız bir istek HTTP durum kodu 404 (bulunamadı) içeren bir yanıtı döndürmelidir sırada HTTP yanıt kodu 200 (Tamam) içermelidir.

RESTful web API'si bağlı kaynak kümesi sunar ve bu kaynakları yönetmek ve kolayca bunlar arasında gezinmek bir uygulamanın temel işlemlerini sağlar. Bu nedenle, tipik RESTful web API'si oluşturan URI'ler onu gösteren veri ve tesis sağlanan kullanım doğrultusunda bu veriler üzerinde çalışmak için HTTP tarafından yönlendirilmiş.

Bir HTTP isteği ve karşılık gelen yanıt iletilerini web sunucusundan bir istemci uygulaması tarafından dahil edilen veri biçimleri, medya türleri olarak bilinen çeşitli sunulan. Bir istemci uygulaması bir ileti gövdesinde veri döndüren bir istek gönderdiğinde, işleyebilir medya türlerini belirtebilirsiniz `Accept` isteği üstbilgisi. Web sunucusu bu ortam türünü destekliyorsa, içeren bir Yanıtla yanıtlayabilir `Content-Type` üstbilgi iletisinin gövdesinde veri biçimini belirtir. Bu ardından yanıt iletisi ayrıştırma ve ileti gövdesini sonuçlarında uygun şekilde yorumlamak için istemci uygulamanın sorumluluğundadır.

REST hakkında daha fazla bilgi için bkz: [API tasarım](/azure/architecture/best-practices/api-design/) ve [API uygulaması](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>RESTful API'lerini kullanma

Model-View-ViewModel (MVVM) düzeni ve etki alanı varlıkları uygulamada kullanılan deseni temsil model öğelerini eShopOnContainers mobil uygulama kullanır. EShopOnContainers başvuru uygulaması denetleyici ve depo sınıflarda kabul edip bu modeli nesnelerin çoğunu dönün. Bu nedenle, bunlar mobil uygulama ve kapsayıcılı mikro hizmetler arasında aktarılan tüm verileri tutmak veri aktarımı nesne (DTOs) olarak kullanılır. Verileri geçirmek ve bir web hizmetinden veri almak için DTOs kullanmanın ana avantajı, tek bir uzak çağrı daha fazla veri ileterek yapılması gereken uzak çağrı sayısı uygulama azaltmak ' dir.

### <a name="making-web-requests"></a>Yapma Web istekleri

EShopOnContainers mobil uygulamanın kullandığı `HttpClient` medya türü olarak kullanılan JSON ile HTTP üzerinden istekler yapmasını sınıfı. Bu sınıf, zaman uyumsuz HTTP istekleri göndermek için işlevsellik sağlar ve kaynak tanımlanan bir URİ'den HTTP yanıtını alma. `HttpResponseMessage` Sınıfı, bir HTTP isteği yapıldıktan sonra bir REST API öğesinden alınan bir HTTP yanıt iletisini temsil eder. Durum kodu, üstbilgiler ve her gövde dahil yanıt hakkında bilgi içerir. `HttpContent` Sınıfı temsil eder HTTP gövdesi ve içerik üstbilgileri gibi `Content-Type` ve `Content-Encoding`. İçeriği herhangi birini kullanarak okunabilir `ReadAs` yöntemleri gibi `ReadAsStringAsync` ve `ReadAsByteArrayAsync`bağlı olarak veri biçimi.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Bir GET isteği yapan

`CatalogService` Sınıfı katalog mikro veri alma sürecini yönetmek için kullanılır. İçinde `RegisterDependencies` yönteminde `ViewModelLocator` sınıfı, `CatalogService` sınıfı karşı türü eşlemesi olarak kayıtlı `ICatalogService` Autofac bağımlılık ekleme kapsayıcısını türüyle. Ardından, bir örneği olduğunda `CatalogViewModel` kurucusu kabul sınıfı oluşturulur, bir `ICatalogService` yazın, hangi Autofac çözümler, bir örneğini döndüren `CatalogService` sınıfı. Bağımlılık ekleme hakkında daha fazla bilgi için bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Şekil 10-1 tarafından görüntüleme için katalog mikro hizmet Kataloğu veri okuma sınıfları etkileşimini gösterir `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Veri Kataloğu mikro hizmet alma")](accessing-remote-data-images/catalogdata-large.png "katalog mikro veri alma")

**Şekil 10-1**: Katalog mikro veri alma

Zaman `CatalogView` için gittiğinizde `OnInitialize` yönteminde `CatalogViewModel` sınıfı çağrılır. Bu yöntem Kataloğu veri Kataloğu mikro aşağıdaki kod örneğinde gösterildiği gibi alır:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Bu yöntemi çağırır `GetCatalogAsync` yöntemi `CatalogService` içine eklenen örneği `CatalogViewModel` Autofac tarafından. Aşağıdaki örnekte gösterildiği kod `GetCatalogAsync` yöntemi:

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

Bu yöntem, istek gönderilecek kaynağı tanımlayan URI oluşturur ve kullanır `RequestProvider` sonuçları döndürmeden önce kaynakta GET HTTP yöntemini çağırmak için sınıf `CatalogViewModel`. `RequestProvider` Sınıfı, bir kaynak, kaynak üzerinde gerçekleştirilecek işlemi gösteren bir HTTP yöntemini tanımlayan bir URI biçiminde bir istek gönderdiğini işlevselliği içerir ve herhangi bir veri içeren bir gövde işlemi gerçekleştirmek için gerekli. Ne hakkında bilgi için `RequestProvider` içine sınıfı eklenen `CatalogService class`, bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

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

Bu yöntemi çağırır `CreateHttpClient` örneğini döndürür yöntemi `HttpClient` uygun üstbilgileri kümesiyle sınıfı. Ardından depolanıyor yanıt URI'si tarafından tanımlanan kaynağa zaman uyumsuz bir GET isteği gönderir `HttpResponseMessage` örneği. `HandleResponse` Yöntemi sonra çağrılır, yanıt başarılı bir HTTP durum kodu içermiyorsa, bir özel durum oluşturur. Yanıt için JSON öğesinden dönüştürülmüş bir dize olarak okuma sonra bir `CatalogRoot` nesne ve döndürülen `CatalogService`.

`CreateHttpClient` Yöntemi, aşağıdaki kod örneğinde gösterilir:

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

Bu yöntem yeni bir örneğini oluşturur `HttpClient` sınıfı ve kümelerini `Accept` tarafından yapılan tüm istekleri üstbilgisinin `HttpClient` için örnek `application/json`, JSON kullanılarak biçimlendirilmesi için herhangi bir yanıt içeriğini beklediğini belirtir. Ardından, bir erişim belirteci bağımsız değişken olarak geçirilen varsa `CreateHttpClient` yöntemi eklenir `Authorization` tarafından yapılan tüm istekleri üstbilgisinin `HttpClient` dizesiyle önekli örneği `Bearer`. Yetkilendirme hakkında daha fazla bilgi için bkz: [yetkilendirme](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Zaman `GetAsync` yönteminde `RequestProvider` sınıfı çağrıları `HttpClient.GetAsync`, `Items` yönteminde `CatalogController` Catalog.API projesinde sınıfı çağrıldığında, aşağıdaki kod örneğinde görüntülendiği:

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

Bu yöntem entityframework öğesini kullanarak SQL veritabanından katalog verilerini alır ve başarı HTTP durum kodu içeren bir yanıt iletisi döndürür ve JSON koleksiyonu biçimlendirilmiş `CatalogItem` örnekleri.

#### <a name="making-a-post-request"></a>Bir POST isteği yapma

`BasketService` Sınıfı, veri alma yönetmek ve işlem Sepeti mikro hizmet ile güncelleştirmek için kullanılır. İçinde `RegisterDependencies` yönteminde `ViewModelLocator` sınıfı, `BasketService` sınıfı karşı türü eşlemesi olarak kayıtlı `IBasketService` Autofac bağımlılık ekleme kapsayıcısını türüyle. Ardından, bir örneği olduğunda `BasketViewModel` kurucusu kabul sınıfı oluşturulur, bir `IBasketService` yazın, hangi Autofac çözümler, bir örneğini döndüren `BasketService `sınıfı. Bağımlılık ekleme hakkında daha fazla bilgi için bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Şekil 10-2 gösterir tarafından görüntülenen Sepeti veri gönderme sınıfları etkileşim `BasketView`, sepet mikro hizmet için.

[![](accessing-remote-data-images/basketdata.png "Sepeti mikro verileri gönderme")](accessing-remote-data-images/basketdata-large.png "Sepeti mikro verileri gönderme")

**Şekil 10-2**: Sepeti mikro verileri gönderme

Alışveriş sepetine bir öğe eklendiğinde `ReCalculateTotalAsync` yönteminde `BasketViewModel` sınıfı çağrılır. Bu yöntem sepetteki maddeler toplam değerini güncelleştirir ve aşağıdaki kod örneğinde gösterildiği gibi Sepeti mikro hizmet Sepeti veri gönderir:

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

Bu yöntemi çağırır `UpdateBasketAsync` yöntemi `BasketService` içine eklenen örneği `BasketViewModel` Autofac tarafından. Aşağıdaki yöntem gösterildiği `UpdateBasketAsync` yöntemi:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Bu yöntem, istek gönderilecek kaynağı tanımlayan URI oluşturur ve kullanır `RequestProvider` sonuçları döndürmeden önce kaynak üzerinde POST HTTP yöntemini çağırmak için sınıf `BasketViewModel`. Bir erişim belirteci elde IdentityServer kimlik doğrulama işlemi sırasında Not Sepeti mikro hizmet isteklerine yetkilendirmek için gereklidir. Yetkilendirme hakkında daha fazla bilgi için bkz: [yetkilendirme](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Aşağıdaki kod örneğinde birini gösterir `PostAsync` yöntemleri `RequestProvider` sınıfı:

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

Bu yöntemi çağırır `CreateHttpClient` örneğini döndürür yöntemi `HttpClient` uygun üstbilgileri kümesiyle sınıfı. Ardından JSON biçimini ve depolanıyor yanıt gönderilen serileştirilmiş Sepeti verilerle URI tarafından tanımlanan kaynağa zaman uyumsuz bir POST isteği gönderir `HttpResponseMessage` örneği. `HandleResponse` Yöntemi sonra çağrılır, yanıt başarılı bir HTTP durum kodu içermiyorsa, bir özel durum oluşturur. Ardından, yanıt için JSON öğesinden dönüştürülmüş bir dize olarak okuma bir `CustomerBasket` nesne ve döndürülen `BasketService`. Hakkında daha fazla bilgi için `CreateHttpClient` yöntemi, bkz: [bir GET isteği yapmadan](#making_a_get_request).

Zaman `PostAsync` yönteminde `RequestProvider` sınıfı çağrıları `HttpClient.PostAsync`, `Post` yönteminde `BasketController` Basket.API projesinde sınıfı çağrıldığında, aşağıdaki kod örneğinde görüntülendiği:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Bu yöntem bir örneğini kullanır `RedisBasketRepository` Redis önbelleği Sepeti verilere kalıcı hale getirmek için sınıf ve başarı HTTP durum kodunu ve JSON içeren bir yanıt iletisi biçimli olarak döndüren `CustomerBasket` örneği.

#### <a name="making-a-delete-request"></a>Bir silme isteği

Şekil 10-3 için Sepeti mikro Sepeti verileri silmek sınıfları etkileşimler gösterilmektedir `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Sepeti mikro hizmet silinirken verileri")

**Şekil 10-3**: Sepet mikro verileri silme

Kullanıma alma işlemi çağrıldığında, `CheckoutAsync` yönteminde `CheckoutViewModel` sınıfı çağrılır. Bu yöntem, aşağıdaki kod örneğinde gösterildiği şekilde alışveriş sepeti temizlemeden önce yeni bir sıra oluşturur:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Bu yöntemi çağırır `ClearBasketAsync` yöntemi `BasketService` içine eklenen örneği `CheckoutViewModel` Autofac tarafından. Aşağıdaki yöntem gösterildiği `ClearBasketAsync` yöntemi:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Bu yöntem, istek gönderilir kaynağı tanımlayan URI oluşturur ve kullanır `RequestProvider` kaynakta DELETE HTTP yöntemini çağırmak için sınıf. Bir erişim belirteci elde IdentityServer kimlik doğrulama işlemi sırasında Not Sepeti mikro hizmet isteklerine yetkilendirmek için gereklidir. Yetkilendirme hakkında daha fazla bilgi için bkz: [yetkilendirme](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Aşağıdaki örnekte gösterildiği kod `DeleteAsync` yönteminde `RequestProvider` sınıfı:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Bu yöntemi çağırır `CreateHttpClient` örneğini döndürür yöntemi `HttpClient` uygun üstbilgileri kümesiyle sınıfı. Ardından, URI tarafından tanımlanan kaynağa zaman uyumsuz bir silme isteği gönderir. Hakkında daha fazla bilgi için `CreateHttpClient` yöntemi, bkz: [bir GET isteği yapmadan](#making_a_get_request).

Zaman `DeleteAsync` yönteminde `RequestProvider` sınıfı çağrıları `HttpClient.DeleteAsync`, `Delete` yönteminde `BasketController` Basket.API projesinde sınıfı çağrıldığında, aşağıdaki kod örneğinde görüntülendiği:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Bu yöntem bir örneğini kullanan `RedisBasketRepository` sınıfı Sepeti veri Redis Önbelleği'nden silin.

## <a name="caching-data"></a>Verileri Önbelleğe Alma

Bir uygulama performansını için Kapat bulunan Hızlı depolama sık erişilen verileri önbelleğe alarak geliştirilebilir uygulama. Hızlı depolama yakın özgün kaynak daha App bulunduğu sonra önbelleğe alma yanıt önemli ölçüde artırabilir veri alınırken zaman.

Önbelleğe alma en yaygın read-through önbelleği, önbellek başvurarak uygulama verileri nerede alır biçimidir. Veri önbellekte değilse, veri deposundan alınan ve önbelleğe eklenen. Uygulamaları ile edilgen önbellek düzeni read-through önbelleğe alma uygulayabilirsiniz. Bu desen öğe şu anda önbellekte olup olmadığını belirler. Öğe önbellekte değilse, veri deposundan okuma ve önbelleğe eklenen. Daha fazla bilgi için bkz: [edilgen önbellek](/azure/architecture/patterns/cache-aside/) düzeni.

> [!TIP]
> Sık sık okunan ve seyrek değişen verileri önbelleğe alır. Bu veriler, bir uygulama tarafından alınan talep ilk kez önbelleğine eklenebilir. Bu uygulamanın veri deposundan veri yalnızca bir kez getirmek gerekir ve bu sonraki erişim önbelleği kullanarak karşılanabilir anlamına gelir.

Uygulama, eShopOnContainers başvuru gibi dağıtılmış uygulamalar, ya da aşağıdaki önbellekleri sağlamanız gerekir:

-   Birden çok makineler veya işlemler tarafından erişilebilen bir paylaşılan önbelleği.
-   Veri uygulamayı çalıştıran cihaz üzerinde yerel olarak burada tutulan bir özel önbelleği.

EShopOnContainers mobil uygulama verileri uygulama örneğini çalıştıran bir cihazda yerel olarak burada tutulan özel bir önbellek kullanır. EShopOnContainers başvuru uygulama tarafından kullanılan önbelleği hakkında daha fazla bilgi için bkz: [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook).

> [!TIP]
> Herhangi bir zamanda kaybolabilir geçici veri deposu olarak önbelleğini düşünün. Veri önbelleği yanı sıra özgün veri deposu korunduğundan emin olmak. Önbellek kullanılamaz hale gelirse veri kaybı olasılığını sonra en aza indirilir.

### <a name="managing-data-expiration"></a>Veri sona erme yönetme

Önbelleğe alınan veriler her zaman özgün verilerle tutarsız olması beklenmektedir zordur. Önbelleğe alınmış verileri eski olur neden önbelleğe sonra özgün veri deposundaki verileri değiştirebilir. Bu nedenle, uygulamaları önbellekteki verileri olabildiğince güncel olduğundan emin olmak için yardımcı olan bir strateji uygulamak ancak aynı zamanda algılayabilir ve önbellekteki verileri eski olduğunda ortaya çıkan durumları işlemek. En önbelleğe alma mekanizmaları verileri süresi dolacak şekilde yapılandırılması önbelleğini etkinleştirmek ve bu nedenle veri güncel olabilir dönemi azaltabilirsiniz.

> [!TIP]
> Varsayılan sona erme tarihini ayarlayabilir bir önbellek yapılandırırken zaman. Birçok önbellekleri verileri geçersiz kılar ve belirli bir süre boyunca erişilemiyorsa önbellekten kaldıran sona erme uygulayın. Ancak, sona erme süresi seçerken dikkatli olunmalıdır. Çok kısa yapılırsa, verileri çok hızlı bir şekilde sona erecek ve önbelleğe alma avantajlarını azaltılır. Bu çok uzun, eski haline gelen veri riskleri yapılırsa. Bu nedenle, sona erme zamanı verileri kullanan uygulamalar için erişim desenini eşleşmelidir.

Önbelleğe alınan veriler süresi dolar, önbellekten kaldırılmalı ve uygulama almanız gerekir özgün veriler verileri depolamak ve tekrar önbelleğe yerleştirin.

Veriler çok uzun bir süre için kalmasına izin verilip verilmediğini bir önbellek dolabilir, mümkündür. Bu nedenle, önbelleğe yeni öğeler eklemek için istekleri olarak bilinen bir işlemle bazı öğeleri kaldırmak için gerekli olabilecek *çıkarma*. Önbelleğe alma hizmetleri genellikle verileri en kısa süre önce kullanılan olarak çıkarın. Ancak, en son kullanılan ve ilk olarak ilk çıkar dahil olmak üzere diğer çıkarma ilkeleri vardır. Daha fazla bilgi için bkz: [önbelleğe alma Kılavuzu](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Görüntüleri önbelleğe alma

EShopOnContainers mobil uygulama önbelleğe alınması fayda uzak ürün görüntüleri tüketir. Bu görüntüleri tarafından görüntülenen [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) denetimi ve `CachedImage` tarafından sağlanan denetim [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) kitaplığı.

Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) denetimi, indirilen görüntülerini önbelleğe alma destekler. Önbelleğe alma, varsayılan olarak etkindir ve görüntünün 24 saat için yerel olarak depolar. Ayrıca, sona erme zamanı ile yapılandırılabilir [ `CacheValidity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) özelliği. Daha fazla bilgi için bkz: [indirilen görüntü önbelleğe alma](~/xamarin-forms/user-interface/images.md#Image_Caching).

FFImageLoading'ın `CachedImage` denetimidir Xamarin.Forms yerine [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) tamamlayıcı işlevselliğini etkinleştirmek ek özellikler sağlayan denetim. Bu işlev arasında hata destekleme ve resim yer tutucularını yükleme sırasında yapılandırılabilir önbelleğe denetimi sağlar. Aşağıdaki kod örneğinde eShopOnContainers mobil uygulamayı nasıl kullandığını gösterir `CachedImage` denetim `ProductTemplate`, hangi tarafından kullanılan veri şablonu [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim `CatalogView`:

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP, WinRT, WinPhone" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP, WinRT, WinPhone" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

`CachedImage` Denetleyen ayarlar `LoadingPlaceholder` ve `ErrorPlaceholder` platforma özgü görüntülerine özellikleri. `LoadingPlaceholder` Özelliği tarafından belirtilen görüntü sırasında görüntülenecek resmi belirtir `Source` özelliği alınır ve `ErrorPlaceholder` özelliği, görüntü almaya çalışırken bir hata oluşursa görüntülenecek resim belirtir tarafından belirtilen `Source` özelliği.

Adından da anlaşılacağı gibi `CachedImage` denetim değeriyle belirtilen süre için cihaz üzerindeki uzak görüntülerini önbelleklerini `CacheDuration` özelliği. Bu özellik değeri açıkça ayarlanmamışsa, varsayılan değer 30 gün olarak uygulanır.

## <a name="increasing-resilience"></a>Artan esneklik

Uzak hizmet ve kaynakları ile iletişim kuran tüm uygulamalar için geçici hataları duyarlı olması gerekir. Geçici hataları anda yaşanan Hizmetleri için ağ bağlantı kaybı, bir hizmet veya Hizmet meşgul olduğunda ortaya çıkan zaman aşımları geçici kullanılamama içerir. Bu hatalar genellikle kendi kendine düzeltme ve uygun bir gecikmeden sonra eylemi yineleniyorsa başarılı olabilir.

Tüm öngörülebilir koşullarda kapsamlı olarak sınanmıştır olsa bile geçici hataları algılanan kalitesini bir uygulama üzerinde büyük bir etkisi olabilir. Uzak Hizmetleri ile iletişim kuran bir uygulamayı güvenilir bir şekilde çalıştığından emin olmak için aşağıdakilerin tümü yapabileceklerinizi olmalıdır:

-   Hataları oluşur ve büyük olasılıkla geçici hataları olup olmadığını belirlemek algılayabilir.
-   Hataya geçici ve işlem yeniden sayısı izlemek büyük olasılıkla olduğunu belirlerse, işlemi yeniden deneyin.
-   Yeniden denemeler, her girişimini ve eylemleri başarısız girişimden sonra gerçekleştirilecek arasındaki gecikme sayısını belirten bir uygun yeniden deneme stratejiyi kullanın.

Bu geçici hata işleme yeniden deneme deseni uygulayan kod uzak bir hizmete erişmek için tüm girişimleri kaydırma tarafından gerçekleştirilebilir.

### <a name="retry-pattern"></a>Desen yeniden deneyin

Uzak hizmet için bir istek göndermeye çalıştığında bir uygulama bir hata algılarsa, bu hata aşağıdaki yollardan biriyle işleyebilir:

-   İşlemi yeniden deneniyor. Uygulama başarısız olan istek hemen yeniden dene.
-   Bir gecikmeden sonra işlemi yeniden deneniyor. Uygulama isteği yeniden denemeden önce uygun bir süre için beklemeniz gerekir.
-   İşlemi iptal ediliyor. Uygulama işlemi iptal edin ve bir özel durum raporu gerekir.

Yeniden deneme stratejisini uygulamanın iş gereksinimlerini karşılamak üzere ayarlanması. Örneğin, yeniden deneme sayısı iyileştirmek ve yeniden deneme aralığı yapılmaya çalışılan işleme önemlidir. İşlemi bir kullanıcı etkileşimi parçası ise, yeniden deneme aralığını kısa ve kullanıcılar için bir yanıt beklemesi yapmaktan kaçınmak için çalıştı yalnızca birkaç yeniden deneme olması gerekir. İşlemi iptal etme veya iş akışını yeniden pahalı ya da zaman alıcı olduğu uzun süre çalışan bir iş akışının parçası ise, girişimleri arasında daha uzun süre bekleyin ve daha fazla kez yeniden denemek için uygundur.

> [!NOTE]
> Agresif yeniden deneme stratejisini girişimleri ve çok sayıda yeniden denemeleri arasındaki en düşük gecikme ile için Kapat veya kapasitesini çalıştıran uzak bir hizmet düşebilir. Ayrıca, sürekli olarak başarısız olan işlemi gerçekleştirmeye çalışıyor, bu tür bir yeniden deneme stratejisini Ayrıca uygulama yanıt hızını etkileyebilir.

Bir isteği yeniden deneme sayısı sonra yine başarısız olursa, uygulama için aynı kaynağına giden diğer istekler önlemek ve bir hata raporu için daha iyi olur. Daha sonra ayarlanmış bir süre sonra uygulama bir veya daha fazla isteği başarılı olursa görmek için kaynak yapabilir. Daha fazla bilgi için bkz: [devre kesici düzeni](#circuit_breaker_pattern).

> [!TIP]
> Hiçbir zaman bir sonsuz yeniden deneme mekanizması uygulayın. Yeniden deneme sınırlı sayıda kullanın veya uygulayan [devre kesici](/azure/architecture/patterns/circuit-breaker/) kurtarmak bir hizmet olanak deseni.

EShopOnContainers mobil uygulama şu anda yeniden deneme düzeni RESTful web isteklerini yaparken uygulamıyor. Ancak, `CachedImage` tarafından sağlanan denetim [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) kitaplığı görüntü yükleme denenerek geçici hata işleme destekler. Görüntü yükleme başarısız olursa, daha fazla deneme yapılmayacak. Deneme sayısı tarafından belirtilen `RetryCount` özelliği ve yeniden deneme tarafından belirtilen bir gecikmeden sonra gerçekleşeceği `RetryDelay` özelliği. Bu özellik değerlerini olmayan açıkça ayarlarsanız, kendi varsayılan değerleri uygulanır – 3 için `RetryCount` özelliği ve 250ms için `RetryDelay` özelliği. Hakkında daha fazla bilgi için `CachedImage` denetlemek için bkz: [önbelleğe alma görüntüleri](#caching_images).

EShopOnContainers başvuru uygulaması yeniden deneme deseni uygular. Daha fazla bilgi için yeniden deneme desenle birleştirme tartışması dahil olmak üzere `HttpClient` sınıfı için bkz: [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook).

Yeniden deneme desen hakkında daha fazla bilgi için bkz: [yeniden deneme](/azure/architecture/patterns/retry/) düzeni.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Devre kesici düzeni

Bazı durumlarda, hataları düzeltmek için daha uzun sürer beklenen olaylar nedeniyle oluşabilir. Bu hataları kısmi bir bağlantı kaybı bir hizmet tam başarısız olmasına aralığında değişebilir. Bu durumlarda, başarılı ve bunun yerine işlemin başarısız olduğunu kabul etmesi ve bu hatanın uygun şekilde işlemek olasılığı bulunmayan bir işlemi yeniden denemek bir uygulama için bir durum sahiptir.

Devre kesici desen bir uygulama başarısız, ayrıca hataya giderilmiş olup olmadığını algılamak uygulamayı etkinleştirirken büyük olasılıkla bir işlemi yürütmek sürekli çalışmasını engelleyebilir.

> [!NOTE]
> Devre kesici düzeni amacını yeniden deneme düzenin farklıdır. Yeniden deneme desen bir işlem başarılı Beklenti, yeniden denemek bir uygulama sağlar. Devre kesici desen bir uygulama, büyük olasılıkla başarısız olacak olan bir işlem gerçekleştirmesini engeller.

Devre kesici başarısız olabilir işlemleri için bir proxy olarak görev yapar. Proxy oluşmuş son hatalarının sayısı izlemek ve devam etmek için veya bir özel durum hemen döndürmek için işleme izin karar vermek için bu bilgileri kullanmanız gerekir.

EShopOnContainers mobil uygulama şu anda devre kesici düzeni uygulamıyor. Ancak, eShopOnContainers yapar. Daha fazla bilgi için bkz: [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook).

> [!TIP]
> Yeniden deneme ve devre kesici desenleri birleştirin. Bir uygulamayı yeniden deneme ve devre kesici desenleri devre kesici aracılığıyla bir işlem çağırmak için Yeniden Dene'yi desenini kullanarak birleştirebilirsiniz. Ancak, yeniden deneme mantığı devre kesici tarafından döndürülen tüm özel durumlar için hassas ve bu bir arıza geçici değil devre kesici olduğunu gösteriyorsa, yeniden deneme girişimleri abandon gerekir.

Devre kesici desen hakkında daha fazla bilgi için bkz: [devre kesici](/azure/architecture/patterns/circuit-breaker/) düzeni.

## <a name="summary"></a>Özet

Birçok modern web tabanlı çözüm olun işlevselliği için Uzak istemci uygulamaları sağlamak için web sunucuları tarafından barındırılan bir web Hizmetleri kullanın. Bir web hizmeti sunan operations web API'si oluşturur ve istemci uygulamaları verileri veya API sunan işlemlerini nasıl uygulandığını bilmeden web API'si verecek olması gerekir.

Bir uygulama performansını için Kapat bulunan Hızlı depolama sık erişilen verileri önbelleğe alarak geliştirilebilir uygulama. Uygulamaları ile edilgen önbellek düzeni read-through önbelleğe alma uygulayabilirsiniz. Bu desen öğe şu anda önbellekte olup olmadığını belirler. Öğe önbellekte değilse, veri deposundan okuma ve önbelleğe eklenen.

Web API'leri ile iletişim kurarken uygulamalar için geçici hataları hassas olması gerekir. Geçici hataları anda yaşanan Hizmetleri için ağ bağlantı kaybı, bir hizmet veya Hizmet meşgul olduğunda ortaya çıkan zaman aşımları geçici kullanılamama içerir. Bu hatalar genellikle kendi kendine düzeltme ve eylem uygun bir gecikmeden sonra yinelenir, sonra başarılı olması olasıdır. Bu nedenle, uygulamaları tüm işleme mekanizması geçici bir hata uygulayan kod web API'si erişim girişimleri kaydırılacağını.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
