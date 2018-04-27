---
title: Düzenleyici
description: Çok satırlı metin girişi
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: dc83defcb3eb69cf53c205793ce77029c0947c2f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="editor"></a>Düzenleyici

_Çok satırlı metin girişi_

`Editor` Denetimi, çok satırlı girişi kabul etmek için kullanılır. Bu makalede ele alınacaktır:

- **[Özelleştirme](#Customization)**  &ndash; klavye ve renk seçeneklerini.
- **[Etkileşim](#Interactivity)**  &ndash; olayları için etkileşim sağlamak için kulak.

## <a name="customization"></a>Özelleştirme

### <a name="setting-and-reading-text"></a>Ve ayarlama ve metin okuma

Diğer metin sunan görünümleri gibi Düzenleyicisi'ni kullanıma sunan `Text` özelliği. `Text` ayarlama ve tarafından sunulan metin okumak için kullanılan `Editor`. Aşağıdaki örnek XAML'de ayarı metin gösterir:

```xaml
<Editor Text="I am an Editor" />
```

C# ' de:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Metin okuma erişimi `Text` özelliği, C#:

```csharp
var text = MyEditor.Text;
```

### <a name="keyboards"></a>Klavyeler

Kullanıcıların etkileşimli olarak yükleyen sunulan klavye bir `Editor` aracılığıyla programlı olarak ayarlanabilir [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/) özelliği.

Klavye türü için Seçenekler şunlardır:

- **Varsayılan** &ndash; varsayılan klavye
- **Sohbet** &ndash; metin & yerler için kullanılan emoji nerede yararlı
- **E-posta** &ndash; e-posta adreslerini girerken kullanılan
- **Sayısal** &ndash; numaralarını girerken kullanılan
- **Telefon** &ndash; telefon numaralarını girerken kullanılan
- **URL** &ndash; dosya yolları & web adresleri girmek için kullanılan

Var olan bir [her klavye örneği](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) bizim tarif bölümünde.

### <a name="colors"></a>Renkleri

`Editor` özel bir arka plan rengi aracılığıyla kullanacak şekilde ayarlanabilir `BackgroundColor` özelliği. Renkler her platformda kullanılabilir olacağından emin olmak özellikle dikkatli gereklidir. Her platform için metin rengi farklı varsayılan ayarlar olduğundan, her platform için bir özel bir arka plan rengi ayarlamanız gerekebilir. Bkz: [Platform Tweaks ile çalışma](~/xamarin-forms/platform/device.md) UI her platform için en iyi duruma getirme hakkında daha fazla bilgi.

C# ' de:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

XAML'de:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "BackgroundColor örnekle Düzenleyicisi")

Seçtiğiniz arka plan ve metin renklerini her platformda kullanılabilir ve herhangi bir yer tutucu metin soyutlamaması yok olduğundan emin olun.

## <a name="interactivity"></a>Etkileşim

`Editor` iki olayları gösterir:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash; metin düzenleyicide değiştiğinde oluşturulur. Metin önce ve sonra değişiklik sağlar.
- [Tamamlanan](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) &ndash; klavyede return tuşuna basarak, kullanıcı giriş sona erdi tetiklenir.

### <a name="completed"></a>Tamamlandı

`Completed` Olay tamamlanması ile etkileşim için tepki göstermek için kullanılan bir `Editor`. `Completed` kullanıcı girişi bir alanla klavyede return tuşuna girerek sona erdiğinde tetiklenir. Olayı için gönderen çıkarmadan bir genel olay işleyici işleyicisidir ve `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Tamamlanan Olay kod ve XAML abone:

C# ' de:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

XAML'de:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Olay, bir alanın içeriği değişikliği tepki göstermek için kullanılır.

`TextChanged` Her tetiklenir `Text` , `Editor` değişiklikler. Olay işleyicisi örneği alır `TextChangedEventArgs`. `TextChangedEventArgs` eski ve yeni değerlerine erişim sağlayan `Editor` `Text` aracılığıyla `OldTextValue` ve `NewTextValue` özellikleri:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Tamamlanan Olay kod ve XAML abone:

Kod:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

XAML'de:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```


## <a name="related-links"></a>İlgili bağlantılar

- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Düzenleyici API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
