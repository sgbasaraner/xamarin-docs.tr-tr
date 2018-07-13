---
title: Kimlik doğrulama ve yetkilendirme
description: Bu bölümde, hizmetine mobil uygulama kimlik doğrulaması ve yetkilendirme kapsayıcılı mikro hizmetler karşı nasıl gerçekleştireceğini açıklar.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: beb9e8f351a1cecc6017a08345f7cfc5e207ba35
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996223"
---
# <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

Kimlik doğrulaması, bir kullanıcıdan kimlik adı ve parola gibi ve bu kimlik bilgilerini bir yetkilisi karşı doğrulama işlemini şeklindedir. Kimlik bilgilerinin geçerli olduğundan, kimlik bilgilerini gönderilen varlık kimliği doğrulanmış kimlik olarak kabul edilir. Kimlik doğrulandıktan sonra bir yetkilendirme işlemine kimliğe belirli bir kaynağa erişim izni olup olmadığını belirler.

Kimlik doğrulama ve yetkilendirme, ASP.NET Core kimliği, Microsoft, Google gibi dış kimlik doğrulama sağlayıcıları kullanma dahil olmak üzere bir ASP.NET MVC web uygulaması ile iletişim kuran bir Xamarin.Forms uygulamasına tümleştirmek için birçok yaklaşım vardır, Facebook veya Twitter ve kimlik doğrulaması ara yazılımı. Hizmetine mobil uygulama kimlik doğrulaması ve yetkilendirme IdentityServer 4 kullanan bir kimlik kapsayıcılı mikro hizmet ile gerçekleştirir. Mobil uygulama veya bir kullanıcı kimlik doğrulaması için bir kaynağa erişim sağlamak için güvenlik belirteçleri IdentityServer ister. Bir kullanıcı adına sorun belirteçleri IdentityServer için kullanıcı için IdentityServer oturum gerekir. Ancak, IdentityServer kullanıcı arabirimi veya veritabanı için kimlik doğrulaması sağlamaz. Bu nedenle, hizmetine başvuru uygulamada ASP.NET çekirdek kimliği bu amaç için kullanılır.

## <a name="authentication"></a>Kimlik doğrulaması

Uygulamanın geçerli kullanıcının kimliğini bilmesi gereken kimlik doğrulaması gereklidir. Kullanıcıları tanımlamak için ASP.NET Core'nın birincil geliştirici tarafından yapılandırılmış bir veri deposunda kullanıcı bilgilerini depolayan ASP.NET Core kimliği üyelik sistemi mekanizmadır. Genellikle, Azure depolama, Azure Cosmos DB veya diğer konumlardan kimlik bilgilerini depolamak için özel depoları veya üçüncü taraf paketlerini kullanılabilmesine rağmen bu veri depolama alanını bir EntityFramework deposu olacaktır.

ASP.NET Core kimliği olun kimlik doğrulama senaryoları bir yerel kullanıcı veri deposunu kullanın ve isteği (tipik ASP.NET MVC web uygulamalarında olduğu gibi) tanımlama bilgileri aracılığıyla arasında kimlik bilgilerini kalıcı hale getirmek için uygun bir çözümdür. Ancak, tanımlama bilgileri her zaman kalıcı hale getirmeniz ve veri aktarımı doğal bir yol değildir. Örneğin, bir mobil uygulamasından erişilen RESTful uç noktayı kullanıma sokan bir ASP.NET Core web uygulaması genellikle tanımlama bilgileri, bu senaryoda kullanılamaz olduğundan taşıyıcı belirteci kimlik doğrulaması kullanmanız gerekir. Ancak, taşıyıcı belirteçleri kolayca alınabilir ve mobil uygulamadan web isteklerini yetkilendirme üst bilgisine dahil.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Sertifika veren taşıyıcı belirteçlerini kullanarak IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) yerel ASP.NET Core kimliği kullanıcılar için güvenlik belirteçleri verme dahil olmak üzere birçok kimlik doğrulama ve yetkilendirme senaryoları için kullanılan ASP.NET Core açık kaynak Openıd Connect ve OAuth 2.0 framework içindir.

> [!NOTE]
> Openıd Connect ve OAuth 2.0 farklı sorumluluklara yaparken çok benzer.

