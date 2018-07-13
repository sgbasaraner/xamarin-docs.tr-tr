---
title: Xamarin.Forms WebView
description: Bu makalede, kullanıcılara yerel sunmak için Xamarin.Forms WebView sınıfını kullanmayı veya ağ web içeriği ve belgeleri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: ed7bec4e25628d938218a40d157442debad8f835
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998380"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms WebView

[`WebView`](xref:Xamarin.Forms.WebView) uygulamanızda web ve HTML içeriğini görüntülemek için bir görünüm olduğundan. Farklı `OpenUri`, cihazda bir web tarayıcısına kullanıcının aldığı `WebView` uygulamanızı HTML içeriğini görüntüler.

![](webview-images/in-app-browser.png "Uygulama tarayıcıda")

## <a name="content"></a>İçerik

`WebView` Aşağıdaki içerik türlerini destekler:

- HTML ve CSS Web siteleri &ndash; WebView HTML ve CSS, JavaScript desteği dahil olmak üzere kullanılarak yazılmış Web siteleri için tam desteği vardır.
- Belgeler &ndash; WebView her platformda yerel bileşenleri kullanılarak uygulandığından WebView her platformda görüntülenebilir belgeler gösterebilme yeteneğine sahiptir. PDF dosyaları iOS ve Android üzerinde çalışmak anlamına gelir.
- HTML dizelerinde &ndash; WebView, HTML dizelerinde bellekten gösterebilir.
- Yerel dosyaları &ndash; WebView katıştırılmış uygulamada herhangi bir içerik türü yukarıdaki sunabileceği.

> [!NOTE]
> `WebView` Bunlar Internet Explorer tarafından bu platformda desteklenen bile Windows üzerinde Silverlight, Flash veya herhangi bir ActiveX denetimini desteklemez.

### <a name="websites"></a>Web siteleri

Bir Web sitesi internet'ten görüntülenecek kümesi `WebView`'s [ `Source` ](xref:Xamarin.Forms.WebViewSource) özelliği bir dize URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URL'leri gerekir tam biçimlendirilmiş belirtilen protokolle (yani, "http://" veya "https:// kendisine başına" olması gerekir).

#### <a name="ios-and-ats"></a>iOS ve ATS

9 sürümünden itibaren iOS yalnızca uygulamanız varsayılan olarak iyi güvenlik uygulamaları sunucuları ile iletişim kurmasına izin verir. Değer ayarlanmalıdır `Info.plist` güvensiz sunucuları ile iletişimi etkinleştirin.

> [!NOTE]
> Uygulamanıza güvenli bir Web sitesine bağlantı gerekiyorsa, her zaman etki alanı kullanarak özel durum olarak girmelisiniz `NSExceptionDomains` tamamen kullanrak ATS kapatma yerine `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` yalnızca aşırı acil durumlarda kullanılmalıdır.

ATS gereksinimlerinin kullanılmasını atlamak için bir özel etki alanında (Bu durum xamarin.com) etkinleştirme gösterir:

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

Yalnızca güvenilen siteler güvenilmeyen etki alanlarındaki ek güvenlik gelen yararlanıyor kullanmanıza olanak sağlayan ATS, atlamak bazı etki alanlarına etkinleştirmek için en iyi bir uygulamadır. ATS uygulama için devre dışı bırakma daha az güvenli bir yöntemi gösterir:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

Bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) iOS 9'daki bu yeni özelliği hakkında daha fazla bilgi.

### <a name="html-strings"></a>HTML dizeleri

Bir dizenin HTML kod içinde dinamik olarak tanımlanan sunmak istiyorsanız, bir örneğini oluşturmanız gerekir [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "WebView görüntüleme HTML dizesi")

Yukarıdaki kodda, `@` HTML bir dize sabit değeri, yani tüm olağan kaçış karakterleri yok sayılır işaretlemek için kullanılır.

### <a name="local-html-content"></a>Yerel HTML içeriği

WebView HTML, CSS içeriği görüntüleyebilir ve uygulama içinde gömülü Javascript kodları. Örneğin:

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

Her platform yazı tiplerine sahip olduğundan yukarıdaki CSS içinde belirtilen yazı tipi her platform için özelleştirilmiş olması gerektiğini unutmayın.

