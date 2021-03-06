---
title: Xamarin'daki dil desteği programlama
description: 'Bu belgede Xamarin tarafından desteklenen çeşitli programlama dillerini açıklanmaktadır. C#, F #, taşınabilir Visual Basic.NET ve Razor şablonları açıklanır.'
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 715f63a0be54ba3342bd63c1c76d89656313359a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781682"
---
# <a name="programming-language-support-in-xamarin"></a>Xamarin'daki dil desteği programlama

## <a name="c"></a>C# 

###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Zaman Uyumsuz Desteğe Genel Bakış](~/cross-platform/platform/async.md)

Zaman uyumsuz işlemleri ifade etmek için iki yeni anahtar sözcüklerle sürüm 5 C# sunulan: async ve await. Bu anahtar sözcükler, başka bir iş parçacığında (örneğin, ağ erişimi) uzun süre çalışan işlemlerini yürütmek için görev paralel kitaplığı kullanan basit kod yazmak ve kolayca tamamlama sonuçlarına erişme olanak tanır. Xamarin.iOS ve Xamarin.Android en son sürümlerini zaman uyumsuz desteği ve await - bu belge, açıklamalar ve Xamarin ile yeni sözdizimini kullanarak bir örnek sağlar.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[C# 6 Dil Özellikleri](~/cross-platform/platform/csharp-six.md)

C# dili – sürüm 6 – en son sürümünü daha az Demirbaş, geliştirilmiş daha anlaşılır olması ve daha fazla tutarlılık sağlamak için dil gelişmeye devam eder. Temizleyici başlatma sözdizimi, kullanabilme `await` içinde `catch/finally` blokları ve null-conditional `?` işleci özellikle kullanışlıdır.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

F # ve Xamarin olan mobil uygulama oluşturma.

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Taşınabilir Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio, Xamarin uygulamaları birleştirilebilir Visual.NET kullanarak taşınabilir sınıf kitaplıkları oluşturulmasını destekler. Bu makalede, yeni bir Visual Basic PCL oluşturmak ve bir örnek Xamarin.iOS, Xamarin.Android ve Windows Phone Uygulama kullanmak gösterilmektedir.

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Razor şablonları kullanarak yapı HTML görünümleri](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin, geliştiricilerin birlikte kolayca verileri HTML, Javascript ve CSS ile el ile HTML dizelerini kod oluşturmanın uğraşmadan birleştirmek için C# ASP.NET MVC ile ilk olarak sunulan Razor şablon motoru yararlanmanızı sağlar.
Bu makalede Razor şablonların Xamarin ile Android ve iOS için nasıl kullanılacağı gösterilmektedir.
