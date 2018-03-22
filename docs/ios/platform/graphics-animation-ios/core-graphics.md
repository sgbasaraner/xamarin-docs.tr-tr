---
title: "Çekirdek grafikleri"
description: "Bu makalede çekirdek grafikleri iOS çerçevelerini anlatılmaktadır. Çekirdek grafikleri geometri, görüntüler ve PDF çizmek için nasıl kullanılacağını gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 128ec8d19dc25dc2231521756ee0f00690e0d134
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="core-graphics"></a>Çekirdek grafikleri

_Bu makalede çekirdek grafikleri iOS çerçevelerini anlatılmaktadır. Çekirdek grafikleri geometri, görüntüler ve PDF çizmek için nasıl kullanılacağını gösterir._

iOS içeren [ *çekirdek grafikleri* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) alt düzey çizim desteği sağlamak üzere framework. Bu çerçeveler ne Uıkit içinde zengin grafik özellikleri etkinleştirmek. 

Çekirdek grafikleri çizim aygıt bağımsız grafik izin veren bir alt düzey 2B grafik çerçevedir. Uıkit içinde çizim tüm 2B çekirdek grafikleri dahili olarak kullanır.

Çekirdek grafikleri senaryolar da dahil olmak üzere çeşitli çizim destekler:

-  [Ekranına çizim bir `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Bellekte veya ekran görüntüleri çizim](#Drawing_Images_and_Text).
-  Oluşturma ve PDF'ye çizim.
-  Okuma ve var olan bir PDF çizim.


## <a name="geometric-space"></a>Geometrik alanı

Senaryo bağımsız olarak, çekirdek grafiklerle yapılan tüm çizim geometrik alanında piksel yerine soyut noktaları çalıştığı anlamına yapılır. Geometri ve renkler, çizgi stillerini, vb. gibi durumu çizim açısından çizilmiş istediğinizi tanımlamak ve her şeyi piksel çevirme çekirdek grafikleri işler. Bu durum bir ressamların tuvale gibi düşünebilirsiniz bir grafik içeriği eklenir.

Bu yaklaşım için bazı avantajları şunlardır:

-  Kod çizim dinamik olur ve çalışma zamanında grafik daha sonra değiştirebilirsiniz.
-  Uygulama paketi statik görüntülerinde gereksinimini azaltma uygulama boyutunu azaltabilirsiniz.
-  Grafik cihaz üzerinden çözünürlüğü değişiklikleri daha esnektir haline gelir.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Bir UIView alt çizme

Her `UIView` sahip bir `Draw` çizilmesi gerektiğinde sistem tarafından çağrılan yöntemi. Çizim kodunu bir görünümüne eklemek için bir alt `UIView` ve geçersiz kılma `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Draw hiçbir zaman doğrudan çağrılmalıdır. Sistem tarafından çalıştırma döngü işleme sırasında adı verilir. İlk kez bir görünüm görünüm hiyerarşisine eklendikten sonra çalışma döngü kendi `Draw` yöntemi çağrılır. Sonraki çağrılar `Draw` görünümü ya da çağırarak çizilmesi gerek olarak işaretlendiğinde oluşur `SetNeedsDisplay` veya `SetNeedsDisplayInRect` görünüm.

### <a name="pattern-for-graphics-code"></a>Grafik kodunda deseni

Kodda `Draw` uygulaması açıklamak ne çizilmiş istemektedir. Çizim kodunu, bazı çizim durumunu ayarlar ve çizilmiş istemek için bir yöntemi çağırır bir desen izler. Bu desen gibi genelleştirilmiş:

1. Bir grafik içeriği alır.

2. Çizim özniteliklerini ayarlayın.

3. Bazı Geometri temelleri çizim oluşturun.

4. Çizim veya vuruş yöntemini çağırın.

### <a name="basic-drawing-example"></a>Temel çizim örneği

Örneğin, aşağıdaki kod parçacığını göz önünde bulundurun:

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

Şimdi bu kodu Bölünme:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
Bu satırla çizim için kullanılacak geçerli grafik içeriği ilk alır. Çizim, vuruş ve dolgu rengi yanı sıra, çizmek için geometri gibi çizim hakkında tüm durumunu içeren gerçekleştiğinden emin tuvale grafik bağlamında düşünebilirsiniz.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Bir grafik içerik edindikten sonra bazı nitelikler çizim, yukarıda gösterilen sırasında kullanılacak ayarlar. Bu durumda çizgi genişliği, vuruş ve dolgu rengi ayarlanır. Grafik bağlamın durumda tutulduğundan tüm sonraki çizim sonra bu öznitelikler kullanın.

Geometri kodu oluşturmak için kullandığı bir `CGPath`, çizgiler ve eğrilerle açıklanan bir grafik yolu sağlar. Bu durumda, yolun bir üçgen yapmak için noktalar dizisi bağlanma satırları ekler. Çekirdek grafikleri kullanır koordinat sistemi çizim görünümün görüntülendiği gibi kaynak sol üst köşedeki, sağa ve aşağı pozitif y yönünde pozitif x doğrudan sahip olduğu:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Yolun oluşturulduktan sonra bu grafik bağlamına çağırma böylece eklenir `AddPath` ve `DrawPath` sırasıyla çizebilirsiniz.

Sonuçta elde edilen görünüm aşağıda gösterilmiştir:

 ![](core-graphics-images/00-bluetriangle.png "Örnek çıktı üçgen")

## <a name="creating-gradient-fills"></a>Gradyan dolgular oluşturma

Çizim daha zengin biçimlerinin de kullanılabilir. Örneğin, çekirdek grafikleri gradyan dolgular oluşturma ve kırpma yolları uygulama sağlar. Önceki örnekten Gradyan Dolgu yolun içindeki çizmek için ilk yol kırpma yolu olarak ayarlanması gerekir:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Doğrusal gradyan çizer kırpma yolunu tüm sonraki çizim, aşağıdaki kod gibi bir yol geometri içinde kısıtlar olarak geçerli yolda ayarı:

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

 ![](core-graphics-images/01-gradient-fill.png "Gradyan Dolgu örnekle")

## <a name="modifying-line-patterns"></a>Çizgi desenleri değiştirme

Satırlar çizme özniteliklerini çekirdek grafiklerle da değiştirilebilir. Bu çizgi deseni kendisini yanı sıra, çizgi genişliğini ve vuruş rengi değiştirerek aşağıdaki kodda görüldüğü gibi içerir:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Aşağıda gösterildiği gibi kesikli vuruşları 10 birimler uzun, kısa çizgi, arasındaki boşluğu 4 birimleri ile herhangi bir çizim işlemleri sonuç önce bu kod ekleme:

 ![](core-graphics-images/02-dashed-stroke.png "Kesikli vuruşları içinde herhangi bir çizim işlemleri sonuç önce bu kod ekleme")
 
Birleşik API içinde Xamarin.iOS kullanırken, dizi türü gerektiğini unutmayın bir `nfloat`ve ayrıca Math.PI için açıkça cast gerekir.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Çizim görüntüler ve metinler

Bir görünümün grafik bağlamda yolları çizim ek olarak, çekirdek grafikleri de çizim görüntüler ve metinler destekler. Resim çizmek için basitçe oluşturma bir `CGImage` ve ona geçirin bir `DrawImage` çağırın:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Ancak, bu ters, aşağıda gösterildiği gibi çizilmiş bir görüntü üretiyor:

 ![](core-graphics-images/03-upside-down-monkey.png "Ters çizilmiş bir görüntü")

Bunun nedeni görünümü sol üst kaynağına sahipken resim çizim için çekirdek grafikleri kaynağını sol alt olup. Bu nedenle, görüntünün doğru olarak görüntülemek için kaynak değiştirilmesi, hangi değiştirerek gerçekleştirilebilir gereken *geçerli dönüştürme matrisini* *(CTM)*. Burada noktaları, olarak da bilinen Canlı CTM tanımlar *kullanıcı alanı*. Y yönünde CTM ters çevirme ve sınırları yüksekliği negatif y yönünde tarafından kaydırma görüntü çevirebilirsiniz.

Grafik bağlamının CTM dönüştürmek için yardımcı yöntemler vardır. Bu durumda, `ScaleCTM` "Çizim çevirir" ve `TranslateCTM` aşağıda gösterildiği gibi üst Sola kaydırır:

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

Elde edilen görüntü sonra görüntülenen dik:

 ![](core-graphics-images/04-upright-monkey.png "Örnek görüntülenen görüntü dikey")

> [!IMPORTANT]
> Grafik içeriği yapılan değişiklikler tüm sonraki çizim işlemleri için uygulanır. Bu nedenle, CTM dönüştürüldüğünde herhangi bir ek çizim etkiler. Örneğin, CTM dönüştürme işleminin ardından üçgen u çizdiğini, ters görünür.

### <a name="adding-text-to-the-image"></a>Metin için görüntü ekleme

Olarak bazı grafik durumunun ayarlanması ve çizmek için bir yöntem çağırma aynı temel deseni çekirdek grafikleri ile metin çizme yolları ve görüntüleri içerir. Söz konusu olduğunda, metin görüntülemek için yöntemini metindir `ShowText`. Aşağıdaki kod örnek çizim görüntüsüne eklendiğinde, çekirdek grafikleri kullanarak bazı metin çizer:

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

Gördüğünüz gibi metin çizim için grafik durumu ayarı geometri çizim benzer. Metin ancak çizim için çizim modu ve yazı tipi metin uygulanır de. Gölgeleri uygulama çizim yolu için aynı şekilde çalışır ancak bu durumda, bir gölge Ayrıca, uygulanır.

Sonuçta elde edilen metin görüntüyle aşağıda gösterildiği gibi görüntülenir:

 ![](core-graphics-images/05-text-on-image.png "Sonuçta elde edilen metin görüntüyle görüntülenir")

## <a name="memory-backed-images"></a>Bellek destekli görüntüleri

Bir görünümün grafik içeriği çizim ek olarak, görüntüleri olarak da bilinen off-screen çizim, bellek çizim çekirdek grafikleri destekler yedeklendi. Bunun yapılması gerekir:

-  Bir grafik içeriği oluşturma bir bellek tarafından yedeklendiği bit eşlem
-  Çizim durumu ayarlama ve çizim komutları verme
-  Bağlamdan görüntüsü alma
-  Bağlam kaldırma


Farklı `Draw` yöntemi, bağlam görünüm tarafından sağlanan burada bu durumda, oluşturduğunuz bağlam iki yoldan biriyle:

1. Çağırarak `UIGraphics.BeginImageContext` (veya `BeginImageContextWithOptions`)

2. Yeni bir oluşturarak `CGBitmapContextInstance`

 `CGBitmapContextInstance` gibi bir özel görüntü işleme algoritması kullandığınız durumlar için doğrudan görüntü BITS ile çalışıyorsanız durumlarda faydalıdır. Diğer durumlarda, kullanmanız gereken `BeginImageContext` veya `BeginImageContextWithOptions`.

Bir yansıma bağlamına sahip olduğunda, kod çizim içinde olduğu gibi ekleme bir `UIView` bir alt kümesi. Örneğin, daha önce bir üçgen çizmek için kullanılan kod örneği yerine, bellekte bir görüntüye çizmek için kullanılabilir bir `UIView`, aşağıda gösterildiği gibi:

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

Herhangi bir görüntüsünü yakalama çizimin bellek destekli bir bit eşlem için yaygın bir kullanımdır `UIView`. Örneğin, aşağıdaki kod bir bit eşlem bağlamı bir görünümün katmana işler ve oluşturur bir `UIImage` dışarı:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>PDF çizme

Görüntüleri ek olarak, çekirdek grafikleri PDF çizim destekler. Görüntüleri gibi bellekte bir PDF işleme yanı sıra işlemede için bir PDF okuma bir `UIView`.

### <a name="pdf-in-a-uiview"></a>Bir UIView PDF

Çekirdek grafikleri de destekleyen bir PDF bir dosyadan okunuyor ve kullanarak bir görünüm oluşturma `CGPDFDocument` sınıfı. `CGPDFDocument` Sınıfı kodda bir PDF temsil eder ve okuma ve sayfaları çizmek için kullanılan.

Örneğin, aşağıdaki kod bir `UIView` alt sınıf bir dosyaya bir PDF okur bir `CGPDFDocument`:

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

`Draw` Yöntemi sonra kullanabilir `CGPDFDocument` bir sayfaya okumak için `CGPDFPage` ve çağırarak işlemek `DrawPDFPage`, aşağıda gösterildiği gibi:

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

Bir bellek içi PDF için çağırarak bir PDF bağlam oluşturmanız gerekir. `BeginPDFContext`. Çizim PDF sayfalarına ayrıntılı ' dir. Her bir sayfa çağrılarak başlatılan `BeginPDFPage` ve çağırarak tamamlandı `EndPDFContent`, grafik ile kod arasında. Ayrıca, resim çizim ile bellek kullanır çizim PDF bir kaynak CTM yalnızca değiştirerek dikkate alınması alt sol yedeklenen gibi görüntülerle ister.

Aşağıdaki kod, bir PDF metin çizme gösterilmektedir:

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

Sonuçta elde edilen metin ardından yer PDF çizilmiş bir `NSData` , kaydedilebilir, karşıya yüklenen, e-postayla gönderilmiş, vb.


## <a name="summary"></a>Özet

Bu makalede aracılığıyla sağlanan grafik yetenekleri inceledik *çekirdek grafikleri* framework. Çekirdek grafikleri geometri, görüntüler ve PDF bağlamında çizmek için nasıl kullanılacağını gördüğümüz bir `UIView,` bellek destekli grafik bağlamları gibi için iyi.

## <a name="related-links"></a>İlgili bağlantılar

- [Çekirdek grafik örneği](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafikler ve animasyon gözden geçirme](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Temel Animasyon](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Animasyon tarif çekirdek](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
