---
title: İOS 11'deki Web değişiklikleri
description: Bu belge WebKit ve Safari Hizmetleri framework iOS 11'deki yapılan değişiklikleri açıklar. Bunu SFSafariViewController güncelleştirmeleri ve yeni özellikleri WKWebView stil ile çalışmaya nasıl açıklar.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2017
ms.openlocfilehash: 00587e3b49e953b780a49623f081ae798e81fa61
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351463"
---
# <a name="web-changes-in-ios-11"></a>İOS 11'deki Web değişiklikleri

iOS 11 WebKit ve SafariServices değişiklikler içeren Safari web tarayıcısı – Safari 11.0 – yeni bir sürümünü sunmaktadır. Bu kılavuz, bu değişiklikleri keşfediyor.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` iOS 9 web içeriği görüntüleme veya uygulamanızdaki kullanıcıların kimliklerini doğrulamak için bir seçenek olarak sunulmuştur. Özellikleri hakkında daha fazla bilgi bulunabilir [Web görünümleri](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) Kılavuzu.

iOS 11 uygulama ve web arasında daha sorunsuz bir deneyim, kullanıcılara dair Safari görünüm denetleyicisi için stil güncelleştirmeleri sundu. Örneğin, adres çubuğuna şimdi size kısa bir tarayıcı yerine bir uygulama içi tarayıcıyı yapısını Safari görünüm denetleyicisi kaldırma. Renk düzenini ayarlayarak uygulamanızın renk düzenini oturum uyacak şekilde özelleştirebilirsiniz `preferredBarTintColor` ve `PreferredControlTintColor` özellikleri:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Aşağıdaki kod parçacığı, aşağıdaki görüntüde gösterildiği mor ve beyaz çubuklarında işler:

![Mor ve teknik işlenen SFSafariViewController çubukları](web-images/image1.png)

Safari görünüm denetleyicisi sunulan Kapat düğmesi ayarlayarak da değiştirilebilir `DismissButtonStyle` ya da özellik `Done`, `Close`, veya `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Kapat düğmesi metin değiştirildi](web-images/image2.png)

Bu değer değiştirilebilir sırada `SFSafariViewController` sunulur.


Safari görünüm denetleyicisi içinde görüntülenen içeriğe bağlı olarak menü çubukları ve kullanıcı sonuçlarda Daralt yoksa emin olmak gerekli olabilir. Bu yeni ayarlayarak etkin `BarCollapsedEnabled` özelliğini `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Devre dışı daraltma çubuğu](web-images/image3.png)

Apple iOS 11'deki Safari görünüm denetleyicisi gizlilik için güncelleştirmeleri de yapılmıştır. Şimdi, gibi tanımlama bilgilerini ve yerel depolama yalnızca Safari görünüm denetleyicisi tüm örnekleri arasında değil, uygulama başına temelinde mevcut veri göz atma. Bu kullanıcı gözatma etkinliği uygulamanızın içinde özel durumda tutar.

Ek özellikler gibi sürükleyin ve bırakın URL'ler için destek ve desteği `window.open()` için eklenmiştir `SFSafariViewController` iOS 11'deki. Yeni özellikler hakkında daha fazla bilgi bulabilirsiniz [Apple'nın SFSafariViewController belgeleri](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` web içeriği görüntüleme, kullanıcıya iOS 8 olarak WebKit parçası olarak kullanıma sunulmuştur. Daha fazla özelleştirilebilir `SFSafariViewController`, kendi gezinme ve kullanıcı arabirimi oluşturma etmenize imkan sağlar.

Apple için üç ana geliştirmeleri kullanıma sunmuştu `WKWebView` iOS 11: 

- Tanımlama bilgilerini yönetme olanağı
- İçerik filtreleme
- Özel kaynak yükleniyor. 

Tanımlama bilgisi yönetim yeni yapılır [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) ekleme ve tanımlama bilgileri, bir WKWebView içinde depolanan tüm tanımlama bilgilerini almak ve depolamak için değişiklikleri tanımlama bilgisi gözlemlemek için silme sağlar sınıfını.

İçerik filtreleme sayesinde, kullanıcının göreceği, içerik türünü yönetmek güvenli, aile kolay olduğundan emin olun etmenize imkan sağlar ve gerekirse, yalnızca kullanıcılar grubunu seçmek için kullanılabilir. Bu yeni uygulanır [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) tetikleyiciler ve Eylemler json'da çiftleri sağlayarak sınıfı. Apple bu tetikleyiciler ve eylemler hakkında daha fazla bilgi bulunabilir [içerik engelleme kuralları](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) Kılavuzu.

iOS 11 verir özelleştirmek `WKWebView` web içeriğiniz için özel kaynak ile. Bu aracılığıyla uygulanır `IWKUrlSchemeHandler` arabirimi, Web Paketi için yerel olmayan bir URL şemalarını sağlar. Bu arabirim, uygulanması gereken bir başlatma ve durdurma yöntemi vardır:

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

İşleyici uygulandıktan sonra ayarlamak için kullanın `SetUrlSchemeHandler` özellikte `WKWebViewConfiguration`. Ardından, özel düzenini kullanan URL bir şeyin yükleyebilirsiniz:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

