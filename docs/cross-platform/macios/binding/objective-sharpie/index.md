---
title: "Amaç Sharpie"
description: "Bu bölümde hedefi Sharpie, Xamarin'ın komut satırı aracı bir Objective-C kitaplığına bağlama oluşturma işlemini otomatikleştirmek için kullanılan bir giriş sağlar"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 08796d38301ba9c6ae15c3ddfe700b1cd2166ace
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="objective-sharpie"></a>Amaç Sharpie

_Bu bölümde hedefi Sharpie, Xamarin'ın komut satırı aracı bir Objective-C kitaplığına bağlama oluşturma işlemini otomatikleştirmek için kullanılan bir giriş sağlar_

- [Genel Bakış](#overview) & [geçmişi](#history)
- [Başlarken](get-started.md)
- [Araçlar ve Komutlar](tools.md)
- [Özellikler](platform/index.md)
- [Örnekler](examples/index.md)
- [İzlenecek tam yol](~/ios/platform/binding-objective-c/walkthrough.md)
- [Sürüm Geçmişi](releases.md)

## <a name="overview"></a>Genel Bakış

Amaç Sharpie bağlaması ilk geçişi bootstrap yardımcı olmak için bir komut satırı aracıdır.
Genel API uygulamasına eşlemek için yerel kitaplık üstbilgi dosyaları ayrıştırarak çalışır [tanımı bağlama](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (daha önce el ile yapıldığı bir işlem).

Böylece bağlama olarak tam ve kapsamlı mümkün olduğunca hedefi Sharpie Clang ayrıştırma üstbilgi dosyaları kullanır. Bu, zaman ve çaba kalite bağlama üretmek için gereken önemli ölçüde azaltabilir.

> [!IMPORTANT]
> Amaç Sharpie Gelişmiş bilgi Objective-C (ve uzantılarının, C) ile deneyimli Xamarin geliştiriciler için bir araçtır. Objective-C Kitaplığı bağlamak denemeden önce yerel kitaplığı komut satırı (ve yerel kitaplığı nasıl çalıştığını iyi anlamış) oluşturmak düz bilgiye sahip olmalıdır.

## <a name="history"></a>Geçmiş

Biz alınan gelişen ve hedefi Sharpie Xamarin son üç yıl için dahili olarak kullanarak. Bir güçlü bir kanıtı olarak hedefi Sharpie gücünü, olarak API'leri Xamarin.iOS ve Xamarin.Mac iOS 8'de, Mac OS X 10.10 sürümünü, bu yana kullanılmaya ve watchOS 2.0 tamamen hedefi Sharpie ile ten. Xamarin yoğun üzerinde hedefi Sharpie dahili olarak kendi ürünleri derleme için kullanır.

Ancak, hedefi Sharpie Objective-C ve C, komut satırında clang derleyici kullanmayı ve genellikle nasıl yerel kitaplıkları araya Gelişmiş bilgisi gerektiren bir çok Gelişmiş aracıdır. Bu yüksek çubuğu nedeniyle Biz bu GUI sahip Keçeli Sihirbazı yanlış beklentilerini ayarlar ve bu nedenle, hedefi Sharpie şu anda yalnızca bir komut satırı aracı olarak kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Amaç Sharpie indirme](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [İzlenecek yol: bir Objective-C Kitaplığı bağlama](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C Kitaplıklarını Bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Bağlama Ayrıntıları](~/cross-platform/macios/binding/overview.md)
- [Bağlama türü Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C geliştiriciler için Xamarin](~/ios/get-started/objective-c-developers/index.md)
