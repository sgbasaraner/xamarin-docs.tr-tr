---
title: "ListView'ın görünümünü özelleştirme"
ms.topic: article
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 18c53ed6428eff911420c696d45b341d8e0fa5c1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="customizing-a-listviews-appearance"></a>ListView'ın görünümünü özelleştirme

<a name="overview" />

## <a name="overview"></a>Genel Bakış

ListView görünümünü görüntülenen satır düzeni tarafından dikte edilir. Görünümü değiştirmek için bir `ListView`, farklı satır düzenini kullanın.

<a name="Built-in_Row_Views" />

## <a name="built-in-row-views"></a>Yerleşik satır görünümleri

Kullanarak başvurulabilir on iki yerleşik görünüm vardır **Android.Resource.Layout**:

- **TestListItem** &ndash; tek satır en az biçimlendirme içeren metin.

- **SimpleListItem1** &ndash; tek satır metin.

- **SimpleListItem2** &ndash; iki satırlık metin.

- **SimpleSelectableListItem** &ndash; tek tek veya birden çok öğe seçimi (API düzeyi 11 eklenir) destekleyen metin satırının.

- **SimpleListItemActivated1** &ndash; SimpleListItem1 benzer, ancak arka plan rengini belirten bir satır seçili olduğunda (API düzeyi 11 eklenir).

- **SimpleListItemActivated2** &ndash; SimpleListItem2 benzer, ancak arka plan rengini belirten bir satır seçili olduğunda (API düzeyi 11 eklenir).

- **SimpleListItemChecked** &ndash; seçimi belirtmek için onay işareti görüntülenir.

- **SimpleListItemMultipleChoice** &ndash; çok Tercihli bir seçim belirtmek için onay kutularını görüntüler.

- **SimpleListItemSingleChoice** &ndash; görüntüler radyo düğmeleri birbirini dışlayan seçimi belirtmek için.

- **TwoLineListItem** &ndash; iki satırlık metin.

- **ActivityListItem** &ndash; tek satırlık bir görüntü içeren metin.

- **SimpleExpandableListItem** &ndash; grupları satır kategorileri ve her grup genişletilmiş veya daraltılmış.

Her bir yerleşik satır görünüm ilişkili yerleşik bir stil içerir. Bu ekran görüntüleri her görünüm nasıl göründüğünü gösterir:

