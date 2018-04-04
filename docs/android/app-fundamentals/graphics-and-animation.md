---
title: Grafikler ve animasyon
description: Android 2B grafik ve animasyonları desteklemek için çok zengin ve farklı bir çerçeve sağlar. Bu konu, bu çerçeveleri tanıtır ve özel grafikler ve animasyonları kullanmak için bir Xamarin.Android uygulamasına nasıl oluşturulacağını açıklar.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 85a7cd2ac5094a4760c5465bd099ce2e3beeae79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="graphics-and-animation"></a>Grafikler ve animasyon

_Android 2B grafik ve animasyonları desteklemek için çok zengin ve farklı bir çerçeve sağlar. Bu konu, bu çerçeveleri tanıtır ve özel grafikler ve animasyonları kullanmak için bir Xamarin.Android uygulamasına nasıl oluşturulacağını açıklar._


## <a name="overview"></a>Genel Bakış

Geleneksel olarak sınırlı gücünü cihazlarda çalışan rağmen yüksek dereceli mobil uygulamalar genellikle bir gelişmiş kullanıcı deneyimi (UX) sahip yüksek kaliteli grafiklerle ve sezgisel, yanıt, dinamik kullanım sağlamak animasyonları tamamlandı. Mobil uygulamalar alma giderek daha gelişmiş bir gibi kullanıcıların uygulamalarından daha da fazla beklenir başlamıştır.

Luckily bize için modern mobil platformları kullanım kolaylığı korurken Gelişmiş animasyonları ve özel grafikler oluşturma için çok güçlü çerçeveleri vardır. Bu, çok az çaba ile zengin etkileşim eklemek için geliştiricilere sağlar.

Android içinde UI API çerçeve kabaca bölme iki kategoriye ayrılır: grafikler ve animasyon.

Grafik, daha fazla 2B ve 3B grafik yapmak için farklı yaklaşımlara olarak bölünür. 3B grafik OpenGL ES (mobil belirli bir sürümünü OpenGL) gibi çerçeveleri ve üçüncü taraf çerçeveleri MonoGame (XNA araç seti ile uyumlu bir platformlar arası araç) gibi yerleşik bir dizi kullanılabilir. 3B grafik, bu makalenin kapsamı içinde olmasa da, yerleşik 2B çizim teknikleri inceleyeceğiz.

Android 2B grafik oluşturmak için iki farklı API'nin sağlar. Üst düzey bildirim temelli bir yaklaşım ve diğer programlama düşük düzey API'si biridir:

-   **Drawable kaynakları** &ndash; bunlar XML dosyalarında çizim yönergeleri gömerek özel grafikler program aracılığıyla veya (genellikle daha) oluşturmak için kullanılır. Drawable kaynaklar genellikle, yönergeleri veya bir 2B grafik işlemek Android için eylemleri içeren XML dosyaları olarak tanımlanır. 

-   **Tuvale** &ndash; Bu, temel alınan bir bitmap üzerinde doğrudan çizim içerir, düşük düzeyli bir API'dir. Görüntülenenleri üzerinde çok ayrıntılı denetim sağlar. 

Bu 2B grafik teknikleri ek olarak, Android animasyon oluşturmak için birçok farklı yöntemle de sağlar:

-   **Drawable animasyonları** &ndash; Android de olarak bilinen kare kare animasyonları destekler *Drawable animasyon*. En basit animasyon API budur. Android sırayla yükler ve Drawable kaynakları dizisi (çok benzer bir çizgi) görüntüler.

-   **Görüntüleme animasyonları** &ndash; *görünüm animasyonları* Android özgün animasyonda API'nin ve Android tüm sürümlerinde kullanılabilir. Bu API, yalnızca görünüm nesneleri ile çalışır ve yalnızca bu görünümleri basit dönüştürmeleri gerçekleştirebilirsiniz sınırlıdır.
    Görünüm animasyonları genellikle bulunan XML dosyaları tanımlanmış `/Resources/anim` klasör.

-   **Özellik animasyonları** &ndash; Android 3.0 olarak bilinen yeni bir animasyon API'nin kümesi sunulan *özellik animasyonları*. Bu yeni API'nin herhangi bir nesnenin özelliklerini animasyon, yalnızca nesneleri görüntülemek için kullanılan bir genişletilebilir ve esnek sistemi kullanıma sunuldu. Bu esneklik, daha kolay paylaşım kodu oluşturan ayrı sınıflarda saklanmasını animasyonları sağlar.


