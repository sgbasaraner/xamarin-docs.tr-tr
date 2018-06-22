---
title: Özelleştirilmiş parça sınıfları
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: ab7045b16d441a3f2ad06c64d08b9b98135b18a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769253"
---
# <a name="specialized-fragment-classes"></a>Özelleştirilmiş parça sınıfları

Parçaları API bazı uygulamalarda bulunan daha yaygın işlevlerini kapsülleyen diğer alt sınıflar sağlar. Bu alt sınıfların şunlardır:

-   **ListFragment** &ndash; bu parça, bir dizi veya bir imleç gibi bir veri kaynağı bağlı öğeleri listesini görüntülemek için kullanılır.

-   **DialogFragment** &ndash; bu parça, bir iletişim kutusu çevresinde bir sarmalayıcı olarak kullanılır. Parça, etkinlik üstünde iletişim kutusu görüntüler.

-   **PreferenceFragment** &ndash; bu parça tercih nesneleri listeleri olarak göstermek için kullanılır.



## <a name="the-listfragment"></a>ListFragment

`ListFragment` Kavram ve işlevselliği için de çok benzer `ListActivity`; sunan bir sarmalayıcı olan bir `ListView` bir parçası olarak. Görüntünün gösterir altında bir `ListFragment` tablet ve bir telefon çalıştıran:

