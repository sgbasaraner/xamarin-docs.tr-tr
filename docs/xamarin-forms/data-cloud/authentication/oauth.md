---
title: Bir kimlik sağlayıcısı ile kullanıcıların kimliğini doğrulama
description: Bu makalede, bir Xamarin.Forms uygulamada kimlik doğrulaması işlemini yönetmek için Xamarin.Auth kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 504b2789ef61b0339d1c32e92c852a779a193b52
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854772"
---
# <a name="authenticating-users-with-an-identity-provider"></a>Bir kimlik sağlayıcısı ile kullanıcıların kimliğini doğrulama

_Xamarin.Auth bir çoklu platform SDK'sı, kullanıcıların kimliğini doğrulama ve depolama hesaplarına içindir. Bu, Google, Microsoft, Facebook ve Twitter gibi kimlik sağlayıcılarla kullanma için destek sağlayan bir OAuth kimlik doğrulayıcılar içerir. Bu makalede, bir Xamarin.Forms uygulamada kimlik doğrulaması işlemini yönetmek için Xamarin.Auth kullanmayı açıklar._

OAuth kimlik doğrulaması için bir açık standarttır ve bir kaynak sağlayıcısı kaynak sahiplerinin kimliğini paylaşmadan bilgilerine erişmek için bir üçüncü tarafa izni verilmelidir bildirmek bir kaynak sahibi sağlar. Buna örnek olarak, kullanıcının kimliğini paylaşmadan, verilere erişmek için bir uygulama için izin verilmelidir (örneğin, Google, Microsoft, Facebook veya Twitter gibi) bir kimlik sağlayıcısı bildirmek bir kullanıcı etkinleştirme. Genellikle kullanıcılar için bir yaklaşım olarak Web siteleri ve uygulamalar bir kimlik sağlayıcısı kullanarak, ancak Web sitesi veya uygulama parolalarını gösterme olmadan oturum için kullanılır.

Bir OAuth kimlik sağlayıcısı kullanan kimlik doğrulaması akışı üst düzey bir genel bakış aşağıdaki gibidir:

1. Uygulamanın bir kimlik sağlayıcısı URL'si için bir tarayıcı gider.
1. Kimlik sağlayıcısı, kullanıcı kimlik doğrulaması gerçekleştirir ve uygulamaya bir yetkilendirme kodu döndürür.
1. Uygulama erişim belirteci kimlik sağlayıcısından alınan yetkilendirme kodunu değiştirir.
1. Uygulama, temel kullanıcı verileri istemeye ilişkin bir API gibi bir kimlik sağlayıcısı API'lere erişim belirtecini kullanır.

