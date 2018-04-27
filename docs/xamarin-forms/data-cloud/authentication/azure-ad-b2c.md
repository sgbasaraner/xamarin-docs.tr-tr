---
title: Kullanıcıların Azure Active Directory B2C ile kimlik doğrulaması
description: Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür. Bu makalede Microsoft kimlik doğrulama kitaplığı ve Azure Active Directory B2C tüketici kimlik yönetimini mobil uygulamasına tümleştirmek için nasıl kullanılacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 627c6773c099c9cf45f871a9bb73a201bf98271a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Kullanıcıların Azure Active Directory B2C ile kimlik doğrulaması

_Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür. Bu makalede Microsoft kimlik doğrulama kitaplığı ve Azure Active Directory B2C tüketici kimlik yönetimini mobil uygulamasına tümleştirmek için nasıl kullanılacağı gösterilmektedir._

![](~/media/shared/preview.png "Bu API şu anda yayın öncesi")

> [!NOTE]
> [Microsoft kimlik doğrulama Kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client) hala önizlemede değil, ancak bir üretim ortamında kullanım için uygundur. Ancak, var. API, dahili önbellek biçimini ve uygulamanızı etkileyebilir kitaplığın başka mekanizmalar önemli değişiklikler.

## <a name="overview"></a>Genel Bakış

Azure Active Directory B2C tüketicilerin oturum tarafından uygulamanıza açma sağlayan bir kimlik yönetimi hizmeti tüketiciye yönelik uygulamalar için şöyledir:

- Var olan sosyal hesaplarını (Microsoft, Google, Facebook, Amazon, LinkedIn) kullanarak.
- Yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturma. Bu kimlik bilgileri olarak adlandırılır *yerel* hesaplar.

Azure Active Directory B2C Identity management hizmeti bir mobil uygulamayla tümleştirmek için işlem aşağıdaki gibidir:

1. Azure Active Directory B2C kiracısı oluşturun. Daha fazla bilgi için bkz: [Azure portalda bir Azure Active Directory B2C kiracısı oluşturma](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Mobil uygulamanız Azure Active Directory B2C Kiracı ile kaydedin. Kayıt işlemine atar bir **uygulama kimliği** , uygulamanızın benzersiz olarak tanımlayan ve **tekrar yönlendirme URL'sini** yanıtları uygulamanıza geri yönlendirmek için kullanılabilir. Daha fazla bilgi için bkz: [Azure Active Directory B2C: uygulamanızı kaydetme](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Kaydolma ve oturum açma bir ilke oluşturun. Bu ilke tüketicileri kaydolma ve oturum açma sırasında geçer deneyimleri tanımlar ve ayrıca uygulama alacağı belirteçleri içeriğini başarıyla belirtir kaydolma veya oturum açma. Daha fazla bilgi için bkz: [Azure Active Directory B2C: yerleşik ilkeleri](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Kullanım [Microsoft kimlik doğrulama Kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) ile Azure Active Directory B2C kiracınızın bir kimlik doğrulama iş akışını başlatmak için mobil uygulamanızda.

> [!NOTE]
> Azure Active Directory B2C Kimlik Yönetimi mobil uygulamalarla tümleştirmek yanı sıra MSAL de Azure Active Directory kimlik yönetimi mobil uygulamalara tümleştirmek için kullanılabilir. Bu Azure Active Directory ile bir mobil uygulama kaydederek gerçekleştirilebilir [uygulama kayıt portalı](https://apps.dev.microsoft.com/). Kayıt işlemine atar bir **uygulama kimliği** MSAL kullanırken belirtilmelidir uygulamanızı benzersiz olarak tanımlar. Daha fazla bilgi için bkz: [v2.0 uç noktası ile bir uygulama nasıl](/azure/active-directory/develop/active-directory-v2-app-registration/), ve [kimlik doğrulaması bilgisayarınızı Mobile Apps kullanarak Microsoft kimlik doğrulama Kitaplığı](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) Xamarin blogunda.

MSAL cihazın web tarayıcısının kimlik doğrulaması yapmak için kullanır. Bu, oturum cihaz başına dönüştürme oranları oturum açma ve yetkilendirme geliştirme uygulamada akar sonra açmak kullanıcılara yalnızca gerektiği bir uygulamanın kullanılabilirliğini artırır. Aygıt tarayıcı de Gelişmiş güvenlik sağlar. Kullanıcı kimlik doğrulama işlemi tamamlandıktan sonra web tarayıcısı sekmesinden denetim uygulamaya döndürür. Bu kimlik doğrulama işlemi, algılama ve gönderildikten sonra Özel URL işleme öğesinden döndürülen yeniden yönlendirme URL'si için özel bir URL şeması kaydederek sağlanır. Özel bir URL şeması seçme hakkında daha fazla bilgi için bkz: [yerel uygulama yeniden yönlendirme URI'si seçerek](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> İşletim sistemi ile özel bir URL şeması kaydetme ve işleme düzeni için mekanizma, her platform için özeldir.

Bir Azure Active Directory B2C Kiracı gönderilen her isteği belirtir bir *İlkesi*. İlkeleri tüketici kimlik deneyimi kaydolma ve oturum açma gibi açıklar. Örneğin, bir kayıt ilkesi kullanarak aşağıdaki ayarları yapılandırılması için Azure Active Directory B2C Kiracı davranışını sağlar:

- Tüketiciler uygulamaya oturum açmak için kullanabileceğiniz hesap türleri.
- Kayıt sırasında tüketiciden Toplanacak veriler.
- Çok faktörlü kimlik doğrulaması.
- Kaydolma sayfa içeriği.
- İlke çalıştırıldığında, mobil uygulama aldığı belirteci talep.

Azure Active Directory kiracısı gerektiği gibi uygulamanızda kullanılabilir farklı türlerde birden çok ilke içerebilir. Ayrıca, ilkeleri tanımlayın ve kodunuzu değiştirmeden tüketici kimlik deneyimi sağlayan uygulamalar arasında yeniden kullanılabilir. İlkeleri hakkında daha fazla bilgi için bkz: [Azure Active Directory B2C: yerleşik ilkeleri](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Kurulum

Microsoft kimlik doğrulama kitaplığı (MSAL) NuGet kitaplığı Xamarin.Forms çözümünü platform projelerinde ve taşınabilir sınıf kitaplığı (PCL) proje eklenmelidir. Aşağıdaki bölümler bir mobil uygulama bir Azure Active Directory B2C kiracısı ile iletişim kurmak için MSAL kullanmak için ek kurulum yönergeleri sağlar.

### <a name="portable-class-library"></a>Taşınabilir Sınıf Kitaplığı

MSAL tüketen PCLs Profile7 kullanılacak güncellendiyse gerekir. PCLs hakkında daha fazla bilgi için bkz: [taşınabilir sınıf kitaplıkları giriş](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

İos'ta, Azure Active Directory B2C ile kaydedildiği özel URL şeması kaydedilmelidir **Info.plist**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](azure-ad-b2c-images/customurl-ios.png "İOS özel bir URL şeması kaydetme")

Azure Active Directory B2C yetkilendirme isteği tamamlandığında, kayıtlı yeniden yönlendirme URL'sine yönlendirir. URL iOS mobil uygulama başlatılmadan sonuçları özel bir şema kullandığından, URL'de işleneceği tarafından bir başlatma parametre olarak geçirme `OpenUrl` uygulamanın geçersiz kılma `AppDelegate` aşağıdaki kodda gösterildiği sınıfı Örnek:

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

Kodda `OpenURL` yöntemi sağlar kimlik doğrulama iş akışı'nın etkileşimli kısmı sona erdikten sonra denetim için MSAL döndürür.

### <a name="android"></a>Android

Android, Azure Active Directory B2C ile kaydedildiği özel URL şeması kaydedilmelidir **AndroidManifest.xml**, ekleyerek bir `<activity>` varolan içindeki öğesi `<application>` öğesi. `<activity>` Öğesi belirttiğinden `IntentFilter` üzerinde `Activity` düzeni işler ve aşağıdaki örnekte gösterilen:

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

Azure Active Directory B2C yetkilendirme isteği tamamlandığında, kayıtlı yeniden yönlendirme URL'sine yönlendirir. URL Android mobil uygulama başlatılmadan sonuçları özel bir şema kullandığından, URL'de işleneceği tarafından bir başlatma parametre olarak geçirme `microsoft.identity.client.BrowserTabActivity`. Unutmayın `data android:scheme` özelliği, Azure Active Directory B2C uygulama ile kayıtlı özel URL şeması için ayarlanmalıdır.

Ayrıca, `MainActivity` sınıfı değiştirilmesi gerekir, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
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

`OnCreate` Yöntemi değişiklik atayarak bir `UIParent` için örnek `App.UiParent` özelliği. Bu kimlik doğrulaması akışı geçerli etkinlik bağlamında oluşmamasını sağlar.

Kodda `OnActivityResult` yöntemi sağlar kimlik doğrulama iş akışı'nın etkileşimli kısmı sona erdikten sonra denetim için MSAL döndürür.

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Evrensel Windows platformu üzerinde hiçbir ek ayar MSAL kullanmak için gereklidir.

## <a name="initialization"></a>Başlatma

Microsoft kimlik doğrulama kitaplığı üyeleri kullanan `PublicClientApplication` bir kimlik doğrulama iş akışını başlatmak için sınıf. Örnek uygulama bildirir ve başlatır bir `public` adlı bu tür özelliği `ADB2CClient`, `AuthenticationProvider` sınıfı. Aşağıdaki kod örneği, bu özelliği nasıl başlatılır gösterir:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Mobil uygulama Azure Active Directory B2C Kiracı ile kayıtlı, kayıt işlemini atanan bir **uygulama kimliği**. Bu kimliği belirtilmelidir `PublicClientApplication` Oluşturucusu ile birlikte bir `Authority` temel URL'sini ve Azure Active Directory B2C İlkesi yürütülecek oluşur sabiti.

## <a name="signing-in"></a>Oturum açma

Örnek uygulama bir oturum açma ekranında aşağıdaki ekran görüntülerinde gösterilir:

![](azure-ad-b2c-images/login.png "Oturum açma sayfası")

Oturum açma sosyal kimlik sağlayıcılar veya yerel bir hesap, izin verilir. Microsoft, Google ve Facebook, yukarıda gösterildiği gibi sosyal kimlik sağlayıcıları olarak kullanılırken diğer kimlik sağlayıcılardan de kullanılabilir.

Aşağıdaki kod örneği, oturum açma işlemini nasıl çağrıldığını gösterir:

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

`AcquireTokenAsync` Yöntemi cihazın web tarayıcısının başlatır ve aracılığıyla başvurulan ilke tarafından belirtilen Azure Active Directory B2C ilkede tanımlanan kimlik doğrulama seçeneklerini görüntüler `Constants.Authority` sabit. Bu ilke tüketicileri kaydolma sırasında ve oturum açma geçer deneyimleri ve uygulama üzerinde başarılı oturum açma veya kaydolma alacak talepleri tanımlar.

Sonucu `AcquireTokenAsync` yöntemi çağrısı bir `AuthenticationResult` örneği. Kimlik doğrulaması başarılı olursa `AuthenticationResult` örneği yerel olarak önbelleğe alınacak bir kimlik belirteci içerir. Kimlik doğrulaması başarısız olduğunda `AuthenticationResult` örnek neden kimlik doğrulaması başarısız oldu gösteren veri içeriyor.

Örnek uygulamasında kimlik doğrulaması başarılı olursa `TodoList` sayfa gittiğinizde için.

## <a name="silent-re-authentication"></a>Sessiz yeniden kimlik doğrulaması

Zaman `LoginPage` örnek uygulama görüntülenir, herhangi bir kimlik doğrulama kullanıcı arabirimini gösteren olmadan bir kullanıcı belirtecini almak için bir deneme yapılır. Bu ile elde edilen `AcquireTokenSilentAsync` aşağıdaki kod örneğinde gösterildiği şekilde yöntemi:

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

`AcquireTokenSilentAsync` Yöntemi kullanıcının oturum açma gerek kalmadan kullanıcı belirteci önbellekten almayı dener. Bu senaryo burada uygun bir belirteç zaten önceki oturumlardan önbellekte bulunabilecek işler. Bir belirteci edinmesine deneme başarılı olursa `TodoList` sayfa gittiğinizde için. Bir belirteç elde denemesi başarısız olursa, hiçbir şey olmaz ve kullanıcının yeni bir kimlik doğrulama iş akışı başlatma seçeneğine sahip olur.

## <a name="signing-out"></a>Oturumu kapatma

Aşağıdaki kod örneği, oturum kapatma işlemini nasıl çağrıldığını gösterir:

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

Bu, yerel önbellek tüm kimlik doğrulama belirteçleri temizler.

## <a name="summary"></a>Özet

Bu makalede Microsoft kimlik doğrulama kitaplığı (MSAL) ve Azure Active Directory B2C tüketici kimlik yönetimini mobil uygulamasına tümleştirmek için nasıl kullanılacağı gösterilmektedir. Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür.


## <a name="related-links"></a>İlgili bağlantılar

- [AzureADB2CAuth (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft kimlik doğrulama kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client)