Openıd Connect, OAuth 2.0 protokolünü üzerine bir kimlik doğrulama katmanı olan. OAuth 2 bir güvenlik belirteci Hizmeti'nden erişim belirteçlerini istemek ve bunları iletişim kurmak için API'leri ile uygulamalar sağlayan bir protokoldür. Kimlik doğrulama ve yetkilendirme merkezi olduğundan bu temsilci hem istemci uygulamaları ve API'leri karmaşıklığını azaltır.

Openıd Connect ve OAuth 2.0 kimlik doğrulaması ve API erişimi iki temel güvenlik kaygıları birleştirebilir ve IdentityServer 4 bu protokolleri uygulamasıdır.

Hizmetine başvuru uygulaması gibi doğrudan istemci-mikro hizmet iletişimi kullanan uygulamaları bir güvenlik belirteci hizmeti (STS) olarak davranan bir özel kimlik doğrulama mikro hizmet, kullanıcıların kimliklerini doğrulamak için gösterildiği gibi kullanılabilir 9-1. Doğrudan istemci-mikro hizmet iletişimi hakkında daha fazla bilgi için bkz. [arasındaki iletişimi istemci ve mikro Hizmetler](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Adanmış kimlik doğrulaması mikro hizmet olarak kimlik doğrulaması")

**Şekil 9-1:** adanmış kimlik doğrulaması mikro hizmet olarak kimlik doğrulaması

Hizmetine mobil uygulama kimlik doğrulaması gerçekleştirmek ve API'ler için erişim denetimini IdentityServer 4 kullanan kimlik mikro hizmet ile iletişim kurar. Bu nedenle, mobil uygulama belirteçleri IdentityServer bir kullanıcı kimlik doğrulaması için veya bir kaynağa erişim sağlamak için istek:

-   IdentityServer ile kullanıcıların kimliğini doğrulama gerçekleştirilir mobil uygulama isteyerek bir *kimlik* belirteci bir kimlik işleminin sonucunu temsil eder. Çıplak en azından, kullanıcı ve nasıl ve ne zaman kullanıcı kimlik doğrulaması hakkında bilgi için bir tanımlayıcı içerir. Ayrıca, ek kimlik verilerini de içerebilir.
-   IdentityServer kaynakla erişim elde edilir mobil uygulama isteyerek bir *erişim* bir API kaynağına erişimi sağlayan bir belirteç. İstemciler, erişim belirteçlerini istemek ve bunları API'sine iletebilir. Erişim belirteçleri, (varsa) istemci ve kullanıcı hakkındaki bilgileri içerir. API'leri, ardından verilerine erişim yetkisi vermek için bu bilgileri kullanın.

> [!NOTE]
> Belirteçleri isteyebilmesi istemci IdentityServer ile kayıtlı olması gerekir.

### <a name="adding-identityserver-to-a-web-application"></a>Bir Web uygulamasına IdentityServer ekleme

Sırayla IdentityServer 4 kullanmak bir ASP.NET Core web uygulaması için web uygulamasının Visual Studio çözümüne eklenmiş olması gerekir. Daha fazla bilgi için [Kurulum ve genel bakış](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) IdentityServer belgelerinde.

Web uygulamasının Visual Studio çözümünde IdentityServer dahildir sonra Openıd Connect ve OAuth 2.0 uç isteklerine hizmet verebilir böylece için web uygulamasının HTTP istek işlem hattı, işleme eklenmelidir. Bunu elde edilir `Configure` web uygulamasının yönteminde `Startup` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Web uygulamasının HTTP isteği ardışık düzen işleme sırası önemlidir. Bu nedenle, IdentityServer oturum açma ekranından uygulayan UI çerçevesi önce ardışık düzene eklenmelidir.

### <a name="configuring-identityserver"></a>IdentityServer yapılandırma

IdentityServer yapılandırılmalıdır `ConfigureServices` web uygulamasının yönteminde `Startup` çağırarak sınıfı `services.AddIdentityServer` hizmetine başvuru uygulamadan alınan aşağıdaki kod örneğinde gösterildiği gibi yöntemi:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

Arama sonra `services.AddIdentityServer` yöntemi, ek fluent API'ler çağrılır seçerek aşağıdakileri yapılandırın:

-   İmzalama için kullanılan kimlik bilgileri.
-   Kullanıcılar isteyebilir API ve kimlik kaynaklara erişimi.
-   İstek belirteçleri bağlanan istemciler.
-   ASP.NET Core kimliği.

>💡 **İpucu**: dinamik olarak IdentityServer 4 yapılandırma yüklenemiyor. IdentityServer 4'ün API'leri bir bellek içi yapılandırma nesnelerini listesinden IdentityServer yapılandırılmasını destekler. Hizmetine başvuru uygulamada, bu uygulamaya sabit kodlanmış bellek içi koleksiyonlarıdır. Ancak, üretim senaryolarında bunlar dinamik olarak yapılandırma dosyasından ya da bir veritabanından yüklenebilir.

ASP.NET Core kimliği kullanılacak IdentityServer yapılandırma hakkında daha fazla bilgi için bkz: [kullanarak ASP.NET Core kimliği](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) IdentityServer belgelerinde.

#### <a name="configuring-api-resources"></a>API kaynaklarını yapılandırma

API kaynakları yapılandırırken `AddInMemoryApiResources` yöntemi bekliyor bir `IEnumerable<ApiResource>` koleksiyonu. Aşağıdaki örnekte gösterildiği kod `GetApis` bu koleksiyon hizmetine sağlayan yöntemi başvuru uygulaması:

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Bu yöntem, sepet API'leri ve siparişler IdentityServer korumalısınız belirtir. Bu nedenle, IdentityServer yönetilen erişim bu API'lere çağrı yaparken belirteçleri gerekli olacaktır. Hakkında daha fazla bilgi için `ApiResource` yazın, bkz: [API kaynak](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) IdentityServer 4 belgelerinde.

#### <a name="configuring-identity-resources"></a>Kimlik kaynaklarını yapılandırma

Kimlik kaynakları yapılandırırken `AddInMemoryIdentityResources` yöntemi bekliyor bir `IEnumerable<IdentityResource>` koleksiyonu. Kimlik, kullanıcı kimliği, adı veya e-posta adresi gibi verileri kaynaklardır. Her bir kimlik kaynağın benzersiz bir ada sahip ve rasgele talep türleri, ardından kullanıcı için kimlik belirtecinde yer alacak atanabilir. Aşağıdaki örnekte gösterildiği kod `GetResources` bu koleksiyon hizmetine sağlayan yöntemi başvuru uygulaması:

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Openıd Connect belirtimi bazı belirtir [standart kimlik kaynakları](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Kullanıcılar için benzersiz bir kimliği yayma için destek sağlanır en düşük gereksinimdir. Bu göstererek sağlanır `IdentityResources.OpenId` kimlik kaynak.

> [!NOTE]
> `IdentityResources` Sınıfı, tüm Openıd Connect belirtiminde (openıd, e-posta, profili, telefon ve adres) tanımladığınız kapsamların destekler.

Özel kimlik kaynakları tanımlama IdentityServer da destekler. Daha fazla bilgi için [özel kimlik kaynakları tanımlama](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) IdentityServer belgelerinde. Hakkında daha fazla bilgi için `IdentityResource` yazın, bkz: [kimlik kaynak](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) IdentityServer 4 belgelerinde.

#### <a name="configuring-clients"></a>İstemcilerini yapılandırma

İstemciler IdentityServer belirteçleri isteyebileceği uygulamalardır. Genellikle, aşağıdaki ayarları her istemci için en az tanımlanması gerekir:

-   Benzersiz istemci kimliği.
-   Belirteci hizmeti (izin verme türü da bilinir) izin verilen etkileşim.
-   Burada kimlik ve erişim belirteçleri gönderilen konumu (yeniden yönlendirme URI'si olarak bilinir).
-   İstemci erişimine izin verilmesini kaynakların bir listesini (kapsamı olarak bilinir).

İstemciler, yapılandırırken `AddInMemoryClients` yöntemi bekliyor bir `IEnumerable<Client>` koleksiyonu. Aşağıdaki kod örneği hizmetine mobil uygulamada yapılandırmasını gösterir `GetClients` bu koleksiyon hizmetine sağlayan yöntemi başvuru uygulaması:

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Bu yapılandırma verilerini aşağıdaki özellikleri belirtir:

-   `ClientId`: Bir istemci için benzersiz kimliği.
-   `ClientName`: Günlüğe kaydetme ve onay ekranında için kullanılır istemci görünen adı.
-   `AllowedGrantTypes`: Nasıl IdentityServer ile etkileşim kurmak bir istemcinin istediği belirtir. Daha fazla bilgi için [kimlik doğrulaması akışı yapılandırma](#configuring_the_authentication_flow).
-   `ClientSecrets`: Belirteç uç noktasından belirteç istenirken kullanılan istemci gizli kimlik bilgilerini belirtir.
-   `RedirectUris`: İzin verilen bir URI'leri belirteçleri veya yetkilendirme kodları döndürülecek için belirtir.
-   `RequireConsent`: Bir onay ekranında gerekli olup olmadığını belirtir.
-   `RequirePkce`: Bir yetkilendirme kodunu kullanarak istemcileri kanıt anahtarı göndermesi gerekip gerekmediğini belirtir.
-   `PostLogoutRedirectUris`: Oturum kapatma yönlendirmek için izin verilen bir URI'leri belirtir.
-   `AllowedCorsOrigins`: Çıkış noktaları arası kaynak çağrılarından IdentityServer izin verebilir böylece istemci kaynağını belirtir.
-   `AllowedScopes`: İstemci erişimi olan kaynakları belirtir. Varsayılan olarak, bir istemci herhangi bir kaynağa erişimi vardır.
-   `AllowOfflineAccess`: İstemci yenileme belirteçleri isteme olanaklarının olup olmadığını belirtir.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Kimlik doğrulaması akışı yapılandırma

Kimlik doğrulaması akışı bir istemci ve IdentityServer arasında verme türlerini belirterek yapılandırılabilir `Client.AllowedGrantTypes` özelliği. Openıd Connect ve OAuth 2.0 belirtimleri kimlik doğrulaması akışı dahil olmak üzere birkaç tanımlayın:

-   Örtük. Bu akış, tarayıcı tabanlı uygulamalar için en iyi duruma getirilmiş ve yalnızca kimlik doğrulamasını bir kullanıcı veya kimlik doğrulaması ve erişim belirteci isteği için kullanılmalıdır. Tüm belirteçlerin tarayıcı üzerinden aktarılan ve bu nedenle yenileme belirteçleri verilmeyen gibi gelişmiş özellikleri.
-   Yetkilendirme kodu. Bu akış ayrıca istemci kimlik doğrulaması desteklerken tarayıcı ön kanal aksine arka kanal belirteçleri alma olanağı sağlar.
-   Karma. Bu akış örtük bir bileşimidir ve yetkilendirme kodu verme türleri. Kimlik belirteci tarayıcı kanal iletilir ve yetkilendirme kodu gibi diğer yapıları birlikte imzalı Protokolü yanıtı içerir. Yanıtın doğrulama başarılı olduktan sonra arka kanal erişimi almak ve belirteci yenilemek için kullanılmalıdır.

> [!TIP]
> Karma kimlik doğrulaması akışı kullanın. Karma kimlik doğrulaması akışı tarayıcı kanala uygulamak saldırıları sayısını azaltır ve erişim belirteçlerini almak için (ve muhtemelen yenileme belirteçlerini) istediğiniz yerel uygulamalar için önerilen akışıdır.

Kimlik doğrulama akışları hakkında daha fazla bilgi için bkz: [verme türleri](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) IdentityServer 4 belgelerinde.

### <a name="performing-authentication"></a>Kimlik doğrulaması gerçekleştirme

Bir kullanıcı adına sorun belirteçleri IdentityServer için kullanıcı için IdentityServer oturum gerekir. Ancak, IdentityServer kullanıcı arabirimi veya veritabanı için kimlik doğrulaması sağlamaz. Bu nedenle, hizmetine başvuru uygulamada ASP.NET çekirdek kimliği bu amaç için kullanılır.

Şekil 9-2'de gösterilen karma kimlik doğrulama akışı ile IdentityServer ile hizmetine mobil uygulama kimlik doğrulaması yapar.

![](authentication-and-authorization-images/sign-in.png "Oturum açma işlemi üst düzey genel bakış")

**Şekil 9-2:** oturum açma işlemi üst düzey genel bakış

Oturum açma isteği yapılır `<base endpoint>:5105/connect/authorize`. Başarılı bir kimlik doğrulamasının IdentityServer bir yetkilendirme kodunu ve bir kimlik belirteci içeren bir kimlik doğrulaması yanıtını döndürür. Yetkilendirme kodu daha sonra gönderilen `<base endpoint>:5105/connect/token`, erişim, kimlik ve yenileme belirteçleri ile yanıtlar.

Hizmetine mobil uygulama açtığında bir istek göndererek artırımına IdentityServer `<base endpoint>:5105/connect/endsession`, ek parametrelere sahip. Oturum kapatma gerçekleştikten sonra bir post oturumu kapatıp yeniden yönlendirme URI'si mobil uygulamaya geri göndererek IdentityServer yanıt verir. Şekil 9-3, bu işlemi göstermektedir.

![](authentication-and-authorization-images/sign-out.png "Oturum kapatma işlemini üst düzey genel bakış")

**Şekil 9-3:** oturum kapatma işlemini üst düzey genel bakış

Mobil uygulama hizmetine IdentityServer ile iletişimi tarafından gerçekleştirilen `IdentityService` sınıfını `IIdentityService` arabirimi. Bu arabirimi uygulayan sınıfa sağlamalısınız belirtir `CreateAuthorizationRequest`, `CreateLogoutRequest`, ve `GetTokenAsync` yöntemleri.

#### <a name="signing-in"></a>Oturum açma

Kullanıcı ne zaman dokunduğunda **oturum açma** düğmesini `LoginView`, `SignInCommand` içinde `LoginViewModel` sınıfı yürütüldüğünde, sırayla yürütür `SignInAsync` yöntemi. Aşağıdaki kod örneği, bu yöntem gösterir:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Bu metodu çağıran `CreateAuthorizationRequest` yönteminde `IdentityService` aşağıdaki kod örneğinde gösterilen sınıfı:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Bu yöntem IdentityServer için kullanıcının aldığı URI'yi oluşturur [yetkilendirme uç noktası](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), gerekli parametrelere sahip. Yetkilendirme uç noktası altındadır `/connect/authorize` üzerinde 5105 bir kullanıcı ayarı olarak kullanıma sunulan temel uç nokta bağlantı noktası. Kullanıcı ayarları hakkında daha fazla bilgi için bkz. [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Saldırı yüzeyini hizmetine mobil uygulamasının OAuth kod Exchange (PKCE) uzantısı için kavram anahtar uygulayarak azaltılır. PKCE, bunu engelledik, kullanılan yetkilendirme kodunu korur. Bu istemci yetkilendirme isteğinde bir karmasını iletilen bir gizli dizi Doğrulayıcı oluşturma tarafından sağlanır ve hangi sunulan yetkilendirme kodu kullanırken doğrulayamaz. PKCE hakkında daha fazla bilgi için bkz: [OAuth genel istemciler tarafından kod Exchange kavram anahtarı](https://tools.ietf.org/html/rfc7636) Internet Engineering Task Force web sitesinde.

Döndürülen URI depolanan `LoginUrl` özelliği `LoginViewModel` sınıfı. Zaman `IsLogin` özellik olur `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) içinde `LoginView` görünür hale gelir. `WebView` Veri bağlar, [ `Source` ](xref:Xamarin.Forms.WebView.Source) özelliğini `LoginUrl` özelliği `LoginViewModel` sınıfı ve bu nedenle oturum açma için IdentityServer istekte olduğunda `LoginUrl` özelliği IdentityServer'ın yetkilendirme uç noktası. IdentityServer bu isteği alır ve kullanıcı kimliği doğrulanmış değil ve `WebView` Şekil 9-4'teki yapılandırılmış oturum açma sayfasına yönlendirilirsiniz.

![](authentication-and-authorization-images/login.png "WebView tarafından görüntülenen oturum açma sayfası")

**Şekil 9-4:** WebView tarafından görüntülenen oturum açma sayfası

Oturum açma tamamlandıktan sonra [ `WebView` ](xref:Xamarin.Forms.WebView) dönüş URI'yı yönlendirilir. Bu `WebView` Gezinti neden `NavigateAsync` yönteminde `LoginViewModel` aşağıdaki kod örneğinde gösterilen yürütülecek sınıfı:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Bu yöntem dönüş URI'de yer alan kimlik doğrulaması yanıtını ayrıştırır ve mevcut geçerli bir yetkilendirme kodudur koşuluyla IdentityServer için ait bir talep gönderir [belirteç uç noktası](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), yetkilendirme kodu geçirme PKCE gizli Doğrulayıcı ve diğer gerekli parametreler. Belirteç uç noktası altındadır `/connect/token` üzerinde 5105 bir kullanıcı ayarı olarak kullanıma sunulan temel uç nokta bağlantı noktası. Kullanıcı ayarları hakkında daha fazla bilgi için bkz. [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **İpucu**: doğrulama URI döndürür. Hizmetine mobil uygulama dönüş URI doğrulamaz olsa da, dönüş URI açık yeniden yönlendirme saldırılarını önlemek için bir bilinen konuma başvuran doğrulamak için en iyi yöntem olacaktır.

Belirteç uç noktası geçerli bir yetkilendirme kodunu ve PKCE gizli Doğrulayıcı alırsa, bir erişim belirteci, kimlik belirteci ve yenileme belirteci ile yanıt verir. (Bu API kaynaklarına erişime izin verir) erişim belirteci ve kimlik belirteci daha sonra uygulama ayarları saklanır ve sayfa gezintisini gerçekleştirilir. Bu nedenle, genel etki hizmetine mobil uygulamasında budur: kullanıcıların IdentityServer ile kimlik doğrulamasını başarıyla yapabildiğinizi şartıyla, bunlar için gitme `MainView` olan sayfa bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) görüntüleyen `CatalogView` Seçili alt sekmesine olarak.

Sayfa gezintisi hakkında daha fazla bilgi için bkz: [Gezinti](~/xamarin-forms/enterprise-application-patterns/navigation.md). Hakkında bilgi için [ `WebView` ](xref:Xamarin.Forms.WebView) Gezinti neden olan bir görünüm modeli yöntemi yürütülmesi için bkz [davranışları Gezinti çağırma](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Uygulama ayarları hakkında daha fazla bilgi için bkz. [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Uygulama sahte Hizmetleri'nde kullanımı yapılandırıldığında hizmetine de bir sahte oturum açma sağlar `SettingsView`. Bu modda uygulamanın kullanıcı kimlik bilgilerini kullanarak oturum açmak yerine izin IdentityServer ile iletişim değil.

#### <a name="signing-out"></a>İmzalama genişletme

Kullanıcı ne zaman dokunduğunda **LOG OUT** düğmesine `ProfileView`, `LogoutCommand` içinde `ProfileViewModel` sınıfı yürütüldüğünde, sırayla yürütür `LogoutAsync` yöntemi. Bu yöntem için sayfa gezintisi gerçekleştirir `LoginView` geçirme sayfasında, bir `LogoutParameter` kümesi örneği `true` bir parametre olarak. Sayfa gezintisi sırasında parametreleri geçirme hakkında daha fazla bilgi için bkz. [gezinti sırasında parametre geçirme](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Bir görünüm oluşturduğunuzda ve için çıkıldığında `InitializeAsync` görünümün ilişkili görünüm modeli yöntemi yürütüldüğünde, ardından yürütür `Logout` yöntemi `LoginViewModel` aşağıdaki kod örneğinde gösterilen sınıfı:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Bu metodu çağıran `CreateLogoutRequest` yönteminde `IdentityService` kimlik belirtece geçirerek sınıf, bir parametre olarak uygulama ayarlarından alınır. Uygulama ayarları hakkında daha fazla bilgi için bkz: [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Aşağıdaki örnekte gösterildiği kod `CreateLogoutRequest` yöntemi:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Bu yöntem IdentityServer'ın için URI oluşturur [oturumu uç noktası end](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), gerekli parametrelere sahip. En son oturum uç noktadır `/connect/endsession` üzerinde 5105 bir kullanıcı ayarı olarak kullanıma sunulan temel uç nokta bağlantı noktası. Kullanıcı ayarları hakkında daha fazla bilgi için bkz. [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Döndürülen URI depolanan `LoginUrl` özelliği `LoginViewModel` sınıfı. Sırada `IsLogin` özelliği `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) içinde `LoginView` görülebilir. `WebView` Veri bağlar, [ `Source` ](xref:Xamarin.Forms.WebView.Source) özelliğini `LoginUrl` özelliği `LoginViewModel` sınıfı ve bu nedenle oturum kapatma için IdentityServer istekte olduğunda `LoginUrl` özelliği IdentityServer'ın son oturum uç noktası. Kullanıcının oturum açmış olması şartıyla IdentityServer bu isteği aldığında, oturum kapatma gerçekleşir. Kimlik doğrulama tanımlama bilgisi kimlik doğrulaması ara yazılımı gelen ASP.NET Core tarafından yönetilen bir tanımlama bilgisi ile izlenir. Bu nedenle, IdentityServer dışında imzalama kimlik doğrulama tanımlama kaldırır ve bu URI'yi istemciye geri post oturum kapatma yeniden yönlendirme gönderir.

Mobil uygulamadaki [ `WebView` ](xref:Xamarin.Forms.WebView) post oturumu kapatıp yeniden yönlendirme URI'si yönlendirilir. Bu `WebView` Gezinti neden `NavigateAsync` yönteminde `LoginViewModel` aşağıdaki kod örneğinde gösterilen yürütülecek sınıfı:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Bu yöntem, kimlik belirteci hem uygulama ayarları erişim belirtecinden temizler ve ayarlar `IsLogin` özelliğini `false`, hangi neden [ `WebView` ](xref:Xamarin.Forms.WebView) üzerinde `LoginView` görünmez olmasını sayfası . Son olarak, `LoginUrl` URI'ın IdentityServer için 's özelliği ayarlandığında [yetkilendirme uç noktası](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), gerekli parametreleri, sonraki açışınızda hazırlığında kullanıcı bir oturum açma başlatır.

Sayfa gezintisi hakkında daha fazla bilgi için bkz: [Gezinti](~/xamarin-forms/enterprise-application-patterns/navigation.md). Hakkında bilgi için [ `WebView` ](xref:Xamarin.Forms.WebView) Gezinti neden olan bir görünüm modeli yöntemi yürütülmesi için bkz [davranışları Gezinti çağırma](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Uygulama ayarları hakkında daha fazla bilgi için bkz. [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Uygulama içinde SettingsView sahte hizmetlerini kullanacak biçimde yapılandırıldığında hizmetine de bir sahte oturum kapatma sağlar. Bu modda uygulama IdentityServer ile iletişim kurmak değil ve bunun yerine uygulama ayarlarından saklı tarafından istenen belirteçleri temizler.

<a name="authorization" />

## <a name="authorization"></a>Yetkilendirme

Kimlik doğrulamasından sonra ASP.NET Core web API, erişim yetkisi vermek için genelde gerekir, bazı kimliği doğrulanmış kullanıcılar tarafından kullanılabilir, ancak çok tüm API'leri yapmak bir hizmet sağlar.

Bir ASP.NET Core MVC yönlendirme için erişimi kısıtlamak için bir denetleyici Authorize özniteliği uygulayarak ulaşılabilecek veya denetleyici erişimi sınırlayan bir eylem veya eylem için aşağıdaki kod örneğinde gösterildiği şekilde kimliği doğrulanmış kullanıcılar:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Yetkisiz bir kullanıcı bir denetleyici veya ile işaretlenen eylem erişmeyi denerse `Authorize` özniteliği, MVC çerçevesi bir 401 (yetkisiz) HTTP durum kodu döndürür.

> [!NOTE]
> Parametreleri belirtilebilir `Authorize` API belirli kullanıcılara kısıtlamak için özniteliği. Daha fazla bilgi için [yetkilendirme](/aspnet/core/security/authorization/introduction/).

İsteğe bağlı olarak erişim belirteçleri denetim yetkilendirme sağlar, böylece IdentityServer yetkilendirme iş akışına tümleştirilebilir. Şekil 9-5'te bu yaklaşım gösterilmektedir.

![](authentication-and-authorization-images/authorization.png "Erişim belirteci yetkilendirmesi")

**Şekil 9-5:** erişim belirteci yetkilendirmesi

Hizmetine mobil uygulama kimlik mikro hizmet ile iletişim kurar ve kimlik doğrulama işleminin bir parçası olarak bir erişim belirteci ister. Erişim belirteci daha sonra erişim isteklerini bir parçası olarak sıralama ve sepet mikro hizmetler tarafından sunulan API'lerde iletilir. Erişim belirteçleri, istemci ve kullanıcı hakkındaki bilgileri içerir. API'leri, ardından verilerine erişim yetkisi vermek için bu bilgileri kullanın. API'ların korunması için IdentityServer yapılandırma hakkında daha fazla bilgi için bkz [yapılandırma API'si kaynaklarına](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Yetkilendirme gerçekleştirmeye IdentityServer yapılandırma

Yetkilendirme IdentityServer ile gerçekleştirmek için kendi yetkilendirme ara yazılımı için web uygulamasının HTTP istek işlem hattı eklenmesi gerekir. Ara yazılım eklenir `ConfigureAuth` web uygulamasının yönteminde `Startup` çağrılır sınıfı `Configure` yöntemi ve hizmetine başvuru uygulamadan alınan aşağıdaki kod örneğinde gösterilmiştir:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Bu yöntem, geçerli erişim belirteciyle yalnızca erişilebilir bir API sağlar. Ara yazılım, güvenilen bir verenden gönderilir emin olmak için gelen belirteci doğrular ve belirteci aldıktan sonra API ile kullanılacak geçerli olduğunu doğrular. Bu nedenle, sıralama veya Sepeti denetleyicisi için gözatma, bir erişim belirteci gerekli olduğunu belirten bir 401 (yetkisiz) HTTP durum kodunu döndürür.

> [!NOTE]
> IdentityServer'ın yetkilendirme ara yazılımı eklenmelidir için web uygulamasının HTTP istek işlem hattı ile MVC eklemeden önce `app.UseMvc()` veya `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>API'lere erişim isteklerini yapma

İstekleri sepet sıralama ve mikro hizmetler için erişim IdentityServer kimlik doğrulama işlemi sırasında elde edilen belirteç yaparken aşağıdaki kod örneğinde gösterildiği gibi istekte eklenmelidir:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Erişim belirteci bir uygulama ayarı olarak depolanır ve platforma özgü depolama alanından alınan ve çağrısında yer `GetOrderAsync` yönteminde `OrderService` sınıfı.

Bir IdentityServer için veri gönderen API, korumalı benzer şekilde, erişim belirteci aşağıdaki kod örneğinde gösterildiği gibi dahil edilmelidir:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Erişim belirteci platforma özgü depolama alanından alınan ve çağrıda bulunan `UpdateBasketAsync` yönteminde `BasketService` sınıfı.

`RequestProvider` Hizmetine mobil uygulamada sınıfı kullanır `HttpClient` hizmetine başvuru uygulama tarafından kullanıma sunulan RESTful API'leri isteklerde bulunmak için sınıf. Sıralama ve sepet yetkilendirme gerektiren API'leri yapma istediğinde, geçerli bir erişim belirteciyle istekle dahil edilmelidir. Bu erişim belirteci için üst bilgilerini ekleyerek gerçekleştirilir `HttpClient` , aşağıdaki kod örneğinde gösterildiği gibi örnek:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Özelliği `HttpClient` sınıfı her istekle beraber gönderilen üst bilgiler sunar ve erişim belirteci eklendiğinden `Authorization` dizesiyle ön ekli üst bilgi `Bearer`. İstek değeri bir RESTful API'sine gönderilen zaman `Authorization` üstbilgi ayıklanır ve güvenilir bir verenden göndermiş olduğunu ve kullanıcının API'yi çağırmak için izni olup olmadığını belirlemek için kullanılan, aldığı doğrulanarak.

Nasıl hizmetine mobil uygulama web isteği yapan hakkında daha fazla bilgi için bkz. [uzak veri erişim](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Özet

Kimlik doğrulama ve yetkilendirme, bir ASP.NET MVC web uygulaması ile iletişim kuran bir Xamarin.Forms uygulamasına tümleştirmek için birçok yaklaşım vardır. Hizmetine mobil uygulama kimlik doğrulaması ve yetkilendirme IdentityServer 4 kullanan bir kimlik kapsayıcılı mikro hizmet ile gerçekleştirir. IdentityServer bir açık kaynak Openıd Connect ve OAuth 2.0 taşıyıcı belirteci kimlik doğrulaması gerçekleştirmek için ASP.NET Core kimliği ile tümleşen bir ASP.NET Core çerçevesidir.

Mobil uygulama veya bir kullanıcı kimlik doğrulaması için bir kaynağa erişim sağlamak için güvenlik belirteçleri IdentityServer ister. Bir kaynağa erişirken bir erişim belirteci isteğinde yetkilendirme gerektiren API'lerine eklenmesi gerekir. IdentityServer'ın Ara yazılım, güvenilen bir verenden gönderilir ve bunlar aldığı API ile kullanılacak geçerli olduğundan emin olmak için gelen erişim belirteçlerini doğrular.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
