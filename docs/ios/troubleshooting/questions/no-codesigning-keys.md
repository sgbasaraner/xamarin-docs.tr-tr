---
title: 'Neden iOS derlemem başarısız oluyor: Anahtarlıkta geçerli bir iPhone kod imzalama anahtarı bulunamadı?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 75f9a78fdc7d15df217378491f016478cde369ff
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351107"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Neden iOS derlemem başarısız oluyor: Anahtarlıkta geçerli bir iPhone kod imzalama anahtarı bulunamadı?

## <a name="cause-of-the-error"></a>Hatanın nedeni
Bu hata iletisini söz konusu proje kod imzalama için geçerli kimlik bilgileri ararken gerçekleşir ancak bunları bulunamıyor. Kod imzalama, test ve dağıtım fiziksel iOS cihazlar için gereklidir; yanı sıra geçici & Uygulama derlemeleri depolayın. 


### <a name="provisioning-devices"></a>Cihaz sağlama
Aşağıdaki kılavuzda bir iOS cihazı önce hazırlamadıysanız tam adım adım işlemi boyunca sürer: [cihaz sağlama Kılavuzu](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>İOS simülatörü kullanılırken hata

> [!NOTE]
> Bu sorun, Visual Studio için Xamarin son sürümleri giderilmiştir. Ancak, yazılımın en son sürümüne bağımlı gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) , tam sürüm oluşturma bilgilerini ve tam derleme günlük çıkışı.


Kod imzalama için simülatör Entitlements.plist derlemeleri eklemek için bir Xamarin.Forms şablonunda iOS projesi neden Xamarin.Visual Studio 3.11 bir hata oluştu; etkili bir şekilde simülatör'ü kullanarak test engelliyor.

### <a name="how-to-fix"></a>Nasıl düzeltileceğini
Geçici çözüm bu sorunu ortadan kaldırarak olabilir `<CodesignEntitlements>` hata ayıklama bayrağı .csproj dosyasında oluşturur. Bunu şu şekilde yapabilirsiniz:

*Uyarı: denemeden önce dosyalarınızı yedekleme için iyi bir fikirdir için .csproj dosyalarının hatalarını, proje bozulabilir.*

1. Çözüm bölmesinde iOS projesine sağ tıklayın ve seçin **projeyi**
2. Projeyi tekrar sağ tıklayın ve seçin **[ProjeAdı] .csproj Düzenle**
3. Hata ayıklama PropertyGroups bulun, şuna bayrağı ile başlamanız gerekir:
   - Hata ayıklama: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Sürüm: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. Her simülatör kullanan derlemeler, silin veya açıklama olarak aşağıdaki özelliği: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Projeyi yeniden yükleyin ve simülatöre dağıtılamadı mümkün olması gerekir.

### <a name="next-steps"></a>Sonraki Adımlar
Bizimle iletişim kurun ya da Yukarıdaki bilgilerin kullanan daha sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [Xamarin için hangi destek seçenekleri mevcuttur?](~/cross-platform/troubleshooting/support-options.md) önerileri olan iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Gerekirse yeni hata bildirin. 
