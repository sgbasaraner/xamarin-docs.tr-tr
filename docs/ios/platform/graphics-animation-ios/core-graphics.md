---
title: Xamarin.iOS çekirdek grafikleri
description: Bu makalede, temel grafikler iOS çerçevelerini açıklanmaktadır. Bu, geometri, resimler ve PDF çizmek için temel grafikler kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 82c54074db722824c56ae3ae86620c804b8d109e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241955"
---
# <a name="core-graphics-in-xamarinios"></a>Xamarin.iOS çekirdek grafikleri

_Bu makalede, temel grafikler iOS çerçevelerini açıklanmaktadır. Bu, geometri, resimler ve PDF çizmek için temel grafikler kullanmayı gösterir._

iOS içerir [ *temel grafikler* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework alt düzey çizim desteği sağlamak için. Bu çerçeveler ne Uıkit içinde zengin grafik özellikleri etkinleştirecek olan. 

Çekirdek grafik çizim cihaz bağımsız grafik izin veren bir alt düzey 2B grafikler çerçevedir. Uıkit içinde çizim tüm 2B temel grafikler dahili olarak kullanır.

Temel grafikler gibi senaryolar sayısında çizim destekler:

-  [Çizim ekran bir `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Bellekte veya ekran görüntüleri çizim](#Drawing_Images_and_Text).
-  Oluşturma ve PDF'ye çizim.
-  Okuma ve var olan bir PDF çizim.


## <a name="geometric-space"></a>Geometrik alanı

Senaryo bağımsız olarak, temel grafikler ile yapılan tüm çizim piksel yerine soyut noktaları çalıştığı anlamına gelir geometrik alanında gerçekleştirilir. Geometri ve durum renk, çizgi stilleri, vb. gibi çizim açısından çizilen istediğiniz açıklar ve her şeyi piksellere çevirme temel grafikler işler. Böyle bir durum painter'ın tuval gibi düşünebilirsiniz bir grafik bağlam eklenir.

Bu yaklaşımın bazı avantajları vardır:

-  Çizim kodunu dinamik olur ve çalışma zamanında grafik sonradan değiştirebilirsiniz.
-  Uygulama paketi statik görüntüleri gereksinimini azaltır, uygulamanın boyutunu küçültebilirsiniz.
-  Grafik cihazlarda çözümleme değişikliklere daha dayanıklı hale gelir.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Çizimde Uıview öğesinin alt sınıfı

Her `UIView` sahip bir `Draw` yeniden çizilmesi gerektiğinde sistem tarafından çağrılan yöntem. Bir görünüme çizim kodu eklemek için alt `UIView` ve geçersiz kılma `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Çizim hiçbir zaman doğrudan çağrılmalıdır. Bu, sistem tarafından çalışma çevrim işleme sırasında çağrılır. İlk kez bir görünüm, görünüm hiyerarşiye eklendikten sonra çalışma döngü kendi `Draw` yöntemi çağrılır. Yapılan sonraki çağrılar `Draw` görünümü ya da çağırarak çizilecek gerek kalmadan olarak işaretlendiğinde gerçekleşir `SetNeedsDisplay` veya `SetNeedsDisplayInRect` görünümü.

### <a name="pattern-for-graphics-code"></a>Grafik kod deseni

Kodda `Draw` uygulaması açıklamak ne çizilen istemektedir. Çizim kodu, bazı çizim durumunu ayarlar ve çizilmiş istemek için bir yöntemi çağıran bir yapıdadır. Bu düzen şu şekilde genelleştirilmiş olmalıdır:

1. Grafik bağlam alın.

2. Çizim özniteliklerini ayarlayın.

3. Bazı geometri ilkel çizmekten oluşturun.

4. Bir çizim veya vuruşuna yöntemini çağırın.

### <a name="basic-drawing-example"></a>Temel çizim örneği

Örneğin, aşağıdaki kod parçacığı göz önünde bulundurun:

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {
            
    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100), 
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);       
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

Bu kod şimdi parçalara ayırın:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
Şu satır ile ilk çizim için kullanılacak geçerli grafik bağlamını alır. Çizim, tüm durumu hakkında vuruşuna ve dolgu rengi yanı sıra, çizmek için geometri gibi çizim içeren gerçekleştiğini grafik bağlamında tuval düşünebilirsiniz.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Grafik bağlam aldıktan sonra kod bazı nitelikler çizim, yukarıda gösterilen sırasında kullanılacak ayarlar. Bu durumda, çizgi genişliği, vuruş ve Dolgu renkleri ayarlanır. Grafik bağlamı'nın durumda tutulduğundan herhangi bir sonraki çizim sonra bu öznitelikleri kullanır.

Geometri kod oluşturmak için kullandığı bir `CGPath`, çizgiler ve eğrilerle açıklanan için bir grafik yolu sağlar. Bu durumda, yolun bir üçgen açmak için bir dizi bağlı çizgileri ekler. Temel grafikler kullanan çizim görünümü için bir koordinat sistemi gösterildiği kaynağı sol üst tarafında, sağa ve aşağı pozitif y yönünde pozitif x-direct ile olduğu:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Yolu oluşturulduktan sonra böylece çağırmak için grafik bağlam eklendikten `AddPath` ve `DrawPath` sırasıyla çizebilirsiniz.

Ekranda aşağıda gösterilmiştir:

 ![](core-graphics-images/00-bluetriangle.png "Örnek çıktı üçgen")

## <a name="creating-gradient-fills"></a>Gradyan Dolgu oluşturma

Çizim daha zengin formlar de mevcuttur. Örneğin, temel grafikler Gradyan Dolgu oluşturma ve kırpma yolu uygulama sağlar. Önceki örnekte bir Gradyan Dolgu yolun içinde çizmek için ilk yolun kırpma yolu olarak ayarlanması gerekir:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Doğrusal gradyan çizer kırpma yolu, aşağıdaki kod gibi bir yol'in geometrisini içindeki tüm sonraki çizim kısıtlar gibi geçerli bir yol ayarlar:

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient, 
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top), 
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom), 
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

