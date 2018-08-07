---
title: Microsoft'un mobil OpenJDK dağıtım Önizleme
description: Mobil Geliştirme için Microsoft'un OpenJDK dağıtımını yapılandırmak için adım adım bir kılavuz.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 2022337ebd65997c7b2492137193586278f2dffd
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573600"
---
# <a name="microsofts-mobile-openjdk-distribution-preview"></a>Microsoft'un mobil OpenJDK dağıtım Önizleme

_Bu kılavuzda Microsoft'un OpenJDK dağıtımını Önizleme sürümüne geçiş adımlarını açıklar. Bu dağıtım, Mobil Geliştirme için tasarlanmıştır._

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="overview"></a>Genel Bakış

Mac 7.7 için Visual Studio 15.9 ve Visual Studio ile başlayarak, Xamarin için Visual Studio Araçları Oracle'dan 's taşınır JDK için bir **basit sürümü yalnızca Android geliştirme için hedeflenen OpenJDK**:

![OpenJDK VS 15,8 Preview 5'teki web önizlemesini sunan yeni bir iş akışı](openjdk-images/openjdk.png)

Bu taşıma avantajları şunlardır:

- Android geliştirme için uygun bir OpenJDK sürümü her zaman olacaktır.

- JDK 9 veya 10 indirme geliştirme deneyimini etkilemez.

- Önemli ölçüde azaltılmış indirme boyutu ve Ayak izi.

- 3. taraf sunucuları ve yükleyicileri ile daha fazla sorun yok.

Geliştirilmiş bir deneyim için daha kısa süre içinde taşımak istiyorsanız, Microsoft Mobil OpenJDK dağılımının yapılar, hem Windows hem de Mac üzerinde test etmek için kullanılabilir Kurulum işlemi, aşağıda açıklanan ve geri Oracle JDK için dilediğiniz zaman geri dönebilirsiniz.

## <a name="download"></a>İndir

Başlamak için sisteminiz için doğru yapı indirin:

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

## <a name="configure"></a>Yapılandırma

Doğru konuma sıkıştırmasını açın:

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Program dosyaları\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

> [!IMPORTANT]
> Bu örnek, derleme 1.8.0.9 kullanır; Ancak, yüklediğiniz sürümü daha yeni olabilir.

IDE için yeni JDK noktası:

- **Mac** &ndash; tıklayın **Araçlar > SDK Yöneticisi > konumları** değiştirip **Java SDK (JDK) konumu** OpenJDK yüklemesinin tam yolu. Aşağıdaki örnekte, bu yolunu ayarlamak **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**.

![Mac üzerinde Microsoft Mobil OpenJDK dağıtım için JDK yolu ayarlama](openjdk-images/vsm.png)

- **Windows** &ndash; tıklayın **Araçlar > Seçenekler > Xamarin > Android ayarları** değiştirip **Java Development Kit konumunu** OpenJDK yüklemesinin tam yolu. Aşağıdaki örnekte, bu yolunu ayarlamak **C:\\Program dosyaları\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**:

![Windows üzerinde Microsoft Mobil OpenJDK dağıtım için JDK yolu ayarlama](openjdk-images/vs.png)

## <a name="revert"></a>Geri Al

Oracle JDK için geri almak için daha önce kullanılan Oracle JDK yolu Java SDK'sı konumu değiştirin ve çözümü yeniden oluşturun. Mac üzerinde Oracle JDK yolu tıklayarak döndürebilirsiniz **varsayılan ayarlarına geri döndürmeyi**.

Microsoft Mobil OpenJDK dağıtım herhangi bir sorun varsa, lütfen böylece izlenen edilebilmeleri ve hızlı bir şekilde düzeltildi, IDE içinde geri bildirim aracını kullanarak sorunları bildirin.

## <a name="known-issues--planned-fix-dates"></a>Bilinen sorunlar ve planlı düzeltme tarihleri

`JAVA_HOME` Ortam değişkeni doğru olarak dışarı aktarılamadı SDK'sı ve cihaz Yöneticisi. Geçici çözüm olarak, bilgisayarınızda OpenJDK konumuna bu ortam değişkeni ayarlayabilirsiniz. Bu 15,9 önizlemelerinde sabittir.

## <a name="summary"></a>Özet

Bu makalede, 2018'de daha sonra kararlı bir sürüm için planlanmıştır Microsoft'un mobil OpenJDK dağıtımı önizleme sürümünü kullanmak için IDE'yi yapılandırma öğrendiniz.
