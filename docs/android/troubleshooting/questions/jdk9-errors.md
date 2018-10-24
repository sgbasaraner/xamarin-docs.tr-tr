---
title: Xamarin.Android ve Java Geliştirme Seti 9
description: Bu makalede, Java Development Kit (JDK) 9 veya sonraki Xamarin.Android hataları çözmek açıklanmaktadır.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: d8c64ff79367d93e282edd9534ffb98f5bb90c93
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "36269679"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android ve Java Geliştirme Seti 9 veya üzeri

_Bu makalede, Java Development Kit (JDK) 9 veya sonraki Xamarin.Android hataları çözmek açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Xamarin.Android, Android uygulamaları oluşturmak ve Android designer'ı çalıştırmak için Android SDK'sıyla tümleştirmek için Java Development Kit (JDK) kullanır. JDK 8 (1.8) veya Microsoft Mobil OpenJDK Önizleme (API 24 ve üzeri) Android SDK'ın son sürümlerini gerektirir. **Google'nın kullanıma Android SDK araçlarının henüz JDK 9 ile uyumlu olmadığından, Xamarin.Android JDK 9 veya üzeri çalışmaz.**

## <a name="jdk-errors"></a>JDK hatalarını

JDK JDK 8 belirtilenden sonraki bir sürümü ile bir Xamarin.Android projesi oluşturmaya çalışırsanız, bu sürümü JDK desteklenmediğini belirten bir açık bir hata alırsınız. Örneğin:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Bu hataları gidermek için JDK 8 (1.8) açıklandığı gibi yüklemeniz gerekir [Java Development Kit (JDK) sürümünü nasıl güncelleştirebilirim?](~/android/troubleshooting/questions/update-jdk.md).
Alternatif olarak, yükleyebileceğiniz [Microsoft Mobil OpenJDK Önizleme](~/android/get-started/installation/openjdk.md) Microsoft Mobil OpenJDK Xamarin.Android geliştirme için JDK 8 sonunda değiştirir.


## <a name="checking-the-jdk-version"></a>JDK sürüm denetimi

Aşağıdaki komutu girerek yüklediğiniz Java sürümünü görmek için kontrol edebilirsiniz (JDK `bin` dizin olmalıdır, `PATH`):

```shell
java -version
```

JDK 9 yüklü değilse, aşağıdaki gibi bir ileti görürsünüz:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 veya üzeri yüklü değilse, Java JDK 8 (1.8) veya Microsoft Mobil OpenJDK Önizleme yüklemeniz gerekir. JDK 8 yükleme hakkında daha fazla bilgi için bkz: [Java Development Kit (JDK) sürümünü nasıl güncelleştirebilirim?](~/android/troubleshooting/questions/update-jdk.md). Microsoft Mobil OpenJDK yükleme hakkında daha fazla bilgi için bkz: [Microsoft Mobil OpenJDK Önizleme](~/android/get-started/installation/openjdk.md).

TID JDK daha sonraki bir sürümünü kaldırmanız gerekmez. Ancak, Xamarin JDK sürüme yerine JDK 8 kullandığından emin olmanız gerekir. Visual Studio'da **Araçlar > Seçenekler > Xamarin > Android ayarları**. Varsa **Java Development Kit konumunu** JDK 8 konuma ayarlı değil (gibi **C:\\Program dosyaları\\Java\\jdk1.8.0_111**), tıklayın **Değiştir**  JDK 8 yüklendiği konumun ayarlayın. Mac için Visual Studio'da gidin **tercihleri > Proje > SDK konumları > Android > Java SDK (JDK)** tıklatıp **Gözat** bu yolu güncelleştirmek için.

## <a name="known-issues-with-jdk-9"></a>JDK 9 ile ilgili bilinen sorunlar

### <a name="apksigner"></a>apksigner

Apksigner ve JDK 9 ile bilinen bir sorun, `apksigner.bat` dosya çağırır `apksigner.jar` ile `-Djava.ext.dirs` yerine `-classpath` , JDK 9 bekliyor. JDK 8 (1.8) kullanmanız önerilir. JDK 8 yükleme hakkında daha fazla bilgi için bkz: [Java Development Kit (JDK) sürümünü nasıl güncelleştirebilirim?](~/android/troubleshooting/questions/update-jdk.md)

JDK 9 yüklediyseniz, aşağıdaki yol üzerinde ayarlı değil emin olmak sizin `PATH` ortam değişkeni olarak yine de JDK 9'a gelin: `C:\ProgramData\Oracle\Java\javapath`. Bu, kaldırdıktan sonra `java-version` JDK 8 bir komut satırında göstermelidir.
