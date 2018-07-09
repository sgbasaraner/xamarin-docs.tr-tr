---
title: Bağlamalar hedef Sharpie ile oluşturma
description: Bu bölümde hedefi Sharpie, Objective-C kitaplığını bağlama oluşturma işlemini otomatik hale getirmek için kullanılan Xamarin'in komut satırı aracını giriş sağlar.
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 53fcbbc408ae147405a3285d9391457051d6e16e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854804"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Bağlamalar hedef Sharpie ile oluşturma

_Bu bölümde hedefi Sharpie, Objective-C kitaplığını bağlama oluşturma işlemini otomatik hale getirmek için kullanılan Xamarin'in komut satırı aracını giriş sağlar._

- [Genel Bakış](#overview) & [geçmişi](#history)
- [Başlarken](get-started.md)
- [Araçlar ve Komutlar](tools.md)
- [Özellikler](platform/index.md)
- [Örnekler](examples/index.md)
- [İzlenecek tam yol](~/ios/platform/binding-objective-c/walkthrough.md)
- [Sürüm Geçmişi](releases.md)

## <a name="overview"></a>Genel Bakış

Amaç Sharpie bağlama ilk geçişinde bootstrap yardımcı olmak için bir komut satırı aracıdır.
Genel API uygulamasına eşlemek için yerel bir kitaplığı üstbilgi dosyaları ayrıştırarak çalıştığını [tanımı bağlama](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (daha önce el ile yapıldığı bir işlem).

Tam ve mümkün olduğunca tam olarak bağlama hedefi Sharpie Clang ayrıştırma üst bilgi dosyaları, kullanır. Bu, zaman ve çaba, kalite bağlama oluşturmak için gereken önemli ölçüde azaltabilir.

> [!IMPORTANT]
> Amaç Sharpie Gelişmiş bilgi Objective-c (ve uzantılarının, C) ile deneyimli bir Xamarin geliştiricilerine yönelik bir araçtır. Objective-C kitaplığını bağlama çalışmadan önce komut satırı (ve yerel kitaplığı nasıl çalıştığını iyi anlamış) yerel kitaplık oluşturmak nasıl sağlam bilgiye sahip olmalıdır.

## <a name="history"></a>Geçmiş

Biz alınan gelişen ve hedefi Sharpie Xamarin son üç yıl için dahili olarak kullanarak. Hedefi Sharpie gücünü bir topluluğun, API'leri itibaren iOS 8'de, Mac OS X 10.10 sürümünü, Xamarin.iOS ve Xamarin.Mac sunulan ve tamamen hedefi Sharpie ile watchOS 2.0 önyüklenemez. Xamarin yoğun üzerinde hedefi Sharpie dahili olarak kendi ürünleri oluşturmak için kullanır.

Ancak, hedefi Sharpie Objective-C ve C, clang derleyici komut satırında kullanmak nasıl ve ne genellikle yerel kitaplıkları araya Gelişmiş bilgi gerektiren bir çok Gelişmiş aracıdır. Bu yüksek çubuğu nedeniyle biz sahip bir GUI düşünmüştür. Sihirbazı ayarlar yanlış beklentileri ve bu nedenle, hedefi Sharpie şu anda yalnızca bir komut satırı aracı olarak kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Amaç Sharpie indirme](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [İzlenecek yol: bir Objective-C kitaplığını bağlama](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C Kitaplıklarını Bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Bağlama Ayrıntıları](~/cross-platform/macios/binding/overview.md)
- [Bağlama türleri Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C geliştiricileri için Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
