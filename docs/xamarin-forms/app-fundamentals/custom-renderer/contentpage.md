---
title: Bir ContentPage özelleştirme
description: Bir ContentPage, tek bir görünüm görüntüler ve ekranın en kaplar görsel bir öğedir. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme ContentPage sayfası için özel Oluşturucu Oluşturma gösterilir.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0f4de4594e8abb8d0ee03690e5829193c51a3736
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-contentpage"></a>Bir ContentPage özelleştirme

_Bir ContentPage, tek bir görünüm görüntüler ve ekranın en kaplar görsel bir öğedir. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme ContentPage sayfası için özel Oluşturucu Oluşturma gösterilir._

Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir. Zaman bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) iOS içinde bir Xamarin.Forms uygulaması tarafından işlenen `PageRenderer` sınıf örneği, hangi sırayla yerel başlatır `UIViewController` denetim. Android platformunda `PageRenderer` sınıfı başlatır bir `ViewGroup` denetim. Üzerinde Evrensel Windows Platformu (UWP), `PageRenderer` sınıfı başlatır bir `FrameworkElement` denetim. Oluşturucu ve Xamarin.Forms denetimleri Eşle yerel denetim sınıfları hakkında daha fazla bilgi için bkz: [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagram arasındaki ilişkiyi gösterir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ve uyguladıktan karşılık gelen yerel denetimlere:

![](contentpage-images/contentpage-classes.png "ContentPage sınıfı ve yerel denetimler uygulamak arasındaki ilişki")

Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için avantajlarından alınabilir bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Xamarin.Forms_Page) Xamarin.Forms sayfası.
1. [Tüketen](#Consuming_the_Xamarin.Forms_Page) Xamarin.Forms sayfasından.
1. [Oluşturma](#Creating_the_Page_Renderer_on_each_Platform) sayfada her platform için özel Oluşturucu.

Her öğe artık sırasıyla uygulamak için incelenecektir bir `CameraPage` akış Canlı Kamera ve fotoğraf yakalama olanağı sağlar.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Xamarin.Forms sayfası oluşturma

Bir değiştirilmemiş [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) paylaşılan Xamarin.Forms projeye, aşağıdaki XAML kod örneğinde gösterildiği gibi eklenir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Benzer şekilde, arka plan kodu dosyayı [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) da aşağıdaki kod örneğinde gösterildiği gibi çakışmıyorsa, kalması gerekir:

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

Aşağıdaki kod örneği, nasıl sayfa C# ' ta oluşturulabilir gösterir:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Örneği `CameraPage` her platformda akış Canlı Kamera görüntülemek için kullanılır. Denetimin özelleştirme gerçekleştirilmesini özel işleyicide ek uygulaması gereken şekilde `CameraPage` sınıfı.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Xamarin.Forms sayfasına tüketen

Boş `CameraPage` Xamarin.Forms uygulaması tarafından görüntülenmesi gerekir. Bu, bir düğme ortaya çıkar `MainPage` örneği dokunduğunuz, hangi sırayla yürütür `OnTakePhotoButtonClicked` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Bu kod yalnızca gider `CameraPage`, hangi özel oluşturucu üzerinde her platformda sayfa görünümünü özelleştirme.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Her platformda sayfa Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `PageRenderer` sınıfı.
1. Geçersiz kılma `OnElementChanged` sayfayı özelleştirmek için yerel sayfası ve yazma mantığı işleyen yöntemi. `OnElementChanged` Yöntemi karşılık gelen Xamarin.Forms Denetim oluşturulduğunda çağrılır.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms sayfasını işlemek için kullanılacak belirtmek için sayfa Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Bir sayfa işleyici her platform projesinde sağlamak isteğe bağlıdır. Bir sayfa işleyici kayıtlı değil, sayfa için varsayılan oluşturucu kullanılır.

Aşağıdaki diyagram, örnek uygulamasında, bunlar arasındaki ilişkinin yanı sıra her proje sorumlulukları gösterir:

![](contentpage-images/solution-structure.png "CameraPage özel Oluşturucu Proje Sorumlulukları")

`CameraPage` Örneği platforma özgü tarafından işlenen `CameraPageRenderer` tüm öğesinden türetilen sınıflar, `PageRenderer` sınıfı, platform için. Bu her sonuçları `CameraPage` örnek Canlı Kamera akış ile işlenen aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](contentpage-images/screenshots.png "Her platformda CameraPage")

`PageRenderer` Sınıf çıkarır `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms sayfa oluşturulduğunda çağrılan yöntemi. Bu yöntem alır bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikleri Xamarin.Forms öğesini temsil, oluşturucu *olan* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olan* bağlı olarak, sırasıyla. Örnek uygulamasında `OldElement` özelliği `null` ve `NewElement` özelliği bir başvuru içerecek `CameraPage` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde `CameraPageRenderer` sınıfı, sistemi, yerel sayfayı özelleştirme gerçekleştirmek için yerdir. İşlenen Xamarin.Forms sayfa örneğine başvuru aracılığıyla elde edilebilir `Element` özelliği.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` Xamarin.Forms ile oluşturucuyu kaydeder özniteliği. Öznitelik iki parametre – oluşturulmakta olan Xamarin.Forms sayfaya tür adını ve özel Oluşturucu Tür adını alır. `assembly` Özniteliğine öneki belirtir. öznitelik tüm derlemesi için geçerlidir.

Uygulaması aşağıdaki bölümlerde ele `CameraPageRenderer` her platform için özel Oluşturucu.

### <a name="creating-the-page-renderer-on-ios"></a>İos'ta sayfa Oluşturucu Oluşturma

Aşağıdaki kod örneğinde iOS platformu için sayfa Oluşturucu gösterir:

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"          ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Temel sınıfın çağrısı `OnElementChanged` yöntemi başlatır iOS `UIViewController` denetim. Canlı Kamera akış yalnızca işleyici zaten var olan bir Xamarin.Forms öğesine bağlı değil ve bir sayfa örneği var olması koşuluyla, özel Oluşturucu tarafından işlenen koşuluyla işlenir.

Sayfa sonra kullanan yöntemleri bir dizi göre özelleştirilmiş `AVCapture` canlı akış kamera ve fotoğraf yakalama olanağı sağlamak için API'ler.

### <a name="creating-the-page-renderer-on-android"></a>Android sayfa Oluşturucu Oluşturma

Aşağıdaki kod örneğinde, Android platformu için sayfa Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"           ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Temel sınıfın çağrısı `OnElementChanged` yöntemi bir Android başlatır `ViewGroup` görünümleri oluşan bir gruptur denetim. Canlı Kamera akış yalnızca işleyici zaten var olan bir Xamarin.Forms öğesine bağlı değil ve bir sayfa örneği var olması koşuluyla, özel Oluşturucu tarafından işlenen koşuluyla işlenir.

Bir dizi kullanan yöntemleri çağırarak sayfa özelleştirildikten `Camera` canlı akış kamera ve bir fotoğraf önce yakalama olanağı sağlamak için API `AddView` yöntemi çağrılır Canlı Kamera eklemek için kullanıcı Arabirimi için akış `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>UWP üzerinde sayfa Oluşturucu Oluşturma

Aşağıdaki kod örneğinde UWP sayfa Oluşturucusu gösterir:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupBasedOnStateAsync();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

Temel sınıfın çağrısı `OnElementChanged` yöntemi başlatır bir `FrameworkElement` denetimi, sayfa işlenir. Canlı Kamera akış yalnızca işleyici zaten var olan bir Xamarin.Forms öğesine bağlı değil ve bir sayfa örneği var olması koşuluyla, özel Oluşturucu tarafından işlenen koşuluyla işlenir. Bir dizi kullanan yöntemleri çağırarak sayfa özelleştirildikten `MediaCapture` canlı akış kamera ve özelleştirilmiş sayfası eklenmesi fotoğraf yakalama olanağı sağlamak için API `Children` görüntü için koleksiyonu.

Öğesinden türetilen özel Oluşturucu uygularken `PageRenderer` UWP üzerinde `ArrangeOverride` de yöntemin sayfası denetimlerini düzenlemek için temel oluşturucu bunlarla yapmanız gerekenler bilmiyor olduğundan. Aksi durumda, boş bir sayfa sonuçlanır. Bu nedenle, bu örnekte `ArrangeOverride` yöntem çağrılarını `Arrange` yöntemi `Page` örneği.

> [!NOTE]
> Durdur ve bir UWP uygulaması kameraya erişebilmesi nesnelerin silmek önemlidir. Bunun Sağlanamaması cihazın kamera erişmeye çalışan diğer uygulamalarla etkileyebilir. Daha fazla bilgi için bkz: [kamera önizlemesini görüntülemek](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Özet

Bu makalede için özel Oluşturucu Oluşturma gösterilmiştir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) sayfasında, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme. A `ContentPage` tek bir görünüm görüntüler ve ekranın en kapladığı görsel bir öğedir.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererContentPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
