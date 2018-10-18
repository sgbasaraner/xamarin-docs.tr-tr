---
title: Xamarin.iOS seçici denetimi
description: Bu belge, tasarım ve Seçici denetimleri bir Xamarin.iOS uygulaması ile çalışmak açıklar. Bu, kod ve iOS Designer bir seçici uygulamak nasıl ele alınmaktadır.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/14/2018
ms.openlocfilehash: 0ef33c2036b1ff2d5a7e2035ca5fa8af58672867
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "34789918"
---
# <a name="picker-control-in-xamarinios"></a>Xamarin.iOS seçici denetimi

A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) farenizin tekerleğini benzeri arabirimi bileşenlerini ayrı ayrı bir listeden bir değer seçmesi mümkün kılar.

Seçiciler, tarih ve saati seçmek için sık kullanılır; Apple sağlar [`UIDatePicker`](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/)
Bu amaç için sınıf.

Bir makalede uygulamak ve kullanmak nasıl `UIPickerView` ve `UIDatePicker` kontrol eder.

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>Bir seçici uygulama

Yeni bir örnekleme tarafından bir seçici uygulamak `UIPickerView`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width, 
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
    )
);
```

### <a name="pickers-and-storyboards"></a>Seçicileri ve görsel Taslaklar

Seçici'de oluşturmak için **iOS Tasarımcısı**, sürükleyin bir **Seçici görünümü** gelen **araç kutusu** tasarım yüzeyine bırakın.

![Seçici görünüm tasarım yüzeyine sürükleyin](picker-images/image1.png "Seçici görünüm tasarım yüzeyine sürükleyin.")

### <a name="working-with-a-picker-control"></a>Seçici denetimi ile çalışma

Bir seçici kullanan bir _modeli_ verilerle etkileşimde bulunmak için:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

[ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) Temel sınıfı, iki arabirim uygular [`IUIPickerDataSource`](https://developer.xamarin.com/api/type/UIKit.IUIPickerViewDataSource/)
ve [ `IUIPickerViewDelegate` ](https://developer.xamarin.com/api/type/UIKit.IUIPickerViewDelegate/), belirttiğiniz bir seçicinin veri çeşitli yöntemler bildirmek ve etkileşim nasıl işler?:

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

Bir seçici birden çok sütuna sahip olabilir veya _bileşenleri_. Bileşenleri, bir Seçici için daha kolay ve daha ayrıntılı veri seçimine izin veren birden çok bölüme, bölüm:

![İki bileşenlerle Seçici](picker-images/image3.png "iki bileşenlerle Seçici")

Seçici'de bileşenlerin sayısını belirtmek için kullanın [`GetComponentCount`](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetComponentCount/p/UIKit.UIPickerView/) 
yöntem.

### <a name="customizing-a-pickers-appearance"></a>Bir seçicinin görünümünü özelleştirme

Bir seçici görünümünü özelleştirmek için [`UIPickerView.UIPickerViewAppearance`](https://developer.xamarin.com/api/type/UIKit.UIPickerView+UIPickerViewAppearance/)
sınıf veya geçersiz kılma [ `GetView` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetView/p/UIKit.UIPickerView/System.nint/System.nint/UIKit.UIView/) ve [ `GetRowHeight` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetRowHeight/p/UIKit.UIPickerView/System.nint/) yöntemleri `UIPickerViewModel`.

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>Bir tarih seçici uygulama

Bir tarih seçici örneği tarafından uygulayan bir `UIDatePicker`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width,
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
     )
);
```

### <a name="date-pickers-and-storyboards"></a>Tarih seçiciler ve görsel Taslaklar

İçinde bir tarih seçici oluşturma **iOS Tasarımcısı**, sürükleyin bir **tarih seçici** gelen **araç kutusu** tasarım yüzeyine bırakın.

![Bir tarih seçici tasarım yüzeyine sürükleyin](picker-images/image2.png "tarih seçici tasarım yüzeyine sürükleyin.")

### <a name="date-picker-properties"></a>Tarih Seçici özellikleri

#### <a name="minimum-and-maximum-date"></a>Minimum ve maksimum tarih

[`MinimumDate`](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MinimumDate/) ve [ `MaximumDate` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MaximumDate/) tarih seçiciden kullanılabilir tarih aralığını sınırla. Örneğin, aşağıdaki kod, bir tarih seçici altmış mevcut ana sonlanan yıla kısıtlar:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

> [!TIP]
> Açık tür dönüştürme mümkündür bir `DateTime` için bir `NSDate`:
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>Dakika aralığı

[ `MinuteInterval` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MinuteInterval/) Özelliğini ayarlar, seçici görüntüler dakika aralık:

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>Mod

