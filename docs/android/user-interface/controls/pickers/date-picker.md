---
title: "Tarih Seçici"
description: "Takvim tarihleri DatePickerDialog ve DialogFragment kullanarak seçme"
ms.topic: article
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 3de935fd407524d7ba62a93205e333c7dd7adde0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="date-picker"></a>Tarih Seçici

## <a name="overview"></a>Genel Bakış

Ne zaman bir kullanıcı bir Android uygulamasına giriş gereken durumlar vardır. Bu konuda yardımcı olması için Android çerçevesi sağlar [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) pencere öğesi ve [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . `DatePicker` Kullanıcıların yıl, ay ve gün cihazlar ve uygulamalar arasında tutarlı bir arayüz seçmesine izin verir. `DatePickerDialog` Yalıtan bir yardımcı sınıf olan `DatePicker` bir iletişim kutusu.

Modern Android uygulamaları görüntülemesi gereken `DatePickerDialog` içinde bir [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Bu DatePicker popup iletişim kutusu olarak görüntülemek bir uygulama izin verir veya bir etkinlikte katıştırılmış. Ayrıca, `DialogFragment` yaşam döngüsü ve uygulanmalı kod miktarının azaltılması iletişim görüntüsünü yönetir.

Bu kılavuz nasıl kullanılacağı gösterilmektedir `DatePickerDialog`, içinde Sarmalanan bir `DialogFragment`. Örnek uygulamayı görüntüler `DatePickerDialog` kullanıcı bir etkinlikte düğmesini tıklattığında kalıcı bir iletişim kutusu olarak. Tarih kullanıcı tarafından ayarlanmış olduğunda bir `TextView` seçilmedi tarihi ile güncelleştirir.

[![Tarih Seçici iletişim kutusu tarafından izlenen çekme tarih ekran düğmesi](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png)

## <a name="requirements"></a>Gereksinimler

Bu kılavuz için örnek uygulama Android 4.1 (API düzeyi hedefler
16) ya da daha yüksek, ancak Android 3.0 (API düzeyi 11 veya üzeri) için geçerlidir. Eski sürümleriyle Android projesi ve bazı kod değişiklikleri Android destek kitaplığı v4 eklenmesi desteklemek mümkündür.

## <a name="using-the-datepicker"></a>DatePicker kullanma

Bu örnek uzatır `DialogFragment`. Bir alt konak ve görüntüleme bir `DatePickerDialog`:

![Tarih Closeup Seçici iletişim kutusu](date-picker-images/image-02.png)

Kullanıcı bir tarih seçer ve tıklar **Tamam** düğmesini `DatePickerDialog` yöntemi çağıracak [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Bu arabirim barındırarak uygulanır `DialogFragment`. Kullanıcı tıklarsa **iptal** düğmesi, parça ve iletişim kendilerini kapatmak sonra.

Birkaç yolu vardır `DialogFragment` seçilen tarihten barındırma etkinlik döndürebilirsiniz:

1. **Bir yöntemi çağırma veya bir özellik Ayarla** &ndash; etkinlik, bir özellik veya yöntem bu değeri ayarlamak için özel olarak sağlayabilir.

2. **Bir olayı** &ndash; `DialogFragment` olacak olay tanımlayabilirsiniz ne zaman yükseltilmiş `OnDateSet` çağrılır.

3. **Kullanım bir `Action`**  &ndash; `DialogFragment` çağırabileceği bir `Action<DateTime>` etkinliğin tarihini görüntülemek için. Etkinlik sağlayacak `Action<DateTime` başlatılırken `DialogFragment`. Bu örnek üçüncü teknik kullanıyor ve etkinlik sağlamanızı gerektiren bir `Action<DateTime>` için `DialogFragment`.


<a name="extending_dialogfragment" />

### <a name="extending-dialogfragment"></a>DialogFragment genişletme

Görüntüleme ilk adımı bir `DatePickerDialog` için sınıfıdır `DialogFragment` ve uygulama `IOnDateSetListener` arabirimi:

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

`NewInstance` Yöntemi yeni bir örneğini oluşturmak için çağrılır `DatePickerFragment`. Bu yöntem alır bir `Action<DateTime>` , çağrılabilir kullanıcı tıkladığında **Tamam** düğmesini `DatePickerDialog`.

Görüntülenecek parçası olduğunda, Android yöntemi çağırın `OnCreateDialog`. Bu yöntem yeni bir oluşturacak `DatePickerDialog` nesne ve geçerli tarih ve geri çağırma nesnesi ile başlatma (geçerli örneği olduğu `DatePickerFragment`).


> [!NOTE]
> **Not:** unutmayın, ay değeri olduğunda `IOnDateSetListener.OnDateSet` çağrılır 0-11 ve değil 1-12 aralığındadır. Ayın 1 için (bağlı olarak ay seçildiği) 31 Aralık içinde olacaktır.


<a name="date_picker_fragment" />

### <a name="showing-the-datepickerfragment"></a>DatePickerFragment gösterme

Şimdi `DialogFragment` bırakıldı uygulanan, bu bölümde bir etkinlikte parça kullanmayı inceleyeceksiniz. Bu kılavuzu eşlik eden örnek uygulamasında etkinliği örneği oluşturulmaz `DialogFragment` kullanarak `NewInstance` Üreteç yöntemi ve onu çağırma sonra görünen `DialogFragment.Show`. Örnek oluşturma bir parçası olarak `DialogFragment`, etkinlik geçirmeden bir `Action<DateTime>`, tarih görüntülenir bir `TextView` etkinlik tarafından barındırılan:

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

<a name="summary" />

## <a name="summary"></a>Özet

Bu örnek nasıl görüntüleneceğini ele alınan bir `DatePicker` pencere öğesi olarak açılan kalıcı bir iletişim kutusu bir Android etkinlik bir parçası olarak. Bir örnek DialogFragment uygulama sağlanan ve ele `IOnDateSetListener` arabirimi. Bu örnek ayrıca DialogFragment seçili tarihini görüntülemek için etkinliği ana bilgisayar ile nasıl etkileşim gösterilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Bir tarih seçin](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/android/controls/datepicker/select_a_date)
