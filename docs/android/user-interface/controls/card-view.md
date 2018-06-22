---
title: Kart görünümü
description: Kart görünümü pencere öğesi kartları benzer görünümlerde metin ve resim içerik sunan UI bir bileşendir. Bu kılavuz, Android önceki sürümleriyle geriye dönük uyumluluk korurken Xamarin.Android uygulamalarda kart görünümü özelleştirmek ve kullanmak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 21e2a2e8ef04936664344cb4fb758bc2af3b4d05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771840"
---
# <a name="cardview"></a>Kart görünümü

_Kart görünümü pencere öğesi kartları benzer görünümlerde metin ve resim içerik sunan UI bir bileşendir. Bu kılavuz, Android önceki sürümleriyle geriye dönük uyumluluk korurken Xamarin.Android uygulamalarda kart görünümü özelleştirmek ve kullanmak açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

`Cardview` Pencere, Android 5.0 (Lolipop) sunulan kartları benzer görünümlerde metin ve görüntü içeriği sunan bir bileşenidir UI. `CardView` olarak uygulanan bir `FrameLayout` Yuvarlatılmış köşeleri ve gölge ile pencere öğesi. Genellikle, bir `CardView` tek satır öğesinde sunmak için kullanılan bir `ListView` veya `GridView` Görünüm Grup. Örneğin, aşağıdaki ekran görüntüsünde uygulayan bir seyahat ayırma uygulama örneğidir `CardView`-seyahat hedef kartları kaydırılabilir bir temel `ListView`:

![Her seyahat hedefi için bir kart görünümü kullanarak örnek uygulama](card-view-images/01-cardview-example.png)

Bu kılavuz nasıl ekleneceğini açıklar `CardView` , Xamarin.Android pakete proje, nasıl ekleneceği `CardView` düzeninizi ve görünümünü özelleştirmek nasıl `CardView` uygulamanızda. Ayrıca, bu kılavuzda ayrıntılı bir listesini sağlar `CardView` , kullanmanıza yardımcı olması için öznitelikler dahil olmak üzere, değiştirebilirsiniz öznitelikleri `CardView` Android Android 5.0 Lolipop'den önceki sürümlerinde.

<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Aşağıdaki yeni Android 5.0 ve sonraki özellikleri kullanmak için gereklidir (de dahil olmak üzere `CardView`) Xamarin tabanlı uygulamalarda:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  **Android SDK** &ndash; Android 5.0 (API 21) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.

