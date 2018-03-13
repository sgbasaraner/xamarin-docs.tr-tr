---
title: Ortam
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7489c2fe38e8433811c5f298296baebacf1c0727
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="environment"></a>Ortam

*Yürütme ortamı* program yürütme etkileyen ortam değişkenleri kümesidir. Ortam değişkenleri geçici olarak Proje Özellikleri'nde veya kalıcı olarak mtouch paketleme aracı için ek bağımsız değişkenler belirterek ayarlayabilirsiniz.

## <a name="temporary-environment-variables"></a>Geçici ortam değişkenleri

Geçici ortam değişkenlerini projesinin ayarlama **özellikleri**/**seçenekleri** penceresinde **Çalıştır > Genel** bölümü. Uygulama bu ortam değişkenleri ayarlanmaz üzerindeki e dokunabilirsiniz elle başlatılmışsa uygulama Mac için Visual Studio kullanarak başlatıldığında, bu ortam değişkenleri yalnızca etkin olacaktır.

## <a name="permanent-environment-variables"></a>Kalıcı ortam değişkenleri

Kalıcı ortam değişkenleri mtouch paketleme aracı için ek bağımsız değişkenler belirterek ayarlanır. Bu ortam değişkenleri yürütülebilir derlenir ve uygulama Xamarin Studio başlatılmadığı olsa bile ayarlanır.

## <a name="example"></a>Örnek

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

