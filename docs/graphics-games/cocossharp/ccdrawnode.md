---
title: Geometri CCDrawNode ile çizme
description: CCDrawNode satırları, daire ve üçgen gibi temel çizim nesneler için yöntemleri sağlar.
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 83d973ed013d1448915ee553069c1366922d28b6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="drawing-geometry-with-ccdrawnode"></a>Geometri CCDrawNode ile çizme

_`CCDrawNode` Satırlar, daire ve üçgen gibi temel çizim nesneler için yöntemleri sağlar._

`CCDrawNode` CocosSharp sınıfında ortak geometrik şekiller çizmek için birden çok yöntemler sağlar. Öğesinden devralınan `CCNode` sınıfı ve genellikle eklenir `CCLayer` örnekleri. Bu kılavuz kullanmayı kapsar `CCDrawNode` özel işleme gerçekleştirmek için örnekleri. Ayrıca kullanılabilir çizim işlevleri kapsamlı bir listesini ekran görüntüleri ve kod örnekleri sağlar.


## <a name="creating-a-ccdrawnode"></a>Bir CCDrawNode oluşturma

`CCDrawNode` Sınıfı, daire, dikdörtgenin ve satırları gibi geometrik nesneleri çizmek için kullanılabilir. Örneğin, aşağıdaki kod örneği nasıl oluşturulacağını gösterir bir `CCDrawNode` bir daire çizer örneğinin bir `CCLayer` sınıfı uygulama:


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

Bu kod, çalışma zamanında aşağıdaki daire üretir:

![](ccdrawnode-images/image1.png "Çalışma zamanında bu daire bu kod üretmez")


## <a name="draw-method-details"></a>Yöntem ayrıntıları çizme

Çizim ile ilgili birkaç ayrıntı bir göz atalım bir `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>Göreli CCDrawNode konumlar olan yöntemleri çizme

Tüm çizim yöntemleri çizim için en az bir konum değeri gerektirir. Bu konum göreli değerdir `CCDrawNode` örneği. Bunun anlamı `CCDrawNode` kendisini bir konuma sahiptir ve tüm yapılan çağrıları çizin `CCDrawNode` de bir veya daha fazla konum değerleri alır. Bu değerleri nasıl birleştirmek anlamanıza yardımcı olması için bazı örnekler bakalım.

İlk inceleyeceğiz `DrawCircle` Yukarıdaki örnek:


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

Bu durumda, `CCDrawNode` (100,100) konumlandırılmış ve çizilmiş daire (0,0) göreli `CCDrawNode`, sonuçta elde edilen en fazla ve oyun ekranın sol alt köşede sağındaki ortalanmış 100 piksel olan daire içinde.

`CCDrawNode` De başlangıç (ekranın sol alt,) konumlandırılmış uzaklıkları daire güvenmek:


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

50 birime dairenin merkezinde sonuçlarında yukarıdaki kodu (`drawNode.PositionX` + `CCPoint.X`) ekran ve 60 sol tarafındaki sağındaki (`drawNode.PositionY` + `CCPoint.Y`) birimleri ekranın üstünde.

Draw yöntemi çağrıldığında çizilmiş nesne sürece değiştirilemez `CCDrawNode.Clear` yöntemi çağrıldığında, üzerinde yapılması gereken şekilde yeniden konumlandırma `CCDrawNode` kendisi.

Tarafından çizilmiş nesneleri `CCNodes` tarafından da etkilenen `CCNode` örneğinin `Rotation` ve `Scale` özellikleri.


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Çizim yöntemleri, her çerçeve çağrılması gerekmez.

Çizim yöntemleri kalıcı bir görsel oluşturmak için yalnızca bir kez çağrılması gerekir. Çağrısı yukarıdaki örnekte `DrawCircle` oluşturucusunun içinde `GameLayer` – `DrawCircle` çerçeve her ekran yenilendiğinde daire yeniden çizmek için çağrılması gerekmez.

Bu, genellikle yalnızca bir çerçeve ekranına bir şey işlemez ve çerçeve her çağrılmalıdır MonoGame çizim yöntemleri farklıdır.

Her çerçeve çizim yöntemleri olarak adlandırılan sonra nesneleri sonunda içinde arama accumulate `CCDrawNode` örneği, bir düşüş kare hızı, daha fazla nesneleri çizilmiş gibi sonuçlanır.


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>Birden fazla çizim çağrı her CCDrawNode destekler

`CCDrawNode` örnekleri birden çok şekiller çizmek için kullanılabilir. Bu karmaşık visual nesnelerinin tek bir nesnede örtülen olanak verir. Örneğin, aşağıdaki kodu biriyle birden çok daireler işlemek için kullanılabilir `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

Aşağıdaki grafikte sonuçları:

![](ccdrawnode-images/image2.png "Bu grafikte sonuçları")


## <a name="draw-call-examples"></a>Çağrı örnekler çizme

