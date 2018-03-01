---
title: "Web görünümü"
ms.topic: article
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 234bd79754ae7f328d3207757156089441fc588c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="web-view"></a>Web görünümü

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) web sayfalarını görüntüleme için kendi pencere oluşturun (veya hatta tam bir tarayıcı geliştirmek) sağlar. Bu öğreticide, basit bir oluşturacaksınız [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) , görüntülemek ve web sayfaları gidin.

Adlı yeni bir proje oluşturma **HelloWebView**.

Açık **Resources/Layout/Main.axml** ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Bu uygulama Internet erişimi getirir çünkü Android uygun izinleri bildirim dosyası eklemeniz gerekir. Çalışması için uygulamanızın gerektirdiği hangi izinlerin belirtmek için proje özelliklerini açın. Etkinleştirme `INTERNET` aşağıda gösterildiği gibi izin:

![Android bildirimi Internet izin ayarlama](web-view-images/01-set-internet-permissions.png)

Şimdi açmak **MainActivity.cs** ve kullanarak bir ekleme yönerge Webkit için:

```csharp
using Android.Webkit;
```

Üstündeki `MainActivity` sınıfı, bildirme bir [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) nesnesi:

```csharp
WebView web_view;
```

Zaman **WebView** olan bir URL yük istenir, onu varsayılan olarak varsayılan tarayıcı isteğini temsil edecek. Olmasını **WebView** URL (varsayılan tarayıcı yerine) yükü, bir alt kümesi olmalıdır `Android.Webkit.WebViewClient` ve geçersiz kılma `ShouldOverriderUrlLoading` yöntemi. Bu özel örneği `WebViewClient` için sağlanan `WebView`. Bunu yapmak için aşağıdaki iç içe geçmiş ekleyin `HelloWebViewClient` sınıfını `MainActivity`:

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

Zaman `ShouldOverrideUrlLoading` döndürür `false`, Android için sinyalleri, geçerli `WebView` örneği işlenen istek ve başka bir eylem gerekli değildir. 

API düzeyini 24 veya üzeri hedefliyorsanız, kullanın `ShouldOverrideUrlLoading` alan bir `IWebResourceRequest` yerine ikinci bağımsız değişkeni için bir `string`:

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

Bu üye başlatır [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) bir kodla [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) düzeni ve JavaScript için sağlar [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) ile[ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (bkz [çağrısı C\# JavaScript gelen](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript) C çağrısına hakkında bilgi için tarif\# JavaScript işlevleri). Son olarak, ilk bir web sayfası ile yüklenen [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Derleme ve uygulamayı çalıştırın. Aşağıdaki ekran görüntüsünde görülen bir basit web sayfası Görüntüleyicisi uygulama görmeniz gerekir:

[![Bir Web görünümü görüntüleme uygulama örneği](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png)

İşlenecek **geri** düğmesini tuşuna, aşağıdakileri ekleyin deyimi kullanarak:

```csharp
using Android.Views;
```

Ardından, içinde aşağıdaki yöntemi ekleyin `HelloWebView` etkinlik:

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

Bu [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) etkinlik çalışırken bir düğmesine basıldığında geri çağırma yöntemi çağrılır. Koşul kullanır içinde [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) tuşa olup olmadığını denetlemek için **geri** düğmesi olup [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) gerçekten ulaşabileceği Geri (geçmişini varsa) gitme. Her ikisi de doğruysa sonra [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) yöntemi çağrıldığında, hangi geri tek adımda gideceği [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) geçmişi. Döndürme `true` olay işlendiğini gösterir. Bu koşul karşılanmazsa, olay sisteme geri gönderilir.

Uygulamayı yeniden çalıştırın. Şimdi bağlantıları izleyin ve sayfa geçmişinde gezinme yapabiliyor olmanız gerekir:

[![Eylemi geri düğmesini örnek ekran görüntüleri](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png)


*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>İlgili bağlantılar

- [C# JavaScript'ten çağırın](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
