---
title: Xamarin.iOS uygulamaları için yürütme ortamı
description: Bu belge, bir Xamarin.iOS uygulaması için geçici ve kalıcı ortam değişkenlerini ayarlama açıklar. Değişkenleri, bir projenin özelliklerinde veya mtouch paketleme aracı ek bağımsız değişken olarak belirtilebilir.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 246c65729f9327dd1ccf549603b4c2b1feb023e8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784972"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Xamarin.iOS uygulamaları için yürütme ortamı

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

