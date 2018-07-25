---
title: CocosSharp ile 2B matematik
description: Bu kılavuz, oyun geliştirme için hazırlanan 2B matematik kapsar. Bu ortak oyun geliştirme görevleri nasıl gerçekleştireceğinizi gösterilecek CocosSharp kullanır ve bu görevleri arkasında matematik açıklar.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 63c722b74c7dc7e034475e539f38204aca87763e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242059"
---
# <a name="2d-math-with-cocossharp"></a>CocosSharp ile 2B matematik

_Bu kılavuz, oyun geliştirme için hazırlanan 2B matematik kapsar. Bu ortak oyun geliştirme görevleri nasıl gerçekleştireceğinizi gösterilecek CocosSharp kullanır ve bu görevleri arkasında matematik açıklar._

Getirin ve kod nesneleri taşımak için bir çekirdek, her boyuttaki geliştirme oyunlar parçasıdır. Taşıma düz bir çizgi veya döndürme için Trigonometri kullanımını bir nesnenin bir oyunu gerektirip gerektirmediği bilgisini konumlandırma ve taşıma matematik, kullanılmasını gerektirir. Bu belge aşağıdaki konuları ele alınacaktır:

 - Hız
 - Hızlandırma
 - CocosSharp nesneleri döndürme
 - Dönüş hızı ile kullanma

Geliştiricilerin güçlü matematik arka plan etkinleştirmemiş veya kimin uzun-Okul aşağıdaki konulardan unutmuş endişelenmenize gerek yok: Bu belge, küçük boyutlu parçalara bölecektir kavramları ve teorik açıklamaları pratik örnekler ile eşlik edecek. Kısacası, bu makalede tanılamalarını matematik Öğrenci sorusunu: "Ne zaman gerçekten ihtiyacım kullanabileceğimiz kullanılacak?"


## <a name="requirements"></a>Gereksinimler

