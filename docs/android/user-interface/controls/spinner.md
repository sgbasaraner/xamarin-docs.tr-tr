---
title: "Değer Değiştirici"
ms.topic: article
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b7c850d0ea06d69c3601081c1e9cde193903eb27
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="spinner"></a>Değer Değiştirici

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) öğeleri seçmek için aşağı açılan liste sunan bir pencere öğesi var. Bu kılavuz, seçili seçeneği ile ilişkili diğer değerler görüntülemek değişiklikler arkasından bir değer değiştiricinin tercih listesi görüntüler basit bir uygulamasının nasıl oluşturulacağını açıklar.

## <a name="basic-spinner"></a>Temel değiştiricisi

Bu öğreticinin ilk bölümü gezegenlerin listesini görüntüleyen bir basit değer değiştirici pencere öğesi oluşturacaksınız. Bir planet seçildiğinde, seçilen öğe bir bildirim iletisi görüntüler:

[![HelloSpinner uygulama örnek ekran görüntüleri](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Adlı yeni bir proje başlatın **HelloSpinner**.

Açık **Resources/Layout/Main.axml** ve aşağıdaki XML Ekle:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

Dikkat [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)'s `android:text` özniteliği ve [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)'s `android:prompt` her iki öznitelik aynı dize kaynağı başvuru. Bu metin, pencere öğesi için bir başlık olarak davranır. Uygulandığında [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/), başlık metnini pencere öğesini seçtikten sonra görüntülenen seçimi iletişim kutusunu görünür.

Düzen **Resources/Values/Strings.xml** ve şuna dosyaya değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

İkinci `<string>` öğesi tanımlar tarafından başvurulan başlık dize [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) ve [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) yukarıdaki düzeninde.
`<string-array>` Öğe listesi olarak görüntülenecek dize listesi tanımlar [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) pencere öğesi.

Şimdi açmak **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimi:

```csharp
using System;
```

Ardından, aşağıdaki kodu ekleyin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

Sonra `Main.axml` düzeni içerik görünümü olarak ayarlanmış [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) pencere öğesi ile düzeninden yakalanır [ `FindViewById<>(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
[ `CreateFromResource()` ](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/) Yöntemi sonra oluşturur Yeni bir [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), hangi bağlanır her öğe dize dizisinde ilk görünümünü [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) (her bir öğeyi değer değiştirici seçildiğinde nasıl görüneceğini olmayan). `Resource.Array.planets_array` Kimliği başvuruları `string-array` yukarıda tanımlanan ve `Android.Resource.Layout.SimpleSpinnerItem` kimliği platform tarafından tanımlanan standart değiştiricisi görünüm için bir düzen başvuruyor.
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/) Pencere açıldığında, her bir öğenin görünümünü tanımlamak için çağrılır. Son olarak, [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) tüm öğeleri ile ilişkilendirmek üzere ayarlanmış [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) ayarlayarak [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter) özelliği.

Şimdi gelen bir öğe seçildiğinde, uygulama notifys geri arama yöntemi sağlar [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/). İşte bu yöntem aşağıdaki gibi görünmelidir:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Gönderenin cast bir öğe seçildiğinde, bir [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) böylece öğelerine erişilebilir. Kullanarak `Position` özelliği `ItemEventArgs`, seçili nesnenin metin bulmak ve görüntülemek için kullanın bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/).

Uygulamayı çalıştırın; aşağıdaki gibi görünmelidir:

[![Değer değiştirici planet seçili Mars ile ekran örneği](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Anahtar/değer çiftleri kullanarak değiştiricisi

Genellikle kullanmak gerekli olan `Spinner` uygulamanız tarafından kullanılan verileri bir tür ile ilişkili anahtar değerleri görüntüler. Çünkü `Spinner` çalışmıyor anahtar/değer çiftleri ile doğrudan gerekir anahtar/değer çifti ayrı olarak depolamak, doldurmak `Spinner` anahtar değerleriyle değiştiricisi seçilen anahtarı konumunu sonra ilişkili veri değeri aramak için kullanın. 

Aşağıdaki adımlarda **HelloSpinner** uygulama değiştiren seçili planet için ortalama sıcaklığı görüntülemek için:

Aşağıdakileri ekleyin `using` ifadesine **MainActivity.cs**:

```csharp
using System.Collections.Generic;
```

Aşağıdaki örnek değişkeni ekleyin `MainActivity` sınıfı.
Bu liste gezegenlerin ve bunların ortalama etme için anahtar/değer çiftleri tutar:

```csharp
private List<KeyValuePair<string, string>> planets;
```

İçinde `OnCreate` yöntemi, önce aşağıdaki kodu ekleyin `adapter` bildirilmiş:

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

Bu kod gezegenlerin ve bunların ilişkili ortalama etme için basit bir deposu oluşturur. (Gerçek bir uygulamada, bir veritabanı genellikle anahtarları ve bunlarla ilişkili verilerin depolamak için kullanılır.)

Yukarıdaki kod sonra hemen anahtarları çıkarmak ve bunları listesini (sırayla) yerleştirin için aşağıdaki satırları ekleyin:

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

Bu listeye geçirmek `ArrayAdapter` Oluşturucusu (yerine `planets_array` kaynak):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Değiştirme `spinner_ItemSelected` böylece seçili konumun seçili planet ile ilişkili değer (sıcaklık) bakmak için kullanılır:

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Uygulamayı çalıştırın; bildirim aşağıdaki gibi görünmelidir:

[![Sıcaklık görüntüleme planet seçimi örneği](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)
   
  

## <a name="resources"></a>Kaynaklar

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).
