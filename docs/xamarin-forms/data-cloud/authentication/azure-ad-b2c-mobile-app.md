---
title: Azure Active Directory B2C Azure mobil uygulamaları ile tümleştirme
description: Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür. Bu makalede, Azure Active Directory B2C kimlik doğrulaması ve yetkilendirme için bir Azure Mobile Apps örneğine Xamarin.Forms ile sağlamak için nasıl kullanılacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: cafc1e78779dc393fa0409daa08b3daa8948a1ee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure Active Directory B2C Azure mobil uygulamaları ile tümleştirme

_Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür. Bu makalede, Azure Active Directory B2C kimlik doğrulaması ve yetkilendirme için bir Azure Mobile Apps örneğine Xamarin.Forms ile sağlamak için nasıl kullanılacağı gösterilmektedir._

![](~/media/shared/preview.png "Bu API şu anda yayın öncesi")

> [!NOTE]
> [Microsoft kimlik doğrulama Kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client) hala önizlemede değil, ancak bir üretim ortamında kullanım için uygundur. Ancak, var. API, dahili önbellek biçimini ve uygulamanızı etkileyebilir kitaplığın başka mekanizmalar önemli değişiklikler.

## <a name="overview"></a>Genel Bakış

Azure Mobile Apps ile ölçeklenebilir arka uçlarını desteği Mobil kimlik doğrulama, çevrimdışı eşitleme ve anında iletme bildirimleri ile Azure App Service içinde barındırılan uygulamalar geliştirmek sağlar. Azure Mobile Apps hakkında daha fazla bilgi için bkz: [Azure mobil uygulaması tüketen](~/xamarin-forms/data-cloud/consuming/azure.md), ve [Azure Mobile Apps ile kimlik doğrulaması yapan kullanıcıların](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C tüketicilerin oturum tarafından uygulamanıza açma sağlayan bir kimlik yönetimi hizmeti tüketiciye yönelik uygulamalar için şöyledir:

- Var olan sosyal hesaplarını (Microsoft, Google, Facebook, Amazon, LinkedIn) kullanarak.
- Yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturma. Bu kimlik bilgileri olarak adlandırılır *yerel* hesaplar.

Azure Active Directory B2C hakkında daha fazla bilgi için bkz: [kimlik doğrulaması yapan kullanıcıların Azure Active Directory B2C ile](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C, kimlik doğrulama iş akışı için bir Azure Mobile uygulamasını yönetmek için kullanılabilir. Bu yaklaşım ile kimlik yönetimi deneyimi buluta tam olarak tanımlanır ve mobil uygulama kodunuzu değiştirmeden değiştirilebilir.

Azure Active Directory B2C kiracısı Azure Mobile Apps örnek tümleştirdiğinizde benimsenen iki kimlik doğrulama iş akışlarının vardır:

- [İstemci tarafından yönetilen](#client_managed) – bu kimlik doğrulaması ile Azure Active Directory B2C Kiracı işlem Xamarin.Forms mobil uygulama başlatır yaklaşmak ve Azure Mobile Apps örneğine alınan kimlik doğrulama belirteci geçirir.
- [Sunucu tarafından yönetilen](#server_managed) – bu yaklaşım Azure Mobile Apps örnek bir web tabanlı iş akışı aracılığıyla kimlik doğrulama işlemini başlatmak için Azure Active Directory B2C Kiracı kullanır.

Her iki durumda da, kimlik doğrulama deneyimi Azure Active Directory B2C Kiracı tarafından sağlanır. Örnek uygulama, bu oturum açma ekranında aşağıdaki ekran görüntülerinde gösterilen sonuçlanır:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Oturum açma sayfası")

Oturum açma sosyal kimlik sağlayıcılar veya yerel bir hesap, izin verilir. Microsoft, Google ve Facebook sosyal kimlik sağlayıcıları bu örnekte olarak kullanılır, ancak diğer kimlik sağlayıcılardan de kullanılabilir.

## <a name="setup"></a>Kurulum

Kullanılan kimlik doğrulama iş akışı bağımsız olarak, Azure Active Directory B2C kiracısı Azure Mobile Apps örneği ile tümleştirmek için ilk işlemi aşağıdaki gibidir:

1. Azure Mobile Apps örneği oluşturun. Daha fazla bilgi için bkz: [Azure mobil uygulaması tüketen](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Azure Mobile Apps örneği ve Xamarin.Forms uygulaması kimlik doğrulamasını etkinleştirin. Daha fazla bilgi için bkz: [Azure Mobile Apps ile kimlik doğrulaması yapan kullanıcıların](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Azure Active Directory B2C kiracısı oluşturun. Daha fazla bilgi için bkz: [kimlik doğrulaması yapan kullanıcıların Azure Active Directory B2C ile](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Microsoft kimlik doğrulama kitaplığı (MSAL) istemcisi yönetilen kimlik doğrulama iş akışı kullanırken gerekli olduğunu unutmayın. MSAL cihazın web tarayıcısının kimlik doğrulaması yapmak için kullanır. Bu, oturum cihaz başına dönüştürme oranları oturum açma ve yetkilendirme geliştirme uygulamada akar sonra açmak kullanıcılara yalnızca gerektiği bir uygulamanın kullanılabilirliğini artırır. Aygıt tarayıcı de Gelişmiş güvenlik sağlar. Kullanıcı kimlik doğrulama işlemi tamamlandıktan sonra web tarayıcısı sekmesinden denetim uygulamaya döndürür. Bu kimlik doğrulama işlemi, algılama ve gönderildikten sonra Özel URL işleme öğesinden döndürülen yeniden yönlendirme URL'si için özel bir URL şeması kaydederek sağlanır. Azure Active Directory B2C kiracısı ile iletişim kurmak için MSAL kullanma hakkında daha fazla bilgi için bkz: [kimlik doğrulaması yapan kullanıcıların Azure Active Directory B2C ile](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Yönetilen istemci kimlik doğrulaması

Yönetilen istemci kimlik doğrulaması, bir Xamarin.Forms mobil uygulama kimlik doğrulama akışı başlatmak için bir Azure Active Directory B2C Kiracı iletişim kurar. Başarılı oturum açma sonra Azure Active Directory B2C Kiracı sonra oturum açma Azure Mobile Apps örneğine sırasında sağlanan bir kimlik belirteci döndürür. Bu, kimliği doğrulanmış kullanıcı izinleri gerektirir Azure Mobile Apps örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması sağlar.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C Kiracı yapılandırması

Yönetilen istemci kimlik doğrulaması için bir iş akışı, Azure Active Directory B2C Kiracı aşağıdaki gibi yapılandırılmalıdır:

- Yerel istemcisi içerir.
- Ardından mobil uygulama benzersiz olarak tanımlayan bir URL şeması için özel yeniden yönlendirme URI'si ayarlayın `://auth/`. Özel bir URL şeması seçme hakkında daha fazla bilgi için bkz: [yerel uygulama yeniden yönlendirme URI'si seçerek](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C Yapılandırması")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C yapılandırması")

Azure Active Directory Kiracı, ayrıca aynı özel URL şemasının için yanıt URL'si oluşturmak yapılandırılmalıdır B2C kullanılan ilkeyi ve ardından `://auth/`. Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory B2C ilkeleri")

### <a name="azure-mobile-app-configuration"></a>Azure mobil uygulama yapılandırması

Yönetilen istemci kimlik doğrulaması için bir iş akışı, Azure Mobile Apps örneği aşağıdaki gibi yapılandırılmalıdır:

- Uygulama hizmeti kimlik doğrulama açık olmalıdır.
- Bir isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem ayarlanmalı **Azure Active Directory ile oturum aç**.

Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure Mobile Apps yapılandırma")

Azure Mobile Apps örnek Azure Active Directory B2C Kiracı ile iletişim kurmak için de yapılandırılması gerekir. Bu etkinleştirerek gerçekleştirilebilir **Gelişmiş** Azure Active Directory kimlik doğrulama sağlayıcısı için modu ile **istemci kimliği** olan **uygulama kimliği** Azure Active Directory B2C Kiracı ve **veren URL'si** Azure Active Directory B2C ilkesi için meta veri uç noktası oluşturuluyor. Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Gelişmiş Yapılandırma azure mobil uygulamalar")

### <a name="signing-in"></a>Oturum açma

Aşağıdaki kod örneği, istemci yönetilen kimlik doğrulaması iş akışını başlatmak gösterilmektedir:

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

Microsoft kimlik doğrulama kitaplığı (MSAL) ile Azure Active Directory B2C Kiracı kimlik doğrulama iş akışını başlatmak için kullanılır. `AcquireTokenAsync` Yöntemi cihazın web tarayıcısının başlatır ve aracılığıyla başvurulan ilke tarafından belirtilen Azure Active Directory B2C ilkede tanımlanan kimlik doğrulama seçeneklerini görüntüler `Constants.Authority` sabit. Bu ilke tüketicileri kaydolma sırasında ve oturum açma geçer deneyimleri ve uygulama üzerinde başarılı oturum açma veya kaydolma alacak talepleri tanımlar.

Sonucu `AcquireTokenAsync` yöntemi çağrısı bir `AuthenticationResult` örneği. Kimlik doğrulaması başarılı olursa `AuthenticationResult` örneği yerel olarak önbelleğe alınacak bir kimlik belirteci içerir. Kimlik doğrulaması başarısız olduğunda `AuthenticationResult` örnek neden kimlik doğrulaması başarısız oldu gösteren veri içeriyor. Azure Active Directory B2C kiracısı ile iletişim kurmak için MSAL kullanma hakkında daha fazla bilgi için bkz: [kimlik doğrulaması yapan kullanıcıların Azure Active Directory B2C ile](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Zaman `MobileServiceClient.LoginAsync` yöntemi çağrıldığında, Azure Mobile Apps örnek sarmalanmış kimlik belirteç alan bir `JObject`. Azure Mobile Apps örnek kendi OAuth 2.0 kimlik doğrulaması akışı başlatmak gerekmez geçerli bir belirteç anlamına varlığını. Bunun yerine, `MobileServiceClient.LoginAsync` yöntemi döndürür bir `MobileServiceUser` depolanır örneği `MobileServiceClient.CurrentUser` özelliği. Bu özellik sağlar `UserId` ve `MobileServiceAuthenticationToken` özellikleri. Bu, kimliği doğrulanmış kullanıcının ve, sona erene kadar kullanılabilecek kullanıcı için bir kimlik doğrulama belirteci temsil eder. Kimlik doğrulama belirteci kimliği doğrulanmış kullanıcı izinleri gerektiren Azure Mobile Apps örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması izin vererek Azure Mobile Apps örneği yapılan tüm istekleri eklenecektir.

### <a name="signing-out"></a>Oturumu kapatma

Aşağıdaki kod örneği, istemci yönetilen oturum kapatma işlemini nasıl çağrıldığını gösterir:

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

`MobileServiceClient.LogoutAsync` Yöntemi XML'deki Azure Mobile Apps örneği ile kullanıcı kimliğini doğrular ve ardından tüm kimlik doğrulama belirteçleri MSAL tarafından oluşturulan yerel önbellek temizlenir.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Yönetilen sunucu kimlik doğrulaması

Yönetilen sunucu kimlik doğrulaması, bir Xamarin.Forms uygulaması B2C ilkesinde tanımlandığı şekilde bir oturum açma sayfası görüntüleyerek OAuth 2.0 kimlik doğrulaması akışı yönetmek için Azure Active Directory B2C Kiracı kullanan bir Azure Mobile Apps örneği ile iletişim kurar. Başarılı oturum açma, Xamarin.Forms uygulaması kimliği doğrulanmış kullanıcı izinleri gerektiren Azure Mobile Apps örneğinde eylemleri gerçekleştirmek imkan tanıyan bir belirteç Azure Mobile Apps örneğini döndürür.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C Kiracı yapılandırması

Yönetilen sunucu kimlik doğrulaması için bir iş akışı, Azure Active Directory B2C Kiracı aşağıdaki gibi yapılandırılmalıdır:

- Örtük akış izin vermek ve bir web uygulaması/web API'si içerir.
- Azure mobil uygulaması adresine yanıt URL'si ayarlayın, ardından `/.auth/login/aad/callback`.

Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C Yapılandırması")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C yapılandırması")

Azure Active Directory Kiracı, ayrıca Azure mobil uygulaması adresine yanıt URL'si oluşturmak yapılandırılmalıdır B2C kullanılan ilkeyi ve ardından `/.auth/login/aad/callback`. Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory B2C ilkeleri")

### <a name="azure-mobile-apps-instance-configuration"></a>Azure Mobile Apps örnek yapılandırması

Bir sunucu yönetilen kimlik doğrulama iş akışı için Azure Mobile Apps örneği aşağıdaki gibi yapılandırılmalıdır:

- Uygulama hizmeti kimlik doğrulama açık olmalıdır.
- Bir isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem ayarlanmalı **Azure Active Directory ile oturum aç**.

Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure Mobile Apps yapılandırma")

Azure Mobile Apps örnek Azure Active Directory B2C Kiracı ile iletişim kurmak için de yapılandırılması gerekir. Bu etkinleştirerek gerçekleştirilebilir **Gelişmiş** Azure Active Directory kimlik doğrulama sağlayıcısı için modu ile **istemci kimliği** olan **uygulama kimliği** Azure Active Directory B2C Kiracı ve **veren URL'si** Azure Active Directory B2C ilkesi için meta veri uç noktası oluşturuluyor. Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Gelişmiş Yapılandırma azure mobil uygulamalar")

### <a name="signing-in"></a>Oturum açma

Aşağıdaki kod örneğinde, sunucu yönetilen kimlik doğrulaması iş akışını başlatmak gösterilmektedir:

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

Zaman `MobileServiceClient.LoginAsync` yöntemi çağrıldığında, Azure Mobile Apps örnek OAuth 2.0 kimlik doğrulaması akışı başlatır bağlı Azure Active Directory B2C İlkesi yürütür. Unutmayın, her `AuthenticateAsync` platforma özgü bir yöntemdir. Bununla birlikte, her `AuthenticateAsync` yöntemi kullanan `MobileServiceClient.LoginAsync` yöntemi ve bir Azure Active Directory Kiracı kimlik doğrulama işleminde kullanılacak belirtir. Daha fazla bilgi için bkz: [kullanıcılar oturum](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

`MobileServiceClient.LoginAsync` Yöntemi döndürür bir `MobileServiceUser` depolanır örneği `MobileServiceClient.CurrentUser` özelliği. Bu özellik sağlar `UserId` ve `MobileServiceAuthenticationToken` özellikleri. Bu, kimliği doğrulanmış kullanıcının ve, sona erene kadar kullanılabilecek kullanıcı için bir kimlik doğrulama belirteci temsil eder. Kimlik doğrulama belirteci kimliği doğrulanmış kullanıcı izinleri gerektiren Azure Mobile Apps örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması izin vererek Azure Mobile Apps örneği yapılan tüm istekleri eklenecektir.

### <a name="signing-out"></a>Oturumu kapatma

Aşağıdaki kod örneğinde, sunucu yönetilen oturum kapatma işlemini nasıl çağrıldığını gösterir:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` Yöntemi XML'deki Azure Mobile Apps örneği ile kullanıcı kimliğini doğrular. Daha fazla bilgi için bkz: [çıkışı oturum açan kullanıcılar](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Özet

Bu makalede, Azure Active Directory B2C kimlik doğrulaması ve yetkilendirme için bir Azure Mobile Apps örneğine Xamarin.Forms ile sağlamak için nasıl kullanılacağı gösterilmektedir. Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoAzureAuth ServerFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Bir Azure mobil uygulaması kullanma](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps kullanıcıların kimlik doğrulaması](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Kullanıcıların Azure Active Directory B2C ile kimlik doğrulaması](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft kimlik doğrulama kitaplığı](https://www.nuget.org/packages/Microsoft.Identity.Client)
