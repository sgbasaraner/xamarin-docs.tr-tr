---
title: "RecyclerView örnek genişletme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: de4683ca660224aa3cf17398ac649086b7e4ad88
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView örnek genişletme


Temel uygulama açıklanan [A temel RecyclerView örnek](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) gerçekten çok yapmaz &ndash; sadece kaydırır ve sabit gözatma kolaylaştırmak için fotoğraf öğeleri listesini görüntüler. Gerçek dünya uygulamalarda kullanıcılar görünen öğeleri dokunarak uygulamayla etkileşim kurabilmesi bekler. Ayrıca, veri kaynağındaki değiştirebilirsiniz (veya uygulama tarafından değiştirilmesi) ve görüntü içeriğini bu değişikliklerle tutarlı kalması gerekir. Aşağıdaki bölümlerde, öğeyi tıklatın olayları işlemek ve güncelleştirmek nasıl öğreneceksiniz `RecyclerView` zaman temel alınan veri kaynağı değişiklikleri.

<a name="itemclick" />

### <a name="handling-item-click-events"></a>Öğe tıklama olaylarını işleme

Bir kullanıcı bir öğeyi zaman dokunur `RecyclerView`, bir öğeyi tıklatın olayı hangi öğesi işlemdeki uygulama bildirmek için oluşturulur. Bu olay tarafından oluşturulmaz `RecyclerView` &ndash; bunun yerine, (Görünüm sahibi paketlenir) öğesi görünümü rötuşları algılar ve tıklama olayları gibi bu rötuşları bildirir.

Öğe tıklama olaylarını nasıl ele alınacağını göstermek için kullanıcı tarafından hangi fotoğraf işlemdeki rapora temel fotoğraf görüntüleme uygulama nasıl değiştirilir aşağıdaki adımlarda açıklanmaktadır. Örnek uygulamasında bir öğeyi tıklatın olay ortaya çıktığında, aşağıdakiler gerçekleşir:

1.  Fotoğraf 's `CardView` öğesi click olayını algılar ve bağdaştırıcı bildirir.

2.  Bağdaştırıcı (bilgilerle öğe konumu) olay etkinliğin öğeyi tıklatın işleyicisine iletir.

3.  Etkinliğin öğesi tıklatma işleyicisini öğeyi tıklatın olaya yanıt verir.

İlk olarak, bir olay işleyicisi üye adlı `ItemClick` eklenen `PhotoAlbumAdapter` sınıf tanımı:

```csharp
public event EventHandler<int> ItemClick;
```

Ardından, bir öğeyi tıklatın olay işleyicisi yöntemi eklenir `MainActivity`.
Bu işleyici kısaca hangi fotoğraf öğesinin işlemdeki belirten bir bildirim görüntüler:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Ardından, bir kod satırı kaydetmek için gereken `OnItemClick` işleyicisi ile `PhotoAlbumAdapter`. Bunu yapmak için bir hemen sonra yerdir `PhotoAlbumAdapter` oluşturulur (ana etkinliğin içinde `OnCreate` yöntemi):

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

