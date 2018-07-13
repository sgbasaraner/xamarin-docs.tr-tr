---
title: Bir görünümü uygulama
description: Bu makalede, cihazın kamerasını Önizleme video akıştan görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu oluşturma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 8ee9926eb3b726673711141e7c75a68b607d02d3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994708"
---
# <a name="implementing-a-view"></a>Bir görünümü uygulama

_Xamarin.Forms özel kullanıcı arabirimi denetimleri, düzenler ve ekrandaki denetimleri yerleştirmek için kullanılan görünüm sınıftan türetilmelidir. Bu makalede, cihazın kamerasını Önizleme video akıştan görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu oluşturma işlemini gösterir._

Her bir Xamarin.Forms görünüm yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici içerir. Olduğunda bir [ `View` ](xref:Xamarin.Forms.View) iOS, bir Xamarin.Forms uygulaması tarafından işlenen `ViewRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `UIView` denetimi. Android platformunda `ViewRenderer` sınıfın örneğini oluşturur, bir yerel `View` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `ViewRenderer` sınıfın örneğini oluşturur, bir yerel `FrameworkElement` denetimi. Oluşturucu ve Xamarin.Forms denetimleri eşleyen yerel denetim sınıfları hakkında daha fazla bilgi için bkz. [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagramda arasındaki ilişkiyi gösterir [ `View` ](xref:Xamarin.Forms.View) ve onu uygulayan karşılık gelen yerel denetimler:

![](view-images/view-classes.png "Görünüm sınıfını ve kendi yerel sınıflar arasındaki ilişki")

İşleme için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için kullanılan bir [ `View` ](xref:Xamarin.Forms.View) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Control) Xamarin.Forms özel bir denetim.
1. [Tüketen](#Consuming_the_Custom_Control) Xamarin.Forms özel denetimden.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda denetimi için özel Oluşturucu.

Her öğe artık sırayla uygulanacağı açıklanmıştır bir `CameraPreview` cihazın kamerasını Önizleme video akıştan görüntüler Oluşturucu. Video akışı üzerinde dokunma durdurun ve başlatın.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Özel denetim oluşturma

Özel bir denetim tarafından sınıflara oluşturulabilir [ `View` ](xref:Xamarin.Forms.View) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

`CameraPreview` Özel denetim taşınabilir sınıf kitaplığı (PCL) projesi oluşturulur ve denetim için API tanımlar. Özel denetim sunan bir `Camera` video akışı ön veya cihazdaki arka kamera görüntülenip görüntülenmeyeceğini denetlemek için kullanılan özellik. İçin bir değer belirtilmezse `Camera` denetimi oluşturulduğunda özelliği, varsayılan arka kamera belirtmek için.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Özel denetim kullanma

`CameraPreview` Özel denetim başvurulabilir XAML içinde PCL projeye bir ad alanı konumu için bildirme ve özel denetim öğesi üzerinde ad alanı öneki kullanarak. Aşağıdaki kod örnekte gösterildiği nasıl `CameraPreview` özel denetimi bir XAML sayfası tarafından kullanılabilir:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` Ad alanı ön eki adı herhangi bir şey. Ancak, `clr-namespace` ve `assembly` değerleri, özel denetimin ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde, ön ek özel denetim başvurusu yapmak için kullanılır.

Aşağıdaki kod örnekte gösterildiği nasıl `CameraPreview` özel denetimin bir C# sayfası tarafından kullanılabilir:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Örneği `CameraPreview` özel denetim, cihazın kamerasını Önizleme video akıştan görüntülemek için kullanılır. İsteğe bağlı olarak için bir değer belirterek yanı sıra `Camera` özelliğinin, denetimin özelleştirme özel oluşturucu içinde gerçekleştirilen.

Özel oluşturucu artık platforma özgü kamera Önizleme denetimleri oluşturmak için her uygulama projesine eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `ViewRenderer<T1,T2>` özel kontrolünü icra eden sınıf. Bu durumda olduğu için oluşturucu özel denetimin ilk tür bağımsız değişkeni olmalıdır `CameraPreview`. İkinci tür bağımsız değişkeni, özel denetimin uygulayan yerel denetim olmalıdır.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için özel denetim ve Yaz mantığı işleyen yöntemi. Bu yöntem, karşılık gelen Xamarin.Forms Denetim oluşturulduğunda çağrılır.
1. Ekleme bir `ExportRenderer` bunu Xamarin.Forms özel denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Xamarin.Forms öğelerin çoğu, her platform projesinde özel bir oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse denetimin taban sınıfı için varsayılan oluşturucu kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [görünümü](xref:Xamarin.Forms.View) öğesi.

Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](view-images/solution-structure.png "CameraPreview özel Oluşturucu Proje Sorumlulukları")

`CameraPreview` Öğesinden türetilen tüm hangi platforma özel Oluşturucu sınıflar tarafından işlenen özel denetim `ViewRenderer` her platform için sınıf. Bu her sonuçları `CameraPreview` aşağıdaki ekran görüntülerinde gösterildiği gibi platforma özgü denetimleriyle işlenen özel denetimi:

![](view-images/screenshots.png "Her platformda CameraPreview")

