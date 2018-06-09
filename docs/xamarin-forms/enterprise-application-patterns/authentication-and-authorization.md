---
title: Kimlik doğrulama ve yetkilendirme
description: Bu bölümde eShopOnContainers mobil uygulama kimlik doğrulama ve yetkilendirme kapsayıcılı mikro karşı nasıl gerçekleştireceğini açıklar.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 9e6cfa566ab455841b3f11e4a857dcf678083417
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242434"
---
# <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

Kimlik doğrulaması bir kullanıcı adı ve parola gibi kimlik alma ve bu kimlik bilgilerini bir yetkilisi karşı doğrulama işlemidir. Kimlik bilgileri geçerli olduğunda, kimlik bilgilerini gönderen varlık kimlik doğrulaması olarak kabul edilir. Bir kimlik doğrulandıktan sonra bir yetkilendirme işlemine kimliğini belirli bir kaynağa erişim izni olup olmadığını belirler.

Microsoft, Google gibi dış kimlik doğrulama sağlayıcıları ASP.NET Core kimliği kullanımı dahil olmak üzere bir ASP.NET MVC web uygulaması ile iletişim kuran bir Xamarin.Forms uygulamada kimlik doğrulama ve yetkilendirme tümleştirmek için birçok yaklaşım vardır, Facebook, ya da Twitter ve kimlik doğrulaması ara yazılımı. EShopOnContainers mobil uygulama kimlik doğrulama ve yetkilendirme IdentityServer 4 kullanan bir kapsayıcılı kimlik mikro hizmet ile gerçekleştirir. Mobil uygulama bir kullanıcı kimlik doğrulaması için veya bir kaynağa erişim sağlamak için güvenlik belirteçleri IdentityServer istekleri. Bir kullanıcı adına sorunu belirteçleri için IdentityServer için kullanıcı oturumu için IdentityServer açma gerekir. Ancak, IdentityServer bir kullanıcı arabirimi veya veritabanı için kimlik doğrulaması sağlamaz. Bu nedenle, eShopOnContainers başvuru uygulaması ASP.NET Core kimliği bu amaç için kullanılır.

## <a name="authentication"></a>Kimlik doğrulaması

Uygulamanın geçerli kullanıcının kimliğinin bilinmesi gerektiğinde kimlik doğrulaması gereklidir. Kullanıcıları tanımlamak için ASP.NET Core'nın birincil geliştirici tarafından yapılandırılan bir veri deposu kullanıcı bilgilerini depolar ASP.NET Core kimlik üyelik sistemi mekanizmadır. Genellikle, bu veri deposu Azure storage, Azure Cosmos DB veya başka konumlara kimlik bilgilerini depolamak için özel depoları veya üçüncü taraf paketleri kullanılabilmesine rağmen bir EntityFramework deposu olacaktır.

Bir yerel kullanıcı veri deposunu olun kimlik doğrulama senaryoları kullanın ve kimlik bilgilerini (ASP.NET MVC web uygulamalarında tipik olarak) tanımlama bilgileri aracılığıyla istekler arasında kalıcı hale getirmek için ASP.NET Core kimliği uygun bir çözümdür. Ancak, tanımlama bilgileri her zaman kalıcı yapma ve veri aktarırken bir doğal değildir. Örneğin, mobil uygulamadan erişilen RESTful uç noktalarını kullanıma sunan bir ASP.NET Core web uygulaması genellikle tanımlama bilgilerini bu senaryoda kullanılamaz olduğundan taşıyıcı belirteci kimlik doğrulaması kullanmanız gerekir. Ancak, taşıyıcı belirteçlerini kolayca alınabilir ve mobil uygulama web isteklerini yetkilendirme üstbilgisinin dahildir.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Taşıyıcı IdentityServer 4 kullanan belirteç

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) yerel ASP.NET Core kimliği kullanıcılar için güvenlik belirteçleri verme dahil olmak üzere birçok kimlik doğrulama ve yetkilendirme senaryoları için kullanılan ASP.NET Core bir açık kaynak Openıd Connect ve OAuth 2.0 framework içindir.

> [!NOTE]
> Openıd Connect ve OAuth 2.0 farklı sorumlulukları yaparken çok benzer.

