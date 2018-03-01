---
title: "Info.plist başvurusu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2017
ms.openlocfilehash: 7732419af22ab8bd37cbfbf828dc59e48841217f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="infoplist-reference"></a>Info.plist başvurusu

Info.plist ile çalışma hakkında daha fazla bilgi için anahtarları lütfen [güvenlik ve gizlilik çalışma](~/ios/app-fundamentals/security-privacy.md) Kılavuzu. 

## <a name="location"></a>Konum 

Kullanıcının konumuna erişme Info.plist değişiklikleri gerektirir. Konum verileri ile ilgili aşağıdaki anahtarları ayarlamanız gerekir: 

* **NSLocationWhenInUseUsageDescription** - uygulamanızı ile etkileşim sırasında zaman kullanıcının konuma erişmek için. 
* **NSLocationAlwaysUsageDescription** - ne zaman, uygulamanızın arka planda kullanıcının konumuna erişir.

## <a name="photos"></a>Fotoğraf 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>Kişiler 

NSContactsUsageDescription 

## <a name="calendar-data"></a>Takvim verileri 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>Anımsatıcı veri 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Bluetooth çevre birimleri 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>Mikrofon 

NSMicrophoneUsageDescription 

## <a name="camera"></a>Kamera 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>HealthKit okuma  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>Arka plan modları 
    
Uıbackgroundmodes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 


## <a name="related-links"></a>İlgili bağlantılar

- [Apple Başvuru Kılavuzu.](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
