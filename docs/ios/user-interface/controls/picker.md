---
title: Xamarin.iOS Seçici denetiminde
description: Bu belge, tasarım ve bir Xamarin.iOS uygulaması seçici denetimlerinde çalışmak açıklar. Nasıl kodu ve Designer iOS bir seçici uygulanacağını anlatır.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: 7f46d354af86027d1e2656171c6595562d3555a6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789918"
---
# <a name="picker-control-in-xamarinios"></a>Xamarin.iOS Seçici denetiminde

Seçici denetim kaydırılabilir vurgulanmış seçili değerine sahip bir değer listesi içeren 'tekerleği benzeri' denetimi görüntüler. Kullanıcıların istedikleri seçeneğini tekerleği döndürün.

Belirli bir kullanıcı servis talebi için seçiciler, tarih ve / veya saat ayarlayın. Bu Apple için sağlamak için özel bir alt UIDatePicker adlı UIPickerView sınıfının oluşturdu.

Uygulama ve kullanarak makaleyi kapsar [Seçici](#picker) ve [tarih seçici](#datepicker) kontrol eder.

<a name="picker" />

## <a name="picker"></a>Seçici

### <a name="implementing-a-picker"></a>Bir seçici uygulama

Bir seçici yeni bir örneği tarafından uygulanan [`UIPickerView`](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>Seçiciler ve film şeritleri

UI oluşturmak için iOS Tasarımcısı kullanıyorsanız, seçici Araç Kutusu'ndan düzeninizde eklenebilir:

![](picker-images/image1.png)


### <a name="working-with-picker"></a>Seçici ile çalışma

Bir kez bir seçici kod olup olmadığını oluşturmuş veya film şeritleri atamanız gerekir bir _modeli_ ona geçirmek ve verilerle; etkileşimli

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

Aşağıdaki kod bir model örneği gösterilmektedir:

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

İlk seçmek bir kullanıcı için farklı seçenekleri sağlamak için bazı veriler geçmesi gerekir. Olası çalıştığınızda bu listeyi olabildiğince kısa tutmak ya da gerekli birden fazla 'Çevir' kullanmayı denerseniz (adlı *bileşenleri*):

![İki bileşenlerle Seçici](picker-images/image3.png)

Bileşenlerin sayısını ayarlamak için geçersiz kılın `GetComponentCount` yöntemi: 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

Dönüş değeri, seçici olacaktır çevirir sayısını belirtir.

### <a name="customizing-appearance"></a>Görünümünü özelleştirme
 
Görünümünü `UIPickerView` kullanarak özelleştirilebilir `UIPickerView.UIPickerViewAppearance` sınıf veya göre geçersiz kılma `UIPickerViewModel.GetView` ve `UIPickerViewModel.GetRowHeight` yöntemleri `UIPickerViewModel`.


<a name="datepicker" />

## <a name="date-picker"></a>Tarih Seçici

### <a name="implementing-a-date-picker"></a>Tarih Seçici uygulama

Tarih Seçici yeni bir örneği tarafından uygulanan [ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>Tarih seçiciler ve film şeritleri

Kullanıcı Arabirimi oluşturmak için iOS Tasarımcısı kullanıyorsanız **tarih seçici** düzeninizde Araç Kutusu'ndan eklenebilir. Aşağıdaki özellikler özellikler panelinden ayarlanabilir:

![](picker-images/image2.png)

* **Mod** – tarih ve saat modu. Bu tarih, saat, tarih ve saat veya geri sayım Zamanlayıcısı olabilir. 
* **Yerel ayar** – tarih seçici yerel. Seçin **varsayılan** sistem varsayılan olarak ayarlamak veya belirli tüm yerel ayarlar için ayarlanamıyor.
* **Aralığı** – saat seçenekleri görüntüleneceği artışı gösterir.
* **Tarih, Minimum tarih, en fazla tarih** – seçilebilir tarihlerini Seçici görüntüler başlangıç tarihi ile kısıtlamaları ayarlar.

### <a name="configuring-the-datepicker"></a>DatePicker yapılandırma

Bir kullanıcı kullanarak seçebilirsiniz tarih aralığını sınırlayabilirsiniz `MinimumDate` ve `MaximumDate` özellikleri. Aşağıdaki kod parçacığını 60 yıl önce ve bugün aralığında ayarlama örneği gösterilir:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

Alternatif olarak, minimum ve maksimum tarih aralığını ayarlamak için .NET denetimleri kullanabilirsiniz. Örneğin:

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

Ayrıca ayarlayabilirsiniz `MinuteInterval` aktarılma Seçici görüntüler dakika aralığını ayarlamak için özellik. Aşağıdaki kod parçacığını 10 aralıklarla ayarlanacak dakika Seçici ayarlamak için kullanılabilir.

```csharp
datePickerView.MinuteInterval = 10;
```

Tarih Seçici kullanarak ayarlanabilir dört modu bulunmaktadır. [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/) özelliği. Aşağıdaki liste, her biri ve uygulamak nasıl örneği gösterilmektedir:

#### <a name="time"></a>Zaman

Zaman modu saati bir saat ve dakika Seçici ve isteğe bağlı bir AM veya PM ataması görüntüler. İle ayarlamak `UIDatePickerMode.Time` özelliği. Örneğin:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

Aşağıdaki resimde bu DatePicker modu örneği gösterilmektedir:

![](picker-images/image8.png)



#### <a name="date"></a>Tarih

Tarih modunu ay, gün ve yıl Seçici tarihi görüntüler. İle ayarlamak `UIDatePickerMode.Date` özelliği. Örneğin:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

Aşağıdaki resimde bu DatePicker örneği gösterilmektedir:

![](picker-images/image7.png)

Yerel üzerinde Seçici sırası bağlıdır `UIDatePicker`. Varsayılan olarak bu sistem varsayılan olarak ayarlanır. Yukarıdaki resimde seçiciler düzenini gösterir `en_US` günü olarak değiştirmek üzere ancak yerel | Ay | Yıl düzeni, yerel ayar için aşağıdaki kodu kullanabilirsiniz:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>Tarih ve Saat

12 veya 24 saat kullanılırsa, tarih ve saat modu tarih, saat, dakika ve isteğe bağlı bir AM veya PM ataması dependings saat shortend görünümünü görüntüler. İle ayarlamak `UIDatePickerMode.DateAndTime` özelliği. Örneğin:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

Aşağıdaki resimde bu DatePicker örneği gösterilmektedir:

![](picker-images/image6.png)

İle [tarih](#Date), yerel üzerinde Seçici sırasını ve 12 veya 24 saat kullanımını bağlıdır `UIDatePicker`.

#### <a name="countdown-timer"></a>Sayım

Geri sayım Zamanlayıcısı modu, saat ve dakika değerleri görüntüler. İle ayarlamak `UIDatePickerMode.CountDownTimer` özelliği. Örneğin:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

Aşağıdaki resimde bu DatePicker örneği gösterilmektedir:

![](picker-images/image5.png)

Kullanabileceğiniz `CountDownDuration` değeri dispayed geri sayım tarih seçici olarak yakalamak için özellik. Örneğin, geçerli tarihe kadar geri sayım değeri eklemek için aşağıdaki kodu kullanabilirsiniz:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>Biçimlendirme 

Saat, tarih ve DateAndTime modları değerleri kullanılarak yakalanabilir `Date` UIDatePicker özelliği (örneğin: `datePickerView.Date`), NSDate türündeki. Bu tarih okunabilir bir şey daha fazla insan biçimlendirmek için [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/). Aşağıdaki örnekler kullanılabilir özelliklerin bazıları bu sınıfa kullanmayı gösterir.

`DateFormat` Tarihi nasıl görüntüleneceğini temsil etmek için bir dize olarak ayarlayın:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

`TimeStyle` Özellik kümeleri bir `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Alanlar için `NSDateFormatterStyle` aşağıdaki gibi görüntüleyin:

![](picker-images/timestyle.png)

`DateStyle` Özellik kümeleri bir `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Alanlar için `NSDateFormatterStyle` aşağıdaki gibi görüntüleyin:

![](picker-images/datestyle.png)

Ardından, aşağıdaki kodu kullanarak bir dizeye biçimlendirilmiş NSDate çıkarabilirsiniz:

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>İlgili bağlantılar

- [PickerControl (örnek)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
