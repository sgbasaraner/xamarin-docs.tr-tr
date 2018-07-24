---
title: Xamarin.Forms etiketi
description: Bu makalede, uygulamaların tek ve çok satırlı metni görüntülemek için Xamarin.Forms etiket sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: b5069381126db0859508480df5596ed5ec85686f
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203042"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms etiketi

_Xamarin.Forms içinde metin görüntüleme_

[ `Label` ](xref:Xamarin.Forms.Label) Görünümü, hem tek hem de çok satırlı bir metin görüntülemek için kullanılır. Etiket (aileleri, boyutlar ve Seçenekler) özel yazı tipleri ve renkli metin olabilir.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Kesme ve kaydırma

Etiketler, tek satırda tarafından kullanıma sunulan birkaç yoldan biriyle sığamıyorsa metin işlemek için ayarlanabilir `LineBreakMode` özelliği. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) Aşağıdaki değerleri olan bir sabit listesi verilmiştir:

- **HeadTruncation** &ndash; sonunu gösteren metnin başındaki keser.
- **CharacterWrap** &ndash; yeni bir satır metni bir karakter sınırında sarmalar.
- **MiddleTruncation** &ndash; başlangıcını ve bitişini üç nokta orta değer ile metin görüntüler.
- **NoWrap** &ndash; yalnızca görüntüleme metni kaydırılacak kadar metin kutusu olarak uygun bir satır.
- **TailTruncation** &ndash; son kesiliyor metnin başlangıcını gösterir.
- **Sözcük kaydırmayı** &ndash; metin sözcük sınırında sarmalar.

## <a name="fonts"></a>Yazı Tipleri

Yazı tipleri belirtme hakkında daha fazla bilgi için bir `Label`, bkz: [yazı tipleri](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="colors"></a>Renkleri

`Label`s bağlanabilir aracılığıyla bir özel metin rengi ayarlanabilir [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) özelliği.

Özel renkler her platformda kullanılabilir olmasını sağlamak gereklidir. Her platform için metin ve arkaplan renklerini varsayılan değerleri farklı olduğundan, her çalışan varsayılan seçmek dikkatli olmanız gerekir.

Aşağıdaki XAML bir etiketin metin rengini ayarlamak için kullanın:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
</ContentPage>
```

Eşdeğer C# kodu verilmiştir:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Aşağıdaki ekran görüntüleri ayarı sonucu göster `TextColor` özelliği:

![](label-images/textcolor.png "Etiket TextColor örneği")

Renkler hakkında daha fazla bilgi için bkz. [renkleri](~/xamarin-forms/user-interface/colors.md).

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Biçimlendirilmiş metin

Etiketleri kullanıma bir [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) birden çok yazı tipi ile metin sunumu sağlar ve aynı görünümde renkleri özelliği.

`FormattedText` Özelliği türüdür [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), bir veya daha fazla oluşur [ `Span` ](xref:Xamarin.Forms.Span) aracılığıyla ayarlanan örnekleri [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) özelliği . Her `Span` aşağıdaki özelliklere sahiptir:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – Aralık arka plan rengi.
- [`Font`](xref:Xamarin.Forms.Span.Font) – metnin yazı tipini de aralık.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – aralık içindeki metni için yazı tipi öznitelikleri.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – metnin yazı tipini de aralık ait olduğu yazı tipi ailesi.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – metnin yazı tipini de aralık boyutu.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – Aralık metin rengi. Bu özellik artık kullanılmıyor ve almıştır `TextColor` özelliği.
- [`Style`](xref:Xamarin.Forms.Span.Style) – yayılma uygulamak için stili.
- [`Text`](xref:Xamarin.Forms.Span.Text) – aralığın metin.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – Aralık metin rengi.

Aşağıdaki XAML örnek gösterir bir `FormattedText` oluşan üç özellik `Span` örnekleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Eşdeğer C# kodu verilmiştir:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> [ `Text` ](xref:Xamarin.Forms.Span.Text) Özelliği bir `Span` veri bağlama aracılığıyla ayarlanabilir. Daha fazla bilgi için [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Aşağıdaki ekran görüntüleri ayarı sonucu göster `FormattedString` üç özellik `Span` örnekleri:

![](label-images/formattedtext.png "Etiket FormattedText örneği")

## <a name="styling-a-label"></a>Bir etiket stil oluşturma

Önceki bölümlerde ele ayarı [ `Label` ](xref:Xamarin.Forms.Label) örneği başına temelinde özellikleri. Ancak, özellik kümeleri, tutarlı bir şekilde bir veya daha çok görünümleri için uygulanan bir stil içinde gruplandırılabilir. Bu kodun okunabilirliğini artırmak ve tasarım değişiklikleri uygulamak daha kolay hale getirebilirsiniz. Daha fazla bilgi için [stilleri](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 3 Xamarin.Forms ile mobil uygulamalar oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Etiket API](xref:Xamarin.Forms.Label)
