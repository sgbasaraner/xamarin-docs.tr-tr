---
title: Ortam
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c46d3aa151d4a596ca3da881a3c4e5e38bb41cbd
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="environment"></a>Ortam

*Yürütme ortamı* program yürütme etkileyen ortam değişkenleri kümesidir. Ortam değişkenleri geçici olarak Proje Özellikleri'nde veya kalıcı olarak mtouch paketleme aracı için ek bağımsız değişkenler belirterek ayarlayabilirsiniz.

## <a name="temporary-environment-variables"></a>Geçici ortam değişkenleri

Geçici ortam değişkenlerini projesinin ayarlama **özellikleri**/**seçenekleri** penceresinde **Çalıştır > Genel** bölümü. Uygulama bu ortam değişkenleri ayarlanmaz üzerindeki e dokunabilirsiniz elle başlatılmışsa uygulama Mac için Visual Studio kullanarak başlatıldığında, bu ortam değişkenleri yalnızca etkin olacaktır.

## <a name="permanent-environment-variables"></a>Kalıcı ortam değişkenleri

Kalıcı ortam değişkenleri mtouch paketleme aracı için ek bağımsız değişkenler belirterek ayarlanır. Bu ortam değişkenleri yürütülebilir derlenir ve uygulamayı Visual Studio'dan Mac için başlatılmadığı olsa bile ayarlanacak

## <a name="example"></a>Örnek

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```
