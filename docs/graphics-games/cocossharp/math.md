---
title: 2B matematik CocosSharp ile
description: Bu kılavuz, oyun geliştirmeye yönelik 2B matematik kapsar. Ortak oyun geliştirme görevleri gerçekleştirmek nasıl göstermek için CocosSharp kullanır ve bu görevlerin arkasında matematik açıklar.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: ae1300936a24ac1381496eaaf78aefb875bd5ed6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="2d-math-with-cocossharp"></a>2B matematik CocosSharp ile

_Bu kılavuz, oyun geliştirmeye yönelik 2B matematik kapsar. Ortak oyun geliştirme görevleri gerçekleştirmek nasıl göstermek için CocosSharp kullanır ve bu görevlerin arkasında matematik açıklar._

Konum ve kod nesneleriyle taşımak için bir çekirdek ölçekteki geliştirme oyunlar parçasıdır. Oyun düz bir çizgi ya da döndürme Trigonometri kullanılmak boyunca bir nesneyi taşıma sağlanıp sağlanmadığını konumlandırma ve taşıma matematik, kullanılmasını gerektirir. Bu belge aşağıdaki konular ele alınacaktır:

 - Hız
 - Hızlandırma
 - CocosSharp nesneleri döndürme
 - Döndürme hız ile kullanma

Güçlü matematik arka plan yok ya da kimin uzun-okul, aşağıdaki konulardan unutmuş geliştiriciler endişelenmeniz gerekmez – bu belgeyi kavramları bite ölçekli parçalara bölecektir ve teorik açıklamaları pratik örneklerle eşlik. Kısacası, bu makalede Paskalya'da matematik Öğrenci soruyu yanıtlayın: "Ne zaman gerçekte gerekir bu olanaklardan kullanılacak?"


## <a name="requirements"></a>Gereksinimler

