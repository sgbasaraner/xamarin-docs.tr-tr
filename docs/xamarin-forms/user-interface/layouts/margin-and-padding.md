---
title: Kenar boşlukları ve doldurma
description: Bir öğenin kullanıcı arabiriminde işlendiğinde kenar boşluğu bırakma ve doldurma özellikleri Düzen davranışları denetler. Bu makalede iki özelliklerini ve bunların nasıl ayarlanacağını arasındaki farkı gösterir.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 595e673c59d23a45cbaf923a0d58faff2000c296
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996622"
---
# <a name="margin-and-padding"></a>Kenar boşlukları ve doldurma

_Bir öğenin kullanıcı arabiriminde işlendiğinde kenar boşluğu bırakma ve doldurma özellikleri Düzen davranışları denetler. Bu makalede iki özelliklerini ve bunların nasıl ayarlanacağını arasındaki farkı gösterir._

## <a name="overview"></a>Genel Bakış

Kenar boşlukları ve doldurma, ilgili Düzen kavramlardır.

- [ `Margin` ](xref:Xamarin.Forms.View.Margin) Özelliği bir öğe ile komşu öğeleri arasındaki uzaklığı gösteren ve öğenin işleme konumunu ve komşuları işleme konumunu denetlemek için kullanılır. `Margin` değer belirtilebilir [Düzen](~/xamarin-forms/user-interface/controls/layouts.md) ve [görünümü](~/xamarin-forms/user-interface/controls/views.md) sınıfları.
- [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Özelliği öğenin alt öğeleri arasındaki uzaklığı gösteren ve kendi içeriğinden denetim ayırmak için kullanılır. `Padding` değer belirtilebilir [Düzen](~/xamarin-forms/user-interface/controls/layouts.md) sınıfları.

Aşağıdaki diyagramda iki kavramları göstermektedir:

[![](margin-and-padding-images/margins-and-padding-sml.png "Kenar boşlukları ve doldurma kavramları")](margin-and-padding-images/margins-and-padding.png#lightbox "kenar boşlukları ve doldurma kavramları")

Unutmayın [ `Margin` ](xref:Xamarin.Forms.View.Margin) değerler eklenebilir. Bu nedenle, iki bitişik öğeyi 20px kenar boşluğu belirtirseniz, öğeler arasındaki uzaklık 40 piksel olacaktır. Ayrıca, kenar boşlukları ve doldurma olan öğenin ve tüm içeriği arasındaki uzaklığı kenar boşluğu artı doldurma olacaktır, her ikisi de, ne zaman uygulanacağını eklenebilir.

## <a name="specifying-a-thickness"></a>Bir kalınlık belirtme

[ `Margin` ](xref:Xamarin.Forms.View.Margin) Ve [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) özellikler: her iki tür [ `Thickness` ](xref:Xamarin.Forms.Thickness). Üç olasılık vardır oluştururken bir `Thickness` yapısı:

- Oluşturma bir [ `Thickness` ](xref:Xamarin.Forms.Thickness) Tekdüzen tek bir değer tarafından tanımlanan yapısı. Tek değer, sol, üst, sağ ve öğenin alt kenarı uygulanır.
- Oluşturma bir [ `Thickness` ](xref:Xamarin.Forms.Thickness) yatay ve dikey değerleri tarafından tanımlanan yapısı. Yatay değeri, simetrik öğe üst ve alt tarafına uygulanan dikey değeri ile sol ve sağ tarafında öğesi simetrik olarak uygulanır.
- Oluşturma bir [ `Thickness` ](xref:Xamarin.Forms.Thickness) sol, üst, sağ ve öğenin alt tarafına uygulanan dört farklı değer tarafından tanımlanan yapısı.

Aşağıdaki XAML kod örneği, tüm üç olasılıklar gösterilmektedir:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

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
> `Thickness` değerler, genellikle kırpar veya içerik overdraws negatif olabilir.

## <a name="summary"></a>Özet

Bu makalede gösterilen arasındaki farkı [ `Margin` ](xref:Xamarin.Forms.View.Margin) ve [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) özelliklerini ve bunların nasıl ayarlanacağını. Bir öğenin kullanıcı arabiriminde işlendiğinde özellikleri düzen davranışını denetler.


## <a name="related-links"></a>İlgili bağlantılar

- [Kenar boşluğu](xref:Xamarin.Forms.View.Margin)
- [Doldurma](xref:Xamarin.Forms.Layout.Padding)
- [Kalınlığı](xref:Xamarin.Forms.Thickness)
