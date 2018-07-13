---
title: Xamarin.Forms DatePicker
description: DatePicker kullanıcının bir tarih seçmesine izin veren bir Xamarin.Forms görünümdür. Bu makalede, bir Xamarin.Forms uygulamasındaki bir DatePicker kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/04/2018
ms.openlocfilehash: 553957bfa06c7b7a9c5261e426ebee4190de5ebb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994932"
---
# <a name="xamarinforms-datepicker"></a>Xamarin.Forms DatePicker

_Kullanıcının bir tarih seçmesine izin veren bir Xamarin.Forms görünümü_

Xamarin.Forms [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) platformun tarih seçici denetimi çağırır ve kullanıcının bir tarih seçmesine olanak sağlar. `DatePicker` sekiz özelliklerini tanımlar:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) tür [ `DateTime` ](xref:System.DateTime), 1900 yılın ilk günü için varsayılan olarak.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) tür `DateTime`, hangi varsayılanlara 2100 yılın son günü.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) tür `DateTime`, değeri varsayılan olarak seçilen tarihten [ `DateTime.Today` ](xref:System.DateTime.Today).
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) tür `string`, [standart](/dotnet/standard/base-types/standard-date-and-time-format-strings/) veya [özel](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET biçimlendirme dizesi, "D" varsayılan olarak, uzun tarih deseni.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) tür [ `Color` ](xref:Xamarin.Forms.Color), varsayılan olarak seçili tarihini görüntülemek için kullanılan rengi [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) tür [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), bunun varsayılan [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) tür `string`, bunun varsayılan `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) tür `double`, -1.0 için varsayılan olarak.

`DatePicker` Ateşlenir bir [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) kullanıcının bir tarih seçtiğinde olay.

> [!WARNING]
> Ayarlarken `MinimumDate` ve `MaximumDate`, emin `MinimumDate` her zaman eşit veya küçük `MaximumDate`. Aksi takdirde, `DatePicker` bir özel durum oluşturacak.

Dahili olarak `DatePicker` sağlar `Date` arasında `MinimumDate` ve `MaximumDate`(dahil). Varsa `MinimumDate` veya `MaximumDate` ayarlanır böylece `Date` bunlar arasında değil `DatePicker` değerini ayarlar `Date`.

Tüm sekiz özellikleri tarafından yedeklenen [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) nesneleri, bunlar biçimlendirilebilir ve veri bağlama hedefleri özellikler olabilir. `Date` Özelliğine sahip bir varsayılan bağlama modu [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), kullanan bir uygulama bir veri bağlama hedefi olabileceği anlamına gelir [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Mimari.

## <a name="initializing-the-datetime-properties"></a>DateTime özellikleri başlatma

Kodda, başlatabilirsiniz `MinimumDate`, `MaximumDate`, ve `Date` türü değerlerinin özelliklerine `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Olduğunda bir `DateTime` değeri, XAML ayrıştırıcı kullanan XAML içinde belirtilirse `DateTime.Parse` yöntemi ile bir `CultureInfo.InvariantCulture` dizeye dönüştürmek için bağımsız değişken bir `DateTime` değeri. Kesin bir biçimde tarihleri belirtilmelidir: dört rakamlı yıl eğik çizgiyle ayrılmış iki haneli ay ve iki basamaklı gün:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Varsa `BindingContext` özelliği `DatePicker` türünün özelliklerini içeren bir ViewModel örneğine ayarlanır `DateTime` adlı `MinDate`, `MaxDate`, ve `SelectedDate` (örneğin), oluşturabileceğiniz `DatePicker` şöyle :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

Bu örnekte, tüm üç özellik ViewModel karşılık gelen özelliklerinde başlatılır. Çünkü `Date` özelliğine sahip bir bağlama modu `TwoWay`, kullanıcı seçer ViewModel içinde otomatik olarak yansıtılan herhangi bir yeni tarihi.

