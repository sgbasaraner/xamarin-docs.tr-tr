---
title: "Görünümlerle ViewPager"
description: "ViewPager jestsel Gezinti uygulamanıza olanak sağlayan bir düzen yöneticisidir. Sol ve sağ adıma veri sayfaları aracılığıyla jestsel Gezinti manyetik kullanıcıya sağlar. Bu kılavuz swipeable UI ViewPager ve PagerTabStrip, veri sayfaları olarak görünümleri ile uygulamak nasıl açıklar (bir sonraki kılavuz parçaları sayfalarının için nasıl kullanılacağını kapsar)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d81f897fb7af39334cec4ea9f806533f09754079
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="viewpager-with-views"></a>Görünümlerle ViewPager

_ViewPager jestsel Gezinti uygulamanıza olanak sağlayan bir düzen yöneticisidir. Sol ve sağ adıma veri sayfaları aracılığıyla jestsel Gezinti manyetik kullanıcıya sağlar. Bu kılavuz swipeable UI ViewPager ve PagerTabStrip, veri sayfaları olarak görünümleri ile uygulamak nasıl açıklar (bir sonraki kılavuz parçaları sayfalarının için nasıl kullanılacağını kapsar)._

<a name="overview" />
 
## <a name="overview"></a>Genel Bakış

Bu kılavuzu kullanmak için adım adım adım bir gösterim sağlar bir kılavuz olduğu `ViewPager` yaprak döken ve her ağaçları bir görüntü Galerisi uygulamak için. Bu uygulamada, kullanıcı swipes sol ve sağ "ağaç Kataloğu" yoluyla ağaç görüntüleri görüntülemek için. Ağaç adı listelenir kataloğun her sayfanın en üstünde bir`PagerTabStrip`, ve görüntünün ağacının görüntülenen bir `ImageView`. Bir bağdaştırıcı kullanılır arabirimine `ViewPager` veri modeli için. Bu uygulamayı türetilmiş bir bağdaştırıcı uygulayan `PagerAdapter`. 

Ancak `ViewPager`-tabanlı uygulamaları ile genellikle uygulanır `Fragment`s, bazı görece basit kullanım durumları vardır burada fazladan karmaşıklığını `Fragment`s gerekli değildir. Örneğin, bu örneklerde gösterilen temel görüntü Galerisi uygulama kullanımını gerektirmez `Fragment`s. Çünkü içerik statiktir ve kullanıcı yalnızca swipes farklı resimler arasında ileri ve geri uygulama standart Android görünümleri ve düzenleri kullanarak basit tutulabilir. 


<a name="start" />

## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Adlı yeni bir Android projesi oluşturma **TreePager** (bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) yeni Android projeler oluşturma hakkında daha fazla bilgi için). Ardından, NuGet Paket Yöneticisi'ni başlatın. (NuGet paketlerini yükleme hakkında daha fazla bilgi için bkz: [izlenecek yol: de dahil olmak üzere bir NuGet projenizdeki](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Bulma ve yükleme **Android destek kitaplığı v4**: 

