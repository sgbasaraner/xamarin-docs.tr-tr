---
title: Bir HybridWebView uygulama
description: Xamarin.Forms özel kullanıcı arabirimi denetimlerini düzenleri ve denetimleri ekranında yerleştirmek için kullanılan görünüm sınıfından türetilmelidir. Bu makalede çağrılacak C# kodu JavaScript'ten izin vermek için platforma özgü web denetimleri geliştirmek nasıl gösteren bir HybridWebView özel denetim için özel Oluşturucu Oluşturma gösterilir.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 4adff8a95f9981dbecc44bf177dcd98b7984a3a9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="implementing-a-hybridwebview"></a>Bir HybridWebView uygulama

_Xamarin.Forms özel kullanıcı arabirimi denetimlerini düzenleri ve denetimleri ekranında yerleştirmek için kullanılan görünüm sınıfından türetilmelidir. Bu makalede çağrılacak C# kodu JavaScript'ten izin vermek için platforma özgü web denetimleri geliştirmek nasıl gösteren bir HybridWebView özel denetim için özel Oluşturucu Oluşturma gösterilir._

Her Xamarin.Forms görünüm yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu yok. Zaman bir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) iOS, bir Xamarin.Forms uygulaması tarafından işlenen `ViewRenderer` sınıf örneği, hangi sırayla yerel başlatır `UIView` denetim. Android platformunda `ViewRenderer` sınıfı başlatır bir `View` denetim. Windows Phone ve evrensel Windows Platformu (UWP) `ViewRenderer` sınıfı başlatır yerel `FrameworkElement` denetim. Oluşturucu ve Xamarin.Forms denetimleri Eşle yerel denetim sınıfları hakkında daha fazla bilgi için bkz: [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagram arasındaki ilişkiyi gösterir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ve uyguladıktan karşılık gelen yerel denetimlere:

![](hybridwebview-images/view-classes.png "View sınıfı ve kendi yerel sınıflar arasındaki ilişki")

Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için kullanılan bir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_HybridWebView) `HybridWebView` özel denetim.
1. [Tüketen](#Consuming_the_HybridWebView) `HybridWebView`Xamarin.Forms gelen.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) için özel Oluşturucu `HybridWebView` her platformda.

Her öğe artık sırayla uygulamak için incelenecektir bir `HybridWebView` çağrılacak C# kodu JavaScript'ten izin vermek için platforma özgü web denetimleri geliştirir Oluşturucu. `HybridWebView` Örneği, kişinin adını girmesini ister bir HTML sayfasını görüntülemek için kullanılacak. Kullanıcı bir HTML düğmesine tıkladığında, ardından bir JavaScript işlevi bir C# çağıracağı `Action` kullanıcı adını içeren bir açılır pencere görüntüler.

Çağıran C# JavaScript gelen işlemi hakkında daha fazla bilgi için bkz: [çağırma C# ' dan JavaScript](#Invoking_C_from_JavaScript). HTML sayfası hakkında daha fazla bilgi için bkz: [Web sayfası oluşturma](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>HybridWebView oluşturma

`HybridWebView` Özel denetim sınıflara tarafından oluşturulabilir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

`HybridWebView` Özel denetim taşınabilir sınıf kitaplığı (PCL) projesinde oluşturulur ve denetim için aşağıdaki API tanımlar:

- A `Uri` yüklenmesi web sayfasının adresini belirten özellik.
- A `RegisterAction` kaydeder yöntemi bir `Action` denetimi ile. JavaScript aracılığıyla başvurulan HTML dosyası içinde yer alan gelen kayıtlı eylem çağrılacak `Uri` özelliği.
- A `CleanUp` kayıtlı başvurusunu kaldırır yöntemi `Action`.
- Bir `InvokeAction` kayıtlı çağırır yöntemi `Action`. Bu yöntem, her bir platforma özgü projedeki özel Oluşturucu öğesinden çağrılır.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>HybridWebView kullanma

`HybridWebView` Özel denetim başvurulabilir XAML'de PCL projesinde konumu için bir ad alanı bildirme ve özel denetimi ad alanı öneki kullanarak. Aşağıdaki örnekte gösterildiği kod nasıl `HybridWebView` özel denetim XAML sayfası tarafından tüketilen:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

`local` Ad alanı öneki adlı bir şey. Ancak, `clr-namespace` ve `assembly` değerlerin özel denetim ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde öneki özel denetim başvurmak için kullanılır.

Aşağıdaki örnekte gösterildiği kod nasıl `HybridWebView` özel denetim bir C# sayfası tarafından tüketilen:

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

`HybridWebView` Örneği, bir yerel web denetimi her platformda görüntülemek için kullanılır. Bunun `Uri` özelliği, bir HTML dosyası her platforma özgü projesinde depolanır ve görüntüleneceği yerel web denetimi tarafından ayarlanır. İşlenen HTML kullanıcıların adı, bir JavaScript işlevi çağırma C# girmesini ister `Action` yanıt olarak bir HTML düğmesini tıklatın.

`HybridWebViewPage` Aşağıdaki kod örneğinde gösterildiği gibi JavaScript'ten çağrılacak eylemi kaydeder:

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

Bu eylemi çağıran [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) tarafından görüntülenen HTML sayfası girilen ad sunan kalıcı bir açılır pencere görüntülenecek yöntemi `HybridWebView` örneği.

Özel oluşturucu artık JavaScript'ten çağrılacak C# kodu sağlayarak platforma özgü web denetimleri artırmak için her uygulama projesi eklenebilir.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `ViewRenderer<T1,T2>` özel denetim işler sınıfı. İlk tür bağımsız değişkeni işleyicidir için bu durumda özel denetim olmalıdır `HybridWebView`. İkinci tür bağımsız değişkeni özel görünüm gerçekleştireceksiniz yerel denetimi olmalıdır.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için özel denetim ve yazma mantık işleyen yöntemi. Karşılık gelen Xamarin.Forms özel denetim oluşturulduğunda, bu yöntem çağrılır.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms özel denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Çoğu Xamarin.Forms öğeleri için her platform projesinde özel Oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse, varsayılan oluşturucu denetimin taban sınıfı için kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [Görünüm](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) öğesi.

Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](hybridwebview-images/solution-structure.png "HybridWebView özel Oluşturucu Proje Sorumlulukları")