Ekran içerik kullanarak yerel bir `WebView`, gibi diğer HTML dosyasını açın ve içeriğini bir dizeye olarak yüklemek ihtiyacınız olacak `Html` özelliği bir `HtmlWebViewSource`. Dosyaları açma hakkında daha fazla bilgi için bkz. [dosyalarıyla çalışma](~/xamarin-forms/app-fundamentals/files.md).

Aşağıdaki ekran görüntüleri her platformda yerel içerik görüntüleme sonucu göster:

![](webview-images/local-content.png "WebView yerel içerik görüntüleme")

İlk sayfa yüklenen olsa da, `WebView` HTML nereden geldiğini, hiçbir bilgiye sahip. Yerel kaynaklara başvuran sayfalar ile işlem yapılırken bir sorun olmasıdır. Örnekler, ne zaman gerçekleşebilir birbirine her, bir sayfa yapar yerel sayfaları bağlantıyı ayrı bir JavaScript dosyasını kullanın ya da CSS stil sayfası için bir sayfa bağlantılar içerir.  

Söyleyin gerek bunu çözmek için `WebView` filesystem dosyaların nerede bulunur. Ayarlayarak bunu `BaseUrl` özelliği `HtmlWebViewSource` tarafından kullanılan `WebView`.

Her bir işletim sistemi dosya sisteminde farklı olduğundan, belirlemek gereken her platformda söz konusu URL. Xamarin.Forms sunan `DependencyService` için çalışma zamanında her platformda bağımlılıkları çözümleniyor.

Kullanılacak `DependencyService`, önce her platformda uygulanabilecek bir arabirim tanımlayın:

```csharp
public interface IBaseUrl { string Get(); }
```