Görünüm animasyonları daha eski öncesi Android 3.0 API'nin (API düzeyi 11) desteklemesi gereken uygulamalar için uygundur. Aksi takdirde uygulamaları yukarıda bahsedilen nedeniyle yeni özellik animasyon API'nin kullanmanız gerekir.

Bu çerçeveleri uygulanabilir tümü seçenekleri ile çalışmak için daha esnek bir API olduğu gibi ancak mümkün olduğunda tercih özelliği animasyonları verilmelidir. Özellik animasyonları daha kolay paylaşım kod yapar ve kod bakımı basitleştirir ayrı sınıflarda saklanmasını animasyon mantığı için izin verir.


## <a name="accessibility"></a>Erişilebilirlik

Grafikler ve animasyonları Yardım Android uygulamalarını çekici yapma ve eğlenceli kullanın; Ancak, bazı etkileşimler screenreaders, diğer giriş aygıtları aracılığıyla veya Yardımlı yakınlaştırma ile ortaya unutmamak önemlidir.
Ayrıca, bazı etkileşimler ses becerileri ortaya çıkabilir.

Uygulamaları yoksa bu durumlarda daha kullanışlı aklınızda erişilebilirlik ile üretildi: ipuçları ve kullanıcı arabiriminde gezinme Yardım sağlama ve metin içeriği veya UI resimsel öğelerin açıklamaları sağlama.

