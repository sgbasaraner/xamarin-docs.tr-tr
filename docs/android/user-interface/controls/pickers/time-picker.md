---
title: "Saat Seçici"
description: "TimePickerDialog ve DialogFragment kullanarak bir kez seçme"
ms.topic: article
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 93a2effd42432d13767dad05a47548aebc9a0b93
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="time-picker"></a>Saat Seçici

Kullanıcının bir zamanı seçmek bir yol sağlamak üzere kullanabileceğiniz [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Android uygulamaları genellikle kullanma `TimePicker` ile [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) bir saat değeri seçme &ndash; bu aygıtlar ve uygulamalar arasında tutarlı bir arayüz sağlamaya yardımcı olur. `TimePicker` Kullanıcıların günün saatini 24 saat veya 12 saatlik AM/PM modunda seçmesine izin verir.
`TimePickerDialog` yalıtan bir yardımcı sınıf olan `TimePicker` bir iletişim kutusu.

[![Eylem Saat Seçici iletişim kutusunun örnek ekran görüntüsü](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Genel Bakış

Modern Android uygulamaları görüntülemek `TimePickerDialog` içinde bir [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Bu uygulamaya görüntülemek için mümkün kılar `TimePicker` bir açılır iletişim kutusu olarak veya bir aktivite ekleme. Ayrıca, `DialogFragment` yaşam döngüsü ve uygulanmalı kod miktarının azaltılması iletişim görüntüsünü yönetir.

Bu kılavuz nasıl kullanılacağı ortaya `TimePickerDialog`, içinde Sarmalanan bir `DialogFragment`. Örnek uygulama görüntüler `TimePickerDialog` kullanıcı bir etkinlikte düğmesini tıklattığında kalıcı bir iletişim kutusu olarak. Zamanı kullanıcı tarafından ayarladığınızda, iletişim çıkar ve bir işleyici güncelleştirmeleri bir `TextView` seçilmedi saat ile etkinlik ekranında.

## <a name="requirements"></a>Gereksinimler

Bu kılavuz için örnek uygulama Android 4.1 (API düzeyi hedefler
16) ya da daha yüksek, ancak Android 3.0 (API düzeyi 11 veya daha yüksek) ile kullanılabilir. Eski sürümleriyle Android projesi ve bazı kod değişiklikleri Android destek kitaplığı v4 eklenmesi desteklemek mümkündür.

## <a name="using-the-timepicker"></a>TimePicker kullanma

Bu örnek genişletir `DialogFragment`; alt uyarlamasını `DialogFragment` (adlı `TimePickerFragment` aşağıda) barındırır ve görüntüler bir `TimePickerDialog`. Örnek uygulamayı ilk kez başlatıldığında, görüntülediği bir **ÇEKME zaman** yukarıdaki düğmesi bir `TextView` seçili zaman görüntülemek için kullanılacak:

[![İlk örnek uygulama ekran](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Tıkladığınızda **ÇEKME zaman** button, örnek uygulamayı başlatır `TimePickerDialog` bu ekran görüntüsünde görüldüğü gibi:

[![Uygulama tarafından görüntülenen varsayılan saat Seçici iletişim kutusunun ekran görüntüsü](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

İçinde `TimePickerDialog`, bir süre seçerek ve tıklatarak **Tamam** düğmesini nedenler `TimePickerDialog` yöntemini çağırmak için [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Bu arabirim barındırarak uygulanır `DialogFragment` (`TimePickerFragment`, aşağıda açıklanmıştır). Tıklatarak **iptal** düğmesi parça ve iletişim oluşturmayabilir neden olur.

`DialogFragment` Seçilen zaman barındırma Actvity üç yöntemden birini döndürür:

1. **Bir yöntemi çağırma veya bir özellik ayarlama** &ndash; etkinlik, bir özellik veya yöntem bu değeri ayarlamak için özel olarak sağlayabilir.

2. **Bir olayı tetiklenmeden** &ndash; `DialogFragment` olacak olay tanımlayabilirsiniz ne zaman yükseltilmiş `OnTimeSet` çağrılır.

3. **Kullanarak bir `Action`**  &ndash; `DialogFragment` çağırabileceği bir `Action<DateTime>` etkinliğin zaman görüntülemek için. Etkinlik sağlayacak `Action<DateTime` başlatılırken `DialogFragment`. 

Bu örnek etkinlik kaynağı gerektiren üçüncü teknik kullanacağı bir `Action<DateTime>` işleyicisine `DialogFragment`.



## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Adlı yeni bir Android projesi Başlat **TimePickerDemo** (Xamarin.Android projeleri oluşturma ile bilmiyorsanız bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) yeni bir proje oluşturmayı öğrenmek için).

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

Temel bir budur [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) ile bir [kutusu TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) saati görüntüler ve [düğmesini](https://developer.xamarin.com/api/type/Android.Widget.Button/) açılır `TimePickerDialog`. Bu düzen uygulama daha basit ve anlaşılması daha kolay hale getirmek için dizeleri sabit kodlanmış ve boyutları kullandığına dikkat edin &ndash; bir üretim uygulaması için bu değerleri normalde kaynakları kullanır (de görüldüğü gibi [DatePicker](https://github.com/xamarin/recipes/blob/master/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) kod örneğinde).

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

[![İlk uygulama ekran](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Tıklatarak **ÇEKME zaman** düğmesi hiçbir şey yapmaz çünkü `DialogFragment` henüz görüntülenecek uygulanmadı `TimePicker`.
Bu oluşturmak için sonraki adımdır `DialogFragment`.



## <a name="extending-dialogfragment"></a>DialogFragment genişletme

Genişletmek için `DialogFragment` ile kullanılmak üzere `TimePicker`, türetilmiş bir alt kümesi oluşturmak gerekli olan `DialogFragment` ve uygulayan `TimePickerDialog.IOnTimeSetListener`. Aşağıdaki sınıf ekleme **MainActivity.cs**:

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

Bu `TimePickerFragment` sınıfı daha küçük parçalara ayrıntılarıyla ve sonraki bölümde açıklanmıştır.


### <a name="dialogfragment-implementation"></a>DialogFragment uygulama

`TimePickerFragment` birkaç yöntemlerini uygular: Üreteç yöntemi, bir iletişim örnekleme yöntemi ve `OnTimeSet` gerektirdiği işleyici yöntemini `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` öğesinin bir alt kümesi olan `DialogFragment`. Ayrıca uygulayan `TimePickerDialog.IOnTimeSetListener` arabirimi (yani, gerekli kaynakları `OnTimeSet` yöntemi):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` günlüğe kaydetme amacıyla başlatıldı (*MyTimePickerFragment* değiştirilebilmesi için kullanmak istediğiniz herhangi bir dize). `timeSelectedHandler` Eylem null başvuru özel durumlarını önlemek için boş bir temsilci başlatılır:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance` Yeni bir örneğini oluşturmak için Üreteç yöntemi çağrıldığında `TimePickerFragment`. Bu yöntem alır bir `Action<DateTime>` kullanıcı tıklattığında çağrılan işleyici **Tamam** düğmesini `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Görüntülenecek parçası olduğunda, Android çağırır `DialogFragment` yöntemi [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Bu yöntem yeni bir oluşturur `TimePickerDialog` nesne ve etkinlik, geri çağırma nesnesi başlatır (geçerli örneği olduğu `TimePickerFragment`) ve geçerli saati:

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

-   Kullanıcı zaman ayarında değiştiğinde `TimePicker` iletişim kutusunda, `OnTimeSet` yöntemi çağrılır. `OnTimeSet` oluşturur bir `DateTime` geçerli tarih ve kullanıcı tarafından seçilen birleştirme süresi (saat ve dakika) kullanarak nesnesi:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   Bu `DateTime` nesne iletilir `timeSelectedHandler` ile kayıtlı `TimePickerFragment` oluşturma zamanında nesne. `OnTimeSet` (Bu işleyici bir sonraki bölümde uygulanır) seçili zaman etkinliğin zaman görüntüsünü güncelleştirmek için bu işleyiciyi çağırır:

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment görüntüleme

Şimdi `DialogFragment` bırakıldı uygulanan örneği oluşturmak için zamanı `DialogFragment` kullanarak `NewInstance` Üreteç yöntemi ve çağırarak görüntüleme [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

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

Sonra `TimeSelectOnClick` başlatır bir `TimePickerFragment`, oluşturur ve etkinliğin zaman görüntüsünü geçirilen süre değeri ile güncelleştirir anonim bir yöntemi için bir temsilci geçirir. Son olarak, başlatır `TimePicker` iletişim parça (aracılığıyla `DialogFragment.Show`) görüntülemek için `TimePicker` kullanıcı.

Sonunda `OnCreate` yöntemi, olay işleyicisi iliştirmek için aşağıdaki satırı ekleyin **ÇEKME zaman** iletişim başlatır düğmesi:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Zaman **ÇEKME zaman** düğmesine tıklandığında, `TimeSelectOnClick` görüntülemek için çağrılan `TimePicker` kullanıcıya iletişim parça.



## <a name="try-it"></a>Deneyin!

Derleme ve uygulamayı çalıştırın. Tıkladığınızda **ÇEKME zaman** düğmesini `TimePickerDialog` etkinliğinde (Bu durumda, 12 saatlik AM/PM modu) için varsayılan saat biçiminde görüntülenir:

[![AM/PM modunda zaman iletişim kutusu görüntülenir](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Tıkladığınızda **Tamam** içinde `TimePicker` iletişim kutusunda, işleyici güncelleştirmeleri etkinliğin `TextView` seçilen zaman ve ardından çıkar:

[![A/M zaman içinde etkinliğini kutusu TextView görüntülenir](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Sonra aşağıdaki kod satırını ekleyin `OnCreateDialog` hemen sonra `is24HourFormat` bildirilir ve başlatıldı:

```csharp
is24HourFormat = true;
```

Bu değişiklik geçirilen bayrak zorlar `TimePickerDialog` olmasını Oluşturucusu `true` , 24 saatlik modu barındırma etkinlik saat biçimi yerine kullanılmaktadır. Derleme ve uygulamayı yeniden çalıştırın, tıklatın **ÇEKME zaman** düğmesi, `TimePicker` iletişim kutusu artık 24 saat biçiminde gösterilir:

[![24 saat biçiminde TimePicker iletişim](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

İşleyici çağırması için [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) etkinliğin süresi yazdırmak için `TextView`, varsayılan 12 saatlik AM/PM biçiminde hala yazdırıldığında.



## <a name="summary"></a>Özet

Bu makalede açıklanan nasıl görüntüleneceği bir `TimePicker` pencere öğesi açılan kalıcı bir iletişim kutusu bir Android etkinliğin olarak. Bir örnek sağlanan `DialogFragment` uygulama ve ele `IOnTimeSetListener` arabirimi. Bu örnek ayrıca gösterilen nasıl `DialogFragment` seçili zaman görüntülemek için etkinliği ana bilgisayar ile etkileşim kurabilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
