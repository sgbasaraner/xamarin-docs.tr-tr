---
title: Razor şablonları kullanarak yapı HTML görünümleri
description: " HTML oluşturmak için bir tam ekran Web sayfasını kullanarak karmaşık bir platformlar arası şekilde biçimlendirme özellikle HTML, Javascript ve CSS bir Web sitesi projeden zaten varsa işlemek için basit ve etkili bir yol olabilir."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 48d7778bf3225401f2819909ae6be320cfa881e3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="building-html-views-using-razor-templates"></a>Razor şablonları kullanarak yapı HTML görünümleri

Mobil Geliştirme dünyada "karma uygulama" terimi genellikle bir barındırılan web Görüntüleyicisi denetimi içinde HTML sayfaları olarak kendi ekranlar bazı (veya tüm) sunan bir uygulamaya anlamına gelir.

Sağlayan bazı geliştirme ortamlarında vardır ancak bu uygulamaları performans sorunlarından karmaşık işleme veya UI efektler gerçekleştirmek çalışırken zarar görebilir ve olan mobil uygulamanızda tamamen HTML ve Javascript, ayrıca platform sınırlı derleme erişebilecekleri özellikleri.

Xamarin, özellikle Razor HTML şablon motoru kullanırken her iki dünyanın en iyi sunar. Xamarin ile Javascript ve CSS kullanır, ancak temel platform API'leri ve C# kullanarak hızlı işleme için tam erişim de platformlar arası şablonlu HTML görünümleri oluşturmak için esnekliğe sahip olursunuz.

Bu belge, şablon altyapısının yapı Xamarin kullanarak mobil platformlarda kullanılan HTML + Javascript + CSS görünümleri Razor kullanımı açıklanmaktadır.

## <a name="using-web-views-programmatically"></a>Program aracılığıyla Web görünümleri kullanma

Bu bölüm, biz Razor hakkında bilgi edinin önce doğrudan – HTML içeriğini görüntülemek için web görünümleri kullanmak özel bir uygulama içinde oluşturulan HTML içeriğini nasıl kapsar.

Oluşturmak ve C# kullanarak HTML görüntülemek kolaydır Xamarin hem iOS hem de Android, temel alınan platform API'leri tam erişim sağlar. Her platform için temel sözdizimi aşağıda gösterilmiştir.

### <a name="ios"></a>iOS

Xamarin.iOS UIWebView denetimindeki HTML görüntüleme, yalnızca birkaç satırlık bir kod alır:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Bkz: [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) tarif UIWebView denetimini kullanma hakkında daha fazla bilgi.

### <a name="android"></a>Android

HTML Xamarin.Android kullanarak bir Web görünümü denetiminde görüntüleme birkaç kod satırıyla, gerçekleştirilir:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Bkz: [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) tarif WebView Denetimi'ni kullanma hakkında daha fazla bilgi.

### <a name="specifying-the-base-directory"></a>Temel dizin belirtme

Her iki platformlarda HTML sayfası için temel dizin belirten bir parametre yok. Bu görüntüleri ve CSS dosyaları gibi kaynaklarına göreli başvuruları çözümlemek için kullanılan cihazın dosya sisteminde konumdur. Örneğin, etiketler gibi

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

Bu dosyalara başvuran: **style.css**, **monkey.jpg** ve **jscript.js**. Temel dizin ayar sayfasına yüklenmiş olması bu dosyaların bulunduğu web görünümü söyler.

#### <a name="ios"></a>iOS

Şablon çıktısı aşağıdaki C# kodu ile iOS oluşturulur:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Temel dizin olarak belirtilen `NSBundle.MainBundle.BundleUrl` , başvurduğu dizinine uygulama yüklenir. Tüm dosyaları **kaynakları** klasörüne kopyalanır bu konuma gibi **style.css** burada gösterilen dosyası:

 ![iPhoneHybrid çözümü](images/image1_240x163.png)

