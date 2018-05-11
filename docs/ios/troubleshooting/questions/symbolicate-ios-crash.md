---
title: İOS kilitlenme günlükleri symbolicate için .dSYM dosyası nereden bulabilirim?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/09/2018
ms.openlocfilehash: 60d897be8739ff5b78a322bc4ea3f43011785bb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>İOS kilitlenme günlükleri symbolicate için .dSYM dosyası nereden bulabilirim?

Visual Studio iOS uygulamasıyla Mac veya Visual Studio 2017 oluştururken, uygulamanızın proje dosyası (.csproj) aynı dizin hiyerarşisindeki kilitlenme raporları symbolicate için gerekli olan .dSYM dosya yerleştirilir. Tam konumu projenizin yapı ayarlarına bağlıdır:

- Aygıta özgü derlemeleri etkinleştirilirse aşağıdaki dizinde .dSYM bulunabilir:

    **&lt;Proje dizini&gt;/bin/&lt;platform&gt;/&lt;yapılandırma&gt;/device-builds /&lt;aygıt&gt; - &lt; işletim sistemi sürümü&gt;/**

    Örneğin:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- Aygıta özgü derlemeleri etkinleştirmediyseniz .dSYM şu dizinde bulunabilir:

    **&lt;Proje dizini&gt;/bin/&lt;platform&gt;/&lt;yapılandırma&gt;/**

    Örneğin:

    **TestApp/bin/iPhone/sürüm /**

> [!NOTE]
> Derleme işleminin bir parçası olarak, Visual Studio 2017 Windows Mac yapı konağı .dSYM dosya kopyalar. Windows .dSYM dosyada görmüyorsanız, uygulamanızın yapılandırma ayarlarını yapılandırdığınız emin olun [bir .ipa dosyası oluşturma](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>Ayrıca bkz.

- [İOS kilitlenme dosyaları (Xamarin.iOS) symbolicating](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [İOS uygulama kilitlenme günlüklerini demystifying](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