Bu belge öncelikle CocosSharp matematiksel tarafında odaklanıyor olsa da, kod örnekleri form devralma nesnelerle çalışmayı varsayın `CCNode`. Ayrıca, bu yana `CCNode` değerleri içermez hız ve Hızlandırma için kod VelocityX, VelocityY, AccelerationX ve AccelerationY gibi değerler sağlayan varlıklarla çalışmaya varsayar. Bizim izlenecek varlıklar hakkında daha fazla bilgi için bakın [CocosSharp varlıklarda](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Hız

Oyun geliştiricilerin terim kullanın *hız* nasıl bir nesne taşıma – tanımlamak için özellikle ne kadar hızlı hareket şeydir ve yönü, BT'nin taşıma. 

Hız, iki tür birimleri kullanılarak tanımlanır: konum birim ve bir zaman birimi. Örneğin, bir otomobilin hızı, saat başına mil veya kilometre saatte olarak tanımlanır. Oyun geliştiriciler genellikle kullanım piksel ne kadar hızlı nesneyi tanımlamak için saniye başına taşır. Örneğin, bir madde işareti, saniye başına 300 piksel hızında taşıyabilir. Madde işareti, saniye başına 300 piksel taşıma ise diğer bir deyişle, sonra da 600 birim iki saniye ve 900 birimleri üç saniye ve benzeri taşınacaktır. Daha fazla genel olarak, hız değeri *ekler* bir nesnenin bir konuma (aşağıda anlatıldığı gibi).

Hız birimleri açıklamak için hız kullandık rağmen hızı ve yön için terim hız başvuruyor ancak terim hızı genellikle yönünü, bağımsız bir değer anlamına gelir. Bu nedenle, madde işareti 's hız (madde işareti gerekli özellikleri içeren bir sınıf olduğunu varsayarak) atama şöyle görünebilir:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Hız uygulama

Taşıma gerektiren nesneler, kendi taşıma mantığı uygulamanız gerekir böylece CocosSharp hız, uygulamıyor. Hız genellikle uygulama yeni oyun geliştiricilerin kendi hız yapma hata kare hızı bağımlı hale getirin. Diğer bir deyişle, aşağıdaki *yanlış uygulama* doğru sonuçlar sağlamak için göründüğü ancak oyunun çerçeve oranına dayalı:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Oyun (örneğin, saniyede 30 kare yerine saniyede 60 çerçeve) daha yüksek bir kare hızında çalıştırıyorsa, yavaş bir çerçeve hızında çalıştırılmadığını daha hızlı taşımak için nesne görüntülenir. Benzer şekilde, oyun (hangi aygıtın kaynaklarını kullanan arka plan işlemleri tarafından kaynaklanıyor olabilir) bir çerçeve oranının yüksek olarak kareleri işleyemiyor ise, oyun yavaşlaması görünecektir.

Bu hesap için hız genellikle bir saat değeri kullanılarak uygulanır. Örneğin, varsa `seconds` değişkeni temsil saniye sayısı (veya kesir) son zamanı hız uygulandıktan sonra aşağıdaki kodu kare hızı bağımsız olarak tutarlı taşıma sahip nesne neden olacağından sonra:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Daha düşük bir çerçeve hızda çalışan oyun nesnelerine daha az konumunu sık güncelleştireceği göz önünde bulundurun. Bu nedenle, her güncelleştirme oyun daha sık güncelleştirmekte olduğunuz durumda daha fazla taşıma nesneleri neden olur. `seconds` Değer hesapları bu için ne kadar süre son güncelleştirmeden bu yana geçen bildirerek.

Zaman tabanlı hareket ekleme örneği için bkz: [zaman kapsayan bu tarif tabanlı taşıma](https://developer.xamarin.com/recipes/cross-platform/game_development/time_based_movement/).


### <a name="calculating-positions-using-velocity"></a>Hız kullanarak konumlar hesaplama

Hız, bir nesne zamanı geçişleri miktar sonra burada olacaktır ilgili tahminlerde ya da nesnelerin davranışını oyunu çalıştırmak gerek kalmadan ince ayar yardımcı olmak için kullanılabilir. Örneğin, bu örneği sonra madde işareti 's hız ayarlamak Mazotlu madde işareti hareketini uygulayan bir geliştirici gerekir. Ekran boyutu hız ayarlamak için bir temel sağlamak üzere kullanılabilir. Diğer bir deyişle, geliştirici biliyorsa madde işareti 2 saniye cinsinden ekran yüksekliğini taşımalısınız sonra hız 2 ile bölünmüş ekran yüksekliğini ayarlanmalıdır. Ekran 800 piksel uzunluğunda ise, madde işareti'nın hızı (Bu 800/2) 400'e ayarlanır.

Benzer şekilde, oyun mantığı, bir nesne, hız verilen bir hedefe ulaşmak için ne kadar sürer hesaplamak gerekebilir. Bu travelling nesnenin hız tarafından seyahat uzaklığı bölünmesiyle hesaplanır. Örneğin, aşağıdaki kodu metnin ne kadar süreyle görüntüleyen bir etiket atama gösterilmiştir hedefine bir Füze ulaşana kadar:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Hızlandırma

*Hızlandırma* oyun geliştirmede ortak bir kavramıdır ve hız ile benzer şekilde paylaşır. Bir nesne hızlandırma ya da (nasıl hız değeri zaman içinde değişir) yavaşlamasının hızlandırma niceliğini belirtir. Hızlandırma *ekler* yalnızca konumlandırmak için hız ekler gibi hız için. Hızlandırma ortak uygulamalarının yer çekimi, bir araba hızlandırma ve kendi thrusters tetikleme alanı sevk içerir. 

Benzer şekilde hız, hızlandırma bir konumu ve zaman birimi tanımlanır; Ancak, hızlandırma'nin zaman birimi olarak adlandırılır bir *squared* nasıl hızlandırma matematiksel olarak tanımlanan yansıtır birim. Diğer bir deyişle, oyun hızlandırma genellikle ölçülür *piksel saniyedeki kare*.

Bir nesne kare saniye başına 10 birim X ivmesini varsa, daha sonra kendi hız 10 saniyede artıracaktır anlamına gelir. Bir saniye ikinci, sonra iki başına 10 birimlerde taşıma olacaktır sonra Durma noktasına başlıyorsanız 20 birim başına ikinci, vb. saniye.

Hızlandırma iki boyutlu bir X ve Y bileşeni gerektirir ve bu nedenle şu şekilde atanabilir:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Hızlandırma yavaşlama karşılaştırması

Hızlandırma ve yavaşlama her gün konuşma bazen ayırt edilen rağmen ikisi arasındaki teknik fark yoktur. Yer çekimi hızlandırma sonuçları bir force ' dir. Nesneyi yukarı oluşturulursa sonra yer çekimi, (yavaşlatıcı aşağı) yavaş, ancak nesne climbing durduruldu ve yer çekimi ile aynı yönde dönmeden sonra ardından yer çekimi bunu (hızlandırmaya yukarı) hızlandırma. Aynı yönde veya tersi yönde hareket uygulanmakta olup olmadığını aşağıda gösterildiği gibi bir hızlandırma aynı uygulamasıdır. 


### <a name="implementing-acceleration"></a>Hızlandırma uygulama

Hızlandırma uygularken için hız benzer – CocosSharp tarafından otomatik olarak uygulanmadı ve zaman tabanlı Hızlandırma (aksine hızlandırma tabanlı çerçeve) istenen uygulamasıdır. Bu nedenle bir basit Hızlandırma (birlikte hız) uygulaması gibi görünebilir:

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Yukarıdaki kodu ne olarak adlandırılır bir *doğrusal yaklaşık* hızlandırma uygulaması. Etkili bir şekilde oldukça Kapat düzeyde doğruluk hızlandırma uygular, ancak hızlandırma mükemmel doğru modeli değil. Bunu yukarıda hızlandırma nasıl uygulandığını kavramı açıklamaya yardımcı olması için dahil edilir.

Aşağıdaki uygulama hızlandırma ve hız, matematiksel olarak doğru bir uygulamadır:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Yukarıdaki kod için en bariz fark `halfSecondsSquared` değişkeni ve konumlandırmak için hızlandırma uygulamak için kullanım. Matematik Bunun nedeni Bu öğretici kapsamında değildir, ancak bu arkasında matematik ilginizi geliştiriciler hakkında daha fazla bilgi bulabilirsiniz [bu tartışma hızlandırma tümleştirme hakkında.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Pratik etkisini `halfSecondSquare` hızlandırma matematiksel tutarlı ve öngörülebilir bir şekilde kare hızı bağımsız olarak davranacak emin olan. Doğrusal yaklaşık hızlandırma olarak çerçeve tabi – düşük kare hızı az doğru yaklaşık hale bırakır hızıdır. Kullanarak `halfSecondsSquared` garanti kodu kare hızı bakılmaksızın aynı şekilde davranır.


## <a name="angles-and-rotation"></a>Açıları ve döndürme

Visual nesneleri gibi `CCSprite` destek döndürme aracılığıyla bir `Rotation` değişkeni. Bu bir değeri döndürme derece cinsinden ayarlamak için atanabilir. Örneğin, aşağıdaki kodu nasıl döndürüleceğini gösterir bir `CCSprite` örneği:


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

![](math-images/image1.png "Bu bu ekran görüntüsünde sonuçları")

45 (olan geçmiş nedeniyle döndürme matematiksel nasıl uygulandığını tersi) derece saat yönünde bir döndürme olduğuna dikkat edin:

![](math-images/image2.png "Geçmiş nedenleri döndürme matematiksel nasıl uygulandığını karşıtı olduğu döndürme 45 derece saat yönünde olduğuna dikkat edin")

Genel olarak aşağıdaki gibi CocosSharp ve normal matematik dönüş canlandırılabilir:

![](math-images/image3.png "Genel CocosSharp ve normal matematik dönüş şöyle canlandırılabilir")

![](math-images/image4.png "Genel CocosSharp ve normal matematik dönüş şöyle canlandırılabilir")

Bu ayrım önemlidir çünkü `System.Math` sınıfı, böylece geliştiriciler bu sınıfla tanıdık açıları ile çalışırken Çevir gerek saatin aksi yönünde bir döndürme kullanır `CCNode` örnekleri.

Yukarıdaki diyagramları derece cinsinden döndürme görüntülemek dikkat etmelidir; Ancak, bazı matematik işlevleri (işlevlerde gibi `System.Math` ad alanı) bekler ve dönüş değerleri *radyan* derece yerine. Bu kılavuzda biraz daha sonra iki birim türü arasında dönüştürme inceleyeceğiz.


### <a name="rotating-to-face-a-direction"></a>Bir yön yüz döndürme

Yukarıda gösterildiği gibi `CCSprite` kullanarak döndürülüp `Rotation` özelliği. `Rotation` Özelliği tarafından sağlanan `CCNode` (için temel sınıfı `CCSprite`), başka bir deyişle, döndürme devralınmalıdır varlıkları uygulanabilir `CCNode` de. 

Bazı oyunlar hedef yüz şekilde döndürülüp nesnelere gerektirir. Örnek bir oynatıcı hedefi veya kullanıcı ekran burada temas noktası doğru giden bir alanı sevk çekim bilgisayar denetimli bir ikincisinde verilebilir. Ancak, bir dönüş değeri ilk tabanlı Döndürülmüş varlık konumu ve yüz hedef konumu hesaplanması gerekir.

Bu işlem birkaç adımı gerektirir:

 - Tanımlayan *uzaklık* hedef. Uzaklık döndürme varlık ve hedef varlık arasındaki X ve Y uzaklığı ifade eder.
 - Uzaklık döndürüleceği açıyı (aşağıda ayrıntılı olarak açıklanmıştır) arktanjantını Trigonometri işlevini kullanarak hesaplama.
 - 0 sağa ve beklemediğiniz Döndürülmüş zaman döndürme varlık işaret yönü doğru işaret eden derece arasında bir fark için ayarlama.
 - Matematik döndürme (saatin tersi yönde) ve CocosSharp döndürme (saat yönünde) arasındaki fark için ayarlama.

(Bir varlık yazılması varsayılır) aşağıdaki işlevi bir hedef yüz varlık döndürür:


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

Yukarıdaki kod, burada kullanıcının ekran gibi değecek noktası bakarken şekilde bir varlık döndürmek için kullanılabilir:


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

Bu kod, aşağıdaki davranış oluşur:

![](math-images/image5.gif "Bu kod bu davranışa neden olur")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>Atan2 kullanarak açıları kaydırır

`System.Math.Atan2` Bir açının uzaklığı dönüştürmek için kullanılabilir. İşlev adı `Atan2` trigonometrik işlevi arktanjantını gelir. "2" soneki bu işlev standart ayıran `Atan` kesinlikle arktanjantını matematiksel davranışını eşleşen işlevi. Arctangent -90 arasında bir değer döndüren bir işlev değil ve + 90 derece (veya eşdeğeri radyan cinsinden). Bilgisayar oyunları dahil olmak üzere birçok uygulama, genellikle tam bir 360 derece değerleri gerektiren böylece `Math` sınıfı içerir `Atan2` bu gereksinimi karşılamak için.

Yukarıdaki kod Y parametresi ilk olarak, ardından X parametresi çağrılırken geçtiğini fark `Atan2` yöntemi. Bu geriye doğru normal X, Y konum koordinatlarını sıralama arasındadır. Daha fazla bilgi için [Atan2 belgeleri bkz](https://msdn.microsoft.com/en-us/library/system.math.atan2(v=vs.110).aspx).

Ayrıca dikkate değerdir dönüş değeri, `Atan2` radyan cinsinden olduğu açıları ölçmek için kullanılan başka bir birimdir. Bu kılavuz değil radyan ayrıntılarını içerir, ancak unutmayın, tüm trigonometrik işlevler `System.Math` ad alanı, herhangi bir değere derece CocosSharp nesnelerde kullanılmadan önce dönüştürülmesi gerekir böylece radyan kullanın. Radyan cinsinden hakkında daha fazla bilgi bulunabilir [radian Wikipedia sayfasındaki](http://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>İletme açı

Bir kez `FacePoint` yöntemi için radyan cinsinden açı dönüştürür, onu tanımlayan bir `forwardAngle` değeri. Bu değer döndürme değeri 0 değerine eşit olduğunda varlık'ın kullanıma yönelik açıyı temsil eder. Bu örnekte, varlık yukarı, 90 derece matematiksel bir döndürme (aksine CocosSharp dönüş) kullanırken olduğu karşılıklı olduğunu varsayalım. Biz döndürme CocosSharp için henüz ters henüz bu yana matematiksel döndürme burada kullanırız.

Aşağıdaki hangi bir varlıkla gösterir bir `forwardAngle` 90 derece aşağıdaki gibi görünmelidir:

![](math-images/image6.png "Bu varlıkta forwardAngle 90 derece ile aşağıdaki gibi görünmelidir gösterir")


### <a name="angled-velocity"></a>Açılı hız

Şu ana kadar açının bir uzaklık dönüştürme inceledik. Bu bölümde, diğer bir yol gider – açı alır ve X dönüştürür ve Y değerleri. Bu karşılıklı yönü veya sevk karşılıklı yönde taşır bir madde işareti çekim alanı sevk taşıma bir araba ortak örnekler. 

Kavramsal olarak, hız, geri döndürülen olduğunda istenen hız ilk tanımlama, ardından bu hız bir varlık karşılıklı açıyla döndürme hesaplanabilir. Bu kavram açıklamasına yardımcı olmak için hız (ve Hızlandırma) bir 2 boyutlu canlandırılabilir *vektör* (genellikle çizilmiş bir ok olarak). Vektör X ile hız değeri 100 ve Y = = 0 görselleştirilen gibi:

![](math-images/image7.png "Vektör X ile hız değeri 100 ve Y = = 0 görselleştirilen gibi")

Bu vektörü içinde yeni bir hız neden için döndürülebilir. Örneğin, 45 (yönünün döndürme kullanarak) derece vektör döndürme şunlara sebep olur:

![](math-images/image8.png "Vektör 45 bu yönünün döndürme sonuçları kullanarak derece döndürme")

Neyse ki, hız, hızlandırma ve hatta konumu tüm değerlerin nasıl uygulandığını bakılmaksızın aynı kodla döndürülebilir. Aşağıdaki genel amaçlı işlevi tarafından CocosSharp döndürme değeri vektör döndürmek için kullanılabilir:


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

Tam bir anlayış `RotateVector` yöntemi, bu makalenin kapsamı dışındadır olan bazı Lineer Cebir, birlikte kosinüsünü ve sinüsünü trigonometrik işlevler aşina olan gerektirir. Ancak, bir kez uygulanan `RotateVector` yöntemi, hız vektör dahil olmak üzere herhangi bir vektör döndürmek için kullanılabilir. Örneğin, aşağıdaki kodu tarafından belirtilen yönde madde işareti yangın `rotation` değeri:


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

Bu kodu aşağıdakine benzer oluşturabilir:

![](math-images/image9.png "Bu kod bu ekran görüntüsüne benzer bir şey üretebilir")


## <a name="summary"></a>Özet

Bu kılavuz 2B oyun geliştirmede ortak matematiksel kavramları kapsar. Hız ve Hızlandırma atayın ve kullanmayı gösterir ve nesneleri ve herhangi bir yönde taşıma vektörleri döndürmek alınmaktadır.
