---
title: Tarih Seçici
description: Takvim tarihlerini DatePickerDialog ile DialogFragment seçme
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 8f4e6318d904efb2f77c36732fc6519699f72ac9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241142"
---
# <a name="date-picker"></a>Tarih Seçici

## <a name="overview"></a>Genel Bakış

Ne zaman bir kullanıcı bir Android uygulamasına giriş verilerini gereken durumlar vardır. Bu konuda yardımcı olmak üzere Android çerçevesi sağlar [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) pencere öğesi ve [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . `DatePicker` Kullanıcıların yıl, ay ve gün cihazlarınız ve uygulamalarınız arasında tutarlı bir arayüz seçmesine izin verir. `DatePickerDialog` Kapsülleyen bir yardımcı sınıftır `DatePicker` bir iletişim kutusu.

Modern Android uygulamaları görüntülemesi gereken `DatePickerDialog` içinde bir [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Bu tarih seçici açılan iletişim kutusu olarak görüntülemek bir uygulama izin verir veya katıştırılmış bir etkinlik. Ayrıca, `DialogFragment` yaşam döngüsü ve uygulanması gereken kod miktarını azaltarak iletişim görünümünü yönetir.

Bu kılavuz nasıl kullanılacağını gösteren `DatePickerDialog`, içinde sarmalanmış bir `DialogFragment`. Örnek uygulamayı görüntüler `DatePickerDialog` kullanıcı bir etkinlik bir düğmeye tıkladığında kalıcı bir iletişim kutusu olarak. Tarih kullanıcı tarafından ayarlanmış olduğunda bir `TextView` seçilen tarihten güncelleştirir.

[![Tarih Seçici iletişim kutusu tarafından izlenen çekme tarih ekran düğmesi](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Gereksinimler

Bu kılavuz için örnek uygulamayı Android 4.1 (API düzeyi hedefler
16) ya da daha yüksek, ancak Android 3.0 (API düzeyi 11 veya üstü) için geçerlidir. Eski sürümleriyle Android projesi ve bazı kod değişikliklerini Android kitaplık desteği v4 eklenmesini desteklemek mümkündür.

## <a name="using-the-datepicker"></a>DatePicker kullanma

Bu örnek genişletecek `DialogFragment`. Alt ana bilgisayar ve görüntüleme bir `DatePickerDialog`:

![Tarih Seçici Closeup iletişim](date-picker-images/image-02.png)

Kullanıcı bir tarih seçer ve tıkladığında **Tamam** düğmesi `DatePickerDialog` yöntemini çağıracaksınız [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Bu arabirim barındırarak uygulanır `DialogFragment`. Kullanıcı tıklarsa **iptal** düğmesi, ardından parça ve iletişim kendilerini kapat.

Birkaç şekilde `DialogFragment` seçilen tarihten barındırma etkinliği döndürebilir:

1. **Bir yöntem çağırma veya bir özelliği ayarlamak** &ndash; etkinlik, bir özellik veya yöntem bu değeri ayarlamak için özel olarak sağlayabilir.

2. **Bir olay tetikleyebilir** &ndash; `DialogFragment` olacak bir olayı tanımlayabilir arandığında `OnDateSet` çağrılır.

3. **Kullanım bir `Action`**  &ndash; `DialogFragment` çağırabilirsiniz bir `Action<DateTime>` etkinliğinde tarih görüntülenecek. Etkinlik sağlayacak `Action<DateTime` örneklerken `DialogFragment`. Bu örnek üçüncü tekniği kullanın ve etkinlik sağlamasını bir `Action<DateTime>` için `DialogFragment`.



### <a name="extending-dialogfragment"></a>DialogFragment genişletme

Görüntüleme ilk adımı bir `DatePickerDialog` alt sınıfı için `DialogFragment` ve uygulama `IOnDateSetListener` arabirimi:

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

`NewInstance` Yöntemi yeni bir örneğini oluşturmak için çağrıldığında `DatePickerFragment`. Bu yöntem bir `Action<DateTime>` , çağrılacak kullanıcı tıkladığında **Tamam** düğmesine `DatePickerDialog`.

Görüntülenecek parçası olduğunda Android yöntemini çağıracaksınız `OnCreateDialog`. Bu yöntem yeni bir oluşturma `DatePickerDialog` nesne ve geçerli tarih ve geri çağırma nesnesi ile Başlat (geçerli örneği olduğu `DatePickerFragment`).


> [!NOTE]
> Unutmayın, ay değeri olduğunda `IOnDateSetListener.OnDateSet` çağrılan 0-11 ve değil 1-12 aralığında olan. Ayın gününü 1 için (bağlı olarak ay seçildiği) 31 aralığında olacaktır.



### <a name="showing-the-datepickerfragment"></a>DatePickerFragment gösteriliyor

Şimdi `DialogFragment` olmuştur uygulanır, bu bölümde bir etkinlikte parça kullanmayı inceleyeceksiniz. Bu kılavuzu eşlik eden örnek uygulamada, etkinlik örneği oluşturmak `DialogFragment` kullanarak `NewInstance` Üreteç yöntemi ve onu çağırmak sonra görünen `DialogFragment.Show`. Örnekleme işleminin bir parçası olarak `DialogFragment`, etkinliği geçirir bir `Action<DateTime>`, tarih görüntülenir bir `TextView` etkinlik tarafından barındırılır:

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```


## <a name="summary"></a>Özet

Bu örnek nasıl görüntüleneceğini ele alınan bir `DatePicker` Android etkinliği bir parçası olarak açılan kalıcı iletişim kutusu olarak pencere öğesi. Ele alınan ve örnek DialogFragment uygulama sağlanan `IOnDateSetListener` arabirimi. Bu örnek ayrıca DialogFragment seçilen tarihten görüntülemek için etkinliği konakla nasıl etkileşimde bulunabilir gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Bir tarih seçin](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
