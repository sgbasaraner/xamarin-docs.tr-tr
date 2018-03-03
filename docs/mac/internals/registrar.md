---
title: Xamarin.Mac registrar
description: "Bu belgede amacını Xamarin.Mac kayıt ve kendi farklı kullanım yapılandırmaları açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 8d5061850a4cb73a81e1bf4c93583eb5b6eeefec
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac registrar

_Bu belgede amacını Xamarin.Mac kayıt ve kendi farklı kullanım yapılandırmaları açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Xamarin.Mac arasında köprü yönetilen (.NET) world ve Cocoa'nın çalışma zamanı, yönetilmeyen Objective-C sınıflar çağırın ve olaylar gerçekleştiğinde geri çağrılması yönetilen sınıflar izin vererek arasındaki boşluk. Bu "Sihirli" işlemini gerçekleştirmek için gereken iş kaydedici tarafından ele alınır ve genel olarak, gizlenir.

Bu kayıt, özellikle uygulama başlama süresi, performans etkileri vardır ve bir bit "başlık altında" neler anlama bazı durumlarda yararlı olabilir.

## <a name="configurations"></a>Yapılandırmalar

Temelde başlangıçta kayıt şirketinin iş iki catagories ayrılabilir:

- Her yönetilen sınıf olanlar NSObject türetme taramak ve Objective-C çalışma zamanı için açığa çıkarılması öğelerin listesini toplayabilirsiniz.
- Bu bilgiler Objective-C çalışma zamanı ile kaydedin.

Üç farklı kayıt yapılandırmaları zaman içinde farklı kullanım durumları karşılamak üzere oluşturulmuş. Her farklı bir yapı vardır ve zaman sonuçları çalıştırın:

- **Dinamik kayıt** – başlatma sırasında .NET yansıma tarama yüklenen her türü, ilgili öğeleri listesini belirlemek için kullanın ve yerel çalışma zamanı bildirin. Bu seçenek yapı süresini sıfır ekler, ancak (kadar birden çok saniye olarak) başlatma sırasında işlem oldukça pahalıdır.
- **Statik kayıt** – yapı sırasında işlem kayıtlı olması ve kayıt işlemek için Objective-C kodunu oluşturmak için öğeleri kümesi. Bu kod, hızlı bir şekilde tüm öğeleri kaydetmek için başlatma sırasında çağrılır. Yapı ancak can önemli bir duraklama önemli miktarda zaman uygulama başından kesme ekler.
- **"Kısmi" statik** – her ikisi de avantajları çoğunu sunan yeni bir "karma" yaklaşım. Dışarı itibaren **Xamarin.Mac.dll** kayıtlarını işlemek ve giriş bağlamak için önceden hesaplanan bir kitaplık kaydetme, sabittir. Yansıma kullanıcı kitaplıkları işlemek, ancak platform bağlamaları bu genellikle yerine hızlı olduğunu çok daha az türleri kullanıcı kitaplıklar olarak dışarı aktarmak için kullanın. Bir neglectable süresi etkisi yapı ve dinamik "maliyetini" büyük bir çoğunluğu azaltır.

Bugün kısmi statik hata ayıklama yapılandırmasını varsayılandır ve statik yayın yapılandırmaları için varsayılandır.

Bazı senaryolar vardır:

- Başlatma NSObject türetilen sınıflar ile yüklenen eklentileri
- Dinamik sınıf örneklerini NSObject türetme oluşturuldu

Burada kayıt şirketi bazı türünü başlangıç kaydetmek için ihtiyaç duyduğu bilmeniz alamıyor. `ObjCRuntime.Runtime.RegisterAssembly` Yöntemi kayıt şirketi dikkate alınacak ek türleri olduğunu bildirmek için sağlanır.