`ViewRenderer` Sınıfı kullanıma sunan `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms özel denetim oluşturulurken çağrılan yöntemi. Bu yöntem bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikler Xamarin.Forms öğesini temsil, oluşturucu *olduğu* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olduğu* bağlı olarak, sırasıyla. Örnek uygulamada, `OldElement` özelliği `null` ve `NewElement` özelliği, bir başvuru içerecek `CameraPreview` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, özelleştirme ve yerel denetim oluşturmada gerçekleştirmek için yerdir. `SetNativeControl` Yöntemi yerel denetim örneği oluşturmak için kullanılması gerekir ve bu yöntem ayrıca Denetim başvurusu atar `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetime başvuru aracılığıyla alınabilir `Element` özelliği.

Bazı durumlarda, `OnElementChanged` yöntemi birden çok kez çağrılabilir. Bu nedenle, bellek sızıntılarını önlemek için dikkatli yeni bir yerel denetim örneği oluşturulurken olunması gerekir. Özel oluşturucu içinde yeni bir yerel denetim örneği oluşturulurken kullanılacak bir yaklaşım aşağıdaki kod örneğinde gösterilmiştir:

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

Yeni bir yerel denetim yalnızca bir kez örneği, `Control` özelliği `null`. Denetim yalnızca yapılandırılmalıdır ve özel Oluşturucu yeni bir Xamarin.Forms öğe eklendiğinde olay işleyicileri abone. Benzer şekilde, abone tüm olay işleyicileri yalnızca Oluşturucu bağlı olduğu öğe değiştiğinde aboneliği olmalıdır. Bu yaklaşımı benimsemeyi bellek sızıntılardan etkilese değil yüksek performanslı özel Oluşturucu oluşturmaya yardımcı olur.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` özniteliği işleyici Xamarin.Forms ile kaydeder. Öznitelik, iki parametre – işlenen Xamarin.Forms özel denetimin tür adı ve özel Oluşturucu türü adını alır. `assembly` Özniteliğiyle önek öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfın uygulaması açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, iOS platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Koşuluyla `Control` özelliği `null`, `SetNativeControl` yöntemi yeni bir örneğini oluşturmak için çağrılır `UICameraPreview` denetimi ve ona bir başvuru atamak `Control` özelliği. `UICameraPreview` Denetimidir kullanan bir platforma özgü özel denetim `AVCapture` kameradan Önizleme akışı sağlamak için API'ler. Kullanıma sunduğu bir `Tapped` tarafından işlenen olay `OnCameraPreviewTapped` durdurmak ve onu dokunulduğunda video Önizlemesi'ni başlatmak için yöntemi. `Tapped` Olayı abone için özel Oluşturucu yeni bir Xamarin.Forms öğesine eklenen ve yalnızca zaman öğesi işleyici değişiklikleri bağlı öğesinden kaldırıldı.

### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, Android platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Koşuluyla `Control` özelliği `null`, `SetNativeControl` yöntemi yeni bir örneğini oluşturmak için çağrılır `CameraPreview` denetlemek ve ona bir başvuru atama `Control` özelliği. `CameraPreview` Denetimidir kullanan bir platforma özgü özel denetim `Camera` kameradan Önizleme akışı sağlamak için API. `CameraPreview` Denetim ardından yapılandırılmış, koşuluyla özel Oluşturucu, yeni bir Xamarin.Forms öğesine bağlı. Bu yapılandırma yeni bir yerel kapsamında `Camera` belirli donanım kameranıza erişmesine izin nesnesi ve işlemek için bir olay işleyicisi kaydedilirken `Click` olay. Sırayla Bu işleyici durdurun ve onu dokunulduğunda video önizlemesi başlatın. `Click` Olay gönderilmedi gelen değişiklikleri Xamarin.Forms öğesi işleyici bağlıysa.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP üzerinde özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

Koşuluyla `Control` özelliği `null`, yeni bir `CaptureElement` örneği ve `SetupCamera` yöntemi çağrıldığında, kullanan `MediaCapture` kameradan Önizleme akışı sağlamak için API. `SetNativeControl` Yöntemi başvuru atamak için daha sonra çağrılır `CaptureElement` için örnek `Control` özelliği. `CaptureElement` Denetiminin bir `Tapped` tarafından işlenen olay `OnCameraPreviewTapped` durdurmak ve onu dokunulduğunda video Önizlemesi'ni başlatmak için yöntemi. `Tapped` Olayı abone için özel Oluşturucu yeni bir Xamarin.Forms öğesine eklenen ve yalnızca zaman öğesi işleyici değişiklikleri bağlı öğesinden kaldırıldı.

> [!NOTE]
> Durdur ve bir UWP uygulaması kameraya erişim sağlayan nesneleri silmek önemlidir. Bunun yapılmaması, cihazın kamerasını erişme denemesi ile diğer uygulamaları etkileyebilir. Daha fazla bilgi için [kamera önizlemesini görüntüleyin](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Özet

Bu makalede, cihazın kamerasını Önizleme video akıştan görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu Oluşturma gösterilmiştir. Xamarin.Forms özel kullanıcı arabirimi denetimleri türetilmelidir [ `View` ](xref:Xamarin.Forms.View) düzenleri ve ekrandaki denetimleri yerleştirmek için kullanılan sınıf.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
