---
title: Java Geliştirme Seti (JDK) sürüm nasıl güncelleştiririm?
description: Bu makalede Windows ve Mac üzerinde Java Geliştirme Seti (JDK) Sürüm güncelleştirme nasıl gösterir
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 979bf4572e0e0865c2254c3e1c2f707c8eecae8d
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269666"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Java Geliştirme Seti (JDK) sürüm nasıl güncelleştiririm?

_Bu makalede Windows ve Mac üzerinde Java Geliştirme Seti (JDK) Sürüm güncelleştirme nasıl gösterir_

## <a name="overview"></a>Genel Bakış

Xamarin.Android Java Geliştirme Seti (JDK) Android Android uygulamaları oluşturmak ve Android Tasarımcısı'nı çalıştırmak için SDK ile tümleştirmek için kullanır. (API 24 ve üzeri) Android SDK'ın en son sürümleri JDK 8 (1.8) gerektirir. JDK 8'e henüz gerçekleştirmediyseniz, yüklemek ve etkinleştirmek için aşağıdaki adımları izleyin:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  JDK 8 (1.8) indirin [Oracle Web sitesi](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Ekran görüntüsü JDK Oracle Web sayfasında indirin](update-jdk-images/image1.png)

2.  Çizmeye izin vermek için 64-bit sürümünü seçin [özel denetimler](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) Xamarin Android Tasarımcısı'nda:

    ![JDK indirme sayfasından indirmek için Windows x64 JDK paketi seçme](update-jdk-images/image2.png)

3.  .exe çalıştırmak ve yüklemek **geliştirme araçları**:

    ![JDK yükleyicisinde geliştirme araçlarını yükleme](update-jdk-images/image3.png)

4.  Açık Visual Studio ve güncelleştirme **Java Geliştirme Seti konumu** altında yeni JDK işaret edecek şekilde **Araçlar > Seçenekler > Xamarin > Android Ayarları > Java Geliştirme Seti konumu > değişiklik**:

    [![Yol ayar IDE seçenekleri Android ayarı sayfasındaki JDK için](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Visual Studio konumu güncelleştirdikten sonra yeniden başlattığınızdan emin olun.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  JDK 8 (1.8) indirin [Oracle Web sitesi](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Ekran görüntüsü JDK Oracle Web sayfasında indirin](update-jdk-images/image1.png)

2.  .Dmg dosyasını açın ve .pkg yükleyiciyi çalıştırın:

    ![JDK yükleyici macOS üzerinde çalışıyor](update-jdk-images/image5.png)

Mac OS otomatik olarak ayarlayacak yeni JDK sürümü varsayılan olarak güncelleştirerek **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Daha sonra iki kez kontrol **Java SDK'sı (JDK)** konumu beklenen varsayılan olarak ayarlanmış **/usr** altında **Mac için Visual Studio > Tercihler > projeleri > SDK konumları > Android > Java SDK'sı (JDK) > konumu**:

![Android SDK konumu sayfasında JDK konumu ayarlama](update-jdk-images/image6.png)

-----

