---
title: "Bir görünümü uygulama"
description: "Xamarin.Forms özel kullanıcı arabirimi denetimlerini düzenleri ve denetimleri ekranında yerleştirmek için kullanılan görünüm sınıfından türetilmelidir. Bu makalede Önizleme video akışına cihazın kameradan görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu Oluşturma gösterilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: d5f9f86447886e2cea46a6317d05506cdbed90bb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="implementing-a-view"></a>Bir görünümü uygulama

_Xamarin.Forms özel kullanıcı arabirimi denetimlerini düzenleri ve denetimleri ekranında yerleştirmek için kullanılan görünüm sınıfından türetilmelidir. Bu makalede Önizleme video akışına cihazın kameradan görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu Oluşturma gösterilir._

Her Xamarin.Forms görünüm yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu yok. Zaman bir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) iOS, bir Xamarin.Forms uygulaması tarafından işlenen `ViewRenderer` sınıf örneği, hangi sırayla yerel başlatır `UIView` denetim. Android platformunda `ViewRenderer` sınıfı başlatır yerel `View` denetim. Windows Phone ve evrensel Windows Platformu (UWP) `ViewRenderer` sınıfı başlatır yerel `FrameworkElement` denetim. Oluşturucu ve Xamarin.Forms denetimleri Eşle yerel denetim sınıfları hakkında daha fazla bilgi için bkz: [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagram arasındaki ilişkiyi gösterir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ve uyguladıktan karşılık gelen yerel denetimlere:

![](view-images/view-classes.png "View sınıfı ve kendi yerel sınıflar arasındaki ilişki")

Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için kullanılan bir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Control) Xamarin.Forms özel bir denetim.
1. [Tüketen](#Consuming_the_Custom_Control) Xamarin.Forms özel denetim.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda denetimi için özel Oluşturucu.

Her öğe artık sırasıyla uygulamak için incelenecektir bir `CameraPreview` Önizleme video akışına cihazın kameradan görüntüler Oluşturucu. Dokunma video akışını durdurun ve başlatın.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Özel denetim oluşturma

Özel bir denetim sınıflara tarafından oluşturulabilir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

`CameraPreview` Özel denetim taşınabilir sınıf kitaplığı (PCL) projesinde oluşturulur ve denetim API'si tanımlar. Özel denetim kullanıma sunan bir `Camera` video akışına ön veya cihazın arka kameradan görüntülenip görüntülenmeyeceğini kontrol etmek için kullanılan özellik. İçin bir değer belirtilmezse `Camera` denetimi oluşturulduğunda özelliği, varsayılan arka kamera belirtmek için.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Özel denetim kullanma

`CameraPreview` Özel denetim başvurulabilir XAML'de PCL projesinde konumu için bir ad alanı bildirme ve özel denetim öğede ad alanı öneki kullanarak. Aşağıdaki örnekte gösterildiği kod nasıl `CameraPreview` özel denetim XAML sayfası tarafından tüketilen:

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

`local` Ad alanı öneki adlı bir şey. Ancak, `clr-namespace` ve `assembly` değerlerin özel denetim ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde öneki özel denetim başvurmak için kullanılır.

Aşağıdaki örnekte gösterildiği kod nasıl `CameraPreview` özel denetim bir C# sayfası tarafından tüketilen:

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

Örneği `CameraPreview` özel denetim, cihazın kameradan Önizleme video akışına görüntülemek için kullanılır. İsteğe bağlı olarak için bir değer belirterek yanı sıra `Camera` özelliği, denetimin özelleştirme özel işleyicide gerçekleştirilmesini.

Özel oluşturucu artık platforma özgü kamera Önizleme denetimleri oluşturmak için her uygulama projesi eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `ViewRenderer<T1,T2>` özel denetim işler sınıfı. İlk tür bağımsız değişkeni işleyicidir için bu durumda özel denetim olmalıdır `CameraPreview`. İkinci tür bağımsız değişkeni, özel denetimin gerçekleştireceksiniz yerel denetimi olmalıdır.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için özel denetim ve yazma mantık işleyen yöntemi. Karşılık gelen Xamarin.Forms Denetim oluşturulduğunda, bu yöntem çağrılır.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms özel denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> **Not**: çoğu Xamarin.Forms öğeleri için her platform projesinde özel Oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse, varsayılan oluşturucu denetimin taban sınıfı için kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [Görünüm](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) öğesi.

Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](view-images/solution-structure.png "CameraPreview özel Oluşturucu Proje Sorumlulukları")

`CameraPreview` Öğesinden türetilen tüm hangi platforma özgü Oluşturucu sınıflar tarafından işlenen özel denetim `ViewRenderer` her platform için sınıf. Bu her sonuçları `CameraPreview` platforma özgü denetimleriyle aşağıdaki ekran görüntülerinde gösterildiği gibi işlenen özel denetim:

![](view-images/screenshots.png "Her platformda CameraPreview")

`ViewRenderer` Sınıf çıkarır `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms özel denetim oluşturulduğunda çağrılan yöntemi. Bu yöntem alır bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikleri Xamarin.Forms öğesini temsil, oluşturucu *olan* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olan* bağlı olarak, sırasıyla. Örnek uygulamasında `OldElement` özelliği `null` ve `NewElement` özelliği bir başvuru içerecek `CameraPreview` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, yerel denetim örnek oluşturma ve özelleştirme gerçekleştirmek için yerdir. `SetNativeControl` Yöntemi yerel denetim örneği oluşturmak için kullanılması gerekir ve bu yöntem aynı zamanda Denetim referansı atayacaktır `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetlemek için bir başvuru aracılığıyla elde edilebilir `Element` özelliği.

