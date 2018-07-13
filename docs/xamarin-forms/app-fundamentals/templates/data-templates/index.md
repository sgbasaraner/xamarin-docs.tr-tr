---
title: Xamarin.Forms veri şablonları
description: DataTemplate desteklenen denetimlere veri görünümünü belirtmek için kullanılır ve genellikle görüntülenecek verilere bağlar.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994977"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms veri şablonları

_DataTemplate desteklenen denetimlere veri görünümünü belirtmek için kullanılır ve genellikle görüntülenecek verilere bağlar._

## <a name="introductionintroductionmd"></a>[Giriş](introduction.md)

Xamarin.Forms veri şablonları desteklenen denetimlere verilerini sunumu tanımlama yeteneği sağlar. Bu makalede konakadı neden İnceleme veri şablonları, bir giriş sağlar.

## <a name="creating-a-datatemplatecreatingmd"></a>[Bir DataTemplate'ı oluşturma](creating.md)

Veri şablonları oluşturulabilir satır içi, bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), veya bir özel tür ya da uygun Xamarin.Forms hücresi türü. Veri şablonu başka bir yerde yeniden gerek yoksa, bir satır içi şablonu kullanılmalıdır. Alternatif olarak, bir veri şablonu bir denetim düzeyi, sayfa düzeyi veya uygulama düzeyinde kaynak veya özel bir tür olarak tanımlayarak yeniden kullanılabilir.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Bir DataTemplateSelector oluşturma](selector.md)

A [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) seçmek için kullanılan bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) verilere bağlı özellik değerine göre çalışma zamanında. Birden çok böylece `DataTemplate` örnekleri aynı belirli nesnelerin görünümünü özelleştirmek için nesne türüne uygulanacak. Bu makale oluşturma ve tüketme yapmayı gösteren bir `DataTemplateSelector`.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri şablonları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
