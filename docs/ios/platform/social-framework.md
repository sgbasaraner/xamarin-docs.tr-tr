---
title: Sosyal Framework
description: "Sosyal Framework Çin'de kullanıcıları için SinaWeibo yanı sıra Twitter ve Facebook dahil olmak üzere sosyal ağlar ile etkileşmek için birleştirilmiş bir API sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: da096c8575896bc9f522a92b3fb94b81f9e772df
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="social-framework"></a>Sosyal Framework

_Sosyal Framework Çin'de kullanıcıları için SinaWeibo yanı sıra Twitter ve Facebook dahil olmak üzere sosyal ağlar ile etkileşmek için birleştirilmiş bir API sağlar._


Sosyal Framework kullanarak uygulamaların tek API'sinden sosyal ağlarla kimlik doğrulamasını yönetmek zorunda kalmadan etkileşime izin verir. Görünüm denetleyicisini gönderileri yanı sıra her sosyal ağ API HTTP üzerinden tüketen izin veren bir Özet oluşturma için sağlanan bir sistem içerir.

> [!IMPORTANT]
> **Not:** bir platformlar arası API çeşitli sosyal ağlara bağlanmak, bkz: [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) Xamarin bileşen Deposu'nda bileşen.

## <a name="connecting-to-twitter"></a>Twitter hesabına bağlanma

### <a name="twitter-account-settings"></a>Twitter hesap ayarları

Sosyal Framework kullanarak Twitter bağlanmak için bir hesap aygıt ayarlarını aşağıda gösterildiği gibi yapılandırılması gerekir:

 [ ![](social-framework-images/twitter01.png "Twitter hesap ayarları")](social-framework-images/twitter01.png)

Bir hesap girdikten ve Twitter ile doğruladıktan sonra sosyal Framework sınıfları Twitter erişmek için kullandığı bir cihazda herhangi bir uygulama bu hesabı kullanın.

### <a name="sending-tweets"></a>Tweet'leri gönderme

Adlı bir denetleyici sosyal Framework içerir `SLComposeViewController` düzenleme ve tweet göndermek için sistem tarafından sağlanan bir görünüm sunar. Aşağıdaki ekran görüntüsünde, bu görünüm örneği gösterilmektedir:

 [ ![](social-framework-images/twitter02.png "Bu ekran SLComposeViewController örneği gösterilmektedir.")](social-framework-images/twitter02.png)

Kullanılacak bir `SLComposeViewController` çağırarak denetleyici örneği Twitter ile oluşturulmalıdır `FromService` yöntemiyle `SLServiceType.Twitter` aşağıda gösterildiği gibi:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

Sonra `SLComposeViewController` örneği döndürülür, Twitter hesabına göndermek için bir Arabirim sunmak için kullanılabilir. Ancak, çağırarak bu durumda, Twitter sosyal ağ kullanılabilirliğini denetlemek için yapılacak ilk şey olduğunu `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` hiçbir kullanıcı etkileşimi olmadan doğrudan tweet gönderir. Ancak, aşağıdaki yöntemlerle başlatılabilir:

-   `SetInitialText` – Tweet göstermek için ilk metin ekler. 
-  `AddUrl` – Bir Url tweet ekler.
-  `AddImage` – Tweet bir görüntü ekler.


Başlatıldıktan sonra arama `PresentVIewController` tarafından oluşturulan görünüm görüntüler `SLComposeViewController`. Kullanıcı sonra isteğe bağlı olarak düzenleyebilir ve tweet gönderecek veya göndermeden iptal edin. Her iki durumda da, denetleyici oluşturmayabilir `CompletionHandler`, burada sonucu da aşağıda gösterildiği gibi tweet gönderilen veya iptal edildi, olmadığını görmek için denetlenebilir:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Tweet örneği

Aşağıdaki kodu kullanarak gösteren `SLComposeViewController` tweet göndermek için kullanılan bir görünümünü sunmak için:

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Twitter API'si çağırma

Sosyal Framework sosyal ağlara HTTP isteklerini yapmak için destek de içerir. İstekte yalıtan bir `SLRequest` belirli sosyal ağ API hedeflemek için kullanılan sınıfı.

