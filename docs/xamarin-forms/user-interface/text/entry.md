---
title: Xamarin.Forms giriş
description: Bu makalede, tek satırlı metin veya bir uygulamadaki parola girişi kabul edecek şekilde Xamarin.Forms giriş sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 95afdfde878759d4a598e200d16fe6fb1fa2005e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998254"
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

### <a name="keyboards"></a>Klavye

Kullanıcılar ile etkileşim kurduğunuzda, sunulan klavye bir `Entry` aracılığıyla programlı olarak ayarlanabilir `Keyboard` özelliği.

Klavye türü seçenekleri şunlardır:

- **Varsayılan** &ndash; varsayılan klavye
- **Sohbet** &ndash; telefonunuza mesaj & basamak için kullanılan emoji burada kullanışlıdır
- **E-posta** &ndash; kullanılması için e-posta adresi girme
- **Sayısal** &ndash; kullanılması için sayı girme
- **Telefon** &ndash; telefon numaraları girerken kullanılan
- **URL** &ndash; dosya yolları ve web adresleri girmek için kullanılan

Var olan bir [her klavye örneği](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) tarifler bölümümüzdeki.

### <a name="enabling-and-disabling-spell-checking"></a>Yazım denetimi devre dışı bırakma ve etkinleştirme

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Özellik denetimleri olup yazım denetimi etkinleştirildi. Varsayılan olarak ayarlandığı `true`. Yazım hatası, kullanıcının girdiği metin olarak belirtilir.

Ancak, bir kullanıcı adı girerek gibi bazı metin girişi senaryolar için yazım denetimi negatif bir deneyim ve bunu devre dışı bırakılmalıdır ayarlayarak sağlar [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) özelliğini `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Zaman [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) özelliği `false`ve özel bir klavye kullanılmıyor, yerel yazım denetimi devre dışı bırakılır. Ancak, bir [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) sahip olan yazım devre dışı bırakan kümesi denetleniyor, gibi [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` özelliği yok sayılır. Bu nedenle, özellik için yazım denetimini etkinleştirme kullanılamaz bir `Keyboard` , açıkça bırakır.

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

- [TextChanged](xref:Xamarin.Forms.Entry.TextChanged) &ndash; giriş metni değiştiğinde harekete geçirilen. Metin önce ve sonra değişiklik sağlar.
- [Tamamlanan](xref:Xamarin.Forms.Entry.Completed) &ndash; kullanıcı girişi klavyede return tuşuna basarak sona erdiğinde oluşturulur.

### <a name="completed"></a>Tamamlandı

`Completed` Olay bir girişi ile etkileşim tamamlanması için tepki vermek için kullanılır. `Completed` kullanıcı girişi bir alanla klavyede return tuşuna girerek sona erdiğinde ortaya çıkar. Olay işleyicisi gönderen alma bir genel olay işleyicisidir ve `EventArgs`:

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
