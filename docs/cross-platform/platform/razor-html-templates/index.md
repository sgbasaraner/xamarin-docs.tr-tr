---
title: Razor şablonları kullanarak yapı HTML görünümleri
description: " HTML oluşturmak için tam ekran Web sayfasını kullanarak, karmaşık bir platformlar arası şekilde biçimlendirme özellikle HTML, Javascript ve CSS bir Web sitesi projesinden zaten varsa işlemek için basit ve etkili bir yol olabilir."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 07/24/2018
ms.openlocfilehash: 7e569aaddef912d9534e98f2f987ad5dfca8a5a6
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270138"
---
# <a name="building-html-views-using-razor-templates"></a>Razor şablonları kullanarak yapı HTML görünümleri

Mobil Geliştirme dünyasında "karma uygulama" terimi genellikle kendi ekranları bazı (veya tümü) olarak HTML sayfaları içinde barındırılan web Görüntüleyici denetimi sunan bir uygulamaya anlamına gelir.

İzin bazı geliştirme ortamları vardır ancak bu uygulamaların Performans sorunlarında karmaşık bir işlem veya kullanıcı Arabirimi etkileri gerçekleştirmek çalışırken zarar görebilir ve olan platform da sınırlı HTML ve Javascript içinde tamamen mobil uygulamanızı oluşturun kullanıcıların erişebilmesi için özellikler.

Xamarin, özellikle HTML Razor şablon oluşturma altyapısı kullanırken her iki platformdan da sunar. Xamarin ile Javascript ve CSS kullanan, ancak temel platform API'leri ve C# kullanarak hızlı işleme tam erişim de platformlar arası şablonlu HTML görünümlerini oluşturma esnekliğine sahip olursunuz.

Bu belge, Razor şablon oluşturma altyapısı yapı kullanarak Xamarin mobil platformlarda kullanılan HTML + Javascript + CSS görünümleri kullanmayı açıklar.

## <a name="using-web-views-programmatically"></a>Program aracılığıyla Web görünümleri kullanma

Bu bölüm, biz Razor hakkında bilgi edinin önce doğrudan – HTML içeriğini görüntülemek için web görünümleri kullanmak nasıl özel bir uygulama içinde oluşturulan HTML içeriği kapsar.

Oluşturma ve C# kullanarak HTML görüntüleme kolaydır hem iOS hem de Android, Xamarin API'leri temel alınan platforma tam erişim sağlar. Her platform için temel sözdizimi aşağıda gösterilmiştir.

### <a name="ios"></a>iOS

HTML Xamarin.iOS UIWebView denetiminde görüntüleme, yalnızca birkaç satır kod alır:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Bkz: [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) tarifler UIWebView denetimi kullanma hakkında daha fazla bilgi.

### <a name="android"></a>Android

HTML Xamarin.Android kullanan bir WebView denetiminde görüntüleme, yalnızca birkaç kod satırıyla gerçekleştirilir:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable Javascript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Bkz: [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) tarifleri WebView denetiminde kullanma hakkında daha fazla bilgi.

### <a name="specifying-the-base-directory"></a>Temel dizin belirtme

Her iki platformlarda HTML sayfasının temel dizinini belirten bir parametresi vardır. Bu görüntüleri ve CSS dosyaları gibi kaynaklara göreli başvurularını çözümlemek için kullanılan cihazın dosya sistemi konumdur. Örneğin, etiketleri gibi

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

Bu dosyaya başvurmuyor: **style.css**, **monkey.jpg** ve **jscript.js**. Temel dizin ayarı sayfasına yüklenen olabilmeleri bu dosyaların bulunduğu web görünümünde söyler.

#### <a name="ios"></a>iOS

Şablon çıktısı, aşağıdaki C# kod ile iOS oluşturulur:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Temel dizin olarak belirtilen `NSBundle.MainBundle.BundleUrl` uygulamanın yüklü dizine başvurur. Tüm dosyaları **kaynakları** klasörüne kopyalanıp kopyalanmayacağını bu konuma gibi **style.css** burada gösterilen dosya:

 ![iPhoneHybrid çözümü](images/image1_240x163.png)