Bu belge, öncelikli olarak matematik CocosSharp tarafında odaklanıyor olsa da, kod örnekleri form devralma nesnelerle çalışmayı varsayar `CCNode`. Ayrıca, bu yana `CCNode` değerlerini içermez hız ve Hızlandırma için kod VelocityX, VelocityY AccelerationX ve AccelerationY gibi değerler sağlayan varlıklarla çalışmaya varsayar. Varlıklar hakkında daha fazla bilgi için izlenecek yol bakın [cocossharp'taki varlıklar](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Hız

Oyun geliştiricileri terimi kullanın *hız* nasıl nesneyi taşıma – tanımlamak için özellikle ne kadar hızlı bir şey taşınıyor ve yön olduğundan taşıma. 

Hız birimleri iki tür kullanarak tanımlanır: konum birim ve bir zaman birimi. Örneğin, bir otomobilin hız saat başına mil veya kilometre saatlik olarak tanımlanır. Oyun geliştiriciler genellikle kullanım piksel ne kadar hızlı bir nesne tanımlamak için saniye başına taşır. Örneğin, bir madde işareti, saniye başına 300 piksel bir hızda taşıyabilir. Bir madde işareti, saniye başına 300 piksel taşıma ise diğer bir deyişle, ardından bunu 600 birim iki saniye ve 900 birimlerinde üç saniye ve benzeri taşınacaktır. Daha genel olarak, hız değeri *ekler* bir nesnenin konumuna (aşağıda anlatıldığı gibi).

Hızı birimlerinin hızı açıklamak için kullandığımız olsa da, terim hız hem hız hem de yönü başvuruyor ancak terimi hızını genellikle yönü, bağımsız bir değere başvuruyor. Bu nedenle, bir madde işareti 's hız (madde işareti gerekli özellikleri içeren bir sınıf olduğunu varsayarak) atamasını şöyle görünebilir:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Hız uygulama

CocosSharp hız, uygulamaz, taşıma gerektiren nesneler, kendi taşıma mantığı uygulamanız gerekir. Hız genellikle uygulama yeni oyun geliştiricileri, hız yapmadan yanlışı kare hızı üzerinde bağımlı hale getirin. Diğer bir deyişle, aşağıdaki *yanlış uygulama* doğru sonuçlar sağlamak için görünüyor ancak oyunun çerçeve hızınıza göre:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Oyun (örneğin, saniyede 30 kare yerine saniyede 60 kare) daha yüksek kare hızında çalıştırıyorsa, daha yavaş kare hızı çalışıyorsa daha hızlı taşımak için nesne görünür. Benzer şekilde, oyun kareleri (Bu cihazın kaynakları kullanarak arka plan işlemleri tarafından kaynaklanıyor olabilir) bir çerçeve oranının yüksek olarak işleyemedi, oyun yavaşlamasına görünür.

Bu hesap için hız genellikle bir saat değeri kullanılarak uygulanır. Örneğin, varsa `seconds` değişkeni temsil saniye sayısı (veya kesir) son saat hızı uygulandıktan sonra ardından aşağıdaki kodu kare hızını bağımsız olarak tutarlı taşıma sahip nesne neden olur:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Düşük kare hızı çalışan bir oyun nesnelerine daha az konumunu sık sık güncelleştirilir göz önünde bulundurun. Bu nedenle, her bir güncelleştirme oyunu daha sık güncelleme durumda daha fazla taşıma nesneleri neden olur. `seconds` Değeri hesaplar bunun için son güncelleştirmeden bu yana ne kadar zaman geçti raporlama tarafından.

Zamana bağlı taşıma ekleme örneği için bkz. [zaman kapsayan bu tarif tabanlı taşıma](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/game_development/time_based_movement).


### <a name="calculating-positions-using-velocity"></a>Hız kullanarak konumları hesaplanıyor

Hız, bir nesne sonra zaman geçişleri miktar burada olacaktır hakkında tahminde bulunmak amacıyla ya da nesnelerin davranışını oyunu çalıştırmak zorunda kalmadan ayarlamanıza yardımcı olmak için kullanılabilir. Örneğin, tetiklenme bir madde işareti hareketini uygulama geliştiriciye örneği oluşturulduğu sonra madde işareti'nın hız ayarlaması gerekiyor. Ekran boyutu, hız ayarlamak için bir temel sağlamak için kullanılabilir. Diğer bir deyişle, geliştirici biliyorsa madde işareti ekran yüksekliği 2 saniye cinsinden taşımanız gerekir ve ardından hız 2 bölünmüş ekran yüksekliği olarak ayarlanmalıdır. 800 piksel yüksekliktir ekran ise madde işareti'nın hızı (aynı 800/2) 400'e ayarlanır.

Benzer şekilde, oyun içi mantıksal bir nesne, hız belirtilen hedefe ulaşmak için ne kadar sürer hesaplamak gerekebilir. Bu uzaklık travelling nesnenin hız tarafından seyahat bölünmesiyle hesaplanır. Örneğin, aşağıdaki kod metin ne kadar süreyle görüntüleyen bir etiket atama gösterir bir Füze hedefine ulaşana kadar:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Hızlandırma

*Hızlandırma* ortak bir kavramıdır oyun geliştirme alanındaki ve hızı ile benzer paylaşır. Bir nesne hızlandırmadan ya da (nasıl hız değeri zaman içinde değişir) yavaşlatmasını hızlandırma niceliğini belirtir. Hızlandırma *ekler* yalnızca konumlandırmak için hız ekler gibi hız. Sık kullanılan uygulamaları hızlandırma yer çekimi hızlandırmadan bir araba ve bir alan kendi thrusters tetikleme sevk içerir. 

Benzer şekilde, hız, hızlandırma konumu ve zaman birimi tanımlanır; Ancak, hızlandırma'nın zaman birimi olarak adlandırılır bir *karesini* hızlandırma matematiksel olarak nasıl tanımlandığını yansıtan birim. Diğer bir deyişle, oyun hızlandırma genellikle ölçülür *piksel saniyedeki kare*.

Bir nesne bir kare saniyede 10 birim X ivmesini varsa, ardından kendi hız saniyede 10 tarafından artacağını anlamına gelir. Bir saniye başına saniye, sonra iki 10 birimlerde taşıma olacaktır sonra bir Durma noktasına başlatılıyorsa, 20 birim başına saniye vb. saniye.

Şu şekilde atanabilir şekilde hızlandırma iki boyutlu bir X ve Y bileşeni gerektirir:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Hızlandırma yavaşlama karşılaştırması

Bazen her gün konuşma ivme ve negatif ayırt edilen olsa da, ikisi arasındaki teknik fark yoktur. Yer çekimi içinde hızlandırma sonuçlanır bir zorla ' dir. Bir nesneyi yukarı oluşturulursa sonra yer çekimi bunu (yavaşlatma aşağı) yavaşlatır, ancak nesne tırmanma durdurdu ve dünya ile aynı yönde dönülüyor sonra sonra yer çekimi bunu (hızlandırma ayarlama) hızlandırma. Aynı yönde veya tersi yönde hareket uygulanıyor olmadığını aşağıda gösterildiği gibi bir hızlandırma aynı uygulamasıdır. 


### <a name="implementing-acceleration"></a>Hızlandırma uygulama

Hızlandırma uygularken hız için benzerdir: CocosSharp ile otomatik olarak uygulanmadı ve zamana bağlı Hızlandırma (aksine, kare tabanlı hızlandırma) istenen uygulamasıdır. Bu nedenle bir basit Hızlandırma (birlikte hızı) uygulaması gibi görünebilir:

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Yukarıdaki kod ne olarak adlandırılır olduğu bir *doğrusal bir yaklaştırma* hızlandırma uygulaması. Etkili bir şekilde oldukça yakın bir doğruluk derecesi ile hızlandırma uygular, ancak hızlandırma mükemmel bir şekilde doğru modeli değil. Bunu yukarıda hızlandırma nasıl uygulandığını kavramı anlaşılmasına yardımcı olacak dahil edilir.

Aşağıdaki uygulama hızlandırma ve hız matematiksel olarak doğru bir uygulamadır:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Yukarıdaki kod için en belirgin fark `halfSecondsSquared` değişkeni ve kullanımını konumlandırmak için hızlandırma uygulamak için. Matematik Bunun nedeni bu öğreticinin kapsamı dışındadır, ancak bunun ardındaki matematik ilginizi geliştiriciler hakkında daha fazla bilgi bulabilirsiniz [hızlandırma tümleştirme hakkındaki bu tartışma.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Pratik etkisi `halfSecondSquare` hızlandırma matematiksel olarak doğru bir şekilde ve daha öngörülebilir bir şekilde bağımsız olarak kare hızı davranış göstereceği anlamına olduğu. Doğrusal hızlandırması kare hızı tabi – düşük kare hızını yaklaşık daha az doğru olur bırakır değeridir. Kullanarak `halfSecondsSquared` garantileri kod kare hızını bağımsız olarak aynı şekilde davranır.


## <a name="angles-and-rotation"></a>Açıları ve döndürme

Görsel nesneler gibi `CCSprite` desteği aracılığıyla döndürme bir `Rotation` değişkeni. Bu bir değere derece döndürme ayarlanacak atanabilir. Örneğin, aşağıdaki kod nasıl döndürüleceğini gösterir bir `CCSprite` örneği:


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

Bu durum şunlara sebep olur:

![](math-images/image1.png "Bu ekran görüntüsünde bu sonuçları")

Döndürme (aynı geçmiş nedeniyle döndürme matematiksel olarak nasıl uygulanacağını tersi) saat yönünde 45 derece olduğuna dikkat edin:

![](math-images/image2.png "Döndürme matematiksel olarak nasıl uygulanacağını tersi geçmiş nedeniyle olduğu döndürme 45 derece saat yönünde olduğuna dikkat edin")

Genel döndürme CocosSharp ve normal Matematiği için şu şekilde görselleştirilebilir:

![](math-images/image3.png "Genel döndürme CocosSharp ve normal Matematiği için şu şekilde görselleştirilebilir")

![](math-images/image4.png "Genel döndürme CocosSharp ve normal Matematiği için şu şekilde görselleştirilebilir")

Bu ayrım önemlidir çünkü `System.Math` sınıfı kullanır; böylelikle geliştiriciler bu sınıfla ilgili bilgi sahibi ile çalışırken açıları ters çevirmek gereken saat yönünün tersine dönüş `CCNode` örnekleri.

Yukarıdaki diyagramları derece döndürme görüntüleme dikkat etmelidir; Ancak, bazı matematik işlevleri (işlevler gibi `System.Math` ad alanı) bekler ve dönüş değerleri *radyan* derece yerine. Bu kılavuzdaki biraz daha sonra iki birim türleri arasında dönüştürme inceleyeceğiz.


### <a name="rotating-to-face-a-direction"></a>Yüz yön için döndürme

Yukarıda gösterildiği gibi `CCSprite` kullanarak döndürülebilir `Rotation` özelliği. `Rotation` Özelliği tarafından sağlanır `CCNode` (için temel sınıf `CCSprite`), yani döndürme devralınacak varlıkları uygulanabilir `CCNode` de. 

Bazı oyunlar, bunlar hedef yüz için döndürülecek nesne gerektirir. Bir bilgisayar denetimli ikincisinde bir oynatıcı hedef ya da kullanıcının ekran burada temas noktası doğru giden bir alanı sevk atma örneklerindendir. Ancak, dönüş değeri ilk tabanlı Döndürülmüş varlık konumunu ve yüz için hedef konumu hesaplanması gerekir.

Bu işlem birkaç adımı gerektirir:

 - Tanımlama *uzaklığı* hedef. X ve Y uzaklık hedef varlık döndürme varlık arasındaki uzaklığı ifade eder.
 - Uzaklık döndürüleceği açıyı (aşağıda ayrıntılı olarak açıklanmıştır) arktanjantını Trigonometri işlevini kullanarak hesaplanıyor.
 - Sağa ve geri Döndürülmüş olduğunda döndürme varlığı işaret yön doğrultusunda gösteren 0 derece arasında bir fark için ayarlanıyor.
 - Matematik döndürme (saat yönünün tersine) CocosSharp döndürme (saat yönünde) arasındaki fark için ayarlanıyor.

Yüz tanıma bir hedef Varlık (bir varlıkta yazılacak varsayıldı) aşağıdaki işlevi döndürür:


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

Yukarıdaki kod, burada kullanıcının ekran aşağıdaki gibi oncollisionstay noktası yüzler için bir varlık döndürmek için kullanılabilir:


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

Bu kod, aşağıdaki davranışa neden olur:

![](math-images/image5.gif "Bu kod bu davranışla sonuçlanır.")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>Atan2 kullanarak dönüştürmek için açıları kaydırır

`System.Math.Atan2` Açı için bir uzaklık dönüştürmek için kullanılabilir. İşlev adı `Atan2` trigonometrik işlevi arktanjantını gelir. "2" soneki bu işlev standart ayırır `Atan` kesinlikle arktanjantını matematik davranışını eşleşen işlevi. Arktanjant, -90 arasında bir değer döndüren bir işlev olan ve + 90 derece (veya eşdeğer radyan cinsinden). Birçok uygulama, bilgisayar oyunları dahil olmak üzere değerlerin tam bir 360 derece genellikle gerektirir böylece `Math` sınıfı içeren `Atan2` bu ihtiyacı karşılamak için.

Yukarıdaki kod Y parametresi ilk olarak, ardından X parametresi çağırırken geçmesini fark `Atan2` yöntemi. Geriye dönük normal X, Y koordinatları konumu sıralama gelen budur. Daha fazla bilgi için [Atan2 belgelere bakın](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx).

Ayrıca hatalarının ayıklanabileceğini belirtmekte yarar döndürülen değeri, `Atan2` radyan cinsinden olacağı açıları ölçmek için kullanılan başka bir birimdir. Bu kılavuzda değil radyan ayrıntılarını kapsar, ancak göz önünde bulundurun, tüm trigonometrik işlevler `System.Math` ad alanını kullanacak radyan için herhangi bir değere derece CocosSharp nesneler üzerinde kullanılmadan önce dönüştürülmelidir. Radyan cinsinden hakkında daha fazla bilgi bulunabilir [radian Wikipedia sayfasında](http://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>İletme açısı

Bir kez `FacePoint` yöntemi açı radyana dönüştürür, tanımladığı bir `forwardAngle` değeri. Bu değeri, dönüş değeri 0 olduğunda varlık yaşıyor açıyı temsil eder. Bu örnekte, 90 derece matematiksel bir döndürme (aksine CocosSharp döndürme) kullanırken olduğu varlık yukarı, yaşıyor varsayıyoruz. Biz döndürme CocosSharp için henüz ters henüz bu yana matematik döndürme burada kullanırız.

Bir varlık ile aşağıdaki gösterir bir `forwardAngle` 90 derece aşağıdaki gibi görünmelidir:

![](math-images/image6.png "Bu bir forwardAngle 90 derece sahip bir varlık aşağıdaki gibi görünmelidir gösterir.")


### <a name="angled-velocity"></a>Açılı hız

Şu ana kadar bir açının bir uzaklık dönüştürme inceledik. Bu bölümde, başka bir şekilde aşması – açı alır ve X dönüştürür ve Y değerleri. Bir araba, karşılıklı yönünü veya bir boşluk sevk karşılıklı yönde taşıyan bir madde işareti atma sevk taşıma ortak örneklerindendir. 

Kavramsal olarak, hız, geri döndürülen, istenen hız ilk tanımlama, daha sonra bir varlığa yönelik bir açıyla, hız döndürme hesaplanabilir. Bu kavramı anlaşılmasına yardımcı olacak hız (ve Hızlandırma) bir 2 boyutlu görselleştirilebilir *vektör* (genellikle çizilmiş bir ok olarak). Bir vektör X ile hız değeri 100 ve Y = = 0 görselleştirilmiş gibi:

![](math-images/image7.png "Bir vektör X ile hız değeri 100 ve Y = = 0 görselleştirilmiş gibi")

İçinde yeni bir hız elde etmek, bu vektörü döndürülebilir. Örneğin, vektör (yönünün döndürme kullanarak) 45 derece döndürme durum şunlara sebep olur:

![](math-images/image8.png "Vektör bu yönünün döndürme sonuçları kullanarak 45 derece döndürme")

Neyse ki, hız, hızlandırma ve hatta konumu tüm değerleri nasıl uygulandığına bakılmaksızın aynı kodla döndürülebilir. Aşağıdaki genel amaçlı işlevi, bir vektör döndürmek için bir CocosSharp dönüş değeri tarafından kullanılabilir:


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

Tam bir anlayış `RotateVector` yöntemi gerektirir, bu makalenin kapsamı dışındadır bazı doğrusal Cebir, birlikte kosinüsü ve sinüs trigonometrik işlevler hakkında bilgi sahibi olan. Ancak, bir kez gerçekleştirilen `RotateVector` yöntemi, hız vektör dahil olmak üzere herhangi bir vektör döndürmek için kullanılabilir. Örneğin, aşağıdaki kod bir madde işareti tarafından belirtilen yönde yangın `rotation` değeri:


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

Bu kod aşağıdaki gibi oluşturulabilir:

![](math-images/image9.png "Bu kod, bu ekran görüntüsüne benzer bir şey oluşturabilir")


## <a name="summary"></a>Özet

Bu kılavuz, 2D oyun geliştirme ortak matematik kavramları kapsar. Atama ve hız ve Hızlandırma uygulamak gösterilmektedir ve nesneleri ve herhangi bir yönde hareket vektörleri döndürmek nasıl etkinleştireceğinizi de açıklar.
