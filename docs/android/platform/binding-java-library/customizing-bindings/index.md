---
title: Bağlamaları özelleştirme
description: Bağlama işlemi denetimleri meta verileri düzenleyerek, bir Xamarin.Android bağlama özelleştirebilirsiniz. El ile yapılan bu değişiklikleri genellikle derleme hataları gidermek ve C# ile daha tutarlı bir elde edilen API şekillendirme için gerekli olan / .NET. Bu kılavuzlar bu meta veri yapısı, meta verileri değiştirme ve JavaDoc yöntemi parametrelerinin adları kurtarmak için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/25/2017
ms.openlocfilehash: bb4f3b24be2072cb8b33893899a23951ace63607
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-bindings"></a>Bağlamaları özelleştirme

_Bağlama işlemi denetimleri meta verileri düzenleyerek, bir Xamarin.Android bağlama özelleştirebilirsiniz. El ile yapılan bu değişiklikleri genellikle derleme hataları gidermek ve C# ile daha tutarlı bir elde edilen API şekillendirme için gerekli olan / .NET. Bu kılavuzlar bu meta veri yapısı, meta verileri değiştirme ve JavaDoc yöntemi parametrelerinin adları kurtarmak için nasıl kullanılacağı açıklanmaktadır._


## <a name="overview"></a>Genel Bakış
 
Xamarin.Android bağlama işlemi çoğunu otomatikleştirir; Ancak, bazı durumlarda, el ile değiştirilmesi aşağıdaki sorunları gidermek için gereklidir:

-   Derleme türleri, karıştırılmış türleri, yinelenen adları, sınıf görünürlük sorunları ve çözümlenemiyor diğer durumlarda eksik kaynaklanan hataları çözümleme Xamarin.Android araçları tarafından. 

-   Xamarin.Android farklı C# Android API bağlamak için kullandığı eşleme değiştirme (örneğin, geliştiricilerin çoğu Java eşlemek tercih ettiğiniz `int` sabitleri C# `enum` sabitleri).

-   Bağlanması gerekmez kullanılmayan türleri kaldırılıyor. 

-   Hiçbir karşılık gelen türlerine temel alınan Java API ekleniyor. 

Bağlama işlemi denetimleri meta verileri değiştirerek bazılarını veya tümünü bu değişiklikleri yapabilirsiniz.


## <a name="guides"></a>Kılavuzlar

Aşağıdaki Kılavuzlar bağlama işlemi denetimleri meta verileri tanımlamak ve bu sorunları gidermek için bu meta verileri değiştirmeye açıklanmaktadır:

-   [Java bağlamaları meta veri](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bir Java bağlama içine giden meta veri genel bir bakış sağlar.
    Bazen bir Java bağlama kitaplığı tamamlamak için gereken çeşitli el ile yapılacak adımlar açıklanır ve daha yakından .NET tasarım yönergeleri izlemek için bir bağlama tarafından kullanıma sunulan bir API şekil olunacağı açıklanmaktadır.

-   [Javadoc parametrelerle adlandırma](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) nasıl ilişkili Java projeden üretilen Javadoc kullanarak bir Java projesi bağlama parametresi adları kurtarılacağını açıklar.


 

