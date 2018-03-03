---
title: "Veri şablonları"
description: "Bir DataTemplate desteklenen denetimlere verilerin görünümünü belirtmek için kullanılır ve genellikle görüntülenecek verileri bağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 99a00c98471ae85af2a8cba2e1e52444370a9332
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="data-templates"></a>Veri şablonları

_Bir DataTemplate desteklenen denetimlere verilerin görünümünü belirtmek için kullanılır ve genellikle görüntülenecek verileri bağlar._

## <a name="introductionintroductionmd"></a>[Giriş](introduction.md)

Xamarin.Forms veri şablonları, desteklenen denetimlere veri sunumu tanımlama yeteneği sağlar. Bu makalede veri şablonları, gerekli neden inceleniyor tanıtılmaktadır.

## <a name="creating-a-datatemplatecreatingmd"></a>[Bir DataTemplate oluşturma](creating.md)

Veri şablonları oluşturulabilir, satır içi bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), veya bir özel tür ya da uygun Xamarin.Forms hücre türü. Başka bir yerde veri şablonu yeniden gerekmiyorsa bir satır içi şablon kullanılmalıdır. Alternatif olarak, veri şablonu özel bir tür olarak ya da denetim düzeyi sayfa düzeyinde veya uygulama düzeyinde kaynak olarak tanımlayarak yeniden kullanılabilir.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Bir DataTemplateSelector oluşturma](selector.md)

A [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) seçmek için kullanılan bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) veri bağlama özelliğinin değeri temel alınarak çalışma zamanında. Bu, birden çok sağlar `DataTemplate` nesnesinin belirli nesnelerin görünümünü özelleştirmek için aynı türü için uygulanacak örnekleri. Bu makalede oluşturmak ve kullanmak nasıl gösteren bir `DataTemplateSelector`.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri şablonları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
