---
title: Azure Active Directory B2C ile kullanıcıların kimliğini doğrulama
description: Azure Active Directory B2C, tüketicilere yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümü ' dir. Bu makalede, Microsoft kimlik doğrulama kitaplığı ve Azure Active Directory B2C mobil uygulamasına tüketici kimlik yönetimini tümleştirmeleri için nasıl kullanılacağını gösterir.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: b6989782c438ec41911cc9317d9f911d6518132d
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38872707"
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile kullanıcıların kimliğini doğrulama

_Azure Active Directory B2C, tüketicilere yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümü ' dir. Bu makalede, Microsoft kimlik doğrulama kitaplığı ve Azure Active Directory B2C mobil uygulamasına tüketici kimlik yönetimini tümleştirmeleri için nasıl kullanılacağını gösterir._

![](~/media/shared/preview.png "Bu API, şu anda yayın öncesi")

> [!NOTE]
> [Microsoft kimlik doğrulama Kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client) hala Önizleme aşamasındadır ancak bir üretim ortamında kullanım için uygundur. Ancak, var. API, dahili önbellek biçimini ve uygulamanızı etkileyebilecek kitaplığın başka mekanizmalar için bozucu değişiklikler.

## <a name="overview"></a>Genel Bakış

Azure Active Directory B2C, tüketicilerin uygulamanıza tarafından oturum sağlayan bir kimlik yönetim hizmeti tüketiciye yönelik uygulamalar için verilmiştir:

- İster mevcut sosyal hesaplarını (Microsoft, Google, Facebook, Amazon, LinkedIn) kullanarak.
- Yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturma. Bu kimlik bilgileri olarak ifade edilir *yerel* hesaplar.

Azure Active Directory B2C Kimlik Yönetimi Hizmeti mobil uygulamasına tümleştirmek için işlem aşağıdaki gibidir:

1. Azure Active Directory B2C kiracısı oluşturun. Daha fazla bilgi için [Azure portalında bir Azure Active Directory B2C kiracısı oluşturmayı](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Mobil uygulamanızı Azure Active Directory B2C kiracısına kaydetmeniz. Kayıt işlemine atar bir **uygulama kimliği** uygulamanıza benzersiz olarak tanımlayan ve **tekrar yönlendirme URL'sini** yanıtları uygulamanıza geri yönlendirmek için kullanılabilir. Daha fazla bilgi için [Azure Active Directory B2C: uygulamanızı kaydetme](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Kaydolma ve oturum açma ilkesi oluşturun. Bu ilke, tüketicilerin kaydolma ve oturum açma sırasında karşılaşacağı deneyimleri tanımlar ve ayrıca uygulamanın alacağı belirteçlerin içeriğini başarıyla belirtir kaydolma veya oturum açma. Daha fazla bilgi için [Azure Active Directory B2C: yerleşik ilkeleri](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Kullanım [Microsoft kimlik doğrulama Kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) mobil uygulamanızda Azure Active Directory B2C kiracınızı bir kimlik doğrulama iş akışı başlatın.

> [!NOTE]
> Azure Active Directory B2C Kimlik Yönetimi, mobil uygulamalarda tümleştirme yanı sıra MSAL ayrıca mobil uygulamalarınızı Azure Active Directory kimlik yönetimini tümleştirmeleri için kullanılabilir. Bu Azure Active Directory ile mobil uygulama kaydederek gerçekleştirilebilir [uygulama kayıt portalı](https://apps.dev.microsoft.com/). Kayıt işlemine atar bir **uygulama kimliği** MSAL kullanırken belirtilmelidir uygulamanızı benzersiz şekilde tanımlar. Daha fazla bilgi için [v2.0 uç noktası ile bir uygulamayı kaydetme](/azure/active-directory/develop/active-directory-v2-app-registration/), ve [kimlik doğrulaması uygulamanızın Mobile Apps kullanarak Microsoft kimlik doğrulama Kitaplığı](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) Xamarin blogundan.

MSAL, kimlik doğrulaması gerçekleştirmek için cihazın web tarayıcısının kullanır. Kullanıcılar yalnızca akış uygulamada oturum açma ve kimlik doğrulama, dönüşüm oranlarını artırma cihaz bir kez oturum açmanız gerekeceğinden, bu uygulamanın kullanılabilirliğini artırır. Cihaz tarayıcısı de Gelişmiş güvenlik sağlar. Kullanıcı kimlik doğrulama işlemi tamamlandıktan sonra Denetim web tarayıcı sekmesini kullanarak uygulamaya döndürür. Bu, kimlik doğrulama işleminde, algılama ve gönderildikten sonra Özel URL işleme öğesinden döndürülen yeniden yönlendirme URL'si için özel bir URL şeması kaydederek sağlanır. Özel bir URL düzeni seçme hakkında daha fazla bilgi için bkz. [yerel uygulama yeniden yönlendirme URI'si seçme](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> İşletim sistemi ile özel bir URL şeması kaydetme ve işleme düzeni mekanizması, her platform için özeldir.

Azure Active Directory B2C kiracısı için gönderilen her isteği belirtir bir *ilke*. İlkeleri, tüketici kimlik deneyimi kaydolma veya oturum açma gibi açıklar. Örneğin, aşağıdaki ayarları yapılandırılması için Azure Active Directory B2C kiracısı davranışını bir kaydolma İlkesi sağlar:

- Hesap türleri Tüketiciler, uygulamaya oturum açmak için kullanabilirsiniz.
- Kayıt sırasında tüketiciden toplanacak verileri.
- Çok faktörlü kimlik doğrulaması.
- Kaydolma sayfası içeriği.
- İlke çalıştırıldığında, mobil uygulamanın aldığı belirteci talepler.

Azure Active Directory kiracısı gerektiği gibi uygulamanızda kullanılabilen farklı türlerde birden çok ilke içerebilir. Ayrıca, ilkeleri tanımlayın ve kodunuzu değiştirmeden tüketici kimlik deneyimi, uygulamalar arasında yeniden kullanılabilir. İlkeleri hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C: yerleşik ilkeleri](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Kurulum

Microsoft Authentication Library (MSAL) NuGet kitaplığı Xamarin.Forms çözümü platformu projelerinde ve taşınabilir sınıf kitaplığı (PCL) proje eklenmelidir. Aşağıdaki bölümlerde, mobil uygulamasından bir Azure Active Directory B2C kiracısı ile iletişim kurmak için MSAL kullanarak ek kurulum yönergeleri sağlar.

### <a name="portable-class-library"></a>Taşınabilir Sınıf Kitaplığı

MSAL tüketen PCLs Profile7 kullanmak için yeniden hedeflenmesi gerekir. PCLs hakkında daha fazla bilgi için bkz: [taşınabilir sınıf kitaplıkları giriş](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

İOS, Azure Active Directory B2C ile kaydedilen özel URL şeması kayıtlı olmalıdır **Info.plist**aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](azure-ad-b2c-images/customurl-ios.png "İOS özel bir URL şeması kaydediliyor")

Azure Active Directory B2C yetkilendirme isteği tamamlandığında, kayıtlı yeniden yönlendirme URL'sine yönlendirir. URL iOS mobil uygulama başlatılırken sonuç bir özel düzeni kullandığından, URL'de burada işlendiği tarafından bir başlatma parametre olarak geçirerek `OpenUrl` uygulamanın geçersiz kılma `AppDelegate` aşağıdaki kodda gösterilen sınıfı Örnek:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return true;
        }
    }
}
```

Kodda `OpenURL` yöntemi, kimlik doğrulama iş akışı etkileşimli kısmı sona erdikten sonra denetim için MSAL döndürür sağlar.

### <a name="android"></a>Android

Android, Azure Active Directory B2C ile kaydedilen özel URL şeması kayıtlı olmalıdır **AndroidManifest.xml**, ekleyerek bir `<activity>` içinde var olan öğe `<application>` öğesi. `<activity>` Öğesi belirtir `IntentFilter` üzerinde `Activity` düzenini işler ve aşağıdaki örnekte gösterilmiştir:

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

Azure Active Directory B2C yetkilendirme isteği tamamlandığında, kayıtlı yeniden yönlendirme URL'sine yönlendirir. URL Android mobil uygulama başlatılırken sonuç bir özel düzeni kullandığından, URL'de burada işlendiği tarafından bir başlatma parametre olarak geçirerek `microsoft.identity.client.BrowserTabActivity`. Unutmayın `data android:scheme` özelliği, Azure Active Directory B2C uygulama ile kayıtlı özel bir URL şeması için ayarlanmalıdır.

Ayrıca, `MainActivity` sınıfı değiştirilmelidir, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

`OnCreate` Yöntemi atayarak değiştirildiğinde bir `UIParent` için örnek `App.UiParent` özelliği. Bu, kimlik doğrulaması akışı geçerli etkinliği bağlamında gerçekleştirilmesini sağlar.

Kodda `OnActivityResult` yöntemi, kimlik doğrulama iş akışı etkileşimli kısmı sona erdikten sonra denetim için MSAL döndürür sağlar.

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Evrensel Windows platformu, ek kurulum MSAL kullanmak için gereklidir.

## <a name="initialization"></a>Başlatma

Microsoft kimlik doğrulama kitaplığı üyeleri kullanan `PublicClientApplication` bir kimlik doğrulama iş akışı başlatmak için sınıf. Örnek uygulamayı bildiren ve başlatan bir `public` adlı, bu tür özelliği `ADB2CClient`, `AuthenticationProvider` sınıfı. Aşağıdaki kod örneği, bu özelliği nasıl başlatılır gösterir:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Mobil uygulamayı Azure Active Directory B2C kiracısı ile kaydettiğinizde, kayıt işlemini atanmış bir **uygulama kimliği**. Bu kimliği belirtilmelidir `PublicClientApplication` Oluşturucusu ile birlikte bir `Authority` temel URL ile Azure Active Directory B2C İlkesi yürütülecek oluşur sabiti.

## <a name="signing-in"></a>Oturum açma

Örnek uygulamada oturum açma ekranı aşağıdaki ekran görüntülerinde gösterilmektedir:

![](azure-ad-b2c-images/login.png "Oturum açma sayfası")

Oturum açma ile yerel bir hesap veya sosyal kimlik sağlayıcıları, izin verilir. Microsoft, Google ve Facebook, yukarıda gösterildiği gibi sosyal kimlik sağlayıcıları olarak kullanılır ancak diğer kimlik sağlayıcılardan de kullanılabilir.

Aşağıdaki kod örneği, oturum açma işlemini nasıl çağrılan gösterir:

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

`AcquireTokenAsync` Yöntemi cihazın web tarayıcısının başlatır ve kimlik doğrulama seçenekleri aracılığıyla başvurulan ilke tarafından belirtilen Azure Active Directory B2C İlkesi'nde tanımlanan görüntüler `Constants.Authority` sabit. Bu ilke, tüketicilerin kaydolma sırasında ve oturum açma karşılaşacağı deneyimleri ve uygulamanın üzerinde başarılı kaydolma veya oturum açma alacağı talepleri tanımlar.

Sonucu `AcquireTokenAsync` yöntem çağrısında bir `AuthenticationResult` örneği. Kimlik doğrulaması başarılı olursa `AuthenticationResult` örneği yerel olarak önbelleğe alınabilir bir kimlik belirteci içerir. Kimlik doğrulaması başarısız olursa `AuthenticationResult` örnek kimlik doğrulama başarısız olmasının gösteren veri içeriyor.

Örnek uygulamada kimlik doğrulaması başarılı olursa `TodoList` sayfası için gitme.

## <a name="silent-re-authentication"></a>Sessiz yeniden kimlik doğrulaması

Zaman `LoginPage` örnek uygulaması görünür, herhangi bir kimlik doğrulama kullanıcı arabirimini gösteren olmadan bir kullanıcı belirteci almak için bir deneme yapılır. Bu ile elde edilir `AcquireTokenSilentAsync` yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

`AcquireTokenSilentAsync` Yöntemi kullanıcının oturum açma gerek kalmadan önbellekten kullanıcı belirteci almayı dener. Bu senaryoyu burada uygun bir belirteç zaten önceki oturumdan önbellekte bulunabilecek işler. Bir belirteç almak için girişim başarılı olursa `TodoList` sayfası için gitme. Belirteç edinme girişimi başarısız olursa, hiçbir şey olmaz ve kullanıcının yeni bir kimlik doğrulama iş akışı başlatma seçeneğine sahip olur.

## <a name="signing-out"></a>Oturum kapatılıyor

Aşağıdaki kod örneği, oturum kapatma işlemini nasıl çağrılan gösterir:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Bu, tüm kimlik doğrulama belirteçlerinizi yerel önbellekten kaldırır.

## <a name="summary"></a>Özet

Bu makalede Microsoft Authentication Library (MSAL) ve Azure Active Directory B2C mobil uygulamasına tüketici kimlik yönetimini tümleştirmeleri için nasıl kullanılacağı gösterilmektedir. Azure Active Directory B2C, tüketicilere yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümü ' dir.


## <a name="related-links"></a>İlgili bağlantılar

- [AzureADB2CAuth (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft kimlik doğrulama kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client)
