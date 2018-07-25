---
title: Xamarin.iOS Web görünümleri
description: Bu belge, bir Xamarin.iOS uygulaması, web içeriği görüntüleyebilir çeşitli yolları açıklar. UIWebView, WKWebView, SFSafariViewController, Safari ve uygulama taşıma güvenliği ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 56b0f0e910cc95ca50d1e5e460ce71a1c8f669a2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242046"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.iOS Web görünümleri

İOS ömrü boyunca Apple web görünümü işlevselliğini uygulamalarında birleştirmek uygulama geliştiricileri için çeşitli yollarla kullanıma sundu. Çoğu kullanıcı iOS cihazlarında yerleşik Safari web tarayıcısı kullanın ve bu nedenle diğer uygulamalardan web görünümü işlevi bu deneyim ile tutarlı olmasını bekler. Aynı hareketleri çözmek için değer ve işlevselliği aynı olacak şekilde performans beklerler.

Bu makalede, biz Apple tarafından sağlanan üç Web görünümlerin her birine inceleyeceksiniz: `UIWebView`, `WKWebview`, ve `SFSafariViewController`, kendi benzerlikleri ve farkları ve nasıl kullanılabilirler. 

iOS 11 hem de yeni değişiklikler sunulan `WKWebView` ve `SFSafariViewController`. Bunlar hakkında daha fazla bilgi için bkz: [Web iOS 11 Kılavuzu'nda değişiklikler](~/ios/platform/introduction-to-ios11/web.md) Kılavuzu.

## <a name="uiwebview"></a>UIWebView

`UIWebView` uygulamanıza web içeriği sağlama Apple'nın eski yoludur. İOS 2.0 yayımlanmıştır ve 8.0 itibarıyla kullanım dışıdır.

İOS sürüm 8.0 öncesi desteklemeyi planlıyorsanız, kullanması gerekir `UIWebView`. Bunun nedeni, `UIWebView` önerilir daha az optimize performans için alternatifler, kullanıcının iOS sürümünü denetlemelisiniz. Varsa, 8.0 veya üstüne kullanma seçeneklerinden birini açıklayan aşağıda daha iyi bir kullanıcı deneyimi oluşturur.
 
Bir UIWebView Xamarin.iOS uygulamanıza eklemek için aşağıdaki kodu kullanın:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Bu, aşağıdaki web görünümü oluşturur:

[![](uiwebview-images/webview.png "ScalesPagesToFit etkisi")](uiwebview-images/webview.png#lightbox)

Kullanma hakkında daha fazla bilgi için `UIWebView`, aşağıdaki tarifleri bakın:


- [Bir Web sayfası yükleme](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Yerel içeriği yükleme](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Web olmayan belge yükleme](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` bir web, mobil Safari benzer bir arabirim uygulamak için iOS 8 sağlayarak uygulama geliştiricileri sunulmuştur. Bu son parçası, olguya, `WKWebView` Nitro Javascript motoru, mobil Safari tarafından kullanılan aynı altyapısını kullanır. `WKWebView` her zaman kullanılması gerektiğini UIWebView olası üzerinden [artan performans](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), kullanıcı dostu hareketlerini ve web sayfası ve uygulamanız arasındaki etkileşimi kolaylığı yerleşik.
  
`WKWebView` Ancak, geliştirici olarak işlevselliği ve UI/UX çok daha fazla denetime sahip bir neredeyse aynı şekilde UIWebView, uygulamanıza eklenebilir. Görünüm gösterilme, kullanıcı nasıl gezinebileceğiniz ve nasıl görünümü kullanıcı çıkar kontrol edebilirsiniz ancak oluşturma ve görüntüleme web view nesnesinin istenen sayfa görüntülenir.  

Aşağıdaki kodu başlatmak için kullanılan bir `WKWebView` Xamarin.iOS uygulamanıza:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Bu, aşağıdaki web görünümü oluşturur:

[![](uiwebview-images/wkwebview.png "Örnek bir web görünümü ScalesPagesToFit olmadan")](uiwebview-images/wkwebview.png#lightbox)

Dikkat etmeniz önemlidir `WKWebView` WebKit ad alanında olduğundan bu using yönergesi, sınıfının en üstüne eklemeniz gerekir.

`WKWebView` Xamarin.Mac uygulamaları içinde de kullanılabilir ve bu nedenle bir platformlar arası Mac/iOS uygulaması oluşturuyorsanız bunu göz önünde bulundurmak isteyebilirsiniz.

[İşlemek JavaScript uyarılar](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) tarif ayrıca WKWebView Javascript ile kullanma hakkında bilgi sağlar

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` Web içeriği uygulamanızdan sağlamak için en son yoludur ve bulunan iOS 9 ve üzeri. Farklı `UIWebView` veya `WKWebView`, `SFSafariViewController` bir görünüm denetleyicisi ve diğer görünümlerle kullanılamaz. Sunması gerektiğini `SFSafariViewController` yeni bir görünüm denetleyicisi aynı şekilde, herhangi bir görünüm denetleyicisi neden olabilir.
 
 `SFSafariViewController` 'uygulamanıza eklenebilen aslında bir mini safari' dir. WKWebView gibi aynı Nitro Javascript motorunu kullanır, ancak çeşitli otomatik doldurma, okuyucu ve mobil Safari ile tanımlama bilgileri ve veri paylaşma olanağı gibi ek Safari özellikler de sağlar. Kullanıcı arasındaki etkileşimi ve `SFSafariViewController` uygulamanıza erişilebilir değil. Uygulamanız varsayılan Safari özelliklerin herhangi birine erişimi yoktur.
 
Ayrıca, varsayılan olarak uygulayan bir **Bitti** kolayca uygulamanıza dönün ve İleri ve geri gezinti düğmeleri, web sayfalarının bir yığın içinde gezinmek, kullanıcının kullanıcıya izin vererek düğme. Ayrıca, ayrıca kullanıcı adresi beklenen web sayfasında oldukları rahat vermeden çubuğu sağlar. Adres çubuğuna URL'yi değiştirmeniz kullanıcıya izin vermez. 

Bu uygulamalar değiştirilemez, bunu `SFSafariViewController` uygulamanızı bir Web sayfası özelleştirme olmadan sunmak istiyorsa, varsayılan tarayıcı olarak kullanmak idealdir.

Aşağıdaki kodu başlatmak için kullanılan bir `SFSafariViewController` Xamarin.iOS uygulamanıza:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Bu, aşağıdaki web görünümü oluşturur:

[![](uiwebview-images/sfsafariviewcontroller.png "SFSafariViewController ile örnek bir web görünümü")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Aşağıdaki kodu kullanarak uygulamanız içinde mobil Safari uygulamadan açmak mümkündür:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Bu, aşağıdaki web görünümü oluşturur:

[![](uiwebview-images/safari.png "Safari'de sunulan bir web sayfası")](uiwebview-images/safari.png#lightbox)

Kullanıcıların uygulamanıza Safari uzağa gezinme genellikle her zaman kaçınılmalıdır. Çoğu kullanıcı, uygulamanızın dışında Gezinti beklememeniz şekilde uygulamanızı uzağa giderseniz, kullanıcılar hiçbir zaman, temelde engagement sonlandırma döndürebilir.

iOS 9 geliştirmeleri Safari sayfanın sol üst köşesindeki sağlanan geri düğmesi aracılığıyla uygulamanıza kolayca döndürülecek verin.

## <a name="app-transport-security"></a>Uygulama aktarım güvenliği

Uygulama aktarım güvenliği veya *ATS* Apple tarafından iOS 9 tüm internet iletişimi güvenli bağlantı en iyi uyumlu olmasını sağlamak için sunulmuştur.

ATS hakkında daha fazla bilgi için uygulamanızda nasıl dahil olmak üzere başvurduğu [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) Kılavuzu.

## <a name="related-links"></a>İlgili bağlantılar

- [Web görünümleri (örnek)](https://developer.xamarin.com/samples/monotouch/WebView/)
