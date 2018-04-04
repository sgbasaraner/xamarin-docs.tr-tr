---
title: Gelişmiş kavramlar ve dahili bileşenleri
description: Temel alınan mimarisinde Xamarin.Android ve onun API tasarım arkasında.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/13/2017
ms.openlocfilehash: 517e21f2decd0dabbd03d752f13831a891ad7138
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="advanced-concepts-and-internals"></a>Gelişmiş kavramlar ve dahili bileşenleri


##  <a name="architectureandroidinternalsarchitecturemd"></a>[Mimari](~/android/internals/architecture.md)

Bu makalede, bir Xamarin.Android uygulaması arkasındaki temel alınan mimarisinde açıklanmaktadır. Xamarin.Android uygulamaları Android çalışma zamanı sanal makine bir Mono yürütme ortamı içindeki çalışma şeklini açıklar ve Android aranabilir sarmalayıcılar ve yönetilen aranabilir sarmalayıcılar olarak gibi temel kavramları açıklar. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API Tasarımı](~/android/internals/api-design.md)

Mono parçası olan bir temel sınıf kitaplıkları çekirdek yanı sıra Xamarin.Android çeşitli Android Mono ile yerel Android uygulamaları oluşturmak geliştiriciler izin vermek API'ler için bağlamaları ile birlikte gelir.

Çekirdeği Xamarin.Android vardır birlikte çalışma altyapısı bu Java world köprüleri C# dünyayla ve C# veya diğer .NET dilleri ile erişim geliştiriciler Java API sağlar.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Bütünleştirilmiş kodlar](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android birkaç derlemeleri ile birlikte gelir. Yalnızca Silverlight Masaüstü .NET derlemelerini genişletilmiş bir alt kümesidir gibi Xamarin.Android ayrıca çeşitli Silverlight ve Masaüstü .NET derlemelerini genişletilmiş bir alt kümesidir. 

