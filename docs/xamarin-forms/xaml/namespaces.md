---
title: "XAML Ad Uzayları"
description: "XAML ad alanı bildirimleri için xmlns XML özniteliğini kullanır. Bu makalede XAML ad uzayı söz dizimi tanıtır ve bir tür erişmek için XAML ad uzayı bildirmek gösterilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2017
ms.openlocfilehash: 55b83151e9c345096aeb0bfdd686d50c5fde62fd
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="xaml-namespaces"></a>XAML Ad Uzayları

_XAML ad alanı bildirimleri için xmlns XML özniteliğini kullanır. Bu makalede XAML ad uzayı söz dizimi tanıtır ve bir tür erişmek için XAML ad uzayı bildirmek gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Her zaman bir XAML dosyasının kök öğesinin içinde olan iki XAML ad uzayları bildirimleri vardır. İlk aşağıdaki XAML kod örneğinde gösterildiği gibi varsayılan ad alanını tanımlar:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Önek ile XAML dosyası içinde tanımlanan öğeleri Xamarin.Forms sınıflarına gibi başvuran varsayılan ad alanını belirtir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

İkinci ad alanı bildirimini kullanır `x` aşağıdaki XAML kod örneğinde gösterildiği gibi öneki:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML önekleri varsayılan olmayan ad alanlarını, türleri ad alanı içindeki başvururken kullanılan önekiyle bildirmek için kullanır. `x` Ad alanı bildiriminin belirtir öğeleri öneki olan XAML içinde tanımlanan `x` öğeleri ve XAML (özellikle 2009 XAML belirtimi) iç öznitelikleri için kullanılır.

Aşağıdaki tabloda ana hatlarını `x` Xamarin.Forms tarafından desteklenen ad öznitelikleri:

|Oluştur|Açıklama|
|--- |--- |
|`x:Arguments`|Varsayılan olmayan bir oluşturucu ya da bir fabrika yöntemi nesne bildirimi oluşturucu bağımsız değişkenlerini belirtir.|
|`x:Class`|XAML içinde tanımlanan bir sınıf için ad alanı ve sınıf adını belirtir. Sınıf adı arka plan kod dosyasının sınıf adı eşleşmelidir. Bu yapıyı bir XAML dosyasının kök öğesinin yalnızca görünebileceğini unutmayın.|
|`x:FactoryMethod`|Nesneyi başlatmak için kullanılan Üreteç yöntemi belirtir.|
|`x:Key`|Her kaynak için benzersiz bir kullanıcı tanımlı anahtar belirten bir `ResourceDictionary`. Anahtarın değerini XAML kaynağı almak için kullanılır ve genellikle için bağımsız değişken olarak kullanılan `StaticResource` biçimlendirme uzantısı.|
|`x:Name`|XAML öğesi için bir çalışma zamanı nesne adı belirtir. Ayarı `x:Name` kod bir değişkende bildirmek için benzer.|
|`x:TypeArguments`|Genel tür bağımsız değişkenleri genel bir tür oluşturucuya belirtir.|

Hakkında daha fazla bilgi için `x:Arguments`, `x:FactoryMethod`, ve `x:TypeArguments` öznitelikleri, [bağımsız değişkenleri geçirme XAML'de](~/xamarin-forms/xaml/passing-arguments.md).

XAML ad alanı bildirimleri alt öğesi üst öğeden devralır. Bu nedenle, bir ad alanı bir XAML dosyasının kök öğesinin tanımlarken, bu dosyanın içindeki tüm öğeler ad alanı bildirimi devralır.

## <a name="declaring-namespaces-for-types"></a>Türleri için ad alanlarını bildirme

Türler ve ad alanı bildiriminin ortak dil çalışma zamanı (CLR) ad alanı adı ve isteğe bağlı olarak bir derleme adı belirterek bir önek ile XAML ad uzayı bildirerek XAML'de başvurulabilir. Bu ad alanı bildirimi içinde aşağıdaki anahtar değerlerini tanımlayarak sağlanır:

- **clr-namespace:** veya **kullanarak:** – CLR ad bildirilen derlemedeki XAML öğeleri kullanıma sunmak için türleri içerir. Bu anahtar gereklidir.
- **derleme =** – başvurulan CLR ad içeren derleme. Dosya adı uzantısı hariç derlemenin adını değerdir. Derleme yolu derlemeye başvurur XAML dosyasını içeren proje dosyasında bir başvuru olarak kurulmalıdır. Bu anahtar sözcük atlanabilir **clr-namespace** değerdir türlerine başvurma uygulama kodu aynı bütünleştirilmiş içinde.

Ayıran karakter unutmayın `clr-namespace` veya `using` ise, iki nokta değerinden belirteci ayıran karakter `assembly` eşittir işareti değerinden belirteci. İki belirteç arasında kullanmak için noktalı virgül karakteridir.

Aşağıdaki kod örneğinde XAML ad alanı bildirimi gösterir:

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

`local` Ad alanı içinde türlerden uygulamaya yerel olduğunu belirtmek için kullanılan bir kuralı önekidir. Farklı bir derlemede tipi varsa, alternatif olarak, derleme adı da ad alanı bildirimi aşağıdaki XAML kod örneğinde gösterildiği şekilde tanımlanması gerekir:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

İçeri aktarılan bir ad alanından türünün bir örneği olarak bildirme aşağıdaki XAML kod örneğinde gösterildiği olduğunda ad alanı öneki sonra belirtilmiştir:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Özet

Bu makalede, XAML ad uzayı söz dizimi sunulan ve bir tür erişmek için XAML ad uzayı bildirmeyi gösterilmektedir. XAML kullanan `xmlns` ad alanı bildirimler ve türler için XML özniteliği başvurulabilir XAML'de XAML ad alanı öneki bildirerek.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML'de bağımsız değişkenleri geçirme](~/xamarin-forms/xaml/passing-arguments.md)
