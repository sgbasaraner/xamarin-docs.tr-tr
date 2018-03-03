---
title: "Bir RESTful Web hizmeti kimlik doğrulama"
description: "HTTP kaynaklara erişimi denetlemek için birden fazla kimlik doğrulama mekanizmaları kullanımını destekler. Temel kimlik doğrulaması doğru kimlik bilgilerine sahip bu istemcilere kaynaklara erişim sağlar. Bu makalede, temel kimlik doğrulaması RESTful web hizmeti kaynaklarına erişimi korumak için nasıl kullanılacağı gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 7aea74f95e8738cc415eaac3a5ac4f86b069d0f7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-a-restful-web-service"></a>Bir RESTful Web hizmeti kimlik doğrulama

_HTTP kaynaklara erişimi denetlemek için birden fazla kimlik doğrulama mekanizmaları kullanımını destekler. Temel kimlik doğrulaması doğru kimlik bilgilerine sahip bu istemcilere kaynaklara erişim sağlar. Bu makalede, temel kimlik doğrulaması RESTful web hizmeti kaynaklarına erişimi korumak için nasıl kullanılacağı gösterilmektedir._

Eşlik eden Xamarin.Forms örnek uygulaması web hizmeti salt okunur erişim sağlayan bir Xamarin barındırılan REST hizmeti kullanır. Bu nedenle, oluşturmak, güncelleştirmek ve verileri silme işlemleri uygulamada kullanılan verileri değiştirmez. Ancak, REST hizmeti barındırılabilir bir sürümü kullanılabilir *TodoRESTService* örnek uygulama ve hizmet ayarlama yönergeleri klasöründe var. bulunabilir. REST hizmeti barındırılabilir bu sürümü tam oluşturma, güncelleştirme, okuma ve silme verilere erişim sağlar.

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>Kullanıcıların HTTP üzerinden kimlik doğrulaması

Temel kimlik doğrulaması HTTP tarafından desteklenen en basit kimlik doğrulama mekanizmasıdır ve kullanıcı adı ve parola şifrelenmemiş base64 ile kodlanmış metin olarak gönderme istemci ilgilidir. Şu şekilde çalışır:

- Bir web hizmeti korumalı bir kaynak için bir istek alırsa, HTTP durum kodunu 401 (erişim reddedildi) isteği reddeder ve aşağıdaki çizimde gösterildiği gibi WWW-Authenticate yanıt üstbilgisini ayarlar:

![](rest-images/basic-authentication-fail.png "Temel kimlik doğrulaması başarısız")

- Bir web hizmeti ile korunan bir kaynağa için bir istek alırsa, `Authorization` üstbilgi doğru şekilde ayarlanması, istek başarılı olduğunu gösteren web hizmeti yanıt bir HTTP durum kodu 200 ' ile ve istenen bilgileri içinde yanıt. Bu senaryo, aşağıdaki çizimde gösterilmiştir:

![](rest-images/basic-authentication-success.png "Temel kimlik doğrulaması başarılı")

> [!NOTE]
> Temel kimlik doğrulaması yalnızca HTTPS bağlantısı üzerinden kullanılmalıdır. Bir HTTP bağlantısı üzerinden kullanıldığında <code>Authorization</code> üstbilgisi kolayca kodunu çözdü HTTP trafiği bir saldırgan tarafından yakalanmış olması durumunda.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Temel kimlik doğrulaması Web isteğinde belirtme

Temel kimlik doğrulaması gibi belirtilir:

1. "Temel" eklenen dize `Authorization` isteği üstbilgisi.
1. Kullanıcı adı ve parola base64 ile kodlanmış ve eklenir ise "kullanıcıadı" biçiminde bir dizeye birleştirilir `Authorization` isteği üstbilgisi.

Bu nedenle, bir kullanıcı adı 'XamarinUser' ve 'XamarinPassword' bir parola ile üstbilgi olur:

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient` Sınıfı ayarlayabilirsiniz `Authorization` üstbilgi değeri `HttpClient.DefaultRequestHeaders.Authorization` özelliği. Çünkü `HttpClient` örnek var. birden çok istekte `Authorization` üstbilgi yalnızca bir kez yerine ne zaman ayarlanması gerekiyor aşağıdaki kod örneğinde gösterildiği gibi her isteği yapan:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    client = new HttpClient ();
    ...
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

Bir web hizmeti işlemi için bir istek yapıldığında istek imzalandığı sonra `Authorization` kullanıcının işlemini çağırmak için izne sahip olup olmadığını belirten üstbilgi.

> [!NOTE]
> Örnek REST hizmeti kimlik bilgileri sabitleri depolarken bunlar yayımlanmış bir uygulamanın güvenli biçimde depolanması. [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet kimlik bilgileri güvenli bir şekilde depolamak için işlevsellik sağlar. Daha fazla bilgi için bkz: [saklama ve hesap bilgilerini cihazlarda](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="processing-the-authorization-header-server-side"></a>Yetkilendirme üstbilgisi sunucu tarafı işleme

Her bir eylemde eşlik eden örnek REST hizmeti süsler `[BasicAuthentication]` özniteliği. Bu öznitelik tarafından uygulanan `BasicAuthenticationAttribute` sınıf çözümde ve ayrıştırmak için kullanılan `Authorization` üstbilgi ve depolanan değerlerle karşılaştırarak base64 ile kodlanmış kimlik bilgileri geçerli olup olmadığını belirlemek *Web.config*. Bu yaklaşım örnek hizmeti için uygun olsa da, genel kullanıma yönelik web hizmeti genişletme gerektirir.

IIS tarafından kullanılan temel kimlik doğrulaması modülünde kullanıcılar kendi Windows kimlik bilgilerine karşı kimlik doğrulaması yapılır. Bu nedenle, kullanıcı hesaplarının sunucunun etki alanında olması gerekir. Ancak, temel kimlik doğrulama modeli, özel kimlik doğrulama, bir veritabanı gibi bir dış kaynağa göre kullanıcı hesaplarını burada kimlik doğrulamalarının izin vermek için yapılandırılabilir. Daha fazla bilgi için bkz: [ASP.NET Web API temel kimlik doğrulaması](http://www.asp.net/web-api/overview/security/basic-authentication) ASP.NET Web sitesi.

> [!NOTE]
> Temel kimlik doğrulaması günlüğü yönetmek için tasarlanmamıştır. Bu nedenle, günlük için standart temel kimlik doğrulaması oturumu sonlandırmak için yaklaşımdır.

## <a name="summary"></a>Özet

Bu makalede bir Xamarin.Forms uygulaması kullanılarak yapılan web istekleri için temel kimlik doğrulaması ekleme gösterilen `HttpClient` sınıfı. Temel kimlik doğrulaması doğru kimlik bilgilerine sahip bu istemcilere kaynaklara erişim sağlar. Nasıl kullanılacağı hakkında bilgi için [Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) kullanıcıların bir arka uç verilerine erişimi yalnızca yaparken paylaşmanızı sağlayan bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için bkz: [kullanıcıların kimlik doğrulaması bir kimlik sağlayıcısı ile](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="related-links"></a>İlgili bağlantılar

- [TodoREST (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [Bir RESTful web hizmetini kullanma](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
