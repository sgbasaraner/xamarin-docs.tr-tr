---
title: Web görünümü
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8d7b0e1abc8eb11bf812a111764b9cccfb41e041
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241181"
---
# <a name="web-view"></a>Web görünümü

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) web sayfaları görüntülemek için kendi penceresi oluştur (veya tam bir tarayıcı bile geliştirmek) sağlar. Bu öğreticide, basit bir oluşturacaksınız [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) , görüntüleyebilir ve web sayfalarına gidin.

Adlı yeni bir proje oluşturma **HelloWebView**.

Açık **Resources/Layout/Main.axml** ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Bu uygulamayı Internet erişir çünkü uygun izinleri Android bildirim dosyası eklemeniz gerekir. Çalışması için uygulamanızın gerektirdiği izinleri belirtmek için projenizin özelliklerini açın. Etkinleştirme `INTERNET` aşağıda gösterildiği gibi izin:

![Android bildirimi Internet izin ayarlama](web-view-images/01-set-internet-permissions.png)

Artık **MainActivity.cs** ve kullanarak bir ekleme Webkit yönergesi:

```csharp
using Android.Webkit;
```

Üst kısmındaki `MainActivity` sınıfı, bildirmek bir [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) nesnesi:

```csharp
WebView web_view;
```

Zaman **WebView** olan bir URL yük istenir, varsayılan olarak varsayılan tarayıcı isteğine temsilci. Olmasını **WebView** URL (varsayılan tarayıcı yerine) yükleme, alt sınıfı olmalıdır `Android.Webkit.WebViewClient` ve geçersiz kılma `ShouldOverriderUrlLoading` yöntemi. Bu özel örneği `WebViewClient` için sağlanan `WebView`. Bunu yapmak için aşağıdaki iç içe ekleme `HelloWebViewClient` sınıfını `MainActivity`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

Zaman `ShouldOverrideUrlLoading` döndürür `false`, Android sinyalleri, geçerli `WebView` örneği işlenen istek ve başka bir eylem gerekli değildir. 

API düzeyi 24 veya üzeri hedefliyorsanız, aşırı yüklemesini kullanın `ShouldOverrideUrlLoading` almayan bir `IWebResourceRequest` yerine ikinci bağımsız değişkeni bir `string`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

Ardından, aşağıdaki kodu kullanın [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

Bu üye başlatır [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) bir kodla [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) düzeni ve JavaScript için etkinleştirir [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) ile[ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (bkz [C çağrı\# JavaScript'ten](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) C çağrısına hakkında bilgi için tarif\# JavaScript işlevleri). Son olarak, bir başlangıç web sayfası ile yüklenen [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Derleme ve uygulamayı çalıştırın. Aşağıdaki ekran görüntüsünde görülen bir basit web sayfası Görüntüleyici uygulamasını görmeniz gerekir:

[![Bir WebView görüntüleyen bir uygulama örneği](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

İşlenecek **geri** tuşuna düğme, aşağıdaki using deyimi:

```csharp
using Android.Views;
```

Ardından, içine aşağıdaki yöntemi ekleyin `HelloWebView` etkinlik:

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

Bu [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) etkinliğin çalıştığı sırada bir düğmeye basıldığında, geri çağırma yöntemi çağrılır. Koşulun içinde kullanır [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) tuşu basılı olup olmadığını denetlemek için **geri** düğmesi ve [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) gerçekten zamanları Geri (geçmişini varsa) gezinme. Her ikisi de doğruysa sonra [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) yöntemi çağrıldığında, hangi arka tek adımda gider [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) geçmişi. Döndüren `true` olay işlendiğini gösterir. Ardından bu koşul karşılanmazsa, olay sisteme geri gönderilir.

Uygulamayı yeniden çalıştırın. Artık bağlantıları izleyin ve sayfa geçmişinde gezinme olmalıdır:

[![Eylem Geri düğmesini örnek ekran görüntüleri](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Bu sayfanın bölümleri olan oluşturulan Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş tabanlı değişiklikleri*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>İlgili bağlantılar

- [C# JavaScript'ten çağırın](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