Dört destek tarih seçiciler [modları](https://developer.xamarin.com/api/type/UIKit.UIDatePickerMode/)aşağıda açıklandığı gibi:

##### <a name="uidatepickermodetime"></a>UIDatePickerMode.Time

`UIDatePickerMode.Time` bir saat ve dakika Seçici ve isteğe bağlı bir AM veya PM gösterimi zaman görüntüler:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode.Time](picker-images/image8.png "UIDatePickerMode.Time")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode.Date

`UIDatePickerMode.Date` bir ay, gün ve yıl Seçici ile tarihi görüntüler:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode.Date](picker-images/image7.png "UIDatePickerMode.Date")

Sistem yerel ayarı kullanır ve varsayılan tarih seçicinin yerel ayarlarda, seçici sırası bağlıdır. Yukarıdaki resimde Seçici düzeni gösterilir `en_US` yerel ayar, ancak aşağıdaki değişiklikleri sırasını gün | Ay | Yıl:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![Gün | Ay | Yıl](picker-images/image9.png "gün | Ay | Yıl")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode.DateAndTime

`UIDatePickerMode.DateAndTime` Tarih, saat, dakika ve (bağlı olarak 12 veya 24 saat kullanılıp kullanılmadığını) isteğe bağlı bir AM veya PM gösterimi saat kısaltılmış bir görünümünü görüntüler:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode.DateAndTime](picker-images/image6.png "UIDatePickerMode.DateAndTime")

Olduğu gibi [ `UIDatePickerMode.Date` ](#uidatepickermodedate), seçici sıralamasını ve 12 veya 24 saat kullanımını tarih seçici yerel ayarına göre değişir.

> [!TIP]
> Kullanım `Date` değerini bir tarih seçici modunda yakalama özelliğini `UIDatePickerMode.Time`, `UIDatePickerMode.Date`, veya `UIDatePickerMode.DateAndTime`. Bu değer olarak depolanan bir `NSDate`.

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode.CountDownTimer

`UIDatePickerMode.CountDownTimer` saat ve dakika değerleri görüntüler:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["UIDatePickerMode.CountDownTimer"](picker-images/image5.png "UIDatePickerMode.CountDownTimer")

`CountDownDuration` Özelliği bir tarih seçiciden değerini yakalayan `UIDatePickerMode.CountDownTimer` modu. Örneğin, geçerli tarihe geri sayım değeri eklemek için şunu yazın:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

Biçimlendirilecek bir `NSDate`, kullanan bir [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/).

Kullanılacak bir `NSDateFormatter`, arama, [ `ToString` ](https://developer.xamarin.com/api/member/Foundation.NSDateFormatter.ToString/p/Foundation.NSDate/) yöntemi. Örneğin:

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

[ `DateFormat` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.DateFormat/) Özelliği (dize) bir `NSDateFormatter` özelleştirilebilir tarih biçim belirtimlerinden için:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

[ `TimeStyle` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.TimeStyle/) Özelliği (bir [ `NSDateFormatterStyle` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatterStyle/)), bir `NSDateFormatter` saat biçimlendirme bağlı olarak belirlenmiş stillerini belirtir:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Çeşitli `NSDateFormatterStyle` değerlerini kez şu şekilde göster:

- `NSDateFormatterStyle.Full`: 19:46:00: 00 Doğu Yaz Saati
- `NSDateFormatterStyle.Long`: 7:47:00 PM EDT
- `NSDateFormatterStyle.Medium`: 7:47:00 PM
- `NSDateFormatterSytle.Short`: 7:47 PM

##### <a name="datestyle"></a>DateStyle

[ `DateStyle` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.DateStyle/) Özelliği (bir `NSDateFormatterStyle`), bir `NSDateFormatter` tarih biçimlendirme bağlı olarak belirlenmiş stillerini belirtir:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Çeşitli `NSDateFormatterStyle` değerlerini tarihleri şu şekilde göster:

- `NSDateFormatterStyle.Full`: Çarşamba, 2 Ağustos 2017 19:48:00
- `NSDateFormatterStyle.Long`: 2 Ağustos 2017, 7:49 PM
- `NSDateFormatterStyle.Medium`: 2 Ağu 2017'den itibaren 7:49 PM
- `NSDateFormatterStyle.Short`: 2/8/17, 7:50 PM

> [!NOTE]
> `DateFormat` ve `DateStyle` / `TimeStyle` tarih ve saat biçimlendirme belirtmenin farklı yollar sağlar. En son tarih biçimlendirici çıktı kümesi özellikleri belirler.

## <a name="related-links"></a>İlgili bağlantılar

- [PickerControl (örnek)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