Örnek uygulamayı Xamarin.Auth Google karşı bir yerel kimlik doğrulaması akışını uygulamak için nasıl kullanılacağını gösterir. Bu konuda kimlik sağlayıcısı olarak Google kullanılırken, diğer kimlik sağlayıcıları için eşit oranda geçerli bir yaklaşımdır. Google OAuth 2.0 uç noktayı kullanarak kimlik doğrulaması hakkında daha fazla bilgi için bkz. [erişim Google API'leri kullanarak OAuth2.0](https://developers.google.com/identity/protocols/OAuth2) Google'nın Web sitesinde.

> [!NOTE]
> İOS 9 ve üzeri, uygulama taşıma güvenliği (ATS), böylece hassas bilgilerin yanlışlıkla açığa önleme internet kaynakları (örneğin, uygulamanın arka uç sunucu) ve uygulama arasında güvenli bağlantılar zorlar. ATS iOS 9 için oluşturulmuş uygulamalarda varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerine tabi olacaktır. Bağlantıları bu gereksinimleri karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul tanesi kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim için internet kaynakları. Bu uygulamanın güncelleştirerek gerçekleştirilebilir **Info.plist** dosya. Daha fazla bilgi için [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Kullanıcıların kimliğini doğrulamak için Xamarin.Auth kullanma

Uygulamaların bir kimlik sağlayıcısının yetkilendirme uç noktası ile etkileşim kurmak iki yaklaşım Xamarin.Auth destekler:

1. Bir ekli web görünümünü kullanarak. Bu yaygın bir uygulama çalışırken, aşağıdaki nedenlerden dolayı artık önerilir:

    - Barındıran web görünümü uygulaması, kullanıcının tam kimlik doğrulama kimlik bilgileri, yalnızca uygulama için tasarlanmıştır OAuth yetkilendirme verme erişebilirsiniz. Uygulama, olası saldırı yüzeyini uygulamanın artan gerektirdiği çok daha güçlü kimlik bilgilerine eriştiğini gibi bu en az ayrıcalık ilkesini ihlal ediyor.
    - Ana bilgisayar uygulaması kullanıcı adları ve parolalar yakalama, otomatik olarak form gönderme ve kullanıcı onayı, atlama ve oturum tanımlama bilgileri kopyalayın ve bunları kullanıcı olarak kimliği doğrulanmış eylemleri gerçekleştirmek için kullanın.
    - Ekli web görünümleri, diğer uygulama veya cihazın web tarayıcısının gerektiren bir alt kullanıcı deneyimi olarak kabul edilen her Yetkilendirme isteği için oturum açmak kullanıcının, kimlik doğrulama durumu paylaşmayın.
    - Bazı yetkilendirme uç noktaları, algılanmasına ve engellenmesine web görünümleri gelen yetkilendirme isteği için adımlar.

1. Önerilen yaklaşım cihazın web tarayıcısının kullanma. Kullanıcılar yalnızca uygulamada oturum açma ve yetkilendirme akışı dönüştürme oranlarını artırma kimlik sağlayıcı, cihaz başına bir kez oturum açmanız gerekeceğinden, OAuth istekleri için cihaz tarayıcısı kullanarak bir uygulamanın kullanılabilirliğini artırır. Uygulamaları denetleme ve içerik web görünümünde ancak tarayıcıda görüntülenen içerik değiştirme olanağına sahip oluyor gibi cihaz tarayıcısı de Gelişmiş güvenlik sağlar. Bu makalede ve örnek uygulamada gerçekleştirilen yaklaşım budur.

Xamarin.Auth örnek uygulama kullanıcıların kimliğini doğrulama ve temel verilerini almak için nasıl kullandığını bir üst düzey genel bakış, aşağıdaki diyagramda gösterilmiştir:

![](oauth-images/google-auth.png "Google ile kimlik doğrulaması için Xamarin.Auth kullanma")

Uygulamanın Google kullanarak kimlik doğrulama isteği yaptığı `OAuth2Authenticator` sınıfı. Kullanıcı erişim belirteci içeren kendi oturum açma sayfası, Google ile başarıyla doğrulandıktan sonra kimlik doğrulaması yanıtını döndürülür. Uygulama daha sonra Google'da kullanarak, temel kullanıcı verileri için istekte `OAuth2Request` sınıfıyla isteğinde erişim belirteci.

### <a name="setup"></a>Kurulum

Google oturum açma bir Xamarin.Forms uygulamayla tümleştirmek için bir Google API konsol projesi oluşturulması gerekir. Bu şekilde gerçekleştirilebilir:

1. Git [Google API Konsolu](http://console.developers.google.com) Web sitesine gidin ve Google hesabı kimlik bilgilerinizle oturum açın.
1. Proje açılır listeden, mevcut bir projeyi seçin veya yeni bir tane oluşturun.
1. "API Yöneticisi" altında Kenar çubuğunda seçin **kimlik bilgilerini**, ardından **OAuth onay ekranı sekmesini**. Seçin bir **e-posta adresi**, belirtin bir **kullanıcılara gösterilen ürün adı**basın **Kaydet**.
1. İçinde **kimlik bilgilerini** sekmesinde **kimlik bilgileri oluştur** aşağı açılan liste ve seçin **OAuth istemcisi kimliği**.
1. Altında **uygulama türü**, mobil uygulama üzerinde çalışacağı platformunu seçin (**iOS** veya **Android**).
1. Gerekli ayrıntıları girin ve seçin **Oluştur** düğmesi.

> [!NOTE]
> Bir istemci kimliği, uygulamanın etkin Google API'lere erişim sağlar ve tek bir platform için mobil uygulamalar için benzersizdir. Bu nedenle, bir **OAuth istemcisi kimliği** Google oturum açma kullanacak her platform için oluşturulmalıdır.

Bu adımları gerçekleştirdikten sonra Xamarin.Auth Google ile bir OAuth2 kimlik doğrulama akışı başlatmak için kullanılabilir.

### <a name="creating-and-configuring-an-authenticator"></a>Oluşturma ve bir kimlik doğrulayıcı yapılandırma

Xamarin.Auth'ın `OAuth2Authenticator` sınıfı, OAuth kimlik doğrulaması akışı işlenmesinden sorumludur. Aşağıdaki kod örneği örneğinin gösterir `OAuth2Authenticator` cihazın web tarayıcısının kullanarak kimlik doğrulaması yapılırken sınıfı:

```csharp
var authenticator = new OAuth2Authenticator(
    clientId,
    null,
    Constants.Scope,
    new Uri(Constants.AuthorizeUrl),
    new Uri(redirectUri),
    new Uri(Constants.AccessTokenUrl),
    null,
    true);
```

`OAuth2Authenticator` Sınıfı bir dizi gibi olan parametre gerektirir:

- **İstemci kimliği** – bu isteği yapan ve projede alınan istemci tanımlar [Google API Konsolu](http://console.developers.google.com).
- **İstemci gizli anahtarı** – bu olmalıdır `null` veya `string.Empty`.
- **Kapsam** – bu uygulama tarafından istenen API erişimi tanımlar ve kullanıcıya gösterilen onay ekranında değeri bildirir. Kapsamlar hakkında daha fazla bilgi için bkz: [yetkilendirme API isteği](https://developers.google.com/+/web/api/rest/oauth) Google'nın Web sitesinde.
- **URL yetkilendirme** – Bu, burada yetkilendirme kodu öğesinden alınan URL'sini tanımlar.
- **Yeniden yönlendirme URL'si** – Bu, burada yanıta gönderilecek URL'yi tanımlar. Bu parametrenin değeri, görünen değerlerin biriyle eşleşmelidir **kimlik bilgilerini** proje için sekmesinde [Google geliştiriciler konsol](https://console.developers.google.com/).
- **AccessToken Url** – bu bir yetkilendirme kodu alındıktan sonra erişim belirteçlerini istemek için kullanılan URL tanımlar.
- **GetUserNameAsync Func** isteğe bağlı bir `Func` başarıyla doğrulandı sonra hesabın kullanıcı adını zaman uyumsuz olarak almak için kullanılır.
- **Yerel kullanıcı arabirimini kullanarak** – bir `boolean` kimlik doğrulama isteği gerçekleştirmek için cihazın web tarayıcısının kullanılıp kullanılmayacağını belirten değer.

### <a name="setup-authentication-event-handlers"></a>Kimlik doğrulaması olay işleyicilerini ayarlama

Kullanıcı arabirimi için bir olay işleyicisi sunma önce `OAuth2Authenticator.Completed` olay kaydedilmelidir, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Bu olay harekete kullanıcı başarıyla kimliğini doğrulayan veya oturum açma iptal ettiğinde oluşturulur.

İsteğe bağlı olarak, bir olay işleyicisi için `OAuth2Authenticator.Error` olay de kaydedilebilir.

### <a name="presenting-the-sign-in-user-interface"></a>Oturum açma kullanıcı arabirimi sunma

Oturum açma kullanıcı arabirimini kullanarak her platform projesinde başlatılmalıdır Xamarin.Auth oturum açma sunucu kullanıcıya sunulabilir. Aşağıdaki kod örneği, oturum açma sunucu başlatılamadı gösterilmektedir `AppDelegate` iOS projesinde sınıfı:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Aşağıdaki kod örneği, oturum açma sunucu başlatılamadı gösterilmektedir `MainActivity` Android projesi sınıfı:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

.NET Standard kitaplığı projesi ardından aşağıdaki gibi oturum açma sunucu çağırabilirsiniz:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Unutmayın bağımsız değişkeni `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` yöntemi `OAuth2Authenticator` örneği. Zaman `Login` yöntemi çağrıldığında, oturum açma kullanıcı arabirimi aşağıdaki ekran görüntülerinde gösterilen cihazın web tarayıcısında kullanıcıya bir sekmede sunulur:

![](oauth-images/login.png "Google oturum açma")

### <a name="processing-the-redirect-url"></a>Yeniden yönlendirme URL'sini işleme

Kullanıcı kimlik doğrulama işlemi tamamlandıktan sonra Denetim web tarayıcı sekmesini kullanarak uygulamaya döndürür. Bu, kimlik doğrulama işleminde, algılama ve gönderildikten sonra Özel URL işleme öğesinden döndürülen yeniden yönlendirme URL'si için özel bir URL şeması kaydederek sağlanır.

Uygulamayla ilişkilendirilecek bir özel URL şeması seçerken, uygulamaları kendi denetimi altında bir adına dayalı bir düzeni kullanmanız gerekir. Bu, iOS ve android'de paket adı paket tanımlayıcı ad kullanma ve URL şeması yapmalarına ters gerçekleştirilebilir. Ancak, Google gibi bazı kimlik sağlayıcıları, ardından ters ve URL düzeni kullanılan etki alanı adlarını temel istemci tanıtıcıları atayın. Örneğin, Google, bir istemci kimliği oluşturursa `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, URL şeması olacaktır `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Yalnızca tek bir Not `/` Düzen bileşenini sonra görünebilir. Bu nedenle, tam bir örnek özel bir URL düzeni kullanan bir yeniden yönlendirme URL'si olan `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Web tarayıcı özel bir URL düzeni içeren kimlik sağlayıcısından gelen bir yanıtı aldığında, yükleme başarısız olur URL'yi dener. Bunun yerine, özel URL düzeni, bir olay yükselterek işletim sistemine bildirilir. İşletim sistemi için kayıtlı şemaları ardından denetler ve bulunması durumunda, işletim sistemi şeması kayıtlı uygulamayı başlatın ve yeniden yönlendirme URL'sini gönderin.

İşletim sistemi ile özel bir URL şeması kaydetme ve işleme düzeni mekanizması, her platform için özeldir.

#### <a name="ios"></a>iOS

Özel bir URL şeması kaydedilmiştir İos'ta **Info.plist**aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](oauth-images/info-plist.png "URL düzeni kayıt")

**Tanımlayıcı** değeri herhangi bir şey olabilir ve **rol** değer ayarlanmalıdır **Görüntüleyicisi**. **Url şemalarını** ile başlayan değeri `com.googleusercontent.apps`, iOS istemci kimliği için proje üzerinde elde edilebilir [Google API Konsolu](http://console.developers.google.com).

Kimlik sağlayıcısının yetkilendirme isteği tamamlandığında, uygulamanın yeniden yönlendirme URL'sine yönlendirir. URL iOS uygulaması başlatılırken sonuç bir özel düzeni kullandığından, URL'de burada işlendiği tarafından bir başlatma parametre olarak geçirerek `OpenUrl` uygulamanın geçersiz kılma `AppDelegate` aşağıdaki kod örneğinde gösterilen sınıfı:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    // Convert NSUrl to Uri
    var uri = new Uri(url.AbsoluteString);

    // Load redirectUrl page
    AuthenticationState.Authenticator.OnPageLoading(uri);

    return true;
}
```

`OpenUrl` Yöntemi dönüştürür alınan URL'den bir `NSUrl` kullanarak bir .NET `Uri`, yeniden yönlendirme URL'si ile işlenmeden önce `OnPageLoading` bir genel yöntem `OAuth2Authenticator` nesne. Bu, web tarayıcı sekmesini kapatın ve alınan OAuth verileri ayrıştırmak için Xamarin.Auth neden olur.

#### <a name="android"></a>Android

Android'de belirterek özel bir URL şeması kayıtlı bir [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) özniteliği `Activity` düzeni ele alacak. Kimlik sağlayıcısının yetkilendirme isteği tamamlandığında, uygulamanın yeniden yönlendirme URL'sine yönlendirir. Android uygulama başlatılırken sonuç bir özel düzeni URL'yi kullanır gibi URL'de burada işlendiği tarafından bir başlatma parametre olarak geçirerek `OnCreate` yöntemi `Activity` özel URL şeması işlemek için kayıtlı. Aşağıdaki kod örneği, özel bir URL şeması işleyen örnek uygulamadan sınıfı göstermektedir:

```csharp
[Activity(Label = "CustomUrlSchemeInterceptorActivity", NoHistory = true, LaunchMode = LaunchMode.SingleTop )]
[IntentFilter(
    new[] { Intent.ActionView },
    Categories = new [] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataSchemes = new [] { "<insert custom URL here>" },
    DataPath = "/oauth2redirect")]
public class CustomUrlSchemeInterceptorActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        // Convert Android.Net.Url to Uri
        var uri = new Uri(Intent.Data.ToString());

        // Load redirectUrl page
        AuthenticationState.Authenticator.OnPageLoading(uri);

        Finish();
    }
}
```

`DataSchemes` Özelliği [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) Android istemci kimliği için proje üzerinde elde edilen ters istemci tanımlayıcısı ayarlanmalıdır [Google API Konsolu](http://console.developers.google.com).

`OnCreate` Yöntemi dönüştürür alınan URL'den bir `Android.Net.Url` kullanarak bir .NET `Uri`, yeniden yönlendirme URL'si ile işlenmeden önce `OnPageLoading` bir genel yöntem `OAuth2Authenticator` nesne. Bu, web tarayıcı sekmesini kapatın ve alınan OAuth verileri ayrıştırmak Xamarin.Auth neden olur.

> [!IMPORTANT]
> Android'de Xamarin.Auth kullanan `CustomTabs` web tarayıcı ve işletim sistemi ile iletişim kurmak için API. Ancak, bu, garantili bir `CustomTabs` uyumlu bir tarayıcıda, kullanıcının cihazında yüklenir.

### <a name="examining-the-oauth-response"></a>OAuth yanıt İnceleme

Alınan OAuth veri ayrıştırma sonra Xamarin.Auth oluşturacağı `OAuth2Authenticator.Completed` olay. Bu olay için olay işleyicisinde `AuthenticatorCompletedEventArgs.IsAuthenticated` özelliği, aşağıdaki kod örneğinde gösterildiği gibi kimlik doğrulaması başarılı olup olmadığını belirlemek için kullanılabilir:

```csharp
async void OnAuthCompleted(object sender, AuthenticatorCompletedEventArgs e)
{
  ...
  if (e.IsAuthenticated)
  {
    ...
  }
}
```

Başarılı bir kimlik doğrulamasını toplanan verileri kullanılabilir `AuthenticatorCompletedEventArgs.Account` özelliği. Bu kimlik sağlayıcısı tarafından sunulan bir API veri istekleri imzalamak için kullanılan bir erişim belirteci içerir.

### <a name="making-requests-for-data"></a>Veri alma istekleri

Uygulama erişim belirteci aldıktan sonra bir isteğin yapılacağı kullanılır `https://www.googleapis.com/oauth2/v2/userinfo` kimlik sağlayıcısından gelen istek temel kullanıcı verileri için bir API. Xamarin.Auth ile ait bu istekte `OAuth2Request` hizmetinden alınan bir hesap kullanarak kimlik doğrulaması yapan bir istek temsil eden sınıf bir `OAuth2Authenticator` , aşağıdaki kod örneğinde gösterildiği gibi örnek:

```csharp
// UserInfoUrl = https://www.googleapis.com/oauth2/v2/userinfo
var request = new OAuth2Request ("GET", new Uri (Constants.UserInfoUrl), null, e.Account);
var response = await request.GetResponseAsync ();
if (response != null)
{
  string userJson = response.GetResponseText ();
  var user = JsonConvert.DeserializeObject<User> (userJson);
}
```

HTTP yöntemi ve API URL'si yanı sıra `OAuth2Request` örneği de belirtir bir `Account` tarafından belirtilen URL'ye isteğini imzalar ve erişim belirteci içeren örneğini `Constants.UserInfoUrl` özelliği. Kimlik sağlayıcısı, temel kullanıcı verilerini daha sonra erişim belirtecinin geçerli olduğu tanıdığı sağlanan kullanıcı adı ve e-posta adresi gibi bir JSON yanıtı olarak döndürür. JSON yanıtı ardından okuma ve seri durumdan `user` değişkeni.

Daha fazla bilgi için [Google API çağırma](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) Google geliştiriciler portalında.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Depolama ve cihazlarda hesap bilgileri alınıyor

Xamarin.Auth güvenli bir şekilde depolar `Account` bir hesapta nesneleri depolamanızı uygulamalar her zaman yeniden kullanıcıların kimliğini doğrulamak izniniz yok. `AccountStore` Sınıfı hesap bilgilerini depolamak için sorumludur ve iOS, Anahtarlık Hizmetleri tarafından desteklenir ve `KeyStore` Android sınıf.

Aşağıdaki örnekte gösterildiği nasıl kod bir `Account` nesne güvenli bir şekilde kaydedildi:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Kaydedilen hesapları, hesap oluşan bir anahtar kullanarak benzersiz şekilde tanımlanır `Username` özelliği ve getirme hesaplarını hesabı Mağazası'ndan kullanılan dize bir hizmeti kimliği. Varsa bir `Account` daha önce çağırma kaydedilmiş `Save` yöntemi yeniden üzerine yazılır.

`Account` bir hizmet çağrılarak alınabilir için nesneleri `FindAccountsForService` yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` Yöntemi döndürür bir `IEnumerable` koleksiyonunu `Account` nesnelerle eşleşen hesap olarak ayarlanan koleksiyondaki ilk öğe.

## <a name="summary"></a>Özet

Bu makalede Xamarin.Auth Xamarin.Forms uygulamada kimlik doğrulaması işlemini yönetmek için nasıl kullanılacağı açıklanmıştır. Xamarin.Auth sağlar `OAuth2Authenticator` ve `OAuth2Request` Google, Microsoft, Facebook ve Twitter gibi kimlik sağlayıcılarla kullanmak için Xamarin.Forms uygulamaları tarafından kullanılan sınıflar.


## <a name="related-links"></a>İlgili bağlantılar

- [OAuthNativeFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [Yerel uygulamalar için OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Google API'lere OAuth2.0 kullanma](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
