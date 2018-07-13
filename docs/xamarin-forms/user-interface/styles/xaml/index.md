---
title: XAML stilleri kullanarak Xamarin.Forms uygulamalarında stil oluşturma
description: Bu kılavuz, XAML stilleri kullanarak Xamarin.Forms uygulaması özelleştirme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 5145572b30302e58c36250fff40e8b637fcd221f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995081"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>XAML stilleri kullanarak Xamarin.Forms uygulamalarında stil oluşturma

## <a name="introductionintroductionmd"></a>[Giriş](introduction.md)

Xamarin.Forms uygulamaları genellikle özdeş bir görünüme sahip birden fazla denetim içerir. Tek tek her denetimin görünümünü ayarı yinelenen olabilir ve hataya açık alanlardır. Bunun yerine, stiller, denetimin görünümünü özelleştirme gruplandırma ve denetim türünde kullanılabilir ayarları özellikleri tarafından oluşturulabilir.

## <a name="explicit-stylesexplicitmd"></a>[Açık Stiller](explicit.md)

Bir *açık* stili denetimleri ayarlayarak seçmeli olarak uygulanan bir, kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri.

## <a name="implicit-stylesimplicitmd"></a>[Örtük Stiller](implicit.md)

Bir *örtük* stili, aynı tüm denetimler tarafından kullanılan bir [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), stili başvurmak için her denetim gerektirmeden.

## <a name="global-stylesapplicationmd"></a>[Genel Stiller](application.md)

Stilleri kullanılabilir hale genel olarak uygulamanın ekleyerek [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Bu çoğaltma stil sayfaları veya denetimler arasında kaçınmaya yardımcı olur.

## <a name="style-inheritanceinheritancemd"></a>[Stil Devralımı](inheritance.md)

Stilleri azaltmak ve yeniden etkinleştirmek için diğer stilleri devralabilir.

## <a name="dynamic-stylesdynamicmd"></a>[Dinamik Stiller](dynamic.md)

Stilleri değil özellik değişikliklerine yanıt ve bir uygulama süresi değişmeden kalır. Ancak, uygulamalar çalışma zamanında dinamik olarak stil değişikliklerini dinamik kaynakları kullanarak yanıt verebilirsiniz.

## <a name="device-stylesdevicemd"></a>[Cihaz Stilleri](device.md)

Xamarin.Forms içeren altı *dinamik* olarak bilinen stilleri *cihaz* stiller, buna [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) sınıfı. Tüm altı stilleri uygulanabilir [ `Label` ](xref:Xamarin.Forms.Label) yalnızca örnekler.
