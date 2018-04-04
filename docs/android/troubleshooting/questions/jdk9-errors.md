---
title: Xamarin.Android ve JDK 9
description: Bu makalede Xamarin.Android içinde Java Geliştirme Seti (JDK) 9 hataların nasıl çözüleceği açıklanmaktadır.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 8857823884447f22b7bc5535f43369671d3285bc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinandroid-and-jdk-9"></a>Xamarin.Android ve JDK 9

_Bu makalede Xamarin.Android içinde Java Geliştirme Seti (JDK) 9 hataların nasıl çözüleceği açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Xamarin.Android Java Geliştirme Seti (JDK) Android Android uygulamaları oluşturmak ve Android Tasarımcısı'nı çalıştırmak için SDK ile tümleştirmek için kullanır. (API 24 ve üzeri) Android SDK'ın en son sürümleri JDK 8 (1.8) gerektirir. **Google bulunan Android SDK Araçları henüz JDK 9 ile uyumlu olmadığından Xamarin.Android JDK 9 ile çalışmaz.**

## <a name="jdk-9-errors"></a>JDK 9 hataları

JDK 9 yüklü bir Xamarin.Android projesi oluşturmayı denemek JDK 9 desteklenmediğini belirten bir açık hata alırsınız.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Bu hataları gidermek için JDK 8 (1.8) açıklandığı gibi yüklemeniz gerekir [Java Geliştirme Seti (JDK) sürüm nasıl güncelleştirebilirim?](~/android/troubleshooting/questions/update-jdk.md).


## <a name="checking-the-jdk-version"></a>JDK sürüm denetimi

Java aşağıdaki komutu girerek yüklü sürümünü görmek için kontrol edebilirsiniz (JDK `bin` dizin olmalıdır, `PATH`):

```shell
java -version
```

JDK 9 yüklü değilse, aşağıdakine benzer bir ileti görürsünüz:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 yüklüyse, Java JDK 8 (1.8) yüklemeniz gerekir. JDK 8 yükleme hakkında daha fazla bilgi için bkz: [Java Geliştirme Seti (JDK) sürüm nasıl güncelleştirebilirim?](~/android/troubleshooting/questions/update-jdk.md)

## <a name="known-issues-with-jdk-9"></a>JDK 9 ile ilgili bilinen sorunlar

### <a name="apksigner"></a>apksigner

Bir bilinen sorun olmadığından apksigner ve JDK 9, `apksigner.bat` dosyasını çağırır `apksigner.jar` ile `-Djava.ext.dirs` yerine `-classpath` JDK 9 bekleyen. JDK 8 (1.8) kullanmanız önerilir. JDK 8 yükleme hakkında daha fazla bilgi için bkz: [Java Geliştirme Seti (JDK) sürüm nasıl güncelleştirebilirim?](~/android/troubleshooting/questions/update-jdk.md)

JDK 9 kaldırıldıktan sonra aşağıdaki yolu üzerinde ayarlanmadı emin olun, `PATH` ortam değişkeni olarak hala JDK 9'a işaret: `C:\ProgramData\Oracle\Java\javapath`. Bu, kaldırdıktan sonra `java -version` bir komut satırında JDK 8 göstermesi gerekir.
