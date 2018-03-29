---
title: "Android'de sorun giderme Xamarin çalışma kitapları"
ms.topic: article
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 530abec733ec1d842559bf9c898217a8e45465aa
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Android'de sorun giderme Xamarin çalışma kitapları

## <a name="emulator-support"></a>Öykünücü desteği

Bir Android çalışma kitabı çalıştırmak için Android öykünücüsünde kullanılabilir olması gerekir. Fiziksel Android cihazları desteklenmez.

Bilgisayarınız destekliyorsa Google öykünücüsü HAXM birlikte öneririz.
Hyper-V, sistemde etkin olması gerekir, bunun yerine Visual Studio Android öykünücü ile gidin.

Android 5.0 veya sonraki sürümünü çalıştıran bir öykünücü olması gerekir. ARM Öykünücüler desteklenmez. Kullanım `x86` veya `x86_64` yalnızca cihazlar.

Lütfen okuyun [Android öykünücüsünü ayarlama Belgelerimizi] [ android-emu] işlemle bilmiyorsanız.

> [!NOTE]
> Çalışma kitapları 1.1 ve önceki deneyin (başarısız kullanılabilir olmaları durumunda kullanım ARM Öykünücüler ve!). Bu, başlatma x86 öykünücüsü açma veya bir Android çalışma kitabı oluşturmadan tercih ettiğiniz geçici olarak çözmek için. Uyumlu olduğu sürece çalışma kitaplarını çalışan öykünücüsünü bağlanmak her zaman tercih eder.

## <a name="workbooks-wont-load"></a>Çalışma kitapları yüklenmiyor

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Çalışma kitabı penceresi sonsuza kadar hiçbir zaman yükler (Windows) döner

İlk olarak, tüm Web sitesi öykünücüsü'nın web tarayıcısında test ederek, öykünücüsü tam olarak çalışan ağ erişimi olduğundan emin olun.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Android öykünücüsü İnternete bağlanılamıyor

Öykünücüsü ağ erişimi yoksa, Hyper-V ağ anahtarı düzeltmek için bu adımları gerekebilir. Sık Wi-Fi ağları arasında geçiş yapıyorsanız bu işlemi düzenli aralıklarla yineleyin gerekebilir:

0. **Bu geçici olarak internet'ten Windows kesebilir gibi kritik ağ işlemleri tamamlandığından emin olun.**
1. Öykünücüler kapatın.
2. Açık `Hyper-V Manager`.
3. Altında `Actions`, açık `Virtual Switch Manager...`.
4. Tüm sanal anahtarları silin.
5. Tıklatın `OK`.
6. VS Android öykünücüsünde başlatın. Sanal ağ anahtarı yeniden oluşturmak için büyük olasılıkla istenir.
7. Test VS Android öykünücüsü'nın tarayıcı Internet'e erişebilir.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>İlgili bağlantılar

- [Raporlama hataları](~/tools/workbooks/install.md#reporting-bugs)