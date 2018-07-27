---
title: Xamarin.Forms Düzenleyicisi
description: Bu makalede, bir uygulamada çok satırlı metin girişi kabul etmek için Xamarin.Forms düzenleyici denetimi kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/13/2018
ms.openlocfilehash: 6e3cf12431440823b1d32d91927bc634f60fd5e2
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270462"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms Düzenleyicisi

_Çok satırlı metin girişi_

[ `Editor` ](xref:Xamarin.Forms.Editor) Denetimi, çok satırlı giriş kabul etmek için kullanılır. Bu makalede ele alınmıştır:

- **[Özelleştirme](#customization)**  &ndash; klavye ve rengi seçeneklerini.
- **[Etkileşim](#interactivity)**  &ndash; olayları için etkileşim sağlamak için Dinledik.

## <a name="customization"></a>Özelleştirme

### <a name="setting-and-reading-text"></a>Metin okuma ve ayarlama

[ `Editor` ](xref:Xamarin.Forms.Editor), Metin sunma diğer görünümleri gibi sunan `Text` özelliği. Bu özellik tarafından sunulan metni okuyun ve ayarlamak için kullanılan `Editor`. Aşağıdaki örnek ayar gösterir `Text` XAML özelliği:

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

### <a name="auto-sizing-an-editor"></a>Bir düzenleyici otomatik boyutlandırma

Bir [ `Editor` ](xref:Xamarin.Forms.Editor) otomatik boyuta içeriğine ayarını belirleyerek yapılabilir [ `Editor.AutoSize` ](xref:Xamarin.Forms.Editor.AutoSize) özelliğini [ `TextChanges` ](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), değeriolduğu[ `EditoAutoSizeOption` ](xref:Xamarin.Forms.EditorAutoSizeOption) sabit listesi. Bu numaralandırma iki değere sahiptir:

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) otomatik yeniden boyutlandırma devre dışı bırakıldı ve varsayılan değer gösterir.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) otomatik yeniden boyutlandırma etkin olduğunu gösterir.

Bu gibi kodda gerçekleştirilebilir:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Yüksekliğini otomatik boyutlandırma etkinleştirildiğinde, [ `Editor` ](xref:Xamarin.Forms.Editor) isteğe bağlı olarak kullanıcı metniyle doldurur ve metin kullanıcı sildiği yüksekliği azalır artacaktır.

> [!NOTE]
> Bir [ `Editor` ](xref:Xamarin.Forms.Editor) olacak otomatik boyutlu if [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) özelliğini ayarlayın.

### <a name="customizing-the-keyboard"></a>Klavye özelleştirme

Kullanıcılar ile etkileşim kurduğunuzda, sunulan klavye bir [ `Editor` ](xref:Xamarin.Forms.Editor) aracılığıyla programlı olarak ayarlanabilir [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) özelliğini Aşağıdakiözelliklerindenbirine[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) sınıfı:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – metin için kullanılan ve emoji burada kullanışlıdır.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – varsayılan klavye.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – e-posta adreslerini girerek kullanılır.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – numaraları girerken kullanılır.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – olmadan herhangi bir metin girerken kullanılan [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags) belirtilen.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – telefon numaraları girerken kullanılır.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – metin girerek kullanılır.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – dosya yolları ve web adreslerini girerek için kullanılır.

Bu gibi XAML içinde gerçekleştirilebilir:

```xaml
<Editor Keyboard="Chat" />
```

Eşdeğer C# kodu verilmiştir:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Her klavye örnekler bulunabilir bizim [tarifler](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) depo.

[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Sınıfı de sahip bir [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) büyük/küçük harf, yazım denetimi ve öneri davranışı belirtilerek bir klavye özelleştirmek için kullanılan Üreteç yöntemi. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) özelleştirilmiş ile yöntemi için bağımsız değişkeni olarak belirtilen sabit listesi değerleri `Keyboard` döndürülen. `KeyboardFlags` Numaralandırma aşağıdaki değerleri içerir:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – herhangi bir özellik için klavyeyi eklenir.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) – Girilen her cümlede ilk sözcüğün ilk harfini otomatik olarak büyük olduğunu gösterir.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) – Bu yazım denetimi, girilen metni gerçekleştirilecek gösterir.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) – Bu sözcük tamamlamaları sunulacak girilen metni gösterir.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – Her sözcüğün ilk harfini otomatik olarak büyük olduğunu gösterir.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – her karakteri otomatik olarak büyük olduğunu gösterir.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – hiçbir otomatik büyük harfe çevirme oluşacağını belirtir.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) -Yazım denetimi, sözcük tamamlamaları ve cümle büyük/küçük harf girilen metni oluşacağını belirtir.

Aşağıdaki XAML kod örneği, varsayılan özelleştirmek gösterilmektedir [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) sözcük tamamlamaları sunar ve büyük harfe girilen her karakter için:

```xaml
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

Eşdeğer C# kodu verilmiştir:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

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
