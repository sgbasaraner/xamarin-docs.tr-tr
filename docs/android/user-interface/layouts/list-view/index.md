---
title: ListView
description: ListView önemli bir Android uygulamaları UI bileşenidir; Kişiler veya Internet Sık Kullanılanlar uzun listelerine kısa listesini menü seçeneklerini her yerde kullanılır. Kaydırma ya da yerleşik tarzıyla biçimlendirilen ya da yaygın özelleştirilmiş satır listesini sunmak için basit bir yol sağlar.
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2018
ms.openlocfilehash: 8499b9f186c12df22518893b6677cab22f0a3568
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="listview"></a>ListView

_ListView önemli bir Android uygulamaları UI bileşenidir; Kişiler veya Internet Sık Kullanılanlar uzun listelerine kısa listesini menü seçeneklerini her yerde kullanılır. Kaydırma ya da yerleşik tarzıyla biçimlendirilen ya da yaygın özelleştirilmiş satır listesini sunmak için basit bir yol sağlar._


## <a name="overview"></a>Genel Bakış

Liste görünümleri ve bağdaştırıcıları en temel yapı taşları Android uygulamaları dahil edilir. `ListView` Sınıfı kısa menü veya uzun kayan listesini olup esnek bir şekilde mevcut verilere sağlar. Hızlı kaydırma, dizinler ve uygulamalarınız için mobil dostu kullanıcı arabirimleri oluşturmanıza yardımcı olmak için tek veya birden çok seçim gibi kullanılabilirlik özellikleri sağlar. A `ListView` örneği gerektirir bir *bağdaştırıcısı* satır görünümlerinde yer alan verilerle akışına.

Bu kılavuz nasıl uygulanacağını anlatır `ListView` ve çeşitli `Adapter` Xamarin.Android sınıflarda. Ayrıca görünümünü özelleştirmek nasıl gösteren bir `ListView`, ve bellek kullanımını azaltmak için satır yeniden kullanım önemini anlatılmaktadır. Ayrıca bazı tartışma etkinlik yaşam döngüsü etkileme olan `ListView` ve `Adapter` kullanın. Xamarin.iOS ile platformlar arası uygulamalar üzerinde çalışıyorsanız `ListView` denetim benzer iOS yapısal olarak `UITableView` (ve Android `Adapter` benzer `UITableViewSource`).

İlk olarak, kısa bir öğretici tanıtır `ListView` temel kod örneği ile. Ardından, kullanmanıza yardımcı olması için daha gelişmiş konulara bağlantılar sağlanır `ListView` gerçek uygulamalarda.


> [!NOTE]
> `RecyclerView` Pencere öğesi olan bir daha gelişmiş ve esnek sürümü `ListView`. Çünkü `RecyclerView` ardıl olacak şekilde tasarlanmıştır `ListView` (ve `GridView`), kullanmanızı öneririz `RecyclerView` yerine `ListView` yeni uygulama geliştirme için. Daha fazla bilgi için bkz: [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).



