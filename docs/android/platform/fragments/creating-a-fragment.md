---
title: Bir bölümü oluşturma
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: eae25dbfaf1125191a83b3cc4326abc19105f22f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770215"
---
# <a name="creating-a-fragment"></a>Bir bölümü oluşturma

Bir parça oluşturmak için bir sınıf öğesinden devralmalıdır `Android.App.Fragment` ve daha sonra geçersiz `OnCreateView` yöntemi. `OnCreateView` ekranda parça koymanın zamanı geldi ve döndürülecek barındırma etkinlik tarafından çağrılacak bir `View`. Tipik bir `OnCreateView` bu oluşturacak `View` düzeni dosyasını inflating ve bir üst kapsayıcıya ekleniyor. Kapsayıcının özellikleri Android üst düzeni parametrelerinin parça kullanıcı Arabirimi için geçerli olacağından önemlidir. Aşağıdaki örnekte bu gösterilmektedir:

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

Yukarıdaki kod görünümü Şişir `Resource.Layout.Example_Fragment`ve bir alt görünüm olarak eklemek `ViewGroup` kapsayıcı.


> [!NOTE]
> Parça alt sınıflar ortak varsayılan bir bağımsız değişken oluşturucu yok olması gerekir.

## <a name="adding-a-fragment-to-an-activity"></a>Etkinlik için bir parça ekleme

Bir parça barındırılabilir iki yolla aktivite içinde:

-   **Bildirimli olarak** &ndash; parçaları bildirimli olarak içinde kullanılabilir `.axml` kullanarak Düzen dosyaları `<Fragment>` etiketi.

-   **Program aracılığıyla** &ndash; parçaları de oluşturulabilir dinamik olarak kullanarak `FragmentManager` sınıfının API.

Aracılığıyla programlı kullanım `FragmentManager` sınıfı bu kılavuzda açıklanır.

### <a name="using-a-fragment-declaratively"></a>Bir parça bildirimli olarak kullanma

Düzen içinde bir parça eklemek gerektirir kullanarak `<fragment>` etiketi ve sonra da sağlayarak parça tanımlama `class` özniteliği veya `android:name` özniteliği. Aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `class` bildirmek için öznitelik bir `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Bu sonraki parçacığı bildirmek gösterilmiştir bir `fragment` kullanarak `android:name` özniteliğini parça sınıfı tanımlamak için:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Etkinlik oluşturulduğunda, Android Düzen dosyasında belirtilen her parça örneği ve oluşturulur görünüm eklemek `OnCreateView` yerine `Fragment` öğesi.
Bir etkinliğe bildirimli olarak eklenen statik ve yok edilir kadar faaliyete kalır; dinamik olarak değiştirin veya böyle bir parça bağlı olduğu etkinlik ömrü boyunca kaldırmak mümkün değildir.

Her parça benzersiz bir tanımlayıcı atanması gerekir:

-  **Android: kimliği** &ndash; bir düzen dosyasında diğer kullanıcı Arabirimi öğeleri ile bu benzersiz bir kimliği olarak

-  **Android: Etiket** &ndash; bu özniteliği benzersiz bir dizedir.

Önceki iki yöntem hiçbiri kullanılırsa, parça kimliği kapsayıcı görünümünün varsayar. Aşağıdaki örnekte ne nerede `android:id` ya da `android:tag` Android kimliği atayacaktır sağlanmış `fragment_container` parça için:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="+@id/fragment_container"
                android:orientation="horizontal"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

        <fragment class="com.example.android.apis.app.TitlesFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
</LinearLayout>
```

### <a name="package-name-case"></a>Paket adı durumu

Android paketini adları büyük harfli karakterler izin vermiyor; bir paket adı bir büyük harf karakter içeriyorsa, görünüm Şişir çalışılırken bir özel durum. Ancak, Xamarin.Android daha fazla forgiving olduğu ve ad alanında büyük harf karakterler tolerans.

Örneğin, aşağıdaki kod parçacıkları her ikisi de Xamarin.Android ile çalışır. Ancak, ikinci parçacığı neden olacak bir `android.view.InflateException` saf Java tabanlı bir Android uygulama tarafından oluşturulan için.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

VEYA

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>Parça yaşam döngüsü

Parçaların biraz bağımsız olarak, ancak hala etkilenen göre kendi ömrü vardır [yaşam döngüsü barındırma etkinliğin](~/android/app-fundamentals/activity-lifecycle/index.md).
Örneğin, bir etkinlik duraklatır, tüm ilişkili parçaları duraklatıldı. Aşağıdaki diyagramda parça yaşam döngüsü özetlenmektedir.

