---
title: Gelişmiş kavramlar ve dahili bileşenleri
description: Temel alınan mimarisinde Xamarin.Android ve onun API tasarım arkasında.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/21/2018
ms.openlocfilehash: 79e61db4c27a2d29b4ee0a9d39f2d25ea5d93303
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2018
ms.locfileid: "34458360"
---
# <a name="advanced-concepts-and-internals"></a>Gelişmiş kavramlar ve dahili bileşenleri

_Bu bölüm mimari, API tasarım ve Xamarin.Android sınırlamaları açıklayan konuları içerir. Buna ek olarak, atık toplama uygulaması ve Xamarin.Android içinde kullanılabilir derlemeleri açıklayan konuları içerir. Xamarin.Android olduğundan [açık kaynak](https://github.com/xamarin/xamarin-android), kendi kaynak kodunu inceleyerek çalışmalar Xamarin.Android anlamak mümkündür._


##  <a name="architectureandroidinternalsarchitecturemd"></a>[Mimari](~/android/internals/architecture.md)

Bu makalede, bir Xamarin.Android uygulaması arkasındaki temel alınan mimarisinde açıklanmaktadır. Xamarin.Android uygulamaları Android çalışma zamanı sanal makine bir Mono yürütme ortamı içindeki çalışma şeklini açıklar ve Android aranabilir sarmalayıcılar ve yönetilen aranabilir sarmalayıcılar olarak gibi temel kavramları açıklar. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API Tasarımı](~/android/internals/api-design.md)

Mono parçası olan bir temel sınıf kitaplıkları çekirdek yanı sıra Xamarin.Android çeşitli Android Mono ile yerel Android uygulamaları oluşturmak geliştiriciler izin vermek API'ler için bağlamaları ile birlikte gelir.

Çekirdeği Xamarin.Android vardır birlikte çalışma altyapısı bu Java world köprüleri C# dünyayla ve C# veya diğer .NET dilleri ile erişim geliştiriciler Java API sağlar.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Bütünleştirilmiş kodlar](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android birkaç derlemeleri ile birlikte gelir. Yalnızca Silverlight Masaüstü .NET derlemelerini genişletilmiş bir alt kümesidir gibi Xamarin.Android ayrıca çeşitli Silverlight ve Masaüstü .NET derlemelerini genişletilmiş bir alt kümesidir. 