## <a name="listview-tutorial"></a>ListView Öğreticisi

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) olan bir [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) kaydırılabilir öğelerinin bir listesini oluşturur. Liste öğeleri listesini kullanarak otomatik olarak eklenen bir [ `IListAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/).

Bu öğreticide, bir dize diziden okuma ülke adları kaydırılabilir listesi oluşturacaksınız. Bir liste öğesi seçildiğinde, listede bir bildirim iletisi öğenin konumunu görüntüler.


Adlı yeni bir proje başlatın **HelloListView**.

Adlı bir XML dosyası oluşturma **list_item.xml** ve içinde kaydederek **kaynakları/Düzen/** klasörü. Aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

Bu dosya düzeni yerleştirilecek her bir öğe için tanımlar [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Açık `MainActivity.cs` ve genişletmek için sınıf değiştirme [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (yerine [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)):

```csharp
public class MainActivity : ListActivity
{
```

İçin aşağıdaki kodu ekleyin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

Bu düzen dosyasını etkinliğini yüklemez, bildirim (genellikle ile bunu [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))).
Bunun yerine, ayarı [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) özelliği otomatik olarak ekler bir [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) ekranının tamamını dolduracak şekilde [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/).
Bu yöntem alır bir [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), içine yerleştirilecek liste öğeleri dizisi yönetir [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).
[ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/) Oluşturucusu geçen uygulama [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/), (önceki adımda oluşturulan), her bir liste öğesi Düzen açıklamasını ve `T[]` veya [ `Java.Util.IList<T>` ](https://developer.xamarin.com/api/type/Java.Util.IList/) eklenecek nesneler dizisi [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) (sonraki tanımlanmış).

[ `TextFilterEnabled` ](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/) Metin için filtreleme özelliğini etkinleştirir [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/), böylece kullanıcı yazmaya başladığında listesi filtrelenir.

[ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) Olay işleyicileri tıklama için abone olmak için kullanılabilir. Bir öğe [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) olan tıklandığında, işleyici olarak adlandırılır ve [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) iletisi görüntülenir, metin tıklatılan öğesinden kullanma.

Liste öğesi tasarımları için kendi düzeni dosyası tanımlayarak yerine platform tarafından sağlanan kullanabilirsiniz [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).
Örneğin, kullanmayı deneyin `Android.Resource.Layout.SimpleListItem1` yerine `Resource.Layout.list_item`.

Aşağıdakileri ekleyin `using` deyimi:

```csharp
using System;
```
Ardından, aşağıdaki dize dizisi bir üyesi olarak eklemek `MainActivity`:

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

İçine yerleştirilecek bir dize dizisi budur [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Uygulamayı çalıştırın. Listeyi kaydırın veya onu filtrelemek için yazın ve sonra bir ileti görmek için bir öğeyi tıklatın. Şöyle bir şey görmeniz gerekir:

[![Örnek penceresinin ekran görüntüsü ListView ülke adları](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

Bir sabit kodlanmış bir dize dizisi kullanarak en iyi tasarım uygulama olmadığını unutmayın. Bir Basitlik için Bu öğreticide göstermek için kullanılan [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) pencere öğesi. İle gibi harici bir kaynak tarafından tanımlanan bir dize dizisi başvurmak için daha iyi uygulamadır bir `string-array` projenizdeki kaynak **Resources/Values/Strings.xml** dosya. Örneğin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
  <string-array name="countries_array">
    <item>Bahrain</item>
    <item>Bangladesh</item>
    <item>Barbados</item>
    <item>Belarus</item>
    <item>Belgium</item>
    <item>Belize</item>
    <item>Benin</item>
  </string-array>
</resources>
```

Bu kaynak dizeleri için kullanılacak [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), özgün dosyayı değiştirmek [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) aşağıdaki satırla:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```
Uygulamayı çalıştırın. Şöyle bir şey görmeniz gerekir:

[![Örnek penceresinin ekran görüntüsü ListView küçük adlarının listesi](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)


## <a name="going-further-with-listview"></a>ListView ile devam

Diğer konular (aşağıda bağlı) ile çalışma kapsamlı bir göz atalım `ListView` sınıfı ve farklı türdeki bağdaştırıcısı türleri ile kullanabilirsiniz. Yapı aşağıdaki gibidir:

-   **Görsel görünümünü** &ndash; bölümlerini `ListView` denetim ve nasıl çalışırlar.

-   **Sınıfları** &ndash; görüntülemek için kullanılan sınıfları genel bakış bir `ListView`.

-   **ListView içinde veri görüntüleme** &ndash; veri basit bir listesini görüntülemek nasıl; nasıl uygulanacağını `ListView's` kullanılabilirlik özellikleri; nasıl farklı yerleşik satır düzenleri; kullanılacağını ve nasıl satır görünümleri yeniden kullanarak bağdaştırıcıları bellek kaydedin.

-   **Özel Görünüm** &ndash; stilini değiştirme `ListView` özel düzenler, yazı tipleri ve renkler.

-   **SQLite kullanarak** &ndash; bir SQLite veritabanı ile verileri görüntülemek nasıl bir `CursorAdapter`.

-   **Etkinlik yaşam döngüsü** &ndash; uygularken tasarım konuları `ListView` etkinlikleri, burada çevriminin, verilerinizi doldurmanız gerekir ve ne zaman kaynakları serbest bırakmak.

Genel Bakış (altı bölümlere ayrılmış) tartışma başlar `ListView` nasıl kullanılacağını giderek daha karmaşık örnekleri sunmadan önce sınıfının kendisini.

-   [ListView Bölümleri ve İşlevleri](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [ListView verilerle doldurma](~/android/user-interface/layouts/list-view/populating.md)
-   [ListView’ın Görünümünü Özelleştirme](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [CursorAdapters Kullanma](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [ContentProvider Kullanma](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView ve Etkinlik Yaşam Döngüsü](~/android/user-interface/layouts/list-view/activity-lifecycle.md)


## <a name="summary"></a>Özet

Bu konular sunulan kümesini `ListView` ve bazı örnekleri yerleşik özelliklerini kullanmak sağlanan `ListActivity`. Özel uygulamaları ele `ListView` renkli düzenleri için izin verilen ve bir SQLite veritabanı kullanıyor ve bu etkinliği yaşam döngüsü alaka kısaca işlemdeki, `ListView` uygulaması.


## <a name="related-links"></a>İlgili bağlantılar

- [AccessoryViews (örnek)](https://developer.xamarin.com/samples/AccessoryViews/)
- [BasicTableAndroid (örnek)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (örnek)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [BuiltInViews (örnek)](https://developer.xamarin.com/samples/BuiltInViews/)
- [CustomRowView (örnek)](https://developer.xamarin.com/samples/CustomRowView/)
- [FastScroll (örnek)](https://developer.xamarin.com/samples/FastScroll/)
- [SectionIndex (örnek)](https://developer.xamarin.com/samples/SectionIndex/)
- [SimpleCursorTableAdapter (sample)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (örnek)](https://developer.xamarin.com/samples/CursorTableAdapter/)
- [Etkinlik yaşam döngüsü Öğreticisi](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Tabloları ve hücrelerde (Xamarin.iOS) ile çalışma](~/ios/user-interface/controls/tables/index.md)
- [ListView sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [ListActivity sınıf başvurusu](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [BaseAdapter sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [ArrayAdapter sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [CursorAdapter sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
