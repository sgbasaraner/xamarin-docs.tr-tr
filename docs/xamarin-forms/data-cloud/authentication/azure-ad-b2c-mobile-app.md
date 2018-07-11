---
title: Azure Active Directory B2C'yi Azure Mobile Apps ile tümleştirme
description: Azure Active Directory B2C, tüketicilere yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümü ' dir. Bu makalede Azure Active Directory B2C kimlik doğrulaması ve yetkilendirme için Azure Mobile Apps örneği ile Xamarin.Forms sağlamak için nasıl kullanılacağını gösterir.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: cafc1e78779dc393fa0409daa08b3daa8948a1ee
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815683"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure Active Directory B2C'yi Azure Mobile Apps ile tümleştirme

_Azure Active Directory B2C, tüketicilere yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümü ' dir. Bu makalede Azure Active Directory B2C kimlik doğrulaması ve yetkilendirme için Azure Mobile Apps örneği ile Xamarin.Forms sağlamak için nasıl kullanılacağını gösterir._

![](~/media/shared/preview.png "Bu API, şu anda yayın öncesi")

> [!NOTE]
> [Microsoft kimlik doğrulama Kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client) hala Önizleme aşamasındadır ancak bir üretim ortamında kullanım için uygundur. Ancak, var. API, dahili önbellek biçimini ve uygulamanızı etkileyebilecek kitaplığın başka mekanizmalar için bozucu değişiklikler.

## <a name="overview"></a>Genel Bakış

Azure Mobile Apps Mobil kimlik doğrulama, çevrimdışı eşitleme ve anında iletme bildirimleri desteği ile Azure App Service'te barındırılan ölçeklenebilir arka uçları ile uygulamalar geliştirmenize imkan sağlar. Azure Mobile Apps hakkında daha fazla bilgi için bkz: [kullanan bir Azure mobil uygulaması](~/xamarin-forms/data-cloud/consuming/azure.md), ve [Azure Mobile Apps ile kimlik doğrulaması yapan kullanıcıların](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C, tüketicilerin uygulamanıza tarafından oturum sağlayan bir kimlik yönetim hizmeti tüketiciye yönelik uygulamalar için verilmiştir:

- İster mevcut sosyal hesaplarını (Microsoft, Google, Facebook, Amazon, LinkedIn) kullanarak.
- Yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturma. Bu kimlik bilgileri olarak ifade edilir *yerel* hesaplar.

