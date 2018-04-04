---
title: Etiketle
description: Xamarin.Forms görüntüleme metni
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: c1056626b336dd9b6ce265ab693ceed2a24eae0f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="label"></a>Etiketle

_Xamarin.Forms görüntüleme metni_

`Label` Görünümü, hem tek hem de çok satırlı metin görüntülemek için kullanılır. Etiket (aileleri, boyutlar ve Seçenekler) özel yazı tipleri ve renkli metin olabilir. Bu makalede aşağıdaki konuları içerir:

- **[Kesme ve kaydırma](#Truncation_and_Wrapping)**  &ndash; kesilmesi ve kaydırma metin olamaz nerelerde tek bir satırda durumları işlemek için seçenekleri.
- **[Yazı tipi](#Font)**  &ndash; yazı tipi seçenekleri.
- **[Renk](#Color)**  &ndash; renk seçeneklerini metin etiketi.
- **[Biçimlendirilmiş metin](#Formatted_Text)**  &ndash; birden çok biçimleri/stilleri satır içi metni görüntülemek için Seçenekler.

## <a name="styling-label"></a>Stil etiketi

Aşağıdaki bölümlerde ayarı özelliklerini kapak `Label` örneği başına temelinde el ile. Bir veya daha çok görünümlerine sürekli olarak uygulanan bir stil özelliklerini ayarlar Not toplanabilir. Bu kod okunabilirliğini artırmak ve tasarım değişiklikleri uygulamak daha kolay hale getirebilirsiniz. Bkz: [stilleri](~/xamarin-forms/user-interface/text/styles.md) daha fazla bilgi için.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Kesme ve kaydırma

Etiketler, tarafından sunulan çeşitli yollardan içinde tek bir satırda sığamıyorsa metin işlemek üzere ayarlanabilir `LineBreakMode` özelliği. [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) Aşağıdaki seçeneklerden bir listesi verilmiştir:

- **HeadTruncation** &ndash; son gösteren metnin başındaki tamsayıya dönüştürür.
- **CharacterWrap** &ndash; yeni bir satır metne karakter sınırında sarmalar.
- **MiddleTruncation** &ndash; başlangıcını ve bitişini metni Orta Değiştir üç nokta ile görüntüler.
- **NoWrap** &ndash; yalnızca görüntüleme metni kaydırılacak kadar metin can olarak tek bir satırda sığmayacak.
- **TailTruncation** &ndash; son kesilmesiyle metnin başlangıcını gösterir.
- **WordWrap** &ndash; metin word sınırında sarmalar.

## <a name="font"></a>Yazı tipi

Bkz: [yazı tipleriyle çalışma](~/xamarin-forms/user-interface/text/fonts.md) daha fazla bilgi için.

## <a name="color"></a>Renk

`Label`Özel metin rengi bağlanabilirse aracılığıyla kullanmak için s ayarlanabilir `TextColor` özelliği.

Renkler her platformda kullanılabilir olacağından emin olmak özellikle dikkatli gereklidir. Her platform için metin ve arka plan renklerini farklı Varsayılanları olduğundan, her üzerinde çalıştığı bir varsayılan çekme dikkatli olun gerekir.

Bir etiket metin rengini ayarlamak için aşağıdaki kodu kullanın:

Kod:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

XAML'de:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "Etiket TextColor örneği")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Biçimlendirilmiş metin

Etiketleri kullanıma bir `FormattedText` metin ile birden çok yazı tipi sunmak sağlar ve aynı görünümünde renkleri özelliği.

`FormattedText` Özelliği türüdür [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/). Biçimlendirilmiş dizeler oluşan bir veya daha fazla `Span`s, her biri aşağıdaki özelliklere sahip:

- **BackgroundColor** &ndash; örneğin vurgulama efekti elde etmek bir arka plan rengini ayarlamak için kullanılabilir.
- **FontAttributes** &ndash; kümesi kalın, italik veya hiçbiri olabilir.
- **FontFamily** &ndash; kullanılacak yazı tipini belirler.
- **FontSize** &ndash; metin boyutunu ayarlar.
- **ForegroundColor** &ndash; metin rengini belirler.
- **Metin** &ndash; sunulacak metin.

Aşağıdaki C# kod burada ilk word kalın ve son sözcük kırmızı bir etiket gösterir:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

XAML'de:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "Etiket FormattedText örneği")


## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 3 Xamarin.Forms ile mobil uygulamaları oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Etiket API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
