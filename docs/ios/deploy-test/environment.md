---
title: Xamarin.iOS uygulamaları için yürütme ortamı
description: Bu belge, bir Xamarin.iOS uygulaması için geçici ve kalıcı ortam değişkenlerini ayarlamak açıklar. Değişkenleri, bir projenin özelliklerini veya mtouch paketleme aracı için ek bağımsız değişkenler olarak belirtilebilir.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 5296f03cae28e1025c760004c520a2b415ec493d
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351596"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Xamarin.iOS uygulamaları için yürütme ortamı

*Yürütme ortamı* program yürütme etkileyen ortam değişkenlerini kümesidir. Ortam değişkenleri proje özelliklerinde veya kalıcı olarak mtouch paketleme aracı için ek bağımsız değişkenler belirterek geçici olarak ayarlanabilir.

## <a name="temporary-environment-variables"></a>Geçici ortam değişkenleri

Geçici ortam değişkenleri projesinin ayarlanır **özellikleri**/**seçenekleri** penceresinde **çalıştırın > Genel** bölümü. Uygulama, bu ortam değişkenleri ayarlanmamış dokunarak el ile başlatıldığında uygulama, Mac için Visual Studio kullanarak başlatıldığında, bu ortam değişkenleri yalnızca geçerli olur.

## <a name="permanent-environment-variables"></a>Kalıcı ortam değişkenleri

Mtouch paketleme aracı için ek bağımsız değişkenler belirterek kalıcı ortam değişkenlerini ayarlar. Bu ortam değişkenleri yürütülebilir dosyada derlenir ve uygulama, Mac için Visual Studio'dan başlatılmaz bile ayarlayın

## <a name="example"></a>Örnek

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

