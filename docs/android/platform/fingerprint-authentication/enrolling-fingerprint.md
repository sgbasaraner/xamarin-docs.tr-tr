---
title: Parmak izi kaydetme
description: "Nasıl ayarlanacağı bir ekran kilidi ayarlama ve parmak izi bir Android cihaz veya öykünücü kaydetme."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 20e6d693f2a3eba54afaf1d3c7054ad75d7a7610
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="enrolling-a-fingerprint"></a>Parmak izi kaydetme

## <a name="enrolling-a-fingerprint-overview"></a>Parmak izi genel bakış kaydetme

Yalnızca aygıt parmak izi kimlik doğrulaması ile yapılandırılmış parmak izi kimlik doğrulaması yararlanan bir Android uygulaması mümkündür. Bu kılavuz, bir Android cihaz veya öykünücü parmak izi nasıl ele alınacaktır. Öykünücüler parmak izi tarama gerçekleştirmek için gerçek donanıma sahip değilseniz, ancak bir parmak izi tarama Android hata ayıklama (aşağıda açıklanmıştır) köprüsü yardımıyla benzetimini mümkündür.  Bu kılavuz, bir Android cihazında ekran kilidi etkinleştirme ve kimlik doğrulama için parmak izi kaydetme nasıl ele alınacaktır.

## <a name="requirements"></a>Gereksinimler

Parmak izi kaydetmek için bir Android cihaz veya API düzeyi 23 (Android 6.0) çalıştıran bir öykünücü olması gerekir.

Komut istemi aşina, Android hata ayıklama Köprüsü (ADB) kullanılmasını gerektirir ve **adb** yürütülebilir, Bash, PowerShell yolunda olması veya komut istemi ortamı gerekir.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Bir ekran kilidi yapılandırma ve parmak izi kaydetme 

Bir ekran kilidi kurulumu için aşağıdaki adımları gerçekleştirin:

1. Git **ayarlar > Güvenlik**seçip **ekran kilidi**:

    ![Ekran kilidi seçimi güvenlik ekranında konumu](enrolling-fingerprint-images/testing-01.png)

2. Görüntülenen sonraki ekranında seçin ve ekran kilidi güvenlik yöntemlerden birini yapılandırın izin verir: 

    ![Manyetik, düzeni, PIN veya parola seçin](enrolling-fingerprint-images/testing-02.png)

   Seçin ve kullanılabilir ekran kilidi yöntemlerden birini tamamlayın.

3. Screenlock yapılandırıldıktan sonra geri dönüp **ayarlar > Güvenlik** sayfasından seçim yapıp **parmak izi**:

    ![Güvenlik ekranında parmak izi seçimini konumu](enrolling-fingerprint-images/testing-03.png)

4. Buradan, parmak izi aygıt eklemek için dizisi izleyin:

    [![Cihazda parmak izi eklemek için ekran görüntüleri bir dizi](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Son ekran, parmak izi tarayıcıya yerleştirin istenir: 

    ![Üzerinde algılayıcı, parmak koyun ister ekranı](enrolling-fingerprint-images/testing-05.png)

    Bir Android cihaz kullanıyorsanız, bir parmak tarayıcısına temas tarafından işlemini tamamlayın. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Parmak izi tarama öykünücüsü üzerinde benzetimini yapma

Bir Android öykünücüsünde Android hata ayıklama köprüsü kullanarak bir parmak izi tarama benzetimini mümkündür. OS X başlangıç bir Terminal oturumu sırasında Windows komut isteminde veya bir Powershell oturumu Başlat ve Çalıştır `adb`:

```shell
$ adb -e emu finger touch 1
```

Değeri **1** olan _parmak\_kimliği_ "taranan" parmak için. Her sanal parmak izi için atadığınız benzersiz bir tamsayıdır. Gelecekte uygulama çalışırken, bu aynı ADB çalıştırmak her zaman öykünücü komut sizden bir parmak izi için çalıştırdığınız `adb` komut ve iletmektir _parmak\_kimliği_ parmak izi tarama benzetimini yapmak için .

Parmak izi tarama tamamlandıktan sonra Android parmak izi eklendiğini bildirir:  

![Eklenen parmak izi görüntüleyen ekranı!](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Özet 

Bu kılavuz bir ekran kilidi Kurulum ve Android cihazda veya Android öykünücüsünde parmak izi kaydetmek nasıl ele. 

