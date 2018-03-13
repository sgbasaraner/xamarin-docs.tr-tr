---
title: "İzlenecek yol"
ms.topic: article
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: e5c058f173f64efe4a5c777872e9ea67120115f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough"></a>İzlenecek yol

Aşağıdaki adımlarda parçaları ile temel bir uygulama oluşturulur. İlk adım, Android projesi için yeni bir Xamarin.Android oluşturmaktır.

## <a name="1-create-a-project"></a>1. Proje oluşturma

Adlı yeni bir Xamarin.Android projesi oluşturma **FragmentSample**. **Minimum Android** sürüm ayarlanmalıdır Android 3.1 veya sonraki sürümlerde, aşağıdaki resimde gösterildiği gibi:

[![Minimum Android sürümü ayarlama](walkthrough-images/00.png)](walkthrough-images/00.png#lightbox)


## <a name="2-create-the-mainactivity"></a>2. MainActivity oluşturma

Ardından, oluşturmamız gerekir `MainActivity` sınıfı. Bu uygulama için başlangıç etkinliği var. Bu etkinlik ekran boyutuna bağlı olarak bir veya iki parça barındırır. `MainActivity` Bu cihaz için uygun olan düzen dosyası yükleyerek yapın.

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` Parça alt sınıflar ortak varsayılan bir bağımsız değişken oluşturucu yok olması gerekir.

## <a name="3-create-the-layout-files"></a>3. Düzen dosyaları oluşturma

İki farklı ekran boyutlarına iki farklı düzeni dosyaları gerektirir. Yeni bir klasör, bu nedenle oluşturalım **kaynakları/düzen-büyük**ve adlı yeni bir düzen oluşturmak **activity_main.axml**. Biz de varsayılan düzen dosyası olarak yeniden adlandırın **Resources/Layout/activity_main.axml**. Bu değişikliklerden sonra aşağıdaki ekran düzeni klasörleri benzemelidir:

[![IDE içinde düzeni klasörlerinin ekran görüntüsü](walkthrough-images/01.png)](walkthrough-images/01.png#lightbox)


Tüm aygıtlara yüklemek ve düzeni dosyasında kullanmak **kaynakları/düzeni**.
Yalnızca görüntüler çok basit bir düzen olan bir `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

Büyük bir ekrana sahip cihazlar için Android düzeni dosyasında yükler **kaynakları/düzen-büyük**. Tabletler düzenini içeriğini aşağıdaki gibidir:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

Düzen dosyasını büyük ekranlar için biraz farklıdır. Değil yalnızca `TitlesFragment` bu düzeni dosyasında görüntülenen ancak `FrameLayout` sağ parça yanındaki eklenir. Daha büyük ekranında `DetailsFragment` program aracılığıyla eklenen `MainActivity` yürütme seçtiğinde kullanıcı. Daha ayrıntılı olarak bunun nasıl yapılacağı daha sonra açıklayacağız.

Android 3.2 ekranı düzeni belirtmek için yeni bir yolunu sunar. Bu yeni niteleyicileri ekran boyutu yerine düzeninizi kapladığı alanı miktarını belirtin. Bu uygulama yalnızca Android 3.2 veya daha yüksek çalışması gerektiği, biz oluşturacak bir **Düzen/kaynak-sw600dp** klasörü (klasör yerine **kaynak/düzen-büyük**) Düzen dosyası için  **activity_main.AXML**. Bu kaynak dosyası 600 yoğunluğu bağımsız piksel minimum ekran genişliği sahip tüm cihazlar tarafından yüklenir. Ancak, bu uygulama olarak hedef Android 3.1 ayarlandığını veya üzeri, eski kaynak niteleyici kullanır.

## <a name="4-create-the-titlesfragment"></a>4. TitlesFragment oluşturma

`TitlesFragment` çeşitli yürütür başlıklarını görüntülemek, bu nedenle proje için yeni bir parçası olarak adlandırılan ekleyelim `TitlesFragment`:

[![Yeni bir parça TitlesFragment projesine ekleme](walkthrough-images/02.png)](walkthrough-images/02.png#lightbox)

Sonra `TitlesFragment` eklendi, biz değiştirmelisiniz sınıfı öğesinden devralınan böylece `Android.App.ListFragment`. `ListFragment` Liste işlevselliği içeren bir özelleştirilmiş parça türüdür.
`TitlesFragment` Ayrıca geçersiz kılar `OnActivityCreated` (başka bir parça yaşam döngüsü yöntemi) ve sağlayan bir `Adapter` , `ListFragment` listeyi doldurmak için kullanır:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

Daha önce belirtildiği gibi uygulamamız için iki düzenleri sahip `MainActivity`. Kodda `OnActivityCreated` varlığını algılar `FrameLayout` ve hangi düzeni dosyası yüklenmiş belirler. Varsa `FrameLayout` düzeninde mevcut sonra `_isDualPane` bayrağı ayarlanmış `true`. `_isDualPane` Bayrağı kullanılan başka bir etkinliğin, özellikle göre `ShowDetails` yöntemi. `ShowDetails` Yöntemi aşağıdaki daha ayrıntılı olarak ele.

`TitlesFragment` bir listesidir ve kullanıcı seçimlerini listesinde yanıtlar gerekir. Bunu yapmak için `TitlesFragment` yöntemini geçersiz kılar `OnListItemClick`. İçinde `OnListItemClick`, yeni bir `DetailsFragment` oluşturulur ve görüntülenen `FrameLayout`, programlı olarak. İçinde ilgili kod `TitlesFragment` değil:

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

Kod biçimlendirme ve seçili play tekliften görüntülemek nasıl aygıttan belirler. Tabletler, söz konusu olduğunda `_isDualPane` bayrağı ayarlanacak `true`, ve teklif yanına görüntülenecek şekilde `TitlesFragment`. Seçilen play `id` zaten görüntülenmez sonra yeni `DetailsFragment` oluşturulur ve ardından yüklenen `FrameLayout` etkinlik. Büyük bir görüntü yok diğer cihazlar için &ndash; telefonlar, örneğin &ndash; `isDualPane` ayarlanacak `false` için yeni bir `DetailsActivity` başlatılır.


## <a name="5-create-the-detailsactivity"></a>5. DetailsActivity oluşturma

`DetailsActivity` Görüntüler `DetailsFragment` küçük aygıtlar için. Bu görmek için önce yeni bir etkinlik adlı projeye ekleyeceğiz `DetailsActivity`. `DetailsActivity` çok basit bir etkinliktir. Bunu oluşturur ve ardından yeni bir ana bilgisayar `DetailsFragment` play için `id` , gönderildi:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Herhangi bir düzen dosyası için yüklenen bildirimi `DetailsActivity`. Bunun yerine, `DetailsFragment` etkinlik kök görünüme yüklenir. Bu kök görünüm özel Kimliğine sahip `Android.Resource.Id.Content`. Yeni bir `DetailFragment` oluşturulur ve bu kök görünümüne içine eklenen bir `FragmentTransaction` etkinliğin tarafından oluşturulan `FragmentManager`.


## <a name="6-create-the-detailsfragment"></a>6. DetailsFragment oluşturma

Şimdi, başka bir parça adlı uygulamayı ekleyelim `DetailsFragment`. Bu parça seçili play tekliften görüntüler. Aşağıdaki kod tam gösterir `DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

Sırayla `DetailsFragment` düzgün çalışması için seçili olduğunda play dizinini olmalıdır `TitlesFragment`. Bu değer sağlamak için birçok yolu vardır `DetailsFragment`; Bu örnekte, yürütmeyi `Id` bir kümeye yerleştirilir ve paket örneği Arguments özelliği için depolanan `DetailsFragment`. Özellik `ShownPlayId` kolaylık sağlaması açısından sunulmuştur &ndash; örnekleri tarafından kullanılacak `DetailsFragment` paketindeki değer alınamadı.

`OnCreateView` Parça kullanıcı arabirimiyle çizmek gereken ve döndürmelidir adlı bir `Android.Views.View` nesnesi. Çoğu durumda bu olan bir `View` varolan bir düzen dosyasından şişirileceğini. Yukarıdaki örnekte söz konusu olduğunda, parça programlı olarak görüntülenmek üzere kullanılacak görünüm oluşturacaksınız.

Tebrikler! Form faktörleri arasında geliştirme basitleştirmek için parçaları kullanan bir uygulamayı şimdi oluşturduğunuzu düşünün.

İçinde [sonraki bölümde](supporting-pre-honeycomb.md), böylece öncesi Android 3.0 cihazlarda çalışır, bu uygulama genişletmek için yapacağız.

