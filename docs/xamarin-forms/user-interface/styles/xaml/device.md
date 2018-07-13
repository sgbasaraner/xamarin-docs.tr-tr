---
title: Xamarin.Forms içinde cihaz stilleri
description: Xamarin.Forms Device.Styles sınıfında cihaz stilleri olarak bilinen altı dinamik stiller içerir. Bu makalede, bir Xamarin.Forms uygulaması cihaz stilleri kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: bba42c966c6a606790655751db8b294d9ca7b6f9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994383"
---
# <a name="device-styles-in-xamarinforms"></a>Xamarin.Forms içinde cihaz stilleri

_Xamarin.Forms Device.Styles sınıfında cihaz stilleri olarak bilinen altı dinamik stiller içerir._

*Cihaz* stiller şunlardır:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

Tüm altı stilleri yalnızca uygulanabilir [ `Label` ](xref:Xamarin.Forms.Label) örnekleri. Örneğin, bir `Label` paragrafı gövdesi görüntülüyor ayarlayabilir, [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özelliğini [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle).

Aşağıdaki kod örneği kullanmayı gösterir *cihaz* XAML sayfası stilleri:

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

Cihaz stilleri kullanarak bağlı `DynamicResource` işaretleme uzantısı. Stilleri dinamik doğasını değiştirerek iOS görülebilir **erişilebilirlik** metin boyutu için ayarlar. Görünümünü *cihaz* aşağıdaki ekran görüntülerinde gösterildiği stilleri her platformda farklı:

![](device-images/device-styles.png "Her platformda cihaz stilleri")

*Cihaz* stilleri de türetilen ayarlayarak [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) özelliğini cihaz stili için anahtar adı. Yukarıdaki kod örneğinde `myBodyStyle` devraldığı [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle) ve vurgulanmış metin rengini belirler. Dinamik stil devralımı hakkında daha fazla bilgi için bkz: [dinamik stil devralımı](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance).

Aşağıdaki kod örneği, C# ' de eşdeğer sayfaya gösterir:

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

[ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Her özellik [ `Label` ](xref:Xamarin.Forms.Label) örneği uygun özelliğinden kümesine [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) sınıfı.

## <a name="accessibility"></a>Erişilebilirlik

*Cihaz* stilleri saygı Erişilebilirlik tercihlerini için yazı tipi boyutlarını Erişilebilirlik tercihlerini her platformda değiştirilen şekilde değiştirir. Bu nedenle, erişilebilir metin desteklemek için emin *cihaz* stilleri, uygulamanızda herhangi bir metin stilleri için temel olarak kullanılır.

Aşağıdaki ekran görüntüleri küçük erişilebilir yazı tipi boyutu ile her platformda cihaz stilleri gösterir:

[![](device-images/minimum-size.png "Her platformda erişilebilir küçük cihaz stilleri")](device-images/minimum-size-large.png#lightbox "her platformda erişilebilir küçük cihaz stilleri")

Aşağıdaki ekran görüntüleri, en büyük erişilebilir yazı tipi boyutu ile her platformda cihaz stilleri gösterir:

![](device-images/maximum-size.png "Her platformda erişilebilir büyük cihaz stilleri")

## <a name="summary"></a>Özet

Xamarin.Forms içeren altı *dinamik* olarak bilinen stilleri *cihaz* stiller, buna [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) sınıfı. Tüm altı stilleri yalnızca uygulanabilir [ `Label` ](xref:Xamarin.Forms.Label) örnekleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Metin stilleri](~/xamarin-forms/user-interface/text/styles.md)
- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dinamik stiller (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Ayarlayıcı](xref:Xamarin.Forms.Setter)
