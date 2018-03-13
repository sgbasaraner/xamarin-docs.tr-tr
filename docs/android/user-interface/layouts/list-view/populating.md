---
title: ListView verilerle doldurma
ms.topic: article
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 12197d238ddc6ddc2bd8f48f77aa15f5eff22a0a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="populating-a-listview-with-data"></a>ListView verilerle doldurma


## <a name="overview"></a>Genel Bakış

Satır eklemek için bir `ListView` , Düzen ve uygulama eklemek gereken bir `IListAdapter` yöntemleriyle, `ListView` kendisini doldurmak için çağrıları. Android içeren yerleşik `ListActivity` ve `ArrayAdapter` herhangi bir özel düzen XML veya kod tanımlamadan kullanabilirsiniz sınıfları. `ListActivity` Sınıfı otomatik olarak oluşturur bir `ListView` ve ortaya çıkaran bir `ListAdapter` bağdaştırıcıyı görüntülemek için satır görünümlerini sağlamak için özellik.

Yerleşik bağdaştırıcıları görünüm kaynak kimliği her satır için kullanılan bir parametre olarak alın. Olanlar gibi yerleşik kaynakları kullanabilir `Android.Resource.Layout` , yazma kendi gerekmez.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>ListActivity ve ArrayAdapter kullanarak&lt;dize&gt;

Örnek **BasicTable/HomeScreen.cs** bu sınıfları görüntülemek için nasıl kullanılacağını gösteren bir `ListView` yalnızca birkaç kod:

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
   protected override void OnListItemClick(ListView l, View v, int position, long id)
}
```


### <a name="handling-row-clicks"></a>Satır işleme tıklar

Genellikle bir `ListView` de (örneğin, bir tıklatın ya da bir kişi arama veya başka bir ekranını gösteren) bazı eylemleri gerçekleştirmek için bir satır touch kullanıcıya izin verir. Yanıt verecek şekilde kullanıcı rötuşları gerekir için daha fazla yöntemi uygulanan `ListActivity` &ndash; `OnListItemClick` &ndash; şöyle:

[![Bir SimpleListItem ekran görüntüsü](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

Kullanıcı bir satır touch artık ve `Toast` uyarı görünür:

[![Ekran görüntüsü, bir satır işlemdeki açtığınızda görüntülenen bildirim](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>Bir ListAdapter uygulama

`ArrayAdapter<string>` kendi Basitlik, ancak buna ait nedeniyle son derece sınırlı mükemmeldir. Bununla birlikte, çoğu zaman bağlamak istediğiniz yalnızca dizeler yerine iş varlıklar koleksiyonu vardır.
Örneğin, verilerinizi çalışan sınıfların koleksiyonu oluşuyorsa, yalnızca her çalışan adlarını görüntülemek için listedeki isteyebilirsiniz. Davranışını özelleştirmek için bir `ListView` hangi verilerin görüntülenme şeklini denetlemek için bir alt sınıfı uygulamalıdır `BaseAdapter` aşağıdaki dört öğelerini geçersiz kılma:

-   **Count** &ndash; denetimi verilerde satır sayısını olan bildirmek için.

-   **GetView** &ndash; her satır için bir görünüme dönmek için verilerle doldurulur.
    Bu yöntem için bir parametre içeriyor `ListView` yeniden kullanım için varolan, kullanılmayan satırda geçirmek için.

-   **GetItemId** &ndash; satır tanımlayıcısı döndürür (genellikle satır numarası, istediğiniz herhangi bir uzun değer olabilmesine rağmen).

-   **Bu [int]** dizin oluşturucu &ndash; belirli bir satır numarası ile ilişkili verileri döndürmek için.

Örnek kodda **BasicTableAdapter/HomeScreenAdapter.cs** gösteren bir alt kümesi nasıl `BaseAdapter`:

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```


### <a name="using-a-custom-adapter"></a>Özel bağdaştırıcı kullanma

Özel bağdaştırıcı kullanılarak benzer yerleşik `ArrayAdapter`, içinde geçen bir `context` ve `string[]` değerleri görüntülemek için:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Bu örnek aynı satır Düzen kullandığından (`SimpleListItem1`) sonuçta elde edilen uygulama önceki örneğe benzer görünür.


### <a name="row-view-re-use"></a>Satır görünümü yeniden kullanma

