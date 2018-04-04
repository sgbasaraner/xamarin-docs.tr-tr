---
title: Xamarin.Android Performance
description: Xamarin.Android ile oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır.
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a22190adc97cb80f5900dda4a073bdcdf80ef99b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinandroid-performance"></a>Xamarin.Android Performance

_Xamarin.Android ile oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır._

## <a name="performance-overview"></a>Performans genel bakış

Zayıf uygulama performans kendisini birçok yolla gösterir. Bir uygulama yapabilirsiniz yanıt vermeyen gibi görünebilir, yavaş kaydırma neden olabilir ve pil ömrünün azaltabilir. Ancak, performansı en iyi duruma getirme daha fazlasını verimli kod uygulama içerir. Uygulama performansı kullanıcı deneyimi de dikkate alınmalıdır. Örneğin, diğer etkinlikler gerçekleştirmeyi kullanıcı engellenmeden işlemlerini yürütmek sağlayarak kullanıcı deneyimini geliştirmek için yardımcı olabilir.

Performans ve algılanan, Xamarin.Android ile oluşturulan uygulamaların performansını artırmak için teknikler mevcuttur. Bunlara aşağıdakiler dahildir:

- [Düzen hiyerarşileri en iyi duruma getirme](#optimizelayout)
- [Liste görünümleri en iyi duruma getirme](#optimizelistviews)
- [Olay işleyicileri aktivitelerde Kaldır](#removeeventhandlers)
- [Hizmetleri Sysprep'in sınırla](#limitservices)
- [Bildirildiğinde kaynakları serbest bırakmak](#releaseresources)
- [Kullanıcı arabirimi gizli kaynakları serbest bırakmak](#releaseresourcesuihidden)
- [Görüntü kaynakları en iyi duruma getirme](#optimizeimages)
- [Kullanılmayan görüntü kaynaklarını silmek](#disposeimages)
- [Kayan nokta aritmetik kaçının](#avoidfloats)
- [İletişim kutularını kapatın](#dismissdialogs)


> [!NOTE]
> Bu makalede okumadan önce ilk okumalısınız [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md), bellek kullanımı ve Xamarin platformu kullanılarak oluşturulan uygulamaların performansını artırmak için platform olmayan belirli teknikler açıklanır.

<a name="optimizelayout" />

## <a name="optimize-layout-hierarchies"></a>Düzen hiyerarşileri en iyi duruma getirme

Bir uygulamaya eklenen her düzeni, başlatma, Düzen ve çizim gerektirir. Düzen geçişi iç içe geçme pahalı olabilir [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) örnekleri kullanan `weight` parametresi, her alt iki kez ölçülecek olduğundan. İç içe geçmiş örneklerini kullanarak `LinearLayout` performansın birden çok kez şişirileceğini düzenleri için neden olabilir, ayrıntılı görünüm hiyerarşi için gibi olarak neden olabilecek bir [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/). Bu nedenle, bu tür düzenleri iyileştirilmiş, bir performans avantajları sonra çarpılır önemlidir.

Örneğin, göz önünde bulundurun [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) simge, bir başlığı ve bir açıklama sahip bir liste görünümü satır için. `LinearLayout` İçerecek bir [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) ve dikey `LinearLayout` iki içeren [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) örnekleri:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

Bu Düzen 3 düzey derinlikte ve her biri için şişirileceğini zaman kayıp [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) satır. Ancak, bu yerleşim düzleştirme tarafından aşağıdaki kod örneğinde gösterildiği gibi geliştirilebilir:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

Önceki 3 düzeyi hiyerarşi 2 düzeyli bir hiyerarşiye ve tek bir düşürüldü [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) iki değiştirilmiştir [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) örnekleri. Önemli ölçüde performans artışı düzeni her biri için inflating zaman kazanılacak [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) satır.

<a name="optimizelistviews" />

## <a name="optimize-list-views"></a>Liste görünümleri en iyi duruma getirme

Kullanıcıların beklediğiniz yumuşak kaydırma ve hızlı yükleme süreleri için [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) örnekleri. Ancak, performans kaydırma her liste görünümü satır iç içe görünüm hiyerarşileri içerdiğinde veya liste görünümü satırları karmaşık düzenleri içerdiğinde olumsuz etkilenebilir. Ancak, düşük önlemek için kullanılan teknikleri vardır `ListView` performans:

- Daha fazla bilgi için satır görünümleri yeniden kullanmak için bkz: [satır görünümleri yeniden](#reuserowviews).
- Düzenleri, mümkün olduğunda düzleştirmek.
- Bir web hizmetinden alınan önbellek satır içeriği.
- Resim ölçekleme kaçının.

Bu teknikler kalmasına topluca yardımcı olur [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) sorunsuz kaydırma örnekleri.

<a name="reuserowviews" />

### <a name="reuse-row-views"></a>Satır görünümleri yeniden

Satırlarda yüzlerce görüntülerken bir [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/), yüzlerce oluşturmak için bellek kaybı olacaktır [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) nesneleri yalnızca az sayıda bunları ekranda aynı anda görüntülendiğinde. Bunun yerine, yalnızca `View` nesneleri ekran satırlarda görünür ile belleğe yüklenebilir **içerik** yeniden nesneleri bunları yükleniyor. Örnekleme süresi ve bellek kaydetme ek nesneler yüzlerce engeller.

Bir satır ekranından kaybolduğunda bu nedenle, kendi görünüm yeniden kullanmak üzere, bir kuyruktaki aşağıdaki kod örneğinde gösterildiği gibi yerleştirilebilir:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Kullanıcı kayarken [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) çağrıları `GetView` görüntülemek için yeni görünümler istemek için geçersiz kılma – varsa kullanılmayan bir görünümde geçirir `convertView` parametresi. Bu değer ise `null` yeni bir kod oluşturur sonra [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) örneği, aksi takdirde `convertView` özellikleri sıfırlamak ve yeniden kullanılabilir.

Daha fazla bilgi için bkz: [satır görünümü yeniden kullanma](~/android/user-interface/layouts/list-view/populating.md#row-view-re-use) içinde [ListView verilerle doldurma](~/android/user-interface/layouts/list-view/populating.md).

<a name="removeeventhandlers" />

## <a name="remove-event-handlers-in-activities"></a>Olay işleyicileri aktivitelerde Kaldır

Bir etkinlik Android çalışma zamanında kaldırıldığı zaman hala Mono çalışma zamanı'nda Canlı olabilir. Bu nedenle, dış nesneleri için olay işleyicileri kaldırmanız `Activity.OnPause` çalışma zamanı yok edildi bir etkinlik başvuruyu tutma önlemek için.

Bir etkinlikte olay handler(s) Sınıf düzeyinde bildirin:

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

Ardından etkinlik işleyiciler gibi uygulama `OnResume`:

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

Etkinlik çalışma durumuna çıktığında `OnPause` olarak adlandırılır. İçinde `OnPause` uygulaması, işleyicileri gibi kaldırın:

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices" />

## <a name="limit-the-lifespan-of-services"></a>Hizmetleri Sysprep'in sınırla

Bir hizmeti başladığında, Android çalıştıran hizmet işlemi tutar. Kendi bellek disk belleği, veya değiştirilemez başka bir yerde kullanılan olduğundan bu işlem maliyetli hale getirir. Gerekli değildir, bir hizmeti çalıştıran bırakarak, bu nedenle performansın bellek kısıtlamaları nedeniyle sergileyen bir uygulama riskini artırır. Bu da uygulama geçiş Android önbelleğe işlemlerin sayısını azaltan daha az verimli hale getirebilirsiniz.

Bir hizmetin kullanım ömrü kullanarak sınırlı olabilir bir `IntentService`, hangi sonlandırır kendisini başlatıldığından hedefi işlediği sonra.

<a name="releaseresources" />

## <a name="release-resources-when-notified"></a>Bildirildiğinde kaynakları serbest bırakmak

Uygulama yaşam döngüsü sırasında [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) geri çağırma aygıt bellek düşük olduğunda bir bildirim sağlar. Bu geri çağırma aşağıdaki bellek düzey bildirimleri dinlemek için uygulanması gerekir:

- [`TrimMemoryRunningModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate/) – Uygulama *olabilir* bazı gereksiz kaynakları serbest bırakmak istediğiniz.
- [`TrimMemoryRunningLow`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningLow/) – Uygulama *gereken* yayın gereksiz kaynakları.
- [`TrimMemoryRunningCritical`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical/) – Uygulama *gereken* mümkün olduğunca kadar kritik olmayan işlemler bırakın.

Uygulama işlemi önbelleğe alındığında, ayrıca, aşağıdaki bellek düzeyi bildirimler tarafından alınması [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) geri çağırma:

- [`TrimMemoryBackground`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryBackground/) – kullanıcının uygulamaya döndürürse, hızlı ve verimli bir şekilde yeniden kaynakları serbest.
- [`TrimMemoryModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryModerate/) – Genel daha iyi performans için önbelleğe alınan diğer işlemleri tutmak sistem kaynakları serbest bırakma yardımcı olabilir.
- [`TrimMemoryComplete`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryComplete/) – Daha fazla bellek yakında kurtarılan değil, uygulama işlemini en kısa sürede sona erdirilecek.

Bildirimler için alınan düzeyine bağlı kaynakları bırakarak yanıt.

<a name="releaseresourcesuihidden" />

## <a name="release-resources-when-the-user-interface-is-hidden"></a>Kullanıcı arabirimi gizli kaynakları serbest bırakmak

Kullanıcı başka bir uygulamaya gittiğinde Android'ın kapasite sırayla bir kullanıcı deneyimi kalitesini olumsuz etkileyebilir önbelleğe alınan işlemler için önemli ölçüde artırabilir olarak uygulamanın kullanıcı arabirimi tarafından kullanılan tüm kaynakları serbest bırakır.

Kullanıcı UI çıktığında bir bildirim almak için uygulama [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) geri `Activity` sınıfları ve dinlemek [ `TrimMemoryUiHidden` ](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryUiHidden/) UI gösterir düzeyi gizlenir. Bu bildirim alınan yalnızca *tüm* uygulama kullanıcı Arabirimi bileşenlerini kullanıcıdan gizlenir. Bu bildirim alındığında UI kaynakları serbest bırakma kullanıcı geri etkinlikten başka bir uygulamadaki giderse, kullanıcı Arabirimi kaynakları hızlı bir şekilde etkinlik devam hala kullanılabilir olmasını sağlar.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Görüntü kaynakları en iyi duruma getirme

Görüntüleri uygulamaları kullanan, en pahalı kaynakları bazıları verilmiştir ve genellikle yüksek çözünürlüklerde yakalanır. Bu nedenle, bir resim görüntülerken, cihazın ekran için gerekli çözünürlükte görüntüleyin. Görüntü ekran daha yüksek çözünürlüğe ise, bu ölçeklendirilmelidir.

Daha fazla bilgi için bkz: [görüntü kaynakları en iyi duruma getirme](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) içinde [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md) Kılavuzu.

<a name="disposeimages" />

## <a name="dispose-of-unused-image-resources"></a>Kullanılmayan görüntü kaynaklarını silmek

Bellek kullanımı kaydetmek için artık gerekli olmayan büyük görüntü kaynaklarını silmek için bir fikirdir. Bununla birlikte, görüntüleri doğru elden emin olmak önemlidir. Açık bir kullanmak yerine `.Dispose()` çağrısı, yararlanabilir [kullanarak](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement) doğru kullanımını sağlamak için deyimleri `IDisposable` nesneleri. 

Örneğin, [bit eşlem](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) uygulayan sınıf `IDisposable`. Örnek oluşturma, sarmalama bir `BitMap` nesnesinde bir `using` blok sağlar, biri doğru Çıkışta bloğundan silinecek olduğunu:

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

Tek kullanımlık kaynakları serbest bırakma hakkında daha fazla bilgi için bkz: [yayın IDisposable kaynakları](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).  


<a name="avoidfloats" />

## <a name="avoid-floating-point-arithmetic"></a>Kayan nokta aritmetik kaçının

Android cihazlarda, kayan nokta aritmetik hakkında tamsayı aritmetiğini daha yavaş x 2'dir. Bu nedenle, kayan nokta aritmetik mümkünse tamsayı aritmetik ile değiştirin. Ancak, arasında yürütme süresi fark yoktur `float` ve `double` son donanım üzerinde aritmetik.

> [!NOTE]
> Tamsayı aritmetiğini için bile, bazı CPU olmaması donanım özellikleri bölün. Bu nedenle, Tamsayı bölme ve modül işlemleri genellikle yazılım gerçekleştirilir.

<a name="dismissdialogs" />

## <a name="dismiss-dialogs"></a>İletişim kutularını kapatın

Kullanırken [ `ProgressDialog` ](https://developer.xamarin.com/api/type/Android.App.ProgressDialog/) sınıfı (veya tüm iletişim ya da uyarı), çağırmak yerine [ `Hide` ](https://developer.xamarin.com/api/member/Android.App.Dialog.Hide()/) iletişim kutusu amacı tamamlandığında, yöntem çağrısı [ `Dismiss` ](https://developer.xamarin.com/api/member/Android.App.Dialog.Dismiss()/) yöntemi. Aksi takdirde iletişim Canlı olmaya devam eder ve etkinlik bir başvuru tutarak sızıntısı.

## <a name="summary"></a>Özet

Bu makalede açıklanan ve Xamarin.Android ile oluşturulan uygulamaların performansını artırmak için teknikleri ele alınan. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası performansı](~/cross-platform/deploy-test/memory-perf-best-practices.md)