[![Akış parça yaşam döngüsü gösteren diyagram](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>Parça oluşturma yaşam döngüsü yöntemleri

Oluşturulmakta olduğu gibi aşağıdaki liste bir parça çevriminin çeşitli geri çağırmaları akışını gösterir:

-   **`OnInflate()`** &ndash; Parça bir görünüm düzeni bir parçası olarak oluşturulduğunda çağrılır. Hemen parça XML düzeni dosyasından bildirimli olarak oluşturulduktan sonra bu çağrılabilir. Parça henüz, kendi etkinliği ile ilişkili değil ancak **etkinlik**, **paket**, ve **AttributeSet** görünümden hiyerarşisi geçirilen parametre olarak. Bu yöntem en iyi ayrıştırmak için kullanılan **AttributeSet** ve için öznitelikleri kaydetmeyi daha sonra parça tarafından kullanılıyor olabilir.

-   **`OnAttach()`** &ndash; Parça etkinliği ile ilişkili sonra çağrılır. Bu parça kullanılmaya hazır olduğunda çalıştırılacak ilk yöntemidir. Genel olarak, parçaları bir oluşturucu uygulamak veya gerekir varsayılan oluşturucu geçersiz kılar. Bu yöntemde parçası için gerekli olan bileşenlerin başlatılması.

-   **`OnCreate()`** &ndash; Parça oluşturmak için etkinlik tarafından çağrılır. Bu yöntem çağrıldığında, parça etkinliğin görünüm hiyerarşisinde daha sonra parça'nın ömrü kadar tüm parçalarını güvenmemelisiniz şekilde barındırma etkinlik görünümü hiyerarşisini tamamen, oluşturulamaz. Örneğin, bu yöntem herhangi bir tweaks veya uygulamanın kullanıcı arabirimine ayarlamalar yapmak için kullanmayın. Bu, parça, gereken veri toplamayı başlayabilir, erken zamandır. Parça UI iş parçacığında bu noktada çalışıyor, böylece her uzun işleme engel veya arka plan iş parçacığında işleme gerçekleştirin. Bu yöntem, atlanabilir **SetRetainInstance(true)** olarak adlandırılır.
    Bu alternatif aşağıdaki daha ayrıntılı olarak açıklanır.

-   **`OnCreateView()`** &ndash; Parça görünümünü oluşturur.
    Bu yöntem etkinliğe ilişkin bir kez çağrılır **OnCreate()** yöntemi tamamlanmıştır. Bu noktada, etkinlik hiyerarşisini görüntüleme ile etkileşim kurmak güvenli değildir. Bu yöntem, parça tarafından kullanılacak görünüm döndürmelidir.

-   **`OnActivityCreated()`** &ndash; Sonra adlı **Activity.OnCreate** barındırma etkinlik tarafından tamamlandı.
    Kullanıcı arabirimi son tweaks şu anda gerçekleştirilmesi gerekir.

-   **`OnStart()`** &ndash; Kapsayıcı etkinlik çıktıktan sonra çağrılır. Bu parça kullanıcıya görünür hale getirir. Çoğu durumda, parça, aksi durumda olacak bir kod bulunacaktır **OnStart()** etkinliğin yöntemi.

-   **`OnResume()`** &ndash; Bu, kullanıcı parça ile etkileşim kurabilmesi için adlı son yöntemidir. Tür, bu yöntemde gerçekleştirilmesi gereken kodu örneği kullanıcı ile kamera gibi etkileşebilir bir cihaz özelliklerini etkinleştirme, konum Hizmetleri. Ancak, bunlar gibi hizmetleri aşırı pil boşaltma neden olabilir ve uygulamanın pil ömrünün korumak için kullanımlarını en aza indirmeniz gerekir.


### <a name="fragment-destruction-lifecycle-methods"></a>Parça yok etme yaşam döngüsü yöntemleri

Sonraki listede bir parça yok olarak adlandırılır yaşam döngüsü yöntemleri açıklanmaktadır:

-   **`OnPause()`** &ndash; Kullanıcı artık parça ile etkileşemeyebilirsiniz. Bu durum başka bir parça işlemi bu parçasını değiştiriyor veya barındırma etkinlik duraklatıldı nedeniyle bulunmaktadır. Bu parçasını barındıran etkinlik hala görünür olabilir, diğer bir deyişle, odak etkinliğinde kısmen saydamdır veya tam ekran kaplar değil mümkündür. Bu yöntem etkin olduğunda, kullanıcı parça ayrılıyor ilk göstergesidir. Parça değişiklikleri kaydetmeniz gerekir.

-   **`OnStop()`** &ndash; Parça artık görünür değildir. Ana etkinlik durdurulmuş olabilir veya bir parçasını işlemi etkinliğin değiştiriyor. Bu geri çağırma aynı amaca hizmet eder **Activity.OnStop**.

-   **`OnDestroyView()`** &ndash; Bu yöntem, görünüm ile ilişkilendirilen kaynakları temizlemek için çağrılır. Parça ile ilişkili görünüm yok, bu adı verilir.

-   **`OnDestroy()`** &ndash; Parça artık kullanımda olmadığında bu yöntem çağrılır. Etkinliği ile hala ilişkilidir, ancak parça artık işlevsel değildir. Bu yöntem gibi parça tarafından kullanılmakta olan tüm kaynakları serbest bırakmalısınız bir [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) , kullanılabilir için kamera. Bu yöntem, atlanabilir **SetRetainInstance(true)** olarak adlandırılır. Bu alternatif aşağıdaki daha ayrıntılı olarak açıklanır.

-   **`OnDetach()`** &ndash; Bu yöntem yalnızca parça artık etkinliği ile ilişkili değil önce çağrılır. Parça görünümü hiyerarşisini artık yok ve bu noktada parça tarafından kullanılan tüm kaynakları serbest.


### <a name="using-setretaininstance"></a>SetRetainInstance kullanma

Etkinlik yeniden oluşturulan kullanılıyorsa, tamamen yok edilmesi değil olduğunu belirtmek bir parçası için mümkündür. `Fragment` SAX yöntemi `SetRetainInstance` bu amaç için. Varsa `true` bu yönteme geçirilen etkinlik yeniden başlatıldığında, parça aynı örneği kullanılır. Aşması durumunda dışındaki tüm geri arama yöntemleri çağrılacak `OnCreate` ve `OnDestroy` yaşam döngüsü geri aramalar. Bu işlem, yukarıda (yeşil noktalı çizgilerle) gösterilen yaşam döngüsü diyagramda gösterilmiştir.


## <a name="fragment-state-management"></a>Parça durum yönetimi

Parça may kaydedin ve örneğini kullanarak parça yaşam döngüsü sırasında durumlarına geri bir `Bundle`. Paket verileri anahtar/değer çiftleri olarak kaydetmek bir parça sağlar ve kadar bellek gerektirmeyen basit veriler için kullanışlıdır. Bir parça çağrısıyla durumuna kaydedebilirsiniz `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

Bir parça yeni bir örneğini oluşturulduğunda, kaydedilmiş durumu `Bundle` yeni örnek için kullanılabilir olacağı `OnCreate`, `OnCreateView`, ve `OnActivityCreated` yeni örnek yöntemleri.
Aşağıdaki örnek, değerini almak gösterilmiştir `current_choice` gelen `Bundle`:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    if (savedInstanceState != null)
    {
        _currentCheckPosition = savedInstanceState.GetInt("current_choice", 0);
    }
}
```

Geçersiz kılma `OnSaveInstanceState` arasında yönlendirme değişiklikleri, bir parça geçici verileri gibi kaydetmek için uygun bir mekanizma `current_choice` yukarıdaki örnekte değeri. Ancak, varsayılan uygulaması `OnSaveInstanceState` atanmış bir Kimliğine sahip her görünüm için kullanıcı arabiriminde geçici verileri kaydetme mvc'deki. Örneğin, olan bir uygulamayı arayın bir `EditText` XML dosyasında şu şekilde tanımlanan öğe:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

Bu yana `EditText` denetimi sahip bir `id` atanan, parça otomatik olarak veri pencere öğesinde kaydeder zaman `OnSaveInstanceState` olarak adlandırılır.


### <a name="bundle-limitations"></a>Paket sınırlamaları

Kullanarak rağmen `OnSaveInstanceState` için bu yöntemi, geçici verileri, kullanımı kolay olan bazı sınırlamalar getirir:

-  Parça geri yığını eklenmemiş sonra kullanıcı bastığında durumuna geri **geri** düğmesi.

-  Paket verileri kaydetmek için kullanıldığında, bu verileri seri değildir. Bu, işleme gecikmeleri neden olabilir.


## <a name="contributing-to-the-menu"></a>Menüye katkıda bulunan

Parçaları barındırma etkinliklerini menüsüne öğelerinin katkıda bulunabilir.
Bir etkinlik menü öğeleri ilk işler. Etkinlik bir işleyiciye sahip değil, daha sonra olay sonra işleyecek parça geçirilecektir.

Etkinliğin menü öğelerini eklemek için bir parça iki şey yapmanız gerekir.
İlk olarak, parça yöntemi uygulamalıdır `OnCreateOptionsMenu` ve öğelerinden menüsüne, aşağıdaki kodda gösterildiği gibi yerleştirin:

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

Önceki kod parçacığını menüde dosyada bulunan aşağıdaki XML'den şişirileceğini `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

Ardından, parça çağırmalısınız `SetHasOptionsMenu(true)`. Bu yöntem çağrısı için Android parça menü seçeneği menüsüne katkıda öğelerine sahip olduğunu bildirir. Bu yöntem çağrısı kılmadığınız sürece parça menü öğeleri etkinliğe ilişkin seçeneği menüsüne eklenen değil. Bu genellikle yaşam döngüsü yönteminde yapılır `OnCreate()`sonraki kod parçacığında gösterildiği gibi:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

Aşağıdaki ekran bu menü nasıl görüneceği gösterilmektedir:

[![Örnek uygulamasının ekran görüntüsü menü öğeleri görüntüleme My dönüşleri](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
