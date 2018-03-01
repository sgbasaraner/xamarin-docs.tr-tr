---
title: "İOS 11 Web değişiklikleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: 224a64b39f9761ca520117a23dfeb7ee08cae719
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="web-changes-in-ios-11"></a>İOS 11 Web değişiklikleri

iOS 11 WebKit ve SafariServices değişiklikler içeren Safari web tarayıcısı – Safari 11.0 – yeni bir sürümünü kullanıma sunmaktadır. Bu değişiklikler bu kılavuzda araştırır.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` iOS 9, web içeriği görüntüleme veya uygulamanızdan kullanıcıların kimlik doğrulaması için bir seçenek olarak sunulmuştur. Özellikleri hakkında daha fazla bilgi bulunabilir [Web görünümleri](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) Kılavuzu.

iOS 11 stili güncelleştirmeleri kullanıcılarınızın bir uygulama ve web arasında daha sorunsuz bir deneyim vermiş Safari View Controller giriş. Örneğin, adres çubuğuna şimdi verir Safari View Controller kısa bir tarayıcı yerine bir uygulama tarayıcı yapısını kaldırma. Uygulamanızı renk düzenini oturum ayarlayarak sığması için renk düzenini özelleştirebilirsiniz `preferredBarTintColor` ve `PreferredControlTintColor` özellikleri:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Aşağıdaki kod parçacığını aşağıdaki görüntüde gösterildiği mor ve beyaz çubuklarında işler:

![İşlenen mor ve beyaz SFSafariViewController çubukları](web-images/image1.png)

Safari görünüm denetleyicisini sunulan kapatma düğmesini ayarlayarak da değiştirilebilir `DismissButtonStyle` ya da özellik `Done`, `Close`, veya `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Düğme metni değiştirilen kapatın](web-images/image2.png)

Bu değer değiştirilebilir sırada `SFSafariViewController` sunulur.


Bir Safari View Controller içinde görüntülenir içeriğe bağlı olarak menü çubukları kullanıcı kayarken Daralt yok emin olmak gerekli olabilir. Bu yeni ayarlayarak etkin `BarCollapsedEnabled` özelliğine `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Devre dışı daraltma çubuğu](web-images/image3.png)

Apple iOS 11 Safari görünüm denetleyiciye gizlilik için güncelleştirmeleri ayrıca sunmuştur. Şimdi, yalnızca tanımlama bilgileri ve yerel depolama Safari görünüm denetleyicisini tüm örneklerini üzerinden değil, uygulama başına temelinde mevcut gibi veri tarama. Bu kullanıcı gözatma etkinliği, uygulamanızın içinde özel tutar.

Ek özellikler gibi sürükleme ve bırakma URL'ler için destek ve desteği `window.open()` de için eklenene `SFSafariViewController` iOS 11 içinde. Bu yeni özellikler hakkında daha fazla bilgi bulabilirsiniz [Apple SFSafariViewController belgeleri](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>Web Kiti

`WKWebView` web içeriği, kullanıcı görüntüleme, iOS 8 bir yol olarak WebKit parçası olarak sunulmuştur. Daha fazla özelleştirilebilir `SFSafariViewController`, sizin kendi gezinti ve kullanıcı arabirimi oluşturmanıza izin verir.

Apple için üç ana geliştirmeleri kullanıma sunulan `WKWebView` iOS 11: 

- Tanımlama bilgilerini yönetme olanağı
- İçerik filtreleme
- Özel kaynak yükleniyor. 

Tanımlama bilgisi yönetim yeni yapılır [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) ekleyin ve tanımlama bilgileri, bir WKWebView depolanan tüm tanımlama bilgilerini almak için ve değişiklikleri saklamak tanımlama bilgisi izlemek için silmenize olanak sağlayan sınıf.

İçerik filtreleme sağlar, kullanıcının göreceği, içerik türü yönetmek, güvenli, aile kolay olduğundan emin olmak izin verme ve gerekirse, yalnızca kullanıcı grubunu seçmek için kullanılabilir. Bu yeni uygulanan [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) tetikleyiciler ve Eylemler JSON içinde çiftleri sağlayarak sınıfı. Apple içinde bu tetikleyiciler ve eylemler hakkında daha fazla bilgi bulunabilir [içerik engelleme kuralları](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) Kılavuzu.

iOS 11 şimdi özelleştirmenizi sağlar `WKWebView` web içeriğinize yüklenirken özel kaynakla. Bu aracılığıyla uygulanır `IWKUrlSchemeHandler` Web pakete yerel olmayan bir URL şemalarını işlemenize olanak sağlayan arabirim. Bu arabirim uygulanmalı başlatma ve durdurma yöntemi vardır:

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

İşleyici uygulandıktan sonra ayarlamak için kullanın `SetUrlSchemeHandler` özelliği `WKWebViewConfiguration`. Sonra özel şema kullanan URL, bir şeyin yükleyin:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

