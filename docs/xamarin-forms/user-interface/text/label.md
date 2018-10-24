---
title: Xamarin.Forms etiketi
description: Bu makalede, uygulamaların tek ve çok satırlı metni görüntülemek için Xamarin.Forms etiket sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/04/2018
ms.openlocfilehash: c98dcc30ac89e3df0338df02e14a32575c0dc847
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39203042"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms etiketi

_Xamarin.Forms içinde metin görüntüleme_

[ `Label` ](xref:Xamarin.Forms.Label) Görünümü, hem tek hem de çok satırlı bir metin görüntülemek için kullanılır. Etiketler, metin dekorasyonlarının renkli metinler vardır ve özel yazı tipleri (aileleri, boyutlar ve Seçenekler) kullanın.

## <a name="text-decorations"></a>Metin süslemeleri

Alt çizgi ve üstü çizili metin düzenlemelerinin uygulanabilir [ `Label` ](xref:Xamarin.Forms.Label) ayarlayarak örnekleri `Label.TextDecoration` bir veya daha fazla özelliğini `TextDecoration` numaralandırma üyeleri:

- `None`
- `Underline`
- `Strikethrough`

Aşağıdaki XAML örnek ayar gösterir `Label.TextDecoration` özelliği:

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

Eşdeğer C# kodu verilmiştir:

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

Aşağıdaki ekran görüntüleri Göster `TextDecoration` uygulanan numaralandırma üyelerini [ `Label` ](xref:Xamarin.Forms.Label) örnekleri:

![](label-images/label-textdecorations.png "Metin süslemeleri etiketleri")

