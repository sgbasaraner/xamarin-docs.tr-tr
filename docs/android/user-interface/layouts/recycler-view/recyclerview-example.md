---
title: Temel bir RecyclerView örneği
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d5be838dcb5530ece76c3701d8fce10403622e8d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770618"
---
# <a name="a-basic-recyclerview-example"></a>Temel bir RecyclerView örneği


Anlamak için nasıl `RecyclerView` bir genel uygulama çalışır, bu konuda ele [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) örnek uygulaması, kullanan basit bir kod örneğidir `RecyclerView` fotoğraf büyük koleksiyonu görüntülemek için: 

[![Fotoğraf görüntülenecek CardViews kullanan RecyclerView uygulamasının iki ekran görüntüleri](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** kullanan [kart görünümü](~/android/user-interface/controls/card-view.md) her fotoğraf öğesini uygulamak için `RecyclerView` düzeni. Nedeniyle `RecyclerView`'s performans avantajı, bu örnek uygulama hızla kaydırın ve belirgin gecikmeler olmadan büyük bir fotoğraf toplulukta mümkün.


### <a name="an-example-data-source"></a>Bir örnek veri kaynağı

Bu örnek uygulamasında, bir "fotoğraf albümü" veri kaynağı (tarafından temsil edilen `PhotoAlbum` sınıfı) sağlayan `RecyclerView` öğesi içeriğe sahip.
`PhotoAlbum` açıklamalı alt fotoğraf koleksiyonudur; Bu örneği, 32 fotoğraf hazır koleksiyonu alın:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Her fotoğraf örnek `PhotoAlbum` kendi görüntü kaynak kimliği okumasına olanak tanıyan özellikler sunar `PhotoID`ve kendi başlık dizesi `Caption`. Fotoğraf koleksiyonunu her fotoğraf bir dizin oluşturucu tarafından erişilebilecek şekilde düzenlenmiştir. Örneğin, aşağıdaki kod satırlarını görüntü kaynak kimliği ile onuncu fotoğraf koleksiyondaki için resim yazısı erişebilirsiniz:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` Ayrıca sağlayan bir `RandomSwap` koleksiyondaki ilk fotoğraf başka bir yerde koleksiyonundaki rastgele seçilmiş fotoğraf ile değiştirilecek çağırabilirsiniz yöntemi:

```csharp
mPhotoAlbum.RandomSwap ();
```

Çünkü uygulama ayrıntılarını `PhotoAlbum` anlamak için ilgili olmayan `RecyclerView`, `PhotoAlbum` kaynak kodu değil sunulur burada. Kaynak koduna `PhotoAlbum` şu adresten edinilebilir [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) içinde [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) örnek uygulama.


### <a name="layout-and-initialization"></a>Düzen ve başlatma

Düzen dosyasını **Main.axml**, bir tek oluşur `RecyclerView` içinde bir `LinearLayout`:

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

Tam adı kullanmalıdır Not **android.support.v7.widget.RecyclerView** çünkü `RecyclerView` destek Kitaplığı'nda paketlenmiştir. `OnCreate` Yöntemi `MainActivity` bu düzeni başlatır, bağdaştırıcı oluşturur ve veri kaynağındaki hazırlar:

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

2. Fotoğraf albümü veri kaynağı bağdaştırıcısı oluşturucuya geçirir `PhotoAlbumAdapter` (tanımlanmış bu kılavuzda). 
   Veri kaynağı bağdaştırıcısı oluşturucusu için parametre olarak geçirmek için en iyi yöntem kabul edilir unutmayın. 

3. Alır `RecyclerView` düzeninden.

4. Bağdaştırıcı takılan `RecyclerView` çağırarak örneği `RecyclerView` `SetAdapter` yukarıda gösterildiği gibi yöntemi.

### <a name="layout-manager"></a>Düzen Yöneticisi

Her öğe `RecyclerView` oluşan bir `CardView` bir fotoğraf görüntü ve fotoğraf resim yazısını içeren (ayrıntıları ele alınmıştır [görünüm sahibi](#view-holder) bölümüne bakın). Önceden tanımlanmış `LinearLayoutManager` her düzenlemek için kullanılan `CardView` dikey bir kaydırma düzenleme içinde:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Bu kod ana etkinliğin içinde bulunduğu `OnCreate` yöntemi. Düzen Yöneticisi oluşturucuya gerektiren bir *bağlamı*, böylece `MainActivity` kullanılarak gönderilir `this` yukarıda görülen.

Yerine predefind `LinearLayoutManager`, iki görüntüleyen bir özel düzen Yöneticisi'nde takın `CardView` öğeleri fotoğraf toplulukta gezinmesine page-turning animasyon efekti uygulama yana,. Bu kılavuzda daha sonra farklı düzen Yöneticisi'nde takas tarafından düzenini değiştirmek nasıl bir örnek görürsünüz.

<a name="view-holder" />

### <a name="view-holder"></a>Görünüm sahibi

Görünüm tutucu sınıfı adlı `PhotoViewHolder`. Her `PhotoViewHolder` örneğini tutan başvurular `ImageView` ve `TextView` içinde düzenlendiği ilgili satır öğenin bir `CardView` burada tasarımını olarak:

[![Bir ImageView ve kutusu TextView içeren kart görünümü diyagramı](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` türetilen `RecyclerView.ViewHolder` ve başvurularını depolamak için Özellikler içeren `ImageView` ve `TextView` yukarıdaki düzende gösterilir.
`PhotoViewHolder` iki özellikleri ve bir oluşturucu oluşur:

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
Bu kod örneğinde, `PhotoViewHolder` Oluşturucusu üst öğesi görünümü başvuru geçirilen ( `CardView`), `PhotoViewHolder` sarmalar. Üst her zaman temel oluşturucuyu öğesi görünüme iletmek unutmayın. `PhotoViewHolder` Oluşturucu çağrıları `FindViewById` her biri kendi alt görünüm başvuruları bulmak için üst öğe görünümde `ImageView` ve `TextView`, sonuçları şurada depolayarak `Image` ve `Caption` özellikleri, sırasıyla. Bu güncelleştirmeleri bağdaştırıcısı görünüm başvuruları daha sonra bu özellikleri alır. `CardView`ait alt görünümleri yeni verilerle.

Hakkında daha fazla bilgi için `RecyclerView.ViewHolder`, bkz: [RecyclerView.ViewHolder sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Bağdaştırıcı

Her bağdaştırıcı yükler `RecyclerView` belirli bir fotoğraf için veri satırı. Belirli bir fotoğraf satır konumunda için *P*, örneğin, bağdaştırıcı konumunda ilişkili verileri bulur *P* kopyalar ve veri kaynağı içinde satır bu veri öğesi konumunda *P* içinde `RecyclerView` koleksiyonu. Başvurularını aramak için Görünüm sahibi bağdaştırıcısı kullanır `ImageView` ve `TextView` tekrar tekrar çağırmak yok şekilde bu konumda `FindViewById` olanlar görünümleri için kullanıcı fotoğrafı koleksiyonu ile birlikte kayar ve görünümleri yeniden kullanır.

İçinde **RecyclerViewer**, bir bağdaştırıcı sınıfı türetilir `RecyclerView.Adapter` oluşturmak için `PhotoAlbumAdapter`:

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

`mPhotoAlbum` Üyeyi içeren oluşturucuya geçirilen veri kaynağı (fotoğraf albümü); Bu üye değişkenine fotoğraf albümü Oluşturucusu kopyalar. Aşağıdaki gerekli `RecyclerView.Adapter` yöntemleri uygulanır:

-   **`OnCreateViewHolder`** &ndash; Öğe düzeni dosya ve görünüm sahibi başlatır.

-   **`OnBindViewHolder`** &ndash; Belirli bir konumda veri içinde belirli görünüm sahibi olan başvurular depolanan görünümleri yükler.

-   **`ItemCount`** &ndash; Veri kaynağında öğe sayısını döndürür.

İçindeki öğeleri konumlandırma sırasında düzen Yöneticisi bu yöntemleri çağırır `RecyclerView`. Aşağıdaki bölümlerde bu yöntemleri uyarlamasını incelenir.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Düzen Yöneticisi çağrıları `OnCreateViewHolder` zaman `RecyclerView` bir öğeyi temsil edecek yeni bir görünüm sahibi gerekiyor. `OnCreateViewHolder` görünümün düzenini dosya öğesi görünümünden Şişir ve yeni bir görünümde sarmalar `PhotoViewHolder` örneği. `PhotoViewHolder` Oluşturucusu bulur ve daha önce açıklandığı gibi alt görünümleri başvurular düzende depolar [görünüm sahibi](#view-holder).

Her satır öğesi tarafından temsil edilen bir `CardView` içeren bir `ImageView` (için fotoğraf) ve bir `TextView` (için resim yazısı). Bu düzen dosyasında bulunan **PhotoCardView.axml**:

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

Bu düzen bir tek satır öğeyi temsil eden `RecyclerView`. `OnBindViewHolder` Yöntemi (aşağıda açıklanmıştır) veri kaynağından verileri kopyalar `ImageView` ve `TextView` bu düzeni.
`OnCreateViewHolder` Bu düzeni verilen fotoğraf konumu Şişir `RecyclerView` ve yeni bir örneğini oluşturur `PhotoViewHolder` örneği (bulur ve başvurularını önbelleğe `ImageView` ve `TextView` ilişkili alt görünümlerde `CardView` düzeni):

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

Elde edilen görünüm sahibi örnek `vh`, geri çağıran (Düzen Yöneticisi) döndürülür.


#### <a name="onbindviewholder"></a>OnBindViewHolder

Düzen Yöneticisi olduğunda belirli bir görünümde görüntülemek için hazır `RecyclerView`'s görünür ekran alanının bağdaştırıcının çağırır `OnBindViewHolder` veri kaynağından alınan içerikle belirtilen satır konumunda öğe doldurmak için yöntem. `OnBindViewHolder` Belirtilen satır konumu (fotoğrafın görüntü kaynağı ve dize fotoğrafın resim yazısı) için fotoğraf bilgilerini alır ve bu verileri ilişkili görünümlerine kopyalar. Görünüm tutucu nesnesindeki başvuruları aracılığıyla görünümleri konumlandırıldığını (hangi geçirilir aracılığıyla `holder` parametresi):

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

Geçilen görünüm sahibi nesnesi ilk türetilmiş görünüm sahibi türe gerekir (Bu durumda, `PhotoViewHolder`), kullanılmadan önce.
Görüntü kaynağı bağdaştırıcısı görünüm sahibinin tarafından başvurulan görünüm yükler `Image` özelliği ve görünüm sahibinin tarafından başvurulan görünümüne başlık metnini kopyalar `Caption` özelliği. Bu *bağlar* verileri ile ilişkili görünüm.

Dikkat `OnBindViewHolder` doğrudan veri yapısı ile ilgilenir kodudur. Bu durumda, `OnBindViewHolder` nasıl eşleneceğini anlar `RecyclerView` öğesi kendi ilişkili veri öğesi bir konuma veri kaynağındaki. Eşleme bu durumda, çünkü konumu bir dizi dizini fotoğraf albümü kullanılabilir açıktır; Ancak, daha karmaşık veri kaynakları gibi bir eşleme oluşturmak için ek kod gerektirebilir.


#### <a name="itemcount"></a>ItemCount

`ItemCount` Yöntemi, veri toplama öğe sayısını döndürür. Örnek Fotoğraf Görüntüleyicisi uygulamada öğe sayısı fotoğraf albümü fotoğraflardaki sayısıdır:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Hakkında daha fazla bilgi için `RecyclerView.Adapter`, bkz: [RecyclerView.Adapter sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Hepsini birleştirme

Elde edilen `RecyclerView` örnek fotoğraf uygulaması için uygulama oluşur `MainActivity` veri kaynağı, Düzen Yöneticisi ve bağdaştırıcısı oluşturan kod. `MainActivity` oluşturur `mRecyclerView` örneği, veri kaynağı ve bağdaştırıcısı oluşturur ve Düzen Yöneticisi ve bağdaştırıcısı takılan:

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

`PhotoViewHolder` bulur ve görünüm başvurularını önbelleğe alır:

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

`PhotoAlbumAdapter` üç gerekli yöntemi geçersiz kılmaları uygular:

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

Bu kod derlenmiş ve çalıştırmak, aşağıdaki ekran görüntülerinde gösterildiği gibi uygulama görüntüleme temel fotoğraf oluşturur:

[![Fotoğraf dikey fotoğraf kartları kaydırma ile uygulama görüntüleme iki ekran görüntüleri](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Temel bu uygulama yalnızca Fotoğraf albümü gözatma destekler. Öğe dokunma olayları için yanıt vermez veya temel alınan verilerde yapılan değişiklikler işlemiyor. Bu işlev eklenir [RecyclerView örnek genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md).


### <a name="changing-the-layoutmanager"></a>LayoutManager değiştirme

Nedeniyle `RecyclerView`'s esneklik, onu farklı düzen manager kullanmak üzere uygulamayı değiştirmek kolay. Aşağıdaki örnekte, fotoğraf albümü yatay olarak kayar bir kılavuz düzeni yerine dikey doğrusal düzen ile görüntülenecek değiştirilir. Bunu yapmak için Düzen Yöneticisi oluşturmada kullanılacak değiştirilir `GridLayoutManager` gibi:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Bu kod değişikliği dikey değiştirir `LinearLayoutManager` ile bir `GridLayoutManager` yatay yönde kaydırma iki satır oluşan bir kılavuz sunar. Derleme ve uygulamayı yeniden çalıştırın, fotoğraflar kılavuzda görüntülenir ve kaydırma yatay yerine dikey olduğunu görürsünüz:

[![Yatay kaydırma fotoğraf kılavuzda uygulamayla örnek ekran görüntüsü](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Yalnızca bir kod satırı değiştirerek olan farklı bir düzen farklı davranışlar ile kullanmak için fotoğraf görüntüleme uygulamayı değiştirmek mümkündür.
Düzen stilini değiştirmek için değiştirilmesi süredir bağdaştırıcısı kod ne XML düzeni olduğunu dikkat edin. 

Sonraki konusunda [RecyclerView örnek genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md), öğeyi tıklatın olayları işlemek ve güncelleştirmek için bu temel örnek uygulaması Genişletilmiş `RecyclerView` zaman temel alınan veri kaynağı değişiklikleri.



## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerViewer (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView bölümleri ve İşlevler](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView örnek genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