Azure Active Directory B2C hakkında daha fazla bilgi için bkz: [Azure Active Directory B2C ile kullanıcıların kimliğini doğrulama](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C, Azure mobil uygulaması için kimlik doğrulama iş akışını yönetmek için kullanılabilir. Bu yaklaşım ile kimlik yönetimi deneyimi bulutta tam olarak tanımlanır ve mobil uygulama kodunuzu değiştirmeden değiştirilebilir.

Azure Active Directory B2C kiracısı bir Azure Mobile Apps örneği ile tümleştirdiğinizde önemsenmeksizin devralınabilir iki kimlik doğrulama iş akışı vardır:

- [Yönetilen](#client_managed) – bu yaklaşımı kimlik doğrulama işlemi ile Azure Active Directory B2C kiracısı Xamarin.Forms mobil uygulama başlatır ve alınan kimlik doğrulaması belirteci Azure Mobile Apps örneğine geçirir.
- [Sunucusu tarafından yönetilen](#server_managed) – bu yaklaşım Azure Mobile Apps örneği web tabanlı bir iş akışı aracılığıyla kimlik doğrulama işlemini başlatmak için Azure Active Directory B2C kiracısı kullanır.

Her iki durumda da, Azure Active Directory B2C kiracısı tarafından kimlik doğrulaması deneyimi sağlanır. Örnek uygulamada oturum açma ekranı aşağıdaki ekran görüntülerinde gösterilen sonuçlanır:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Oturum açma sayfası")

Oturum açma ile yerel bir hesap veya sosyal kimlik sağlayıcıları, izin verilir. Bu örnekte sosyal kimlik sağlayıcıları olarak Microsoft, Google ve Facebook kullanılırken, diğer kimlik sağlayıcılardan de kullanılabilir.

## <a name="setup"></a>Kurulum

Kullanılan kimlik doğrulama iş akışı bağımsız olarak, Azure Active Directory B2C kiracısı bir Azure Mobile Apps örneği ile tümleştirmek için ilk işlem aşağıdaki gibidir:

1. Azure Mobile Apps örneği oluşturun. Daha fazla bilgi için [kullanan bir Azure mobil uygulaması](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Azure Mobile Apps örneği ve Xamarin.Forms uygulaması ile kimlik doğrulamasını etkinleştirin. Daha fazla bilgi için [Azure Mobile Apps ile kimlik doğrulaması yapan kullanıcıların](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Azure Active Directory B2C kiracısı oluşturun. Daha fazla bilgi için [Azure Active Directory B2C ile kullanıcıların kimliğini doğrulama](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Microsoft Authentication Library (MSAL) yönetilen bir kimlik doğrulama iş akışı kullanırken gerekli olduğunu unutmayın. MSAL, kimlik doğrulaması gerçekleştirmek için cihazın web tarayıcısının kullanır. Kullanıcılar yalnızca akış uygulamada oturum açma ve kimlik doğrulama, dönüşüm oranlarını artırma cihaz bir kez oturum açmanız gerekeceğinden, bu uygulamanın kullanılabilirliğini artırır. Cihaz tarayıcısı de Gelişmiş güvenlik sağlar. Kullanıcı kimlik doğrulama işlemi tamamlandıktan sonra Denetim web tarayıcı sekmesini kullanarak uygulamaya döndürür. Bu, kimlik doğrulama işleminde, algılama ve gönderildikten sonra Özel URL işleme öğesinden döndürülen yeniden yönlendirme URL'si için özel bir URL şeması kaydederek sağlanır. Bir Azure Active Directory B2C kiracısı ile iletişim kurmak için MSAL kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C ile kullanıcıların kimliğini doğrulama](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Yönetilen kimlik doğrulaması

Yönetilen kimlik doğrulaması, bir Xamarin.Forms mobil uygulama kimlik doğrulama akışı başlatmak için bir Azure Active Directory B2C kiracısı ile iletişim kurar. Başarılı oturum açma işleminden sonra Azure Active Directory B2C kiracısı sonra oturum açma için Azure Mobile Apps örneği sırasında sağlanan bir kimlik belirteci döndürür. Bu, kimliği doğrulanmış kullanıcı izinleri gerektiren Azure Mobile Apps örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması sağlar.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C Kiracısı yapılandırması

Bir yönetilen kimlik doğrulama iş akışı için Azure Active Directory B2C kiracısı aşağıdaki gibi yapılandırılmalıdır:

- Yerel bir istemci içerir.
- Ardından, mobil uygulama benzersiz olarak tanımlayan bir URL şeması için özel yeniden yönlendirme URI'sini ayarlayın `://auth/`. Özel bir URL düzeni seçme hakkında daha fazla bilgi için bkz. [yerel uygulama yeniden yönlendirme URI'si seçme](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C yapılandırmasını")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C'yi yapılandırma")

Azure Active Directory Kiracı, ayrıca yanıt URL'si aynı özel URL düzeni ayarlanır böylece yapılandırılmalıdır B2C kullanılan ilkeyi ve ardından `://auth/`. Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory B2C ilkeleri")

### <a name="azure-mobile-app-configuration"></a>Azure mobil uygulama yapılandırması

Bir yönetilen kimlik doğrulama iş akışı için Azure Mobile Apps örneği aşağıdaki gibi yapılandırılmalıdır:

- App Service kimlik doğrulaması açık olması.
- Bir isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem ayarlanmalıdır **Azure Active Directory ile oturum aç**.

Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure mobil uygulamaları yapılandırma")

Azure Active Directory B2C kiracısı ile iletişim kurmak için Azure Mobile Apps örneği de yapılandırılmalıdır. Bu etkinleştirerek gerçekleştirilebilir **Gelişmiş** Azure Active Directory kimlik doğrulama sağlayıcısı için modu ile **istemci kimliği** olan **uygulama kimliği** Azure Active Directory B2C kiracısı ve **veren URL'si** Azure Active Directory B2C ilkesi için meta veri uç noktası oluşturuluyor. Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure mobil uygulamalarda Gelişmiş Yapılandırma")

### <a name="signing-in"></a>Oturum açma

Aşağıdaki kod örneği, bir yönetilen kimlik doğrulama iş akışı başlatmak gösterilmektedir:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);

    ...
    var payload = new JObject();
    payload["access_token"] = authenticationResult.IdToken;

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    ...
}
```

Microsoft Authentication Library (MSAL), Azure Active Directory B2C kiracısı ile bir kimlik doğrulama iş akışı başlatmak için kullanılır. `AcquireTokenAsync` Yöntemi cihazın web tarayıcısının başlatır ve kimlik doğrulama seçenekleri aracılığıyla başvurulan ilke tarafından belirtilen Azure Active Directory B2C İlkesi'nde tanımlanan görüntüler `Constants.Authority` sabit. Bu ilke, tüketicilerin kaydolma sırasında ve oturum açma karşılaşacağı deneyimleri ve uygulamanın üzerinde başarılı kaydolma veya oturum açma alacağı talepleri tanımlar.

Sonucu `AcquireTokenAsync` yöntem çağrısında bir `AuthenticationResult` örneği. Kimlik doğrulaması başarılı olursa `AuthenticationResult` örneği yerel olarak önbelleğe alınabilir bir kimlik belirteci içerir. Kimlik doğrulaması başarısız olursa `AuthenticationResult` örnek kimlik doğrulama başarısız olmasının gösteren veri içeriyor. Bir Azure Active Directory B2C kiracısı ile iletişim kurmak için MSAL kullanma hakkında daha fazla bilgi için bkz: [Azure Active Directory B2C ile kullanıcıların kimliğini doğrulama](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Zaman `MobileServiceClient.LoginAsync` yöntemi çağrıldığında, Azure Mobile Apps örnek sarmalanmış kimlik belirteci alır bir `JObject`. Varlığı kendi OAuth 2.0 kimlik doğrulama akışı başlatmak için Azure Mobile Apps örneği gerekmiyor bir geçerli belirteç anlamına gelir. Bunun yerine, `MobileServiceClient.LoginAsync` yöntemi döndürür bir `MobileServiceUser` barındırılacaktır örneği `MobileServiceClient.CurrentUser` özelliği. Bu özellik sağlar `UserId` ve `MobileServiceAuthenticationToken` özellikleri. Bu, kimliği doğrulanmış kullanıcı ve süresi sona erene kadar kullanılabilen kullanıcı için bir kimlik doğrulama belirteci temsil eder. Kimlik doğrulama belirteci kimliği doğrulanmış kullanıcı izinleri gerektiren Azure Mobile Apps örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması izin vererek Azure Mobile Apps örneğinin yapılan tüm isteklere eklenecektir.

### <a name="signing-out"></a>Oturum kapatılıyor

Aşağıdaki kod örneği, istemci yönetilen oturum kapatma işlemini nasıl çağrılan gösterir:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

`MobileServiceClient.LogoutAsync` Yöntemi serbest Azure Mobile Apps örneği ile kullanıcı kimliğini doğrular ve ardından tüm kimlik doğrulama belirteçlerinizi MSAL tarafından oluşturulan yerel önbellekten kaldırılır.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Sunucu yönetilen kimlik doğrulaması

Sunucusu tarafından yönetilen kimlik doğrulaması, bir Xamarin.Forms uygulaması OAuth 2.0 kimlik doğrulama akışı B2C İlkesi'nde tanımlandığı şekilde bir oturum açma sayfası görüntüleyerek yönetmek için Azure Active Directory B2C kiracısı kullanan bir Azure Mobile Apps örneği ile iletişim kurar. Başarılı oturum açma, Azure Mobile Apps örneği kimliği doğrulanmış kullanıcı izinleri gerektiren Azure Mobile Apps örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması sağlayan bir belirteç döndürür.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C Kiracısı yapılandırması

Bir sunucu yönetilen kimlik doğrulama iş akışı için Azure Active Directory B2C kiracısı aşağıdaki gibi yapılandırılmalıdır:

- Örtük akışa izin ver ve web uygulaması/web API'si içerir.
- Yanıt URL'si Azure mobil uygulaması adresine ayarlayın, ardından `/.auth/login/aad/callback`.

Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C yapılandırmasını")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C'yi yapılandırma")

Azure Active Directory Kiracı, ayrıca Azure mobil uygulaması adresine yanıt URL'si ayarlanır böylece yapılandırılmalıdır B2C kullanılan ilkeyi ve ardından `/.auth/login/aad/callback`. Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory B2C ilkeleri")

### <a name="azure-mobile-apps-instance-configuration"></a>Azure mobil uygulamaları örnek yapılandırması

Bir sunucu yönetilen kimlik doğrulama iş akışı için Azure Mobile Apps örneği aşağıdaki gibi yapılandırılmalıdır:

- App Service kimlik doğrulaması açık olması.
- Bir isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem ayarlanmalıdır **Azure Active Directory ile oturum aç**.

Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure mobil uygulamaları yapılandırma")

Azure Active Directory B2C kiracısı ile iletişim kurmak için Azure Mobile Apps örneği de yapılandırılmalıdır. Bu etkinleştirerek gerçekleştirilebilir **Gelişmiş** Azure Active Directory kimlik doğrulama sağlayıcısı için modu ile **istemci kimliği** olan **uygulama kimliği** Azure Active Directory B2C kiracısı ve **veren URL'si** Azure Active Directory B2C ilkesi için meta veri uç noktası oluşturuluyor. Bu yapılandırma aşağıdaki ekran görüntüsünde gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure mobil uygulamalarda Gelişmiş Yapılandırma")

### <a name="signing-in"></a>Oturum açma

Aşağıdaki kod örneği, bir sunucu yönetilen kimlik doğrulama iş akışı başlatmak gösterilmektedir:

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...
    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        UIApplication.SharedApplication.KeyWindow.RootViewController,
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);
    ...
}
```

Zaman `MobileServiceClient.LoginAsync` yöntemi çağrıldığında, OAuth 2.0 kimlik doğrulama akışı başlatan bağlı Azure Active Directory B2C İlkesi, Azure Mobile Apps örneği yürütür. Unutmayın, her `AuthenticateAsync` platforma özgü bir yöntemdir. Ancak, her `AuthenticateAsync` yöntemi kullanan `MobileServiceClient.LoginAsync` yöntemi ve bir Azure Active Directory Kiracı kimlik doğrulama işleminde kullanılacak belirtir. Daha fazla bilgi için [kullanıcılar oturum](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

`MobileServiceClient.LoginAsync` Yöntemi döndürür bir `MobileServiceUser` barındırılacaktır örneği `MobileServiceClient.CurrentUser` özelliği. Bu özellik sağlar `UserId` ve `MobileServiceAuthenticationToken` özellikleri. Bu, kimliği doğrulanmış kullanıcı ve süresi sona erene kadar kullanılabilen kullanıcı için bir kimlik doğrulama belirteci temsil eder. Kimlik doğrulama belirteci kimliği doğrulanmış kullanıcı izinleri gerektiren Azure Mobile Apps örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması izin vererek Azure Mobile Apps örneğinin yapılan tüm isteklere eklenecektir.

### <a name="signing-out"></a>Oturum kapatılıyor

Aşağıdaki kod örneği, sunucu yönetilen oturum kapatma işlemini nasıl çağrılan gösterir:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` Yöntemi serbest Azure Mobile Apps örneği ile kullanıcı kimliğini doğrular. Daha fazla bilgi için [kullanıma oturum açan kullanıcılar](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Özet

Bu makalede Xamarin.Forms ile kimlik doğrulaması ve yetkilendirme için Azure Mobile Apps örneği sağlamak için Azure Active Directory B2C kullanma gösterilmektedir. Azure Active Directory B2C, tüketicilere yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümü ' dir.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoAzureAuth ServerFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Bir Azure mobil uygulaması kullanma](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure mobil uygulamaları ile kullanıcıların kimliğini doğrulama](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Active Directory B2C ile kullanıcıların kimliğini doğrulama](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft kimlik doğrulama kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client)
