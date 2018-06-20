---
title: Xamarin.Forms içinde XAML alan değiştiricileri
description: 'X: FieldModifier ad özniteliği adlandırılmış XAML öğeleri için oluşturulan alanlara erişim düzeyini belirtir.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209494"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Xamarin.Forms içinde XAML alan değiştiricileri

_`x:FieldModifier` Ad özniteliği, adlandırılmış XAML öğeleri için oluşturulan alanlara erişim düzeyini belirtir._

## <a name="overview"></a>Genel Bakış

Özniteliğin geçerli değerler şunlardır:

- `Public` – oluşturulan alanın XAML öğesi için olduğunu belirtir `public`.
- `NotPublic` – oluşturulan alanın XAML öğesi için olduğunu belirtir `internal` derleme.

Özniteliğin değerini ayarlanmamışsa, öğe için oluşturulan alan olacaktır `private`.

İçin aşağıdaki koşulların karşılanması gereken bir `x:FieldModifier` işlenecek özniteliği:

- En üst düzey XAML öğesi geçerli bir olmalıdır `x:Class`.
- Geçerli XAML öğeye sahip bir `x:Name` belirtilen.

Aşağıdaki XAML özniteliğini örnekleri gösterir:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier` Öznitelik, bir XAML sınıf erişim düzeyini belirtmek için kullanılamaz.
