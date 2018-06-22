---
title: MDocArchiveToMsxDocConverter.exe rver bulunamadı. BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 6d7fe8f48d22c6b5d445fa8356e52f924549f6e0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775682"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe rver bulunamadı. BaseCommand.OnRequest

> [!IMPORTANT]
> Xamarin en son sürümleri bu sorunu Çözümlendi. Ancak, en son sürümünü yazılım üzerinde gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.


## <a name="error-message"></a>Hata iletisi

Bu hata görüntülenebilir *Mac sunucu günlüğü* Visual Studio'da:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

Bu ileti 2 ayrı sorunlar vardır:

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Bu hata zararsız, ancak aynı zamanda yanıltıcı. Bu [kaldırılacak](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) gelecek bir sürümde.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Bu hata gerçek bir sorundur. Ne yazık ki, verilecek bir [sınırlaması](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) bu özel durum yığın izlemesi *tamamlanmamış*. Bu gibi tamamlanmamış yığın izlemesi Mac Server günlüğünde fark ederseniz kontrol edebilirsiniz `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` tam yığını izleme bulmak için Mac yapı ana dosya.
