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
ms.openlocfilehash: b0afba90dab5cba4bad385f8d6447d8b83c1de3d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
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

<table>
 <thead>
   <tr>
     <td><strong>Construct</strong></td>
     <td><strong>Açıklama</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><code>x:Arguments</code></td>
     <td>Varsayılan olmayan bir oluşturucu ya da bir fabrika yöntemi nesne bildirimi oluşturucu bağımsız değişkenlerini belirtir.</td>
   </tr>
   <tr>
     <td><code>x:Class</code></td>
     <td>XAML içinde tanımlanan bir sınıf için ad alanı ve sınıf adını belirtir. Sınıf adı arka plan kod dosyasının sınıf adı eşleşmelidir. Bu yapıyı bir XAML dosyasının kök öğesinin yalnızca görünebileceğini unutmayın.</td>
   </tr>
   <tr>
     <td><code>x:FactoryMethod</code></td>
     <td>Nesneyi başlatmak için kullanılan Üreteç yöntemi belirtir.</td>
   </tr>
   <tr>
     <td><code>x:Key</code></td>
     <td>Her kaynak için benzersiz bir kullanıcı tanımlı anahtar belirten bir <code>ResourceDictionary</code>. Anahtarın değerini XAML kaynağı almak için kullanılır ve genellikle için bağımsız değişken olarak kullanılan <code>StaticResource</code> biçimlendirme uzantısı.</td>
   </tr>
   <tr>
     <td><code>x:Name</code></td>
     <td>XAML öğesi için bir çalışma zamanı nesne adı belirtir. Ayarı <code>x:Name</code> kod bir değişkende bildirmek için benzer.</td>
   </tr>
   <tr>
     <td><code>x:TypeArguments</code></td>
     <td>Genel tür bağımsız değişkenleri genel bir tür oluşturucuya belirtir.</td>
   </tr>
 </tbody>
</table>

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
