---
title: Xamarin.Forms animasyonu
description: Xamarin.Forms de karmaşık animasyon oluşturmak için verimli olmanın yanı sıra, basit animasyon oluşturmak için basit kendi animasyon altyapısını içerir.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998312"
---
# <a name="animation-in-xamarinforms"></a>Xamarin.Forms animasyonu

_Xamarin.Forms de karmaşık animasyon oluşturmak için verimli olmanın yanı sıra, basit animasyon oluşturmak için basit kendi animasyon altyapısını içerir._

Xamarin.Forms animasyonu sınıfları, aşamalı bir özelliği bir değerden başka bir süre değiştirme tipik bir animasyon ile görsel öğelerin farklı özellikleri hedefleyin. Xamarin.Forms animasyonu sınıfları için XAML arabirim olduğuna dikkat edin. Ancak, animasyonları içinde saklanmasını [davranışları](~/xamarin-forms/app-fundamentals/behaviors/index.md) ve ardından XAML başvurulan.

## <a name="simple-animationssimplemd"></a>[Basit Animasyonlar](simple.md)

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Sınıfı, döndürme, ölçeklendirme, çevirme ve Soldurma basit animasyon oluşturmak için kullanılan genişletme yöntemleri sağlar [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) örnekleri. Bu makalede, oluşturma ve animasyonları kullanarak iptal etme işlemlerini gösterir `ViewExtensions` sınıfı.

## <a name="easing-functionseasingmd"></a>[Hızlandırma İşlevleri](easing.md)

Xamarin.Forms içeren bir [ `Easing` ](xref:Xamarin.Forms.Easing) nasıl animasyonları hızlandırma denetleyen bir aktarım işlevi belirtin veya gibi çalıştırdıkları yavaşlamasına sağlar sınıfını. Bu makalede, önceden tanımlanmış kolaylaştırıcı işlevler kullanma ve özel kolaylaştırıcı işlevler oluşturma işlemini gösterir.

## <a name="custom-animationscustommd"></a>[Özel Animasyonlar](custom.md)

[ `Animation` ](xref:Xamarin.Forms.Animation) Sınıf, yapı taşı alanında uzantı yöntemlerini içeren tüm Xamarin.Forms animasyon [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı bir veya daha fazla oluşturma `Animation` nesneleri. Bu makalede nasıl yapılacağı açıklanır `Animation` oluşturmak ve animasyonları iptal, birden çok animasyonlarına eşitleyebilir ve mevcut animasyon yöntemlerle animasyonlu olmayan özelliklerine animasyon özel animasyon oluşturmak için sınıf.