Örneğin, aşağıdaki kodu ortak zaman çizelgesi (yukarıda verilen kodu genişleterek) almak için Twitter istek yapar:

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access 
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Bu kod ayrıntılı bakalım. İlk olarak, hesap deposu erişim kazanır ve bir Twitter hesabı türünü alır:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Ardından, uygulamanız kendi Twitter hesabına erişebilir ve erişim verilirse, hesap bellek ve güncelleştirilmiş UI yüklenir, kullanıcıya sorar:

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

Kullanıcının (kullanıcı arabiriminde bir düğme dokunarak) zaman çizelgesi veri istediğinde, uygulamanın ilk Twitter'dan verilere erişmek için bir istek oluşturur:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
Bu örnekte son on girişleri döndürülen sonuçları dahil ederek sınırlama `?count=10` URL. Son olarak, (yukarıda yüklendi) Twitter hesabına istek ekler ve veri getirmek için Twitter çağrısı gerçekleştirir:

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

Verilerin başarıyla yüklendiğini ham JSON verilerini (örnekte olduğu gibi aşağıdaki çıktıda) görüntülenir:

[ ![](social-framework-images/twitter03.png "Ham JSON veri görünümü örneği")](social-framework-images/twitter03.png)

Gerçek bir uygulamada, JSON sonuçları sonra kullanıcıya sunulan sonuçları ve normal olarak ayrıştırılması. Bkz: [giriş Web Hizmetleri](~/cross-platform/data-cloud/web-services/index.md) JSON ayrıştırma hakkında bilgi için.

## <a name="connecting-to-facebook"></a>Facebook hesabına bağlanma

### <a name="facebook-account-settings"></a>Facebook hesap ayarları

Facebook sosyal Framework ile bağlanma neredeyse yukarıda gösterilen Twitter için kullanılan işlem aynıdır. Bir Facebook kullanıcı hesabı, aşağıda gösterildiği gibi aygıt ayarlarını yapılandırılmalıdır:

[ ![](social-framework-images/facebook01.png "Facebook hesap ayarları")](social-framework-images/facebook01.png)

Bir kere yapılandırıldığında, sosyal çerçevesi kullanır, bu aygıttaki herhangi bir uygulama için Facebook bağlanmak için bu hesabı kullanır.

### <a name="posting-to-facebook"></a>Facebook için gönderme

Sosyal Framework birden fazla sosyal ağlara erişim için tasarlanmış bir birleşik API'dir gibi kod kullanılan sosyal ağ bağımsız olarak neredeyse aynı kalır.

Örneğin, `SLComposeViewController` tam olarak yalnızca farklı geçiş Facebook özgü ayarları ve seçenekleri için daha önce gösterilen Twitter örnekte olduğu gibi kullanılabilir. Örneğin:

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

Facebook ile kullanıldığında `SLComposeViewController` gösteren Twitter örnek için neredeyse aynı benzeyen bir görünüm görüntüler **Facebook** başlığı bu durumda:

[ ![](social-framework-images/facebook02.png "SLComposeViewController görüntüleme")](social-framework-images/facebook02.png)

### <a name="calling-facebook-graph-api"></a>Facebook grafik API'si çağırma

Twitter örneğe benzer, sosyal Framework's `SLRequest` nesne Facebook'ın grafik API'si ile kullanılabilir. Örneğin, aşağıdaki kod bilgileri Xamarin hesabı hakkında grafik API'si (yukarıda verilen kodu genişleterek) verir:

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access 
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Yalnızca gerçek bu kodu ve yukarıda gösterilen Twitter sürümü arasındaki farktır hangi isteği yapılırken bir seçenek olarak ayarlanması gerekir (Facebook'ın Geliştirici portalından oluşturabilir) bir geliştirici/uygulama belirli Kimliğini almak için Facebook'ın gereksinimi:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Bu seçenek (veya geçersiz bir anahtar kullanarak) ayarlanamadı, bir hata veya döndürülen veri neden olur.

## <a name="summary"></a>Özet

Bu makalede, sosyal Framework Twitter ve Facebook ile etkileşim kurmak için nasıl kullanılacağı gösterilmiştir. Her bir sosyal ağ hesaplarını aygıt ayarlarını yapılandırmak nereye gösterdi. Ayrıca nasıl kullanılacağını ele alınan `SLComposeViewController` nakil sosyal ağlar için birleştirilmiş bir görünümünü sunmak için. Ayrıca, denetlenen `SLRequest` her sosyal ağ API'sini çağırmak için kullanılan sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [SocialFrameworkDemo (örnek)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Web hizmetlerine giriş](~/cross-platform/data-cloud/web-services/index.md)
