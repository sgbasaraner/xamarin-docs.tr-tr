---
title: Grafik API'si erişme
description: Xamarin kullanarak grafik API'si sorgulamak için Active Directory'yi kullanma
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e177ac680a100a2723732c2ee7252ea0c16ea972
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="accessing-the-graph-api"></a>Grafik API'si erişme

_Xamarin kullanarak grafik API'si sorgulamak için Active Directory'yi kullanma_

Xamarin uygulamanızda grafik API'SİNDEN kullanmak için aşağıdaki adımları izleyin:

1. [Azure Active Directory ile kaydetme](~/cross-platform/data-cloud/active-directory/get-started/register.md) üzerinde *windowsazure.com* portal, ardından
2. [Hizmetleri Yapılandırma](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Adım 3. Active Directory kimlik doğrulaması için bir uygulama ekleme

Uygulamanızda bir başvuru ekleyin **Azure Active Directory Authentication Library (Azure ADAL)** Mac için Visual Studio veya Visual Studio NuGet Paket Yöneticisi'ni kullanma
Seçtiğinizden emin olun **Göster ön sürüm paketlerini** hala Önizleme'de olduğu gibi bu paketi eklemek için.

> [!IMPORTANT]
> Not: Azure ADAL 3.0 şu anda Önizleme ve son sürümü yayımlanmadan önce önemli değişiklikler olabilir. 


![](graph-images/06.-adal-nuget-package.jpg "Azure Active Directory Authentication Library (Azure ADAL) için bir başvuru ekleyin")

Uygulamanızda artık kimlik doğrulaması akışı için gerekli olan aşağıdaki sınıf düzeyi değişkenleri eklemeniz gerekir.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

İşte not etmek için bir şey `commonAuthority`. Kimlik doğrulama uç noktası olduğunda `common`, uygulamanızı hale **çok kiracılı**, herhangi bir kullanıcı başka bir deyişle, oturum açma ile Active Directory kimlik bilgilerini kullanabilirsiniz. Kimlik doğrulamasından sonra kullanıcı, kendi Active Directory bağlamında çalışır – başka bir deyişle, Active Directory ile ilgili ayrıntıları görürsünüz.

### <a name="write-method-to-acquire-access-token"></a>Erişim belirteci almak üzere yöntemi yazma

Aşağıdaki kodu (Android) kimlik doğrulamasını başlatmasını ve tamamlandıktan sonra sonuç atayın `authResult`. İOS ve Windows Phone uygulamaları biraz farklılık gösterir: ikinci parametre (`Activity`) yok ve İos'ta farklı Windows Phone üzerinde.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

Yukarıdaki kod `AuthenticationContext` commonAuthority ile kimlik doğrulaması sorumludur. Bunun bir `AcquireTokenAsync` parametreleri, bu durumda erişilebilmesi için gereken bir kaynak olarak ele yöntemi `graphResourceUri`, `clientId`, ve `returnUri`. Uygulama dönersiniz `returnUri` kimlik doğrulaması tamamlandığında. Bu kod tüm platformlar, ancak son parametresi için aynı kalacaksa `AuthorizationParameters`, farklı platformlarda farklı olacaktır ve kimlik doğrulaması akışı yöneten sorumludur.

Android veya iOS söz konusu olduğunda, biz geçirmek `this` parametresi `AuthorizationParameters(this)` Windows'da onu herhangi bir parametre olarak yeni geçirilen ancak içeriği paylaşmak için `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Android için tanıtıcı devamlılık

Kimlik doğrulama tamamlandıktan sonra akışı uygulamaya döndürmelidir. Aşağıdaki kod tarafından işlenir Android söz konusu olduğunda, hangi eklenmesi için **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Windows Phone için tanıtıcı devamlılık

Windows Phone için değiştirme `OnActivated` yönteminde **App.xaml.cs** ile dosya kod aşağıda:

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Şimdi çalıştırırsanız uygulama bir kimlik doğrulama iletişim kutusu görmeniz gerekir.
Başarılı kimlik doğrulaması sırasında (Bu örnekte grafik API'si) kaynaklara erişmek için izinleri ister:

![](graph-images/08.-authentication-flow.jpg "Başarılı kimlik doğrulaması sırasında grafik API'si Bu örnekte kaynaklara erişmek için izinlerinizi sorar")

Kimlik doğrulama başarılı olur ve kaynaklara erişmek için uygulama yetkilendirdiğiniz, alması gereken bir `AccessToken` ve `RefreshToken` açılan içinde `authResult`. Bu belirteçler, Azure Active Directory ile yetkilendirme arka planda ve daha fazla API çağrıları için gereklidir.

![](graph-images/07.-access-token-for-authentication.jpg "Bu belirteçler Azure Active Directory ile yetkilendirme arka planda ve daha fazla API çağrıları için gereklidir")

Örneğin, aşağıdaki kod, Active Directory'den kullanıcı listesini almak sağlar. Azure AD tarafından korunan, Web API ile Web API URL'sini değiştirebilirsiniz.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

