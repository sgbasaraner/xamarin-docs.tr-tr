---
title: RecyclerView örneği genişletme
description: RecyclerView örneği uygulamaya öğesi tıklama olay işleyicileri ekleme.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 73c14e76a4a65c73c5fe0cc3d43329a9f4965c74
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038527"
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView örneği genişletme


Temel uygulama açıklanan [bir temel RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) gerçekten çok işe yaramaz &ndash; yalnızca kaydırılır ve fotoğraf öğeleri gözatma kolaylaştırmak için sabit bir listesini görüntüler. Gerçek dünyadaki uygulamalarda kullanıcılar öğelerinde görünen dokunarak uygulamayla etkileşim kurmak erişebilmeyi beklerler. Ayrıca, temel alınan veri kaynağına değiştirebilirsiniz (veya uygulama tarafından değiştirilmesi) ve görüntü içeriğini bu değişikliklerle tutarlı kalması gerekir. Aşağıdaki bölümlerde, öğe tıklama olayları işlemek ve güncelleştirme hakkında bilgi edineceksiniz `RecyclerView` ne zaman temel alınan veri kaynağının değişiklikler.


### <a name="handling-item-click-events"></a>Öğe tıklama olayları işleme

Bir kullanıcı bir öğeyi zaman geliştirmelere değinmektedir `RecyclerView`, hangi öğe dokunulan bildirmek bir öğe tıklama olayı oluşturulur. Bu olay tarafından oluşturulmaz `RecyclerView` &ndash; (Görünüm sahibi içinde sarmalanmış) öğesi görünümü bunun yerine, değişiklikleri algılar ve tıklama olayları gibi bu değişiklikleri bildirir.

Öğe tıklama olayları işlemek nasıl göstermek için aşağıdaki adımlar, kullanıcı tarafından hangi fotoğraf dokunulan rapora temel fotoğraf görüntüleme uygulama nasıl değiştirilir açıklamaktadır. Örnek uygulamada bir öğe tıklama olayı ortaya çıktığında, aşağıdaki sırayla gerçekleşir:

1.  Fotoğraf'ın `CardView` öğesi click olayı algılar ve bağdaştırıcı bildirir.

2.  Bağdaştırıcı için etkinliğin öğesi tıklama işleyicisi olay (ile öğesi konum bilgileri) iletir.

3.  Etkinliğin öğesi tıklama işleyicisi öğesi tıklama olayı yanıtlar.

İlk olarak, bir olay işleyicisi üye adı `ItemClick` eklenir `PhotoAlbumAdapter` sınıf tanımını:

```csharp
public event EventHandler<int> ItemClick;
```

Ardından, bir öğe tıklama olay işleyicisi yöntemi eklenir `MainActivity`.
Bu işleyici kısaca hangi fotoğraf öğesi dokunulan belirten bir bildirim görüntüler:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Ardından, kaydetmek için bir kod satırı gereken `OnItemClick` işleyicisi `PhotoAlbumAdapter`. Bunu yapmak için uygun bir hemen sonra yerdir `PhotoAlbumAdapter` oluşturulur: 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

Bu temel örnekte ana etkinliğin yerinde işleyici kaydı alır `OnCreate` yöntemi, ancak bir üretim uygulaması işleyicisinde kaydetme `OnResume` ve içinde kaydını `OnPause` &ndash; bkz [etkinlik yaşam döngüsü ](~/android/app-fundamentals/activity-lifecycle/index.md) daha fazla bilgi için.

`PhotoAlbumAdapter` artık çağıran `OnItemClick` aldığında bir öğe click olayı. Sonraki adımda bu yükselten bağdaştırıcıda bir işleyici oluşturmaktır `ItemClick` olay. Aşağıdaki yöntem `OnClick`, hemen bağdaştırıcının sonra eklenen `ItemCount` yöntemi:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Bu `OnClick` yöntemidir bağdaştırıcının *dinleyici* öğesi görünümlerinden öğesi tıklama olayları. Bir öğesi görünümü (aracılığıyla öğesi görünümün görünüm sahibi), bu dinleyici kaydedilmeden önce `PhotoViewHolder` Oluşturucusu değiştirilmiş, bu yöntem ek bağımsız değişken olarak kabul edin ve kaydetmek için `OnClick` öğesi görünümü `Click` olay.
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

