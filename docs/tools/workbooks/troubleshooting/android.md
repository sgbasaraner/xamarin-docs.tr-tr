---
title: Xamarin Android çalışma kitapları sorunlarını giderme
description: Bu belge, Xamarin Android çalışma kitapları ile çalışmak için sorun giderme ipuçları sağlar. Öykünücü desteği, yüklenmiyor çalışma kitapları ve diğer konular ele alınmaktadır.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: b0333e1a40570374ee6218b7a848d2dd1c06b872
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351723"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Xamarin Android çalışma kitapları sorunlarını giderme

## <a name="emulator-support"></a>Öykünücü desteği

Android çalışma kitabının çalıştırmak için bir Android öykünücüsü'nü kullanıma hazır olması gerekir. Fiziksel bir Android cihazlar desteklenmez.

Bilgisayarınızı destekliyorsa, Google'nın öykünücüsü HAXM birlikte öneririz.
Hyper-V sisteminizde etkin olması gerekir, bunun yerine Visual Studio Android öykünücüsü ile gidin.

Android 5.0 veya sonraki sürümünü çalıştıran bir öykünücü olması gerekir. ARM öykünücüleri desteklenmez. Kullanım `x86` veya `x86_64` yalnızca cihazlar.

Lütfen okuma [belgelerimizin Android öykünücüleri ayarlama] [ android-emu] işlemiyle ilgili bilgi sahibi değilseniz.

> [!NOTE]
> Çalışma kitapları 1.1 ve önceki deneyin (başarısız kullanılabilir olmaları durumunda kullanım ARM öykünücüleri ve!). Bu, başlatma x86 öykünücü açmadan veya oluşturmadan bir Android çalışma kitabı önce tercih ettiğiniz geçici olarak çözmek için. Uyumlu olduğu sürece çalışma kitapları çalışan bir öykünücü bağlanmak her zaman tercih eder.

## <a name="workbooks-wont-load"></a>Çalışma kitapları yüklenmiyor

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Çalışma kitabı penceresi sonsuza kadar hiçbir zaman yükler (Windows) döner

İlk olarak, tüm Web sitelerinin öykünücü'nın web tarayıcısında test ederek, öykünücüsü tam olarak çalışan ağ erişimi olduğundan emin olun.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Android öykünücüsü internet'e bağlanılamıyor

Uygulamanızı öykünücü ağ erişimi yoksa, Hyper-V ağ anahtarı düzeltmek için bu adımları gerekebilir. Sık Wi-Fi ağları arasında geçiş yaptığınızda, düzenli aralıklarla yinelemek gerekebilir:

0. **Bu geçici olarak internet'ten Windows kesebilir tam, kritik ağ işlemleri olduğundan emin olun.**
1. Öykünücüler kapatın.
2. Açık `Hyper-V Manager`.
3. Altında `Actions`açın `Virtual Switch Manager...`.
4. Tüm sanal anahtarlar silin.
5. Tıklayın `OK`.
6. VS Android öykünücüsünde başlatın. Büyük olasılıkla sanal ağ anahtarı oluşturmanız istenir.
7. Test VS Android Emulator'ın tarayıcı internet erişebilirsiniz.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>İlgili bağlantılar

- [Hata Raporlama](~/tools/workbooks/install.md#reporting-bugs)