`HybridWebView` Öğesinden türetilen tüm hangi platforma özgü Oluşturucu sınıflar tarafından işlenen özel denetim `ViewRenderer` her platform için sınıf. Bu her sonuçları `HybridWebView` platforma özgü web denetimleri ile aşağıdaki ekran görüntülerinde gösterildiği gibi işlenen özel denetim:

![](hybridwebview-images/screenshots.png "Her platformda HybridWebView")

`ViewRenderer` Sınıf çıkarır `OnElementChanged` karşılık gelen yerel web denetimi oluşturmak için Xamarin.Forms özel denetim oluşturulduğunda çağrılan yöntemi. Bu yöntem alır bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikleri Xamarin.Forms öğesini temsil, oluşturucu *olan* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olan* bağlı olarak, sırasıyla. Örnek uygulamasında `OldElement` özelliği `null` ve `NewElement` özelliği bir başvuru içerecek `HybridWebView` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, yerel web denetimi örnek oluşturma ve özelleştirme gerçekleştirmek için yerdir. `SetNativeControl` Yöntemi yerel web denetimi örneği oluşturmak için kullanılması gerekir ve bu yöntem aynı zamanda Denetim referansı atayacaktır `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetlemek için bir başvuru aracılığıyla elde edilebilir `Element` özelliği.

Bazı durumlarda `OnElementChanged` yöntemi birden çok kez çağrılabilir. Bu nedenle, bellek sızıntıları önlemek için dikkatli yeni bir yerel denetim başlatılırken alınması gerekir. Aşağıdaki kod örneğinde yaklaşımı özel oluşturucu içinde yeni bir yerel denetim başlatılırken kullanacak şekilde gösterilir:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Yeni bir yerel denetim yalnızca bir kez örneğinin oluşturulması, ne zaman `Control` özelliği `null`. Denetimi yalnızca yapılandırılmalıdır ve olay işleyicileri özel Oluşturucu yeni bir Xamarin.Forms öğesi eklendiğinde abone. Oluşturucu öğesi değişiklikleri iliştirildiğinde benzer şekilde, abone tüm olay işleyicileri yalnızca gelen aboneliği olmalıdır. Bu yaklaşım benimsenmesi bellek sızıntılarını yaşar olmayan bir kullanıcı özel Oluşturucu oluşturmak için yardımcı olur.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` Xamarin.Forms ile oluşturucuyu kaydeder özniteliği. Öznitelik iki parametre – işlenen Xamarin.Forms özel denetim türü adı ve özel Oluşturucu Tür adını alır. `assembly` Özniteliğine öneki belirtir. öznitelik tüm derlemesi için geçerlidir.

