---
title: Web görünümü
description: Yerel veya ağ web içerik ve belgeleri sunar.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: a96c57b66e5debbbb7318c22e33a21eb9b998395
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2018
---
# <a name="webview"></a>Web görünümü

[Web görünümü](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) web ve HTML görüntüleme için uygulamanızda içerik görülmektedir. Farklı `OpenUri`, cihazda web tarayıcısı için kullanıcının aldığı `WebView` uygulamanızı HTML içeriğini görüntüler.

Bu kılavuz aşağıdaki bölümlerden oluşur:

- **[İçerik](#Content)**  &ndash; WebView katıştırılmış HTML, web sayfaları ve HTML dizeleri dahil olmak üzere çeşitli içerik kaynakları destekler.
- **[Gezinti](#Navigation)**  &ndash; WebView belirli bir sayfa gezinme ve geri dönerseniz için destek içerir.
- **[Olayları](#Events)**  &ndash; dinler ve WebView kullanıcı tarafından gerçekleştirilen eylemler yanıt verir.
- **[Performans](#Performance)**  &ndash; WebView her platformda performans özellikleri hakkında bilgi edinin.
- **[İzinleri](#Permissions)**  &ndash; WebView uygulamanızda çalışacak şekilde izinleri ayarlayın öğrenin.
- **[Düzen](#Layout)**  &ndash; WebView nasıl düzenlendiğini için bazı çok belirli gereksinimleri vardır. Web görünümü düzgün görüntülediğinden emin olun öğrenin:

![](webview-images/in-app-browser.png "Uygulamayı tarayıcıda")

## <a name="content"></a>İçerik

Web görünümü şu içerik türlerini desteği ile birlikte gelir:

- HTML ve CSS Web siteleri &ndash; WebView HTML ve CSS, JavaScript desteği dahil olmak üzere kullanılarak yazılan Web siteleri için tam destek vardır.
- Belgeleri &ndash; WebView yerel bileşenleri her platformda kullanılarak uygulanır, Web görünümü her platformda görüntülenebilir belgeleri görüntüleyebiliyorsa olmasıdır. PDF dosyalarını iOS ve Android ancak olmayan Windows Phone iş anlamına gelir.
- HTML dizelerini &ndash; WebView bellek HTML dizelerden gösterebilir.
- Yerel dosya &ndash; WebView türlerinden herhangi birini içerik yukarıdaki uygulamada katıştırılmış sunabileceği.

> [!NOTE]
> `WebView` Internet Explorer'ın bu platformda desteklenir olsa bile Windows ve Windows Phone Silverlight, Flash veya herhangi bir ActiveX denetimini desteklemiyor.

### <a name="websites"></a>Web siteleri

Bir Web sitesi Internet görüntülemek için ayarlanmış `WebView`'s [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) özelliği bir dize URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URL'leri gerekir tam olarak biçimlendirilmiş belirtilen protokolü ile (örneğin, "http://" veya "https:// kendisine $a" olması gerekir).

#### <a name="ios-and-ats"></a>iOS ve ATS

Sürüm 9 itibaren iOS yalnızca varsayılan olarak en iyi yöntem güvenlik uygulayan sunucularla iletişim kurmak için uygulamanıza izin verir. Değerleri ayarlama, `Info.plist` güvensiz sunucularıyla iletişim sağlamak için.

> [!NOTE]
> Uygulamanız bir bağlantı güvenli olmayan bir Web sitesine gerektiriyorsa, her zaman etki alanı kullanarak özel durum olarak girmelisiniz `NSExceptionDomains` tamamen kullanarak devre dışı ATS kapatma yerine `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` yalnızca aşırı acil durumlarda kullanılmalıdır.

Aşağıdaki ATS gereksinimlerini atlamak için bir özel etki alanında (Bu örnek xamarin.com) etkinleştirmek gösterilmiştir:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
```

Yalnızca güvenilen siteler güvenilmeyen etki alanlarındaki ek güvenlik gelen teknolojisinden yararlanan sırasında kullanmanıza olanak sağlayan ATS atlamak bazı etki alanlarını etkinleştirmek için en iyi bir uygulamadır. Aşağıdaki ATS uygulama için devre dışı bırakma daha az güvenli yöntem gösterilmektedir:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

Bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) iOS 9'deki bu yeni özellik hakkında daha fazla bilgi.

### <a name="html-strings"></a>HTML dizeleri

HTML kodunda dinamik olarak tanımlanan bir dizi sunmak istiyorsanız, bir örneğini oluşturmanız gerekir [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Web görünümü görüntüleme HTML dizesi")

Yukarıdaki kod `@` HTML olarak bir dize değişmez değer, tüm olağan kaçış karakterleri yoksayılır anlamı işaretlemek için kullanılır.

### <a name="local-html-content"></a>Local HTML Content

Web görünümü HTML, CSS içeriğini görüntüleyebilir ve Javascript uygulama içinde katıştırılmış. Örneğin:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Her platform aynı yazı tipleri taşıdığından yukarıdaki CSS içinde belirtilen yazı tipi her platform için özelleştirilmiş olması gerektiğine dikkat edin.

Yerel içerik kullanarak görüntüleme için bir `WebView`, diğer gibi HTML dosyasını açın ve ardından bir dizeye olarak içeriğini yüklemek gerekecektir `Html` özelliği bir `HtmlWebViewSource`. Dosyaları açma hakkında daha fazla bilgi için bkz: [dosyalarıyla çalışma](~/xamarin-forms/app-fundamentals/files.md).

Aşağıdaki ekran görüntüleri her platformda yerel içerik görüntüleme sonucu göster:

![](webview-images/local-content.png "Web görünümü yerel içerik görüntüleme")

İlk sayfa yüklenen rağmen `WebView` HTML nereden geldiğini, hiçbir bilgiye sahip. Yerel kaynaklar başvuru sayfaları ile ilgilenirken, bir sorun oluşturur. Ne zaman bu durum oluşabilir örnekleri yerel sayfaları bağlantı için her diğer bir sayfa yapar kullandığınızda ayrı bir JavaScript dosyası veya bir sayfa için bir CSS stil bağlantılar içerir.  

Bunu çözmek için söyleyin gereksinim `WebView` filesystem dosyaları nerede bulacağını. Ayarlayarak bunu `BaseUrl` özelliği `HtmlWebViewSource` tarafından kullanılan `WebView`.

Dosya sistemi işletim sistemlerinin her birinde farklı olduğundan, belirlemek gereken her platformda bu URL. Xamarin.Forms sunan `DependencyService` her platformda çalışma zamanında bağımlılıkları çözmek için.

Kullanılacak `DependencyService`, önce her platformda uygulanabileceği bir arabirim tanımlayın:

```csharp
public interface IBaseUrl { string Get(); }
```

Arabirim her platformda uygulanana kadar uygulama çalışmaz olduğunu unutmayın. Ortak projesinde ayarlamak unutmayın emin olun `BaseUrl` kullanarak `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Arabirim her platform için uygulamalarını sonra sağlanmalıdır.

#### <a name="ios"></a>iOS

İos'ta, web içeriği projenin kök dizininde bulunmalıdır veya **kaynakları** yapı eylemiyle dizin *BundleResource*, aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "İos'ta yerel dosyaları")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](webview-images/ios-xs.png "İos'ta yerel dosyaları")

-----

`BaseUrl` Ana paketin yolu için ayarlamanız gerekir:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Android, HTML, CSS ve görüntüleri yapı eylemine sahip varlıklar klasöre yerleştirin *AndroidAsset* aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Android yerel dosyaları")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](webview-images/android-xs.png "Android yerel dosyaları")

-----

Android'de `BaseUrl` ayarlanmalı `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

Android, dosyalar **varlıklar** klasörü de üzerinden erişilebilir tarafından sunulan geçerli Android bağlamı `MainActivity.Instance` özelliği:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="windows-phone"></a>Windows Phone

Windows Phone üzerinde HTML, CSS ve görüntüleri proje kök dizininde kümesine yapı eylemiyle yerleştirin *içerik* aşağıda gösterildiği gibi:

![](webview-images/windows-vs.png "Windows Phone yerel dosyaları")

Windows Phone üzerinde `BaseUrl` ayarlanmalı `""`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Windows))]
namespace WorkingWithWebview.Windows {
  public class BaseUrl_Windows : IBaseUrl {
    public string Get() {
      return "";
    }
  }
}
```

#### <a name="windows-runtime-and-universal-windows-platform"></a>Windows çalışma zamanı ve evrensel Windows platformu

Windows çalışma zamanı ve evrensel Windows Platformu (UWP) projelerde HTML, CSS ve görüntüleri proje kök dizininde kümesine yapı eylemiyle yerleştirin *içerik*.

`BaseUrl` Ayarlanmalı `"ms-appx-web:///"`:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Gezinme

Web görünümü gezinmeyi çeşitli yöntemleri ve kullanılabilir hale getirir özellikleri destekler:

- **GoForward()** &ndash; varsa `CanGoForward` çağırma doğrudur `GoForward` İleri ziyaret edilen sonraki sayfasına gider.
- **GoBack()** &ndash; varsa `CanGoBack` çağırma doğrudur `GoBack` son ziyaret edilen sayfasına gidin.
- **CanGoBack** &ndash; `true` geri, gitmek için sayfaları varsa `false` tarayıcı başlangıç URL'de ise.
- **CanGoForward** &ndash; `true` kullanıcı geriye doğru gezinen ve İleri zaten ziyaret bir sayfaya taşıyabilirsiniz.

Sayfalar içinde `WebView` çok dokunma hareketleri desteklemiyor. Önemli emin olmak için bu içerik mobil iyileştirilmiş ve yakınlaştırma için gerek kalmadan görüntülenir.

Bir bağlantıyı göstermek üzere uygulamalar için ortak bir `WebView`, cihazın tarayıcı yerine. Bu durumlarda, normal Gezinti izin vermek kullanışlıdır ancak başlangıç bağlantısında olduğunuzda kullanıcı isabet yedeklediğinizde, uygulama normal uygulama görünümüne döndürmelidir.

Bu senaryoyu etkinleştirmek için yerleşik gezinti yöntemlerini ve özelliklerini kullanın.

Tarayıcı görünümünün sayfa oluşturarak başlayın:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Bizim kod arkasında:

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

İşte bu kadar!

![](webview-images/in-app-browser.png "Web görünümü gezinti düğmeleri")

## <a name="events"></a>Olaylar

Web görünümü durumda değişikliklerine yanıt verme yardımcı olmak üzere iki olaylar oluşur:

- **Gezinme** &ndash; olay WebView yeni bir sayfa yükleme başladığında gerçekleşti.
- **Gittiğinizde** &ndash; olay Sayfa yüklendikten ve gezinti durdurdu tetiklenir.

Yüklenmesi uzun zaman Web sayfalarını kullanarak öngörüyorsanız, bir durum göstergesi uygulamak için bu olayları kullanmayı düşünün. Örneğin:

Bizim XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
    <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        isVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
Bizim iki olay işleyicileri:

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
            LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
            LoadingLabel.IsVisible = false;
}
```

Bu (yüklenirken) şunlara sebep olur:

![](webview-images/loading-start.png "Web görünümü gezinme olay örneği")

Tamamlanan yükleme:

![](webview-images/loading-end.png "Web görünümü gittiğinizde olay örneği")

## <a name="performance"></a>Performans

En son gelişmeleri her işleme ve JavaScript derleme donanım hızlandırılmış gibi teknolojileri benimsemeye popüler web tarayıcılarının gördünüz. Ne yazık ki, güvenlik kısıtlamaları nedeniyle bu geliştirmeleri çoğunu kullanılabilir değil, iOS equaivalent `WebView`, `UIWebView`. Xamarin.Forms `WebView` kullanan `UIWebView`. Bu bir sorun olursa, kullanan özel Oluşturucu yazma gerekir `WKWebView`, daha hızlı tarama destekler. Unutmayın `WKWebView` yalnızca iOS 8 ve sonraki sürümleri desteklenir.

Varsayılan olarak android'de WebView yerleşik tarayıcı olarak yaklaşık olarak hızlıdır.

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) Microsoft Edge işleme altyapısı kullanır. Masaüstü ve tablet aygıtları aynı performans Edge tarayıcı kullanarak olarak görmeniz gerekir.

`WebBrowser` Denetimi Windows Phone 8 ve Windows Phone 8.1 mu değil destek en yeni HTML5 özellikleri ve performansın sıklıkla olabilir. Siteleri Windows Phone içinde 's nasıl görüntüleneceğini unutmayın `WebView`. Internet Explorer'da test etmek yeterli değil.

## <a name="permissions"></a>İzinler

Sırayla `WebView` çalışmak için her platform için izinlerinin ayarlandığından emin olmanız gerekir. Bazı platformlarda unutmayın `WebView` hata ayıklama modunda, ancak sürüm için değil yapılandırıldığında çalışır. Hata ayıklama modunda Mac için Visual Studio tarafından varsayılan olarak android'de internet erişimi için olanlar gibi bazı izinler ayarlanan olmasıdır.

- **Windows Phone 8.0** &ndash; gerektirir `ID_CAP_WEBBROWSERCOMPONENT` denetiminin ve `ID_CAP_NETWORKING` internet erişimi için.
- **Windows Phone 8.1 ve UWP** &ndash; ağ içerik görüntülerken, Internet (istemci ve sunucu) özelliği gerektirir.
- **Android** &ndash; gerektirir `INTERNET` içeriği ağdan görüntülenirken. Yerel içerik özel izinler gerektirir.
- **iOS** &ndash; özel izinler gerektirir.

## <a name="layout"></a>Düzen

Diğer çoğu Xamarin.Forms görünümleri aksine `WebView` gerektiren `HeightRequest` ve `WidthRequest` StackLayout veya RelativeLayout içindeki aktarılırken belirtilir. Bu özellikler belirtmek başarısız olursa `WebView` değil olarak kabul eder.

Aşağıdaki örnekler, çalışma neden düzenleri göstermek işleme `WebView`: %s

StackLayout WidthRequest & HeightRequest:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout WidthRequest & HeightRequest:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

AbsoluteLayout *olmadan* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Kılavuz *olmadan* WidthRequest & HeightRequest. Kılavuz istenen yükseklik ve Genişlik belirtme gerektirmez birkaç düzenleri biridir.:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```


## <a name="related-links"></a>İlgili bağlantılar

- [Web görünümü (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [Web görünümü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