Her platformda arabirimi uygulanana kadar uygulamayı değil çalışacağını unutmayın. Ortak proje ayarlanacağını unutmayın emin `BaseUrl` kullanarak `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Her platform için arabirim uygulamaları ardından sağlanmalıdır.

#### <a name="ios"></a>iOS

İOS, web içeriği projenin kök dizininde bulunmalıdır veya **kaynakları** derleme eylemi ile dizin *BundleResource*, aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "İos'ta yerel dosyaları")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](webview-images/ios-xs.png "İos'ta yerel dosyaları")

-----

`BaseUrl` Ana paket yoluna ayarlanmalıdır:

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

Android, HTML, CSS ve görüntüleri derleme eylemi ile varlıkları klasöre yerleştirin *AndroidAsset* aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Android yerel dosyaları")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](webview-images/android-xs.png "Android yerel dosyaları")

-----

Android, `BaseUrl` ayarlanmalıdır `"file:///android_asset/"`:

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

Android, dosyalar **varlıklar** klasör de üzerinden erişilebilir tarafından sunulan geçerli Android bağlam `MainActivity.Instance` özelliği:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Evrensel Windows Platformu (UWP) projelerde, HTML, CSS ve görüntü proje kök dizininde ayarlamak yapı eylemi yerleştirin *içerik*.

`BaseUrl` Ayarlanmalıdır `"ms-appx-web:///"`:

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

## <a name="navigation"></a>Gezinti

WebView gezinmeyi çeşitli yöntemleri ve kullanıma sunduğu özellikleri destekler:

- **GoForward()** &ndash; varsa `CanGoForward` çağırma true ise `GoForward` ziyaret edilen sonraki sayfaya ileri gider.
- **GoBack()** &ndash; varsa `CanGoBack` çağırma true ise `GoBack` son ziyaret edilen sayfasına gider.
- **CanGoBack** &ndash; `true` , dönün sayfalarına varsa `false` tarayıcı başlangıç URL'SİNDE ise.
- **CanGoForward** &ndash; `true` kullanıcı geriye gittikten daha önceden ziyaret bir sayfa İleri taşıyabilirsiniz.

Sayfa içinde `WebView` çok noktalı dokunma hareketlerini desteklemez. Önemli olduğunu emin olmak için bu içeriği mobil için iyileştirilmiş ve yakınlaştırma için gerek kalmadan görünür.

Bir bağlantı içinde gösterilecek uygulamalar için ortak bir `WebView`, cihazın tarayıcı yerine. Bu gibi durumlarda normal gezintiden izin vermek yararlıdır, ancak başlangıç bağlantıyı sırasında kullanıcı isabet yedeklediğinizde, uygulama normal uygulama görünümüne döndürmelidir.

Bu senaryoyu etkinleştirmek için yerleşik gezinti yöntemleri ve özellikleri kullanın.

Tarayıcı Görünümü sayfayı oluşturarak başlayın:

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

Bizim içinde arka plan kodu:

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

![](webview-images/in-app-browser.png "WebView gezinti düğmeleri")

## <a name="events"></a>Olaylar

WebView durumunda değişikliklerine yanıt verme yardımcı olacak iki olay meydana getirir:

- **Gezinme** &ndash; WebView yeni bir sayfa yükleme başladığında harekete geçirilen olay.
- **Geçtiğiniz** &ndash; sayfa yüklenen ve gezinti durdurdu harekete geçirilen olay.

Yüklemek için uzun süren Web sayfalarını düşünüyorsanız, bir durum göstergesi uygulamak için bu olayları kullanarak göz önünde bulundurun. Örneğin XAML şöyle görünür:

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
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

İki olay işleyiciler:

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

Bu, (yükleniyor) aşağıdaki çıktı olur:

![](webview-images/loading-start.png "WebView gezinme Event örneği")

Tamamlanan yükleme:

![](webview-images/loading-end.png "WebView taşınabilecek Event örneği")

## <a name="performance"></a>Performans

Son gelişmelerden en popüler web tarayıcısı gibi donanım hızlandırılmış işleme ve JavaScript derleme teknolojilerini benimsemeye her gördünüz. Ne yazık ki, güvenlik kısıtlamaları nedeniyle bu ilerlemeleri çoğunu kullanılabilir değil, iOS equaivalent `WebView`, `UIWebView`. Xamarin.Forms `WebView` kullanan `UIWebView`. Bir sorun varsa, özel bir oluşturucu kullanan yazma gerekecektir `WKWebView`, daha hızlı gözatma destekler. Unutmayın `WKWebView` yalnızca iOS 8 ve üzeri sürümlerde desteklenir.

Varsayılan olarak Android WebView yaklaşık yerleşik tarayıcı olarak hızlı çalışır.

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) Microsoft Edge işleme altyapısı kullanır. Masaüstü ve tablet cihazları olarak Edge tarayıcısı kullanarak aynı performans görmeniz gerekir.

## <a name="permissions"></a>İzinler

Sırayla `WebView` çözmek için izinleri her platform için ayarlandığından emin olmanız gerekir. Bazı platformlarda, dikkat `WebView` hata ayıklama modunda, ancak sürüm için yerleşik olmayan olduğunda çalışır. Hata ayıklama modunda Mac için Visual Studio tarafından varsayılan olarak, android'de internet erişimi için olanlar gibi bazı izinler ayarlanmış olmasıdır.

- **UWP** &ndash; ağ içeriği görüntülenirken Internet (istemci ve sunucu) özelliği gerektirir.
- **Android** &ndash; gerektirir `INTERNET` içerikleri ağdan görüntülenirken. Yerel içeriği özel izin gerektirir.
- **iOS** &ndash; hiçbir özel izinler gerektirir.

## <a name="layout"></a>Düzen

Diğer çoğu Xamarin.Forms görünümleri aksine `WebView` gerektiren `HeightRequest` ve `WidthRequest` StackLayout veya RelativeLayout içerdiğinde belirtilir. Bu özellikler belirtmek başarısız olursa `WebView` işlenmez.

Aşağıdaki örnekler, çalışma neden düzenlerini göstermek işleme `WebView`: %s

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

Kılavuz *olmadan* WidthRequest & HeightRequest. Kılavuz istenen yükseklik ve Genişlik belirtme gerektirmeyen birkaç düzenleri biridir.:

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

## <a name="invoking-javascript"></a>JavaScript çağırma

[ `WebView` ](xref:Xamarin.Forms.WebView) C# bir JavaScript işlevi çağırabilir ve arama için C# kodu herhangi bir sonuç özelliğini içerir. Bu ile gerçekleştirilir [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) aşağıdaki örnekte gösterilen yöntemi [WebView](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) örnek:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) Yöntemi bağımsız değişken olarak belirtilir ve herhangi bir sonuç olarak döndüren JavaScript değerlendirir bir `string`. Bu örnekte, `factorial` JavaScript işlev çağrıldığında, çarpımını döndürür `number` sonucunda. Bu JavaScript işlevi yerel HTML dosyasında tanımlanan dosya [ `WebView` ](xref:Xamarin.Forms.WebView) yükler ve aşağıdaki örnekte gösterilmiştir:

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="related-links"></a>İlgili bağlantılar

- [WebView (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