[![NuGet Paket Yöneticisi'nde seçili ekran desteği v4 Nuget](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png)

Bu aynı zamanda herhangi bir ek paketler reaquired tarafından yükleyecek **Android destek kitaplığı v4**.


<a name="datasource" />

## <a name="add-an-example-data-source"></a>Bir örnek veri kaynağı ekleme

Bu örnekte, ağaç Kataloğu veri kaynağı (tarafından temsil edilen `TreeCatalog` sınıfı) sağlayan `ViewPager` öğesi içeriğe sahip. 
`TreeCatalog` ağacı görüntüler ve bağdaştırıcısı oluşturmak için kullanacağı ağaç başlıkları hazır koleksiyonunu içeren `View`s. `TreeCatalog` Oluşturucu bağımsız değişken gerektirir:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Görüntülerde koleksiyonunu `TreeCatalog` her görüntü bir dizin oluşturucu tarafından erişilebilecek şekilde düzenlenmiştir. Örneğin, aşağıdaki kod satırını koleksiyondaki üçüncü görüntü için görüntü kaynak kimliği alır: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Çünkü uygulama ayrıntılarını `TreeCatalog` anlamak için ilgili olmayan `ViewPager`, `TreeCatalog` kodu buraya listelenmiyor. Kaynak koduna `TreeCatalog` şu adresten edinilebilir [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Bu kaynak dosyasını karşıdan yükleyin (veya kodu kopyalayıp yeni dosyaya yapıştırın **TreeCatalog.cs** dosyası) ve projenize ekleyin. Ayrıca, indirip sıkıştırmasını [görüntü dosyaları](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) içine, **kaynakları/drawable** klasörü ve projeye ekleyin. 


<a name="layout" />

## <a name="create-a-viewpager-layout"></a>ViewPager düzenini oluşturma

Açık **Resources/layout/Main.axml** ve içeriğini aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 


<a name="setup" />

## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

Değiştir `OnCreate` aşağıdaki kod ile yöntemi:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

Bu kod şunları yapar:

1.  Görünümden ayarlar **Main.axml** düzeni kaynak.

2.  Bir başvuru alır `ViewPager` düzeninden.

3.  Yeni bir örneğini oluşturur `TreeCatalog` veri kaynağı olarak.

Derleme ve bu kodu çalıştırmak, aşağıdaki ekran görüntüsüne benzer bir ekran görmeniz gerekir: 

[![Boş bir ViewPager görüntüleme uygulamasının ekran görüntüsü](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png)

Bu noktada, `ViewPager` içeriğinde erişmek için bir bağdaştırıcı bulunmaması boş olduğundan **TreeCatalog**. Sonraki bölümde bir **PagerAdapter** bağlanmak için oluşturulan `ViewPager` için **TreeCatalog**. 


<a name="adapter" />

## <a name="create-the-adapter"></a>Bağdaştırıcısı oluştur

`ViewPager` arasında bulunur bağdaştırıcısı denetleyicisi nesnesini kullanan `ViewPager` ve veri kaynağı (çizimde bkz [bağdaştırıcısı](~/android/user-interface/controls/view-pager/index.md#adapter)). Bu verilere erişmek için `ViewPager` türetilen özel bir bağdaştırıcı sağlamanızı ister `PagerAdapter`. Bu bağdaştırıcı her doldurur `ViewPager` veri kaynağından içerik sayfası. Bu veri kaynağı uygulamaya özgü olduğundan, özel bağdaştırıcı verilere nasıl erişileceğini anlar kodudur. Sayfaları aracılığıyla kullanıcı swipes olarak `ViewPager`, bağdaştırıcı veri kaynağından bilgileri ayıklar ve sayfalar için yükler `ViewPager` görüntülemek için. 

Ne zaman uygulamanız bir `PagerAdapter`, aşağıdaki geçersiz kılmanız gerekir:

-   **InstantiateItem** &ndash; sayfası oluşturur (`View`) için verilen bir konuma ve ona ekler `ViewPager`'s görünümler koleksiyonu. 

-   **DestroyItem** &ndash; belirli bir konumdan bir sayfa kaldırır.

-   **Count** &ndash; Görünüm (sayfaları) kullanılabilir sayısını döndürür özelliği salt okunur. 

-   **IsViewFromObject** &ndash; bir sayfa belirli bir anahtar nesne ile ilişkili olup olmadığını belirler. (Bu nesne tarafından oluşturulan `InstantiateItem` yöntemi.) Bu örnekte, anahtar nesnedir `TreeCatalog` veri nesnesi.

Adlı yeni bir dosya ekleme **TreePagerAdapter.cs** ve içeriğini aşağıdaki kodla değiştirin: 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

Bu kod gerekli yerleştirir `PagerAdapter` uygulaması. Aşağıdaki bölümlerde bu yöntemlerin her biri çalışma kodu ile değiştirilir. 


<a name="ctor" />

### <a name="implement-the-constructor"></a>Uygulama Oluşturucu

Uygulamanın ne zaman başlatır `TreePagerAdapter`, bir bağlam sağlar ( `MainActivity`) ve bir örneklenen `TreeCatalog`. Aşağıdaki üye değişkenleri ve Oluşturucusu en üst kısmına ekleyin `TreePagerAdapter` sınıfını **TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Bu oluşturucu amacı bağlam depolamaktır ve `TreeCatalog` örneğine `TreePagerAdapter` kullanır. 


<a name="count" />

### <a name="implement-count"></a>Uygulama sayısı

`Count` Uygulaması oldukça basittir: ağaç Kataloğu'nda ağaçları sayısını döndürür. Değiştir `Count` aşağıdaki kod ile:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees` Özelliği `TreeCatalog` veri kümesinde ağaçları (sayfa sayısı) sayısını döndürür.


<a name="instantiateitem" />

### <a name="implement-instantiateitem"></a>Uygulama InstantiateItem

`InstantiateItem` Yöntemi, verilen bir konuma sayfası oluşturur. Yeni oluşturulan görünümüne eklemelisiniz `ViewPager`'s görüntülemek koleksiyonu. Bu olası yapmak için `ViewPager` kendisini kapsayıcı parametre olarak geçirir. 

Değiştir `InstantiateItem` aşağıdaki kod ile yöntemi:

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

Bu kod şunları yapar:

1.  Yeni bir örneğini oluşturur `ImageView` belirtilen konumda ağaç görüntüyü görüntülemek için. Uygulamanın `MainActivity` öğesine geçen bağlam `ImageView` Oluşturucusu.

2.  Ayarlar `ImageView` kaynağa `TreeCatalog` kaynak kimliği belirli bir konumda görüntü.

3.  Geçirilen kapsayıcı bıraktığı `View` için bir `ViewPager` başvuru.
    Kullanmanız gereken Not `JavaCast<ViewPager>()` düzgün (Bu gerekir böylece Android çalışma zamanı işaretli tür dönüştürmesi gerçekleştirir) bu dönüştürme gerçekleştirmek için.

4.  Örneklenen ekler `ImageView` için `ViewPager` ve döndürür `ImageView` çağırana.

Zaman `ViewPager` yansımayı görüntüler `position`, bu görüntüler `ImageView`. Başlangıçta, `InstantiateItem` iki kez görünümleri ilk iki sayfalarıyla doldurmak için çağrılır. Kullanıcı kayarken yeniden görünümler yalnızca arkasında ve şu anda görüntülenen öğenin önünde korumak için çağrılır. 


<a name="destroyitem" />

### <a name="implement-destroyitem"></a>Uygulama DestroyItem

`DestroyItem` Yöntemi, belirtilen konumdan bir sayfa kaldırır. Burada verilen bir konuma Sergi değiştirebilir, uygulamalarda `ViewPager` ile yeni bir görünüm değiştirmeden önce bu konumda eski bir görünüm kaldırmanın bazı yolu olmalıdır. İçinde `TreeCatalog` örnek, her konumunda görünüm değiştirmez, bir görünüm tarafından kaldırılmış şekilde `DestroyItem` yalnızca ne zaman yeniden eklenir `InstantiateItem` bu konum için çağrılır. (Daha iyi verimlilik için bir geri dönüşüm için bir havuz uygulamak `View`aynı konumda yeniden görüntülenen s.) 

Değiştir `DestroyItem` aşağıdaki kod ile yöntemi: 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Bu kod şunları yapar:

1.  Geçirilen kapsayıcı bıraktığı `View` içine bir `ViewPager` başvuru.

2.  Geçirilen Java nesnesine çevirir (`view`) içine bir C# `View` (`view as View`);

3.  Görünümden kaldırır `ViewPager`. 


<a name="isviewfromobject" />

### <a name="implement-isviewfromobject"></a>Implement IsViewFromObject

İçerik sayfaları aracılığıyla sağ ve sol kullanıcı slayt olarak `ViewPager` çağrıları `IsViewFromObject` doğrulamak için alt `View` verilen konumdaki aynı bu konum için bağdaştırıcının nesnesi ile ilişkilidir (Bu nedenle, bağdaştırıcının nesnesi adında bir *nesne anahtarı*). Görece basit uygulamalar için kimlik birini ilişkilendirmedir &ndash; bağdaştırıcının nesne bu örneğe anahtarda daha önce döndürülen görünümdür `ViewPager` aracılığıyla `InstantiateItem`. Ancak diğer uygulamalar için nesne anahtarı ile ilişkili bazı bir bağdaştırıcıya özgü sınıf örneği (ancak aynısı değildir) olabilir alt görüntülemek `ViewPager` o konumdan görüntüler. Geçirilen görünümü ve nesne anahtarı ilişkili olup olmadığını yalnızca bağdaştırıcı bilmez. 

`IsViewFromObject` için uygulanmalı `PagerAdapter` düzgün çalışması için. Varsa `IsViewFromObject` döndürür `false` için verilen bir konuma `ViewPager` görünüm o konumdan görüntülemez. İçinde `TreePager` uygulama, nesne tarafından döndürülen anahtar `InstantiateItem` *olan* sayfa `View` kodu yalnızca kimlik için denetlenecek nedenle bir ağacının (yani, nesne anahtarı ve görünümü bir aynı). Değiştir `IsViewFromObject` aşağıdaki kod ile: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

<a name="addadapter" />

## <a name="add-the-adapter-to-the-viewpager"></a>Bağdaştırıcı için ViewPager Ekle

Şimdi `TreePagerAdapter` olan uygulanan eklemek için zamanı `ViewPager`. İçinde **MainActivity.cs**, sonuna aşağıdaki kod satırını ekleyin `OnCreate` yöntemi:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Bu kod başlatır `TreePagerAdapter`, içinde geçen `MainActivity` bağlamı olarak (`this`). Örneklenen `TreeCatalog` Oluşturucusu kişinin ikinci bağımsız değişken geçirildi. `ViewPager`'S `Adapter` özelliği ayarlanmış örneklenen için `TreePagerAdapter` nesne; bu fişleri `TreePagerAdapter` içine `ViewPager`. 

Çekirdek uygulamasını tamamlanmıştır &ndash; oluşturmak ve uygulamayı çalıştırın. Sol sonraki ekran görüntüsünde gösterildiği gibi ekranda görüntülenen görüntünün ilk ağaç kataloğun görmeniz gerekir. Daha fazla ağaç görünümleri görmek için sola ağaç katalog geri taşımak için sonra manyetik sağ geçirme: 

[![Ekran görüntüleri, TreePager uygulama ağaç resimler arasında geçirme](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png)


<a name="pagetabstrip" />

## <a name="add-a-pager-indicator"></a>Çağrı cihazı göstergesi Ekle

Bu en az `ViewPager` uygulama ağaç Kataloğu resimleri görüntüler, ancak kullanıcının Kataloğu içinde olduğu konusunda herhangi bir gösterge sağlar. Sonraki adım eklemektir bir `PagerTabStrip`. `PagerTabStrip` Hangi sayfası görüntülenir ve önceki ve sonraki sayfaların ipucu görüntüleyerek Gezinme bağlamı sağlar kullanıcı için olarak bildirir. `PagerTabStrip` Geçerli sayfa için bir göstergesi olarak kullanılacak hedeflenen bir `ViewPager`; kaydırır ve her bir sayfa aracılığıyla kullanıcı swipes olarak güncelleştirir. 

Açık **Resources/layout/Main.axml** ve ekleme bir `PagerTabStrip` Düzen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager` ve `PagerTabStrip` birlikte çalışmak üzere tasarlanmıştır. Ne zaman bildirdiğiniz bir `PagerTabStrip` içinde bir `ViewPager` düzeni `ViewPager` otomatik olarak bulur `PagerTabStrip` ve bağdaştırıcısına bağlanacak. Derleme ve uygulamayı çalıştırma, boş görmelisiniz `PagerTabStrip` her ekranın en üstünde gösterilir: 

[![Boş bir PagerTabStrip Closeup ekran görüntüsü](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png)


<a name="title" />

### <a name="display-a-title"></a>Bir başlığı görüntüleme

Her bir sayfa sekmesini bir başlık eklemek için uygulama `GetPageTitleFormatted` yönteminde `PagerAdapter`-türetilmiş sınıf. `ViewPager` çağrıları `GetPageTitleFormatted` (uygulanırsa) belirli bir konumda sayfasını açıklar başlık dize elde edilir. Aşağıdaki yöntemi ekleyin `TreePagerAdapter` sınıfını **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Bu kodu belirtilen sayfasından (konum) ağaç kataloğunda ağaç resim yazısı dizesini alır, bir Java dönüştürür `String`ve geri döndüğünde `ViewPager`. Bu yeni yöntemiyle uygulamayı çalıştırdığınızda, her bir sayfa ağaç yazısının görüntüler `PagerTabStrip`. Bir alt çizginin olmadan ekranın üst ağaç adını görmeniz gerekir: 

[![Metin doldurulmuş PagerTabStrip sekmelerle sayfaların ekran görüntüleri](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png)

İleri ve geri katalogda her başlıklı ağaç resmi görmek için doğru çekin. 


<a name="pagertitlestrip" />

### <a name="pagertitlestrip-variation"></a>PagerTitleStrip Variation

`PagerTitleStrip` çok benzer `PagerTabStrip` dışında `PagerTabStrip` şu anda seçili sekmenin altı çizili ekler. Değiştirebileceğiniz `PagerTabStrip` ile `PagerTitleStrip` yukarıdaki düzen ve çalışma yeniden ile nasıl göründüğünü görmek için uygulamaya `PagerTitleStrip`: 

[![PagerTitleStrip alt çizgilerle metinden kaldırıldı](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png)

İçin dönüştürürken, alt çizgi kaldırıldığını unutmayın `PagerTitleStrip`. 


<a name="summary" />
 
## <a name="summary"></a>Özet

Bu kılavuz temel yapı konusunda adım adım örnek sağlanan `ViewPager`-kullanmadan uygulama tabanlı `Fragment`s. Görüntüleri ve resim yazısı dizeleri içeren bir örnek veri kaynağı sunulan bir `ViewPager` görüntüleri göstermek için Düzen ve `PagerAdapter` bağlayan bir alt `ViewPager` veri kaynağı. Veri kümesi aracılığıyla gidin kullanıcının yardımcı olmak için yönergeleri dahil nasıl ekleneceği açıklayan bir `PagerTabStrip` veya `PagerTitleStrip` resim yazısı her sayfanın en üstünde görüntülenecek. 


## <a name="related-links"></a>İlgili bağlantılar

- [TreePager (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