> [!NOTE]
> Metin süslemeleri da uygulanabilir [ `Span` ](xref:Xamarin.Forms.Span) örnekleri. Hakkında daha fazla bilgi için `Span` sınıfı [biçimlendirilmiş metin](#Formatted_Text).

## <a name="colors"></a>Renkleri

Etiketler, bir özel metin rengi bağlanabilir aracılığıyla ayarlanabilir [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) özelliği.

Özel renkler her platformda kullanılabilir olmasını sağlamak gereklidir. Her platform için metin ve arkaplan renklerini varsayılan değerleri farklı olduğundan, her çalışan varsayılan seçmek dikkatli olmanız gerekir.

Aşağıdaki XAML örnek metin rengini ayarlar bir `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a green label." />
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
        var label = new Label { Text="This is a green label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Aşağıdaki ekran görüntüleri ayarı sonucu göster `TextColor` özelliği:

![](label-images/textcolor.png "Etiket TextColor örneği")

Renkler hakkında daha fazla bilgi için bkz. [renkleri](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Yazı Tipleri

Yazı tipleri belirtme hakkında daha fazla bilgi için bir `Label`, bkz: [yazı tipleri](~/xamarin-forms/user-interface/text/fonts.md).

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Kesme ve kaydırma

Etiketler, tek satırda tarafından kullanıma sunulan birkaç yoldan biriyle sığamıyorsa metin işlemek için ayarlanabilir `LineBreakMode` özelliği. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) Aşağıdaki değerleri olan bir sabit listesi verilmiştir:

- **HeadTruncation** &ndash; sonunu gösteren metnin başındaki keser.
- **CharacterWrap** &ndash; yeni bir satır metni bir karakter sınırında sarmalar.
- **MiddleTruncation** &ndash; başlangıcını ve bitişini üç nokta orta değer ile metin görüntüler.
- **NoWrap** &ndash; yalnızca görüntüleme metni kaydırılacak kadar metin kutusu olarak uygun bir satır.
- **TailTruncation** &ndash; son kesiliyor metnin başlangıcını gösterir.
- **Sözcük kaydırmayı** &ndash; metin sözcük sınırında sarmalar.

## <a name="displaying-a-specific-number-of-lines"></a>Belirli sayıda satır görüntüleme

Tarafından görüntülenen satır sayısı bir [ `Label` ](xref:Xamarin.Forms.Label) ayarlayarak belirtilebilir `Label.MaxLines` özelliğini bir `int` değeri:

- Zaman `MaxLines` 0 ' dır `Label` değerini uyar [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) ya da büyük olasılıkla kesildi, tek bir satır özelliğini veya tüm satırları tüm metin.
- Zaman `MaxLines` 1 sonuç ayarına aynıdır [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) özelliğini [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode), veya [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode). Ancak, `Label` değerini uyar [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) özelliği varsa bir üç nokta yerleşimini onaylamaz.
- Zaman `MaxLines` 1'den büyükse `Label` satırları, belirtilen sayıda en fazla değeri bağlı kalarak görüntüler [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) özelliği varsa bir üç nokta yerleşimini onaylamaz. Ancak, ayarı `MaxLines` 1 etkisizdir büyük bir değere [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) özelliği [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode).

Aşağıdaki XAML örnek ayar gösterir `MaxLines` özelliği bir [ `Label` ](xref:Xamarin.Forms.Label):

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

Eşdeğer C# kodu verilmiştir:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

Aşağıdaki ekran görüntüleri ayarı sonucu göster `MaxLines` özelliğini metni 2'den fazla satır kaplayabilir yetecek kadar uzun olduğunda 2:

![](label-images/label-maxlines.png "Etiket MaxLines örneği")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Biçimlendirilmiş metin

Etiketleri kullanıma bir [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) birden çok yazı tipi ile metin sunumu sağlar ve aynı görünümde renkleri özelliği.

`FormattedText` Özelliği türüdür [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), bir veya daha fazla oluşur [ `Span` ](xref:Xamarin.Forms.Span) aracılığıyla ayarlanan örnekleri [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) özelliği . Aşağıdaki `Span` özellikler, görsel görünümünü ayarlamak için kullanılabilir:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – Aralık arka plan rengi.
- [`Font`](xref:Xamarin.Forms.Span.Font) – metnin yazı tipini de aralık.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – aralık içindeki metni için yazı tipi öznitelikleri.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – metnin yazı tipini de aralık ait olduğu yazı tipi ailesi.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – metnin yazı tipini de aralık boyutu.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – Aralık metin rengi. Bu özellik artık kullanılmıyor ve almıştır `TextColor` özelliği.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -Aralık varsayılan satır yüksekliği uygulamak için çarpan. Daha fazla bilgi için [satır yüksekliği](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style) – yayılma uygulamak için stili.
- [`Text`](xref:Xamarin.Forms.Span.Text) – aralığın metin.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – Aralık metin rengi.
- `TextDecoration` -Aralık metinde uygulamak için süslemeleri. Daha fazla bilgi için [metin düzenlemelerinin](#text-decorations).

Ayrıca, [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) özelliği hareketlerine yanıt vereceğini hareket tanıyıcılar koleksiyonu tanımlamak için kullanılabilir [ `Span` ](xref:Xamarin.Forms.Span).

Aşağıdaki XAML örnek gösterir bir `FormattedText` oluşan üç özellik [ `Span` ](xref:Xamarin.Forms.Span) örnekleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label LineBreakMode="WordWrap">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}">
                        <Span.GestureRecognizers>
                            <TapGestureRecognizer Command="{Binding TapCommand}" />
                        </Span.GestureRecognizers>
                    </Span>
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

        var span = new Span { Text = "default, " };
        span.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(async () => await DisplayAlert("Tapped", "This is a tapped Span.", "OK")) });
        formattedString.Spans.Add(span);
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!IMPORTANT]
> [ `Text` ](xref:Xamarin.Forms.Span.Text) Özelliği bir `Span` veri bağlama aracılığıyla ayarlanabilir. Daha fazla bilgi için [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Unutmayın bir [ `Span` ](xref:Xamarin.Forms.Span) de aralık için kullanıcının eklenen tüm hareketleri yanıt verebilir [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) koleksiyonu. Örneğin, bir [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) ikinci eklenen `Span` , yukarıdaki kod örnekleri. Bu nedenle, bu `Span` dokunulduğunda `TapGestureRecognizer` yürüterek yanıtlar `ICommand` tarafından tanımlanan [ `Command` ](xref:Xamarin.Forms.TapGestureRecognizer.Command) özelliği. Hareket tanıyıcılar hakkında daha fazla bilgi için bkz: [Xamarin.Forms hareketlerini](~/xamarin-forms/app-fundamentals/gestures/index.md).

Aşağıdaki ekran görüntüleri ayarı sonucu göster `FormattedString` üç özellik `Span` örnekleri:

![](label-images/formattedtext.png "Etiket FormattedText örneği")

## <a name="line-height"></a>Satır yüksekliği

Dikey yüksekliğini bir [ `Label` ](xref:Xamarin.Forms.Label) ve [ `Span` ](xref:Xamarin.Forms.Span) ayarlanarak özelleştirilebilir [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) özelliği veya [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) için bir `double` değeri. İOS ve Android'de bu çarpanları özgün satır yüksekliği ve evrensel Windows Platformu (UWP) üzerinde değerler `Label.LineHeight` özellik değeri, etiket yazı tipi boyutu bir çarpan.

> [!NOTE]
> - İos'ta [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) ve [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) özelliklerini tek bir satıra sığacak metin ve çoklu satırlara sarar metin satırının yüksekliğini değiştirin.
> - Android, [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) ve [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) özellikleri yalnızca çoklu satırlara sarar metin satırının yüksekliğini değiştirin.
> - UWP üzerinde [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) satır yüksekliği, birden çok satırlara sarar metin özelliğini değiştirir ve [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) özelliğinin hiçbir etkisi.

Aşağıdaki XAML örnek ayar gösterir [ `LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) özelliği bir [ `Label` ](xref:Xamarin.Forms.Label):

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

