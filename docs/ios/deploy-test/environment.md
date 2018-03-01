---
title: Ortam
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ef5cfa2a9eee0d5d01922e5b7b3f89a209396c56
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
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