Tüm statik içerik dosyaları için derleme eylemi olmalıdır **BundleResource**:

 ![iOS projenizi eylem: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android, html dizelerinde bir web görünümü'nde görüntülendiğinde, parametre olarak geçirilecek bir temel dizin de gerektirir.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Özel dize **file:///android_asset/** içeren burada gösterilen uygulama, Android varlıklarını klasöründe başvurduğu **style.css** dosya.

 ![AndroidHybrid çözümü](images/image3_240x167.png)

Tüm statik içerik dosyaları için derleme eylemi olmalıdır **AndroidAsset**.

 ![Android projesi derleme eylemi: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>HTML ve Javascript C# ' ı çağırma

Bir html sayfası web görünümüne yüklendiğinde, sayfa sunucudan yüklendiyse olduğu gibi bağlantı ve forms değerlendirir. Bu, kullanıcı bir bağlantıya tıklar veya form gönderen web görünümü belirtilen hedef gitmek deneyeceğini anlamına gelir.

Bağlantı dış bir sunucuya (örneğin, google.com) ise web görünümü (Internet bağlantısı varsayılarak) dış Web sitesi yüklemeyi deneyecek.

```html
<a href="http://google.com/">Google</a>
```

Bağlantıyı göreli ise web görünümü temel dizin içeriği yüklemeyi deneyecek. Cihazda uygulama içerik depolanırken açıkça hiçbir ağ bağlantısı Bunun çalışması için gereklidir.

```html
<a href="somepage.html">Local content</a>
```

Form eylemlerini aynı kural izleyin.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

İstemci web sunucusunda barındırmak için alınmamaktadır; Ancak, günümüzün esnek tasarım desenlerini işe aynı sunucu iletişimi teknikleri HTTP GET hizmetlerini çağırmak için kullanın ve Javascript yayma tarafından yanıtları zaman uyumsuz olarak işlemek (veya çağıran Javascript web görünümü'nde zaten barındırılan). Bu, kolayca yeniden işleme sonra sonuçları HTML sayfasında yeniden görüntü için C# kodu içine HTML veri iletmek sağlar.

Hem iOS hem de Android için uygulama kodu, uygulama kodu (gerekliyse) yanıt verebilir böylece bu gezinti olayların ele alınması bir mekanizma sağlar. Bu özellik, yerel kod web görünümü ile etkileşime izin verdiğinden, karma uygulamalar oluşturmak için çok önemlidir.

#### <a name="ios"></a>iOS

İOS web görünümünde ShouldStartLoad olayı uygulama kodu (örneğin, bir bağlantıya tıklayın) bir gezinti isteği işlemeye izin vermek için geçersiz kılınabilir. Yöntem parametreleri tüm bilgileri sağlayın

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

' i tıklatın ve sonra olay işleyicisi atayın:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Android'de yalnızca alt WebViewClient ve sonra Gezinti isteğine yanıt vermek için uygulama kodu.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

ve ardından istemci web görünümünde ayarlayın:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>C# ' tan JavaScript çağırma

Yeni bir HTML sayfası yüklemek için bir web görünümü belirten ek olarak, C# kodu ayrıca Javascript şu anda görüntülenen sayfa içinde çalıştırabilirsiniz. Tüm Javascript kod blokları dizeleri C# kullanarak oluşturulabilir ve yürütülen veya Javascript sayfasında zaten kullanılabilir yöntem çağrılarını oluşturabileceği `script` etiketler.

#### <a name="android"></a>Android

İle ön ek ve yürütülmesi için Javascript kodunu oluşturma "javascript:" ve o dizeyi yüklemek üzere web görünümüne bildirin:

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

Bu bölümde web görünümü denetimleri bize, Xamarin ile karma uygulamalar oluşturmanıza olanak tanıyan iOS ve Android cihazlardaki özellikleri kullanıma sundu dahil olmak üzere:

-  HTML kod içinde oluşturulan dizeleri yükleme olanağı
-  Yerel dosyaları (CSS, Javascript, görüntüleri veya diğer HTML dosyaları), başvurabilirsiniz
-  C# kodunda gezinme istekleri kesmenize olanağı
-  JavaScript, C# kod arama olanağı.


Sonraki bölümde kullanmak için HTML karma uygulamalar oluşturmak kolaylaştıran Razor tanıtır.

## <a name="what-is-razor"></a>Razor nedir?

Razor, başlangıçta sunucusunda çalıştırın ve web tarayıcılarına alınacağı HTML oluşturmak için ASP.NET MVC ile sunulan bir şablon oluşturma motorudur.

Düzen express ve CSS stil sayfaları ve Javascript bir kolayca eklemesine standart HTML sözdizimi, C# ile Razor şablon oluşturma altyapısını genişletir. Şablon, özel bir tür olabilen ve özelliklerini şablonu doğrudan erişilebilen bir Model sınıfı başvurabilirsiniz. Bunun başlıca avantajlarından HTML ve C# sözdizimi bir kolayca birlikte biridir.

Razor şablonları için sunucu tarafı kullanım sınırlı değildir, Xamarin uygulamaları da eklenebilir. Program aracılığıyla web görünümleri ile çalışmak için Razor şablonları özelliği ile birlikte kullanarak, Xamarin ile oluşturulacak Gelişmiş platformlar arası karma uygulamalar sağlar.

### <a name="razor-template-basics"></a>Razor şablon temelleri

Razor şablon dosyalarına sahip bir **.cshtml** dosya uzantısı. Metin şablonu oluşturma bölümünden bir Xamarin projesi için bunlar eklenebilir **yeni dosya** iletişim:

 ![Yeni dosya - Razor şablonu](images/image5_400x201.png)

Basit bir Razor şablonu ( **RazorView.cshtml**) aşağıda gösterilmiştir.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Normal bir HTML dosyası çizgisiyle aşağıdaki farkları dikkat edin:

-  `@` Sembol, Razor şablonlarına özel bir anlamı vardır: aşağıdaki ifade uyumluluğunun değerlendirilebilmesi için C# olduğunu gösterir.
- `@model` yönergesi, her zaman bir Razor şablon dosyasının ilk satırı görünür.
-  `@model` Yönergesi bir tür tarafından izlenen. Bu örnekte basit bir dize şablona geçirilir, ancak özel bir sınıf olması neden olmuş olabilir.
-  Zaman `@Model` başvurulan isteğe bağlı olarak şablonun tamamında (bir dize olacak Bu örnekte) oluşturulduğunda şablona geçirilen nesnesine bir başvuru sağlar.
-  IDE kısmi sınıf şablonları için otomatik olarak oluşturur (ile dosyaları **.cshtml** uzantısı). Bu kodu görüntüleyebilirsiniz ancak düzenlenemez.
 ![RazorView.cshtml](images/image6_125x34.png) kısmi sınıf RazorView .cshtml şablon dosya adıyla eşleşecek şekilde adlandırılır. C# kodu şablonda başvurmak için kullanılan bu adıdır.
- `@using` deyimleri, ayrıca ek ad alanları eklemek için bir Razor şablonu üstüne dahil edilebilir.


Son HTML çıkışı, ardından aşağıdaki C# kod ile oluşturulabilir. "Hello, işlenmiş şablon çıkış eklenecektir World" bir dize olacak şekilde modeli belirttiğimiz unutmayın.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

İOS simülatörü ve Android öykünücüsü üzerinde bir web görünümü'nde gösterilen çıktısı aşağıdaki gibidir:

 ![Merhaba Dünya](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Daha fazla Razor sözdizimi

Başlamanıza yardımcı olmak için bazı temel Razor sözdizimi tanıtmak için yapacağız Bu bölümde, kullanıyor. Bu bölümdeki örneklerde, aşağıdaki sınıf verilerle doldurmak ve Razor kullanarak görüntüleyin:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Tüm örnekler için aşağıdaki veri başlatma kodu kullanın

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Model özelliklerini görüntüleme

Model bir sınıf özelliklere sahip olduğunda, Razor şablon kolayca bu örnek şablonda görüldüğü gibi başvurulur:

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

Son çıkış web görünümünde iOS simülatörü ve Android öykünücüsü'nü aşağıda gösterilmiştir:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>C# ifadeleri

Daha karmaşık C# Model özellik güncelleştirmeleri ve bu örnekte yaş hesaplama gibi bir şablon eklenebilir:

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

(Yaş biçimlendirme gibi) karmaşık tek satır C# ifadeleri tarafından kod ile çevreleyen yazabileceğiniz `@()`.

Birden çok C# ifadeleri ile çevreleyen tarafından yazılabilir `@{}`.

#### <a name="if-else-statements"></a>İf-else deyimleri

Kod dalları ifade edilemez ile `@if` Bu şablon örneğinde gösterildiği gibi.

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

Döngü yapıları gibi `foreach` da eklenebilir. `@` Önek üzerinde döngü değişkeninin kullanılabilir ( `@food` bu durumda) HTML işlemek için.

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

Yukarıdaki şablonu çıktısını iOS simülatörü ve Android öykünücüsü'nü çalıştıran gösterilmektedir:

 ![Rupert X Monkey](images/image9_520x277.png)

Bu bölümde, basit bir salt okunur görünüm işlemek için Razor şablonları kullanmanın temellerini kapsamına. Sonraki bölümde, kullanıcı girişi kabul edebilir ve Javascript için HTML görünümü ve C# arasında birlikte çalışmak Razor kullanarak daha kapsamlı uygulamaları nasıl oluşturacağınızı açıklar.

## <a name="using-razor-templates-with-xamarin"></a>Razor şablonları Xamarin ile kullanma

Bu bölümde, çözüm şablonları, Mac için Visual Studio kullanarak kendi karma uygulamanızı derleme kullanmayı açıklar Üç şablonları kullanılabilir **Dosya > Yeni > çözüm...**  penceresi:

- **Android > Uygulama > Android WebView uygulaması**
- **iOS > Uygulama > WebView uygulaması**
- **ASP.NET MVC projesi**



**Yeni çözüm** penceresi görünür, bu gibi iPhone ve Android projeleri için - Razor şablon oluşturma altyapısı için destek çözümü açıklama sağdaki vurgular.

 ![İPhone ve Android çözümleri oluşturma](images/image13_1139x959.png)

Kolayca ekleyebileceğiniz Not bir **.cshtml** Razor şablonu *herhangi* Xamarin projesi var, bu çözüm şablonları gerekli değildir. iOS projeleri Razor ya da kullanmak bir görsel taslak gerektirmez; yalnızca UIWebView denetim için herhangi bir görünüm programlı olarak ekleyin ve Razor şablonları tüm C# kodunda işleyebilirsiniz.

İPhone ve Android projeleri için varsayılan şablon çözüm içeriği aşağıda verilmiştir:

 ![iPhone ve Android şablonları](images/image10_428x310.png)

Şablonları Razor şablonu bir veri modeli nesnesi ile yüklemek için kullanıcı girişini işlemek ve Javascript aracılığıyla kullanıcıya iletişim kurmak için hazır go uygulaması altyapısı sağlar.

Çözüm önemli bölümleri şunlardır:

-  Statik içerik gibi **style.css** dosya.
-  Razor .cshtml şablon dosyaları ister **RazorView.cshtml** .
-  Razor şablonları gibi başvurulan sınıfların model **ExampleModel.cs** .
-  Web görünümü oluşturur ve şablon gibi işler platforma özgü sınıf `MainActivity` Android üzerinde ve `iPhoneHybridViewController` ios'ta.


Aşağıdaki bölümde, projelerin nasıl çalıştığı açıklanmaktadır.

### <a name="static-content"></a>Statik İçerik

CSS stil sayfaları, görüntüler, Javascript dosyaları veya diğer içeriklere bağlantı veya bir web görünümü'nde görüntülenen bir HTML dosyası tarafından başvurulan statik içerik içerir.

Statik içerik dahil karma uygulamanızı nasıl yükleneceğini göstermek için bir en az bir stil sayfası şablonu projeleri içerir. CSS stil sayfası şablonu bu gibi başvurulur:

```html
<link rel="stylesheet" href="style.css" />
```

Hangi stil sayfası ve JQuery gibi çerçeveleri de dahil olmak üzere ihtiyacınız olan Javascript dosyaları ekleyebilirsiniz.

### <a name="razor-cshtml-templates"></a>Razor cshtml şablonları

Bir Razor şablonu içerir **.cshtml** önceden yazılmış HTML/Javascript ve C# arasında veri iletmek için kod dosyası. Bu izin yoksa yalnızca Model salt okunur verileri görüntülemek ancak kullanıcı girişini HTML kabul da geçirin karmaşık karma uygulamalar yeniden işleme veya depolama için C# kodu derleme.

#### <a name="rendering-the-template"></a>Şablon oluşturma

Çağırma `GenerateString` şablona HTML web görünümünde görüntülenmesi için hazır olarak işler. Ardından şablonu bir modeli kullanıyorsa, önce işleme sağlanmalıdır. Bu diyagram oluşturma – statik kaynaklar tarafından belirtilen dosyalarını bulmak için sağlanan temel dizinini kullanarak, çalışma zamanında web görünümü çözümlendiği işleyişi değil gösterir.

 ![Razor akış çizelgesi](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Şablondan C# kodu çağırma

C# için geri çağırma işlenmiş web görünümü gelen iletişimi web görünümünde URL'sini ayarlama ve sonra web görünümü yeniden olmadan yerel istek işlemeye istek C# kesintiye gerçekleştirilir.

Bir örnek RazorView'ın düğmesi nasıl işlendiğini içinde görülebilir. Düğme, aşağıdaki HTML'yi sahiptir:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Javascript işlevi okur, tüm değerleri, ayarlar ve HTML Form `location.href` web görünümü için:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

Bir URL (ör. özel bir düzen ile web görünümüne gitmek bu çalışır `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Yerel web görünümünde bu gezinme isteği işlerken, müdahale fırsatı sahibiz. İOS, bu UIWebView'ın HandleShouldStartLoad olay işleme tarafından gerçekleştirilir. Android, biz yalnızca WebViewClient biçiminde kullanılan ve ShouldOverrideUrlLoading geçersiz alt.

Bu iki Gezinti dinleyicileri içyüzü temelde aynıdır.

İlk olarak, web görünümü yüklenmeye çalışırken URL'yi denetleyin ve özel şema ile başlatılmazsa (`hybrid:`), normal olarak gerçekleşmesi Gezinti izin.

Özel URL şeması için URL'nin şeması arasındaki her şeyi ve "?" (Bu durumda "UpdateLabel") işlenecek yöntemi adıdır. Sorgu dizesinde her şeyi yöntem çağrısına parametre olarak kabul edilir:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` Bu örnekte dize düzenlemesi ("diyor C# dizeye" eklenmesini) metin kutusuna parametresi üzerinde en az miktarda yapar ve web görünümüne geri çağırır.

Web görünümü son özel URL'ye geçerken çalışmaz, URL işleme sonra Gezinti yöntemi durdurur.

#### <a name="manipulating-the-template-from-c"></a>C# şablonu düzenleme

İşlenmiş HTML web görünümü iletişimi C#, Javascript web görünümünde çağrılarak gerçekleştirilir. İos'ta bu çağrılarak gerçekleştirilir `EvaluateJavascript` UIWebView üzerinde:

```csharp
webView.EvaluateJavascript (js);
```

Android, Javascript web görünümünde Javascript kullanarak bir URL olarak yükleyerek çağrılabilir `"javascript:"` URL şeması:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Uygulama yaparak tamamen karma

Bu şablonlar haline getirmez her platformda yerel denetimleri kullanın – ekranın tamamını tek bir web görünümü ile doldurulur.

HTML prototip oluşturma için harika olabilir ve nesnelerin interneti türlerini görüntüleme web zengin metin ve duyarlı düzen gibi en iyi şekilde. Ancak tüm görevleri, HTML ve Javascript verileri uzun listeler kaydırma – uygundur, örneğin, daha iyi yerel kullanıcı Arabirimi denetimleri (iOS üzerinde UITableView) veya ListView gibi Android'de kullanarak gerçekleştirir.

Şablondaki web görünümleri kolayca platforma özgü denetimleriyle – yalnızca düzenleme Genişletilebilir **MainStoryboard.storyboard** iOS Tasarımcısı'nda veya **Resources/layout/Main.axml** Android.

### <a name="razortodo-sample"></a>RazorTodo örnek

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) depo, tamamen HTML temelli bir uygulamayı HTML yerel denetimleri ile birleştiren bir uygulama arasındaki farkı göstermek için iki ayrı çözümler içerir:

-  **RazorTodo** -Razor şablonları kullanarak tamamen HTML temelli bir uygulama.
-  **RazorNativeTodo** - iOS ve Android için yerel bir liste görünümü denetimi kullanır ancak ile HTML ve Razor düzenleme ekranını görüntüler.


Hem iOS hem de taşınabilir sınıf kitaplıkları (veritabanı ve model sınıfları gibi ortak kod paylaşmak için PCLs) kullanarak Android, Xamarin uygulamaları bu çalıştırın. Razor **.cshtml** şablonları da dahil edilebilir PCL'de platformlar arasında kolayca paylaşılabilir şekilde.

Her iki örnek uygulama Twitter paylaşımı ve Xamarin ile karma uygulamalar erişime sahip olmaya devam temel işlevleri için HTML Razor görünümleri şablon temelli olduğunu gösteren metin okuma API'leri yerel platformundan dahil edilip derecelendirilir.

**RazorTodo** uygulama listesi ve Düzen görünümleri için HTML Razor şablonları kullanır. Bu ediyoruz paylaşılan taşınabilir sınıf kitaplığı uygulamada neredeyse tamamen oluşturabilirsiniz anlamına gelir (veritabanı dahil olmak üzere ve **.cshtml** Razor şablonlar). Aşağıdaki ekran görüntüleri, iOS ve Android uygulamaları gösterir.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** uygulama düzenleme görünümü için bir HTML Razor şablonu kullanır, ancak her platformda yerel kayan listesini uygular. Bu, çok sayıda dahil olmak üzere fayda sağlar:

-  -Performans, hızlı ve kesintisiz bile verileri uzun listesi ile kaydırma emin olmak için sanallaştırma yerel kayan denetimleri kullanın.
-  Yerel deneyimi - platforma özgü kullanıcı Arabirimi öğeleri olan kolayca hızlı kaydırma dizini desteği olan iOS ve Android gibi etkin.


Xamarin ile karma uygulamalar oluşturmanın önemli bir avantajı, (İlk örnek gibi) tamamen HTML tabanlı kullanıcı arabirimi ile başlatın ve ardından (ikinci örnek gösterildiği gibi) gerektiğinde, platforma özel işlevsellik eklemek ' dir. Yerel liste ekranları ve HTML Razor hem iOS ekranlarda düzenleyin ve Android aşağıda gösterilmektedir.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Özet

Bu makalede, iOS ve Android'de yapı kolaylaştıran özellikler kullanılabilir web görünümü denetimleri açıklanan hibrit uygulamalar.

Ardından, Razor şablon oluşturma altyapısı ve HTML kullanarak Xamarin uygulamaları kolayca oluşturmak için kullanılan söz dizimi açıklanmıştır. **cshtml** Razor şablon dosyaları. Hızlı bir şekilde olanak tanıyan çözüm şablonları, Xamarin ile karma uygulamaları oluşturmaya başlayın Mac için Visual Studio de açıklanmaktadır.

Son olarak web görünümleri yerel kullanıcı arabirimleri ve API'leri ile birleştirerek nasıl gösteren RazorTodo örnekler kullanıma sunuldu.

### <a name="related-links"></a>İlgili bağlantılar

- [RazorTodo örnek](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 - Razor görünüm altyapısı (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [ASP.NET Web programlama Razor söz dizimini (Microsoft) kullanarak giriş](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