Aşağıdaki çizim çağrıları kullanılabilir `CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` değişken sayıda noktaları aracılığıyla eğri bir çizgi oluşturur. 

`config` Parametre eğri aracılığıyla geçecek hangi noktaları tanımlar. Aşağıdaki örnek, dört noktaları üzerinden geçen bir eğri gösterir.

`tension` Parametre denetimlerini nasıl diyez veya eğri noktalarında round görünür. A `tension` 0 değeri bir eğri eğri içinde neden olur ve bir `tension` 1 değerini düz çizgiler ve sabit kenarları tarafından çizilen eğri neden olur.

Eğrileri eğri çizgiler olsa da, CocosSharp Eğrileri düz çizgili çizer. `segments` Parametresi denetler eğri çizmek için kullanılacak kaç kesim. Daha büyük bir sayı sorunsuz eğri eğri performans artırılabilir sonuçlanır. 

Kod örneği:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "Eğri çizmek için kullanılacak kaç kesim kesimleri parametre denetler")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` Eğri bir çizgi geçen noktaları, benzer değişken sayıda oluşturur `DrawCardinalLine`. Bu yöntem gerilim parametre içermez.

Kod örneği:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "Eğri bir çizgi geçen noktası DrawCardinalLine için benzer bir değişken sayısı DrawCatmullRom oluşturur")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` bir çevre dairenin oluşturur bir verilen `radius`.

Kod örneği:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "Belirli bir RADIUS dairenin çevre DrawCircle oluşturur")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` denetim noktaları arasında iki nokta yolunu ayarlamak için kullanarak, iki nokta arasındaki eğri bir çizgi çizer.

Kod örneği:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier eğri bir çizgi arasında iki nokta çizer.")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` özetini oluşturur bir *elips*, genellikle adlandırılır oval (iki geometrik aynı olmasa da). Elips şeklini tarafından tanımlanan bir `CCRect` örneği.

Kod örneği:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "DrawEllipse oval genellikle adlandırılır elips özetini oluşturur")


### <a name="drawline"></a>DrawLine

`DrawLine` belirli bir genişlikte bir çizgiyle noktalarına bağlanır. Bu yöntem benzer `DrawSegment`, yuvarlak uç noktaları aksine düz uç noktalar oluşturur dışında.

Kod örneği:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "Belirli bir genişlikte bir satır noktalarıyla DrawLine bağlanır")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` tarafından belirlenen nokta çiftleri bağlayarak birden fazla satır oluşturur bir `CCV3F_C4B` dizi. `CCV3F_C4B` Yapı konumu ve rengi için değerleri içerir. `verts` Parametresi her zaman içermelidir noktalarının, çift sayı her satırın iki noktaları tarafından tanımlanan.

Kod örneği:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Her iki noktaları tarafından tanımlanan verts parametresi her zaman noktalarının, çift sayı içermelidir")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` doldurulmuş çokgen bir değişken genişlik ve renk özetini oluşturur.

Kod örneği:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon doldurulmuş çokgen bir değişken genişlik ve renk özetini oluşturur.")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` iki nokta ile bir satır bağlanır. Benzer şekilde davranır `DrawCubicBezier` ancak yalnızca bir tek bir denetim noktası destekler.

Kod örneği:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier iki nokta ile bir satır bağlanır.")


### <a name="drawrect"></a>DrawRect

`DrawRect` doldurulmuş dikdörtgen bir değişken genişlik ve renk özetini oluşturur.

Kod örneği: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect doldurulmuş dikdörtgen bir değişken genişlik ve renk özetini oluşturur.")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` iki nokta satırı değişken genişlik ve renk ile bağlanır. Aşağıdakine benzer `DrawLine`, dışında düz uç noktaları yerine yuvarlak uç noktaları oluşturur.

Kod örneği:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment iki nokta satırı değişken genişlik ve renk ile bağlanır.")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` doldurulmuş bir üçgen verilen renk ve RADIUS oluşturur.

Kod örneği:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "Doldurulmuş bir üçgen verilen renk ve RADIUS DrawSolidArc oluşturur")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` doldurulmuş bir daire verilen RADIUS oluşturur.

Kod örneği:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "Doldurulmuş bir daire verilen RADIUS DrawCircle oluşturur")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` üçgenler listesini oluşturur. Her üçgen üç tarafından tanımlanan `CCV3F_C4B` bir dizi örnekleri. Dizi tepe sayısı geçirilen `verts` parametresi üç katı olmalıdır. Tek bilgi yer alan Not `CCV3F_C4B` verts ve bunların renk – konumudur `DrawTriangleList` yöntemi dokuları çizim üçgenle desteklemez.

Kod örneği:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList üçgenler listesini oluşturur")


## <a name="summary"></a>Özet

Bu kılavuz nasıl oluşturulacağını açıklar bir `CCDrawNode` ve ilkel tabanlı işleme gerçekleştirin. Her bir çizim çağrıları bir örnek sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [Tam örnek](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