Openıd Connect bir OAuth 2.0 protokolünü üstünde kimlik doğrulaması katmanıdır. OAuth 2 uygulamaların bir güvenlik belirteci Hizmeti'nden erişim belirteçleri istemek ve bunları API'leri ile iletişim kurmanızı sağlayan bir protokoldür. Kimlik doğrulama ve yetkilendirme merkezi beri bu temsilci istemci uygulamalar ve API'ler karmaşıklığını azaltır.

Openıd Connect ve OAuth 2.0 birleşimi iki temel güvenlik sorunlarının kimlik doğrulama ve API erişimini birleştirmek ve IdentityServer 4 bu protokollerin bir uygulamasıdır.

EShopOnContainers başvuru uygulaması gibi doğrudan istemci mikro hizmet iletişimi kullanan uygulamalarda bir güvenlik belirteci hizmeti (STS) olarak davranan bir özel kimlik doğrulama mikro hizmet kullanıcıların kimliklerini doğrulamak için şekilde gösterildiği gibi kullanılabilir 9-1. Doğrudan istemci mikro hizmet iletişim hakkında daha fazla bilgi için bkz: [istemci arasında iletişim ve mikro Hizmetler](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Özel kimlik doğrulama mikro hizmet tarafından kimlik doğrulaması")

**Şekil 9-1:** özel kimlik doğrulama mikro hizmet tarafından kimlik doğrulaması

EShopOnContainers mobil uygulama kimlik doğrulaması yapmak ve API'ler için erişim denetimini IdentityServer 4 kullanan kimlik mikro hizmet ile iletişim kurar. Bu nedenle, mobil uygulama belirteçleri IdentityServer bir kullanıcı kimlik doğrulaması için veya bir kaynağa erişim sağlamak için istek:

-   Kullanıcıların IdentityServer ile kimlik doğrulaması gerçekleştirilir mobil uygulama isteyerek bir *kimlik* bir kimlik işleminin sonucunu temsil eden bir belirteç. Tam en azından kullanıcı ve nasıl ve ne zaman kullanıcı kimlik doğrulaması hakkında daha fazla bilgi için bir tanımlayıcı içerir. Ayrıca, ek kimlik verilerini de içerebilir.
-   IdentityServer sahip bir kaynak erişim elde edilir mobil uygulama isteyerek bir *erişim* bir API kaynağa erişim izni verir belirteci. İstemcilerin erişim belirteçleri isteyin ve API için iletin. Erişim belirteçleri, (varsa) istemci ve kullanıcı hakkındaki bilgileri içerir. API'leri daha sonra verilerine erişim yetkisi vermek için bu bilgileri kullanın.

> [!NOTE]
> Belirteçleri isteyebilmesi istemci IdentityServer ile kayıtlı olması gerekir.

### <a name="adding-identityserver-to-a-web-application"></a>Bir Web uygulamasına IdentityServer ekleme

Sırayla IdentityServer 4 kullanmak bir ASP.NET Core web uygulaması için web uygulamasının Visual Studio çözümüne eklenmiş olması gerekir. Daha fazla bilgi için bkz: [Kurulum ve genel bakış](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) IdentityServer belgelerinde.

IdentityServer web uygulamasının Visual Studio çözümünde dahil sonra böylece Openıd Connect ve OAuth 2.0 uç noktaları isteklerine hizmet vermek için web uygulamasının HTTP istek ardışık işleme eklenmelidir. Bunu elde edilen `Configure` web uygulamasının yönteminde `Startup` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

İçinde web uygulamasının HTTP istek ardışık düzen işleme sırası önemlidir. Bu nedenle, IdentityServer oturum açma ekranı uygulayan UI çerçevesi önce ardışık düzene eklenmelidir.

### <a name="configuring-identityserver"></a>IdentityServer yapılandırma

IdentityServer yapılandırılmalıdır `ConfigureServices` web uygulamasının yönteminde `Startup` çağırarak sınıfı `services.AddIdentityServer` eShopOnContainers başvuru uygulamasından aşağıdaki kod örneğinde gösterildiği şekilde yöntemi:

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

Çağırdıktan sonra `services.AddIdentityServer` yöntemi, ek fluent API aşağıdakileri yapılandırmak için çağrılır:

-   İmzalama için kullanılan kimlik bilgileri.
-   Kullanıcıların isteyebilir API ve kimlik kaynaklara erişim sağlar.
-   Belirteç isteme bağlanan istemciler.
-   ASP.NET Core kimliği.

>💡 **İpucu**: IdentityServer 4 yapılandırmasını dinamik olarak yükleme. IdentityServer 4 API'leri yapılandırma nesneleri, bir bellek içi listesinden IdentityServer yapılandırılmasını destekler. EShopOnContainers başvuru uygulaması bu uygulamaya sabit kodlanmış bellek içi koleksiyonlarıdır. Ancak, üretim senaryolarında bunların dinamik olarak bir yapılandırma dosyası veya bir veritabanından yüklenebilir.

ASP.NET Core kimlik kullanmak üzere IdentityServer yapılandırma hakkında daha fazla bilgi için bkz: [kullanarak ASP.NET Core kimliği](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) IdentityServer belgelerinde.

#### <a name="configuring-api-resources"></a>API kaynaklarını yapılandırma

API kaynakları yapılandırırken `AddInMemoryApiResources` yönteminin beklediği bir `IEnumerable<ApiResource>` koleksiyonu. Aşağıdaki örnekte gösterildiği kod `GetApis` eShopOnContainers bu koleksiyonda sağlayan yöntemi başvuru uygulama:

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

Bu yöntem IdentityServer Sepeti API'leri ve siparişleri korumalısınız belirtir. Bu nedenle, IdentityServer yönetilen erişim bu API'lerine çağrılar yapılırken belirteçleri gerekli olacaktır. Hakkında daha fazla bilgi için `ApiResource` yazın, bkz: [API kaynak](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) IdentityServer 4 belgelerinde.

#### <a name="configuring-identity-resources"></a>Kimlik kaynaklarını yapılandırma

Identity kaynaklar yapılandırırken `AddInMemoryIdentityResources` yönteminin beklediği bir `IEnumerable<IdentityResource>` koleksiyonu. Kimlik, kullanıcı kimliği, ad veya e-posta adresi gibi veri kaynaklardır. Her kimlik kaynak benzersiz bir ada sahip ve rasgele talep türleri, hangi kullanıcı için kimlik belirteci sonra dahil edilecek atanabilir. Aşağıdaki örnekte gösterildiği kod `GetResources` eShopOnContainers bu koleksiyonda sağlayan yöntemi başvuru uygulama:

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

Openıd Connect belirtimi bazı belirtir [standart Identity kaynaklar](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Kullanıcılar için benzersiz bir kimliği yayma için destek sağlanan en düşük gereksinimdir. Bu göstererek sağlanır `IdentityResources.OpenId` kimlik kaynak.

> [!NOTE]
> `IdentityResources` Sınıfı, tüm Openıd Connect açıklamasında (openıd, e-posta, profil, telefon ve adresi) tanımlanan kapsamların destekler.

Özel kimlik kaynakları tanımlama IdentityServer de destekler. Daha fazla bilgi için bkz: [özel kimlik kaynakları tanımlama](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) IdentityServer belgelerinde. Hakkında daha fazla bilgi için `IdentityResource` yazın, bkz: [kimlik kaynak](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) IdentityServer 4 belgelerinde.

#### <a name="configuring-clients"></a>İstemcilerini yapılandırma

İstemcileri belirteçleri IdentityServer isteyebileceği uygulamalardır. Genellikle, aşağıdaki ayarları her istemci için en az tanımlanmış olması gerekir:

-   Benzersiz istemci kimliği.
-   İzin verilen etkileşimler belirteci hizmetiyle (grant türü olarak da bilinir).
-   Burada kimlik ve erişim belirteçleri gönderilen konum (bir yeniden yönlendirme URI'si da bilinir).
-   İstemci erişim izni verilir kaynakların bir listesini (kapsamlar olarak da bilinir).

İstemciler, yapılandırırken `AddInMemoryClients` yönteminin beklediği bir `IEnumerable<Client>` koleksiyonu. Aşağıdaki kod örneğinde eShopOnContainers mobil uygulama yapılandırması gösterilmektedir `GetClients` eShopOnContainers bu koleksiyonda sağlayan yöntemi başvuru uygulama:

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

-   `ClientId`İstemci benzersiz kimliği.
-   `ClientName`: İstemci günlüğü ve onay ekran için kullanılan adını görüntüler.
-   `AllowedGrantTypes`: Bir istemci nasıl IdentityServer ile etkileşim kurmak istediği belirtir. Daha fazla bilgi için bkz: [kimlik doğrulaması akışı yapılandırma](#configuring_the_authentication_flow).
-   `ClientSecrets`: İsteyen belirteç uç noktasından belirteçler yükleyen kullanılan istemci gizli kimlik bilgilerini belirtir.
-   `RedirectUris`: Eklendiği belirteçler veya yetkilendirme kodları döndürmek izin verilen URI belirtir.
-   `RequireConsent`: Bir onay ekranı gerekip gerekmediğini belirtir.
-   `RequirePkce`: Bir kimlik doğrulama kodu kullanan istemciler kanıt anahtarı göndermesi gerekip gerekmediğini belirtir.
-   `PostLogoutRedirectUris`: Oturum kapatma yeniden yönlendirmek için izin verilen URI belirtir.
-   `AllowedCorsOrigins`: İstemci kaynak IdentityServer arası çağrı kaynaktan izin verebilir böylece belirtir.
-   `AllowedScopes`: İstemci erişimi kaynaklarını belirtir. Varsayılan olarak, bir istemci hiçbir herhangi bir kaynağa erişebilir.
-   `AllowOfflineAccess`: İstemci yenileme belirteçleri isteyip isteyemeyeceklerini belirtir.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Kimlik doğrulaması akışı yapılandırma

Kimlik doğrulaması akışı bir istemci arasında IdentityServer grant türlerinde belirterek yapılandırılabilir `Client.AllowedGrantTypes` özelliği. Openıd Connect ve OAuth 2.0 belirtimleri kimlik doğrulaması akışı dahil olmak üzere, çeşitli tanımlayın:

-   Örtük. Bu akış tarayıcı tabanlı uygulamalar için en iyi duruma getirilmiş ve kullanıcı yalnızca kimlik doğrulama veya yetkilendirme ve erişim belirteci istekleri için kullanılmalıdır. Tüm belirteçleri tarayıcı üzerinden aktarılan ve bu nedenle gelişmiş yenileme belirteçleri verilmeyen gibi özellikler.
-   Yetkilendirme kodu. Bu akış özelliği de istemci kimlik doğrulaması desteklerken tarayıcı ön kanal aksine arka kanal belirteçlerini almasını sağlar.
-   Karma. Bu akış örtük bileşimidir ve yetki kodu izin türleri. Kimlik belirtecinin tarayıcı kanal iletilen ve yetkilendirme kodu gibi diğer yapılarını birlikte imzalı protokolü yanıt içerir. Yanıtın doğrulama başarılı olduktan sonra arka kanal erişim almak ve belirteç yenilemek için kullanılması gerekir.

> [!TIP]
> Karma kimlik doğrulama akışı kullanın. Karma kimlik doğrulama akışı tarayıcı kanala Uygula saldırılarını sayısını azaltır ve erişim belirteçlerini almasını (ve muhtemelen yenileme belirteçlerini) istediğiniz yerel uygulamalar için önerilen akışıdır.

Kimlik doğrulama akışı hakkında daha fazla bilgi için bkz: [sağlama türleri](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) IdentityServer 4 belgelerinde.

### <a name="performing-authentication"></a>Kimlik doğrulaması gerçekleştirme

Bir kullanıcı adına sorunu belirteçleri için IdentityServer için kullanıcı oturumu için IdentityServer açma gerekir. Ancak, IdentityServer bir kullanıcı arabirimi veya veritabanı için kimlik doğrulaması sağlamaz. Bu nedenle, eShopOnContainers başvuru uygulaması ASP.NET Core kimliği bu amaç için kullanılır.

Şekil 9-2'de gösterilen karma kimlik doğrulama akışı ile IdentityServer ile eShopOnContainers mobil uygulama kimlik doğrulamasını yapar.

![](authentication-and-authorization-images/sign-in.png "Oturum açma işleminin üst düzey genel bakış")

**Şekil 9-2:** oturum açma işleminin üst düzey genel bakış

Bir oturum açma isteği yapılan `<base endpoint>:5105/connect/authorize`. Başarılı kimlik doğrulamasının, IdentityServer bir yetkilendirme kodu ve bir kimlik belirteci içeren bir kimlik doğrulama yanıtı döndürür. Yetkilendirme kodu sonra gönderilir `<base endpoint>:5105/connect/token`, erişim, kimlik ve yenileme belirteçleri ile yanıtlar.

EShopOnContainers mobil uygulama işaretlerini-bir istek göndererek dışı IdentityServer `<base endpoint>:5105/connect/endsession`, ek parametrelere sahip. Oturum kapatma gerçekleştikten sonra IdentityServer mobil uygulamasına bir post oturumu kapatıp yeniden yönlendirme URI'si göndererek yanıt verir. Şekil 9-3 Bu işlemi gösterilmektedir.

![](authentication-and-authorization-images/sign-out.png "Oturum kapatma işlemini üst düzey genel bakış")

**Şekil 9-3:** oturum kapatma işlemini üst düzey genel bakış

Mobil uygulama eShopOnContainers IdentityServer ile iletişim tarafından gerçekleştirilen `IdentityService` sınıfı, hangi uygulayan `IIdentityService` arabirimi. Bu arabirimi uygulayan sınıfa sağlamalısınız belirtir `CreateAuthorizationRequest`, `CreateLogoutRequest`, ve `GetTokenAsync` yöntemleri.

#### <a name="signing-in"></a>Oturum açma

Kullanıcı ne zaman dokunur **oturum açma** düğmesini `LoginView`, `SignInCommand` içinde `LoginViewModel` sınıfı yürütüldüğünde, hangi sırayla yürütür `SignInAsync` yöntemi. Aşağıdaki kod örneği, bu yöntem gösterilmektedir:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Bu yöntemi çağırır `CreateAuthorizationRequest` yönteminde `IdentityService` aşağıdaki kod örneğinde gösterildiği sınıfı:

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

Bu yöntem IdentityServer için ait URI oluşturur [yetkilendirme uç noktası](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), gerekli parametrelere sahip. Yetkilendirme uç noktası altındadır `/connect/authorize` bağlantı noktası üzerindeki kullanıcı ayarı olarak sunulan temel uç noktasının 5105. Kullanıcı ayarları hakkında daha fazla bilgi için bkz: [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers mobil uygulama, saldırı yüzeyini OAuth kod Exchange (PKCE) uzantısı için kanıt anahtar uygulayarak azalır. PKCE müdahale varsa kullanılan yetkilendirme kodu korur. Bu bir karmasını yetkilendirme isteği geçirilir, gizli bir doğrulayıcı oluşturma istemci tarafından sağlanır ve hangi sunulan yetkilendirme kodu redeeming zaman doğrulayamaz. PKCE hakkında daha fazla bilgi için bkz: [kod Exchange OAuth ortak istemcileri tarafından kanıt anahtarı](https://tools.ietf.org/html/rfc7636) Internet Engineering Task Force web sitesinde.

Döndürülen URI depolanan `LoginUrl` özelliği `LoginViewModel` sınıfı. Zaman `IsLogin` özellik olur `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) içinde `LoginView` görünür hale gelir. `WebView` Veri bağlamalar kendi [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) özelliğine `LoginUrl` özelliği `LoginViewModel` sınıfı ve bu nedenle bir oturum açma için IdentityServer istekte zaman `LoginUrl` özelliği ayarlanmış IdentityServer'ın yetkilendirme uç noktası. IdentityServer bu isteği alır ve kullanıcının doğrulandığında değil, `WebView` Şekil 9-4'te gösterilen yapılandırılmış oturum açma sayfasına yönlendirilir.

![](authentication-and-authorization-images/login.png "Web görünümü tarafından görüntülenen oturum açma sayfası")

**Şekil 9-4:** WebView tarafından görüntülenen oturum açma sayfası

Oturum açma tamamlandıktan sonra [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) dönüş URI'yı yeniden yönlendirilir. Bu `WebView` Gezinti neden olacak `NavigateAsync` yönteminde `LoginViewModel` aşağıdaki kod örneğinde gösterildiği yürütülmek üzere sınıfı:

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

Bu yöntemin dönüş URI'de bulunan kimlik doğrulama yanıtı ayrıştırır ve geçerli bir yetkilendirme kod mevcut olduğundan koşuluyla IdentityServer için kullanıcının istekte bulunan [belirteç uç noktası](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), yetkilendirme kodu geçirme PKCE gizli Doğrulayıcı ve diğer parametreler gereklidir. Belirteç uç noktası altındadır `/connect/token` bağlantı noktası üzerindeki kullanıcı ayarı olarak sunulan temel uç noktasının 5105. Kullanıcı ayarları hakkında daha fazla bilgi için bkz: [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **İpucu**: doğrulama URI'lar döndürür. EShopOnContainers mobil uygulama dönüş URI doğrulamak değil de, en iyi uygulama dönüş URI açma yeniden yönlendirme saldırılarını önlemek için bilinen bir konuma, başvuruyor doğrulamaktır.

Belirteç uç noktası geçerli bir yetkilendirme kodu ve PKCE gizli Doğrulayıcı alırsa, bir erişim belirteci, kimlik belirtecinin ve yenileme belirteci ile yanıt verir. Uygulama ayarları (API kaynaklara erişim sağlayan) erişim belirteci ve kimlik belirtecinin sonra depolanır ve sayfa gezintisi gerçekleştirilir. Bu nedenle, genel eShopOnContainers mobil uygulama bu etkilidir: kullanıcıların başarılı bir şekilde IdentityServer ile kimlik doğrulaması için olması koşuluyla, bunlar için gittiğinizde `MainView` olan sayfası, bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) görüntüleyen `CatalogView` seçili sekme olarak.

Sayfa gezintisi hakkında daha fazla bilgi için bkz: [Gezinti](~/xamarin-forms/enterprise-application-patterns/navigation.md). Hakkında bilgi için [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Gezinti neden olan bir görünüm modeli yöntemi yürütülmesi için bkz: [davranışları kullanarak gezinti çağırma](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Uygulama ayarları hakkında daha fazla bilgi için bkz: [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Uygulama sahte Hizmetleri'nde kullanmak üzere yapılandırıldığında eShopOnContainers ayrıca bir sahte oturum açma sağlar `SettingsView`. Bu modda uygulamanın yerine tüm kimlik bilgilerini kullanarak oturum açın kullanıcıya izin vererek IdentityServer ile iletişim değil.

#### <a name="signing-out"></a>İmzalama genişletme

Kullanıcı ne zaman dokunur **LOG OUT** düğmesini `ProfileView`, `LogoutCommand` içinde `ProfileViewModel` sınıfı yürütüldüğünde, hangi sırayla yürütür `LogoutAsync` yöntemi. Bu yöntem için sayfa gezintisi gerçekleştirir `LoginView` geçirme sayfasında, bir `LogoutParameter` örneğini ayarlamak `true` bir parametre olarak. Sayfa gezintisi sırasında parametreleri geçirme hakkında daha fazla bilgi için bkz: [gezinti sırasında geçirme parametreleri](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Bir görünüm oluşturduğunuzda ve için gittiğinizde `InitializeAsync` görünümün ilişkili görünüm modeli yöntemi yürütüldüğünde, hangi ardından yürütür `Logout` yöntemi `LoginViewModel` aşağıdaki kod örneğinde gösterildiği sınıfı:

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

Bu yöntemi çağırır `CreateLogoutRequest` yönteminde `IdentityService` kimlik belirtecinin geçirme sınıfı, bir parametre olarak uygulama ayarlarından alınır. Uygulama ayarları hakkında daha fazla bilgi için bkz: [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Aşağıdaki örnekte gösterildiği kod `CreateLogoutRequest` yöntemi:

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

Bu yöntem IdentityServer'ın için URI oluşturur [oturum uç noktası end](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), gerekli parametrelere sahip. En son oturumun uç noktadır `/connect/endsession` bağlantı noktası üzerindeki kullanıcı ayarı olarak sunulan temel uç noktasının 5105. Kullanıcı ayarları hakkında daha fazla bilgi için bkz: [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Döndürülen URI depolanan `LoginUrl` özelliği `LoginViewModel` sınıfı. Sırada `IsLogin` özelliği `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) içinde `LoginView` görünür. `WebView` Veri bağlamalar kendi [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) özelliğine `LoginUrl` özelliği `LoginViewModel` sınıfı ve bu nedenle oturum kapatma için IdentityServer istekte zaman `LoginUrl` özelliği ayarlanmış IdentityServer'ın son oturum uç noktası. Kullanıcı oturum açmış koşuluyla IdentityServer bu isteği aldığında, oturum kapatma oluşur. Kimlik doğrulama tanımlama bilgisi kimlik doğrulaması Ara ASP.NET Core tarafından yönetilen bir tanımlama bilgisi ile izlenir. Bu nedenle, IdentityServer dışında imzalama kimlik doğrulama tanımlama bilgisini kaldırır ve post oturum kapatma yönlendirme URI istemciye geri gönderir.

Mobil uygulama [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) post oturumu kapatıp yeniden yönlendirme URI'si yönlendirilir. Bu `WebView` Gezinti neden olacak `NavigateAsync` yönteminde `LoginViewModel` aşağıdaki kod örneğinde gösterildiği yürütülmek üzere sınıfı:

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

Bu yöntem, kimlik belirtecinin ve uygulama ayarlarını erişim belirtecinden temizler ve ayarlar `IsLogin` özelliğine `false`, hangi nedenler [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) üzerinde `LoginView` görünmez olmasını sayfası . Son olarak, `LoginUrl` özelliği URI IdentityServer için kullanıcının ayarlanmış [yetkilendirme uç noktası](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), gerekli parametreleri içeren sonraki hazırlığı kullanıcı bir oturum açma işlemini başlatır.

Sayfa gezintisi hakkında daha fazla bilgi için bkz: [Gezinti](~/xamarin-forms/enterprise-application-patterns/navigation.md). Hakkında bilgi için [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Gezinti neden olan bir görünüm modeli yöntemi yürütülmesi için bkz: [davranışları kullanarak gezinti çağırma](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Uygulama ayarları hakkında daha fazla bilgi için bkz: [yapılandırma yönetimi](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Uygulama SettingsView sahte hizmetlerini kullanmak üzere yapılandırıldığı zaman eShopOnContainers de bir mock oturum kapatma sağlar. Bu modda, uygulama ile IdentityServer iletişim değil ve bunun yerine uygulama ayarlarından herhangi bir saklı belirtece temizler.

<a name="authorization" />

## <a name="authorization"></a>Yetkilendirme

Kimlik doğrulamasından sonra ASP.NET Core web API'leri, erişim yetkisi vermek için çoğunlukla gerekir, bazı kimliği doğrulanmış kullanıcılar için kullanılabilir, ancak değil tüm API'leri yapmak bir hizmet sağlar.

Bir ASP.NET Core MVC rotaya erişimi kısıtlama bir denetleyiciye Authorize özniteliğin uygulayarak elde edilebilir veya denetleyicisine erişimi sınırlar eylem veya eylem için aşağıdaki kod örneğinde gösterildiği şekilde kimliği doğrulanmış kullanıcılar:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Yetkisiz bir kullanıcının bir denetleyici veya ile işaretlenen eylem erişmeyi denerse `Authorize` özniteliği, MVC çerçevesi bir 401 (yetkisiz) HTTP durum kodu döndürür.

> [!NOTE]
> Parametreleri belirtilebilir `Authorize` bir API belirli kullanıcılara kısıtlamak için özniteliği. Daha fazla bilgi için bkz: [yetkilendirme](/aspnet/core/security/authorization/introduction/).

İsteğe bağlı olarak erişim belirteçleri denetim yetkilendirme sağlamasını IdentityServer yetkilendirme iş akışı ile tümleştirilebilir. Bu yaklaşım, Şekil 9-5'te gösterilir.

![](authentication-and-authorization-images/authorization.png "Erişim belirteci yetkilendirmesi")

**Şekil 9-5:** erişim belirteci yetkilendirmesi

EShopOnContainers mobil uygulama kimlik mikro hizmet ile iletişim kurar ve kimlik doğrulama işleminin bir parçası olarak bir erişim belirteci ister. Erişim belirteci sonra sıralama ve sepeti mikro hizmetler tarafından erişim isteklerini bir parçası olarak sunulan API'lerde iletilir. Erişim belirteçleri, istemci ve kullanıcı hakkındaki bilgileri içerir. API'leri daha sonra verilerine erişim yetkisi vermek için bu bilgileri kullanın. API'leri korumak için IdentityServer yapılandırma hakkında daha fazla bilgi için bkz: [yapılandırma API kaynakları](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Yetkilendirme gerçekleştirmek üzere IdentityServer yapılandırma

Yetkilendirme IdentityServer ile gerçekleştirmek için kendi yetkilendirme ara yazılımı web uygulamasının HTTP istek ardışık düzene eklenmelidir. Ara yazılım eklenir `ConfigureAuth` web uygulamasının yönteminde `Startup` öğesinden çağrılır sınıfı `Configure` yöntemi ve aşağıdaki kod örneğinde eShopOnContainers başvuru uygulamasından gösterilmiştir:

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

Bu yöntem, geçerli erişim belirteciyle API yalnızca erişilip sağlar. Ara yazılım bir güvenilen verenin gönderilir emin olmak için gelen belirteci doğrular ve belirteç aldığı API ile kullanılması için geçerli olduğunu doğrular. Bu nedenle, sıralama veya Sepeti denetleyiciye gözatma bir erişim belirteci gerekli olduğunu belirten bir 401 (yetkisiz) HTTP durum kodunu döndürür.

> [!NOTE]
> IdentityServer'ın yetkilendirme ara yazılımı eklenmelidir web uygulamasının HTTP istek ardışık düzenine sahip MVC eklemeden önce `app.UseMvc()` veya `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>API'lerine erişim isteklerini yapma

İstekleri sıralama ve sepeti mikro hizmetler için erişim IdentityServer kimlik doğrulama işlemi sırasında elde edilen belirteç, yaparken, aşağıdaki kod örneğinde gösterildiği gibi isteğine dahil gerekir:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Erişim belirteci bir uygulama ayarı olarak depolanır ve platforma özgü depolama biriminden alınan ve çağrısında yer `GetOrderAsync` yönteminde `OrderService` sınıfı.

API, korumalı bir IdentityServer verileri gönderme benzer şekilde, erişim belirteci aşağıdaki kod örneğinde gösterildiği gibi dahil edilmelidir:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Erişim belirteci platforma özgü depolama biriminden alınan ve çağrısında dahil `UpdateBasketAsync` yönteminde `BasketService` sınıfı.

`RequestProvider` EShopOnContainers mobil uygulamasında sınıfı kullanır `HttpClient` eShopOnContainers başvuru uygulaması tarafından sunulan RESTful API'lerini istekleri yapmak için sınıf. Sıralama ve sepeti yetkilendirme gerektiren API'leri yapma istediğinde, geçerli erişim belirteci istekle dahil edilmelidir. Bu üst bilgilerinin için erişim belirteci ekleyerek sağlanır `HttpClient` , aşağıdaki kod örneğinde gösterildiği şekilde örneği:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Özelliği `HttpClient` sınıfı, her bir istekle gönderilen üstbilgileri sunar ve erişim belirteci eklenir `Authorization` dizesiyle önekli üstbilgi `Bearer`. İstek bir RESTful API'si değerini gönderilen zaman `Authorization` üstbilgi ayıklanan ve güvenilir bir verenden gönderdi ve kullanıcı API çağırma izni olup olmadığını belirlemek için kullanılan, aldığı emin olmak için doğrulandı.

EShopOnContainers mobil uygulama web istekleri nasıl kolaylaştırdığını hakkında daha fazla bilgi için bkz: [erişme uzak veri](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Özet

Bir ASP.NET MVC web uygulamasıyla iletişim kuran bir Xamarin.Forms uygulamada kimlik doğrulama ve yetkilendirme tümleştirmek için birçok yaklaşım vardır. EShopOnContainers mobil uygulama kimlik doğrulama ve yetkilendirme IdentityServer 4 kullanan bir kapsayıcılı kimlik mikro hizmet ile gerçekleştirir. IdentityServer, taşıyıcı belirteci kimlik doğrulaması gerçekleştirmek için ASP.NET Core kimliği ile tümleşir ASP.NET Core bir açık kaynak Openıd Connect ve OAuth 2.0 çerçevedir.

Mobil uygulama bir kullanıcı kimlik doğrulaması için veya bir kaynağa erişim sağlamak için güvenlik belirteçleri IdentityServer istekleri. Bir kaynağa erişilirken bir erişim belirteci yetkilendirme gerektiren API'leri isteğine dahil edilmesi gerekir. IdentityServer'ın ara yazılımı gelen erişim belirteçleri güvenilen verenden gönderilir ve bunları alan API ile kullanılması için geçerli olduğundan emin olun doğrular.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
