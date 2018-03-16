---
title: DatePicker kullanma
description: "Kullanıcının bir tarih seçmesine olanak veren bir Xamarin.Forms görünümü"
ms.topic: article
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/12/2018
ms.openlocfilehash: d47499c1e309fbc67c85b55cacbbba3942188f54
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="using-datepicker"></a>DatePicker kullanma

_Kullanıcının bir tarih seçmesine olanak veren bir Xamarin.Forms görünümü_

Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) platformun tarih seçici denetim çağırır ve bir tarih seçin olanak tanır. `DatePicker` beş özelliklerini tanımlar:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) tür [ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/), 1900 yılın ilk günü için varsayılan olarak.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) tür `DateTime`, hangi varsayılanlara 2100 yılın son günü.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) tür `DateTime`, değeri varsayılan olarak seçilen tarihten [ `DateTime.Today` ](https://developer.xamarin.com/api/property/System.DateTime.Today/).
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) tür `string`, [standart](/dotnet/standard/base-types/standard-date-and-time-format-strings/) veya [özel](/dotnet/standard/base-types/custom-date-and-time-format-strings/) "D" varsayılan olarak, dize biçimlendirme .NET uzun tarih düzeni.
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) tür [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/), varsayılan olarak seçilen tarihini görüntülemek için kullanılan renk [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).

`DatePicker` Ateşlenir bir [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) kullanıcı bir tarih seçtiğinde olay.

> [!WARNING]
> Ayarlarken `MinimumDate` ve `MaximumDate`, olduğundan emin olun `MinimumDate` her zaman küçük veya buna eşit olduğundan `MaximumDate`. Aksi takdirde, `DatePicker` bir özel durum oluşturacak.

Dahili olarak, `DatePicker` sağlar `Date` arasında `MinimumDate` ve `MaximumDate`(dahil). Varsa `MinimumDate` veya `MaximumDate` ayarlanmış şekilde `Date` bunlar arasında değil `DatePicker` değerini ayarlar `Date`.

Tüm beş özellikleri tarafından yedeklenen [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) nesneleri bunlar biçimlendirilebilir ve özellikler veri bağlamaların hedefleri olabilir anlamına gelir. `Date` Özelliğine sahip bir varsayılan bağlama modu [ `BindingMode.TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/), kullanan bir uygulama içinde veri bağlama hedefi olabileceği anlamına gelir [Model View ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Mimari.

## <a name="initializing-the-datetime-properties"></a>DateTime özellikleri başlatma

Kodda, başlatabilir `MinimumDate`, `MaximumDate`, ve `Date` türü değerleri özelliklerine `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Zaman bir `DateTime` değerinde XAML ayrıştırıcısı kullanan XAML içinde belirtilen `DateTime.Parse` yöntemi ile bir `CultureInfo.InvariantCulture` dizeye dönüştürmek için bağımsız değişken bir `DateTime` değeri. Tarihleri kesin bir biçimde belirtilmesi gerekir: iki basamaklı ay, iki basamaklı gün ve eğik çizgiyle ayrılmış dört rakamlı yıl:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Varsa `BindingContext` özelliği `DatePicker` türünün özelliklerini içeren bir ViewModel örneğine ayarlanmış `DateTime` adlı `MinDate`, `MaxDate`, ve `SelectedDate` (örneğin), örneği `DatePicker` şöyle :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

Bu örnekte, tüm üç özellik ViewModel karşılık gelen özelliklerinde başlatılır. Çünkü `Date` özelliğine sahip bir bağlama modu `TwoWay`, kullanıcı seçer ViewModel otomatik olarak yansıtılır herhangi bir yeni tarihi.

Varsa `DatePicker` üzerinde bir bağlama içermiyor kendi `Date` özelliği, bir uygulama eklemek için bir işleyici `DateSelected` olması için olay bilgi sahibi kullanıcı yeni bir tarih seçtiğinde.

## <a name="datepicker-and-layout"></a>DatePicker ve düzeni

Kısıtlanmamış yatay düzen seçeneği gibi kullanmak da mümkündür `Center`, `Start`, veya `End` ile `DatePicker`:

```xaml
<DatePicker ··· 
            HorizontalOptions="Center" 
            ··· />
```

Ancak, bu önerilmez. Ayarını bağlı olarak `Format` özelliği, Seçili tarihleri farklı görüntü genişliği gerekebilir. Örneğin, "D" biçim dizesi neden `DateTime` uzun bir biçimde ve "Çarşamba, 12 Eylül 2018" tarihleri görüntülemek için "Cuma, May 4, 2018" daha büyük görüntü genişliğini gerektirir. Platforma bağlı olarak bu fark neden olabilecek `DateTime` genişliğini düzeninde ya da kesilecek görüntülenmek değiştirmek için görünümü.

> [!TIP]
> Varsayılan kullanmak en iyisidir `HorizontalOptions` ayarıyla `Fill` ile `DatePicker`ve genişliği kullanmamayı `Auto` koyma zaman `DatePicker` içinde bir `Grid` hücre.

## <a name="datepicker-in-an-application"></a>Bir uygulamada DatePicker

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) örnek içeren iki `DatePicker` , sayfa görünümleri. Bu iki tarih seçmek için kullanılabilir ve bu tarihler arasındaki gün sayısını program hesaplar. Program ayarlarını değiştirmez `MinimumDate` ve `MaximumDate` özellikleri, iki tarih 1900 ile 2100 arasında olması gerekir.

XAML dosyası şöyledir:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Her `DatePicker` atanmış bir `Format` "D" özelliği için uzun tarih biçimi. Ayrıca dikkat `endDatePicker` nesne sahip hedefleyen bir bağlama kendi `MinimumDate` özelliği. Bağlama kaynağıdır seçili `Date` özelliği `startDatePicker` nesnesi. Bu, son tarihi daha sonra her zaman başlangıç tarihine eşit veya sağlar. İki ek olarak `DatePicker` nesneleri, bir `Switch` "Include toplamda iki gün" olarak etiketlenir. 

İki `DatePicker` görünümleri sahip ilişik işleyiciler `DateSelected` olayı ve `Switch` için bir işleyici eklenmiş kendi `Toggled` olay. Bu olay işleyicileri ve iki tarih arasındaki gün yeni bir hesaplaması tetiklemek arka plan kod dosyasına verilmiştir:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

Örnek ilk çalıştırıldığında, her ikisi de `DatePicker` görünümleri bugüne başlatılır. Aşağıdaki ekran görüntüsünde, iOS, Android ve evrensel Windows platformu üzerinde çalışan program gösterir:

[![Tarih arasındaki gün başlangıç](datepicker-images/DaysBetweenDatesStart.png "tarih arasındaki gün başlangıç")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "tarih arasındaki gün Başlat")

Aşağıdakilerden birini dokunarak `DatePicker` görüntüler platform tarih seçici çağırır. Üç platformları bu tarih seçici çok farklı şekillerde uygulamak, ancak her bir yaklaşım, platform kullanıcılara alışkın olduğu:

[![Tarih arasındaki gün seçin](datepicker-images/DaysBetweenDatesSelect.png "tarih arasındaki gün seçin")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "tarih arasındaki gün seçin")

İki tarih seçtikten sonra uygulama bu tarihlerin arasındaki gün sayısını görüntüler:

[![Tarihleri sonuç arasındaki gün](datepicker-images/DaysBetweenDatesResult.png "tarihleri sonuç arasındaki gün")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "tarihleri sonuç arasındaki gün")

## <a name="related-links"></a>İlgili bağlantılar

- [DaysBetweenDates örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
