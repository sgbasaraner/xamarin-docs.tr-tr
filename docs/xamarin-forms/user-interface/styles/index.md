---
title: Stilleri
description: Stilleri görünümünü özelleştirmek için kullanma
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7a19f7597ee17282bc8b41e7f0e7e3ade2361a50
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="styles"></a>Stilleri

## <a name="introductionintroductionmd"></a>[Giriş](introduction.md)

Xamarin.Forms uygulamalar genellikle özdeş bir görünüme sahip birden çok denetim içerir. Her denetim görünümünü ayarlama yinelenen olabilir ve hataya. Bunun yerine, stiller, denetimin görünümünü özelleştirme gruplandırma ve denetim türünde kullanılabilen ayarları özellikler tarafından oluşturulabilir.

## <a name="explicit-stylesexplicitmd"></a>[Açık Stiller](explicit.md)

Bir *açık* stili denetimleri ayarlayarak seçmeli olarak uygulanan bir olduğundan kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özellikleri.

## <a name="implicit-stylesimplicitmd"></a>[Örtük Stiller](implicit.md)

Bir *örtük* stili aynı tüm denetimleri tarafından kullanılan bir olduğundan [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), stil başvurmak için her denetim gerektirmeden.

## <a name="global-stylesapplicationmd"></a>[Genel Stiller](application.md)

Stilleri kullanılabilir hale getirebilir genel olarak uygulamanın ekleyerek [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Bu sayfalar veya denetimleri boyunca stilleri yinelenmesini önlemek için yardımcı olur.

## <a name="style-inheritanceinheritancemd"></a>[Stil Devralımı](inheritance.md)

Stilleri çoğaltma azaltın ve yeniden etkinleştirmek için diğer stilleri devralabilirsiniz.

## <a name="dynamic-stylesdynamicmd"></a>[Dinamik Stiller](dynamic.md)

Stilleri değil özelliği değişikliklere yanıt verir ve bir uygulama süresi değişmeden kalır. Ancak, uygulamalar çalışma zamanında dinamik olarak stil değişiklikleri dinamik kaynaklar kullanarak yanıt verebilir.

## <a name="device-stylesdevicemd"></a>[Cihaz Stilleri](device.md)

Xamarin.Forms içeren altı *dinamik* olarak bilinen stilleri *aygıt* stiller, buna [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) sınıfı. Tüm altı stilleri uygulanabilir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) yalnızca örnekleri.
