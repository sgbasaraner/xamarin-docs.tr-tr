---
title: "İOS kilitlenme günlükleri symbolicate için .dSYM dosyası nereden bulabilirim?"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 223e1977bb6229760d6428fdca2f5372b1e25c23
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>İOS kilitlenme günlükleri symbolicate için .dSYM dosyası nereden bulabilirim?

Visual Studio'dan iOS uygulamaları oluştururken, kilitlenme raporları symbolicate için kullanılan .dSYM dosya yolunda derleme konaktaki sonlanır:
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

Unutmayın `~/Library` klasörü gizli Finder içinde varsayılan olarak, böylece kullanılacak Bulanın **Git > klasörüne gidin** menü ve girin: `~/Library/Caches/Xamarin/mtbs/builds/` klasörünü açın.  

Alternatif olarak gösterebileceği `~/Library` kullanarak klasörüne **Görünüm Seçenekleri Göster** paneli giriş klasörü için. Kenar çubuğunda Bulucu giriş klasörü seçin ve Bulucu menüsünü kullanın, **Görünüm > Görünümü Seçenekleri Göster** (veya cmd-j) için bir onay kutusu görür **kitaplık klasörünü göster**.


### <a name="see-also"></a>Ayrıca Bkz.
- İOS symbolicating genişletilmiş adımları kilitlenme raporları: [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [İOS uygulama kilitlenme günlüklerini demystifying](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
