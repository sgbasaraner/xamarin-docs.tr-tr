---
title: Xamarin.Forms etiketi
description: Bu makalede, uygulamaların tek ve çok satırlı metni görüntülemek için Xamarin.Forms etiket sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: ce602a84ea1024dc22298a3ec1567a9a34ad4a82
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995972"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms etiketi

_Xamarin.Forms içinde metin görüntüleme_

`Label` Görünümü, hem tek hem de çok satırlı bir metin görüntülemek için kullanılır. Etiket (aileleri, boyutlar ve Seçenekler) özel yazı tipleri ve renkli metin olabilir. Bu makalede, aşağıdaki konular ele alınmaktadır:

- **[Kesme ve sarmalama](#Truncation_and_Wrapping)**  &ndash; kesme ve kaydırma metin olamaz nerelerde tek bir satırda durumları işlemek için seçenekleri.
- **[Yazı tipi](#Font)**  &ndash; yazı tipi seçenekleri.
- **[Renk](#Color)**  &ndash; metin rengi seçeneklerini etiketleyin.
- **[Biçimlendirilmiş metin](#Formatted_Text)**  &ndash; ile birden çok biçimleri/stilleri satır içi metin görüntüleme seçenekleri.

## <a name="styling-label"></a>Stil etiketi

Aşağıdaki bölümlerde ayarı özelliklerini kapsayan `Label` örneği başına temelinde el ile. Bir veya daha çok görünümleri için sürekli olarak uygulanan bir stil özelliklerini ayarlar Not toplanabilir. Bu kodun okunabilirliğini artırmak ve tasarım değişiklikleri uygulamak daha kolay hale getirebilirsiniz. Bkz: [stilleri](~/xamarin-forms/user-interface/text/styles.md) daha fazla bilgi için.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Kesme ve kaydırma

Etiketler, tek satırda tarafından kullanıma sunulan birkaç yoldan biriyle sığamıyorsa metin işlemek için ayarlanabilir `LineBreakMode` özelliği. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) Aşağıdaki seçeneklerden bir sabit listesi verilmiştir:

- **HeadTruncation** &ndash; sonunu gösteren metnin başındaki keser.
- **CharacterWrap** &ndash; yeni bir satır metni bir karakter sınırında sarmalar.
- **MiddleTruncation** &ndash; başlangıcını ve bitişini üç nokta orta değer ile metin görüntüler.
- **NoWrap** &ndash; yalnızca görüntüleme metni kaydırılacak kadar metin kutusu olarak uygun bir satır.
- **TailTruncation** &ndash; son kesiliyor metnin başlangıcını gösterir.
- **Sözcük kaydırmayı** &ndash; metin sözcük sınırında sarmalar.

## <a name="font"></a>Yazı tipi

Bkz: [yazı tipleri ile çalışma](~/xamarin-forms/user-interface/text/fonts.md) daha fazla bilgi için.

## <a name="color"></a>Renk

`Label`s bağlanabilir aracılığıyla bir özel metin rengi ayarlanabilir `TextColor` özelliği.

Özel renkler her platformda kullanılabilir olmasını sağlamak gereklidir. Her platform için metin ve arkaplan renklerini varsayılan değerleri farklı olduğundan, her çalışan varsayılan seçmek dikkatli olmanız gerekir.

Bir etiketin metin rengini ayarlamak için aşağıdaki kodu kullanın:

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

XAML içinde:

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

Etiketleri kullanıma bir `FormattedText` birden çok yazı tipi ile metin sunmanızı sağlar ve aynı görünümde renkleri özelliği.

`FormattedText` Özelliği türüdür [ `FormattedString` ](xref:Xamarin.Forms.FormattedString). Biçimlendirilmiş dizeler oluşan bir veya daha fazla `Span`s, her biri aşağıdaki özelliklere sahip:

- **BackgroundColor** &ndash; vurgulama etkiyi elde etmek örneğin bir arka plan rengini ayarlamak için kullanılabilir.
- **FontAttributes** &ndash; kümesi kalın, italik veya hiçbiri olabilir.
- **FontFamily** &ndash; kullanılacak yazı tipini ayarlar.
- **FontSize** &ndash; metin boyutunu ayarlar.
- **ForegroundColor** &ndash; metnin rengini ayarlar.
- **Metin** &ndash; metin gösterilir.

Aşağıdaki C# kod burada ilk sözcük bold olarak belirlenmiştir ve son sözcüğü kırmızı bir etiket gösterilmektedir:

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

XAML içinde:

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

- [Bölüm 3 Xamarin.Forms ile mobil uygulamalar oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Etiket API](xref:Xamarin.Forms.Label)
