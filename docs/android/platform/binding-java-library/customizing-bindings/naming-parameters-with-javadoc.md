---
title: "Javadoc parametrelerle adlandırma"
description: "Bu makalede, Java projeden oluşturulan Javadoc kullanarak Java projesinde bağlama parametre adları kurtarmak açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: 84dfe88e912241eb0024143bca568ae75e5bfa28
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc parametrelerle adlandırma

_Bu makalede, Java projeden oluşturulan Javadoc kullanarak Java projesinde bağlama parametre adları kurtarmak açıklanmaktadır._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Varolan bir Java kitaplığını bağlanırken, ilişkili API hakkında bazı meta veriler kaybolur. Özellikle yöntemlerine parametrelerinin adları. Parametre adları olarak görünür `p0`, `p1`vb. Bunun nedeni, Java `.class` dosyaları Java kaynak kodunda kullanılan parametre adları korumak değil. 

Özgün kitaplığından Javadoc HTML erişim izni varsa bir Xamarin.Android Java bağlama proje parametre adları sağlayabilir. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Proje bağlama Java Javadoc HTML tümleştirme

Javadoc HTML bir Java bağlama projeye tümleştirme aşağıdaki adımlardan oluşan elle yapılan bir işlemdir: 

1.  Kitaplık için Javadoc indirin
2.  Düzen `.csproj` dosya ve ekleme bir `<JavaDocPaths>` özelliği:
3.  Temizleyin ve projeyi yeniden derleyin

Bunu yaptıktan sonra özgün Java parametre adları bir Java projesi bağlama tarafından bağlı API'leri mevcut olmalıdır. 


> [!NOTE]
> **Not:** JavaDoc çıkış varyans büyük bir bölümünü yoktur. . JAR bağlama araç zinciri, her tek olası sıralamaya desteklemez ve bu nedenle bazı parametre değil düzgün bir şekilde adlandırılmış olabilir.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede nasıl ele anlamı parametre adları ilişkili API'ler için sağlamak için bir Java projesi bağlama Javadoc kullanın. 

