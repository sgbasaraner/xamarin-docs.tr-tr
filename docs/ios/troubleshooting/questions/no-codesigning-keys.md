---
title: "My iOS yapı neden başarısız: imzalama anahtarlarının hiçbir geçerli iPhone kodu bulunan Anahtarlıkta?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6a853bc7186647dbdae1d5d12f3b185d302a8088
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>My iOS yapı neden başarısız: imzalama anahtarlarının hiçbir geçerli iPhone kodu bulunan Anahtarlıkta?

## <a name="cause-of-the-error"></a>Hatanın nedeni
Bu hata iletisini söz konusu proje kod imzalama için geçerli kimlik bilgileri bakarken oluşur ancak bunları bulamıyor. Kod imzalama, test ve fiziksel iOS cihazlarda dağıtımlar için gereklidir; aynı zamanda derlemeleri deposundan geçici & uygulama. 


### <a name="provisioning-devices"></a>Aygıtları sağlama
Aşağıdaki kılavuzu bir iOS aygıtı önce sağlanan henüz varsa tam adım adım işlemi boyunca sürer: [cihaz sağlama Kılavuzu](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>İOS simülatörü'nü kullanırken hata

> [!NOTE]
> Bu sorun, Visual Studio için Xamarin en güncel sürümlerini çözümlendi. Ancak, en son sürümünü yazılım üzerinde gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.


İOS projesine Simulator Entitlements.plist aşağıda derlemeler eklemek için bir Xamarin.Forms şablonunda neden Xamarin.Visual Studio 3.11 bir hata vardı; etkili bir şekilde Simulator kullanarak test engelliyor.

### <a name="how-to-fix"></a>Düzeltme yapma
Geçici çözüm bu sorunu kaldırarak yapabilirsiniz `<CodesignEntitlements>` hata ayıklama bayrağı .csproj dosyanızda oluşturur. Bunu şu şekilde yapabilirsiniz:

*Uyarı: Bu denemeden önce dosyalarınızı yedeklemek için iyi bir fikirdir şekilde .csproj dosyaları hataları, projenizin bozulabilir.*

1. Çözüm bölmesinde iOS projesine sağ tıklayın ve seçin **projeyi Kaldır**
2. Sağ projeyi yeniden tıklatın ve seçin **[ProjectName] .csproj Düzenle**
3. Hata ayıklama PropertyGroups bulun, şuna bayrağı ile başlamanız gerekir:
   - Hata ayıklama: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Sürüm: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. Her simulator kullanan derlemeler, silin veya aşağıdaki özellik Açıklama: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Projeyi yeniden yüklemek ve simulator dağıtamaz olması gerekir.

### <a name="next-steps"></a>Sonraki Adımlar
Bizimle iletişime geçin veya bile Yukarıdaki bilgilerin kullanılarak sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [için Xamarin hangi destek seçenekleri kullanılabilir?](~/cross-platform/troubleshooting/support-options.md) önerileri, iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Yeni bir hatanın gerekirse dosya. 
