---
title: Xamarin Studio'da nasıl iOS projeleri için Mono çalışma zamanı ortam değişkenlerini ayarlama?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 5f4f3a2de012d35ddca9c1fa830d599d9d5acb17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio'da nasıl iOS projeleri için Mono çalışma zamanı ortam değişkenlerini ayarlama?

Tüm çalışma zamanı Mono için ortam değişkenlerini ayarlama ihtiyacınız varsa, bunlar ayarlanabilir **proje Seçenekleri > Çalıştır > Genel** sayfası.

Not: Atık toplama ortam değişkenleri SGen için (MONO\_GC\_PARAMS) bu şekilde, yalnızca Xamarin Studio'dan başlatılırken kullanılacak kümesi. Cihaz uygulama başlatma, Sgen ayarlarını göz ardı edilir. 

Bir uygulama için bir ortam değişkeni kalıcı olarak ayarlamak için bu ek mtouch bağımsız değişken (için tüm ilgili yapılandırmaları) olarak eklemeniz gerekir:

```csharp
   --setenv=NAME=VALUE
```

Ayarlanabilir ortam değişkenleri görmek için Mono adam sayfasına bakın: [ http://docs.go-mono.com/?link=man%3amono(1) ](http://docs.go-mono.com/?link=man%3amono(1)) başlıklı bölüme bakın: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Bir proje için ortam değişkenlerini ayarlama")