[![ListFragment ekran görüntüleri bir telefon ve tablet üzerinde](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>ListAdapter ile veri bağlama

`ListFragment` Sınıfı geçersiz kılmak gerekli değildir varsayılan bir düzen zaten sağlar `OnCreateView` içeriğini görüntülemek için `ListFragment`. `ListView` Verileri kullanarak bağlı bir `ListAdapter` uygulaması. Aşağıdaki örnek, nasıl bu basit bir dize dizisi kullanılarak yapılabilir gösterir:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

Ayarlarken `ListAdapter`, kullanmak önemlidir `ListFragment.ListAdapter` özelliği ve `ListView.ListAdapter` özelliği. Kullanarak `ListView.ListAdapter` önemli başlatma kodun atlanması neden olur.



### <a name="responding-to-user-selection"></a>Kullanıcı Seçimi yanıt

Bir uygulama için kullanıcı seçimlerini yanıt vermek için kılmalıdır `OnListItemClick` yöntemi. Aşağıdaki örnek, böyle bir olasılık gösterir:

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

Yukarıdaki, kullanıcı bir öğeyi seçtiğinde kodda `ListFragment`, yeni parça seçili öğe hakkında daha fazla ayrıntı gösteren barındırma etkinlik görüntülenir.



## <a name="dialogfragment"></a>DialogFragment

*DialogFragment* iletişim nesnesi etkinliğin penceresinin en üstünde float bir parça içinde görüntülemek için kullanılan bir parçası değil. Yönetilen iletişim (Android 3. 0'dan başlayarak) API'ları değiştirmek için tasarlanmıştır. Aşağıdaki ekran görüntüsünde bir örneği gösterilmektedir bir `DialogFragment`:

[![Yeni araç EditBox eklemek görüntüleme DialogFragment ekran görüntüsü](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

A `DialogFragment` sağlar durumu parça ve iletişim arasında tutarlı kalır. Tüm etkileşimleri ve iletişim nesnenin denetimini aracılığıyla olacağını `DialogFragment` API'sini ve iletişim nesnesi üzerinde doğrudan çağrılar ile yapılması değil. `DialogFragment` API sağlayan her örneği ile bir `Show()` bir parça görüntülemek için kullanılan yöntem. Bir parçasını kurtulmak için iki yolu vardır:

-  Çağrı `DialogFragment.Dismiss()` üzerinde `DialogFragment` örneği. 

-  Başka bir görüntüleme `DialogFragment`.

Oluşturmak için bir `DialogFragment`, bir sınıfının devraldığı `Android.App.DialogFragment,` ve ardından aşağıdaki iki yöntemden biri geçersiz kılar:

- **OnCreateView** &ndash; bu oluşturur ve bir görünüm verir.

- **OnCreateDialog** &ndash; bu özel bir iletişim kutusu oluşturur. Göstermek için genellikle kullanılan bir *AlertDialog*. Bu yöntemi geçersiz kılarken, geçersiz kılmak ise gerekli değildir `OnCreateView` .



### <a name="a-simple-dialogfragment"></a>Basit bir DialogFragment

Aşağıdaki ekran görüntüsü, basit bir gösterir `DialogFragment` olan bir `TextView` ve iki `Button`: %s

[![Örnek DialogFragment bir kutusu TextView ve iki düğmeleri](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView` Kullanıcı bir düğmesini tıklamıştır sayısı görüntülenir `DialogFragment`, diğer düğmesini tıklatarak parça kapanacak. Kodunu `DialogFragment` değil:

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```


### <a name="displaying-a-fragment"></a>Bir parça görüntüleme

Tüm parçaları gibi bir `DialogFragment` bağlamında görüntülenen bir `FragmentTransaction`.

`Show()` Yöntemi bir `DialogFragment` geçen bir `FragmentTransaction` ve `string` bir girdi olarak. İletişim kutusu etkinliğe eklenir ve `FragmentTransaction` kaydedilmiş.

Aşağıdaki kod, bir etkinlik kullanabilir olası bir yol gösterir `Show()` göstermek için yöntemi bir `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>Bir parça kapatılıyor

Çağırma `Dismiss()` örneği üzerinde bir `DialogFragment` etkinliğinden kaldırılacak bir parça neden olur ve bu işlem kaydeder.
Bir parça yok etme ile ilgili olan standart parça yaşam döngüsü yöntemlerini çağrılır.


### <a name="alert-dialog"></a>Uyarı iletişim kutusu

Geçersiz kılma yerine `OnCreateView`, `DialogFragment` yerine geçersiz kılabilir `OnCreateDialog`. Böylece, bir uygulama oluşturmak bir [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) bir parçası yönetilir. Aşağıdaki kodu kullanan bir örnek: `AlertDialog.Builder` oluşturmak için bir `Dialog`:

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```



## <a name="preferencefragment"></a>PreferenceFragment

Tercihler yönetmenize yardımcı olmak için parçaları API sağlar `PreferenceFragment` bir alt kümesi. `PreferenceFragment` Benzer [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/) &ndash; bir parçası olarak kullanıcı tercihleri hiyerarşisini gösterir. Kullanıcı tercihleri ile etkileşim gibi bunlar otomatik olarak kaydedilecek [SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html).
Android 3.0 ya da daha yüksek uygulamalar kullanmak `PreferenceFragment` uygulamalarında tercihleri uğraşmanız. Aşağıdaki resimde bir örneği gösterilmektedir bir `PreferenceFragment`:

[![Satır içi, iletişim ve başlatma seçeneklerle örnek PreferencesFragment](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>Bir tercih parça kaynağı oluşturma

Parça şişirileceğini XML kaynak dosyasından kullanarak tercih [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) yöntemi. Parça yaşam döngüsündeki bu yöntemi çağırmak için mantıksal bir yerde olması `OnCreate` yöntemi.

`PreferenceFragment` Resimdeki XML'den kaynak yükleyerek yukarıdaki oluşturuldu. Kaynak dosya adı:

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

Parça tercih için kod aşağıdaki gibidir:

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```



### <a name="querying-activities-to-create-a-preference-fragment"></a>Bir tercih parça oluşturmak için etkinlikleri sorgulama

Oluşturmak için başka bir yöntem bir `PreferenceFragment` etkinlikleri sorgulama içerir. Her etkinlik kullanabilirsiniz [meta veri\_anahtar\_TERCİH](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) XML kaynak dosyasına işaret edecek özniteliği. Xamarin.Android içinde bu olmayan bir etkinliği eklemek tarafından yapılır `MetaDataAttribute`ve kullanmak için kaynak dosyası belirtme. `PreferenceFragment` SAX yöntemi [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)) bu XML kaynak bulma ve onu için tercih hiyerarşi Şişir için bir etkinlik sorgulamak için kullanılabilir.

Bu işlem örneği kullanan aşağıdaki kod parçacığında, sağlanan `AddPreferencesFromIntent` oluşturmak için bir `PreferenceFragment`:

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

Android sınıfı görünür `MyActivityWithPreference`. Sınıf ile donatılacak `MetaDataAttribute,` aşağıdaki kod parçacığında gösterildiği gibi:

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

`MetaDataAttribute` Bildiren bir XML kaynak dosyası `PreferenceFragment` tercih hiyerarşi Şişir için kullanır. Varsa `MetatDataAttribute` çalışma zamanında bir özel durum atılır sonra sağlanmadı. Bu kod çalıştığında, `PreferenceFragment` aşağıdaki ekran görüntüsünde olduğu gibi görünür:

[![Görüntülenen PreferenceFragment örnek uygulamasının ekran görüntüsü](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