Bu değişiklikler, aşağıda gösterildiği gibi bir Gradyan Dolgu üretir:

 ![](core-graphics-images/01-gradient-fill.png "Gradyan dolgulu bir örnek")

## <a name="modifying-line-patterns"></a>Çizgi desenleri değiştirme

Satırları çizim öznitelikleri ile temel grafikler de değiştirilebilir. Bu satırı deseninin kendisi, yanı sıra, çizgi genişliği ve vuruş rengi değiştirme aşağıdaki kodda görüldüğü gibi içerir:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Bu kodu herhangi bir çizim operations sonuç önce kesikli strokes 10 birim uzun 4 birimleri arasında tire, boşluk ile aşağıda gösterildiği gibi ekleme:

 ![](core-graphics-images/02-dashed-stroke.png "Bu kodu herhangi bir çizim operations sonuç önce içinde kesikli strokes ekleme")
 
Birleşik API Xamarin.ios'ta kullanırken, dizi türü olması gerektiğini aklınızda bulundurun bir `nfloat`ve ayrıca Math.PI için açıkça yayınlanması gerekiyor.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Çizimi görüntü ve metin

Bir görünümün grafik bağlam içinde yollar çizme ek olarak temel grafikler de çizim görüntü ve metin destekler. Resim çizmek için basitçe oluşturma bir `CGImage` ve geçirin bir `DrawImage` çağırın:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Ancak, bu baş aşağı, aşağıda gösterildiği gibi çizilmiş bir görüntü oluşturur:

 ![](core-graphics-images/03-upside-down-monkey.png "Tersyüz çizilmiş bir görüntü")

Bunun nedeni görünümü sol üst kaynağına sahipken resim çizim için temel grafikler kaynağını sol alt numarasıdır. Bu nedenle, görüntünün düzgün görüntülenmesi için kaynak değiştirilmesi, hangi değiştirerek gerçekleştirilebilir gereken *geçerli dönüştürme matrisini* *(CTM)*. Burada noktaları, olarak da bilinen Canlı CTM tanımlar *kullanıcı alanı*. Y yönünde CTM ters çevirme ve sınırları yüksekliği negatif y yönünde tarafından kaydırma görüntü çevirebilirsiniz.

Grafik bağlam CTM dönüştürmek için yardımcı yöntemler vardır. Bu durumda, `ScaleCTM` "Çizim döndürür" ve `TranslateCTM` aşağıda gösterildiği gibi üst Sola kaydırır:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
    
    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}   
