---
title: Giriş
description: Tek satırlı metin veya parola giriş
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 2e40effa7bc54b7b7cf73edaa882256fed521a95
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="entry"></a>Giriş

_Tek satırlı metin veya parola giriş_

Xamarin.Forms `Entry` tek satırlı metin girişi için kullanılır. `Entry`, düzenleyici görünümü gibi birden çok klavye türlerini destekler. Ayrıca, `Entry` parola alanı kullanılabilir.

## <a name="display-customization"></a>Görüntü özelleştirme

### <a name="setting-and-reading-text"></a>Ve ayarlama ve metin okuma

Diğer metin sunan görünümleri gibi giriş sunar `Text` özelliği. `Text` ayarlama ve tarafından sunulan metin okumak için kullanılan `Entry`. Aşağıdaki örnek XAML'de metin ayarlanmasını gösterir:

```xaml
<Entry Text="I am an Entry" />
```

C# ' de:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Metin okuma erişimi `Text` özelliği, C#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Genişliğini bir `Entry` ayarlayarak tanımlanabilir kendi `WidthRequest` özelliği. Genişliğini bağlı olmayan bir `Entry` tanımlanmakta değeri temel alarak kendi `Text` özelliği.

### <a name="keyboards"></a>Klavyeler

Kullanıcıların etkileşimli olarak yükleyen sunulan klavye bir `Entry` aracılığıyla programlı olarak ayarlanabilir `Keyboard` özelliği.

Klavye türü için Seçenekler şunlardır:

- **Varsayılan** &ndash; varsayılan klavye
- **Sohbet** &ndash; metin & yerler için kullanılan emoji nerede yararlı
- **E-posta** &ndash; e-posta adreslerini girerken kullanılan
- **Sayısal** &ndash; numaralarını girerken kullanılan
- **Telefon** &ndash; telefon numaralarını girerken kullanılan
- **URL** &ndash; dosya yolları ve web adresleri girmek için kullanılan

Var olan bir [her klavye örneği](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) bizim tarif bölümünde.

### <a name="placeholders"></a>Yer tutucuları

`Entry` kullanıcı girişi depolamayın yer tutucu metin göstermek için ayarlanabilir. Uygulamada, bu çok formlarında belirli bir alan için uygun olan içerik açıklamak için görülür. Yer tutucu metin rengi özelleştirilemez ve bakılmaksızın aynı olacak `TextColor` ayarı. Tasarımınız için özel bir yer tutucu renk çağırıyorsa, geri döner gerekir bir [özel Oluşturucu](). Aşağıdaki oluşturacak bir `Entry` XAML'de yer tutucu olarak "Username" ile:

```xaml
<Entry Placeholder="Username" />
```

C# ' de:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Giriş yer tutucu örneği")

### <a name="password-fields"></a>Parola alanları

`Entry` sağlar `IsPassword` özelliği. Zaman `IsPassword` olan `true`, alanın içeriği siyah daireler olarak sunulur:

XAML'de:

```xaml
<Entry IsPassword="true" />
```

C# ' de:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Giriş IsPassword örneği")

Yer tutucuları örnekleriyle kullanılan `Entry` parola alanlar olarak yapılandırılır:

XAML'de:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C# ' de:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Giriş IsPassword ve yer tutucu örneği")


### <a name="colors"></a>Renkleri

Giriş, özel bir arka plan ve metin renkleri bağlanabilirse aşağıdakiler aracılığıyla kullanmak üzere ayarlanabilir:

- **TextColor** &ndash; metin rengini belirler.
- **BackgroundColor** &ndash; gösterilen metin rengini belirler.

Renkler her platformda kullanılabilir olacağından emin olmak özellikle dikkatli gereklidir. Her platform için metin ve arka plan renklerini farklı Varsayılanları olduğundan, genellikle bir ayarlarsanız her ikisi de ayarlamanız gerekir.

Bir giriş metin rengini ayarlamak için aşağıdaki kodu kullanın:

XAML'de:

```xaml
<Entry TextColor="Green" />
```

C# ' de:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "Giriş TextColor örneği")

Yer tutucu olmadığına dikkat edin etkilenen tarafından belirtilen `TextColor`.

XAML'de arka plan rengini ayarlamak için:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C# ' de:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Giriş BackgroundColor örneği")

Seçtiğiniz arka plan ve metin renklerini her platformda kullanılabilir ve herhangi bir yer tutucu metin soyutlamaması yok emin olmak dikkatli olun.

## <a name="events-and-interactivity"></a>Olaylar ve etkileşim

Giriş iki olayları gösterir:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; giriş metni değiştirir tetiklenir. Metin önce ve sonra değişiklik sağlar.
- [Tamamlanan](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; klavyede return tuşuna basarak, kullanıcı giriş sona erdi tetiklenir.

### <a name="completed"></a>Tamamlandı

`Completed` Olay tamamlanması için bir girişi ile etkileşim tepki göstermek için kullanılır. `Completed` kullanıcı girişi bir alanla klavyede return tuşuna girerek sona erdiğinde tetiklenir. Olayı için gönderen çıkarmadan bir genel olay işleyici işleyicisidir ve `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Tamamlanan Olay XAML'de abone:

```xaml
<Entry Completed="Entry_Completed" />
```

ve C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Olay, bir alanın içeriği değişikliği tepki göstermek için kullanılır.

`TextChanged` Her tetiklenir `Text` , `Entry` değişiklikler. Olay işleyicisi örneği alır `TextChangedEventArgs`. `TextChangedEventArgs` eski ve yeni değerlerine erişim sağlayan `Entry` `Text` aracılığıyla `OldTextValue` ve `NewTextValue` özellikleri:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Olay abone olmak için XAML içinde:

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
- [Giriş API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
