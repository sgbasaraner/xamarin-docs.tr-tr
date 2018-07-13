---
title: Bir Contentpage'i özelleştirme
description: Bir Contentpage'i tek bir görünümde görüntüleyen ve ekranın çoğunu kaplayan görsel bir öğedir. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak ContentPage sayfası için özel Oluşturucu oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 2369b249681b926476cf3938c51c99745eba9098
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995748"
---
# <a name="customizing-a-contentpage"></a>Bir Contentpage'i özelleştirme

_Bir Contentpage'i tek bir görünümde görüntüleyen ve ekranın çoğunu kaplayan görsel bir öğedir. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak ContentPage sayfası için özel Oluşturucu oluşturma işlemini gösterir._

Yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici her Xamarin.Forms denetleyebilir. Olduğunda bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) bir Xamarin.Forms uygulaması iOS tarafından işlenen `PageRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `UIViewController` denetimi. Android platformunda `PageRenderer` sınıfı örnekleyen bir `ViewGroup` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `PageRenderer` sınıfı örnekleyen bir `FrameworkElement` denetimi. Oluşturucu ve Xamarin.Forms denetimleri eşleyen yerel denetim sınıfları hakkında daha fazla bilgi için bkz. [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagramda arasındaki ilişkiyi gösterir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) ve onu uygulayan karşılık gelen yerel denetimler:

![](contentpage-images/contentpage-classes.png "ContentPage sınıfı ve yerel denetimleri uygulama arasındaki ilişki")

İşleme sürecini avantajlarından platforma özgü özelleştirmeler için özel Oluşturucu oluşturarak uygulamak için gerçekleştirilebilecek bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Xamarin.Forms_Page) Xamarin.Forms sayfası.
1. [Tüketen](#Consuming_the_Xamarin.Forms_Page) Xamarin.Forms sayfasından.
1. [Oluşturma](#Creating_the_Page_Renderer_on_each_Platform) sayfada her platform için özel Oluşturucu.

Her öğe artık sırayla uygulanacağı açıklanmıştır bir `CameraPage` akış canlı bir kamera ve fotoğraf yakalama olanağı sağlar.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Xamarin.Forms sayfası oluşturma

Değiştirilmemiş bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) aşağıdaki XAML kod örneğinde gösterildiği gibi paylaşılan Xamarin.Forms projesine eklenebilir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Benzer şekilde, arka plan kod dosyası için [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) ayrıca aşağıdaki kod örneğinde gösterildiği gibi değiştirilmemiş, kalması gerekir:

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

Örneği `CameraPage` Canlı Kamera her platformda akışını görüntülemek için kullanılır. Denetimin özelleştirme gerçekleştirilmesini özel işleyicide ek uygulaması gerekli şekilde `CameraPage` sınıfı.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Xamarin.Forms sayfanın kullanma

Boş `CameraPage` Xamarin.Forms uygulama tarafından görüntülenmesi gerekir. Bu, bir düğme ortaya çıkar `MainPage` örneği dokunulduğunda, sırayla yürütür `OnTakePhotoButtonClicked` yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Bu kod yalnızca gider `CameraPage`, hangi özel oluşturucular üzerinde sayfa görünümünü her platformda özelleştireceksiniz.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Her platformda sayfa Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `PageRenderer` sınıfı.
1. Geçersiz kılma `OnElementChanged` sayfayı özelleştirmek için yerel sayfası ve yazma mantığı işleyen yöntemi. `OnElementChanged` Yöntemi, karşılık gelen Xamarin.Forms Denetim oluşturulduğunda çağrılır.
1. Ekleme bir `ExportRenderer` sayfa işleyici sınıfına bunu Xamarin.Forms sayfayı oluşturmak için kullanılacak belirtmek için özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Her platform projesinde bir sayfa işleyici sağlamak isteğe bağlıdır. Bir sayfa işleyici kayıtlı değilse, sayfa için varsayılan oluşturucu kullanılır.

Aşağıdaki diyagram, her proje örnek uygulamada, aralarında bir ilişki birlikte sorumluluklarını gösterir:

![](contentpage-images/solution-structure.png "CameraPage özel Oluşturucu Proje Sorumlulukları")

`CameraPage` Örneği platforma özgü tarafından işlenen `CameraPageRenderer` tüm türetilen sınıfları `PageRenderer` , platform için sınıf. Bu her sonuçları `CameraPage` örnek kamera canlı akış ile işlenen aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](contentpage-images/screenshots.png "Her platformda CameraPage")

`PageRenderer` Sınıfı kullanıma sunan `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms sayfası oluşturulduğunda çağrılan yöntemi. Bu yöntem bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikler Xamarin.Forms öğesini temsil, oluşturucu *olduğu* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olduğu* bağlı olarak, sırasıyla. Örnek uygulamada `OldElement` özelliği `null` ve `NewElement` özelliği, bir başvuru içerecek `CameraPage` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde `CameraPageRenderer` sınıfı, nelerin yerel sayfayı özelleştirme işlemi gerçekleştirin. İşlenen Xamarin.Forms sayfa örneğe bir başvuru üzerinden alınan `Element` özelliği.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` özniteliği işleyici Xamarin.Forms ile kaydeder. Öznitelik, iki parametre – oluşturulmakta olan sayfaya Xamarin.Forms tür adını ve özel Oluşturucu türü adını alır. `assembly` Özniteliğiyle önek öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki bölümlerde yürütmesinin `CameraPageRenderer` her platform için özel Oluşturucu.

### <a name="creating-the-page-renderer-on-ios"></a>İos'ta sayfa Oluşturucu Oluşturma

Aşağıdaki kod örneği, iOS platformu için sayfa oluşturucuyu gösterir:

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
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Temel sınıfın çağrısı `OnElementChanged` yöntemi örnekleyen bir iOS `UIViewController` denetimi. Canlı Kamera akışını yalnızca Oluşturucu zaten var olan bir Xamarin.Forms öğesine ekli değildir ve sayfa örneği var olması koşuluyla, özel Oluşturucu tarafından işlenen koşuluyla işlenir.

Sayfa, ardından bir dizi yöntem tarafından özelleştirildikten `AVCapture` Canlı akıştan kamera ve fotoğraf yakalama olanağı sağlamak için API'ler.

### <a name="creating-the-page-renderer-on-android"></a>Android'de sayfa Oluşturucu Oluşturma

Aşağıdaki kod örneği, Android platformu için sayfa oluşturucuyu gösterir:

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
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Temel sınıfın çağrısı `OnElementChanged` yöntemi örnekleyen bir Android `ViewGroup` form veya denetim görünümleri grubudur. Canlı Kamera akışını yalnızca Oluşturucu zaten var olan bir Xamarin.Forms öğesine ekli değildir ve sayfa örneği var olması koşuluyla, özel Oluşturucu tarafından işlenen koşuluyla işlenir.

Bir dizi kullanan yöntemleri çağırarak sayfa özelleştirildikten `Camera` Canlı akıştan kamera ve fotoğraf önce yakalama olanağı sağlamak için API `AddView` metodunu çağırmak Canlı Kamera eklemek için kullanıcı Arabirimi için akış `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>UWP üzerinde sayfa Oluşturucu Oluşturma

