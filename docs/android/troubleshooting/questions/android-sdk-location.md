---
title: "My Android SDK konumları burada ayarlayabilir miyim?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 11/16/2017
ms.openlocfilehash: 0113cc15bf1de5e0e668b05c2b0288a6ead141b5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>My Android SDK konumları burada ayarlayabilir miyim?

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da gidin **Araçlar > Seçenekler > Xamarin > Android ayarları** görüntülemek ve Android SDK'sı konumu ayarlamak için:

[![Tercihlerinde örnek konumları sekmesi](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Her bir yol için varsayılan konum aşağıdaki gibidir:

- Java Geliştirme Seti konumu: 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Android SDK'sı konumu: 

    **C:\\Program Files (x86)\\Android\\android-SDK'sı**

- Android NDK konumu: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android ndk r13b**

NDK sürüm numarasını değişebileceğini unutmayın. Örneğin, yerine, **android ndk r13b**, önceki bir sürümünü gibi olabilir **android ndk r10e**.

Android SDK'sı konumu ayarlamak için Android SDK dizine tam yolunu girin **Android SDK konumu** kutusu. Dosya Gezgini'nde Android SDK konumuna gidin, Adres çubuğundan yolu kopyalayın ve bu yola yapıştırmak **Android SDK konumu** kutusu.
Örneğin, Android SDK konumunuz adresindeki ise **C:\\kullanıcılar\\kullanıcıadı\\AppData\\yerel\\Android\\Sdk**, eskiyolundatemizleyin **Android SDK konumu** kutusuna bu yolda yapıştırın ve tıklatın **Tamam**.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da gidin **Tercihler > projeleri > SDK konumları > Android**. İçinde **Android** sayfasında, **konumları** sekmesini görüntülemek ve SDK konumu ayarlamak için:

[![Tercihlerinde örnek konumları sekmesi](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Her bir yol için varsayılan konum aşağıdaki gibidir:

- Android SDK'sı konumu: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK konumu: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) Location: 

    **/usr**

NDK sürüm numarasını değişebileceğini unutmayın. Örneğin, yerine, **android ndk r14b**, önceki bir sürümünü gibi olabilir **android ndk r10e**.

Android SDK'sı konumu ayarlamak için Android SDK dizine tam yolunu girin **Android SDK konumu** kutusu. Android SDK klasörüne Bulucu, basın seçebilirsiniz **CTRL +&#8984;+ ı** klasör bilgileri görüntülemek için tıklatın ve yol sağına sürükleyin **burada:**, kopyalamak ve ardından ona yapıştırın **Android SDK'sı Konum** kutusunda **konumları** sekmesi. Örneğin, Android SDK konumunuz adresindeki ise **~/Library/Developer/Android/Sdk**, eski yolunda temizleyin **Android SDK konumu** kutusuna bu yolda yapıştırın ve tıklatın **Tamam**.

-----
