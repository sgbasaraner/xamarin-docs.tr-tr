---
title: İle CCAction animasyon ekleme
description: CCAction sınıfını eklemeyi animasyonları CocosSharp oyunlara basitleştirir. Animasyonlarına işlevselliğini uygulayan veya Lehçe eklemek için kullanılabilir.
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: b6209816f741423f40945a0fe4391fe921cb35de
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="animating-with-ccaction"></a>İle CCAction animasyon ekleme

_CCAction sınıfını eklemeyi animasyonları CocosSharp oyunlara basitleştirir. Animasyonlarına işlevselliğini uygulayan veya Lehçe eklemek için kullanılabilir._

`CCAction` CocosSharp nesnelere animasyon ekleme için kullanılan bir temel sınıftır. Bu kılavuz yerleşik kapsar `CCAction` konumlandırma, ölçeklendirme ve döndürme gibi ortak görevler için uygulamaları. Ayrıca devralarak özel uygulamalar oluşturmak nasıl göründüğünü `CCAction`.

Bu Kılavuzu adlı bir proje kullanır **ActionProject** hangi [burada indirilebilir](https://developer.xamarin.com/samples/mobile/CCAction). Bu kılavuzu kullanır `CCDrawNode` içinde ele sınıfı [çizim geometri CCDrawNode ile](~/graphics-games/cocossharp/ccdrawnode.md) Kılavuzu.


## <a name="running-the-actionproject"></a>ActionProject çalıştırma

**ActionProject** iOS ve Android için yerleşik bir CocosSharp çözümüdür. Hem nasıl kullanılacağına ilişkin bir kod örneği olarak hizmet veren `CCAction` sınıfı ve gerçek zamanlı demo yaygın olarak `CCAction` uygulamaları.

Çalışırken, ActionProject üç görüntüler `CCLabel` sol ekran ve örnekleri iki tarafından çizilmiş görsel bir nesne `CCDrawNode` çeşitli eylemler görüntüleme örnekleri:

![](ccaction-images/image1.png "ActionProject ekran ve çeşitli eylemler görüntülemek için iki CCDrawNode örnekleri tarafından çizilmiş görsel bir nesnenin solundaki üç CCLabel örneğini gösterir.")

Hangi tür soldaki etiketleri gösterir `CCAction` ekranda dokunarak oluşturulur. Varsayılan olarak, **konumu** değeri seçildiğinde, sonuçta bir `CCMove` eylem oluşturulan ve kırmızı daire uygulanır:

![](ccaction-images/image2.gif "Oluşturulan ve kırmızı daire uygulanan CCMove olan eylem kaynaklanan konum değeri seçilir")

Sol taraftaki etiketleri tıklayarak değişiklikleri hangi tür `CCAction` daireye gerçekleştirilir. Örneğin, tıklatarak **konumu** etiket değiştirilebilmesi için farklı değerler döngüsü:

![](ccaction-images/image3.gif "Konum etiketinin tıklatarak değiştirilebilmesi için farklı değerler döngüsü")

## <a name="common-variable-changing-ccaction-classes"></a>Ortak değişken değiştirme CCAction sınıfları

**ActionProject** aşağıdaki kullanan `CCAction`-CocosSharp parçası olan sınıflarını devralıp:

 - `CCMoveTo` – Değişiklikleri bir `CCNode` örneğinin `Position` özelliği
 - `CCScaleTo` – Değişiklikleri bir `CCNode` örneğinin `Scale` özelliği
 - `CCRotateTo` – Değişiklikleri bir `CCNode` örneğinin `Rotation` özelliği

Bu eylem olarak bu kılavuzu başvurduğu *değişkeni değiştirme*, başka bir deyişle, bunlar doğrudan değişkeninin etkiler `CCNode` için eklenen. Diğer tür eylemlerin denir *kolaylaştırma* Eylemler, bu kılavuzda ele alınmıştır.

**ActionProject** zaman içinde bir değişken değiştirmek için bu eylemleri amacı olduğunu gösterir. Özellikle, bu `CCActions` oluşturucular ele iki bağımsız değişken: olabilmesi için saat ve atanacak değer uzunluğu. Aşağıdaki kod parçası, Eylemler üç tür nasıl oluşturulacağını gösterir:


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

Eylem oluşturulduktan sonra bir CCNode aşağıdaki gibi ayarlayın:


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` başlatır `CCAction` örneğin davranışını ve otomatik olarak gerçekleştirecek mantığı çerçeve-sonra-çerçevesini tamamlanana kadar.

Word ile biter yukarıda listelenen türlerinin her biri *için* anlamına `CCAction` değiştirecek `CCNode` böylece eylemi tamamladığında bağımsız değişken değeri son durumunu temsil eder. Örneğin, oluşturma bir `CCMoveTo` X bir konumunu 100 ve Y = 200 sonuçlarında = `CCNode` örneğinin `Position` X ayarlanan = 100, Y belirtilen, ne olursa olsun, süre sonunda 200 = `CCNode` örneği başlangıç konumu.

Her "İçin" sınıfı ayrıca, bağımsız değişken değeri geçerli değerine üzerinde eklemek bir "İle" sürümüne sahip `CCNode`. Örneğin, oluşturma bir `CCMoveBy` X bir konumunu = 100 ve Y = 200 neden olur `CCNode` sağ 100 birimlerine ve 200 birimleri olan eylemi başlatıldığı adresindeki konumdan taşınan örneği.


## <a name="easing-actions"></a>Eylemler kolaylaştırma

Varsayılan olarak, değişken değiştirme eylemleri gerçekleştirecek *doğrusal ilişkilendirme* – eylem sabit bir oranda istenen değeri doğru hareket eder. Enterpolasyonla varsa *konumu* doğrusal olarak, taşıma nesne hemen başlatmak ve eylem başında ve sonunda taşımayı durdurun ve eylemi yürüten gibi kendi hızı sabit kalır. 

Olmayan doğrusal ilişkilendirme daha az jarring ve CocosSharp değişkeni değiştirme eylemleri değiştirmek için kullanılan eylemler kolaylaştırma bir çeşitli sağlamadığından, Lehçe, bir öğe ekler.

İçinde **ActionProject** örnek, biz geçirebilirsiniz bu ikinci etikette tıklayarak hareket hızı eylem türleri arasında (varsayılan olarak **<None>**):

![](ccaction-images/image4.gif "Kullanıcı bu ikinci etikette tıklayarak hareket hızı eylem türleri arasında geçiş yapabilirsiniz")

Hareket hızı Eylemler özellikle güçlü olduklarından herhangi belirli değişken ayarı eyleme bağlı değil. Başka bir deyişle, aynı hareket hızı eylemi (Bu kılavuzda gösterilecek gibi) konumunu, döndürme, Ölçek ya da özel eylemler atamak için kullanılabilir.

Hareket hızı Eylemler sarmalama değişken ayarı eylemleri (değişken ayar eylemi devraldığı sürece `CCFiniteTimeAction`) bir değişken ayar eylemi kendi oluşturucular bir bağımsız değişken olarak kabul tarafından.

Örneğin, etiketleri ayarlandıysa **konumu**, **CCEaseElastic**, aşağıdaki kod bir touch algılandığında yürütecek sonra (kod ilgili satırlara vurgulamak için atlanmış unutmayın):


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

Uygulama tarafından gösterildiği gibi tam aynı kolaylaştırma diğer değişken ayarı eylemler gibi uygulanabilir `CCRotateTo`:

![](ccaction-images/image5.gif "Tam aynı kolaylaştırma CCRotateTo gibi diğer değişken ayarı eylemleri uygulanabilir")


## <a name="easing-in-out-and-inout"></a>In, Out ve Inout kolaylaştırma

Tüm hareket hızı Eylemler durumunda `In`, `Out`, veya `InOut` hareket hızı türü eklenir. Bu koşulları kolaylaştırma uygulandığında bakın: `In` kolaylaştırma başında uygulanacak anlamına gelir `Out` sonunda anlamına gelir ve `InOut` hem başlangıç ve bitiş anlamına gelir.

Bir `In` eylem kolaylaştırma bir değişkeni, tüm ilişkilendirme (her ikisi de başında ve sonunda) boyunca uygulandığı şekilde etkileyecek, ancak genellikle hareket hızı eylem en tanınabilir özelliklerini başında gerçekleşir. Benzer şekilde, `Out` hareket hızı Eylemler davranışlarını ilişkilendirme sonunda tarafından işlemleri. Örneğin, `CCEaseBounceOut` bir nesne eylem sonunda geçirmek neden olur.


### <a name="out"></a>Out

`Out` genellikle kolaylaştırma ilişkilendirme sonunda en dikkat çeken değişiklikleri uygular. Örneğin, `CCEaseExponentialOut` hedef değer yaklaştığında değiştirme değişkeni değişikliği hızı yavaş:

![](ccaction-images/image6.gif "Hedef değer yaklaştığında CCEaseExponentialOut değiştirme değişkeni değişikliği hızı yavaş")


### <a name="in"></a>İçindeki

`In` genellikle kolaylaştırma ilişkilendirme başındaki en dikkat çeken değişikliğini uygular. Örneğin, `CCEaseExponentialIn` eylemi başında daha yavaş taşır:

![](ccaction-images/image7.gif "CCEaseExponentialIn daha yavaş eylemi başında taşır")


### <a name="inout"></a>Inout

`InOut` genellikle hem başlangıç ve bitiş en dikkat çeken değişiklikleri uygular. `InOut` kolaylaştırma genellikle simetrik. Örneğin, `CCEaseExponentialInOut` eylemi başında ve sonunda yavaş taşır:

![](ccaction-images/image8.gif "CCEaseExponentialInOut eylemi başında ve sonunda yavaş taşıma")


## <a name="implementing-a-custom-ccaction"></a>Özel CCAction uygulama

Biz kadarki ele sınıfların tümü ortak işlevsellik sağlamak için CocosSharp içinde yer almaktadır. Özel `CCAction` uygulamaları ek esneklik sağlayabilir. Örneğin, bir `CCAction` böylece kullanıcı deneyimi kazandığı, deneyimi çubuğu düzgün bir şekilde büyür bir deneyim çubuğu doldurulmuş oranını denetleyen kullanılabilir.

`CCAction` uygulamaları, genellikle iki sınıf gerektirir:

 - `CCFiniteTimeAction` Uygulama – eylemi başlatmak için sınırlı zamanı eylem sınıfı sorumludur. Örneği sınıf olduğunu ve ya da doğrudan eklenen bir `CCNode` veya hareket hızı eylem. Dönüş örneği ve gereken bir `CCFiniteTimeActionState`, güncelleştirmeleri gerçekleştirir.
 - `CCFiniteTimeActionState` Uygulama – eylemde söz konusu değişkenleri güncelleştirmek için sınırlı zamanı eylem durumu sınıfı sorumludur. Bir saat değeri göre hedefte değer atayan bir güncelleştirme işlevi uygulamalıdır. Bu sınıf dışında açıkça başvurulmuyor `CCFiniteTimeAction` onu oluşturan. Yalnızca "arka planda" de çalışır.

**ActionProject** özel sağlar `CCFiniteTimeAction` adlı uygulama `LineWidthAction,` üstünde kırmızı daire sarı çizgi genişliğini ayarlamak için kullanılır. Örneği ve dönmek için yalnızca işini olduğuna dikkat edin bir `LineWidthState` örneği:


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

Yukarıda belirtildiği gibi `LineWidthState` satırın atama çalışır `Width` özelliğine göre ne kadar `time` geçti:


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

LineWidthAction aşağıdaki animasyonda gösterildiği gibi çeşitli şekillerde çizgi genişliğini değiştirmek için hareket hızı bir işlem ile birleştirilebilir:

![](ccaction-images/image9.gif "LineWidthAction çeşitli şekillerde çizgi genişliğini değiştirmek için herhangi bir hareket hızı eylem içeren bu animasyonda gösterildiği gibi birleştirilebilir")


### <a name="interpolation-and-the-update-method"></a>İlişkilendirme ve güncelleştirme yöntemi

Yukarıdaki sınıflarda değerlerini depolama yanı sıra yalnızca mantığı yaşadığı `LineWidthState.Update` yöntemi. `startWidth` Değişkeni depolar hedef genişliğini `LineNode` eylem başlangıcında ve `deltaWidth` değişkeni depolar eylemi süresince ne kadar değerini değiştirir.

Değiştirerek tarafından `time` değişken değeri 0 olan, görebiliriz hedef `LineNode` başlangıç konumunda olacaktır:


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

Benzer şekilde, görebiliriz hedef `LineNode` hedefine saat değişkeni 1 değerini getirilmesiyle olacaktır:


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

`time` Değeri 0 ile 1 - arasında genellikle olacaktır, ancak her zaman - ve `Update` uygulamalarında bu sınırlar değil varsayın. Hareket hızı bazı yöntemler (gibi `CCEaseBackIn` ve `CCEaseBackOut`) bir saat değeri 0'dan 1 aralığın dışında sağlayacaktır.


## <a name="conclusion"></a>Sonuç

İlişkilendirme ve kolaylaştırma özellikle kullanıcı arabirimleri oluştururken zarif bir oyun oluşturma önemli bir parçasıdır. Bu kılavuz kullanmayı kapsar `CCActions` konumu ve döndürme gibi standart değerler ve aynı zamanda özel değerler yanı. `LineWidthState` Ve `LineWidthAction` sınıfları özel bir eylemi uygulamak nasıl gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [Tam örnek](https://developer.xamarin.com/samples/mobile/CCAction/)
