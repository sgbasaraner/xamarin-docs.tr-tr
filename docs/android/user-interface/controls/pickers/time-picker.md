---
title: Saat Seçici
description: Bir kez TimePickerDialog ve DialogFragment seçme
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85d887cba7a596226d44bc13ca7155bc4a0d03ee
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242309"
---
# <a name="time-picker"></a>Saat Seçici

Kullanıcı bir zamanı seçmek bir yol sunmak üzere kullanabileceğiniz [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Android uygulamaları genellikle kullanma `TimePicker` ile [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) bir zaman değeri seçme &ndash; bu cihazlar ve uygulamalar arasında tutarlı bir arayüz sağlamaya yardımcı olur. `TimePicker` 24 saat veya 12 saatlik AM/PM modunda günün saatini seçmek kullanıcıların sağlar.
`TimePickerDialog` kapsülleyen bir yardımcı sınıftır `TimePicker` bir iletişim kutusu.

[![Örnek zaman Seçici iletişim kutusunun ekran görüntüsü, eylem](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Genel Bakış

Modern Android uygulamaları görüntülemek `TimePickerDialog` içinde bir [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Bu uygulamanın görüntülenecek mümkün kılar `TimePicker` bir açılır iletişim kutusu olarak veya bir etkinlik ekleyin. Ayrıca, `DialogFragment` yaşam döngüsü ve uygulanması gereken kod miktarını azaltarak iletişim görünümünü yönetir.

Bu kılavuz nasıl kullanılacağını gösteren `TimePickerDialog`, içinde sarmalanmış bir `DialogFragment`. Örnek uygulama görüntüler `TimePickerDialog` kullanıcı bir etkinlik bir düğmeye tıkladığında kalıcı bir iletişim kutusu olarak. Kullanıcı tarafından saat olarak ayarlandığında iletişim çıkar ve bir işleyici güncelleştirmeleri bir `TextView` etkinlik ekranında, seçilen süre ile.

## <a name="requirements"></a>Gereksinimler

Bu kılavuz için örnek uygulamayı Android 4.1 (API düzeyi hedefler
16) ya da daha yüksek, ancak Android 3.0 (API düzeyi 11 veya üstü) ile kullanılabilir. Eski sürümleriyle Android projesi ve bazı kod değişikliklerini Android kitaplık desteği v4 eklenmesini desteklemek mümkündür.

## <a name="using-the-timepicker"></a>TimePicker kullanma

Bu örnekte genişletir `DialogFragment`; öğesinin alt sınıfı uygulaması `DialogFragment` (adlı `TimePickerFragment` aşağıda) barındıran ve görüntüler bir `TimePickerDialog`. Örnek uygulamayı ilk kez başlatıldığında, bu görüntüler bir **ÇEKME süresi** yukarıdaki düğmesini bir `TextView` seçilen zaman görüntülemek için kullanılır:

[![İlk örnek uygulaması ekranı](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Tıkladığınızda **ÇEKME süresi** button, örnek uygulaması açılır `TimePickerDialog` bu ekran görüntüsünde görüldüğü gibi:

[![Uygulama tarafından görüntülenen varsayılan saat Seçici iletişim kutusunun ekran görüntüsü](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

İçinde `TimePickerDialog`, teker seçip tıklatarak **Tamam** düğmesini nedenleri `TimePickerDialog` yöntemi çağırmak mı [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Bu arabirim barındırarak uygulanır `DialogFragment` (`TimePickerFragment`aşağıda açıklandığı gibi). Tıklayarak **iptal** düğmesi oluşturmayabilir için iletişim ve parça neden olur.

`DialogFragment` Seçilen zamanda barındırma Actvity üç yoldan birini döndürür:

1. **Bir özelliği ayarlamak veya yöntemi çağırma** &ndash; etkinlik, bir özellik veya yöntem bu değeri ayarlamak için özel olarak sağlayabilir.

2. **Olay bildirmek** &ndash; `DialogFragment` olacak bir olayı tanımlayabilir arandığında `OnTimeSet` çağrılır.

3. **Kullanarak bir `Action`**  &ndash; `DialogFragment` çağırabilirsiniz bir `Action<DateTime>` etkinliğinde bir saati görüntüleme. Etkinlik sağlayacak `Action<DateTime` örneklerken `DialogFragment`. 

Bu örnek etkinlik kaynağı gerektiren üçüncü teknik kullanacağı bir `Action<DateTime>` işleyicisine `DialogFragment`.



## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Adlı yeni bir Android projesi Başlat **TimePickerDemo** (Xamarin.Android projeleri oluşturma ile ilgili bilgi sahibi değilseniz bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) yeni bir proje oluşturma konusunda bilgi almak için).

Düzen **Resources/layout/Main.axml** ve içeriğini aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

Bu, temel bir [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) ile bir [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) zaman görüntüleyen ve [düğmesi](https://developer.xamarin.com/api/type/Android.Widget.Button/) açılır `TimePickerDialog`. Bu düzen uygulama daha basit ve daha kolay anlaşılır hale getirmek için sabit kodlanmış dizeleri ve boyutları kullandığına dikkat edin &ndash; bir üretim uygulaması için bu değerleri normalde kaynaklarını kullanır (de görüldüğü gibi [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) kod örneği).

Düzen **MainActivity.cs** ve içeriğini aşağıdaki kodla değiştirin:

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

Derleme ve bu örneği çalıştırmak, aşağıdaki ekran görüntüsüne benzer bir başlangıç ekranı görmeniz gerekir:

[![Başlangıç uygulaması ekranı](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Tıklayarak **ÇEKME süresi** düğmesi hiçbir şey yapmaz çünkü `DialogFragment` henüz görüntülenecek uygulanmadı `TimePicker`.
Bu oluşturma sonraki adımdır `DialogFragment`.



## <a name="extending-dialogfragment"></a>DialogFragment genişletme

Genişletmek için `DialogFragment` ile kullanılmak üzere `TimePicker`, türetilen öğesinin alt sınıfı oluşturmak gerekli olan `DialogFragment` ve uygulayan `TimePickerDialog.IOnTimeSetListener`. Aşağıdaki sınıfa eklemek **MainActivity.cs**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

Bu `TimePickerFragment` sınıfı daha küçük parçalara ayrılmış ve sonraki bölümde açıklanmıştır.


### <a name="dialogfragment-implementation"></a>DialogFragment uygulama

`TimePickerFragment` çeşitli yöntemler uygular: bir Üreteç yöntemi, bir iletişim kutusu örnek oluşturma metodu ve `OnTimeSet` gerektirdiği işleyici yöntemini `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` sınıfıdır `DialogFragment`. Ayrıca uygulayan `TimePickerDialog.IOnTimeSetListener` arabirimi (diğer bir deyişle, gerekli kaynakları `OnTimeSet` yöntemi):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` günlüğe kaydetme amacıyla başlatılır (*MyTimePickerFragment* değiştirilebilmesi için kullanmak istediğiniz herhangi bir dize olarak). `timeSelectedHandler` Eylem null başvuru özel durumları engellemek için boş bir temsilciye başlatılır:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance` Yeni bir örneğini oluşturmak için Üreteç yöntemi çağrıldığında `TimePickerFragment`. Bu yöntem bir `Action<DateTime>` kullanıcı tıkladığında, çağrılan işleyici **Tamam** düğmesine `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Görüntülenecek parçası olan Android çağırır `DialogFragment` yöntemi [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Bu yöntem yeni bir oluşturur `TimePickerDialog` nesne ve etkinlik, geri çağırma nesnesi başlatır (geçerli örneği olduğu `TimePickerFragment`) ve geçerli zaman:

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

-   Kullanıcı zaman ayar değiştiğinde `TimePicker` iletişim kutusunda `OnTimeSet` yöntemi çağrılır. `OnTimeSet` oluşturur bir `DateTime` geçerli tarih ve zaman (saat ve dakika) kullanıcı tarafından seçilen birleştirmeleri kullanarak nesne:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   Bu `DateTime` nesnesi `timeSelectedHandler` ile kayıtlı `TimePickerFragment` olacak şekilde oluşturulma zamanında nesne. `OnTimeSet` (Bu işleyici, sonraki bölümde uygulanır) seçilen zaman etkinliğin zaman görüntüsünü güncelleştirmek için bu işleyici çağırır:

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment görüntüleme

Şimdi `DialogFragment` olmuştur uygulanan örneklendirilecek zamanı `DialogFragment` kullanarak `NewInstance` Üreteç yöntemi ve çağırarak görüntüleme [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

Aşağıdaki yöntemi ekleyin `MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

Sonra `TimeSelectOnClick` örnekleyen bir `TimePickerFragment`, oluşturur ve etkinliğin saat gösterimini geçirilen zaman değeri ile güncelleştirir anonim bir yöntem için bir temsilci geçirir. Son olarak, başlatan `TimePicker` iletişim parça (aracılığıyla `DialogFragment.Show`) görüntülenecek `TimePicker` kullanıcı.

Sonunda `OnCreate` yöntemi, olay işleyicisi eklemek için aşağıdaki satırı ekleyin **ÇEKME süresi** iletişim kutusunu başlatan bir düğme:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Zaman **ÇEKME süresi** düğmesine tıklandığında, `TimeSelectOnClick` görüntülemek için çağrılacak `TimePicker` kullanıcıya iletişim parça.



## <a name="try-it"></a>Deneyin!

Derleme ve uygulamayı çalıştırın. Tıkladığınızda **ÇEKME süresi** düğme `TimePickerDialog` etkinlik (Bu durumda, 12 saatlik AM/PM modu) için varsayılan saat biçiminde görüntülenir:

[![Zaman iletişim AM/PM modunda görüntülenir.](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Tıkladığınızda **Tamam** içinde `TimePicker` iletişim kutusunda, işleyici güncelleştirir etkinliğin `TextView` seçilen zaman ve ardından çıkış yapar:

[![A/dk zaman içinde etkinlik TextView görüntülenir](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Ardından, aşağıdaki kod satırını ekleyin `OnCreateDialog` hemen sonra `is24HourFormat` bildirilir ve başlatılır:

```csharp
is24HourFormat = true;
```

Bu değişiklik geçirilen bayrağı zorlar `TimePickerDialog` olmasını Oluşturucusu `true` bu nedenle, 24 saatlik modu barındırma etkinliğin saat biçimi yerine kullanılır. Derleme ve uygulamayı yeniden çalıştırın, tıklayın **ÇEKME süresi** düğme `TimePicker` iletişim 24 saat biçiminde görüntülenir:

[![24 saat biçiminde TimePicker iletişim](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

İşleyici çağırır çünkü [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) etkinliğin zaman yazdırmak için `TextView`, zaman, hala varsayılan 12 saatlik AM/PM biçiminde yazdırılır.



## <a name="summary"></a>Özet

Bu makalede açıklanan nasıl görüntüleneceği bir `TimePicker` pencere öğesi olarak bir açılan pencere kalıcı iletişim kutusunu bir Android etkinliği. Bir örnek sağlanan `DialogFragment` uygulama ve ele `IOnTimeSetListener` arabirimi. Bu örnekte de gösterildiği nasıl `DialogFragment` seçilen zaman görüntülemek için etkinliği konak ile etkileşim kurabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
