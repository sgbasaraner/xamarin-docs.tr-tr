---
title: 'Hata MT1009: derleme kopyalanamadı.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: de1d7ea8ce4c358ad69150be3da6b38fb6c730ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Hata MT1009: derleme kopyalanamadı.

> [!IMPORTANT]
> Yeni Xamarin.iOS sürümlerinde bu sorunu Çözümlendi. Ancak, en son sürümünü yazılım üzerinde gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.

Bölümünde açıklandığı gibi bizim [sürüm notları](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), Xamarin.iOS 7.2.6 içinde bilinen bir sorundur budur. Bu sorunu Xamarin.iOS farklı bir kullanıcı hesabıyla yüklendiğinde, daha yüksek ayrıcalıklar gerektiren dosya izinleri nedeniyle sonra Geliştirici ana hesabıdır.

Geçici çözüm bu sorunu için Mac istasyonunda Terminal.app açın ve aşağıdaki komutu çalıştırın:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