[![TestListItem, SimpleSelectableListItem, SimpleListitem1 ve SimpleListItem2 ekran görüntüleri](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png)

[![SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked ve SimpleListItemMultipleChecked ekran görüntüleri](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png)

[![SimpleListItemSingleChoice, TwoLineListItem, ActivityListItem ve SimpleExpandableListItem ekran görüntüleri](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png)

**BuiltInViews/HomeScreenAdapter.cs** örnek dosyası (içinde **BuiltInViews** çözüm) Genişletilebilir olmayan liste öğesi ekranlar üretmek için kodunu içerir. Görünüm kümesinde `GetView` yöntemi şuna benzer:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Görünümün özelliklerini sonra standart denetim tanımlayıcıları başvurarak ayarlanabilir `Text1`, `Text2` ve `Icon` altında `Android.Resource.Id` (Görünüm içermiyor veya bir özel durum özellikleri ayarlı değil):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter.cs** örnek dosyası (içinde **BuiltInViews** çözüm) SimpleExpandableListItem ekran üretmek için kodunu içerir. Grup görünümü kümesinde `GetGroupView` yöntemi şuna benzer:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Alt görünümü kümesinde `GetChildView` yöntemi şuna benzer:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Grup görünümü ve alt görünümü özelliklerini sonra standart başvurarak ayarlanabilir `Text1` ve `Text2` yukarıda gösterildiği gibi tanımlayıcıları denetim. (Yukarıda gösterilen) SimpleExpandableListItem ekran tek satır grubu görünümü (SimpleExpandableListItem1) ve iki satır alt Görünüm (SimpleExpandableListItem2) gösteren bir örnek sağlar. Alternatif olarak, grubu görünümü için iki satırı (SimpleExpandableListItem2) yapılandırılabilir ve alt görünümü için bir satır (SimpleExpandableListItem1) yapılandırılabilir veya her ikisi de Görünüm Grup ve alt görünümü satırları aynı sayıda olabilir. 


<a name="Accessories" />

## <a name="accessories"></a>Donatılar

Satır seçimi durumu göstermek için Görünüm sağında eklenen Donatılar sahip olabilir:

- **SimpleListItemChecked** &ndash; göstergesi olarak onay tek seçim listesi oluşturur.

- **SimpleListItemSingleChoice** &ndash; Creates radio-button-type lists where only one choice is possible.

- **SimpleListItemMultipleChoice** &ndash; birden çok seçenek nerede olası checkbox türü listesi oluşturur.

Daha önce bahsedilen Donatılar aşağıdaki ekranda, kendi ilgili sırayla gösterilmiştir:

[![Ekran görüntüleri, SimpleListItemChecked, SimpleListItemSingleChoice ve SimpleListItemMultipleChoice Donatılar ile](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png)

Bu Donatılar geçişi birini görüntülemek için gerekli düzeni kaynak kimliği bağdaştırıcıya sonra el ile seçim durumu gerekli satırlar için ayarlayın. Bu kod satırı oluşturmak ve atamak gösterilmiştir bir `Adapter` bu düzenlerden birini kullanarak:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView` Kendi görüntülenmekte donatıyı bağımsız olarak farklı seçim modunu destekler. Karışıklığı önlemek için `Single` seçim modu ile `Checked` ve `SingleChoice` Donatılar ve `Multiple` moduyla `MultipleChoice` stili. Seçim modu tarafından denetlenen `ChoiceMode` özelliği `ListView`.

<a name="Handling_API_Level" />

### <a name="handling-api-level"></a>İşleme API düzeyi

Xamarin.Android'ın önceki sürümlerini numaralandırmalar tamsayı özellikleri olarak uygulanır. En son sürümü, olası seçenekleri keşfetmek kolaylaşır uygun .NET Numaralandırma türleri anlatılmıştır.

Hangi API düzeyine bağlı olarak, hedeflediğiniz, `ChoiceMode` bir tamsayı ya da numaralandırması. Örnek dosyayı **AccessoryViews/HomeScreen.cs** Zencefilli kurabiye API hedeflemek istiyorsanız bir blok kılınmıştır vardır:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```

<a name="Selecting_Items_Programmatically" />

### <a name="selecting-items-programmatically"></a>Program aracılığıyla öğeleri seçme

El ile hangi öğelerin ayarlama 'seçilir' gerçekleştirilir `SetItemChecked` yöntemi (bunu çağrılabilir birden çok kez birden çok seçim için):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Kod ayrıca birden çok seçimin tek seçim farklı algılamak gerekir. Hangi satır içinde seçili belirlemek üzere `Single` modu kullanmak `CheckedItemPosition` tamsayı özelliği:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Hangi satır seçilmedi belirlemek için `Multiple` döngü gereken modu `CheckedItemPositions` `SparseBooleanArray`. Aramakta tüm diziyi geçmesi gereken şekilde bir seyrek yalnızca girişler içeren bir sözlük gibi değeri değiştirildi Burada, dizidir `true` ne listesinde aşağıdaki kod parçacığında gösterildiği gibi seçilmiş öğrenmek için değerler:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```

<a name="Creating_Custom_Row_Layouts" />

## <a name="creating-custom-row-layouts"></a>Özel satır düzenleri oluşturma

Dört yerleşik satır görünümleri çok basittir. Daha karmaşık düzenleri (örneğin, e-postalar, veya tweet'leri veya kişi bilgilerini listesi) görüntülemek için özel görünüm gereklidir. Özel görünümler AXML dosyalar genellikle bildirildiğinde **kaynakları/düzeni** dizin ve ardından yüklenen kendi kaynak kimliği bir özel bağdaştırıcısı kullanarak. Görünümü görüntüleme sınıfları (örneğin, TextViews, ImageViews ve diğer denetimlerin) herhangi bir sayıda özel renkleri, yazı tipleri ve düzeni içerebilir.

Bu örnek, çeşitli yollarla önceki örneklerde farklıdır:

-  Öğesinden devralınan `Activity` değil `ListActivity` . Satırlar için özelleştirebileceğiniz `ListView` , diğer denetimler de eklenebilir ancak bir `Activity` Düzen (örneğin, bir başlığı, düğmeler veya diğer kullanıcı arabirimi öğeleri). Bu örnek, yukarıdaki bir başlık ekler `ListView` göstermek için.

-  Bir AXML düzeni dosyası için ekran gerektirir; Önceki örneklerde `ListActivity` düzeni dosyasını gerektirmez. Bu AXML içeren bir `ListView` denetim bildirimi.

-  Her satır işlemek için bir AXML düzeni dosyası gerektirir. Bu AXML dosya metin ve resim denetimleri özel yazı tipi ve renk ayarlarını içerir.

-  Satır görünümü seçildiğinde ayarlamak için isteğe bağlı özel Seçici XML dosyasını kullanır.

-  `Adapter` Uygulama döndürür özel düzeninden `GetView` geçersiz kılar.

-  `ItemClick` farklı bildirilmesi gerekir (bir olay işleyicisi bağlı `ListView.ItemClick` bir geçersiz kılma yerine `OnListItemClick` içinde `ListActivity`).


Bu değişiklikler, etkinliğin görünümü ve özel satır görünümü oluşturma ve bunları işlemek için bağdaştırıcı ve etkinlik değişiklikleri kapsayan başlayarak, aşağıda açıklanmıştır.

<a name="Adding_a_ListView_to_an_Activity_Layout" />

### <a name="adding-a-listview-to-an-activity-layout"></a>Bir etkinlik düzene ListView ekleme

Çünkü `HomeScreen` artık devraldığı `ListActivity` düzeni AXML dosyasını HomeScreen'ın görünümünü oluşturulmalıdır için varsayılan görünüm yok. Bu örnekte, bir başlığı görünüm olacaktır (kullanarak bir `TextView`) ve bir `ListView` verileri görüntülemek için. Düzen tanımlanan **Resources/Layout/HomeScreen.axml** burada gösterilen dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

Kullanmanın faydası bir `Activity` olan özel bir düzen (yerine bir `ListActivity`) ekranına başlığı gibi ek denetimleri ekleme yapamamasına içinde kaynaklandığını `TextView` Bu örnekte.

<a name="Creating_a_Custom_Row_Layout" />

### <a name="creating-a-custom-row-layout"></a>Özel satır düzeni oluşturma

Başka bir AXML düzeni dosyasını liste görünümünde görünür her satır için özel düzen içermesi gerekir. Bu örnekte, Yeşil arka plan, Kahverengi metin ve görüntü sağa hizalı satır sahip olur. Bu düzen bildirmek için Android XML Biçimlendirme açıklanan **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

Özel sıra düzenini birçok farklı denetim içerebilir ancak performans kaydırma karmaşık tasarımları tarafından etkilenebilir ve (özellikle, ağ üzerinden yüklenecek varsa) kullanarak görüntüleri. Kaydırma performans sorunlarını çözdükten üzerinde daha fazla bilgi için Google makalesine bakın.

<a name="Referencing_a_Custom_Row_View" />

### <a name="referencing-a-custom-row-view"></a>Bir özel satır görünümü başvurma

Özel bağdaştırıcı örnek olarak uygulamasıdır `HomeScreenAdapter.cs`. Anahtar yöntemi `GetView` yüklerken burada kaynak Kimliğini kullanarak özel AXML `Resource.Layout.CustomView`ve ardından her onu dönmeden önce görünümünde denetimlerin özelliklerini ayarlar. Tam bağdaştırıcısı sınıfı gösterilir:

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```

<a name="Referencing_the_Custom_ListView_in_the_Activity" />

### <a name="referencing-the-custom-listview-in-the-activity"></a>Özel ListView etkinliğinde başvurma

Çünkü `HomeScreen` şimdi sınıfının devraldığı `Activity`, `ListView` AXML bildirilen denetlemek için bir başvuru tutmak için sınıf içinde bildirilen alan:

```csharp
ListView listView;
```

Sınıfı etkinliğe ilişkin özel düzen AXML sonra yüklemelisiniz kullanarak `SetContentView` yöntemi. Ardından bulabilirsiniz `ListView` düzeni denetiminde sonra oluşturur ve bağdaştırıcı atar ve click işleyicisi atar. OnCreate yöntemi için kod aşağıda gösterilmiştir:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Son olarak `ItemClick` işleyici tanımlanmalıdır; bu durumda yalnızca görüntüler bir `Toast` ileti:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Sonuçta elde edilen ekran şöyle görünür:

[![Sonuçta elde edilen CustomRowView ekran görüntüsü](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png)


<a name="Customizing_the_Row_Selector_Color" />

### <a name="customizing-the-row-selector-color"></a>Satır seçici renk özelleştirme

Bir satır işlemdeki olduğunda kullanıcı geri bildirim için vurgulanmış olması gerekir. Özel bir görünüm belirttiğinde arka plan rengi olarak olarak **CustomView.axml** değil ayrıca geçersiz kılar seçim Vurgusu. Bu kod satırı **CustomView.axml** açık yeşil, ancak arka plan anlamına da gelir yapıldığında görsel bir gösterge satır işlemdeki kümeleri:

```xml
android:background="#FFDAFF7F"
```

Vurgu davranışı yeniden etkinleştirmek için ve ayrıca kullanılan rengi özelleştirmek için arka plan özniteliği için özel bir seçici ayarlayın. Seçici hem varsayılan arka plan rengi, hem de vurgulama renk bildirir. Dosya **Resources/Drawable/CustomSelector.xml** aşağıdaki bildirimini içerir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

Özel Seçici başvurmak için arka plan özniteliğinde değiştirin **CustomView.axml** için:

```xml
android:background="@drawable/CustomSelector"
```

Seçilen satırın ve karşılık gelen `Toast` iletisi şimdi şöyle görünür:

[![Seçilen satırın seçili satır adını görüntüleme bildirim iletisi turuncu](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png)


<a name="Preventing_Flickering_on_Custom_Layouts" />

### <a name="preventing-flickering-on-custom-layouts"></a>Özel düzenleri titremeyi önleme

Android çalışır performansını artırmak `ListView` düzen bilgilerini önbelleğe alarak kaydırma. Uzun veri listeleri kaydırma varsa, aynı zamanda ayarlamalısınız `android:cacheColorHint` özellikte `ListView` bildiriminde etkinliğin AXML tanımına (özel satır düzeni ait arka plan aynı renk değeri). Bu ipucu dahil etmek için başarısız bir 'titreşimi' kullanıcı kayarken olarak özel satır arka plan renklerini listesiyle aracılığıyla neden olabilir.



## <a name="related-links"></a>İlgili bağlantılar

- [BuiltInViews (örnek)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (örnek)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (örnek)](https://developer.xamarin.com/samples/CustomRowView/)
