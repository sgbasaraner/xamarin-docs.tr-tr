---
title: Animasyon
description: Xamarin.Forms da karmaşık animasyon oluşturmak için yönlü devam ederken basit bir animasyon oluşturmak için basittir kendi animasyon altyapısını içerir.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 7cff122e7ecc24f5ad93bd863ee422981871f857
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="animation"></a>Animasyon

_Xamarin.Forms da karmaşık animasyon oluşturmak için yönlü devam ederken basit bir animasyon oluşturmak için basittir kendi animasyon altyapısını içerir._

Xamarin.Forms animasyon sınıfları görsel öğelerin farklı özellikleri aşamalı bir özelliği bir değerden başka bir süre boyunca değiştirme tipik bir animasyon ile hedefleyin. Xamarin.Forms animasyon sınıfları için XAML arabirim olduğuna dikkat edin. Ancak, animasyon olarak saklanmasını [davranışları](~/xamarin-forms/app-fundamentals/behaviors/index.md) ve XAML başvurulabilir.

## <a name="simple-animationssimplemd"></a>[Basit Animasyonlar](simple.md)

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Sınıfı, döndürme, Ölçek, çevir ve silinerek basit animasyon oluşturmak için kullanılan genişletme yöntemleri sağlar [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) örnekleri. Bu makalede, oluşturma ve kullanma animasyonları iptal etme gösterilmektedir `ViewExtensions` sınıfı.

## <a name="easing-functionseasingmd"></a>[Hızlandırma İşlevleri](easing.md)

Xamarin.Forms içeren bir [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) nasıl animasyonları hızlandırma denetleyen aktarımı işlev belirtin veya çalıştırdıkları gibi yavaşlaması sağlayan sınıf. Bu makalede önceden tanımlanmış hareket hızı işlevlerini kullanma ve nasıl özel hareket hızı işlevler oluşturulacağı gösterilmektedir.

## <a name="custom-animationscustommd"></a>[Özel Animasyonlar](custom.md)

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Sınıf, yapı taşı genişletme yöntemleri ile tüm Xamarin.Forms animasyonların [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) bir veya daha fazla oluşturma sınıfı `Animation` nesneleri. Bu makalede nasıl kullanılacağı gösterilmektedir `Animation` oluşturmak ve animasyonları iptal, birden çok animasyon eşitlemek ve varolan animasyon yöntemlerle animasyonlu değildir özelliklerine animasyon özel animasyon oluşturmak için sınıfı.

