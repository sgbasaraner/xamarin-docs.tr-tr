---
title: Xamarin.iOS analiz kuralları
description: Bu belgede daha/better-optimized ayarları olup olmadığını belirlemek için Xamarin.iOS projesi ayarlarını denetleyin çözümleme kurallarını açıklar.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 25d2936f70981ed239626193ba6e4993f1378108
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788904"
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS analiz kuralları

Xamarin.iOS analiz, daha iyi/daha çok en iyi duruma getirilmiş ayarları kullanılabilir olup olmadığını belirlemek için proje ayarlarınızı denetleyin kurallar kümesidir.

Çözümleme kurallarını olası geliştirmeleri önceden bulmak ve geliştirme zamandan tasarruf için mümkün olduğunca sık çalıştırın.

Visual Studio Mac'ın menüsü kuralları çalıştırmak için seçin **Proje > Kod Analizi çalıştırmak**.

> [!NOTE]
> Xamarin.iOS analiz yalnızca şu anda seçili yapılandırmanızı üzerinde çalışır. Hata ayıklama için Aracı'nı çalıştıran önerilir **ve** yayın yapılandırmaları.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: DisabledLinkerRule

- **Sorun:** bağlayıcı aygıt hata ayıklama modu için devre dışı bırakıldı.
- **Düzeltme:** herhangi beklenmeyen durumları önlemek için bağlayıcı ile kodunuzu çalıştırmak denemelisiniz.
Bunu ayarlamak için projeye Git > iOS Yapı > bağlayıcı davranışı.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: TestCloudAgentReleaseRule

- **Sorun:** Test bulut Aracısı'nı başlatma uygulama derlemeleri özel API kullandıkları gönderildiğinde, Apple tarafından reddedilir.
- **Düzeltme:** Ekle veya #if gerekli düzeltme ve kodu tanımlar.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: IPADebugBuildsRule

- **Sorun:** imzalama anahtarlarının Geliştirici kullanan hata ayıklama yapılandırmasını yalnızca şimdi Yayımlama Sihirbazı'nı kullanan dağıtım için gerektiği gibi bir IPA üretmemelidir.
- **Düzeltme:** proje seçenekleri IPA derleme için hata ayıklama yapılandırmasını devre dışı bırakın.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **Sorun:** desteklenen mimarisi "yayın | Aygıt"64-bit ARM64 eksik uyumlu değil. Apple AppStore 32 bit yalnızca iOS uygulamaları kabul etmiyor gibi bir sorun budur.
- **Düzeltme:** çift iOS projenizi tıklatın, derleme için Git > iOS oluşturma ve desteklenen mimariler ARM64 sahip şekilde değiştirin.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **Sorun:** float32 seçeneği kullanılarak değil (--Uygulama Nesne Ağacı seçenekleri = O float32 =) maliyet, özellikle mobil Cihazınızda yüklü bir miktar performans çift duyarlık matematik sorunlarında daha yavaş olduğu yol açar. Precision ve büyük olasılıkla, uyumluluk etkiler, bu seçeneği etkinleştirmek için .NET çift duyarlıklı dahili olarak, hatta float için kullandığını unutmayın.
- **Düzeltme:** çift iOS projenizi tıklatın, derleme için Git > iOS oluşturma ve işaretini "tüm 32-bit float 64-bit float işlemleri".

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: HttpClientAvoidManaged

- **Sorun:** yerine yönetilen bir daha iyi performans için daha küçük yürütülebilir boyutu, yerel HttpClient işleyici kullanmanız önerilir ve yeni standartları kolayca desteklemek için.
- **Düzeltme:** çift iOS projenizi tıklatın, derleme için Git > iOS oluşturma ve HttpClient uygulamasını NSUrlSession (iOS 7 +) ya da CFNetwork için iOS 7 önceki sürümü desteklemek için değiştirin.
