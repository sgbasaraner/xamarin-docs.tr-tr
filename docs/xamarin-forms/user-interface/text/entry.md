---
title: Xamarin.Forms giriş
description: Bu makalede, tek satırlı metin veya bir uygulamadaki parola girişi kabul edecek şekilde Xamarin.Forms giriş sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: 5ccd2a653e5190df11a58477905e868b25878e44
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270118"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms giriş

_Tek satırlı metin veya parola giriş_

Xamarin.Forms `Entry` tek satırlı metin girişi için kullanılır. `Entry`Gibi `Editor` görüntüleme, birden fazla klavye türlerini destekler. Ayrıca, `Entry` parola alanı kullanılabilir.

## <a name="display-customization"></a>Görüntü özelleştirme

### <a name="setting-and-reading-text"></a>Metin okuma ve ayarlama

`Entry`, Metin sunma diğer görünümleri gibi sunan `Text` özelliği. Bu özellik tarafından sunulan metni okuyun ve ayarlamak için kullanılan `Entry`. Aşağıdaki örnek ayar gösterir `Text` XAML özelliği:

```xaml
<Entry Text="I am an Entry" />
```

C# içinde:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Metin okuma erişimi `Text` C# özelliği:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Genişliğini bir `Entry` ayarlayarak tanımlanabilir, `WidthRequest` özelliği. Genişliğini bağlı olmayan bir `Entry` tanımlanan değerini temel alarak kendi `Text` özelliği.

### <a name="limiting-input-length"></a>Giriş sınırlandırma

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Özelliği için izin verilen giriş uzunluğu sınırlamak için kullanılabilir [ `Entry` ](xref:Xamarin.Forms.Entry). Bu özellik, pozitif bir tamsayı olarak ayarlanmalıdır:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) özellik değerinin 0 gösterir müdahalesi izin verilmeyeceğini, değerini `int.MaxValue`, varsayılan değeri olduğu bir [ `Entry` ](xref:Xamarin.Forms.Entry), olduğunu gösterir yok etkili girilebilir karakter sayısını sınırlama.

### <a name="customizing-the-keyboard"></a>Klavye özelleştirme