Tüm statik içerik dosyaları için yapı eylemi olmalıdır **BundleResource**:

 ![iOS projesi yapı eylemi: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android, aynı zamanda bir web görünümü html dizeleri görüntülendiğinde parametre olarak geçirilecek temel bir dizin gerektirir.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Özel dize **file:///android_asset/** burada içeren gösterilen uygulamanızı, Android varlıklar klasöründe başvurduğu **style.css** dosya.

 ![AndroidHybrid çözümü](images/image3_240x167.png)

Tüm statik içerik dosyaları için yapı eylemi olmalıdır **AndroidAsset**.

 ![Android projesi yapı eylemi: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>C# HTML ve Javascript çağırma

Html sayfası web görünümüne yüklendiğinde, sayfa bir sunucudan yüklendiyse olduğu gibi bağlantıları ve formlar değerlendirir. Bu, kullanıcı bir bağlantıya tıklar ya da bir formu gönderdikten web görünümü belirtilen hedef gitmek deneyeceğini anlamına gelir.

Bağlantı için bir dış sunucu (örneğin, google.com) ise web görünümü (bir internet bağlantısı varsayılarak) dış Web sitesi yük dener.

```html
<a href="http://google.com/">Google</a>
```

Bağlantıyı göreli değilse temel dizinden içeriği yüklemek web görünümü dener. İçerik cihaza uygulamanın içinde saklandığı şekilde açıkça ağ bağlantısı yok Bunun çalışması için gereklidir.

```html
<a href="somepage.html">Local content</a>
```

Form eylemlerini aynı kural izleyin.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

İstemci web sunucusunda barındırmak için başlatacağınız değil; Ancak, HTTP GET Hizmetleri çağırmak için bugünün esnek tasarım desenleri işe aynı sunucu iletişimi teknikleri kullanan ve Javascript yayma tarafından yanıtları zaman uyumsuz olarak işleyen (veya arama Javascript zaten web görünümünde barındırılan). Bu, kolayca veri işleme sonra sonuçları HTML sayfasında geri görüntü için C# kod uygulamasına geri HTML aktarmak sağlar.

İOS ve Android uygulama kodu (gerekliyse) yanıt vermesi bu gezinti olayları izlemesine uygulama kodu için bir mekanizma sağlar. Bu özellik, yerel kod web görünümü ile etkileşime izin verdiğinden karma uygulamalar oluşturmak için önemlidir.

#### <a name="ios"></a>iOS

İOS web görünümünde ShouldStartLoad olayı (örneğin, bir bağlantıyı tıklatın) gezinti isteği işlemek üzere uygulama kodu izin vermek için geçersiz kılınabilir. Yöntem parametreleri tüm bilgileri sağlayın

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

ve olay işleyicisi atayabilirsiniz:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Android'de yalnızca bir alt WebViewClient ve sonra Gezinti isteğine yanıt vermek için uygulama kodu.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, string url) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

ve ardından istemci web görünümünde ayarlayın:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Arama JavaScript'ten C#

Yeni bir HTML dosyası yüklemek için bir web görünümü belirten ek olarak, C# kod ayrıca Javascript içinde görüntülenmekte olan sayfası çalıştırabilirsiniz. Tüm Javascript kod blokları C# dizeleri kullanılarak oluşturulan ve yürütülen veya Javascript sayfasında zaten kullanılabilir yöntem çağrılarını oluşturabileceği `script` etiketler.

#### <a name="android"></a>Android

Yürütülebilir ve ardından önüne Javascript kodu oluşturmak "javascript:" ve bu dizeyi yüklemek için web görünümü bildirin:

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS web görünümleri Javascript özellikle çağrılacak bir yöntem sağlar:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Özet

Bu bölümde web görünümü denetimleri Android ve Xamarin ile karma uygulamalar oluşturmanıza bize iOS özelliklerini anlatılmıştır dahil olmak üzere:

-  HTML kodunda oluşturulan dizelerden Yükleme özelliğini,
-  Özelliği yerel dosyaları (CSS, Javascript, görüntüleri veya diğer HTML dosyaları), başvuru
-  C# kodunda gezinme istekleri müdahale özelliği,
-  C# kodundan JavaScript çağırma yeteneği.


Sonraki bölüm kullanmak için HTML karma uygulamalar oluşturmak kolaylaştırır Razor tanıtır.

## <a name="what-is-razor"></a>Razor nedir?

Razor başlangıçta sunucu üzerinde çalışan ve web tarayıcıları sunulması için HTML oluşturmak için ASP.NET MVC ile sunulan bir şablon altyapısıdır.

Böylece düzeni express ve CSS stil sayfaları ve Javascript kolayca birleştirmek Razor şablon motoru standart HTML sözdizimi C# ile genişletir. Şablonu herhangi bir özel türü olabilen ve özelliklerini şablonu doğrudan erişilebilir bir Model sınıfı başvuruda bulunabilir. Kendi ana avantajları de HTML ve C# sözdizimi kolayca karışık yeteneğidir.

Razor şablonları için sunucu tarafı kullanım sınırlı değildir, Xamarin uygulamaları da eklenebilir. Program aracılığıyla web görünümleri ile çalışmak için Razor şablonları özelliği ile birlikte kullanarak, Gelişmiş platformlar arası karma uygulamaların Xamarin ile oluşturulan olanak sağlar.

### <a name="razor-template-basics"></a>Razor şablon temelleri

Razor şablon dosyalarını sahip bir **.cshtml** dosya uzantısı. Metin şablonu oluşturma bölümünden Xamarin projeye bunlar eklenebilir **yeni dosya** iletişim:

 ![Yeni dosya - Razor şablon](images/image5_400x201.png)

Basit bir Razor şablon ( **RazorView.cshtml**) aşağıda gösterilmiştir.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Normal bir HTML dosyası aşağıdaki farkları dikkat edin:

-  `@` Simgesi Razor şablonlarında özel bir anlamı olan – ifadesini değerlendirilecek C# olduğunu gösterir.
- `@model` yönergesi her zaman bir Razor şablon dosyası ilk satır görünür.
-  `@model` Yönergesi türü tarafından uyulması. Bu örnekte basit bir dize şablona geçirilmiş, ancak bu özel bir sınıf olabilir.
-  Zaman `@Model` başvurulan isteğe bağlı olarak şablon (bir dize olarak bu örnekte) oluşturulduğunda şablona geçirilen nesnesine başvuru sağlar.
-  IDE parçalı sınıf şablonları için otomatik olarak oluşturur (ile dosyaları **.cshtml** uzantısı). Bu kodu görüntüleyebilirsiniz, ancak düzenlenemez.
 ![RazorView.cshtml](images/image6_125x34.png) .cshtml şablon dosya adı ile eşleşmesi için RazorView adlı kısmi sınıfı. C# kodu şablonunda başvurmak için kullanılan bu adıdır.
- `@using` deyimleri de ek ad alanlarını dahil etmek için bir Razor şablon üstünde dahil edilebilir.


Son HTML çıktı sonra aşağıdaki C# kod ile oluşturulabilir. "Hello, işlenen şablon çıktısı birleştirilir World" bir dize olacak şekilde Model belirttiğimiz unutmayın.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

Bir web görünümü iOS simülatörü ve Android öykünücüsü gösterilen çıktısı şöyledir:

 ![Merhaba Dünya](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Daha fazla Razor sözdizimi

Bu bölümde başlamanıza yardımcı olması için bazı temel Razor sözdizimi tanıtmak için yapacağız bunu kullanma. Bu bölümdeki örnekleri aşağıdaki sınıf verilerle doldurmak ve Razor kullanarak görüntüleyin:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Tüm örnekler aşağıdaki veri başlatma kodunu kullanın

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Model özelliklerini görüntüleme

Model bir sınıf özelliklerine sahip olduğunda, bunlar Razor şablonunda kolayca bu örnek şablonda gösterildiği gibi başvurulur:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

Bu, aşağıdaki kodu kullanarak bir dizeye oluşturulabilir:

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

Son çıkışı burada web görünümü iOS simülatörü ve Android öykünücüsü'gösterilir:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>C# deyimleri

Daha karmaşık C# Model özelliği güncelleştirmeleri ve bu örnekte yaş hesaplama gibi şablon eklenebilir:

```html
@model Monkey
<html>
    <body>
    @{
        Model.Name = "Rupert X. Monkey";
        Model.Birthday = new DateTime(2011,3,1);
    }
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    </body>
</html>
```

Kodla çevreleyen tarafından (yaş biçimlendirme gibi) karmaşık tek satırlı C# ifadeler yazabilirsiniz `@()`.

Birden çok C# deyimleri ile çevreleyen tarafından yazılabilir `@{}`.

#### <a name="if-else-statements"></a>İf-else ifadeleri

Kod dalları ifade edilir ile `@if` bu şablonu örnekte gösterildiği gibi.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <p>@Model.FavoriteFoods.Count favorites</p>
    }
    </body>
</html>
```

#### <a name="loops"></a>Döngüler

Döngü yapıları gibi `foreach` de eklenebilir. `@` Öneki döngü değişkeni kullanılabilir ( `@food` bu durumda) HTML oluşturulacak.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <ul>
            @foreach (var food in @Model.FavoriteFoods) {
                <li>@food</li>
            }
        </ul>
    }
    </body>
</html>
```

Yukarıdaki şablonu çıktısını iOS simülatörü ve Android öykünücüsü çalıştıran gösterilir:

 ![Rupert X Monkey](images/image9_520x277.png)

Bu bölümde, basit salt okunur görünümlerde işlemek için Razor şablonları kullanma temelleri ele. Sonraki bölümde, kullanıcı girişi kabul edebilir ve Javascript HTML görünümü ve C# arasında çalışmanız Razor kullanarak daha kapsamlı uygulamalar oluşturmak açıklanmaktadır.

## <a name="using-razor-templates-with-xamarin"></a>Xamarin ile Razor şablonlarını kullanma

Bu bölümde, Mac için Visual Studio'da Çözüm şablonları kullanarak kendi karma uygulama yapı kullanmak üzere açıklanmaktadır Üç şablonları listesinden kullanılabilir **Dosya > Yeni > çözüm...**  penceresi:

- **Android > Uygulama > Android WebView uygulama**
- **iOS > Uygulama > WebView uygulama**
- **ASP.NET MVC proje**



**Yeni çözüm** pencere görünür, aşağıdaki gibi iPhone ve Android projeleri için - çözüm açıklama sağdaki Razor şablon motoru desteği vurgular.

 ![İPhone ve Android çözümleri oluşturma](images/image13_1139x959.png)

Kolayca ekleyebilirsiniz Not bir **.cshtml** Razor şablon *herhangi* Xamarin projesi var, bu çözüm şablonları kullanmak ise gerekli değildir. iOS projeleri Razor ya da kullanılacak film şeridi gerektirmez; yalnızca bir UIWebView denetimi için herhangi bir görünüm programlı olarak ekleyin ve Razor şablonları tüm C# kodunda hale getirebilir.

İPhone ve Android projeleri için varsayılan şablon çözüm içeriğini aşağıda verilmiştir:

 ![iPhone ve Android şablonları](images/image10_428x310.png)

Şablonları yük Razor şablon bir veri modeli nesnesi ile kullanıcı girişini işlemek ve Javascript aracılığıyla kullanıcıya iletişim kurmak için hazır Git uygulama altyapısı sağlar.

Çözüm önemli bölümleri şunlardır:

-  Statik içerik gibi **style.css** dosya.
-  Razor .cshtml şablon dosyalarını ister **RazorView.cshtml** .
-  Model Razor şablonlarında gibi başvurulan sınıfları **ExampleModel.cs** .
-  Web görünümü oluşturur ve şablon gibi işler platforma özgü sınıf `MainActivity` android'de ve `iPhoneHybridViewController` iOS.


Aşağıdaki bölümde projeleri nasıl çalıştığı açıklanmıştır.

### <a name="static-content"></a>Statik İçerik

Statik içerik CSS stil sayfaları, görüntüler, Javascript dosyaları veya gelen bağlı veya bir web görünümünde görüntülenen HTML dosyası tarafından başvurulan diğer içeriklere içerir.

Şablon projelerini karma uygulamanızda statik içerik dahil etmek nasıl göstermek için bir en az stil sayfası içerir. CSS stil şablonu bu gibi başvurulur:

```html
<link rel="stylesheet" href="style.css" />
```

Hangi stil ve JQuery gibi çerçeveleri dahil olmak üzere, gereksinim Javascript dosyaları ekleyebilirsiniz.

### <a name="razor-cshtml-templates"></a>Razor cshtml şablonları

Bir Razor şablon içerir **.cshtml** HTML/Javascript ve C# arasında veri iletmek için kod önceden yazmıştır dosya. Bu, olanak tanır yok yalnızca Model salt okunur verileri görüntülemek, ancak ayrıca HTML uygulamasında kullanıcı girdisi kabul etmek ve bu geçirin Gelişmiş karma uygulamalar başa işleme veya depolama için C# kod derleme.

#### <a name="rendering-the-template"></a>Şablon oluşturma

Çağırma `GenerateString` bir şablonu temel HTML hazır bir web görünümü içinde görüntülemek için işler. Ardından şablonu bir modeli kullanıyorsa önce işleme sağlanmalıdır. Bu diyagramda, işleme – statik kaynakları web görünümü belirtilen dosyaları bulmak için sağlanan temel dizinini kullanarak çalışma zamanında tarafından çözülen çalışma şeklini değil gösterilir.

 ![Razor akış çizelgesi](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Şablondan C# kod çağırma

C# için geri çağırma işlenmiş web görünümü gelen iletişimi, web görünümü URL'sini ayarlayarak ve web görünümü yeniden olmadan yerel isteği işlemek üzere talep C# kesintiye uğratan yapılır.

Örnek RazorView'ın düğmesi nasıl işleneceğini görülebilir. Aşağıdaki HTML düğmesi bulunur:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Javascript işlevi okur, tüm değerleri, ayarlar ve HTML Form `location.href` web görünümü için:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

Bu URL (ör. özel bir şema ile web görünümüne gitmek çalışır `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Yerel web görünümü bu gezinti isteği işlerken, müdahale fırsatı sahibiz. İOS, bu UIWebView'ın HandleShouldStartLoad olay işleme tarafından gerçekleştirilir. Android, biz WebViewClient formda kullanılan ve ShouldOverrideUrlLoading geçersiz kılma yalnızca bir alt kümesi.

Bu iki Gezinti dinleyiciler içyüzü temelde aynıdır.

İlk olarak, web görünümü yüklenmeye çalışılıyor URL'yi denetleyin ve özel düzeniyle başlamazsa (`hybrid:`), gerçekleşmesi Gezinti normal olarak izin.

Özel URL şeması için her şeyi düzeni arasında URL ve "?" (Bu durumda "UpdateLabel") işlenecek yöntemi adıdır. Sorgu dizesindeki her şeyi yöntemi çağrısına parametre olarak kabul edilir:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` Bu örnekte en düşük düzeyde dize düzenlemesi ("diyor" C# dizeye eklenmesini) textbox parametresindeki yapar ve web görünümüne geri çağırır.

Böylece web görünümü özel URL'ye geçerken son çalışmaz URL işleme sonra Gezinti yöntemi durdurur.

#### <a name="manipulating-the-template-from-c"></a>C# şablondan düzenleme

C# gelen iletişimi için işlenen HTML web görünümü web görünümünde Javascript çağırarak yapılır. İos'ta, bu çağırarak yapılır `EvaluateJavascript` UIWebView üzerinde:

```csharp
webView.EvaluateJavascript (js);
```

Android, Javascript web görünümünde kullanarak bir URL olarak Javascript yükleyerek çağrılabilir `"javascript:"` URL şeması:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Bir uygulama yapma gerçekten karma

Bu şablonlar hale her platformda yerel denetimlerin kullanın – tüm ekranı tek web görünümü ile doldurulur.

HTML prototip oluşturma için harika olabilir ve tür şeyler görüntüleme web zengin metin ve yanıt düzeni gibi en iyisidir. Ancak tüm görevleri HTML ve Javascript – verilerin uzun listeleriyle kaydırma için uygundur, örneğin, daha iyi Android (UITableView iOS) veya ListView gibi yerel kullanıcı Arabirimi denetimlerini kullanarak gerçekleştirir.

Şablon web görünümleri kolayca platforma özgü denetimleriyle – yalnızca düzenleme Genişletilebilir **MainStoryboard.storyboard** iOS Tasarımcısı'nda veya **Resources/layout/Main.axml** android'de.

### <a name="razortodo-sample"></a>RazorTodo örnek

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) depo tamamen HTML tabanlı bir uygulama ve yerel denetimleriyle HTML birleştiren bir uygulama arasındaki farklar göstermek için iki ayrı çözümleri içerir:

-  **RazorTodo** -tamamen HTML temelli uygulama Razor şablonları kullanma.
-  **RazorNativeTodo** - iOS ve Android için yerel liste görünümü denetimlerini kullanır ancak HTML ve Razor Düzenle ekran görüntüler.


İOS ve Android, taşınabilir sınıf kitaplıkları (veritabanı ve model sınıflar gibi ortak kodun paylaşmak için PCLs) kullanılarak bu Xamarin uygulamaları çalıştırın. Razor **.cshtml** şablonları da dahil edilebilir PCL platformları arasında kolayca paylaşılan şekilde.

Her iki örnek uygulamaları Twitter paylaşımı ve Xamarin ile karma uygulamalar hala erişim temel işlevleri için HTML Razor görünümleri şablon temelli olduğunu gösteren metin okuma API'leri yerel platformdan dahil.

**RazorTodo** uygulama için liste ve düzenleme görünümleri HTML Razor şablonları kullanır. Bu sorundan neredeyse tamamen paylaşılan bir taşınabilir Sınıf Kitaplığı'nda uygulama oluşturabilir anlamına gelir (veritabanı dahil olmak üzere ve **.cshtml** Razor şablon). Aşağıdaki ekran görüntüleri iOS ve Android uygulamalarını gösterir.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** uygulama düzenleme görünümü için bir HTML Razor şablon kullanır, ancak her platformda yerel kaydırma listesini uygular. Bu da dahil olmak üzere avantajları sağlar:

-  Performans - hızlı ve sorunsuz bile çok uzun listeleriyle veri kaydırma emin olmak için sanallaştırma yerel kaydırma denetimleri kullanın.
-  Yerel deneyimi - platforma özel kullanıcı Arabirimi öğeleri olduğu kolayca, iOS ve Android hızlı kaydırma dizin desteği gibi etkin.


Xamarin ile karma uygulamalar oluşturmanın temel bir avantaj tamamen HTML temelli kullanıcı arabirimi (örneğin, ilk örneği) ile başlatın ve sonra (ikinci örnek gösterildiği gibi) gerektiğinde platforma özel işlevsellik eklemek olmalıdır. HTML Razor ve yerel liste ekranlar hem iOS ekranlarda düzenleyin ve Android aşağıda gösterilmektedir.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Özet

Bu makalede, iOS ve yapı kolaylaştırmak Android web görünümü denetimleri kullanılabilir özelliklerini açıklandığı karma uygulamalar.

Ardından Razor şablon motoru ve HTML kolayca kullanarak Xamarin uygulamaları oluşturmak için kullanılan sözdizimi açıklanır. **cshtml** Razor şablon dosyalarını. Mac hızlı bir şekilde izin çözüm şablonları Xamarin ile karma uygulamalar oluşturmaya başlamak için Visual Studio de açıklanmaktadır.

Son olarak, yerel kullanıcı arabirimleri ve API ile web görünümü birleştirmek nasıl göstermek RazorTodo örnekleri kullanıma sunuldu.

### <a name="related-links"></a>İlgili bağlantılar

- [RazorTodo örnek](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 - Razor görüntüleme altyapısı (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Razor sözdizimini (Microsoft) kullanarak ASP.NET Web programlamaya giriş](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
