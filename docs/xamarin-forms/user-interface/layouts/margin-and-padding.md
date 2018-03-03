---
title: "Kenar boşluğu bırakma ve doldurma"
description: "Bir öğenin kullanıcı arabiriminde işlendiğinde kenar boşlukları ve doldurma özellikleri düzen davranışını denetler. Bu makalede, iki özellik ve bunları nasıl ayarlayacağınız arasındaki fark gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 7bab512ef11f8e0f553a00f0240d82f860fe2676
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="margin-and-padding"></a>Kenar boşluğu bırakma ve doldurma

_Bir öğenin kullanıcı arabiriminde işlendiğinde kenar boşlukları ve doldurma özellikleri düzen davranışını denetler. Bu makalede, iki özellik ve bunları nasıl ayarlayacağınız arasındaki fark gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Kenar boşluğu bırakma ve doldurma ilgili düzeni kavramları şunlardır:

- [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Özelliği bir öğe bitişik öğeleri arasındaki mesafeyi temsil eder ve öğenin işleme konumunu ve komşuları işleme konumunu denetlemek için kullanılır. `Margin` değer belirtilebilir [düzeni](~/xamarin-forms/user-interface/controls/layouts.md) ve [Görünüm](~/xamarin-forms/user-interface/controls/views.md) sınıfları.
- [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Özellik bir öğeyi ve alt öğeleri arasındaki mesafeyi temsil eder ve denetimi kendi içeriğinden ayırmak için kullanılır. `Padding` değer belirtilebilir [düzeni](~/xamarin-forms/user-interface/controls/layouts.md) sınıfları.

Aşağıdaki diyagramda iki kavramları göstermektedir:

[![](margin-and-padding-images/margins-and-padding-sml.png "Kenar boşlukları ve doldurma kavramları")](margin-and-padding-images/margins-and-padding.png "kenar boşlukları ve doldurma kavramları")

Unutmayın [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) değerler eklenebilir. Bu nedenle, iki bitişik öğeleri 20 piksel kenar boşluğu belirtirseniz, öğeleri arasındaki uzaklığı 40 piksel olacaktır. Ayrıca, kenar boşluğu bırakma ve doldurma olan bir öğeyi ve herhangi bir içerik arasındaki uzaklığı kenar boşluğu artı doldurma olacaktır, her ikisi de, ne zaman uygulanacağını ADDITIVE.

## <a name="specifying-a-thickness"></a>Bir kalınlığı belirtme

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Ve [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) özelliklerdir türü her ikisi de [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/). Üç olasılık vardır oluştururken bir `Thickness` yapısı:

- Oluşturma bir [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) tek bir Tekdüzen değer tarafından tanımlanan yapısı. Tek değer sol, üst, sağ ve öğenin alt kenarı uygulanır.
- Oluşturma bir [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) yatay ve dikey değerleri tarafından tanımlanan yapısı. Yatay değeri, simetrik öğenin üst ve alt tarafına uygulanan dikey değerle ve öğenin sol tarafında için simetrik olarak uygulanır.
- Oluşturma bir [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) sol, üst, sağ ve öğenin alt tarafına uygulanan dört farklı değerleri tarafından tanımlanan yapısı.

Aşağıdaki XAML kod örneği, tüm üç olasılıklarını gösterir:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> **Not**: `Thickness` genellikle kırpar veya içeriği overdraws değerleri negatif olabilir.

## <a name="summary"></a>Özet

Bu makalede gösterilen arasındaki farkı [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) ve [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) özellikleri ve bunları nasıl ayarlayacağınız. Bir öğenin kullanıcı arabiriminde işlendiğinde özellikleri düzen davranışını denetler.


## <a name="related-links"></a>İlgili bağlantılar

- [Kenar boşluğu](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [Doldurma](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [Kalınlığı](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
