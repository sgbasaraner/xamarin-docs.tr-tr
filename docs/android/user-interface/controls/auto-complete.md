---
title: Otomatik Tamamlama
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/31/2018
ms.openlocfilehash: 134b8e93279dd60f860e44a444e75e0200e66b99
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "30764456"
---
# <a name="auto-complete"></a>Otomatik Tamamlama

`AutoCompleteTextView` kullanıcının karşın, otomatik olarak tamamlama önerileri gösterir bir düzenlenebilir metin görünümü öğesidir. Bir açılan menüden kullanıcı düzenleme kutusu içeriğini değiştirmek için bir öğe seçebilir öneriler listesi görüntülenir.

![Otomatik Tamamlama örneği](images/auto-complete.png)

## <a name="overview"></a>Genel Bakış

Otomatik Tamamlama önerileri sağlayan bir metin girişi pencere öğesi oluşturmak için kullanın [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
pencere öğesi. Öneriler pencere öğesi ile ilişkili dizelerin bir koleksiyondan alınan bir [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/).

Bu öğreticide, oluşturacağınız bir [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
ülke adı için öneriler sağlar. pencere öğesi.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

[ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) Tanıtan bir etiket [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
pencere öğesi.


## <a name="tutorial"></a>Eğitmen

Adlı yeni bir proje başlatın *HelloAutoComplete*.

Adlı bir XML dosyası oluşturun `list_item.xml` ve içinde kaydederek **kaynakları/Düzen** klasör. Bu dosyanın derleme eylemi ayarlamak `AndroidResource`. Bu şekilde dosyayı düzenleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>

<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp"
    android:textColor="#000">
</TextView>
```

Bu dosya, basit bir tanımlar [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) önerileri listesinde görüntülenen her öğe için kullanılır.

Açık **Resources/Layout/Main.axml** ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

Açık **MainActivity.cs** ve aşağıdaki kodu ekleyin [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    AutoCompleteTextView textView = FindViewById<AutoCompleteTextView> (Resource.Id.autocomplete_country);
    var adapter = new ArrayAdapter<String> (this, Resource.Layout.list_item, COUNTRIES);

    textView.Adapter = adapter;
}
```

İçerik görünümü ayarladıktan sonra `main.xml` düzeni [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
pencere öğesi ile düzeninden yakalanır [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). Yeni bir [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) bağlamak için başlatılır `list_item.xml` her bir liste öğesi için Düzen `COUNTRIES` (sonraki adımda tanımlanan) dize dizisi. Son olarak, `SetAdapter()` ilişkilendirmek için çağrılan [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) ile [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
pencere öğesi böylece dize dizisi öneriler listesini doldurur.

İçinde `MainActivity` sınıfı, dize dizisi olarak ekleyin:

```csharp
static string[] COUNTRIES = new string[] {
  "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra",
  "Angola", "Anguilla", "Antarctica", "Antigua and Barbuda", "Argentina",
  "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium",
  "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia",
  "Bosnia and Herzegovina", "Botswana", "Bouvet Island", "Brazil", "British Indian Ocean Territory",
  "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi",
  "Cote d'Ivoire", "Cambodia", "Cameroon", "Canada", "Cape Verde",
  "Cayman Islands", "Central African Republic", "Chad", "Chile", "China",
  "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo",
  "Cook Islands", "Costa Rica", "Croatia", "Cuba", "Cyprus", "Czech Republic",
  "Democratic Republic of the Congo", "Denmark", "Djibouti", "Dominica", "Dominican Republic",
  "East Timor", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Ethiopia", "Faeroe Islands", "Falkland Islands", "Fiji", "Finland",
  "Former Yugoslav Republic of Macedonia", "France", "French Guiana", "French Polynesia",
  "French Southern Territories", "Gabon", "Georgia", "Germany", "Ghana", "Gibraltar",
  "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau",
  "Guyana", "Haiti", "Heard Island and McDonald Islands", "Honduras", "Hong Kong", "Hungary",
  "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
  "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
  "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg",
  "Macau", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands",
  "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia",
  "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
  "Nicaragua", "Niger", "Nigeria", "Niue", "Norfolk Island", "North Korea", "Northern Marianas",
  "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay", "Peru",
  "Philippines", "Pitcairn Islands", "Poland", "Portugal", "Puerto Rico", "Qatar",
  "Reunion", "Romania", "Russia", "Rwanda", "Sqo Tome and Principe", "Saint Helena",
  "Saint Kitts and Nevis", "Saint Lucia", "Saint Pierre and Miquelon",
  "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Saudi Arabia", "Senegal",
  "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands",
  "Somalia", "South Africa", "South Georgia and the South Sandwich Islands", "South Korea",
  "Spain", "Sri Lanka", "Sudan", "Suriname", "Svalbard and Jan Mayen", "Swaziland", "Sweden",
  "Switzerland", "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "The Bahamas",
  "The Gambia", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey",
  "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", "Virgin Islands", "Uganda",
  "Ukraine", "United Arab Emirates", "United Kingdom",
  "United States", "United States Minor Outlying Islands", "Uruguay", "Uzbekistan",
  "Vanuatu", "Vatican City", "Venezuela", "Vietnam", "Wallis and Futuna", "Western Sahara",
  "Yemen", "Yugoslavia", "Zambia", "Zimbabwe"
};
```

Kullanıcının içine girdiğinde, aşağı açılan listede sağlanan öneriler listesidir [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
pencere öğesi.

Uygulamayı çalıştırın. Siz yazarken, şöyle bir şey görmeniz gerekir:

[![Örnek ekran otomatik tamamlama "ca" içeren adları listeleme](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>Daha fazla bilgi

Uygulama kodunuza davranışı, içeriğin değil odaklanmanız gerekir çünkü sabit kodlanmış bir dize dizisi kullanarak önerilen tasarım uygulama olmadığını unutmayın. Dizeleri gibi uygulama içeriği, içerik değişiklikleri kolaylaştırmak ve yerelleştirme içeriğin kolaylaştırmak için koddan externalized. Sabit kodlanmış dizeleri, bu öğreticide yalnızca basit hale getirmek ve odaklanmak için kullanılır [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
pencere öğesi. Bunun yerine, uygulamanızın bir XML dosyasında tür dizesi diziler bildirmelidir. Bu ile yapılabilir bir `<string-array>` kaynak projenizdeki `res/values/strings.xml` dosya. Örneğin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
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

Bu kaynak dizeleri için kullanılacak [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), orijinalin yerine geçecek [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
Aşağıdaki satırı Oluşturucu:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>Referanslar

-   [AutoCompleteTextView tarif](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input) &ndash; Xamarin.Android örnek projesini `AutoCompleteTextView`.
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
-   [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)

*Bu sayfanın bölümleri olan oluşturulan Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş tabanlı değişiklikleri* 
 [ *Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/) *. Bu öğreticide dayanır* 
 [ *Android otomatik tam öğretici*](http://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
 *.*