Başvurmak [Google'nın erişilebilirlik Kılavuzu](http://developer.android.com/guide/topics/ui/accessibility/) Android'ın erişilebilirlik API kullanma hakkında daha fazla bilgi için.



## <a name="2d-graphics"></a>2B grafik

Android uygulamalarında yaygın bir teknik drawable kaynaklardır. Diğer kaynaklarla Drawable kaynakları tanımlayıcı olarak &ndash; XML dosyalarında tanımlanan. Bu yaklaşım temiz bir kod ayrılması kaynaklardan sağlar. Güncelleştirmek veya bir Android uygulamasını grafikleri değiştirmek için kodunu değiştirmek gerekli olmadığından bu geliştirme ve Bakım kolaylaştırabilir. Drawable kaynakları pek çok basit ve ortak grafik gereksinimleri için yararlı olsa da, ancak bunlar güç ve denetim tuvale API yoksundur.

Diğer bir yöntem kullanarak [tuvale](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) nesne, System.Drawing ya da iOS'ın çekirdek çizim gibi geleneksel diğer API çerçeveler çok benzer. Tuvale nesnesi kullanarak nasıl 2B grafik en fazla kontrolü oluşturulur sağlar. Drawable kaynak çalışmaz veya çalışmak zor olduğu durumlar için uygundur. Örneğin, kaydırıcıyı değerine ilgili hesaplamaları temel görünümünü değiştirir özel kaydırıcı denetimi çizmek gerekli olabilir.

İlk Drawable kaynakları inceleyelim. Bunlar daha basit ve en sık karşılaşılan özel çizim durumlarda kapsar.


### <a name="drawable-resources"></a>Drawable kaynakları

Drawable kaynakları dizindeki bir XML dosyasında tanımlanmış `/Resources/drawable`. PNG veya JPEG'ın katıştırma aksine Drawable kaynakları yoğunluğu özgü sürümlerini sağlamak ise gerekli değildir.
Çalışma zamanında bir Android uygulaması bu kaynakları yükleyin ve 2B grafik oluşturmak için bu XML dosyalarında bulunan yönergeleri kullanın.
Android Drawable kaynakları birkaç farklı türde tanımlar:

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; bu basit bir geometrik şekil çizer ve bu şekil grafik etkileri sınırlı sayıda uygular, Drawable bir nesnedir. Bunlar düğmelerini özelleştirme veya TextViews arka planı ayarlama gibi şeyler için çok kullanışlıdır. Nasıl kullanılacağına ilişkin bir örnek göreceğiz bir `ShapeDrawable` bu makalenin ilerisinde yer.

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; görünümü bir pencere öğesi/denetim durumuna bağlı değiştirecek bir Drawable kaynak budur. Örneğin, bir düğme görünümünü olup, veya basıldığında bağlı olarak değişebilir.

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; bu Drawable birkaç diğer drawables bir başka üstünde gruplanır kaynağı. Örnek olarak bir *LayerDrawable* aşağıdaki ekran görüntüsünde gösterilen:

    ![LayerDrawable örneği](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; bu bir *LayerDrawable* ancak bir arasındaki fark. A *TransitionDrawable* başka bir üst gösteren bir katman animasyon yapabiliyor.

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; bu çok benzer bir *StateListDrawable* içeren belirli koşullara göre bir görüntü görüntülenir. Ancak, farklı bir *StateListDrawable*, *LevelListDrawable* görüntünün bir tamsayı değerine göre görüntüler. Örnek olarak bir *LevelListDrawable* WiFi sinyal gücünü görüntülenecek olacaktır. WiFi sinyal değişiklikleri gücünü görüntülenen drawable uygun şekilde değiştirir.

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; kendi adından da anlaşılacağı gibi bu Drawables ölçeklendirme hem işlevselliği kırpma sağlayın. *ScaleDrawable* başka bir Drawable, while ölçeklenir *ClipDrawable* başka bir Drawable küçük.

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; bu Drawable geçerli iç metinleri başka bir Drawable kaynak tarafında. Bir görünümü görünümün gerçek sınırları küçük olan bir arka plan gerektiğinde kullanılır.

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; bu dosya üzerinde gerçek bir bit eşlem gerçekleştirilecek olan yönergeleri, XML, kümesidir. Android gerçekleştirebileceğiniz bazı eylemler döşemesini, titreme ve düzgünleştirme. Bu çok yaygın kullanımları bir düzen arka plan arasında bir bit eşlem döşeme biridir.


#### <a name="drawable-example"></a>Drawable örneği

Bir 2B grafik kullanarak oluşturmak nasıl hızlı bir örneğe bakalım bir `ShapeDrawable`. A `ShapeDrawable` dört temel şekillerin tanımlayabilirsiniz: dikdörtgen, oval, satır ve halkası. Gradyan, renk ve boyutu gibi temel efektler uygulamak mümkündür. Aşağıdaki XML bir `ShapeDrawable` içinde bulunabilir *AnimationsDemo* yardımcı proje (dosyasındaki `Resources/drawable/shape_rounded_blue_rect.xml`).
Dikdörtgene mor gradyan arka planı ile tanımlar ve yuvarlanmış köşeleri:

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
<!-- Specify a gradient for the background -->
<gradient android:angle="45"
          android:startColor="#55000066"
          android:centerColor="#00000000"
          android:endColor="#00000000"
          android:centerX="0.75" />

<padding android:left="5dp"
          android:right="5dp"
          android:top="5dp"
          android:bottom="5dp" />

<corners android:topLeftRadius="10dp"
          android:topRightRadius="10dp"
          android:bottomLeftRadius="10dp"
          android:bottomRightRadius="10dp" />
</shape>
```

Biz bu Drawable kaynağında bildirimli olarak bir düzeni veya aşağıdaki XML dosyasında gösterildiği gibi diğer Drawable başvurabilir:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#33000000">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_centerInParent="true"
              android:background="@drawable/shape_rounded_blue_rect"
              android:text="@string/message_shapedrawable" />
</RelativeLayout>
```

Drawable kaynakları de bir program aracılığıyla uygulanabilir. Aşağıdaki kod parçacığını bir kutusu TextView arka plan program aracılığıyla ayarlamak nasıl gösterir:

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Bu göründüğünü görmek için çalıştırın *AnimationsDemo* proje ve ana menüden şekli Drawable öğeyi seçin. Biz, aşağıdaki ekran görüntüsüne benzer bir şey görmeniz gerekir:

![Özel arka plan gradyan ve Yuvarlatılmış köşeleri ile drawable ile kutusu TextView](graphics-and-animation-images/image1.png)

XML öğeleri ve Drawable kaynakların sözdizimi hakkında daha fazla ayrıntı için başvurun [Google belgelerine](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).


### <a name="using-the-canvas-drawing-api"></a>Tuvale çizim API'yi kullanma

Drawables güçlüdür ancak bunların sınırlamaları vardır. Belirli konular mümkün değil ya da çok karmaşık (örneğin: cihazdaki kameranın tarafından gerçekleştirilecek bir resmi bir filtre uygulamak). Drawable kaynak kullanarak kırmızı göz azaltma uygulamak oldukça zor olacaktır.
Bunun yerine, tuvalin API seçmeli olarak resmin belirli bir parçasını renkleri değiştirmek için çok hassas bir denetim bir uygulama sağlar.

Tuvale ile sık kullanılan bir sınıftır [boyama](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) sınıfı. Bu sınıf nasıl çizileceğini renkli ve stil bilgilerini içerir. Bu tür bir renk ve saydamlık şeyler sağlamak için kullanılır.

Tuvale API'sini kullanan *ressamların modeli* 2B grafik çizmek için.
İşlemleri birbirinin art arda katmanlarında uygulanır. Her işlem, temel alınan bit eşlem bazı kısımları ele alınacaktır. Alan bir önceden boyanmış alanı çakışıyor olduğunda, yeni boyama kısmen olur veya tamamen eski soyutlamaması. Diğer çizim birçok API'leri System.Drawing ve iOS'ın çekirdek grafikleri gibi iş aynı yoldur.

İki yolla elde etmek için bir `Canvas` nesnesi. İlk yol tanımlama içerir bir [bit eşlem](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) nesnesi ve ardından başlatmasını bir `Canvas` onunla nesnesi. Örneğin, aşağıdaki kod parçacığını Yeni tuvali ile temel bir bit eşlem oluşturur:

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Elde etmek için diğer bir yol bir `Canvas` nesnesidir tarafından [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) sağlanan geri çağırma yöntemi [Görünüm](https://developer.xamarin.com/api/type/Android.Views.View/) temel sınıfı. Android, bir görünüm kendisini çizme gerekir ve içinde geçirir kapılarını açtığında bu yöntemi çağırır bir `Canvas` çalışmak görünüm için nesne.

Tuvale sınıfı program aracılığıyla çizim yönergeler sağlamak için yöntemleri sunar. Örneğin:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; tüm tuval bit eşlem belirtilen paint ile doldurur.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; belirtilen paint ile belirtilen geometrik şekil çizer.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; belirtilen renk ile tuval üzerinde bir metin çizer. Metin konumda çizilir `x,y` .



#### <a name="drawing-with-the-canvas-api"></a>API tuvali ile çizme

Eylem tuvale API örneği görelim. Aşağıdaki kod parçacığını nasıl bir görünüm çizileceğini gösterir:

```csharp
public class MyView : View
{
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        Paint green = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0x99, 0xcc, 0),
        };
        green.SetStyle(Paint.Style.FillAndStroke);

        Paint red = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0xff, 0x44, 0x44)
        };
        red.SetStyle(Paint.Style.FillAndStroke);

        float middle = canvas.Width * 0.25f;
        canvas.DrawPaint(red);
        canvas.DrawRect(0, 0, middle, canvas.Height, green);
    }
}
```

Bu kodu Yukarıdaki ilk kırmızı boyama ve yeşil boyama nesnesi oluşturur. Tuvale içeriğini kırmızı ile doldurur ve tuvalin genişliği % 25'tir yeşil bir dikdörtgen çizmek için tuvale talimatı verir. Bunun bir örneği tarafından görülebilir `AnimationsDemo` Bu makale için kaynak kodu ile birlikte proje. Uygulamasını başlatıp çizim öğesi ana menüden seçerek biz aşağıdakine benzer bir ekran gerekir:

![Ekran kırmızı boyama ve yeşil boyama nesneleri ile](graphics-and-animation-images/image3.png)


## <a name="animation"></a>Animasyon

Kullanıcıların kendi uygulamalarını taşımak şeyler ister. Animasyon, bir uygulamanın kullanıcı deneyimini geliştirmek ve göze yardımcı olmak için harika bir yoldur. En iyi animasyonları doğal eşitleyerek olduğundan kullanıcıları fark yoktur olanlardır. Android aşağıdaki üç API animasyonlarını sağlar:

-   **Animasyon görüntülemek** &ndash; özgün API budur. Animasyonlarına belirli bir görünüme bağlıdır ve görünüm içeriğine basit dönüştürmeleri gerçekleştirebilirsiniz. Kolaylık olması nedeniyle, işlemler için hala yararlı bu API alfa animasyonları, döndürme ve diğerleri gibi.

-   **Özellik animasyon** &ndash; özellik animasyonları Android 3. 0'sunulmuştur. Bunlar, neredeyse her şeyi animasyon uygulamak bir uygulama sağlar. Bu nesne ekranda görünür değilse bile özellik animasyonları herhangi bir nesnenin herhangi bir özelliği değiştirmek için kullanılabilir.

-   **Drawable animasyon** &ndash; bu çok basit bir animasyon uygulamak için kullanılan özel bir Drawable kaynak etkilemek için düzenler.

Genel olarak, özellik animasyon daha esnektir ve daha fazla özellik sunar olarak kullanmak için tercih edilen sistemidir.


### <a name="view-animations"></a>Görünüm animasyonları

Görünüm animasyonları görünümle sınırlıdır ve yalnızca animasyon başlangıç ve bitiş noktalarını, boyutu, döndürme ve saydamlık gibi değerler üzerinde gerçekleştirebilirsiniz.
Bu türdeki animasyonları, tipik olarak adlandırılır *Ara animasyonları*. Görünüm animasyonları iki şekilde tanımlanabilir &ndash; program aracılığıyla kodda veya XML dosyaları kullanarak. Bunlar daha okunabilir ve sürdürmek daha kolay olduğu XML dosyaları görünüm animasyonları bildirmek için tercih edilen yoludur.

Animasyon XML dosyaları içinde depolanan `/Resources/anim` bir Xamarin.Android projesi dizininde. Bu dosya kök öğesi olarak aşağıdaki öğeleri birine sahip olmalıdır:

-   `alpha` &ndash; Soluklaşmasını veya fade-out animasyon.

-   `rotate` &ndash; Bir döndürme animasyon.

-   `scale` &ndash; Yeniden boyutlandırma animasyon.

-   `translate` &ndash; Yatay ve/veya dikey hareket.

-   `set` &ndash; Bir veya daha fazla diğer animasyon öğelerini tutan kapsayıcı.

Varsayılan olarak, bir XML dosyasındaki tüm animasyonları eşzamanlı olarak uygulanır. Sıralı olarak ortaya animasyonları yapmak için ayarlanmış `android:startOffset` yukarıda tanımlanan öğelerden birini özniteliği.

Kullanarak bir animasyon içinde değişimin hızını etkileyen mümkündür bir *bir Ara*. Bir ara animasyon efektleri hızlandırılmış, yinelenen veya decelerated mümkün kılar. Android framework (ancak bunlarla sınırlı değil) gibi çeşitli ınterpolators kutusunu dışında sağlar:

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; Bu ınterpolators artırın veya animasyonda değişme oranı azaltın.

-   `BounceInterpolator` &ndash; değişiklik sonunda geri dönmeler.

-   `LinearInterpolator` &ndash; değişiklik hızı sabittir.


Aşağıdaki XML bu öğelerin bazıları birleştiren bir animasyon dosyası örneği gösterilmektedir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android=http://schemas.android.com/apk/res/android
     android:shareInterpolator="false">

    <scale android:interpolator="@android:anim/accelerate_decelerate_interpolator"
           android:fromXScale="1.0"
           android:toXScale="1.4"
           android:fromYScale="1.0"
           android:toYScale="0.6"
           android:pivotX="50%"
           android:pivotY="50%"
           android:fillEnabled="true"
           android:fillAfter="false"
           android:duration="700" />

    <set android:interpolator="@android:anim/accelerate_interpolator">
        <scale android:fromXScale="1.4"
               android:toXScale="0.0"
               android:fromYScale="0.6"
               android:toYScale="0.0"
               android:pivotX="50%"
               android:pivotY="50%"
               android:fillEnabled="true"
               android:fillBefore="false"
               android:fillAfter="true"
               android:startOffset="700"
               android:duration="400" />

        <rotate android:fromDegrees="0"
                android:toDegrees="-45"
                android:toYScale="0.0"
                android:pivotX="50%"
                android:pivotY="50%"
                android:fillEnabled="true"
                android:fillBefore="false"
                android:fillAfter="true"
                android:startOffset="700"
                android:duration="400" />
    </set>
</set>
```

Bu animasyon animasyonları tümünün aynı anda gerçekleştirir. İlk ölçek animasyon resmi yatay olarak uzatma ve dikey olarak küçültmek ve sonra görüntüyü aynı anda Döndürülmüş 45 dereceyi yönünün ve olması Küçült, ekranından kayboluyor.

Animasyonun animasyon inflating ve bir görünüm için uygulama tarafından bir görünüme program aracılığıyla uygulanabilir. Android yardımcı sınıfı sağlar `Android.Views.Animations.AnimationUtils` , bir animasyon kaynak Şişir ve bir örneğini döndürmek `Android.Views.Animations.Animation`. Bu nesne için bir görünüm çağırarak uygulanan `StartAnimation` ve geçirme `Animation` nesnesi. Aşağıdaki kod parçacığını Bu örneği gösterilmektedir:

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Biz nasıl görünüm animasyonları işe temel bilgiye sahip olduğunuza göre özellik animasyonları taşıma sağlar.


### <a name="property-animations"></a>Özellik animasyonları

Özellik animatörleri Android 3. 0'sunulan yeni bir API ' dir.
Bunlar, herhangi bir nesne üzerinde herhangi bir özelliği animasyon uygulamak için kullanılan daha genişletilebilir bir API sağlar.

Tüm özellik animasyonları örnekleri tarafından oluşturulan [Animator'da](https://developer.xamarin.com/api/type/Android.Animation.Animator/) bir alt kümesi. Uygulamalar bu sınıf doğrudan kullanmaz, bunun yerine, buna ait alt sınıflarından birini kullanın:

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; bu sınıfı tüm özellik animasyonda API en önemli sınıftır. Değiştirilmesi gereken özelliklerinin değerlerini hesaplar. `ViewAnimator` Doğrudan bu değerleri; güncelleştirmez bunun yerine, animasyonlu nesneleri güncelleştirmek için kullanılan olayları başlatır.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; Bu sınıf sınıfıdır `ValueAnimator` . Hedef nesne ve güncelleştirmek için özellik kabul ederek hareketlendirme nesnelerin işlemini kolaylaştırmak için tasarlanmıştır.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; Bu sınıf animasyonları ilişkisinde birbirine çalışma şeklini yönetme için sorumludur. Animasyon aynı anda, sıralı olarak veya belirtilen gecikme ile aralarında çalıştırabilirsiniz.


*Değerlendiricisi* tarafından animatörleri sırasında bir animasyon yeni değerleri hesaplamak için kullanılan özel sınıflarıdır. Kutudan çıktığında, Android aşağıdaki değerlendiricisi sağlar:

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; tamsayı özelliklerinin değerlerini hesaplar.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; float özelliklerinin değerlerini hesaplar.

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; renkli özelliklerinin değerlerini hesaplar.

Animasyon eklenir özellik değilse, bir `float`, `int` veya renkli, uygulamalar oluşturabilir, kendi değerlendirici uygulayarak `ITypeEvaluator` arabirimi. (Özel değerlendiricisi uygulama bu konunun kapsamı dışındadır olur.)

#### <a name="using-the-valueanimator"></a>ValueAnimator kullanma

Hiçbir animasyon iki bölümü vardır: animasyonlu değerleri hesaplama ve bazı nesne üzerindeki özellikleri bu değerleri ayarlama. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) değerleri yalnızca hesaplar, ancak nesneler üzerinde doğrudan çalışmaz. Bunun yerine, nesneleri animasyon kullanım ömrü sırasında çağrılacak olay işleyicileri içinde güncelleştirilecek. Bu tasarım, bir animasyonlu değerden güncelleştirilmesi birkaç özellikleri sağlar.

Bir örneği elde `ValueAnimator` aşağıdaki Fabrika yöntemlerden birini çağırarak:

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

Bunu yaptıktan, `ValueAnimator` örneği olmalıdır süresinin ayarlayın ve ardından başlatılabilir. Aşağıdaki örnek, bir değeri 0'dan 1 1000 milisaniye cinsinden aralığı animasyon gösterilmektedir:

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Ancak kendisi, yukarıdaki kod parçacığında çok kullanışlı değildir &ndash; Animator'da çalışır ancak hiçbir hedef güncelleştirilmiş değeri yok. `Animator` Sınıfının yeni bir değer dinleyicileri hakkında bilgilendirmek gerekli olduğuna karar güncelleştirme olay Yükselt. Uygulamaları aşağıdaki kod parçacığında gösterildiği gibi bu olaya yanıt için bir olay işleyicisi sağlayabilir:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

animator.Update += (object sender, ValueAnimator.AnimatorUpdateEventArgs e) =>
{
    int newValue = (int) e.Animation.AnimatedValue;
    // Apply this new value to the object being animated.
    myObj.SomeIntegerValue = newValue;
};
```

Biz bir anlayış sahip olduğunuza `ValueAnimator`, hakkında daha fazla bilgi sağlayan `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>ObjectAnimator kullanma

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) sınıfıdır `ViewAnimator` zamanlama altyapısı ve değer hesaplaması birleştirir `ValueAnimator` olay işleyicileri wire için gerekli mantığı ile. `ValueAnimator` Açıkça bir olay işleyicisini kablo applications gerektirir &ndash; `ObjectAnimator` bu adımında bize ilgilenebilmek.

API için `ObjectAnimator` API'si için çok benzer `ViewAnimator`, ancak nesne ve güncelleştirmek için özellik adını sağlamanızı gerektirir. Aşağıdaki örnek, kullanma örneği gösterir `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Önceki kod parçacığını gördüğünüz `ObjectAnimator` azaltabilir ve nesneyi animasyon için gerekli olan kodu basitleştirebilir.


### <a name="drawable-animations"></a>Drawable animasyonları

Son animasyon API Drawable animasyon API'dir. Drawable animasyonları Drawable kaynakları tek bir dizi art arda yükleme ve bunları sırayla, görüntüleme Çevir BT çizgi benzer.

Drawable kaynaklara sahip bir XML dosyasında tanımlanmış bir `<animation-list>` öğesi kök öğe ve bir dizi olarak `<item>` her çerçeve animasyon tanımlayan öğeleri. Bu XML dosyası depolanan `/Resource/drawable` uygulamanın klasör. Aşağıdaki XML drawable animasyonun örneğidir:

```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@drawable/asteroid01" android:duration="100" />
  <item android:drawable="@drawable/asteroid02" android:duration="100" />
  <item android:drawable="@drawable/asteroid03" android:duration="100" />
  <item android:drawable="@drawable/asteroid04" android:duration="100" />
  <item android:drawable="@drawable/asteroid05" android:duration="100" />
  <item android:drawable="@drawable/asteroid06" android:duration="100" />
</animation-list>
```

Bu animasyon altı çerçeveler boyunca çalışır. `android:duration` Öznitelik bildirir her çerçeve ne kadar süreyle görüntülenir. Sonraki kod parçacığını Drawable animasyon oluşturma ve kullanıcı ekranında düğmesini tıklattığında başlayarak bir örnek gösterilmektedir:

```csharp
AnimationDrawable _asteroidDrawable;

protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    _asteroidDrawable = (Android.Graphics.Drawables.AnimationDrawable)
    Resources.GetDrawable(Resource.Drawable.spinning_asteroid);

    ImageView asteroidImage = FindViewById<ImageView>(Resource.Id.imageView2);
    asteroidImage.SetImageDrawable((Android.Graphics.Drawables.Drawable) _asteroidDrawable);

    Button asteroidButton = FindViewById<Button>(Resource.Id.spinAsteroid);
    asteroidButton.Click += (sender, e) =>
    {
        _asteroidDrawable.Start();
    };
}
```

Bu noktada biz API'leri Android bir uygulamada kullanılabilir animasyon temelleri kapsamına.


## <a name="summary"></a>Özet

Bu makalede sunulan çok sayıda yeni kavramlar ve bazı grafik Android uygulama eklemek yardımcı olmak için API kullanıcının. İlk çeşitli 2B grafik API'SİNİN ele alınan ve Android doğrudan bir tuval nesnesi kullanarak ekranına çizmek uygulamaların nasıl sağlayan gösterilmektedir. Ayrıca bildirimli olarak XML dosyaları kullanılarak oluşturulması grafik izin bazı alternatif teknikleri gördük. Sonra şu Android animasyonları oluşturmak için eski ve yeni API tartışmak için geçti.



## <a name="related-links"></a>İlgili bağlantılar

- [Animasyon Demo (örnek)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [Animasyon ve grafikler](http://developer.android.com/guide/topics/graphics/index.html)
- [Animasyon kullanarak mobil uygulamalarınız hayata geçirin](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Tuval](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [Nesne Animator'da](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [Değer Animator'da](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
