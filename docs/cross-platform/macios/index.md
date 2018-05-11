---
title: Apple Platform (iOS ve Mac)
description: Bu bölümdeki kod Xamarin.iOS ve Xamarin.Mac projeleriniz arasında paylaşmak için stratejileri kapsar.
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3c01ff4af699dd0374729b638470d1ef34aa7022
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="apple-platform-ios-and-mac"></a>Apple Platform (iOS ve Mac)

_Bu bölümdeki kod Xamarin.iOS ve Xamarin.Mac projeleriniz arasında paylaşmak için stratejileri kapsar._

## <a name="code-sharing"></a>Kod paylaşımını

Kodunuzun için kullanıcı arabirimi öğeleri paylaşmak için en iyi yöntem sahip öğeleri iOS ve Mac arasında hala kullanımını kodudur [taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md).

Bazı kullanıcı arabirimi iş yapmak için gereken kodu için ve paylaşmak, istediğiniz henüz, kullanmanız gereken [paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md) tek bir projede paylaşma ve Mac ve başvurulduğunda iOS ile derlenmiş sağlamak için kodu yerleştirin izin.

##  <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

İOS ve Mac projeleri için birleşik API aynı ad alanları çerçeveyi kullanır, böylece aynı kod dosyası için sorunsuz kod paylaşımını her iki platform üzerinde kullanılabilir. Ayrıca, 32 ve 64 bit derlemeleri sağlar. Birleşik API şablonu varsayılan erken 2015 tarihinden itibaren kaldırıldı ve tüm yeni projeler için - önerilen *yalnızca* Unified API projeleri uygulama mağazasında gönderilen.

### <a name="classic-apis"></a>Klasik API'leri

> [!NOTE]
> **Klasik profili kullanımdan:** yeni platformlar Xamarin.iOS içinde eklendikçe biz kademeli olarak Klasik profilinden (monotouch.dll) özellikleri alanı onaylanamadı başladılar. Örneğin, NRC olmayan (yeni ref-sayısı) seçeneği kaldırıldı. NRC tüm birleştirilmiş uygulamalar için her zaman etkinleştirildi (hiçbir zaman bir seçenek, yani olmayan NRC) ve bilinen bir sorun vardır. Gelecek sürümlerde Boehm atık toplayıcı kullanma seçeneğini kaldırır. Ayrıca, bu birleşik uygulamalara hiçbir zaman kullanılabilen bir seçenektir. Tam temizleme Klasik destek Xamarin.iOS 10.0 sürümü ile sonbaharda 2016 için zamanlandı.

Yerel çerçeveler ya da sahip olduğundan Xamarin.Mac API'leri ve özgün (Birleşik olmayan) Xamarin.iOS kod paylaşımını daha zor hale `MonoTouch.` veya `MonoMac.` ad alanı öneklerini.  Bizim sağladığımız ekleyerek kod paylaşmak geliştiricilerinin sağlayan bazı boş ad alanları `using` MonoMac ve MonoTouch ad alanları aynı dosyaya, ancak bu başvuru deyimleri biraz çirkin oluştu. Klasik API yalnızca dahili olarak dağıtılan eski uygulamalarda kullanılmaya devam (Birleşik API için yükseltme önerilir).


### <a name="updating-from-classic-to-the-unified-api"></a>Birleşik API Klasikten güncelleştiriliyor

Klasik herhangi bir uygulamadan Unified API güncelleştirmek için ayrıntılı yönergeler vardır.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Objective-C Kitaplıklarını Bağlama](binding/index.md)

Xamarin yerel kitaplıkları uygulamalarınızla bağlamalarla getirmenize olanak tanır. Bu bölümde açıklanmaktadır:

- bağlamaları nasıl çalıştığını,
- el ile Objective-C kodunu Xamarin getirmenize olanak sağlayan bir bağlama proje oluşturma ve
- nasıl kullanılacağını bizim **hedefi Sharpie** işlemini otomatikleştirmek için aracı.

## <a name="native-referencesnative-referencesmd"></a>[Yerel Başvurular](native-references.md)



##  <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS yerel türler](nativetypes.md)

32 ve 64 bit kodundan saydam C# ve F # desteklemek için yeni veri türleri sunuyoruz.   Buradan bilgi edinebilirsiniz.

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[32 ve 64 bit uygulamalar oluşturma](32-and-64/index.md)

32 ve 64 bit uygulamalarını desteklemek için bilmeniz gerekenler.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](native-types-cross-platform.md)

Bu makalede, yeni iOS Unified API yerel türler kullanma yer almaktadır (`nint`, `nuint`, `nfloat`) burada kod paylaşılan Android veya Windows Phone işletim sistemleri gibi iOS olmayan aygıtlarla platformlar arası uygulamasında.
Yerel türler ne zaman kullanılmalı içine bilgiler sağlar ve yeni türü platformlar arası koduyla burada kullanılmalıdır durumlarda birkaç olası çözümleri sağlar.


## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient Yığını ve SSL/TLS Uygulama Seçicisi](http-stack.md)

Yeni HttpClient Yığın Seçicisi Xamarin.iOS, Xamarin.tvOS ve Xamarin.Mac uygulamanızda kullanmak için hangi HttpClient uygulama denetler. Artık iOS'ın kullanan bir uygulama için tvOS'ın veya OS x yerel aktarımları geçebilirsiniz (`NSUrlSession` veya `CFNetwork` işletim sistemi bağlı olarak).

SSL (Güvenli Yuva Katmanı) ve onun ardıl TLS (Aktarım Katmanı Güvenliği), HTTP ve diğer ağ bağlantıları üzerinden için destek sağlayan `System.Net.Security.SslStream`. Yeni SSL/TLS uygulaması derleme seçeneği Mono'nın kendi TLS yığını ve Apple'nın TLS yığını Mac ve iOS mevcut tarafından desteklenen bir geçiş yapar.
