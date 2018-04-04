---
title: My Android Resource.designer.cs dosyasını güncelleştirmez
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 6d20c92fbb61ab11fcfeda3e9d2f63fdfc5a6d54
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>My Android Resource.designer.cs dosyasını güncelleştirmez

> [!NOTE]
> Xamarin Studio 5.1.4 ve sonraki sürümlerinde bu sorunu Çözümlendi. Ancak, Mac için Visual Studio'daki sorun ortaya çıkarsa, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.

Xamarin.Studio 5.1 hatada kısmen veya tamamen .csproj dosyasındaki xml kodunu silerek .csproj dosyaları daha önce bozuk. Bu önemli Android bölümlerini yapı (Android Resource.designer.cs güncelleştirme gibi) sistem neden başarısız. Kararlı 5.1.4 itibariyle 15 Temmuz sürüm, bu hatanın düzeltildiğini; Ancak birçok durumda proje dosyası aşağıda açıklandığı gibi el ile onarılması gerekmektedir.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Proje dosyanın düzeltmek için iki olası yaklaşım

### <a name="either"></a>Şunlardan biri:

1) Yepyeni bir Xamarin.Android uygulaması projesi oluşturduğunuzda, eski projenizi eşleşmesi için tüm proje özelliklerini ayarlamak ve tüm kaynaklarınız ekleme, kaynak dosyaları, vb. projeye yedekleyin.

### <a name="or"></a>VEYA

2) Bir yedek kopyasının özgün projenizin .csproj dosyasının, bir metin düzenleyicisinde açın ve düzgün bir şekilde oluşturulan .csproj dosyasından eksik öğeleri geri ekleyin.

### <a name="if-this-does-not-solve-the-problem"></a>Bu sorunu çözmezse

Bu öğeler ile denedikten sonra geri öğeleri ekleme ve projeyi yeniden derlemeyi, Resource.designer.cs dosyasını güncelleştirmeniz gerekir, ancak sonra hala kapatın ve yeniden tanımak için kod tamamlama almak için çözüm açın gerekebilir sonra değiştiğini görebilirsiniz Resource.Designer.cs içinde bulunan yeni türler. 
