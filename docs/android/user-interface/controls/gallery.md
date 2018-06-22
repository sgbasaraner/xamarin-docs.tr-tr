---
title: Galerisi
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: cb05dfb79c00e9c6f9fa1a8d80627abdb911c36e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765655"
---
# <a name="gallery"></a>Galerisi

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) Yatay kaydırma listedeki öğeleri görüntülemek için kullanılan bir düzen pencere öğesi olduğundan ve geçerli seçim görünümü merkezinde yerleştirir.

> [!IMPORTANT]
> Bu pencere öğesi, Android 4.1 (API düzeyi 16) kullanımdan kaldırılmıştır. 

Bu öğreticide, Fotoğraf Galerisi oluşturun ve ardından bir galeri öğesi seçili her zaman bir bildirim iletisi görüntüler.

Sonra `Main.axml` düzeni içerik görünümünü ayarlanmış `Gallery` ile düzeninden yakalanan [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
[ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Özelliği özel bağdaştırıcı ayarlamak için kullanılan sonra ( `ImageAdapter`) dallery görüntülenecek tüm öğeler için kaynak olarak. `ImageAdapter` Sonraki adımda oluşturulur.

Galerideki bir öğe tıklatıldığında bir şey yapmak için anonim bir temsilci abone olduğu [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) olay. Bunu gösteren bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Columns'u öğesinin (sıfır tabanlı) dizin konumunu görüntüler (gerçek dünya senaryoda konumu başka bir görev için tam boyutlu görüntüyü almak için kullanılabilir).

İlk olarak, bir dizi drawable kaynakları dizinine görüntüleri başvuru kimlikleri de dahil olmak üzere birkaç üye değişkenleri vardır (**kaynakları/drawable**).

Sonraki sınıfı oluşturucusu olduğu yere [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) için bir `ImageAdapter` örneği tanımlı ve yerel bir alana kaydedilir.
Ardından, bu devralınan bazı gerekli yöntemlerini uygular [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).
Yapıcı ve [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) özellik değerleridir. Normalde, [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/) gerçek nesne bağdaştırıcı belirtilen konumda döndürmesi gerekir, ancak bu örnek için göz ardı edilir. Benzer şekilde, [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/) öğesinin satır kimliği dönmesi gerekir, ancak onu buraya gerekli değildir.

Yöntemin bir görüntüye uygulanacak çalışır bir [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) , katıştırılmış içinde [ `Gallery` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) Bu yöntemde, üye [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) olduğu Yeni bir oluşturmak için kullanılan [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).
[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) Drawable kaynaklar, ayarı yerel dizisinden bir görüntü uygulayarak hazırlanır [ `Gallery.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/) sığdırmakiçinÖlçeklendirayarıgörüntüsügenişliğiveyüksekliği[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) boyutları ve son olarak oluşturucuda edinilen styleable özniteliğini kullanmak için arka plan ayarlama.

Bkz: [ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/) ölçeklendirme seçenekleri diğer görüntü için.

## <a name="walkthrough"></a>İzlenecek yol

Adlı yeni bir proje başlatın *HelloGallery*.

[![Yeni bir çözüm iletişim kutusunda yeni Android projesi ekran görüntüsü](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

Kullanmak için gibi bazı fotoğrafları bulma veya [Bu örnek resimleri yüklemek](http://developer.android.com/shareables/sample_images.zip).
Projenin resim dosyalarını eklemek **kaynakları/Drawable** dizini. İçinde **özellikleri** penceresindeki için her yapı eylemi ayarlayın **AndroidResource**.

Açık **Resources/Layout/Main.axml** ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Açık `MainActivity.cs` ve aşağıdaki kodu ekleyin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Adlı yeni bir sınıf oluşturmak `ImageAdapter` o alt sınıfların [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

    public override Java.Lang.Object GetItem (int position)
    {
          return null;
    }

    public override long GetItemId (int position)
    {
          return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

Uygulamayı çalıştırın. Aşağıdaki ekran görüntüsüne görünmelidir:

![Örnek görüntüleri görüntüleme HelloGallery ekran görüntüsü](gallery-images/hellogallery3.png)



## <a name="references"></a>Referanslar

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).


