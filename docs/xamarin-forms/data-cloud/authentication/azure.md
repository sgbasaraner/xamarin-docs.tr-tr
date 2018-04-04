---
title: Azure Mobile Apps kullanıcıların kimlik doğrulaması
description: Azure Mobile Apps Dış kimlik sağlayıcıları, çeşitli kimlik doğrulama ve uygulama kullanıcıları, Facebook, Google, Microsoft, Twitter ve Azure Active Directory dahil olmak üzere yetkilendirmek desteklemek üzere kullanır. İzinler, yalnızca kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için tablolarda ayarlanabilir. Bu makalede Azure Mobile Apps bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 5f5c69601c11a3c0d25bc804c60883841b0fb30d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Azure Mobile Apps kullanıcıların kimlik doğrulaması

_Azure Mobile Apps Dış kimlik sağlayıcıları, çeşitli kimlik doğrulama ve uygulama kullanıcıları, Facebook, Google, Microsoft, Twitter ve Azure Active Directory dahil olmak üzere yetkilendirmek desteklemek üzere kullanır. İzinler, yalnızca kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için tablolarda ayarlanabilir. Bu makalede Azure Mobile Apps bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Azure Mobile Apps bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek sahip işlemi aşağıdaki gibidir:

1. Bir kimlik sağlayıcısının sitesinde Azure mobil uygulamanızı kaydetmenizi ve Mobile Apps arka uç sağlayıcısı tarafından oluşturulan kimlik bilgilerini ayarlayın. Daha fazla bilgi için bkz: [kimlik doğrulaması için uygulamanızı kaydetme ve uygulama hizmetleri yapılandırma](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Kimlik doğrulama işlemi tamamlandıktan sonra geri Xamarin.Forms uygulamaya yeniden yönlendirmek kimlik doğrulama sistemi veren Xamarin.Forms uygulamanız için yeni bir URL şemasına tanımlayın. Daha fazla bilgi için bkz: [izin dış yönlendirme URL'si için uygulamanızı ekleme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Azure Mobile Apps arka ucuna istemcileri kimlik doğrulaması yalnızca kısıtlayın. Daha fazla bilgi için bkz: [kimliği doğrulanmış kullanıcılar için izinleri kısıtla](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Xamarin.Forms uygulaması kimlik doğrulamasını çağırır. Daha fazla bilgi için bkz: [taşınabilir sınıf kitaplığı kimlik doğrulaması ekleme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [iOS uygulaması için kimlik doğrulaması ekleme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [Android uygulaması için kimlik doğrulaması ekleme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)ve [ Windows 10 uygulaması projeleri için kimlik doğrulaması ekleme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

Tarihsel olarak, mobil uygulamalar, kimlik sağlayıcısının ile kimlik doğrulaması gerçekleştirmek için katıştırılmış web görünümleri kullandınız. Bu, artık aşağıdaki nedenlerle önerilir:

- Web görünümü barındıran uygulama, kullanıcının tam kimlik doğrulama kimlik bilgileri, yalnızca uygulama için hedeflenen yetkilendirme verme erişebilir. Bu, olası saldırı yüzeyini uygulamanın artan gerektirdiğinden daha daha güçlü kimlik bilgileri uygulama erişimi gibi bu en az ayrıcalık ilkesini ihlal ediyor.
- Ana bilgisayar uygulamasını kullanıcı adları ve parolalar yakalama, otomatik olarak form gönderme ve kullanıcı izni atlama ve oturum tanımlama bilgileri kopyalayın ve bunları kullanıcı olarak kimliği doğrulanmış eylemleri gerçekleştirmek için kullanın.
- Katıştırılmış web görünümleri, diğer uygulamalar veya cihazın web tarayıcısının ast kullanıcı deneyimi olarak kabul edilen her Yetkilendirme isteği için oturum açmak kullanıcının gerektiren, kimlik doğrulama durumuna paylaşmayın.
- Bazı yetkilendirme uç noktaları algılamak ve web görünümleri gelen yetkilendirme isteklerini engellemek için adımları uygulayın.

En son sürümü tarafından uygulanan yaklaşıma olan kimlik doğrulaması yapmak için cihazın web tarayıcısının kullanmaya alternatiftir [Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). Kullanıcılar yalnızca oturum dönüştürme oranları uygulamada oturum açma ve yetkilendirme akışı geliştirme kimlik sağlayıcısı her aygıt için bir kez açmak gereken kimlik doğrulama istekleri için cihaz tarayıcı kullanarak bir uygulamanın kullanılabilirliğini artırır. Uygulamaları inceleyin ve bir web görünümü içeriğinde, ancak tarayıcı'da gösterilen içeriği değiştirmek mümkün olduğu gibi cihaz tarayıcı de Gelişmiş güvenlik sağlar.

## <a name="using-an-azure-mobile-apps-instance"></a>Azure Mobile Apps örneğini kullanma

[Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) sağlar `MobileServiceClient` bir Xamarin.Forms uygulaması tarafından Azure Mobile Apps örneğine erişmek için kullanılan sınıf.

Örnek uygulama Google Xamarin.Forms uygulaması için oturum açma için Google hesaplarıyla kullanıcılara kimlik sağlayıcısı kullanır. Bu makalede kimlik sağlayıcısı Google kullanılırken, diğer kimlik sağlayıcılardan eşit geçerli bir yaklaşımdır.

<a name="logging-in" />

### <a name="logging-in-users"></a>Kullanıcılar oturum

Örnek uygulama oturum açma ekranında aşağıdaki ekran görüntülerinde gösterilir:

![](azure-images/login.png "Oturum açma sayfası")

Google kimlik sağlayıcısı olarak kullanılırken, Facebook, Microsoft, Twitter ve Azure Active Directory gibi diğer kimlik sağlayıcılardan çeşitli kullanılabilir.

Aşağıdaki kod örneği, oturum açma işlemini nasıl çağrıldığını gösterir:

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

`App.Authenticator` Özelliği bir `IAuthenticate` her platforma özgü projesi Ayarla örneği. `IAuthenticate` Arabirimi belirtir bir `AuthenticateAsync` her platform projesi tarafından sağlanması gereken işlemi. Bu nedenle, çağırma `App.Authenticator.AuthenticateAsync` yöntemini yürütür `IAuthenticate.AuthenticateAsync` platform projesinde yöntemi.

Tüm platform `IAuthenticate.AuthenticateAsync` yöntem çağrısı `MobileServiceClient.LoginAsync` oturum açma arabirimi ve önbellek verilerini görüntülemek için yöntem. Aşağıdaki örnekte gösterildiği kod `LoginAsync` yöntemi iOS platformu için:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Aşağıdaki örnekte gösterildiği kod `LoginAsync` yöntemi Android platformu için:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Aşağıdaki örnekte gösterildiği kod `LoginAsync` yöntemi Evrensel Windows platformu için:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Tüm platformlarda `MobileServiceAuthenticationProvider` numaralandırma, kimlik doğrulama işleminde kullanılacak kimlik sağlayıcısı belirtmek için kullanılır. Zaman `MobileServiceClient.LoginAsync` yöntemi çağrıldığında, Azure Mobile Apps için seçilen sağlayıcıya oturum açma sayfasını görüntüler ve kimlik sağlayıcısı ile başarılı oturum açma işleminden sonra kimlik doğrulama belirtecini oluşturmak tarafından bir kimlik doğrulama akışı başlatır. `MobileServiceClient.LoginAsync` Yöntemi döndürür bir `MobileServiceUser` depolanır örneği `MobileServiceClient.CurrentUser` özelliği. Bu özellik sağlar `UserId` ve `MobileServiceAuthenticationToken` özellikleri. Bu, kimliği doğrulanmış kullanıcı ve kullanıcı için bir kimlik doğrulama belirteci temsil eder. Kimlik doğrulama belirteci kimliği doğrulanmış kullanıcı izinleri gerektiren Azure mobil uygulaması örneğinde eylemleri gerçekleştirmek Xamarin.Forms uygulaması izin vererek Azure Mobile Apps örneği yapılan tüm istekleri eklenecektir.

<a name="logging-out" />

### <a name="logging-out-users"></a>Kullanıcıların oturumunu günlüğe kaydetme

Aşağıdaki kod örneği, oturum kapatma işlemini nasıl çağrıldığını gösterir:

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

`App.Authenticator` Özelliği bir `IAuthenticate` tarafından her platformproject ayarlama örneği. `IAuthenticate` Arabirimi belirtir bir `LogoutAsync` her platform projesi tarafından sağlanması gereken işlemi. Bu nedenle, çağırma `App.Authenticator.LogoutAsync` yöntemini yürütür `IAuthenticate.LogoutAsync` platform projesinde yöntemi.

Tüm platform `IAuthenticate.LogoutAsync` yöntem çağrısı `MobileServiceClient.LogoutAsync` kimlik sağlayıcısı ile oturum açmış kullanıcının XML'deki kimliğini doğrulamak için yöntem. Aşağıdaki örnekte gösterildiği kod `LogoutAsync` yöntemi iOS platformu için:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Aşağıdaki örnekte gösterildiği kod `LogoutAsync` yöntemi Android platformu için:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Aşağıdaki örnekte gösterildiği kod `LogoutAsync` yöntemi Evrensel Windows platformu için:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Zaman `IAuthenticate.LogoutAsync` yöntemi çağrıldığında, kimlik sağlayıcısı tarafından ayarlanmış hiçbir tanımlama bilgisi, önce temizlenir `MobileServiceClient.LogoutAsync` yöntemi, oturum açmış kullanıcının kimlik sağlayıcısı ile XML'deki kimliğini doğrulamak için çağrılır.

## <a name="summary"></a>Özet

Bu makalede Azure Mobile Apps bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmıştır. Azure Mobile Apps Dış kimlik sağlayıcıları, çeşitli kimlik doğrulama ve uygulama kullanıcıları, Facebook, Google, Microsoft, Twitter ve Azure Active Directory dahil olmak üzere yetkilendirmek desteklemek üzere kullanır. `MobileServiceClient` Sınıfı, Azure Mobile Apps örneğine erişimi denetlemek için Xamarin.Forms uygulaması tarafından kullanılır.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoAzureAuth (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Bir Azure mobil uygulaması kullanma](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Xamarin.Forms uygulamanıza kimlik doğrulaması ekleme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
