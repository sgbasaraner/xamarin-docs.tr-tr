---
title: Xamarin.Forms XAML ad alanları
description: XAML için ad alanı bildirimi xmlns XML özniteliği kullanır. Bu makalede, XAML ad alanı sözdizimi tanıtır ve bir tür erişmek için XAML ad alanı bildirmek gösterilmektedir.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: 30cbb2c3aebdafe2ebf35598c520ae725e01ce65
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995150"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Xamarin.Forms XAML ad alanları

_XAML için ad alanı bildirimi xmlns XML özniteliği kullanır. Bu makalede, XAML ad alanı sözdizimi tanıtır ve bir tür erişmek için XAML ad alanı bildirmek gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Bir XAML dosyasının kök öğesi içinde her zaman olan iki XAML ad alanı bildirimi vardır. İlk aşağıdaki XAML kod örneğinde gösterildiği gibi varsayılan ad alanını tanımlar:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Varsayılan ad alanı önek ile XAML dosyası içinde tanımlanan öğeler Xamarin.Forms sınıflarına gibi başvurduğunu belirtir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

İkinci ad alanı bildirimi kullanan `x` , aşağıdaki XAML kod örneğinde gösterildiği gibi önek:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML ad alanı içindeki türleri başvururken kullanılan öneki ile varsayılan olmayan ad alanları, bildirmek için ön ekleri kullanır. `x` Ad alanı bildirimi belirtir öneki olan XAML içinde tanımlanan öğeleri `x` öğeleri ve XAML için (özellikle 2009 XAML belirtimi) iç öznitelikleri için kullanılır.

Aşağıdaki tabloda ana hatlarını `x` Xamarin.Forms tarafından desteklenen ad alanı öznitelikleri:

|Oluştur|Açıklama|
|--- |--- |
|`x:Arguments`|Varsayılan olmayan bir oluşturucu ya da bir fabrika yöntemi nesne bildirimi oluşturucu bağımsız değişkenleri belirtir.|
|`x:Class`|XAML içinde tanımlanmış bir sınıf ad alanını ve sınıf adını belirtir. Sınıf adı, arka plan kod dosyasını sınıf adı eşleşmelidir. Bu yapı bir XAML dosyasının kök öğesi yalnızca görünebilir unutmayın.|
|`x:FactoryMethod`|Bir nesneyi başlatmak için kullanılan bir Üreteç yöntemi belirtir.|
|`x:FieldModifier`|Oluşturulan alanlar adlandırılmış XAML öğeleri için erişim düzeyini belirtir.|
|`x:Key`|Her kaynak için benzersiz bir kullanıcı tanımlı anahtar belirtir bir `ResourceDictionary`. Anahtarın değeri, XAML kaynak almak için kullanılır ve genellikle bağımsız değişkeni olarak kullanılır `StaticResource` işaretleme uzantısı.|
|`x:Name`|XAML öğesi için bir çalışma zamanı nesne adını belirtir. Ayar `x:Name` kod içinde bir değişken bildirmek için benzer.|
|`x:TypeArguments`|Oluşturucuya genel bir türün genel tür bağımsız değişkenleri belirtir.|

Hakkında daha fazla bilgi için `x:FieldModifier` özniteliği için bkz: [alan değiştiricileri](~/xamarin-forms/xaml/field-modifiers.md). Hakkında daha fazla bilgi için `x:Arguments`, `x:FactoryMethod`, ve `x:TypeArguments` öznitelikleri, [Passing Arguments XAML içinde](~/xamarin-forms/xaml/passing-arguments.md).

XAML içinde ad alanı bildirimi alt öğeye üst öğeden devralır. Bu nedenle, bir ad alanı bir XAML dosyasının kök öğesi tanımlarken, bu dosyanın içerdiği tüm öğeleri ad alanı bildirimi devralır.

## <a name="declaring-namespaces-for-types"></a>Türler için ad alanlarını bildirme

Türler ortak dil çalışma zamanı (CLR) ad alanı adı ve isteğe bağlı olarak bir derleme adı belirterek ad alanı bildirimi ile bir XAML ad alanı öneki, bildirerek XAML içinde başvurulabilir. Bu ad alanı bildirimi içinde aşağıdaki anahtar sözcükler için değerleri tanımlayarak sağlanır:

- **clr-namespace:** veya **kullanma:** – CLR ad alanı bildirimi derlemedeki XAML öğeleri olarak kullanıma sunmak için türler içerir. Bu anahtar gereklidir.
- **derleme =** – başvurulan CLR ad uzayı içeren derleme. Bu değer, dosya uzantısı olmadan derlemenin adıdır. Derleme yolu, bir derlemeye başvuran bir XAML dosyasını içeren proje dosyası içinde bir başvuru olarak kurulacaktır. Bu anahtar sözcüğü atlanabilir **clr-namespace** türlerine başvuran uygulama kodu ile aynı bütünleştirilmiş kod içinde değerdir.

Ayıran karakter unutmayın `clr-namespace` veya `using` ise, iki nokta değerini belirteç ayırıcı karakter `assembly` eşittir işareti değerini belirteci. İki belirteç arasında kullanılacak noktalı virgül karakteridir.

Aşağıdaki kod örneği bir XAML ad alanı bildirimi gösterilmektedir:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternatif olarak, bu olarak yazılabilir:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local` Ad alanı içindeki türleri uygulamaya yerel olduğunu belirtmek için kullanılan bir kuralı önekidir. Farklı bir derlemede türleriyse alternatif olarak, derleme adı da ad alanı bildiriminde aşağıdaki XAML kod örneğinde gösterildiği gibi tanımlanması gerekir:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Ad alanı öneki olarak içeri aktarılan bir ad alanındaki bir türün örneğini bildirme aşağıdaki XAML kod örneğinde gösterilen ardından belirtilir:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Özet

Bu makalede, XAML ad alanı sözdizimi sunulan ve bir tür erişmek için XAML ad alanı bildirmek nasıl gösterilmiştir. XAML kullanan `xmlns` ad alanı bildirimi ve türleri için XML özniteliği başvurulabilir XAML içinde bir XAML ad alanı öneki bildirerek.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML bağımsız değişkenleri geçirme](~/xamarin-forms/xaml/passing-arguments.md)
