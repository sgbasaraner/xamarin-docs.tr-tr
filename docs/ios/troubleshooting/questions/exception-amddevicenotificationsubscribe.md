---
title: System.Exception AMDeviceNotificationSubscribe döndürülen...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 257e0c14de3c23825b6abe6601c25438db81c58e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe döndürülen...

> [!IMPORTANT]
> Xamarin en son sürümleri bu sorunu Çözümlendi. Ancak, en son sürümünü yazılım üzerinde gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.


## <a name="fix"></a>Düzeltme

1.  KILL `usbmuxd` sistem yeniden böylece işlem:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  Bu sorunu çözmezse, Mac yeniden başlatma

## <a name="error-message"></a>Hata iletisi

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

İlk Visual Studio veya Mac için başlattığınızda bir hata iletişim kutusunda Bu ileti görünebilir `mtbserver.log` Xamarin.iOS yapı konağı uygulama dosyasında (**Xamarin.iOS yapı konağı > Yapı konak Günlüğü Görüntüle**).

Bu seyrek bir sorun olduğunu unutmayın. Visual Studio Mac yapı konağa bağlanırken sorun yaşıyor varsa görünür olasılığı yüksek olan diğer hataları `mtbserver.log` dosya.

### <a name="errors-in-systemlog"></a>System.log hataları

Bazı durumlarda aşağıdaki iki hata iletileri de art arda içinde görünebilir `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Ek bilgiler

Hatanın temel nedenlerinden biri, bir tahmin nadir durumlarda Sistem Hizmetleri iOS aygıtı ve simulator bilgilerini raporlama için sorumlu olabilir OS X beklenmeyen bir durum girin ' dir. Xamarin düzgün bir şekilde bu durum sistem hizmetleriyle etkileşime giremezler. Bilgisayar yeniden başlatıldığı sistem hizmetleri yeniden başlatır ve sorunu çözer.

Hataları göre `system.log` Bu sorun için Bonjour ilgili görünür (`mDNSResponder`). Sorun çıkma olasılığını artırmak için farklı WiFi ağlar arasında değiştirme gibi görünüyor.

## <a name="references"></a>Referanslar

*   [Hata 11789 - MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe döndürdü: 0xe8000063 [NORESPONSE ÇÖZÜMLENDİ]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