Eşdeğer C# kodu verilmiştir:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

Aşağıdaki ekran görüntüleri ayarı sonucu göster [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) 1.8 özelliği:

![](label-images/label-lineheight.png "Etiket LineHeight örneği")

Aşağıdaki XAML örnek ayar gösterir [ `LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) özelliği bir [ `Span` ](xref:Xamarin.Forms.Span):

```xaml
<Label LineBreakMode="WordWrap">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. "
                  LineHeight="1.8"/>
            <Span Text="Nullam feugiat sodales elit, et maximus nibh vulputate id."
                  LineHeight="1.8" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Eşdeğer C# kodu verilmiştir:

```csharp
var formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. ",
  LineHeight = 1.8
});
formattedString.Spans.Add(new Span
{
  Text = "Nullam feugiat sodales elit, et maximus nibh vulputate id.",
  LineHeight = 1.8
});
var label = new Label
{
  FormattedText = formattedString,
  LineBreakMode = LineBreakMode.WordWrap
};
```

Aşağıdaki ekran görüntüleri ayarı sonucu göster [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) 1.8 özelliği:

![](label-images/span-lineheight.png "Span LineHeight örneği")

## <a name="styling-labels"></a>Stil etiketleri

Önceki bölümlerde ele ayarı [ `Label` ](xref:Xamarin.Forms.Label) ve [ `Span` ](xref:Xamarin.Forms.Span) örneği başına temelinde özellikleri. Ancak, özellik kümeleri, tutarlı bir şekilde bir veya daha çok görünümleri için uygulanan bir stil içinde gruplandırılabilir. Bu kodun okunabilirliğini artırmak ve tasarım değişiklikleri uygulamak daha kolay hale getirebilirsiniz. Daha fazla bilgi için [stilleri](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Bölüm 3 Xamarin.Forms ile mobil uygulamalar oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Etiket API](xref:Xamarin.Forms.Label)
- [Yayılma API](xref:Xamarin.Forms.Span)
