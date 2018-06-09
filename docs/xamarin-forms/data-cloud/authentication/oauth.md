---
title: Bir kimlik sağlayıcısı ile kullanıcıların kimlik doğrulaması
description: Bu makalede Xamarin.Auth bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 361b5e5583b10b7ea07abd1460350d6445cae1c2
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241266"
---
# <a name="authenticating-users-with-an-identity-provider"></a>Bir kimlik sağlayıcısı ile kullanıcıların kimlik doğrulaması

_Xamarin.Auth platformlar arası SDK kullanıcıların kimlik doğrulaması ve hesaplarını depolamak içindir. Google, Microsoft, Facebook ve Twitter gibi kimlik sağlayıcıları kullanma için destek sağlayan OAuth kimlik doğrulayan içerir. Bu makalede Xamarin.Auth bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmaktadır._

OAuth kimlik doğrulaması için açık bir standart ve kaynak sahipleri kimlik paylaşmadan kendi bilgilerine erişmek için bir üçüncü tarafa izin verilmelidir bir kaynak sağlayıcısı bildirmek bir kaynak sahibi etkinleştirir. Buna örnek olarak kullanıcının kimliğini paylaşmadan kendi verilere erişmek için bir uygulama için izin verilmelidir (örneğin, Google, Microsoft, Facebook veya Twitter'da) bir kimlik sağlayıcısı bildirmek bir kullanıcı etkinleştirilmesi. Genellikle bir yaklaşım için kullanıcıların oturum Web siteleri ve uygulamaları bir kimlik sağlayıcısı kullanarak, ancak Web sitesi veya uygulama parolalarını gösterme olmadan açmak için kullanılıyor.

Yüksek düzeyde genel bakış bir OAuth kimlik sağlayıcısı kullanırken kimlik doğrulaması akışı aşağıdaki gibidir:

1. Uygulama bir kimlik sağlayıcısı URL'sini bir tarayıcı gider.
1. Kimlik sağlayıcısı, kullanıcı kimlik doğrulaması gerçekleştirir ve uygulamayı bir yetkilendirme kodu döndürür.
1. Uygulama kimlik sağlayıcısı'ndan bir erişim belirteci yetkilendirme kodu değiş tokuş eder.
1. Uygulama erişim belirtecini API'leri gibi temel kullanıcı verileri isteyen bir API kimlik sağlayıcısı'na erişmek için kullanır.

Örnek uygulama Xamarin.Auth yerel kimlik doğrulaması akışı Google karşı uygulamak için nasıl kullanılacağını gösterir. Bu konuda kimlik sağlayıcısı Google kullanılırken, diğer kimlik sağlayıcılardan eşit geçerli bir yaklaşımdır. Google OAuth 2.0 endpoint kullanarak kimlik doğrulaması hakkında daha fazla bilgi için bkz: [erişim Google API'leri kullanarak OAuth2.0](https://developers.google.com/identity/protocols/OAuth2) Google Web sitesinde.

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Kullanıcıların kimliğini doğrulamak için Xamarin.Auth kullanma

Xamarin.Auth bir kimlik sağlayıcısının yetkilendirme uç noktası ile etkileşim kurmak üzere uygulamalar için iki yaklaşım destekler:

1. Katıştırılmış web görünümü kullanarak. Bu ortak bir uygulama çalışırken, aşağıdaki nedenlerden dolayı artık önerilir:

    - Web görünümü barındıran uygulama, kullanıcının tam kimlik doğrulama kimlik bilgileri, yalnızca uygulama için hedeflenen OAuth yetkilendirme verme erişebilir. Bu, olası saldırı yüzeyini uygulamanın artan gerektirdiğinden daha daha güçlü kimlik bilgileri uygulama erişimi gibi bu en az ayrıcalık ilkesini ihlal ediyor.
    - Ana bilgisayar uygulamasını kullanıcı adları ve parolalar yakalama, otomatik olarak form gönderme ve kullanıcı izni atlama ve oturum tanımlama bilgileri kopyalayın ve bunları kullanıcı olarak kimliği doğrulanmış eylemleri gerçekleştirmek için kullanın.
    - Katıştırılmış web görünümleri, diğer uygulamalar veya cihazın web tarayıcısının ast kullanıcı deneyimi olarak kabul edilen her Yetkilendirme isteği için oturum açmak kullanıcının gerektiren, kimlik doğrulama durumuna paylaşmayın.
    - Bazı yetkilendirme uç noktaları algılamak ve web görünümleri gelen yetkilendirme isteklerini engellemek için adımları uygulayın.

1. Önerilen yaklaşım cihazın web tarayıcısının kullanma. Oturum dönüştürme oranları uygulamada oturum açma ve yetkilendirme akışı geliştirme kimlik sağlayıcısı her aygıt için bir kez açmak kullanıcılara yalnızca gerektiği için OAuth istek aygıt tarayıcı kullanarak bir uygulamanın kullanılabilirliğini artırır. Uygulamaları inceleyin ve bir web görünümü içeriğinde, ancak tarayıcı'da gösterilen içeriği değiştirmek mümkün olduğu gibi cihaz tarayıcı de Gelişmiş güvenlik sağlar. Bu makale ve örnek uygulamasında uygulanan yaklaşıma budur.

Xamarin.Auth örnek uygulama kullanıcıların kimliğini doğrulamak ve temel verilerini almak için nasıl kullandığını bir üst düzey genel bakış, aşağıdaki çizimde gösterilmiştir:

![](oauth-images/google-auth.png "Google ile kimlik doğrulaması için Xamarin.Auth kullanma")

Google kullanarak kimlik doğrulama isteği uygulamanın yaptığı `OAuth2Authenticator` sınıfı. Kullanıcı başarılı bir şekilde bir erişim belirteci içeren kendi oturum açma sayfası, Google ile doğrulamadan sonra bir kimlik doğrulaması yanıt döndürdü. Uygulama için Google kullanarak temel kullanıcı verileri için istekte `OAuth2Request` sınıfıyla isteğinde erişim belirteci.

### <a name="setup"></a>Kurulum

Google oturum açma bir Xamarin.Forms uygulaması ile tümleştirmek için bir Google API Konsolu proje oluşturulması gerekir. Bu şekilde gerçekleştirilebilir:

1. Git [Google API Konsolu](http://console.developers.google.com) Web sitesi ve Google hesabı kimlik bilgileriyle oturum açın.
1. Proje açılır, varolan projesini seçin veya yeni bir tane oluşturun.
1. "API Yöneticisi" altında Kenar çubuğunda seçin **kimlik bilgileri**seçeneğini belirleyip **OAuth izni ekran sekmesini**. Seçin bir **e-posta adresi**, belirtin bir **kullanıcılara gösterilen ürün adı**ve basın **kaydetmek**.
1. İçinde **kimlik bilgileri** sekmesine **kimlik bilgileri oluşturma** aşağı açılan listesinde ve seçin **OAuth istemci kimliği**.
1. Altında **uygulama türü**, mobil uygulamanın çalışacağı platformu seçin (**iOS** veya **Android**).
1. Gerekli ayrıntıları doldurun ve seçin **oluşturma** düğmesi.

> [!NOTE]
> Bir istemci kimliği, uygulamanın etkin Google API'leri erişim sağlar ve mobil uygulamalar için tek bir platform için benzersizdir. Bu nedenle, bir **OAuth istemci kimliği** Google oturum açma kullanacak her platform için oluşturulmalıdır.

Bu adımları gerçekleştirdikten sonra Xamarin.Auth bir OAuth2 kimlik doğrulama akışı Google ile başlatmak için kullanılabilir.

### <a name="creating-and-configuring-an-authenticator"></a>Oluşturma ve bir kimlik doğrulayıcı yapılandırma

Xamarin.Auth'ın `OAuth2Authenticator` sınıftır OAuth kimlik doğrulaması akışı işlenmesinden sorumludur. Aşağıdaki kod örneği, örnek oluşturma gösterir `OAuth2Authenticator` sınıfı cihazın web tarayıcısının kullanarak kimlik doğrulaması gerçekleştirirken:

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

`OAuth2Authenticator` Sınıf gibidir parametreleri gerektirir:

- **İstemci kimliği** – bu isteği yapan ve projede alınan istemci tanımlar [Google API Konsolu](http://console.developers.google.com).
- **İstemci parolası** – bu olmalıdır `null` veya `string.Empty`.
- **Kapsam** – bu uygulama tarafından istenen API erişimini tanımlar ve değer kullanıcıya gösterilen izin ekran size bildirir. Kapsamları hakkında daha fazla bilgi için bkz: [yetkilendirme API isteği](https://developers.google.com/+/web/api/rest/oauth) Google Web sitesinde.
- **URL yetkilendirme** – Bu, yetkilendirme kodu öğesinden elde edilebilir URL tanımlar.
- **Yeniden yönlendirme URL'si** – bu burada yanıtın gönderileceği URL tanımlar. Bu parametrenin değeri görünür değerlerden biri eşleşmelidir **kimlik bilgileri** projede sekmesi [Google geliştiriciler konsol](https://console.developers.google.com/).
- **AccessToken Url** – Bu, bir kimlik doğrulama kodu alındıktan sonra erişim belirteçleri istemek için kullanılan URL tanımlar.
- **GetUserNameAsync Func** – isteğe bağlı bir `Func` başarıyla yetkilendirilmiş sonra hesabın kullanıcı adını zaman uyumsuz olarak almak için kullanılır.
- **Yerel kullanıcı arabirimini kullanma** – bir `boolean` kimlik doğrulama isteği gerçekleştirmek için cihazın web tarayıcısının kullanılıp kullanılmayacağını belirten değer.

### <a name="setup-authentication-event-handlers"></a>Kimlik doğrulaması olay işleyicileri Kurulum

Kullanıcı arabirimi, olay işleyicisi için sunmadan önce `OAuth2Authenticator.Completed` olay kaydedilmelidir, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Kullanıcı başarıyla doğruladığında veya oturum açma iptal eder, bu olayı ateşlenir.

İsteğe bağlı olarak, bir olay işleyicisi için `OAuth2Authenticator.Error` olay da kaydedilebilir.

### <a name="presenting-the-sign-in-user-interface"></a>Oturum açma kullanıcı arabirimi sunma

Oturum açma kullanıcı arabirimi, her platform projesinde başlatılmalıdır Xamarin.Auth oturum açma sunum kullanarak kullanıcıya sunulabilir. Aşağıdaki kod örneği, bir oturum açma sunum başlatmak gösterilmiştir `AppDelegate` iOS projesi sınıfında:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Aşağıdaki kod örneği, bir oturum açma sunum başlatmak gösterilmiştir `MainActivity` Android projesi sınıfında:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Taşınabilir sınıf kitaplığı (PCL) proje sonra aşağıdaki gibi oturum açma sunum çağırabilirsiniz:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Unutmayın bağımsız değişkeni `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` yöntemi `OAuth2Authenticator` örneği. Zaman `Login` yöntemi çağrıldığında, oturum açma kullanıcı arabirimi aşağıdaki ekran görüntülerinde gösterilen cihazın web tarayıcısının gelen bir sekmede kullanıcıya sunulur:

![](oauth-images/login.png "Google oturum açma")

### <a name="processing-the-redirect-url"></a>Yeniden yönlendirme URL'sini işleme

Kullanıcı kimlik doğrulama işlemi tamamlandıktan sonra web tarayıcısı sekmesinden denetim uygulamaya döndürür. Bu kimlik doğrulama işlemi, algılama ve gönderildikten sonra Özel URL işleme öğesinden döndürülen yeniden yönlendirme URL'si için özel bir URL şeması kaydederek sağlanır.

Bir uygulama ile ilişkilendirmek için özel bir URL şeması seçerken, uygulamaları kendi denetimindeki adına dayalı bir şeması kullanması gerekir. Bu, iOS ve android'de paket adı paket tanımlayıcı adı kullanılarak ve URL şeması yapmalarına ters yararlanılabilir. Ancak, Google gibi bazı kimlik sağlayıcıları sonra ters ve URL şeması kullanılan etki alanı adlarını temel istemci tanımlayıcıları atayın. Örneğin, Google bir istemci kimliği oluşturursa `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, URL şemasının olacaktır `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Bu yalnızca tek bir Not `/` düzeni bileşen sonra ortaya çıkabilir. Bu nedenle, özel bir URL şemasını kullanan bir yönlendirme URL'si, tam bir örnek olduğundan `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Web tarayıcısı özel bir URL şeması içerir kimlik sağlayıcısından gelen yanıt aldığında başarısız olur URL'yi yüklemeye çalışır. Bunun yerine, özel URL şeması, bir olay yükselterek işletim sistemine bildirilir. İşletim sistemi için kayıtlı şemaları sonra denetler ve bulunması durumunda işletim sistemi düzeni kayıtlı uygulamayı başlatın ve yeniden yönlendirme URL'sini göndermek.

İşletim sistemi ile özel bir URL şeması kaydetme ve işleme düzeni için mekanizma, her platform için özeldir.

#### <a name="ios"></a>iOS

Özel bir URL şeması kaydedilir, iOS **Info.plist**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](oauth-images/info-plist.png "URL şeması kayıt")

**Tanımlayıcısı** değeri herhangi bir şey olabilir ve **rol** değeri ayarlanmalıdır **Görüntüleyicisi**. **Url şemalarını** ile başlayan değeri `com.googleusercontent.apps`, proje için iOS istemci kimliği üzerinde elde edilebilir [Google API Konsolu](http://console.developers.google.com).

Kimlik sağlayıcısı yetkilendirme isteği tamamlandığında, uygulamanın yeniden yönlendirme URL'sine yönlendirir. URL Uygulama başlatılmadan iOS sonuçları özel bir şema kullandığından, URL'de işleneceği tarafından bir başlatma parametre olarak geçirme `OpenUrl` uygulamanın geçersiz kılma `AppDelegate` aşağıdaki kod örneğinde gösterildiği sınıfı:

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

`OpenUrl` Yöntemi dönüştürür alınan URL'den bir `NSUrl` bir .NET için `Uri`, yeniden yönlendirme URL'si ile işlenmeden önce `OnPageLoading` ortak bir yöntemi `OAuth2Authenticator` nesnesi. Bu, bir web tarayıcısı sekmesi kapatmak ve alınan OAuth verileri ayrıştırmak için Xamarin.Auth neden olur.

#### <a name="android"></a>Android

Android, özel bir URL şeması belirterek kayıtlı bir [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) özniteliği `Activity` düzeni işleyecek. Kimlik sağlayıcısı yetkilendirme isteği tamamlandığında, uygulamanın yeniden yönlendirme URL'sine yönlendirir. Uygulama başlatılmadan Android sonuçları özel bir şema URL'yi kullanır gibi URL'de işleneceği tarafından bir başlatma parametre olarak geçirme `OnCreate` yöntemi `Activity` özel URL şeması işlemek üzere kaydettirilmiş. Aşağıdaki kod örneğinde özel URL şeması işleme örnek uygulama sınıfından gösterir:

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

`DataSchemes` Özelliği [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) projeyi Android istemci kimliği üzerinde alınır ters istemci tanımlayıcısı ayarlanmalıdır [Google API Konsolu](http://console.developers.google.com).

`OnCreate` Yöntemi dönüştürür alınan URL'den bir `Android.Net.Url` bir .NET için `Uri`, yeniden yönlendirme URL'si ile işlenmeden önce `OnPageLoading` ortak bir yöntemi `OAuth2Authenticator` nesnesi. Bu, bir web tarayıcısı sekmesi kapatmak ve alınan OAuth verileri ayrıştırmak Xamarin.Auth neden olur.

> [!IMPORTANT]
> Android, Xamarin.Auth kullanan `CustomTabs` işletim sistemi ve web tarayıcısı ile iletişim kurmak için API. Ancak, bunu, garanti olmayan bir `CustomTabs` uyumlu tarayıcı kullanıcının aygıtına yüklenecek.

### <a name="examining-the-oauth-response"></a>OAuth yanıt inceleniyor

Alınan OAuth veri ayrıştırmadan sonra Xamarin.Auth oluşturacağı `OAuth2Authenticator.Completed` olay. Bu olay için olay işleyicisini içinde `AuthenticatorCompletedEventArgs.IsAuthenticated` özelliği, aşağıdaki kod örneğinde gösterildiği gibi kimlik doğrulama başarılı olup olmadığını belirlemek için kullanılabilir:

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

Başarılı bir kimlik doğrulamasını toplanan verileri kullanılabilir `AuthenticatorCompletedEventArgs.Account` özelliği. Bu, kimlik sağlayıcısı tarafından sağlanan bir API veri istekleri imzalamak için kullanılan bir erişim belirteci içerir.

### <a name="making-requests-for-data"></a>Veri alma istekleri

Uygulama bir erişim belirteci aldıktan sonra bir isteğin yapılacağı kullanılır `https://www.googleapis.com/oauth2/v2/userinfo` API'sine kimlik sağlayıcısından gelen istek temel kullanıcı verileri. Xamarin.Auth ile kullanıcının bu istekte `OAuth2Request` kaynağından alınan bir hesabı kullanarak kimlik doğrulaması yapan bir istek temsil eden sınıf bir `OAuth2Authenticator` , aşağıdaki kod örneğinde gösterildiği gibi örneği:

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

HTTP yöntemi ve API URL yanı sıra `OAuth2Request` örneği ayrıca belirtir bir `Account` tarafından belirtilen URL'ye isteğini imzalar erişim belirteci içeren örneği `Constants.UserInfoUrl` özelliği. Kimlik sağlayıcısı, temel kullanıcı verilerini sonra geçerli olduğu erişim belirteci tanıdığı koşuluyla, kullanıcı adı ve e-posta adresi de dahil olmak üzere bir JSON yanıt olarak döndürür. JSON yanıt dosyayı okuyabilir ve içine seri durumdan `user` değişkeni.

Daha fazla bilgi için bkz: [bir Google API çağırma](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) Google geliştiriciler Portal.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Depolama ve cihazlarda hesap bilgileri alınıyor

Xamarin.Auth güvenli bir şekilde depolar `Account` bir hesap nesneleri depolamanızı uygulamalar her zaman kullanıcı yeniden kimlik doğrulaması gerekmez. `AccountStore` Sınıfı hesap bilgilerini depolamak için sorumludur ve iOS, Anahtarlık Hizmetleri tarafından yedeklenir ve `KeyStore` Android sınıfında.

Aşağıdaki örnekte gösterildiği nasıl kod bir `Account` nesne güvenli bir şekilde kaydedildi:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Kaydedilen hesapları, hesabın oluşan bir anahtar kullanarak benzersiz şekilde tanımlanır `Username` özelliği ve getirme hesaplarını, hesap deposundan kullanılan bir dize bir hizmet kimliği. Varsa bir `Account` daha önce çağırma kaydedilmiş `Save` yöntemi yeniden üzerine yazılacak onu.

`Account` bir hizmet çağırarak alınabilir için nesneleri `FindAccountsForService` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` Yöntemi döndürür bir `IEnumerable` koleksiyonu `Account` nesneleriyle eşleşen hesabı olarak ayarlanan koleksiyondaki ilk öğe.

## <a name="summary"></a>Özet

Bu makalede Xamarin.Auth bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmıştır. Xamarin.Auth sağlar `OAuth2Authenticator` ve `OAuth2Request` , Google, Microsoft, Facebook ve Twitter gibi kimlik sağlayıcıları tüketmeye Xamarin.Forms uygulamalar tarafından kullanılan sınıfları.


## <a name="related-links"></a>İlgili bağlantılar

- [OAuthNativeFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [Yerel uygulamalar için OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Google API'leri erişmek için OAuth2.0 kullanma](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