-  **Java JDK 1.8** &ndash; JDK 1.7 özellikle atamak API düzey 23 ve önceki varsa kullanılabilir. JDK 1.8 edinilebilir [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Uygulamanızı de dahil etmelisiniz `Xamarin.Android.Support.v7.CardView` paket. Eklenecek `Xamarin.Android.Support.v7.CardView` Mac için Visual Studio paketi:

1. Projenizi açın, sağ **paketleri** seçip **paketleri Ekle**.

2. İçinde **paketleri Ekle** iletişim, arama **kart görünümü**.

3. Seçin **Xamarin destek kitaplığı v7 kart görünümü**, ardından **Paketi Ekle**.

Eklenecek `Xamarin.Android.Support.v7.CardView` Visual Studio'da paket:

1. Projenizi açın, sağ tıklatın **başvuruları** düğümü (içinde **Çözüm Gezgini** bölmesi) seçip **NuGet paketlerini Yönet...** .

2. Zaman **NuGet paketlerini Yönet** iletişim kutusu gösterilir, girin **kart görünümü** arama kutusuna.

3. Zaman **Xamarin destek kitaplığı v7 kart görünümü** görünen tıklatın **yükleme**.

Bir Android 5.0 uygulama projesi yapılandırma konusunda bilgi edinmek için [ayarı oluşturan bir Android 5.0 proje](~/android/platform/lollipop.md).
NuGet paketlerini yükleme hakkında daha fazla bilgi için bkz: [izlenecek yol: de dahil olmak üzere bir NuGet projenizdeki](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


## <a name="introducing-cardview"></a>Kart görünümü Tanıtımı

Varsayılan `CardView` beyaz kartla en düşük düzeyde Yuvarlatılmış köşeleri ve hafif bir gölge benzer. Aşağıdaki örnek **Main.axml** düzenini görüntüleyen tek bir `CardView` içeren pencere öğesi bir `TextView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Varolan içeriğini değiştirmek için bu XML kullanıyorsanız **Main.axml**, tüm kod çıkışı açıklama özen **MainActivity.cs** önceki XML kaynaklarına başvuruyor.

Bu düzen örnek varsayılan oluşturur `CardView` tek bir çizgi aşağıdaki ekran görüntüsünde gösterildiği gibi bir metin:

[![Kart görünümü ekran beyaz arka plan ve metin satırı ile](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

Bu örnekte, uygulama stili ışık malzeme tema ayarlanır (`Theme.Material.Light`) böylece `CardView` gölgeleri ve kenarları daha kolay. Tema Android 5.0 uygulamalar hakkında daha fazla bilgi için bkz: [malzeme tema](~/android/user-interface/material-theme.md). Sonraki bölümde, biz nasıl özelleştirileceğini öğreneceksiniz `CardView` bir uygulama için.


## <a name="customizing-cardview"></a>Kart görünümü özelleştirme

Temel değiştirebileceğiniz `CardView` görünümünü özelleştirmek için özniteliklerin `CardView` uygulamanızda. Örneğin, ayrıcalıkların `CardView` (olmasını sağlayan float arka plan üstüne yüksek göründüğü kartı) daha büyük bir gölge yayınlanamıyor artırılabilir. Ayrıca, daha fazla kart köşelerinde yuvarlanmasını yapmak için köşe yarıçapını artırılabilir.

Sonraki örnekte düzeni, özelleştirilmiş `CardView` yazdırma fotoğraf ("anlık görüntü") benzetimi oluşturmak için kullanılır. Bir `ImageView` eklenen `CardView` görüntü görüntülemek ve bir `TextView` altına yerleştirilmiş `ImageView` görüntünün başlığı görüntüleme.
Bu örnek düzende `CardView` aşağıdaki özelleştirmeler vardır:

-  `cardElevation` Daha büyük bir gölge cast 4dp için artırılır.
-  `cardCornerRadius` Daha yuvarlatılmış görünür köşeleri yapmak için 5dp artar.

Çünkü `CardView` Android v7 destek kitaplığı tarafından özniteliklerini kullanılamaz sağlanır `android:` ad alanı. Bu nedenle, kendi XML ad alanı tanımlamak ve bu ad alanı olarak kullanmanız gerekir `CardView` özniteliğinin öneki. Düzen aşağıdaki örnekte, bu satırın adlı ad alanı tanımlamak için kullanacağız `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Bu ad alanı çağırabilirsiniz `card_view` ve hatta `myapp` seçerseniz (yalnızca bu dosya kapsamı içinde erişilebilir durumda). Ne olursa olsun seçtiğinizde bu ad alanı arama, ön eki kullanmalısınız `CardView` değiştirmek istediğiniz özniteliği. Bu düzen örnekte `cardview` ad alanı öneki olan `cardElevation` ve `cardCornerRadius`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Görüntüyü bir fotoğraf görüntüleme uygulaması görüntülemek için bu düzeni örneği kullanıldığında `CardView` aşağıdaki ekran görüntüsünde gösterildiği gibi bir fotoğraf anlık görünüm vardır:

[![Bir resim ve görüntü altına resim yazısı kart görünümü](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

Bu ekran alınırlar [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) kullanan örnek uygulaması bir `RecyclerView` kaydırma listesini sunmak için pencere öğesi `CardView` fotoğraf görüntüleme için görüntüler. Hakkında daha fazla bilgi için `RecyclerView`, bkz: [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) Kılavuzu.

Dikkat bir `CardView` birden fazla alt görünümü içerik alanında görüntüleyebilirsiniz. Örneğin, yukarıdaki fotoğraf uygulama örneği görüntüleme içerik alanının oluşur bir `ListView` içeren bir `ImageView` ve `TextView`. Ancak `CardView` örnekleri genellikle düzenlenir dikey, ayrıca bunları yatay düzenleyebilirsiniz (bkz [özel görünüm stili oluşturma](~/android/user-interface/material-theme.md#customview) örnek bir ekran görüntüsü için).


### <a name="cardview-layout-options"></a>Kart görünümü Düzen Seçenekleri

`CardView` doldurma, yükseltme, köşe yarıçapı ve arka plan rengi etkileyen bir veya daha fazla öznitelikleri ayarlayarak düzenleri özelleştirilebilir:

[![Kart görünümü öznitelikleri diyagramı](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Her öznitelik da dinamik olarak bir karşılık gelen çağırarak değiştirilebilir `CardView` yöntemi (hakkında daha fazla bilgi için `CardView` yöntemleri bkz [kart görünümü sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Bu öznitelikler (dışında arka plan rengi) birim tarafından izlenen bir ondalık sayı bir boyut değeri kabul unutmayın. Örneğin, `11.5dp` 11.5 yoğunluğu bağımsız piksel belirtir.


#### <a name="padding"></a>Doldurma
`
CardView` kart içinde konumlandırmak için beş doldurma öznitelikleri sunar. Düzen XML ayarlayabilirsiniz veya benzer yöntemleri kodunuzda çağırabilirsiniz:

[![Öznitelikleri doldurma kart görünümü diyagramı](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Doldurma öznitelikleri gibi açıklanmıştır:

-  `contentPadding` &ndash; İç alt görünümleri arasında doldurma `CardView` ve kart tüm kenarlarına.

-  `contentPaddingBottom` &ndash; İç alt görünümleri arasında doldurma `CardView` ve kart kenar.

-  `contentPaddingLeft` &ndash; İç alt görünümleri arasında doldurma `CardView` kartın sol kenarı.

-  `contentPaddingRight` &ndash; İç alt görünümleri arasında doldurma `CardView` ve kart sağ köşesine.

-  `contentPaddingTop` &ndash; İç alt görünümleri arasında doldurma `CardView` ve kart üst kenarı.

İçerik doldurma öznitelikleri, içerik alanının yerine içerik alanının içinde bulunan belirli pencere öğesi için sınır göre görüntülenir.
Örneğin, varsa `contentPadding` yeterince fotoğraf görüntüleme uygulamada daha fazla `CardView` görüntü ve kart üzerinde gösterilen metin kırpma.



#### <a name="elevation"></a>Yükseltme

`CardView` kendi ayrıcalık denetlemek için iki ayrıcalık öznitelik sağlar ve sonuç olarak, gölgesini boyutu:

[![Kart görünümü ayrıcalık öznitelikleri diyagramı](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Yükseltme öznitelikleri gibi açıklanmıştır:

-  `cardElevation` &ndash; Ayrıcalıkların `CardView` (Z ekseni temsil eder).

-  `cardMaxElevation` &ndash; En büyük değerini `CardView`'s yükseltme.

Daha büyük değerlerini `cardElevation` yapmak için gölge boyutunu artırın `CardView` float arka plan üstüne yüksek görünüyor. `cardElevation` Özniteliği de görünümler çakışan çizim sırası belirler; bu, `CardView` başka bir çakışan görünümü ve daha düşük bir yükseltme ayarı çakışan herhangi görünümlerle üstü daha yüksek bir ayrıcalık ayar ile altında çizilmiştir.
`cardMaxElevation` Ayardır uygulamanızı ayrıcalık dinamik olarak değiştiğinde için yararlı &ndash; bu ayarlarla tanımladığınız sınırı aşan genişletme gölge engeller.


#### <a name="corner-radius-and-background-color"></a>Köşe yarıçapı ve arka plan rengi

`CardView` köşe yarıçapı ve arka plan rengini denetlemek için kullanabileceğiniz özniteliklerin sunar. Bu iki özellik genel stilini değiştirme izin `CardView`:

[![Kart görünümü köşe radious ve arka plan rengi özniteliklerini diyagramı](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Bu öznitelikler aşağıda açıklanmıştır:

-  `cardCornerRadius` &ndash; Tüm köşelerinde köşe yarıçapını `CardView`.

-  `cardBackgroundColor` &ndash; Arka plan rengini `CardView`.

Bu diyagramda `cardCornerRadius` daha yuvarlak 10dp için ayarlanır ve `cardBackgroundColor` ayarlanır `"#FFFFCC"` (Açık Sarı).


## <a name="compatibility"></a>Uyumluluk

Kullanabileceğiniz `CardView` Android Android 5.0 Lolipop'den önceki sürümlerinde. Çünkü `CardView` parçasıdır Android v7 destek Kitaplığı'nın kullandığınız `CardView` Android 2.1 (API düzeyi 7) ve daha yüksek.
Ancak, yüklemeniz gereken `Xamarin.Android.Support.v7.CardView` paketini açıklandığı gibi [gereksinimleri](#requirements), yukarıdaki.

`CardView` Lolipop (API düzeyi 21) önce cihazlarda biraz farklı bir davranış gösteriyor:

-  `CardView` Ek doldurma ekler programlı gölge uygulama kullanır.

-  `CardView` ile kesiştiği alt görünümleri küçük değil `CardView`yuvarlanmış köşeleri.

Bu uyumluluk farklılıklar yönetimine yardımcı olmak için `CardView` , düzende yapılandırabilirsiniz birkaç ek öznitelikler sağlar:

-   `cardPreventCornerOverlap` &ndash; Bu öznitelik ayarlanırsa `true` önceki Android sürümlerinde (API düzeyi 20 ve önceki sürümler) uygulamanızı çalıştırırken doldurma eklemek için. Bu ayar engeller `CardView` ile kesişen içeriği `CardView`yuvarlanmış köşeleri.

-   `cardUseCompatPadding` &ndash; Bu öznitelik ayarlanırsa `true` sürümlerinde, Android veya API düzeyi 21'dan büyük, uygulamanızı çalıştırırken doldurma eklemek için. Kullanmak istiyorsanız, `CardView` öncesi Lolipop cihazlarda ve Lolipop (veya üstü)'de aynı görünür, bu öznitelik ayarlanırsa `true`. Bu özellik etkinleştirildiğinde, `CardView` öncesi Lolipop cihazlarda çalıştığında gölgeleri çizmek için ek doldurma ekler. Bu ön Lolipop programlı gölge uygulamaları etkin olduğunda, sunulan doldurma farklılıkları aşmak için yardımcı olur.

Android önceki sürümleriyle uyumluluk bakımı hakkında daha fazla bilgi için bkz: [Bakımı Uyumluluk](https://developer.android.com/training/material/compatibility.html).


## <a name="summary"></a>Özet

Bu kılavuz yeni sunulan `CardView` Android 5.0 (Lolipop) dahil pencere öğesi. Varsayılan gösterilen `CardView` görünümünü ve nasıl özelleştirileceği açıklanmıştır `CardView` yükseltme, köşe yuvarlaklığını değiştirerek, içerik doldurma ve arka plan rengi. Listelendiği `CardView` düzeni özniteliklerle (başvuru diyagramları) ve nasıl kullanılacağını açıklanmıştır `CardView` Android 5.0 Lolipop'den önceki Android cihazlarda. Hakkında daha fazla bilgi için `CardView`, bkz: [kart görünümü sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/CardView.html).


## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerView (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Lolipop giriş](~/android/platform/lollipop.md)
- [Kart görünümü sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