Aşağıdaki bölümlerde, her bir yerel web denetimi, işlem bildirilecekse C# ' dan JavaScript için ve her platforma özgü özel Oluşturucu sınıfı bu uygulamasında tarafından yüklenen web sayfasının yapısına açıklanmaktadır.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Web sayfası oluşturma

Aşağıdaki kod örneği tarafından görüntülenen web sayfasını gösteren `HybridWebView` özel denetim:

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
        log(err);
    }
}
</script>
</body>
</html>
```

Web sayfası, adında girmesini sağlar. bir `input` öğesi ve sağlayan bir `button` tıklatıldığında C# kodu çağıracağı öğesi. Bunu elde işlemi aşağıdaki gibidir:

- Kullanıcı tıkladığında üzerinde `button` öğesi, `invokeCSCode` JavaScript işlevi çağrıldığında, değeriyle `input` işlevine geçirilen öğesi.
- `invokeCSCode` İşlev çağrıları `log` göndermeden C# için verileri görüntülemek için işlevi `Action`. Daha sonra çağırır `invokeCSharpAction` C# çağrılacak yöntem `Action`, alınan parametre geçirme `input` öğesi.

`invokeCSharpAction` JavaScript işlevi web sayfasında tanımlı değil ve her özel Oluşturucu tarafından içine eklenecek.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>C# JavaScript içinden çağırma

İşlem için bildirilecekse C# ' dan JavaScript her platformda benzerdir:

- Özel oluşturucu bir yerel web denetimi oluşturur ve belirtilen HTML dosyası yükler `HybridWebView.Uri` özelliği.
- Web sayfası yüklendikten sonra özel Oluşturucu yerleştirir `invokeCSharpAction` web sayfasına JavaScript işlevi.
- Kullanıcı kendi adını girer ve HTML tıklar `button` öğesi, `invokeCSCode` işlev çağrıldığında, hangi sırayla çağırır `invokeCSharpAction` işlevi.
- `invokeCSharpAction` İşlevi çağırır sırayla çağırır özel Oluşturucu yönteminde `HybridWebView.InvokeAction` yöntemi.
- `HybridWebView.InvokeAction` Yöntemini çağırır kayıtlı `Action`.

Aşağıdaki bölümlerde, bu işlem her platformda nasıl uygulandığı ele alınacaktır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde iOS platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

`HybridWebViewRenderer` Sınıfı yükler belirtilen web sayfasını `HybridWebView.Uri` yerel bir özellikte [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) denetimi ve `invokeCSharpAction` JavaScript işlevi web sayfasına eklenmiş. Kullanıcı kendi adını girer ve HTML tıklar sonra `button` öğesi, `invokeCSharpAction` JavaScript işlevi yürütüldüğünde, ile `DidReceiveScriptMessage` web sayfasından bir ileti alındığında çağrılan yöntem. Buna karşılık, bu yöntemi çağırır `HybridWebView.InvokeAction` açılır görüntülemek için kayıtlı eylem çağırma yöntemi.

Bu işlevsellik şu şekilde gerçekleştirilir:

- Koşuluyla `Control` özelliği `null`, aşağıdaki işlemler gerçekleştirilir:
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) örneği oluşturulduğunda, ileti gönderme ve bir web sayfasına kullanıcı komut dosyaları injecting sağlar.
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) örneği eklemesine oluşturulur `invokeCSharpAction` web sayfası yüklendikten sonra web sayfasına JavaScript işlevi.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Yöntemi ekler [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) içerik denetleyici örneği.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Yöntemi ekler adlı bir komut dosyası ileti işleyicisi `invokeAction` için [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) JavaScript işlevinin neden olacak örnek `window.webkit.messageHandlers.invokeAction.postMessage(data)` tümünde tanımlanmamış Çerçeve kullanacağı tüm web görünümlerde `WKUserContentController` örneği.
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) örneği oluşturulduğunda, ile [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) içerik denetleyicisi olarak ayarlanan örneği.
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) denetim örneği ve `SetNativeControl` yöntemi, bir başvuru atamak için çağrılır `WKWebView` denetimini `Control` özelliği.
- Özel oluşturucu yeni bir Xamarin.Forms öğesine bağlı olduğu sağlanan:
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Yöntemi tarafından belirtilen HTML dosyası yükler `HybridWebView.Uri` özelliği. Kod dosyası depolanır belirtir `Content` projenin klasör. Web sayfası görüntülendikten sonra `invokeCSharpAction` JavaScript işlevi web sayfasına eklenmiş.
- Ne zaman Oluşturucu öğesi değişiklikleri eklenir:
  - Kaynakları serbest bırakılır.

> [!NOTE]
> `WKWebView` Sınıfı yalnızca iOS 8 ve üzeri desteklenir.

### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde, Android platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl(string.Format("file:///android_asset/Content/{0}", Element.Uri));
                InjectJS(JavaScriptFunction);
            }
        }

        void InjectJS(string script)
        {
            if (Control != null)
            {
                Control.LoadUrl(string.Format("javascript: {0}", script));
            }
        }
    }
}
```