Bu örnekte yalnızca altı öğe yok. Ekran sekiz sığabilecek olduğundan, hiçbir satır yeniden kullanımını gerekli. Ancak, yüzlerce veya binlerce satır görüntülerken, yüzlerce veya binlerce oluşturmak için bellek kaybı olacaktır `View` nesneler yalnızca sekiz aynı anda ekranda sığmayacak. Bir satır bir kuyruk yeniden kullanım için kendi görünüm yerleştirilir ekranından kaybolduğunda bu durumdan kaçınmak için. Kullanıcı kayarken `ListView` çağrıları `GetView` görüntülemek için yeni görünümler istemek için &ndash; kullanılabilir bunu kullanılmayan bir görünümde geçirir, `convertView` parametresi. Aksi durumda bu değer null ise, kodunuzu yeni bir görünüm örneği oluşturmanız, nesnenin özelliklerini yeniden ayarlayın ve yeniden kullanma.

`GetView` Yöntemi yeniden kullanımını satır görünümlerine bu deseni izlemelidir:

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

Özel bağdaştırıcı uygulamaları gereken *her zaman* yeniden kullanımını `convertView` değil çalıştırdıkları bellek yetersiz uzun listeler görüntülenirken emin olmak için yeni görünümler oluşturmadan önce nesne.

Bazı bağdaştırıcısı uygulamaları (gibi `CursorAdapter`) yoksa bir `GetView` yöntemi, iki farklı yöntem yerine duyarlar `NewView` ve `BindView` hangi zorunlu satır yeniden kullanımını sorumluluklarını ayırarak `GetView` iki içine yöntemleri. Var olan bir `CursorAdapter` belgenin devamındaki örnek.


## <a name="enabling-fast-scrolling"></a>Hızlı kaydırmayı etkinleştirme

Hızlı kaydırma 'listesinin bir parçası doğrudan erişmek için bir kaydırma çubuğunun davranan bir ek tanıtıcısı' sağlayarak uzun listeleriyle kaydırma kullanıcıya yardımcı olur. Bu ekran hızlı kaydırma tutacağı gösterir:

[![Kaydırma işleyici ile hızlı olarak kaydırma ekran görüntüsü](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

Görüntülenecek hızlı kaydırma tanıtıcı neden ayarı olarak basit `FastScrollEnabled` özelliğine `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>Bir bölüm dizini ekleme

Uzun bir listesi ile hızlı kaydırma olduklarında bölüm dizini ek görüş kullanıcıları için sağlar. &ndash; 'bunlar kaydırılan için hangi bölüm' gösterir. Bir bağdaştırıcı alt uygulanmalı görünmesi bölüm dizini neden `ISectionIndexer` arabirimi görüntülenen satır bağlı olarak dizin metin girin:

[![H ile başlayan bölümün yukarısında görünmesini H ekran görüntüsü](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

Uygulanacak `ISectionIndexer` bir bağdaştırıcı için üç yöntem eklemeniz gerekir:

-   **GetSections** &ndash; görüntülenemedi dizin başlıkları bölüm tam listesini sağlar. Kodu oluşturmak için gereken bu yöntem Java nesnelerinin bir dizisi gerektirir ve bu nedenle bir `Java.Lang.Object[]` bir .NET koleksiyonundan. İsteğe bağlı olarak Bizim örneğimizde listesi olarak ilk karakter bir listesini döndürür `Java.Lang.String` .

-   **GetPositionForSection** &ndash; belirtilen bölüm dizini ilk satır konumunu döndürür.

-   **GetSectionForPosition** &ndash; için belirli bir satır görüntülenecek bölüm dizinini döndürür.


Örnek `SectionIndex/HomeScreenAdapter.cs` dosya oluşturucuda bu yöntemler ve bazı ek kod uygular. Her satır üzerinden döngü ve ilk karakter (öğeleri zaten Bunun çalışması için sıralanmalıdır) başlığın ayıklama tarafından bölüm dizini Oluşturucusu oluşturur.

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

Oluşturulan, veri yapılarını ile `ISectionIndexer` çok basit yöntemler şunlardır:

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

Bölüm dizin başlıklarınızın 1:1, gerçek bölümlere eşlemek için gerek yoktur. Nedeni budur `GetPositionForSection` yöntem vardır.
`GetPositionForSection` Liste görünümünde ne olursa olsun bölümleridir için dizin listesinde ne olursa olsun dizinlerini olan eşlemek için fırsatı sunar. Örneğin, "z", dizininizdeki olabilir, ancak her harfi için bir bölüm sahip değilse, 26 "z" eşleme yerine için eşleyebilir şekilde 25 24 ya da her bölüm dizini "z" eşlenmesi gerekir.



## <a name="related-links"></a>İlgili bağlantılar

- [BasicTableAndroid (örnek)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (örnek)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (örnek)](https://developer.xamarin.com/samples/FastScroll/)
