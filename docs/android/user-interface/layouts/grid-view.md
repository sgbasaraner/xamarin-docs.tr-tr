---
title: GridView
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c28296af43f0091443eda0364fc0c28a938a7760
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) olan bir [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) iki boyutlu, kaydırılabilir kılavuzda öğeleri görüntüler. Düzen kullanmaya kılavuz öğeleri otomatik olarak eklenen bir [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).

Bu öğreticide, görüntü küçük resimleri oluşan bir kılavuz oluşturacaksınız. Bir öğe seçildiğinde, bir bildirim iletisi resmin konumunu görüntüler.

Adlı yeni bir proje başlatın **HelloGridView**.

Kullanmak için gibi bazı fotoğrafları bulma veya [Bu örnek resimleri yüklemek](http://developer.android.com/shareables/sample_images.zip). Projenin resim dosyalarını eklemek **kaynakları/Drawable** dizini. İçinde **özellikleri** penceresindeki için her yapı eylemi ayarlayın **AndroidResource**.

Açık **Resources/Layout/Main.axml** dosya ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

Bu [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) tüm ekranı doldurur. Bunun yerine kendi açıklama öznitelikleridir. Geçerli öznitelikleri hakkında daha fazla bilgi için bkz: [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) başvuru.

Açık `HelloGridView.cs` ve aşağıdaki kodu ekleyin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Sonra **Main.axml** düzeni içerik görünümünü ayarlanmış [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) ile düzeninden yakalanan [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Özelliği özel bağdaştırıcı ayarlamak için kullanılan sonra (`ImageAdapter`) ızgarada gösterilecek tüm öğeler için kaynak olarak. `ImageAdapter` Sonraki adımda oluşturulur.

Kılavuz içinde bir öğe tıklatıldığında bir şey yapmak için anonim bir temsilci abone olduğu [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) olay.
Bunu gösterir bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) dizin konumunu (sıfır tabanlı) seçili öğe görüntüleyen (gerçek dünya senaryoda konumu başka bir görev için tam boyutlu görüntüyü almak için kullanılabilir). Java stil dinleyicisi sınıfları .NET olayları yerine kullanılabileceğini unutmayın.

Adlı yeni bir sınıf oluşturmak `ImageAdapter` o alt sınıfların [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

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
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

İlk olarak, bu devralınan bazı gerekli yöntemlerini uygular [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/). Yapıcı ve [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) özellik değerleridir. Normalde, [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/) gerçek nesne bağdaştırıcı belirtilen konumda döndürmesi gerekir, ancak bu örnek için göz ardı edilir. Benzer şekilde, [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/) öğesinin satır kimliği dönmesi gerekir, ancak onu buraya gerekli değildir.

İlk yöntem gerekli [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/).
Bu yöntem yeni bir oluşturur [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) eklenen her görüntüsünün `ImageAdapter`. Bu çağrıldığında bir [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) geçirilen içinde olduğu normalde geri dönüştürüldüğünde bir nesne (en az bu kez çağrıldıktan sonra), bu yüzden nesne null olup olmadığını denetler. Varsa, *olan* null bir [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) örneği ve görüntü sunu için istenen özelliklere sahip yapılandırılır:

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) Görünüm için yüksekliğini ve genişliğini ayarlar&mdash;bu boyutunu drawable olsun, her görüntü yeniden boyutlandırılabilir ve uygun şekilde bu boyutlar sığdırmak için kırpılmış, sağlar.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) görüntüleri merkezine (gerekirse) kırpılmış olduğunu bildirir.

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) tüm kenarlara doldurma tanımlar. (Görüntüleri farklı en boy oranlarına varsa, ardından daha az doldurma ImageView verilen boyutları eşleşmiyorsa daha fazla görüntüsü kırpma için neden olur, unutmayın.)

Varsa [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) geçirilen [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) olan *değil* null sonra yerel [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) ile başlatıldı geri dönüştürüldüğünde [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) nesnesi.

Sonunda [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) yöntemi, `position` görüntüden seçmek için kullanılan yönteme geçirilen tamsayı `thumbIds` için görüntü kaynağı olarak ayarlanmış olan dizi [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

Kalan tek şey tanımlamak için `thumbIds` drawable kaynakları dizisi.

Uygulamayı çalıştırın. Kılavuz düzeni aşağıdakine benzer görünmelidir:

[![Örnek ekran görüntüsü GridView 15 görüntüleri görüntüleme](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png)

İle davranışlarını gözlemleyin [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) ve [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) özelliklerini ayarlayarak öğeleri. Örneğin, kullanmak yerine [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) kullanmayı deneyin [ `SetAdjustViewBounds()` ](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/).

<a name="References" />

## <a name="references"></a>Referanslar

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).