Varsa `DatePicker` üzerinde bir bağlama içermiyor, `Date` özelliği, uygulama eklemek için bir işleyici `DateSelected` olmasını olay haberdar kullanıcı yeni bir tarih seçtiğinde.

Yazı tipi özelliklerini ayarlama hakkında daha fazla bilgi için bkz. [yazı tipleri](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker ve düzeni

Sınırlandırılmamış yatay düzeni seçeneği gibi kullanmak da mümkündür `Center`, `Start`, veya `End` ile `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Ancak, bu önerilmez. Ayarına bağlı olarak `Format` seçili özelliği, tarihler, farklı ekran genişliği gerektirebilir. Örneğin, "D" biçim dizesi neden `DateTime` tarihleri uzun biçimi ve "Çarşamba, Eylül 12 2018" görüntülemek için "Friday, Mayıs 4 2018" daha büyük bir görüntü genişliğini gerektirir. Platforma bağlı olarak, bu fark neden olabilecek `DateTime` düzeni veya kesilecek görüntü genişliğini değiştirmek için görünümü.

> [!TIP]
> Varsayılan kullanmak en iyisidir `HorizontalOptions` ayarıyla `Fill` ile `DatePicker`ve genişliği kullanmayı `Auto` koyarak olduğunda `DatePicker` içinde bir `Grid` hücre.

## <a name="datepicker-in-an-application"></a>Bir uygulamada tarih seçici

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) örnek içeren iki `DatePicker` , sayfa görünümleri. Bu iki tarihleri seçmesini kullanılabilir ve programın bu tarih arasındaki gün sayısını hesaplar. Program ayarlarını değiştirmez `MinimumDate` ve `MaximumDate` özellikleri, iki tarih 1900 ile 2100 arasında olması gerekir.

XAML dosyası aşağıda verilmiştir:

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

Her `DatePicker` atanmış bir `Format` "D" özelliği için bir uzun tarih biçimi. Ayrıca dikkat `endDatePicker` nenesindeki hedefleyen bir bağlama kendi `MinimumDate` özelliği. Bağlama kaynağı seçili olduğu `Date` özelliği `startDatePicker` nesne. Bu, bitiş tarihi daha sonra her zaman başlangıç tarihine eşit veya sağlar. İki ek olarak `DatePicker` nesneleri bir `Switch` "her iki gün toplam ekleme" olarak etiketlenir.

İki `DatePicker` görünümleri sahip bağlı işleyicileri `DateSelected` olay ve `Switch` için bir işleyici eklenmiş kendi `Toggled` olay. Bu olay işleyicileri, arka plan kod dosyasında olan ve yeni bir hesaplama iki tarih arasındaki gün tetikleyin:

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

Örnek ilk kez çalıştırdığınızda, her ikisi de `DatePicker` görünümleri, bugünün tarihine başlatılır. Aşağıdaki ekran görüntüsünde, iOS, Android ve evrensel Windows platformu üzerinde çalışan bir program gösterilmektedir:

[![Tarih arasındaki gün başlangıç](datepicker-images/DaysBetweenDatesStart.png "tarih arasındaki gün başlangıç")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "tarih arasındaki gün Başlat")

Aşağıdakilerden birini dokunarak `DatePicker` görüntüler platform tarih seçici çağırır. Üç platformda bu tarih seçicinin çok farklı yollarla uygulayın, ancak her platform kullanıcılara tanıdık bir yaklaşımdır:

[![Tarih arasındaki gün seçin](datepicker-images/DaysBetweenDatesSelect.png "tarih arasındaki gün seçin")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "tarih arasındaki gün seçin")

İki tarih seçtikten sonra uygulama bu tarih arasındaki gün sayısını görüntüler:

[![Tarihleri sonuç arasındaki gün](datepicker-images/DaysBetweenDatesResult.png "tarihleri sonuç arasındaki gün")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "tarihleri sonuç arasındaki gün")

## <a name="related-links"></a>İlgili bağlantılar

- [DaysBetweenDates örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker API](xref:Xamarin.Forms.DatePicker)