Aşağıdaki kod örneği, UWP için sayfa oluşturucuyu gösterir:

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

Temel sınıfın çağrısı `OnElementChanged` yöntemi oluşturur bir `FrameworkElement` denetimi, sayfanın işlenir. Canlı Kamera akışını yalnızca Oluşturucu zaten var olan bir Xamarin.Forms öğesine ekli değildir ve sayfa örneği var olması koşuluyla, özel Oluşturucu tarafından işlenen koşuluyla işlenir. Bir dizi kullanan yöntemleri çağırarak sayfa özelleştirildikten `MediaCapture` Canlı akıştan kamera ve fotoğraf özelleştirilmiş sayfası eklenmesi yakalama olanağı sağlamak için API `Children` görüntülemek için bir koleksiyon.

Öğesinden türetilen özel Oluşturucu uygularken `PageRenderer` UWP üzerinde `ArrangeOverride` yönteminin de uygulanmasını sayfası denetimleri düzenlemek için temel oluşturucu ne kendileriyle yapılacağını bilmediği. Aksi takdirde, boş bir sayfa sonuçlanır. Bu nedenle, bu örnekte `ArrangeOverride` yöntem çağrılarını `Arrange` metodunda `Page` örneği.

> [!NOTE]
> Durdur ve bir UWP uygulaması kameraya erişim sağlayan nesneleri silmek önemlidir. Bunun yapılmaması, cihazın kamerasını erişme denemesi ile diğer uygulamaları etkileyebilir. Daha fazla bilgi için [kamera önizlemesini görüntüleyin](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Özet

Bu makalede için özel Oluşturucu Oluşturma gösterilmiştir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) sayfası, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak. A `ContentPage` tek bir görünümde görüntüleyen ve ekranın çoğunu kaplayan görsel bir öğedir.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererContentPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
