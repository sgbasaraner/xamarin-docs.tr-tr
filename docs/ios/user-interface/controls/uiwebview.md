---
title: "Web görünümleri"
description: "Belirsizliği iOS web görünümü seçenekleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 83e01bba82713b55a0a69858c2a83aa243f71588
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="web-views"></a>Web görünümleri

İOS ömrü boyunca çeşitli yollarla uygulamalarını web görünümü işlevlerini içerecek şekilde uygulama geliştiricileri için Apple yayımladı. Kullanıcıların çoğunun iOS cihazlarında yerleşik Safari web tarayıcısı kullanan ve bu nedenle diğer uygulamalardan web görünümü işlevi bu deneyim ile tutarlı olmasını bekler. Bunlar aynı hareketleri çalışmak için nominal ve işlevselliği aynı olması için performans bekler.

Bu makalede, sizi her Apple'nın sağladığı üç Web görünümlerinin inceleyeceksiniz: `UIWebView`, `WKWebview`, ve `SFSafariViewController`, kendi benzerlikler ve farklar ve bunların nasıl kullanılabilir. 

iOS 11 sunulan yeni değişiklikleri hem de `WKWebView` ve `SFSafariViewController`. Bunlar hakkında daha fazla bilgi için bkz: [Web iOS 11 Kılavuzu'ndaki değişiklikler](~/ios/platform/introduction-to-ios11/web.md) Kılavuzu.

## <a name="uiwebview"></a>UIWebView

`UIWebView` uygulamanızda web içeriği sağlama Apple'nın eski bir yoludur. İOS 2.0 yayımlanmıştır ve 8.0 sürümünden itibaren kullanım dışı bırakıldı.

8.0'den önceki iOS sürümlerini desteklemeyi planlıyorsanız, kullanması gerekir `UIWebView`. Due için nedeni olduğunu `UIWebView` olduğundan daha az iyileştirilmiş performans için alternatifleri önerilir kullanıcının iOS sürüm denetlemeniz gerekir. Varsa, 8.0 veya üstüne kullanarak seçeneklerden birini açıklayan altına daha iyi bir kullanıcı deneyimi oluşturacaktır.
 
Xamarin.iOS uygulamanızı bir UIWebView eklemek için aşağıdaki kodu kullanın:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Bu, aşağıdaki web görünümü oluşturur:

[ ![](uiwebview-images/webview.png "ScalesPagesToFit etkisi")](uiwebview-images/webview.png)

Kullanma hakkında daha fazla bilgi için `UIWebView`, aşağıdaki tarif bakın:


- [Bir Web sayfası yükleme](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [Yerel içerik yükleme](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [Web olmayan belge yükleme](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` bir web arabirimi, mobil Safari benzer tarama uygulamak için iOS 8 izin uygulama geliştiriciler sunulmuştur. Bu bulgu parçası olarak, son, `WKWebView` Nitro Javascript altyapısı, mobil Safari tarafından kullanılan aynı altyapısını kullanır. `WKWebView` her zaman kullanılması gereken UIWebView nedeniyle olası üzerinden [artan performans](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), web sayfası ve uygulamanız arasındaki etkileşim kolaylığı ve kullanıcı dostu hareketler yerleşik.
  
`WKWebView` Geliştirici olarak işlevselliği ve kullanıcı Arabirimi/UX üzerinde daha fazla denetime sahip ancak bir neredeyse aynı şekilde UIWebView, uygulamanıza eklenebilir. Görünümü gösterilme, kullanıcının nasıl gidebilir ve kullanıcı görünümü nasıl çıkar kontrol edebilirsiniz ancak oluşturma ve web görünümü nesnesi görüntüleme istenen sayfa görüntülenir.  

Aşağıdaki kodu başlatmak için kullanılan bir `WKWebView` Xamarin.iOS uygulamanızda:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Bu, aşağıdaki web görünümü oluşturur:

[ ![](uiwebview-images/wkwebview.png "ScalesPagesToFit olmayan bir örnek web görünümü")](uiwebview-images/wkwebview.png)

Bu dikkate almak önemlidir `WKWebView` WebKit ad alanında olduğundan bu using yönergesi sınıfının üstüne eklemeniz gerekir.

`WKWebView` Xamarin.Mac apps içinde de kullanılabilir ve bu nedenle, platformlar arası Mac/iOS uygulaması oluşturuyorsanız kullanmayı düşünün isteyebilirsiniz.

[İşlemek JavaScript uyarıları](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/) tarif de WKWebView Javascript ile kullanma hakkında bilgi sağlar

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` Web içerik uygulamanızdan sağlamak için en son yoludur ve bulunan iOS 9 ve üstü. Farklı `UIWebView` veya `WKWebView`, `SFSafariViewController` bir görünüm denetleyicisidir ve bu nedenle diğer görünümlerle kullanılamaz. Sunması gerektiğini `SFSafariViewController` yeni bir görünüm denetleyicisi aynı şekilde, herhangi bir görünüm denetleyicisini sunmak.
 
 `SFSafariViewController` 'ı uygulamanıza katıştırılmış aslında bir mini safari' dir. WKWebView gibi aynı Nitro Javascript altyapısını kullanır, ancak çeşitli otomatik doldurmaya, okuyucu ve tanımlama bilgileri ve verileri mobil Safari ile paylaşma olanağı gibi ek Safari özellikler de sağlar. Kullanıcı arasındaki etkileşimi ve `SFSafariViewController` uygulamanıza erişilebilir değil. Uygulamanız varsayılan Safari özelliklerin herhangi birinden erişebilir değil.
 
Ayrıca, varsayılan olarak, uygulayan bir **Bitti** kolayca, uygulamaya geri dönün ve İleri ve geri gezinti düğmeleri, web sayfaları yığınından gitmek, kullanıcının kullanıcıya izin vererek düğmesi. Ayrıca, ayrıca kullanıcı beklenen web sayfasında oldukları içiniz vermiş çubuğu adresiyle sağlar. Adres çubuğundaki URL'yi değiştirmek kullanıcı izin vermiyor. 

Bu uygulamalar değiştirilemez, bunu `SFSafariViewController` uygulamanızı bir Web sayfası hiçbir özelleştirme gerektirmeden sunmak isterse varsayılan tarayıcı olarak kullanmak idealdir.

Aşağıdaki kodu başlatmak için kullanılan bir `SFSafariViewController` Xamarin.iOS uygulamanızda:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Bu, aşağıdaki web görünümü oluşturur:

[ ![](uiwebview-images/sfsafariviewcontroller.png "Bir örnek web görünümü SFSafariViewController ile")](uiwebview-images/sfsafariviewcontroller.png)

## <a name="safari"></a>Safari

Aşağıdaki kodu kullanarak, uygulamanızın içinde mobil Safari uygulamayı açmak mümkündür:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Bu, aşağıdaki web görünümü oluşturur:

[ ![](uiwebview-images/safari.png "Safari ile sunulan bir web sayfası")](uiwebview-images/safari.png)

Kullanıcılar uygulamanıza Safari çıktığınızda gezinme genellikle her zaman kaçınılmalıdır. Kullanıcıların çoğunun, uygulamanızın dışında Gezinti beklememeniz uygulamanızı çıktığınızda giderseniz, kullanıcıların hiçbir zaman, aslında katılım sonlandırılması döndürebilir şekilde.

iOS 9 geliştirmeleri uygulamanızı Safari sayfanın sol üst köşesinde sağlanan bir geri düğmesini aracılığıyla kolayca dönmek verin.

## <a name="app-transport-security"></a>Uygulama taşıma güvenliği

Uygulama taşıma güvenliği veya *ATS* tarafından Apple iOS 9 tüm Internet iletişimlerini güvenli bağlantı en iyi yöntemler uyumlu olmasını sağlamak için sunulmuştur.

ATS hakkında daha fazla bilgi için uygulamanızda uygulamak da dahil olmak üzere başvurmak için [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) Kılavuzu.

## <a name="related-links"></a>İlgili bağlantılar

- [Web görünümleri (örnek)](https://developer.xamarin.com/samples/monotouch/WebView/)
