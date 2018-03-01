---
title: "Xamarin.iOS analiz kuralları"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 7cf627f369b666bb54d0f512dc1361d2a685a057
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS analiz kuralları


## <a name="a-namexia0001xia0001-disabledlinkerrule"></a><a name="XIA0001"/>XIA0001: DisabledLinkerRule

- **Sorun:** bağlayıcı aygıt hata ayıklama modu için devre dışı bırakıldı.
- **Düzeltme:** herhangi beklenmeyen durumları önlemek için bağlayıcı ile kodunuzu çalıştırmak denemelisiniz.
Bunu ayarlamak için projeye Git > iOS Yapı > bağlayıcı davranışı.

## <a name="a-namexia0002xia0002-testcloudagentreleaserule"></a><a name="XIA0002"/>XIA0002: TestCloudAgentReleaseRule

- **Sorun:** Test bulut Aracısı'nı başlatma uygulama derlemeleri özel API kullandıkları gönderildiğinde, Apple tarafından reddedilir.
- **Düzeltme:** Ekle veya #if gerekli düzeltme ve kodu tanımlar.

## <a name="a-namexia0003xia0003-ipadebugbuildsrule"></a><a name="XIA0003"/>XIA0003: IPADebugBuildsRule

- **Sorun:** imzalama anahtarlarının Geliştirici kullanan hata ayıklama yapılandırmasını yalnızca şimdi Yayımlama Sihirbazı'nı kullanan dağıtım için gerektiği gibi bir IPA üretmemelidir.
- **Düzeltme:** proje seçenekleri IPA derleme için hata ayıklama yapılandırmasını devre dışı bırakın.

## <a name="a-namexia0004xia0004-missing64bitsupportrule"></a><a name="XIA0004"/>XIA0004: Missing64BitSupportRule

- **Sorun:** desteklenen mimarisi "yayın | Aygıt"64-bit ARM64 eksik uyumlu değil. Apple AppStore 32 bit yalnızca iOS uygulamaları kabul etmiyor gibi bir sorun budur.
- **Düzeltme:** çift iOS projenizi tıklatın, derleme için Git > iOS oluşturma ve desteklenen mimariler ARM64 sahip şekilde değiştirin.

## <a name="a-namexia0005xia0005-float32rule"></a><a name="XIA0005"/>XIA0005: Float32Rule

- **Sorun:** float32 seçeneği kullanılarak değil (--Uygulama Nesne Ağacı seçenekleri = O float32 =) maliyet, özel olarak mobil Cihazınızda yüklü bir miktar performans çift duyarlık matematik sorunlarında daha yavaş olduğu yol açar. Precision ve büyük olasılıkla, uyumluluk etkiler, bu seçeneği etkinleştirmek için .NET çift duyarlıklı dahili olarak, hatta float için kullandığını unutmayın.
- **Düzeltme:** çift iOS projenizi tıklatın, derleme için Git > iOS oluşturma ve işaretini "tüm 32-bit float 64-bit float işlemleri".
