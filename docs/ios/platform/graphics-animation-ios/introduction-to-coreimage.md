---
title: CoreImage
description: CoreImage, iOS ve video geliştirme işlevselliği dinamik görüntü işleme sağlamak üzere 5 ile sunulan yeni bir çerçevedir. Bu makalede Xamarin.iOS örnekleri ile bu özellikleri sunar.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 0bb2c3b8b563da53e432ad16e6518ada67a4655e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="coreimage"></a>CoreImage

_CoreImage, iOS ve video geliştirme işlevselliği dinamik görüntü işleme sağlamak üzere 5 ile sunulan yeni bir çerçevedir. Bu makalede Xamarin.iOS örnekleri ile bu özellikleri sunar._

CoreImage bir dizi yerleşik filtreler ve görüntüleri ve video yüz algılama dahil olmak üzere, uygulama etkileri sağlayan bir iOS 5'de sunulan yeni bir çerçevedir.

Bu belge basit örnekleri içerir:

-  Yüz algılama.
-  Bir görüntüye filtre uygulama
-  Kullanılabilir filtreleri listeleniyor.


Bu örnekler Xamarin.iOS uygulamalarınıza CoreImage özellikleri başlamanıza yardımcı olması.

## <a name="requirements"></a>Gereksinimler

Xcode en son sürümünü kullanmanız gerekir.

## <a name="face-detection"></a>Yüz algılama

Yalnızca ne yazacaktır CoreImage yüz algılama özelliğini mu – bir fotoğraf yüzeyleri tanımlamaya çalışır ve tanıdığı yüzeyleri koordinatlarını döndürür. Bu bilgiler görüntüdeki kişiler sayısı, Göstergeler görüntüde (ör. çizmek için kullanılabilir 'kişiler bir fotoğraf etiketleme için'), ya da, düşünme başka bir şey.

Bu koddan CoreImage\SampleCode.cs oluşturmak ve katıştırılmış görüntüde yüz algılama kullanmak nasıl gösterir:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Özellik dizisi ile doldurulur `CIFaceFeature` (tüm yüzeyleri algılanırsa) nesneleri. Var olan bir `CIFaceFeature` her yüz için. `CIFaceFeature` aşağıdaki özelliklere sahiptir:

-  HasMouthPosition – bir ağzınıza için bu yüz olup algılandı.
-  HasLeftEyePosition – sol göz için bu yüz olup algılandı.
-  HasRightEyePosition – sağ göz için bu yüz olup algılandı. 
-  MouthPosition – Bu yüz ağzınıza koordinatları.
-  LeftEyePosition – Bu yüz için sol göz koordinatları.
-  RightEyePosition – Bu yüz için doğru göz koordinatları.


Bu özelliklerin koordinatları kendi kaynak sol alt – sol üst kaynağı kullanan Uıkit aksine sahip. Koordinatları kullanırken `CIFaceFeature` 'bunları ters çevirmek ' emin olun. Bu CoreImage\CoreImageViewController.cs çok temel özel görüntü görünümde görüntüde 'Yüz göstergesi' üçgenler çizin gösterir (Not `FlipForBottomOrigin` yöntemi):

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Görüntüyü yeniden önce SampleCode.cs dosyasında görüntü ve Özellikler atanır:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Ekran örnek çıkış şunları gösterir: algılanan yüz özelliklerine konumlarını bir UITextView görüntülenir ve CoreGraphics kullanarak kaynak yansımayı çizilmiştir.

Yüz tanıma çalışma şeklini nedeniyle bazen şeyleri (ör. Bu Oyuncak monkeys!) İnsan yüzeyleri yanı sıra algılar.

## <a name="filters"></a>FilTReleri

Yeni filtreler uygulanabilir framework genişletilebilir, böylelikle ve 50'den farklı yerleşik filtreler vardır.

## <a name="using-filters"></a>Filtreleri kullanma

Görüntüye filtre uygulama dört farklı adımlar vardır: yükleme görüntüsünü, filtre oluşturma, filtre uygulamak ve kaydetme (veya görüntüleme) sonucu.

Görüntüye ilk olarak, yükleme bir `CIImage` nesnesi.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

İkinci olarak, filtre sınıfı oluşturun ve özelliklerini ayarlayın.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Üçüncü erişim `OutputImage` özelliği ve çağrı `CreateCGImage` sonucunu işlemek için yöntem.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Son olarak, görüntünün sonuçları görmek için bir görünüm atayın. Gerçek dünya uygulamada elde edilen görüntü dosya sistemi, fotoğraf albümü, bir Tweet veya e-posta için kaydedilmiş olabilir.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Bu ekran görüntüleri sonucu göster `CISepia` ve `CIHueAdjust` CoreImage.zip gösterilen filtreleri örnek kodu.

Bkz: [sözleşme ayarlamak ve bir görüntü tarif parlaklığını](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) ilişkin bir örnek `CIColorControls` Filtresi.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Filtreler ve bunların özelliklerini listeleme

Bu koddan CoreImage\SampleCode.cs yerleşik filtreler tam listesi ve bunların parametrelerini çıkarır.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

[CIFilter sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) 50 yerleşik filtreler ve bunların özelliklerini açıklar. Yukarıdaki kodu kullanarak parametrelerinin varsayılan değerleri ve (bir filtre uygulanmadan önce girişleri doğrulamak için kullanılabilir) maksimum ve minimum izin verilen değerleri dahil olmak üzere filtre sınıfları sorgulayabilirsiniz.

Liste kategorileri çıktı simulator'da şöyle – tüm filtreleri ve bunların parametrelerini görmek için listenin kaydırın.

 [![](introduction-to-coreimage-images/coreimage05.png "Liste kategorileri çıktı simulator'da şöyle")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Derleme tarayıcı veya Mac için Visual Studio veya Visual Studio'da otomatik tamamlama kullanılarak Xamarin.iOS.CoreImage API de gözatabilirsiniz Xamarin.iOS, sınıfında olarak listelenen her filtre açık. 

## <a name="summary"></a>Özet

Bu makalede, bazı yüz algılama ve görüntüye filtre uygulayarak gibi yeni iOS 5 CoreImage framework Özellikleri'nin nasıl kullanıldığını göstermiştir. Farklı bir resim filtreleri düzinelerce Framework'te kullanmanız için kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Çekirdek görüntü (örnek)](https://developer.xamarin.com/samples/CoreImage/)
- [Sözleşme ve bir görüntü tarif parlaklığını ayarla](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [CoreImage filtrelerini kullanma](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
