---
title: Xamarin.Forms Düzenleyicisi
description: Bu makalede, bir uygulamada çok satırlı metin girişi kabul etmek için Xamarin.Forms düzenleyici denetimi kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 4879ff88d5bbdab5aa92024bee7f50239a141e3b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995873"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms Düzenleyicisi

_Çok satırlı metin girişi_

`Editor` Denetimi, çok satırlı giriş kabul etmek için kullanılır. Bu makalede ele alınacaktır:

- **[Özelleştirme](#customization)**  &ndash; klavye ve rengi seçeneklerini.
- **[Etkileşim](#interactivity)**  &ndash; olayları için etkileşim sağlamak için Dinledik.

## <a name="customization"></a>Özelleştirme

### <a name="setting-and-reading-text"></a>Metin okuma ve ayarlama

`Editor`, Metin sunma diğer görünümleri gibi sunan `Text` özelliği. Bu özellik tarafından sunulan metni okuyun ve ayarlamak için kullanılan `Editor`. Aşağıdaki örnek ayar gösterir `Text` XAML özelliği:

```xaml
<Editor Text="I am an Editor" />
```

C# içinde:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Metin okuma erişimi `Text` C# özelliği:

```csharp
var text = MyEditor.Text;
```

### <a name="limiting-input-length"></a>Giriş sınırlandırma

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Özelliği için izin verilen giriş uzunluğu sınırlamak için kullanılabilir [ `Editor` ](xref:Xamarin.Forms.Editor). Bu özellik, pozitif bir tamsayı olarak ayarlanmalıdır:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) özellik değerinin 0 gösterir müdahalesi izin verilmeyeceğini, değerini `int.MaxValue`, varsayılan değeri olduğu bir [ `Editor` ](xref:Xamarin.Forms.Editor), olduğunu gösterir yok etkili girilebilir karakter sayısını sınırlama.

### <a name="keyboards"></a>Klavye

Kullanıcılar ile etkileşim kurduğunuzda, sunulan klavye bir `Editor` aracılığıyla programlı olarak ayarlanabilir [ ``Keyboard`` ](xref:Xamarin.Forms.Keyboard) özelliği.

Klavye türü seçenekleri şunlardır:

- **Varsayılan** &ndash; varsayılan klavye
- **Sohbet** &ndash; telefonunuza mesaj & basamak için kullanılan emoji burada kullanışlıdır
- **E-posta** &ndash; kullanılması için e-posta adresi girme
- **Sayısal** &ndash; kullanılması için sayı girme
- **Telefon** &ndash; telefon numaraları girerken kullanılan
- **URL** &ndash; dosya yolları & web adresleri girmek için kullanılan

Var olan bir [her klavye örneği](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) tarifler bölümümüzdeki.

### <a name="enabling-and-disabling-spell-checking"></a>Yazım denetimi devre dışı bırakma ve etkinleştirme

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Özellik denetimleri olup yazım denetimi etkinleştirildi. Varsayılan olarak ayarlandığı `true`. Yazım hatası, kullanıcının girdiği metin olarak belirtilir.

Ancak, bir kullanıcı adı girerek gibi bazı metin girişi senaryolar için yazım denetimi negatif bir deneyim ve bunu devre dışı bırakılmalıdır ayarlayarak sağlar [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) özelliğini `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Zaman [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) özelliği `false`ve özel bir klavye kullanılmıyor, yerel yazım denetimi devre dışı bırakılır. Ancak, bir [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) sahip olan yazım devre dışı bırakan kümesi denetleniyor, gibi [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` özelliği yok sayılır. Bu nedenle, özellik için yazım denetimini etkinleştirme kullanılamaz bir `Keyboard` , açıkça bırakır.

### <a name="colors"></a>Renkleri

`Editor` Özel arka plan rengi ile kullanmak için ayarlanabilir `BackgroundColor` özelliği. Özel renkler her platformda kullanılabilir olmasını sağlamak gereklidir. Her platform text color için varsayılan değerleri farklı olduğundan, her platform için bir özel arka plan rengini ayarlamak gerekebilir. Bkz: [Platform ince ayarlar ile çalışan](~/xamarin-forms/platform/device.md) UI her platform için en iyi duruma getirme hakkında daha fazla bilgi.

C# içinde:

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

XAML içinde:

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

Seçtiğiniz arka plan ve metin renklerini her platformda kullanılabilir ve herhangi bir yer tutucu metin gizlememeniz yoksa emin olun.

## <a name="interactivity"></a>Etkileşim

`Editor` iki olay kullanıma sunar:

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash; düzenleyicide metni değiştiğinde harekete geçirilen. Metin önce ve sonra değişiklik sağlar.
- [Tamamlanan](xref:Xamarin.Forms.Editor.Completed) &ndash; kullanıcı girişi klavyede return tuşuna basarak sona erdiğinde oluşturulur.

### <a name="completed"></a>Tamamlandı

`Completed` Olay bir etkileşim tamamlanması için tepki vermek için kullanılan bir `Editor`. `Completed` kullanıcı girişi bir alanla klavyede return tuşuna girerek sona erdiğinde ortaya çıkar. Olay işleyicisi gönderen alma bir genel olay işleyicisidir ve `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Tamamlanan Olay kod ve XAML abone:

C# içinde:

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

XAML içinde:

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

`TextChanged` Olay, bir alanın içeriğini bir değişiklik için tepki vermek için kullanılır.

`TextChanged` ne zaman tetiklenir `Text` , `Editor` değişiklikler. Olay işleyicisi örneğini alır `TextChangedEventArgs`. `TextChangedEventArgs` eski ve yeni değerlerine erişim sağlayan `Editor` `Text` aracılığıyla `OldTextValue` ve `NewTextValue` özellikleri:

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

XAML içinde:

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
- [API Düzenleyicisi](xref:Xamarin.Forms.Editor)
