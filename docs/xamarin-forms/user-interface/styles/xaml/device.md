---
title: Cihaz stilleri
description: Xamarin.Forms Device.Styles sınıfında aygıt stilleri olarak bilinen altı dinamik stiller içerir.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: daf6569038cb65ab7fdb2050fe776a330a264279
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="device-styles"></a>Cihaz stilleri

_Xamarin.Forms Device.Styles sınıfında aygıt stilleri olarak bilinen altı dinamik stiller içerir._

*Aygıt* stillerdir:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)

Tüm altı stilleri yalnızca uygulanabilir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri. Örneğin, bir `Label` bir paragraf gövdesi görüntüleme ayarlayabilir, [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliğine [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/).

Aşağıdaki kod örneğinde kullanımı gösterilir *aygıt* XAML sayfası stillerde:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Cihaz stilleri kullanarak bağlı olan `DynamicResource` biçimlendirme uzantısı. Stilleri dinamik yapısını değiştirerek iOS görülebilir **erişilebilirlik** metin boyutu ayarları. Görünümünü *aygıt* aşağıdaki ekran görüntülerinde gösterildiği gibi stilleri her platformda farklı:

![](device-images/device-styles.png "Her platformda aygıt stilleri")

*Aygıt* stilleri ayrıca türetilen ayarlayarak [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) özelliğini anahtar adına aygıt stili. Yukarıdaki kod örneğinde `myBodyStyle` devraldığı [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/) ve aksanlı metin rengini belirler. Dinamik stili devralma hakkında daha fazla bilgi için bkz: [dinamik stili devralma](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance).

Aşağıdaki kod örneği, C# eşdeğer sayfaya gösterir:

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Her özellik [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örneği uygun özelliğinden ayarlanmış [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) sınıfı.

## <a name="accessibility"></a>Erişilebilirlik

*Aygıt* stilleri saygı Erişilebilirlik tercihleri Erişilebilirlik tercihleri her platformda değiştirilen olarak yazı tipi boyutlarını değiştirecek şekilde. Bu nedenle, erişilebilir metin desteklemek için emin *aygıt* stilleri, uygulamanızda herhangi metin stili için temel olarak kullanılır.

Aşağıdaki ekran görüntüleri küçük erişilebilir yazı tipi boyutu olan her bir platformda aygıt stilleri gösterir:

[![](device-images/minimum-size.png "Her platformda erişilebilir küçük aygıt stilleri")](device-images/minimum-size-large.png#lightbox "her platformda erişilebilir küçük aygıt stilleri")

Aşağıdaki ekran görüntüleri büyük erişilebilir yazı tipi boyutu olan her bir platformda aygıt stilleri gösterir:

![](device-images/maximum-size.png "Her platformda erişilebilir büyük cihaz stilleri")

## <a name="summary"></a>Özet

Xamarin.Forms içeren altı *dinamik* olarak bilinen stilleri *aygıt* stiller, buna [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) sınıfı. Tüm altı stilleri yalnızca uygulanabilir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Metin stilleri](~/xamarin-forms/user-interface/text/styles.md)
- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dinamik stilleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Ayarlama](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
