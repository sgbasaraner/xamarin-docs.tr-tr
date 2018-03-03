---
title: "Java için C# bağlantı noktası oluşturma"
description: "Java kullanarak bir Xamarin.Android uygulaması için bir üçüncü Java kaynak koduna C# bağlantı noktasına seçeneğidir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2016
ms.openlocfilehash: bf881861eec0b28e59704253c3dab3f4e5dbae46
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="porting-java-to-c"></a>Java için C# bağlantı noktası oluşturma

_Java kullanarak bir Xamarin.Android uygulaması için bir üçüncü Java kaynak koduna C# bağlantı noktasına seçeneğidir._

## <a name="overview"></a>Genel Bakış

Bu yaklaşım, kuruluşlara ilgi olabilir:

-  **Teknoloji yığınları Java'dan C# geçiş.**
-  **C# ve aynı ürünün Java sürümünü bulundurmanız gerekir.**
-  **Popüler bir Java kitaplığı .NET sürümüne sahip istiyor.**


C# Java kod bağlantı noktası için iki yolu vardır. İlk kod el ile bağlantı noktasına yoludur. Bu, .NET ve Java anlamak ve her dil için uygun deyimleri bilginiz becerikli geliştiriciler içerir. Bu yaklaşım kod küçük miktarda veya tamamen çıktığınızda Java C# taşımak istediğiniz kuruluşlar için en anlamlı.

İkinci taşıma yöntemleri deneyin ve bir kod dönüştürücü gibi kullanarak işlemini otomatikleştirmek için olan [keskinleştirme](https://github.com/mono/sharpen). [Keskinleştirme](https://github.com/mono/sharpen) için kod bağlantı noktası için ilk olarak kullanılan Versant gelen açık kaynak dönüştürücü *db4o* C# Java'dan. db4o Versant Java'da geliştirilen ve .NET için bağlantı noktası kurulmuş bir nesne yönelimli veritabanıdır. Bir kod dönüştürücü kullanarak her iki dilde bulunmalı ve ikisi arasındaki bazı eşlik gerektiren projeleri için mantıklı olabilir.

Ne zaman bir otomatik kod dönüştürme aracı mantıklı bir örnek görülebilir [ngit](https://github.com/mono/ngit) projesi.
Ngit olan bir bağlantı noktası Java projesinin [jgit](http://eclipse.org/).
Jgit kendisini olan bir Java uygulaması [Git](http://git-scm.com/) kaynak kodu yönetim sistemi. Java'dan C# kodu oluşturmak için Java kod jgit ayıklayın, dönüştürme işlemini gerçekleştirmek için bazı yamalar uygulama ve C# kod oluşturur keskinleştirme çalıştırmak için özel bir Otomatik Sistem ngit programcıları kullanın. Bu ngit proje üzerinde jgit yapılır sürekli, devam eden iş yararlanmasını sağlar.

Yoktur genellikle Önemsiz olmayan bir iş miktarı otomatik kod dönüştürme aracı önyükleme ile ilgili ve bunu kullanmak için bir engel olacak şekilde kanıtlamak. Çoğu durumda, bu el ile daha basit ve bağlantı noktası Java için C# daha kolay olabilir.



## <a name="related-links"></a>İlgili bağlantılar

- [Keskinleştirme dönüştürme aracı](https://github.com/mono/sharpen)