Kullanıcılar ile etkileşim kurduğunuzda, sunulan klavye bir [ `Entry` ](xref:Xamarin.Forms.Entry) aracılığıyla programlı olarak ayarlanabilir [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) özelliğini Aşağıdakiözelliklerindenbirine[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) sınıfı:

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
<Entry Keyboard="Chat" />
```

Eşdeğer C# kodu verilmiştir:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
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
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

Eşdeğer C# kodu verilmiştir:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Return tuşuna özelleştirme

Return tuşuna olduğundan geçici klavye görünümünü olduğunda görüntülenen bir [ `Entry` ](xref:Xamarin.Forms.Entry) odaklanmış, ayarlanarak özelleştirilebilir [ `ReturnType` ](xref:Xamarin.Forms.Entry.ReturnType) birdeğere[ `ReturnType` ](xref:Xamarin.Forms.ReturnType) sabit listesi:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) – belirli dönüş anahtar gereklidir ve platform varsayılan kullanılacak gösterir.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) – bir "Bitti" dönüş anahtar gösterir.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) – "Git" dönüş anahtar belirtir.
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – bir "İleri" dönüş anahtar gösterir.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – "Arama" dönüş anahtar belirtir.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – bir "Gönderme" dönüş anahtar gösterir.

Aşağıdaki XAML örnek return tuşuna ayarlama işlemini gösterir:

```xaml
<Entry ReturnType="Send" />
```

Eşdeğer C# kodu verilmiştir:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Return tuşuna tam görünümünü platformuna göre bağlıdır. İOS, dönüş anahtar metin tabanlı bir düğme bulunur. Ancak, Android ve evrensel Windows Platform dönüş anahtar simgesi tabanlı bir düğme bulunur.

Return tuşuna basıldığında [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) olay harekete geçirilir ve tüm `ICommand` tarafından belirtilen [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) özelliği yürütülür. Ayrıca, tüm `object` tarafından belirtilen [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) özelliği geçirilir `ICommand` bir parametre olarak. Komutlar hakkında daha fazla bilgi için bkz. [komut arabirimi](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

### <a name="enabling-and-disabling-spell-checking"></a>Yazım denetimi devre dışı bırakma ve etkinleştirme

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Özellik denetimleri olup yazım denetimi etkinleştirildi. Varsayılan olarak ayarlandığı `true`. Yazım hatası, kullanıcının girdiği metin olarak belirtilir.

Ancak, bir kullanıcı adı girerek gibi bazı metin girişi senaryolar için yazım denetimi bir negatif deneyimi sağlar ve ayarı devre dışı bırakılmalıdır [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) özelliğini `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Zaman [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) özelliği `false`ve özel bir klavye kullanılmıyor, yerel yazım denetimi devre dışı bırakılır. Ancak, bir [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) sahip olan yazım devre dışı bırakan kümesi denetleniyor, gibi [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` özelliği yok sayılır. Bu nedenle, özellik için yazım denetimini etkinleştirme kullanılamaz bir `Keyboard` , açıkça bırakır.

### <a name="enabling-and-disabling-text-prediction"></a>Metin tahmini devre dışı bırakma ve etkinleştirme

[ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) Özellik denetimleri olmadığını metin tahminini ve otomatik metin düzeltme etkindir. Varsayılan olarak ayarlandığı `true`. Kullanıcının girdiği metin gibi sözcük Öngörüler sunulur.

Ancak, bazı metin girişi senaryolar için bir kullanıcı adı, metin tahmini ve otomatik metin girerek gibi düzeltme negatif bir deneyim sağlar ve ayarı devre dışı bırakılmalıdır [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) özelliğini `false`:

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Zaman [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) özelliği `false`, ve özel bir klavye değil kullanıldığında, metin tahmini ve otomatik metin düzeltme devre dışı bırakıldı. Ancak, bir [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) devre dışı bırakır, metin tahmini ayarlandı `IsTextPredictionEnabled` özelliği yok sayılır. Bu nedenle, özellik için metin tahmininin etkinleştirmek için kullanılamaz bir `Keyboard` , açıkça bırakır.

### <a name="placeholders"></a>Yer tutucuları

`Entry` kullanıcı girişi işlendiğinde değil yer tutucu metin göstermek için ayarlanabilir. Uygulamada, bu çok formlarında belirli bir alan için uygun olan içerik açıklamak için görülür. Yer tutucu metin rengi özelleştirilemiyor ve bakılmaksızın aynı olacak `TextColor` ayarı. Tasarımınızı için özel bir yer tutucu rengi çağırırsa, geri döner gerekecektir bir [özel Oluşturucu](). Aşağıdaki oluşturacak bir `Entry` XAML içinde yer tutucu olarak "Username" ile:

```xaml
<Entry Placeholder="Username" />
```

C# içinde:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Yer tutucu örnek giriş")

### <a name="password-fields"></a>Parola alanı

`Entry` sağlar `IsPassword` özelliği. Zaman `IsPassword` olduğu `true`, alanın içeriğini Siyah daire sunulur:

XAML içinde:

```xaml
<Entry IsPassword="true" />
```

C# içinde:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Giriş IsPassword örneği")

Yer tutucuları örnekleriyle kullanılan `Entry` parola alanları olarak yapılandırılır:

XAML içinde:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C# içinde:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Giriş IsPassword ve yer tutucu örneği")

### <a name="colors"></a>Renkleri

Giriş, özel bir arka plan ve metin renklerini aşağıdaki bağlanabilir özellikler aracılığıyla ayarlanabilir:

- **TextColor** &ndash; metnin rengini ayarlar.
- **BackgroundColor** &ndash; gösterilen metin rengini belirler.

Özel renkler her platformda kullanılabilir olmasını sağlamak gereklidir. Her platform için metin ve arkaplan renklerini varsayılan değerleri farklı olduğundan, genellikle bir ayarlarsanız her ikisi de ayarlamanız gerekir.

Bir girdinin metin rengini ayarlamak için aşağıdaki kodu kullanın:

XAML içinde:

```xaml
<Entry TextColor="Green" />
```

C# içinde:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "TextColor örnek giriş")

Yer tutucu olmadığına dikkat edin etkilenen tarafından belirtilen `TextColor`.

XAML içinde arka plan rengini ayarlamak için:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C# içinde:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "BackgroundColor örnek giriş")

Seçtiğiniz arka plan ve metin renklerini her platformda kullanılabilir ve herhangi bir yer tutucu metin gizlememeniz yoksa emin olmak dikkatli olun.

## <a name="events-and-interactivity"></a>Olaylar ve etkileşim

Giriş iki olayları gösterir:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; metin girişi değiştiğinde oluşturulur. Metin önce ve sonra değişiklik sağlar.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; kullanıcı girişi klavyede return tuşuna basarak sona erdiğinde oluşturulur.

### <a name="completed"></a>Tamamlandı

`Completed` Olay bir girişi ile etkileşim tamamlanması için tepki vermek için kullanılır. `Completed` kullanıcı girişi bir alanla klavyede return tuşuna basarak sona erdiğinde ortaya çıkar. Olay işleyicisi gönderen alma bir genel olay işleyicisidir ve `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Tamamlanan Olay XAML içinde abone:

```xaml
<Entry Completed="Entry_Completed" />
```

ve C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

Sonra [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) olayı tetikler, her `ICommand` tarafından belirtilen [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) özelliği yürütüldüğünde, ile `object` tarafından belirtilen [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) özelliği için geçirilen `ICommand`.

### <a name="textchanged"></a>TextChanged

`TextChanged` Olay, bir alanın içeriğini bir değişiklik için tepki vermek için kullanılır.

`TextChanged` ne zaman tetiklenir `Text` , `Entry` değişiklikler. Olay işleyicisi örneğini alır `TextChangedEventArgs`. `TextChangedEventArgs` eski ve yeni değerlerine erişim sağlayan `Entry` `Text` aracılığıyla `OldTextValue` ve `NewTextValue` özellikleri:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Olay abone olabilir XAML içinde:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

ve C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>İlgili bağlantılar

- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Giriş API'si](xref:Xamarin.Forms.Entry)
