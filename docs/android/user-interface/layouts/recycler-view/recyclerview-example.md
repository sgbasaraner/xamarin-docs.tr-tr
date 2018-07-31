---
title: Basit bir RecyclerView örneği
description: RecyclerView kullanmayı gösteren bir örnek uygulama.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/30/2018
ms.openlocfilehash: d48796b3c62fc342bd86f2d58e74c5f1710174bb
ms.sourcegitcommit: 0a1c392829454468dbe92f81d975e124a22b7014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39360844"
---
# <a name="a-basic-recyclerview-example"></a>Basit bir RecyclerView örneği

Anlamak için nasıl `RecyclerView` tipik bir uygulaması çalışır, bu konuda ele [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) örnek uygulama, kullanan bir basit kod örnek `RecyclerView` fotoğraf büyük koleksiyonunu görüntülemek için: 

[![Fotoğrafları görüntülemek için CardViews kullanan RecyclerView uygulaması iki ekran görüntüleri](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** kullanan [CardView](~/android/user-interface/controls/card-view.md) her fotoğrafı öğesini uygulamak için `RecyclerView` düzeni. Nedeniyle `RecyclerView`ait performans avantajları, bu örnek uygulamayı hızla ve belirgin gecikmeler olmadan fotoğrafların büyük bir koleksiyonda gezinebilirsiniz.


### <a name="an-example-data-source"></a>Bir örnek veri kaynağı

Bu örnek uygulamasında, bir "fotoğraf albümü" veri kaynağı (tarafından temsil edilen `PhotoAlbum` sınıfı) sağladığı `RecyclerView` öğesi içeriğe sahip.
`PhotoAlbum` açıklamalı alt yazılar sahip fotoğraf koleksiyonudur; Bunu başlattığınızda, kullanıma hazır bir koleksiyon 32 fotoğrafların alın:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Her fotoğraf örneğinde `PhotoAlbum` görüntü kaynak Kimliğini okumak tanıyan özellikler sunan `PhotoID`ve kendi açıklamalı alt yazı dize `Caption`. Fotoğraf koleksiyonunu her resim bir dizin oluşturucu tarafından erişilebilir olacak şekilde düzenlenir. Örneğin, aşağıdaki kod satırlarını koleksiyondaki onuncu fotoğraf için resim yazısı ve görüntü kaynak kimliği erişin:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` Ayrıca sağlar bir `RandomSwap` koleksiyondaki ilk fotoğrafı koleksiyonu içindeki başka bir yerde rasgele seçilen fotoğraf ile takas için çağırabileceğiniz yöntemi:

```csharp
mPhotoAlbum.RandomSwap ();
```

Çünkü uygulama ayrıntılarını `PhotoAlbum` anlamak için ilgili olmayan `RecyclerView`, `PhotoAlbum` kaynak kodu değil sunulur burada. Kaynak koduna `PhotoAlbum` kullanılabilir [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) içinde [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) örnek uygulaması.


### <a name="layout-and-initialization"></a>Düzen ve başlatma

Düzen dosyası **Main.axml**, bir tek oluşur `RecyclerView` içinde bir `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Tam adı kullanmanız gerekir Not **android.support.v7.widget.RecyclerView** çünkü `RecyclerView` destek kitaplığa paketlenmiştir. `OnCreate` Yöntemi `MainActivity` bu düzen başlatır, bağdaştırıcı başlatır ve temel alınan veri kaynağı hazırlar:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

Bu kod şunları yapar:

1. Başlatır `PhotoAlbum` veri kaynağı.

2. Oluşturucuya bağdaştırıcısının, fotoğraf albümü veri kaynağına geçirir `PhotoAlbumAdapter` (Bu kılavuzun devamında tanımlanmış). 
   Bu veri kaynağı bağdaştırıcısı oluşturucusuna bir parametre olarak geçirmek için en iyi uygulama varsayılır unutmayın. 

3. Alır `RecyclerView` düzeninden.

4. Bağdaştırıcı takılan `RecyclerView` çağırarak örneği `RecyclerView` `SetAdapter` yukarıda da gösterildiği gibi yöntemi.

### <a name="layout-manager"></a>Düzen Yöneticisi

Her öğe `RecyclerView` oluşan bir `CardView` bir fotoğraf resmini ve fotoğraf yazısı içeren (ayrıntıları içinde ele alınmıştır [görünümü sahibi](#view-holder) bölümüne bakın). Önceden tanımlanmış `LinearLayoutManager` her düzenlemek için kullanılan `CardView` içinde dikey bir kaydırma düzenleme:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Bu kod ana etkinlik içinde bulunan `OnCreate` yöntemi. Düzen Yöneticisi oluşturucuya gerektiren bir *bağlam*, bu nedenle `MainActivity` kullanarak geçirilir `this` üstündeki görülenle.

Yerine predefind `LinearLayoutManager`, iki görüntüleyen bir özel düzen Manager'da takılabilir `CardView` -fotoğraf koleksiyonunu geçirmek için page-turning animasyon efekti uygulama yan, öğeleri. Bu kılavuzda daha sonra farklı bir düzen Yöneticisi'nde değiştirerek düzenini değiştirmeye ilişkin bir örnek görürsünüz.

<a name="view-holder" />

### <a name="view-holder"></a>Görünüm sahibi

Görünüm sahibi sınıfı olarak adlandırılan `PhotoViewHolder`. Her `PhotoViewHolder` örneği başvuru tutan `ImageView` ve `TextView` , düzenlendiğini ilişkili satır öğenin bir `CardView` burada tasarımını gibi:

[![Bir ImageView ve TextView içeren CardView diyagramı](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` öğesinden türetilen `RecyclerView.ViewHolder` ve başvuruları depolamak için özellikleri içeren `ImageView` ve `TextView` yukarıdaki düzende gösterilir.
`PhotoViewHolder` iki özellik ve bir oluşturucu oluşur:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```
Bu kod örneğinde `PhotoViewHolder` Oluşturucusu üst öğesi görünüme bir başvuru geçirilen ( `CardView`), `PhotoViewHolder` sarmalar. Üst her zaman temel oluşturucu öğesi görünüme iletmek unutmayın. `PhotoViewHolder` Oluşturucu çağrıları `FindViewById` her biri kendi alt görünüm başvurularını bulmak için üst öğe görünümünde `ImageView` ve `TextView`, sonuçları şurada depolayarak `Image` ve `Caption` özellikleri, sırasıyla. Bu güncelleştirmeleri bağdaştırıcısı görünümü başvuruları daha sonra bu özellikleri alır. `CardView`ait alt görünümleri yeni verilerle.

Hakkında daha fazla bilgi için `RecyclerView.ViewHolder`, bkz: [RecyclerView.ViewHolder sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Bağdaştırıcı

Her bağdaştırıcı yükler `RecyclerView` satır ile belirli bir fotoğraf verileri. Belirli bir fotoğraf satır konumunda için *P*, örneğin, bağdaştırıcı konumunda ilişkili verileri bulur *P* kopyalar ve veri kaynağı içinde bu verileri satır öğesi konumunda *P* içinde `RecyclerView` koleksiyonu. Bağdaştırıcı için başvurular aramak için görünümü sahibi kullanan `ImageView` ve `TextView` tekrar tekrar çağırmak zorunda değildir, bu nedenle, ilgili konumdaki `FindViewById` bu görünümleri için kullanıcı kaydırır fotoğraf toplulukta ve görünümleri yeniden kullanır.

İçinde **RecyclerViewer**, bağdaştırıcı sınıfı türetilen `RecyclerView.Adapter` oluşturmak için `PhotoAlbumAdapter`:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

`mPhotoAlbum` Üyeyi içeren oluşturucuya geçirilen veri kaynağı (fotoğraf albümü); Oluşturucu fotoğraf albümü bu üye değişkeni kopyalar. Aşağıdaki gerekli `RecyclerView.Adapter` yöntemleri uygulanır:

-   **`OnCreateViewHolder`** &ndash; Dosya ve görünüm öğesi düzeni sahibi başlatır.

-   **`OnBindViewHolder`** &ndash; Belirli bir konumda veri içinde belirli görünüm sahibi olan başvuruları depolanan görünümleri yükler.

-   **`ItemCount`** &ndash; Veri kaynağındaki öğe sayısını döndürür.

Bu öğeleri konumlandırma sırasında düzen Yöneticisi bu yöntemleri çağıran `RecyclerView`. Aşağıdaki bölümlerde bu yöntemlerin uygulama incelenir.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Düzen manager çağrıları `OnCreateViewHolder` olduğunda `RecyclerView` bir öğeyi temsil etmek için yeni bir görünüm sahibi gerekir. `OnCreateViewHolder` görünümün Düzen dosyası öğesi görünümünden Şişir ve yeni bir görünüm sarmalar `PhotoViewHolder` örneği. `PhotoViewHolder` Oluşturucusu bulur ve daha önce açıklandığı gibi alt görünümleri başvuruları düzende depolar [görünümü sahibi](#view-holder).

Her satır öğesi tarafından temsil edilen bir `CardView` içeren bir `ImageView` (için fotoğrafı) ve bir `TextView` (için resim yazısı). Bu düzen dosyasında bulunan **PhotoCardView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

Bu düzen bir tek satır öğeyi temsil `RecyclerView`. `OnBindViewHolder` Yöntemi (aşağıda açıklanmıştır) veri kaynağından veri kopyalar `ImageView` ve `TextView` bu düzeni.
`OnCreateViewHolder` Bu düzen verilen fotoğraf konumu Şişir `RecyclerView` ve yeni bir örneğini oluşturur `PhotoViewHolder` örneği (bulur ve başvurular önbelleğe `ImageView` ve `TextView` ilişkili alt görünümlerde `CardView` Düzen):

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

Sonuçta elde edilen görünümü sahibi örneği `vh`, tekrar çağırana (Düzen Yöneticisi) döndürülür.


#### <a name="onbindviewholder"></a>OnBindViewHolder

Düzen Yöneticisi olduğunda belirli bir görünümde görüntülenecek hazır `RecyclerView`'s görünür ekran alanı, bağdaştırıcının çağırır `OnBindViewHolder` veri kaynağından alınan içerikle belirtilen satır konumunda öğeyi doldurmak için yöntemi. `OnBindViewHolder` Belirtilen satır konumu (fotoğrafın görüntü kaynağı ve fotoğrafın resim yazısı için dize) fotoğraf bilgilerini alır ve bu verileri ilişkili görünümlerine kopyalar. Görünüm sahibi nesnesinde depolanan başvurular üzerinden görünümlerini yer (hangi geçirilir aracılığıyla `holder` parametresi):

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

Geçirilen görünümü tutucu nesnesindeki ilk türetilmiş görünüm sahibi türe dönüştürmeniz gerekir (Bu durumda, `PhotoViewHolder`), kullanılmadan önce.
Görüntü kaynağı görünümü sahibinin tarafından başvurulan görünüm bağdaştırıcısı yükler `Image` özelliği ve görünüm sahibinin tarafından başvurulan bir görünümde açıklamalı alt yazı metni kopyalar `Caption` özelliği. Bu *bağlar* verilerini ile ilişkili görünüm.

Dikkat `OnBindViewHolder` doğrudan veri yapısı ile ilgilenen kodudur. Bu durumda, `OnBindViewHolder` nasıl eşleneceğini anlar `RecyclerView` öğesini kendi ilişkili veri öğesi için veri kaynağı konumu. Eşleme, bu durumda çünkü konumu bir dizi dizini fotoğraf albümü kullanılabilir basittir; Ancak, daha karmaşık veri kaynakları, böyle bir eşleme oluşturmak için ek bir kod gerektirebilir.


#### <a name="itemcount"></a>ItemCount

`ItemCount` Yöntemi veri koleksiyondaki öğe sayısını döndürür. Örnek Fotoğraf Görüntüleyici uygulamasında öğe sayısı fotoğraf albümü fotoğraflardaki sayısıdır:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Hakkında daha fazla bilgi için `RecyclerView.Adapter`, bkz: [RecyclerView.Adapter sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Hepsini birleştirme

Ortaya çıkan `RecyclerView` örnek fotoğraf uygulaması için uygulama oluşur `MainActivity` veri kaynağını, düzeni Yöneticisi ve bağdaştırıcı oluşturan kodu. `MainActivity` oluşturur `mRecyclerView` örneği, veri kaynağı ve bağdaştırıcısı başlatır ve Düzen Yöneticisi ve bağdaştırıcısı yararlanmanıza imkan sağlar:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` bulur ve görünüm başvuruları önbelleğe alır:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` üç gereken yöntemini geçersiz kılmaları uygular:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

Bu kod derlenir ve çalıştırma, aşağıdaki ekran görüntülerinde gösterildiği uygulaması görüntülendiğinde temel fotoğraf oluşturur:

[![Fotoğraf fotoğraf kartları dikey kaydırma ile uygulama görüntüleme iki ekran görüntüleri](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Shadows (yukarıdaki ekran görüntüsünde görüldüğü gibi) çizilen değil, Düzen **Properties/AndroidManifest.xml** ve aşağıdaki öznitelik ayarı ekleme `<application>` öğesi:

```xml
android:hardwareAccelerated="true"
```

Bu temel bir uygulama, fotoğraf albümü göz atma yalnızca destekler. Öğe dokunma olayları için yanıt vermez ve temel alınan verilerde yapılan değişiklikler işlemiyor. Bu işlev eklenir [RecyclerView örneği genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md).




### <a name="changing-the-layoutmanager"></a>LayoutManager değiştirme

Nedeniyle `RecyclerView`ın esneklik, onu farklı bir düzen Yöneticisi'ni kullanmak için uygulamayı değiştirilmesi kolay. Aşağıdaki örnekte, fotoğraf albümü yatay kaydırma bir kılavuz düzeni yerine dikey doğrusal bir düzen ile görüntülemek için değiştirilir. Bunu yapmak için Düzen manager oluşturmada kullanılacak değiştirilir `GridLayoutManager` gibi:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Bu kod değişikliği dikey değiştirir `LinearLayoutManager` ile bir `GridLayoutManager` yatay yönde kaydırma iki satır oluşan bir kılavuz gösterir. Derleme ve uygulamayı yeniden çalıştırın, fotoğraflar kılavuzda görüntülenir ve kaydırma yatay yerine dikey olduğunu görürsünüz:

[![Kılavuz yatay kaydırma fotoğraflarla uygulamasının örnek ekran görüntüsü](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Tek satırlık bir kod değiştirerek olduğu fotoğraf görüntüleme uygulamayı farklı bir düzene farklı bir davranış kullanmak üzere değiştirmek mümkündür.
Düzen stilini değiştirmek için değiştirilmesi süredir bağdaştırıcısı kod ne XML düzenini olduğunu dikkat edin. 

Bir sonraki konu başlığında [RecyclerView örneği genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md), öğe tıklama olayları işlemek ve güncelleştirmek için bu basit örnek uygulama Genişletilmiş `RecyclerView` ne zaman temel alınan veri kaynağının değişiklikler.



## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerViewer (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView bölümleri ve İşlevler](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView örneği genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