Bazı durumlarda, `OnElementChanged` yöntemi birden çok kez çağrılabilir. Bu nedenle, bellek sızıntıları önlemek için dikkatli yeni bir yerel denetim başlatılırken alınması gerekir. Aşağıdaki kod örneğinde yaklaşımı özel oluşturucu içinde yeni bir yerel denetim başlatılırken kullanacak şekilde gösterilir:

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

Yeni bir yerel denetim yalnızca bir kez örneğinin oluşturulması, ne zaman `Control` özelliği `null`. Denetimi yalnızca yapılandırılmalıdır ve olay işleyicileri özel Oluşturucu yeni bir Xamarin.Forms öğesi eklendiğinde abone. Benzer şekilde, abone tüm olay işleyicileri yalnızca Oluşturucu bağlı öğesi değiştiğinde aboneliği olmalıdır. Bu yaklaşım benimsenmesi bellek sızıntılarını yaşar olmayan bir kullanıcı özel Oluşturucu oluşturmak için yardımcı olur.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` Xamarin.Forms ile oluşturucuyu kaydeder özniteliği. Öznitelik iki parametre – işlenen Xamarin.Forms özel denetim türü adı ve özel Oluşturucu Tür adını alır. `assembly` Özniteliğine öneki belirtir. öznitelik tüm derlemesi için geçerlidir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfı uyarlamasını açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde iOS platformu için özel Oluşturucu gösterir:

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

Koşuluyla `Control` özelliği `null`, `SetNativeControl` yöntemi yeni bir örneğini oluşturmak için çağrılır `UICameraPreview` denetim ve kendisine bir başvuru atamak için `Control` özelliği. `UICameraPreview` Denetimidir kullanan platforma özgü özel bir denetim `AVCapture` kameradan Önizleme akışı sağlamak için API'ler. Bu kullanıma sunan bir `Tapped` tarafından işlenen olay `OnCameraPreviewTapped` durdurmanız ve onu dokunduğunuz video önizlemesi başlattığınızda yöntemi. `Tapped` Olay abone için özel Oluşturucu yeni bir Xamarin.Forms öğesi bağlı ve yalnızca zaman Oluşturucu öğesi değişiklikleri bağlı gelen iptal.

### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde, Android platformu için özel Oluşturucu gösterir:

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

Koşuluyla `Control` özelliği `null`, `SetNativeControl` yöntemi yeni bir örneğini oluşturmak için çağrılır `CameraPreview` denetlemek ve kendisine bir başvuru atamak `Control` özelliği. `CameraPreview` Denetimidir kullanan platforma özgü özel bir denetim `Camera` kameradan Önizleme akışı sağlamak için API. `CameraPreview` Denetim ardından yapılandırılmış, koşuluyla özel Oluşturucu yeni bir Xamarin.Forms öğesine bağlı. Bu yapılandırma yeni bir yerel oluşturursunuz `Camera` belirli donanım kamera erişimini nesne ve işlemek için bir olay işleyicisi kaydetme `Click` olay. Sırayla Bu işleyici durdurun ve onu dokunduğunuz video önizlemesi başlattığınızda. `Click` Olay aboneliği kaldırılan gelen değişiklikler Xamarin.Forms öğesi Oluşturucu bağlıysa.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Windows Phone üzerinde özel Oluşturucu Oluşturma ve UWP

Aşağıdaki kod örneği, Windows Phone ve UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        MediaCapture mediaCapture;
        CaptureElement captureElement;
        CameraOptions cameraOptions;
        Application app;
        bool isPreviewing = false;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                ...
                captureElement = new CaptureElement ();
                captureElement.Stretch = Stretch.UniformToFill;

                InitializeAsync ();
                SetNativeControl (captureElement);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped (object sender, TappedRoutedEventArgs e)
        {
            if (isPreviewing) {
                await StopPreviewAsync ();
            } else {
                await StartPreviewAsync ();
            }
        }
        ...
    }
}
```

Koşuluyla `Control` özelliği `null`, yeni bir `CaptureElement` örneği ve `InitializeAsync` yöntemi çağrıldığında, kullanan `MediaCapture` kameradan Önizleme akışı sağlamak için API. `SetNativeControl` Yöntemi bir başvuru atamak için çağrıldıktan sonra `CaptureElement` için örnek `Control` özelliği. `CaptureElement` Kontrol çıkarır bir `Tapped` tarafından işlenen olay `OnCameraPreviewTapped` durdurmanız ve onu dokunduğunuz video önizlemesi başlattığınızda yöntemi. `Tapped` Olay abone için özel Oluşturucu yeni bir Xamarin.Forms öğesi bağlı ve yalnızca zaman Oluşturucu öğesi değişiklikleri bağlı gelen iptal.

> [!NOTE]
> **Not**: durdurun ve Windows Phone veya UWP uygulamasında kameraya erişebilmesi nesnelerin silmek önemlidir. Bunun Sağlanamaması cihazın kamera erişmeye çalışan diğer uygulamalarla etkileyebilir. Daha fazla bilgi için bkz: ve [hızlı başlangıç: MediaCapture API'sini kullanarak video yakalama](https://msdn.microsoft.com/library/windows/apps/xaml/dn642092.aspx) Windows çalışma zamanı uygulamaları için ve [kamera önizlemesini görüntülemek](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access) UWP uygulamalar için.

## <a name="summary"></a>Özet

Bu makalede, cihazın kamera öğesinden bir önizleme video akışı görüntülemek için kullanılan bir Xamarin.Forms özel denetim için özel Oluşturucu Oluşturma gösterilmiştir. Xamarin.Forms özel kullanıcı arabirimi denetimlerini türetilen [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) düzenleri ve denetimleri ekranında yerleştirmek için kullanılan sınıf.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