`HybridWebViewRenderer` Sınıfı yükler belirtilen web sayfasını `HybridWebView.Uri` yerel bir özellikte [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) denetimi ve `invokeCSharpAction` JavaScript işlevi web sayfası yüklendikten sonra web sayfasına eklenen ile `InjectJS` yöntemi. Kullanıcı kendi adını girer ve HTML tıklar sonra `button` öğesi, `invokeCSharpAction` JavaScript işlevi gerçekleştirilir. Bu işlevsellik şu şekilde gerçekleştirilir:

- Koşuluyla `Control` özelliği `null`, aşağıdaki işlemler gerçekleştirilir:
  - Yerel [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) örneği oluşturulur ve JavaScript denetiminde etkindir.
  - `SetNativeControl` Yöntemi yerel bir başvuru atamak için çağrılır [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) denetimini `Control` özelliği.
- Özel oluşturucu yeni bir Xamarin.Forms öğesine bağlı olduğu sağlanan:
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Yöntemi yeni bir yerleştirir `JSBridge` adlandırma WebView'ın JavaScript içerik ana çerçevesine örneği `jsBridge`. Bu yöntemleri sağlar `JSBridge` JavaScript'ten erişilecek sınıfı.
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Yöntemi tarafından belirtilen HTML dosyası yükler `HybridWebView.Uri` özelliği. Kod dosyası depolanır belirtir `Content` projenin klasör.
  - `InjectJS` Yöntemi çağrılır eklemesine `invokeCSharpAction` web sayfasına JavaScript işlevi.
- Ne zaman Oluşturucu öğesi değişiklikleri eklenir:
  - Kaynakları serbest bırakılır.