`PhotoAlbumAdapter` Şimdi çağıracak `OnItemClick` aldığı ne zaman bir öğeyi tıklatın olay. ' Deki bu başlatır bağdaştırıcı bir işleyici oluşturmak için sonraki adımdır `ItemClick` olay. Aşağıdaki yöntem `OnClick`, hemen bağdaştırıcının sonra eklenen `ItemCount` yöntemi:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Bu `OnClick` yöntemidir bağdaştırıcının *dinleyici* öğesi tıklama olaylarını öğesi görünümlerinden için. Bu dinleyici (aracılığıyla öğesi görünümün görünüm sahibi), bir öğe görünümüyle kaydedilmeden önce `PhotoViewHolder` Oluşturucusu değiştirilmiş, bu yöntem ek bağımsız değişken olarak kabul edin ve kaydetmek için `OnClick` öğesi görünümü ile `Click` olay.
İşte değiştirilmiş `PhotoViewHolder` Oluşturucusu:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` Parametresi için bir başvuru içeriyor `CardView` kullanıcı tarafından işlemdeki. Görünüm sahibi temel sınıfı öğenin yerleşim konumunu bilir unutmayın (`CardView`) temsil ettiği (aracılığıyla `LayoutPosition` özelliği), ve bu konum bağdaştırıcının geçirilen `OnClick` bir öğeyi tıklatın olay gerçekleştiğinde yöntemi. Bağdaştırıcının `OnCreateViewHolder` yöntemi değiştiren bağdaştırıcının geçirmek için `OnClick` görünüm sahibinin oluşturucuya yöntemi:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Şimdi, yapı ve örnek fotoğraf görüntüleme uygulama çalıştırıldığında görüntü fotoğrafı dokunarak hangi fotoğraf işlemdeki raporların görünmesi bir bildirim neden olur:

[ ![Fotoğraf Kartı görünür örnek bildirim dokunduğunuz](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png)

Bu örnek ile olay işleyicileri uygulamak için yalnızca bir yaklaşım gösterir `RecyclerView`. Burada kullanılabilecek başka bir görünüm tutucu olayları yerleştirin ve bu olaylara abone bağdaştırıcıya sahip bir yaklaşımdır. Örnek fotoğraf uygulaması bir fotoğraf yetenek düzenleme verdiyse, ayrı olayları için gerekli olacak `ImageView` ve `TextView` her `CardView`: dokunur `TextView` başlatılan bir `EditView` Düzenle kullanıcı sağlayan iletişim Başlık ve üzerinde rötuşları `ImageView` kırpma veya fotoğrafı döndürmek kullanıcı olanak sağlayan bir Fotoğraf Rötuş aracını başlatmak. Uygulamanızın gereksinimlerine bağlı olarak, işleme ve dokunma olayları yanıtlama için en iyi yaklaşımı tasarlamanız gerekir.

Göstermek için nasıl `RecyclerView` rastgele veri kaynağında bir fotoğraf seçin ve ilk fotoğrafı ile takas için veri kümesi değişikliklerin, örnek fotoğraf görüntüleme uygulaması değiştirilebilir olduğunda güncelleştirilebilir. İlk olarak, bir **rastgele çekme** düğmesi örnek fotoğraf uygulamanın için eklenen **Main.axml** Düzen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Ardından, kod ana etkinliğin sonuna eklenir `OnCreate` bulmaya yöntemi `Random Pick` düzende düğmesine tıklayın ve bunu için bir işleyici ekleyin:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

Fotoğraf albüm Bu işleyici çağırması `RandomSwap` yöntemi zaman **rastgele çekme** düğmesi dokunduğunuz. `RandomSwap` Yöntemi rastgele bir fotoğraf veri kaynağındaki ilk fotoğrafı ile değiştirir ve rastgele takas fotoğraf dizinini döndürür. Derleme ve şu kodla örnek uygulama çalıştırıldığında dokunarak **rastgele çekme** düğmesi neden olmaz görüntü değişikliği nedeniyle `RecyclerView` veri kaynağı bir değişiklik farkında değildir.

Tutmak için `RecyclerView` değişiklikler, veri kaynağı sonra güncelleştirilmiş **rastgele çekme** tıklatın işleyici değiştiren, bağdaştırıcının çağırmak için `NotifyItemChanged` yöntemi değişti koleksiyondaki her öğe için (Bu durumda, iki öğeleriniz Değiştirilen: ilk fotoğraf ve takas fotoğraf). Bu neden `RecyclerView` böylece yeni veri kaynağı durumuyla tutarlı görünümünü güncelleştirmek için:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

Şimdi, ne zaman **rastgele çekme** düğmesi dokunduğunuz, `RecyclerView` fotoğraf daha aşağı koleksiyonda koleksiyondaki ilk fotoğrafı ile takas olduğunu göstermek için ekranı güncelleştirir:

[ ![Takas, ikinci ekran takas sonra önce ilk ekran görüntüsü](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png)

Elbette, `NotifyDataSetChanged` iki aramalarına yerine çağrılmış `NotifyItemChanged`, ancak bunu yapmak böylece zorlamak `RecyclerView` koleksiyondaki yalnızca iki öğe değiştirilmiş olsa bile tüm koleksiyon yenilenecek. Çağırma `NotifyItemChanged` arama daha önemli ölçüde daha etkilidir `NotifyDataSetChanged`.


## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerViewer (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView bölümleri ve İşlevler](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Temel bir RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