`itemView` Parametresi içeren bir başvuru `CardView` kullanıcı tarafından dokunulan. Temel sınıf görünümü sahibi öğesinin Düzen konumuna bilir unutmayın (`CardView`) temsil ettiği (aracılığıyla `LayoutPosition` özelliği), ve bu konum bağdaştırıcının geçirilir `OnClick` öğesi tıklama olay gerçekleştiğinde yöntemi. Bağdaştırıcının `OnCreateViewHolder` yöntemi değiştirilmişse bağdaştırıcının geçirilecek `OnClick` görünümü sahibinin oluşturucusuna yöntemi:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Şimdi oluşturun ve örnek fotoğraf görüntüleme uygulamayı çalıştırın, dokunarak bir fotoğraf görüntüdeki hangi fotoğraf dokunulan raporların görünmesi bir bildirim neden olur:

[![Fotoğraf kart görünür örnek bildirim dokunulduğunda](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Bu örnek olay işleyicileri ile uygulamak için yalnızca bir yaklaşımı gösterir `RecyclerView`. Burada kullanılabilen başka bir olay görünümü tutucu yerleştirin ve bu olaylara abone bağdaştırıcıya sahip yaklaşımdır. Örnek fotoğraf uygulaması bir fotoğraf düzenleme yeteneğini sağlanırsa, ayrı olayları için gerekli olacaktır `ImageView` ve `TextView` her `CardView`: dokunduğu `TextView` başlatılan bir `EditView` düzenleme kullanıcı sağlayan iletişim açıklamalı alt yazı ve üzerinde dokunmalar `ImageView` kırpma veya fotoğrafı döndürmek kullanıcının sağlayan bir Fotoğraf Rötuş aracını başlatmak. Uygulamanızın ihtiyaçlarına bağlı olarak, dokunma olayları yanıtlama ve işleme için en iyi yaklaşım tasarlamanız gerekir.

Göstermek için nasıl `RecyclerView` rastgele veri kaynağında bir fotoğraf çekme ve ilk fotoğrafı ile takas için veri kümesi değişikliklerin, örnek fotoğraf görüntüleme uygulama değiştirilebilir olduğunda güncelleştirilebilir. İlk olarak, bir **rastgele çekme** düğmesi örnek fotoğraf uygulamanın eklenen **Main.axml** düzeni:

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

Ardından, kod ana etkinliğin sona eklenen `OnCreate` bulunacak yöntemi `Random Pick` düzende düğme ve bir işleyici ekler:

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

Fotoğraf albümü'nın bu işleyici çağırır `RandomSwap` yöntemi zaman **rastgele çekme** düğmeye dokunulduğunda. `RandomSwap` Yöntemi rasgele bir fotoğraf veri kaynağındaki ilk fotoğraf ile değiştirir ve ardından rastgele takas fotoğraf dizinini döndürür. Derlemek ve örnek uygulamayı şu kodla çalıştırmak, dokunarak **rastgele çekme** düğmesi neden olmaz görüntü değişikliği nedeniyle `RecyclerView` değişikliği veri kaynağına farkında değil.

Tutmak `RecyclerView` değişiklikleri veri kaynağı sonra güncelleştirilmiş **rastgele çekme** tıklayın işleyicisini değiştirme, bağdaştırıcının çağrılacak `NotifyItemChanged` yöntemi değişti koleksiyondaki her öğe için (Bu durumda, iki öğe sahip Değiştirilen: ilk fotoğraf ve takas fotoğraf). Bu neden `RecyclerView` yeni veri kaynağı durumuyla tutarlı olması görünümünü güncelleştirmek için:

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

Şimdi, **rastgele çekme** düğmeye dokunulduğunda, `RecyclerView` fotoğraf daha aşağı koleksiyonda koleksiyondaki ilk fotoğraf ile takas olduğunu göstermek güncelleştirir:

[![Takas, ikinci ekran görüntüsüne takas sonra önce ilk ekran görüntüsü](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Elbette, `NotifyDataSetChanged` iki aramaların yerine çağrılmış olabilir `NotifyItemChanged`, ancak bunun yapılması böylece zorlamak `RecyclerView` yalnızca iki öğe koleksiyonundaki değiştirilmiş olsa bile tüm koleksiyon yenilemek için. Çağırma `NotifyItemChanged` arama daha önemli ölçüde daha etkilidir `NotifyDataSetChanged`.


## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerViewer (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView bölümleri ve İşlevler](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Basit bir RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