Zaman `invokeCSharpAction` JavaScript işlevi yürütülürse, buna karşılık çağırır `JSBridge.InvokeAction` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer)) {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

Sınıf öğesinden türetilmelidir `Java.Lang.Object`, ve JavaScript için sunulan yöntemleri donatılmış, ile `[JavascriptInterface]` ve `[Export]` öznitelikleri. Bu nedenle, `invokeCSharpAction` JavaScript işlevi web sayfasına eklenen ve çalıştırılır, bunu çağıracak `JSBridge.InvokeAction` ile donatılmış nedeniyle yöntemi `[JavascriptInterface]` ve `[Export("invokeAction")]` öznitelikleri. Buna karşılık, `InvokeAction` yöntemini çağırır `HybridWebView.InvokeAction` hangi çağrılan açılır görüntülemek için kayıtlı eylem yöntemi.

> [!NOTE]
> Projeleri kullanan `[Export]` özniteliği için bir başvuru içermelidir `Mono.Android.Export`, veya bir derleyici hatasına neden olur.

Unutmayın `JSBridge` sınıfı tutan bir `WeakReference` için `HybridWebViewRenderer` sınıfı. Bu iki sınıf arasında döngüsel bir başvuruya oluşturmamak için yapılır. Daha fazla bilgi için bkz: [zayıf başvurular](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) konusuna bakın.

> [!IMPORTANT]
> Android derleme bildirimi ayarlar üzerinde Android Oreo olun **hedef Android sürümü** için **otomatik**. Aksi takdirde, bu kodu çalıştıran hata iletisi "invokeCSharpAction tanımlı değil" neden olur.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Windows Phone üzerinde özel Oluşturucu Oluşturma ve UWP

Aşağıdaki kod örneği, Windows Phone ve UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

`HybridWebViewRenderer` Sınıfı yükler belirtilen web sayfasını `HybridWebView.Uri` yerel bir özellikte `WebView` denetimi ve `invokeCSharpAction` JavaScript işlevi web sayfası yüklendikten sonra web sayfasına eklenen ile `WebView.InvokeScriptAsync` yöntemi. Kullanıcı kendi adını girer ve HTML tıklar sonra `button` öğesi, `invokeCSharpAction` JavaScript işlevi yürütüldüğünde, ile `OnWebViewScriptNotify` web sayfasından bir bildirim alındıktan sonra çağrılan yöntem. Buna karşılık, bu yöntemi çağırır `HybridWebView.InvokeAction` açılır görüntülemek için kayıtlı eylem çağırma yöntemi.

Bu işlevsellik şu şekilde gerçekleştirilir:

- Koşuluyla `Control` özelliği `null`, aşağıdaki işlemler gerçekleştirilir:
  - `SetNativeControl` Yöntemi yeni bir yerel örneği oluşturmak için çağrılır `WebView` denetlemek ve kendisine bir başvuru atamak `Control` özelliği.
- Özel oluşturucu yeni bir Xamarin.Forms öğesine bağlı olduğu sağlanan:
  - Olay işleyicileri için `NavigationCompleted` ve `ScriptNotify` olayları kayıtlı. `NavigationCompleted` Olay harekete ya da yerel `WebView` denetim geçerli içerik yükleme tamamlandı ya da gezinti başarısız oldu. `ScriptNotify` Olay harekete yerel içeriği `WebView` denetimi uygulamaya bir dizeyi geçirmek için JavaScript kullanır. Web sayfası ateşlenir `ScriptNotify` çağırarak olay `window.external.notify` geçirme sırasında bir `string` parametresi.
  - `WebView.Source` Özelliği tarafından belirtilen URI HTML dosyası olarak ayarlanmış `HybridWebView.Uri` özelliği. Kod dosyası depolanır varsayar `Content` projenin klasör. Web sayfası görüntülendikten sonra `NavigationCompleted` olayını ateşle ve `OnWebViewNavigationCompleted` yöntemi çağrılabilir. `invokeCSharpAction` JavaScript işlevi ardından eklenen ile web sayfasına `WebView.InvokeScriptAsync` yöntemi, sağlanan, gezinti başarıyla tamamlandı.
- Ne zaman Oluşturucu öğesi değişiklikleri eklenir:
  - Gelen aboneliği olaylardır.

## <a name="summary"></a>Özet

Bu makalede için özel Oluşturucu Oluşturma gösterilmiştir bir `HybridWebView` çağrılacak C# kodu JavaScript'ten izin vermek için platforma özgü web denetimleri geliştirmek nasıl oluşturulduğunu gösteren özel denetim.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererHybridWebView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [C# JavaScript'ten çağırın](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
