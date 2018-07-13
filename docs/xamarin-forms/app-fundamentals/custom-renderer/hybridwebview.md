---
title: Bir Hybridwebview'i uygulama
description: Bu makalede, JavaScript'ten çağrılacak C# kod izin vermek için platforma özgü web denetimleri geliştirmek nasıl gösterir bir Hybridwebview'i özel denetim için özel Oluşturucu oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: f0b21277b91c44edbb574aece92664de2e49a65a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996366"
---
# <a name="implementing-a-hybridwebview"></a>Bir Hybridwebview'i uygulama

_Xamarin.Forms özel kullanıcı arabirimi denetimleri, düzenler ve ekrandaki denetimleri yerleştirmek için kullanılan görünüm sınıftan türetilmelidir. Bu makalede, JavaScript'ten çağrılacak C# kod izin vermek için platforma özgü web denetimleri geliştirmek nasıl gösterir bir Hybridwebview'i özel denetim için özel Oluşturucu oluşturma işlemini gösterir._

Her bir Xamarin.Forms görünüm yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici içerir. Olduğunda bir [ `View` ](xref:Xamarin.Forms.View) iOS, bir Xamarin.Forms uygulaması tarafından işlenen `ViewRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `UIView` denetimi. Android platformunda `ViewRenderer` sınıfı örnekleyen bir `View` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `ViewRenderer` sınıfın örneğini oluşturur, bir yerel `FrameworkElement` denetimi. Oluşturucu ve Xamarin.Forms denetimleri eşleyen yerel denetim sınıfları hakkında daha fazla bilgi için bkz. [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagramda arasındaki ilişkiyi gösterir [ `View` ](xref:Xamarin.Forms.View) ve onu uygulayan karşılık gelen yerel denetimler:

![](hybridwebview-images/view-classes.png "Görünüm sınıfını ve kendi yerel sınıflar arasındaki ilişki")

İşleme için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için kullanılan bir [ `View` ](xref:Xamarin.Forms.View) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_HybridWebView) `HybridWebView` özel denetim.
1. [Tüketen](#Consuming_the_HybridWebView) `HybridWebView`Xamarin.Forms öğesinden.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) için özel Oluşturucu `HybridWebView` her platformda.

Her öğe artık sırayla uygulanacağı açıklanmıştır bir `HybridWebView` JavaScript'ten çağrılacak C# kod izin vermek için platforma özgü web denetimleri geliştiren Oluşturucu. `HybridWebView` Örneği, kullanıcıların adlarını girmesi için kullanıcıdan bir HTML sayfasını görüntülemek için kullanılır. Kullanıcı bir HTML düğmesine tıkladığında, ardından bir JavaScript işlevi C# çağıracağı `Action` kullanıcı adını içeren bir açılır pencere görüntüler.

Çağrılıyor C# ' tan JavaScript için işlemi hakkında daha fazla bilgi için bkz. [çağırma C# ' tan JavaScript](#Invoking_C_from_JavaScript). HTML sayfası hakkında daha fazla bilgi için bkz. [Web sayfası oluşturma](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Bir Hybridwebview'i oluşturma

`HybridWebView` Özel denetim tarafından sınıflara oluşturulabilir [ `View` ](xref:Xamarin.Forms.View) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

`HybridWebView` Özel denetim .NET Standard kitaplığı projesinde oluşturulur ve denetim için aşağıdaki API'yi tanımlar:

- A `Uri` yüklenecek web sayfasının adresi belirten özelliği.
- A `RegisterAction` kaydeder yöntemi bir `Action` denetimi ile. Kayıtlı eylem aracılığıyla başvurulan HTML dosyasında yer alan JavaScript'ten çağrılacak `Uri` özelliği.
- A `CleanUp` kayıtlı başvuruyu kaldırır yöntemi `Action`.
- Bir `InvokeAction` kayıtlı çağıran yöntem `Action`. Bu yöntem, gelen her platforma özgü projede özel Oluşturucu çağrılır.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Bir Hybridwebview'i kullanma

`HybridWebView` Özel denetim başvurulabilir XAML içinde .NET Standard kitaplığı projesinde konumu için bir ad alanı bildirmek ve özel denetim üzerinde ad alanı öneki kullanarak. Aşağıdaki kod örnekte gösterildiği nasıl `HybridWebView` özel denetimi bir XAML sayfası tarafından kullanılabilir:

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

`local` Ad alanı ön eki adı herhangi bir şey. Ancak, `clr-namespace` ve `assembly` değerleri, özel denetimin ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde, ön ek özel denetim başvurusu yapmak için kullanılır.

Aşağıdaki kod örnekte gösterildiği nasıl `HybridWebView` özel denetimin bir C# sayfası tarafından kullanılabilir:

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

`HybridWebView` Örneği, bir yerel web denetimi her platformda görüntülemek için kullanılır. Sahip `Uri` her platforma özgü projede depolanan ve görüntüleneceği bir yerel web denetimi tarafından bir HTML dosyası özelliğini ayarlayın. İşlenmiş HTML kullanıcıların adı, bir JavaScript işlevi çağrılırken bir C# girmesini ister `Action` yanıt olarak bir HTML düğmesini tıklatın.

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

Bu eylem çağırır [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) yöntemi tarafından görüntülenen HTML sayfasındaki girdiğiniz ad sunan kalıcı bir açılır pencere görüntülenecek `HybridWebView` örneği.

Özel oluşturucu, artık JavaScript üzerinden çağrılacak C# kodu sağlayarak platforma özgü web denetimleri geliştirmek için her bir uygulama projesine eklenebilir.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `ViewRenderer<T1,T2>` özel kontrolünü icra eden sınıf. Bu durumda olduğu için oluşturucu özel denetimin ilk tür bağımsız değişkeni olmalıdır `HybridWebView`. İkinci tür bağımsız değişkeni özel görünüm uygulayan yerel denetim olmalıdır.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için özel denetim ve Yaz mantığı işleyen yöntemi. Bu yöntem, karşılık gelen Xamarin.Forms özel denetim oluşturulduğunda çağrılır.
1. Ekleme bir `ExportRenderer` bunu Xamarin.Forms özel denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Xamarin.Forms öğelerin çoğu, her platform projesinde özel bir oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse denetimin taban sınıfı için varsayılan oluşturucu kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [görünümü](xref:Xamarin.Forms.View) öğesi.

Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](hybridwebview-images/solution-structure.png "Hybridwebview'i özel Oluşturucu Proje Sorumlulukları")

`HybridWebView` Öğesinden türetilen tüm hangi platforma özel Oluşturucu sınıflar tarafından işlenen özel denetim `ViewRenderer` her platform için sınıf. Bu her sonuçları `HybridWebView` aşağıdaki ekran görüntülerinde gösterildiği gibi platforma özgü web denetimleri ile işlenen özel denetimi:

![](hybridwebview-images/screenshots.png "Her platformda Hybridwebview'i")

`ViewRenderer` Sınıfı kullanıma sunan `OnElementChanged` karşılık gelen yerel web denetimi oluşturmak için Xamarin.Forms özel denetim oluşturulurken çağrılan yöntemi. Bu yöntem bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikler Xamarin.Forms öğesini temsil, oluşturucu *olduğu* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olduğu* bağlı olarak, sırasıyla. Örnek uygulamada `OldElement` özelliği `null` ve `NewElement` özelliği, bir başvuru içerecek `HybridWebView` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, özelleştirme ve yerel web denetimi oluşturmada gerçekleştirmek için yerdir. `SetNativeControl` Yöntemi yerel web denetimi oluşturmak için kullanılması gerekir ve bu yöntem ayrıca Denetim başvurusu atar `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetime başvuru aracılığıyla alınabilir `Element` özelliği.