```

Elde edilen görüntü ardından görüntülenen dik:

 ![](core-graphics-images/04-upright-monkey.png "Örnek görüntülenen görüntü dikey")

> [!IMPORTANT]
> Grafik bağlam yapılan değişiklikler tüm sonraki çizim işlemlere uygulanır. Bu nedenle, CTM dönüştürüldüğünde, herhangi bir ek çizim etkiler. Örneğin, CTM dönüştürme işleminin ardından bu üçgen u çizdiğini, baş aşağı görünür.

### <a name="adding-text-to-the-image"></a>Görüntünün metin ekleme

Olarak bazı grafik durumu ayarlama ve çizmek için bir yöntem çağırma aynı temel düzeni temel grafikler ile metin çizme yolları ve görüntü ile ilgilidir. Metin söz konusu olduğunda, metni görüntülemek için yöntemdir `ShowText`. Aşağıdaki kod, örnek çizim görüntüsüne eklendiğinde, temel grafikler kullanarak metin çizer:

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);
    
    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

Gördüğünüz gibi metin çizim için grafik durumu ayarlama geometri çizim benzerdir. Metin ancak çizmek için çizim modu ve yazı tipi metin uygulanır de. Gölgeleri uygulama aynı çizim yolu için çalışır, ancak bu durumda, bir gölge de uygulanır.

Sonuç metni, görüntüyü aşağıda gösterildiği gibi görüntülenir:

 ![](core-graphics-images/05-text-on-image.png "Elde edilen metnini görüntüyle görüntülenir")

## <a name="memory-backed-images"></a>Bellek destekli görüntüleri

Bir görünümün grafik bağlam için çizim ek olarak, görüntüleri olarak da bilinen off-screen çizim, bellek çizim çekirdek grafikleri destekler desteklenir. Bunun yapılması şunları gerektirir:

-  Grafik bağlam oluşturma bir bellek içi tarafından desteklenen bit eşlem
-  Çizim durumu ayarlama ve çizim komutları verme
-  Bağlamdan görüntüsü alma
-  İçerik kaldırma


Farklı `Draw` yöntemi bağlamı bir görünüm tarafından sağlanan burada bu durumda, oluşturduğunuz bağlamı iki yoldan biriyle:

1. Çağırarak `UIGraphics.BeginImageContext` (veya `BeginImageContextWithOptions`)

2. Yeni bir oluşturarak `CGBitmapContextInstance`

 `CGBitmapContextInstance` özel görüntü işleme algoritması burada kullandığınız durumlar için olduğu gibi doğrudan görüntü BITS ile çalışıyorsanız yararlı olur. Diğer durumlarda, kullanmanız gereken `BeginImageContext` veya `BeginImageContextWithOptions`.

Bir resim bağlamını aldıktan sonra kod içinde olduğu gibi ekleme bir `UIView` öğesinin alt sınıfı. Örneğin, daha önce bir üçgen çizmek için kullanılan kod örneği yerine de bellekte görüntüye çizmek için kullanılabilir bir `UIView`, aşağıda gösterildiği gibi:

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100), 
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }
    
    return triangleImage;
}
```

Herhangi bir görüntü yakalamak için genel kullanımı, bellek destekli bir bit eşlemi çizme olan `UIView`. Örneğin, aşağıdaki kod bir bit eşlem bağlamı bir görünüm katmana işler ve oluşturur bir `UIImage` almaktır:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>PDF çizme

Görüntüleri ek olarak temel grafikler, PDF çizim destekler. Görüntüleri gibi bellekte bir PDF işleme yapabilir, işleme için bir PDF okuma bir `UIView`.

### <a name="pdf-in-a-uiview"></a>Bir Uıview PDF

Temel grafikler de destekler PDF Dosyadan okuma ve görünümünü kullanarak işleme `CGPDFDocument` sınıfı. `CGPDFDocument` Sınıfı kodda bir PDF temsil eder ve okuma ve sayfaları çizmek için kullanılabilir.

Örneğin, aşağıdaki kod bir `UIView` öğesinin alt sınıfı bir dosyaya bir PDF okur bir `CGPDFDocument`:

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }
  
     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

`Draw` Yöntemi kullanabilirsiniz `CGPDFDocument` bir sayfaya okunacak `CGPDFPage` ve bunu çağırarak işler `DrawPDFPage`, aşağıda gösterildiği gibi:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
        
    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);
        
        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {
            
        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);
        
        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);
        
        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>Bellek tarafından desteklenen PDF

Bir bellek içi PDF için çağırarak bir PDF bağlam oluşturmanız gerekir. `BeginPDFContext`. Çizim PDF'ye sayfalara ayrıntılı olur. Her sayfanın çağrılarak başlatılır `BeginPDFPage` ve çağırarak tamamlandı `EndPDFContent`, grafik ile kod arasında. Ayrıca, resim çizim ile bellek kullanan çizim PDF CTM yalnızca değiştirerek muhasebesi alt sol kaynağı yedeği olarak görüntülerle ister.

Aşağıdaki kod, bir PDF için metin Çiz gösterilmektedir:

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();
       
using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);      
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }
    
//complete a PDF page
UIGraphics.EndPDFContent ();
```

Elde edilen metnini sonra içerdiği PDF'ye çizilmiş bir `NSData` , kaydedilebilir, karşıya yüklenen, e-postayla, vb.


## <a name="summary"></a>Özet

Bu makalede aracılığıyla sağlanan grafik özellikler inceledik *temel grafikler* framework. Geometri, resimler ve PDF bağlamında çizmek için temel grafikler kullanmayı gördüğümüz bir `UIView,` bellek destekli grafik bağlamlarına gibi iyi.

## <a name="related-links"></a>İlgili bağlantılar

- [Core grafik örneği](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafikler ve animasyon için izlenecek yol](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Temel Animasyon](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Temel animasyon tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