Bazı durumlarda `OnElementChanged` yöntemi birden çok kez çağrılabilir. Bu nedenle, bellek sızıntılarını önlemek için dikkatli yeni bir yerel denetim örneği oluşturulurken olunması gerekir. Özel oluşturucu içinde yeni bir yerel denetim örneği oluşturulurken kullanılacak bir yaklaşım aşağıdaki kod örneğinde gösterilmiştir:

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

Yeni bir yerel denetim yalnızca bir kez örneği, `Control` özelliği `null`. Denetim yalnızca yapılandırılmalıdır ve özel Oluşturucu yeni bir Xamarin.Forms öğe eklendiğinde olay işleyicileri abone. Değişiklikleri Oluşturucu öğesi eklendiğinde benzer şekilde, abone tüm olay işleyicileri yalnızca gelen aboneliği olması gerekir. Bu yaklaşımı benimsemeyi bellek sızıntılardan etkilese değil yüksek performanslı özel Oluşturucu oluşturmaya yardımcı olur.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` özniteliği işleyici Xamarin.Forms ile kaydeder. Öznitelik, iki parametre – işlenen Xamarin.Forms özel denetimin tür adı ve özel Oluşturucu türü adını alır. `assembly` Özniteliğiyle önek öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki bölümlerde, her bir yerel web denetimi, işlem için çağrılıyor C# ' tan JavaScript ve her platforma özgü özel Oluşturucu sınıfı bu uygulamada tarafından yüklenen web sayfasının yapısı açıklanmaktadır.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Web sayfası oluşturma

Aşağıdaki kod örneği tarafından görüntülenen web sayfasının gösterir `HybridWebView` özel denetimi:

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

Web sayfası sağlar, kullanıcıların adlarını girmesi bir kullanıcı bir `input` öğesi ve sağlar bir `button` öğesi tıklatıldığında C# kodu çağırır. Bunu elde etmenin işlemi aşağıdaki gibidir:

- Kullanıcı tıkladığında üzerinde `button` öğesi `invokeCSCode` JavaScript işlevi çağrılır, değeriyle `input` işlevine geçirilen öğe.
- `invokeCSCode` İşlev çağrılarında `log` göndermeden C# için verileri görüntülemek için işlev `Action`. Ardından `invokeCSharpAction` C# çağrılacak yöntem `Action`, alınan parametre geçirerek `input` öğesi.

`invokeCSharpAction` JavaScript işlevi, web sayfasındaki tanımlanmayan ve her özel Oluşturucu tarafından içine eklenecek.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>C# ' tan JavaScript çağırma

Her platformda çağrılıyor C# ' tan JavaScript için işlem aynıdır:

- Özel oluşturucu bir yerel web denetimi oluşturur ve belirtilen HTML dosyası yükler `HybridWebView.Uri` özelliği.
- Web sayfası yüklendikten sonra özel Oluşturucu ekler `invokeCSharpAction` web sayfasına JavaScript işlevi.
- Kullanıcı adını girer ve HTML tıkladığında `button` öğesi `invokeCSCode` işlev çağrıldığında, hangi sırayla çağırır `invokeCSharpAction` işlevi.
- `invokeCSharpAction` İşlevi çağırır sırayla çağıran özel Oluşturucu yönteminde `HybridWebView.InvokeAction` yöntemi.
- `HybridWebView.InvokeAction` Yöntemini çağıran kayıtlı `Action`.

Aşağıdaki bölümlerde, her platformda bu işlemi nasıl uygulandığını ele alınacaktır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, iOS platformu için özel Oluşturucu gösterir:

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

`HybridWebViewRenderer` Sınıfı belirtilen web sayfasını yükler `HybridWebView.Uri` yerel bir özellikte [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) denetimi ve `invokeCSharpAction` JavaScript işlevi web sayfalarına eklenmiş. Kullanıcı adını girer ve HTML tıkladığında sonra `button` öğesi `invokeCSharpAction` JavaScript işlevi yürütüldüğünde, ile `DidReceiveScriptMessage` web sayfasından bir ileti alındığında çağrılan yöntem. Sırayla bu metodu çağıran `HybridWebView.InvokeAction` açılır görüntülemek için kayıtlı eylemi çağıracak yöntemi.

Bu işlevsellik şu şekilde gerçekleştirilir:

- Koşuluyla `Control` özelliği `null`, şu işlemler gerçekleştirilir:
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) örneği oluşturulur, ileti gönderme ve kullanıcı betikleri bir web sayfasına ekleme sağlar.
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) eklemesine örneği oluşturulan `invokeCSharpAction` web sayfası yüklendikten sonra web sayfasına JavaScript işlevi.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Yöntemi ekler [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) içerik denetleyici örneği.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Yöntemi adlı bir betik ileti işleyicisi ekler `invokeAction` için [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) JavaScript işlevinin neden olacak örneği `window.webkit.messageHandlers.invokeAction.postMessage(data)` tüm tanımlanmalıdır kullanacağınız tüm web görünümleri çerçevelerde `WKUserContentController` örneği.
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) örneği oluşturulduğunda ile [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) örnek içerik denetleyicisi olarak ayarlanıyor.
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) denetimi oluşturulana ve `SetNativeControl` yöntemi, bir başvuru atamak için çağrılır `WKWebView` denetimini `Control` özelliği.
- Özel oluşturucu yeni bir Xamarin.Forms öğesine bağlı olmasını sağladı:
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Yöntemi tarafından belirtilen HTML dosyası yükler `HybridWebView.Uri` özelliği. Kod dosyası depolanır belirtir `Content` proje klasörü. Web sayfası görüntülendikten sonra `invokeCSharpAction` JavaScript işlevi web sayfalarına eklenmiş.
- Ne zaman ' % s'öğesi işleyici değişiklikleri eklenir:
  - Kaynakları serbest bırakılır.

> [!NOTE]
> `WKWebView` Sınıfı yalnızca iOS 8 ve üzeri desteklenir.

### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, Android platformu için özel Oluşturucu gösterir:

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

`HybridWebViewRenderer` Sınıfı belirtilen web sayfasını yükler `HybridWebView.Uri` yerel bir özellikte [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) denetimi ve `invokeCSharpAction` JavaScript işlevi web sayfası yüklendikten sonra web sayfasına eklenen ile `InjectJS` yöntemi. Kullanıcı adını girer ve HTML tıkladığında sonra `button` öğesi `invokeCSharpAction` JavaScript işlev yürütülür. Bu işlevsellik şu şekilde gerçekleştirilir:

- Koşuluyla `Control` özelliği `null`, şu işlemler gerçekleştirilir:
  - Yerel [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) örneği oluşturulur ve JavaScript denetimi etkinleştirildi.
  - `SetNativeControl` Yerel başvuru atamak yöntemi çağrıldığında [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) denetimini `Control` özelliği.
- Özel oluşturucu yeni bir Xamarin.Forms öğesine bağlı olmasını sağladı:
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Yöntemi yeni bir eklediği `JSBridge` adlandırma WebView'ın JavaScript içerik ana çerçeve içine örneğini `jsBridge`. Bu yöntemleri tanır `JSBridge` JavaScript'ten erişilecek sınıf.
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Yöntemi tarafından belirtilen HTML dosyası yükler `HybridWebView.Uri` özelliği. Kod dosyası depolanır belirtir `Content` proje klasörü.
  - `InjectJS` Metodunu çağırmak eklemesine `invokeCSharpAction` web sayfasına JavaScript işlevi.
- Ne zaman ' % s'öğesi işleyici değişiklikleri eklenir:
  - Kaynakları serbest bırakılır.

Zaman `invokeCSharpAction` JavaScript işlevi yürütüldüğünde, sırayla çağırır `JSBridge.InvokeAction` aşağıdaki kod örneğinde gösterilen yöntemi:

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

Sınıf öğesinden türetilmelidir `Java.Lang.Object`, ve JavaScript için kullanıma sunulan yöntemleri düzenlenmiş, ile `[JavascriptInterface]` ve `[Export]` öznitelikleri. Bu nedenle, `invokeCSharpAction` JavaScript işlevi web sayfalarına eklenmiş olur ve çalıştırılır, çağırır `JSBridge.InvokeAction` yöntemi ile donatılmış nedeniyle `[JavascriptInterface]` ve `[Export("invokeAction")]` öznitelikleri. Buna karşılık, `InvokeAction` yöntemini çağıran `HybridWebView.InvokeAction` oluşturacak çağrılan açılır görüntülemek için kayıtlı eylem yöntemi.

> [!NOTE]
> Projeleri `[Export]` özniteliği, bir başvuru içermelidir `Mono.Android.Export`, veya bir derleyici hatasına neden olur.

Unutmayın `JSBridge` sınıfı tutan bir `WeakReference` için `HybridWebViewRenderer` sınıfı. Bu, iki sınıf arasında döngüsel bir başvuru oluşturuyor önlemek içindir. Daha fazla bilgi için [zayıf başvurular](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) MSDN'de.

> [!IMPORTANT]
> Android bildirimi ayarlar üzerinde Android Oreo olun **hedef Android sürümü** için **otomatik**. Aksi takdirde, bu kod çalıştırma hata iletisi "invokeCSharpAction tanımlı değil" neden olur.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP üzerinde özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
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

`HybridWebViewRenderer` Sınıfı belirtilen web sayfasını yükler `HybridWebView.Uri` yerel bir özellikte `WebView` denetimi ve `invokeCSharpAction` JavaScript işlevi web sayfası yüklendikten sonra web sayfasına eklenen ile `WebView.InvokeScriptAsync` yöntemi. Kullanıcı adını girer ve HTML tıkladığında sonra `button` öğesi `invokeCSharpAction` JavaScript işlevi yürütüldüğünde, ile `OnWebViewScriptNotify` web sayfasından bir bildirim alındıktan sonra çağrılan yöntem. Sırayla bu metodu çağıran `HybridWebView.InvokeAction` açılır görüntülemek için kayıtlı eylemi çağıracak yöntemi.

Bu işlevsellik şu şekilde gerçekleştirilir:

- Koşuluyla `Control` özelliği `null`, şu işlemler gerçekleştirilir:
  - `SetNativeControl` Yöntemi yeni bir yerel örneği oluşturmak için çağrılır `WebView` denetlemek ve ona bir başvuru atama `Control` özelliği.
- Özel oluşturucu yeni bir Xamarin.Forms öğesine bağlı olmasını sağladı:
  - Olay işleyicileri `NavigationCompleted` ve `ScriptNotify` olayları kayıtlı. `NavigationCompleted` Olayı tetikler ya da yerel `WebView` denetimi geçerli içerik yükleme tamamlandı veya gezinti başarısız oldu. `ScriptNotify` Olayı tetikler yerel içeriği `WebView` denetimi, uygulama için bir dizeyi geçirmek için JavaScript kullanır. Web sayfası ateşlenir `ScriptNotify` çağırarak olay `window.external.notify` geçirilirken bir `string` parametresi.
  - `WebView.Source` Özelliği tarafından belirtilen URI HTML dosyasının kümesine `HybridWebView.Uri` özelliği. Kod dosyanın depolandığını varsayar `Content` proje klasörü. Web sayfası görüntülendikten sonra `NavigationCompleted` olayını ateşle ve `OnWebViewNavigationCompleted` yöntemi çağrılacak. `invokeCSharpAction` JavaScript işlevi ardından eklenen ile web sayfasına `WebView.InvokeScriptAsync` yöntemi, sağlanan, gezinti başarıyla tamamlandı.
- Ne zaman ' % s'öğesi işleyici değişiklikleri eklenir:
  - Gelen aboneliği olaylardır.

## <a name="summary"></a>Özet

Bu makalede için özel Oluşturucu Oluşturma gösterilmiştir bir `HybridWebView` JavaScript'ten çağrılacak C# kod izin vermek için platforma özgü web denetimleri geliştirmek nasıl erişileceğini gösteren özel bir denetim.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererHybridWebView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [C# JavaScript'ten çağırın](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
