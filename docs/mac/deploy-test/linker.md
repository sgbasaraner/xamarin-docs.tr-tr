---
title: "Xamarin.Mac bağlayıcı seçenekleri"
description: "Bağlama, kullanılmayan kodu kaldırarak, uygulamanızın küçültür olanak veren bir güçlü iyileştirme aracıdır."
ms.topic: article
ms.prod: xamarin
ms.assetid: F03176C3-F8D4-4DE8-870C-7F27D8CE525A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: bee5f86682048fcd72d2212706c188c894eb148a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-linker-options"></a>Xamarin.Mac bağlayıcı seçenekleri

_Bağlama, kullanılmayan kodu kaldırarak, uygulamanızın küçültür olanak veren bir güçlü iyileştirme aracıdır._

## <a name="overview"></a>Genel Bakış

Temel [hedef Framework](~/mac/platform/target-framework.md) projenizi kullanan, bağlayıcı seçenekleri sınırlı olabilir. Bu bağlama, uygulamanız tarafından kullanılan her türünde bir nesne grafiğinin oluşturulmasını gerektirir ve bu System.Configuration nedeniyle tam (veya desteklenmiyor) mümkün değildir due için olabilir.

Dört seçenek vardır:

- **Hiçbiri** – tüm bağlama devre dışı bırakın. Hata ayıklama yapılandırmasını da Modern ve tüm yapılandırmaları tam varsayılan.
- **SDK** – kullanıcı derlemeleri hariç tüm SDK derleme bağlar. Modern sürüm yapılandırmasında varsayılan. Tam kullanılamaz.
- **Tam** – tüm derlemelerde bağlantı. Bu kullanıcı kodunun bağlayıcı güvenli, bkz: olmasını gerektirir [notları](~/ios/deploy-test/linker.md) daha fazla bilgi için. Tam kullanılamaz.
- **Platform** – yalnızca Xamarin.Mac.dll bağlantı. Ayrıntılar için aşağıya bakın.

## <a name="platform-linking"></a>Platform bağlama

Tam kullanan uygulamalar bağlama [hedef Framework](~/mac/platform/target-framework.md) genellikle güvenli değildir ancak bağlama çok sınırlı bir form olduğu gerekli senaryolar sayısı.

Örneğin, uygulama mağazası macOS için gönderilen uygulamaları kullanım dışı API'leri bazıları Xamarin.Mac bağlamaları içerir (örneğin, QTKit), bir dizi başvuru içermemelidir. Bir uygulama çağırmaz olsa bile bu bağlantıları çağırma SDK'ın mevcut olur ve reddedilir.

Platform bağlama, uygulama ve BCL bağlayıcı güvenli ve kullanılmayan kod Xamarin.Mac.dll kaldırmayı varsayar. 

Xamarin.Mac.dll türlerinde yansıtma olmayan herhangi bir uygulama kaldırma gereksiz türlerinin bir ikincil başlangıç iyileştirmeden görürsünüz.

Modern uygulamanın daha güçlü SDK seçeneği kullanabilirsiniz gibi platform bağlama genellikle yalnızca tam hedef Framework'ü kullanan uygulamalar için yararlıdır.

## <a name="setting-the-linker-configuration"></a>Bağlayıcı yapılandırması

Xamarin.Mac projesi için bağlayıcı yapılandırmasını değiştirmek için aşağıdakileri yapın:

1. Xamarin.Mac proje Mac için Visual Studio'da açın
2. İçinde **Çözüm Gezgini**, proje dosyasına çift tıklayarak açın **proje seçenekleri** iletişim kutusu.
3. Gelen **Mac yapı** sekmesinde, türünü seçin **bağlayıcı davranışı** uygulamanızın gereksinimlerine uygun:

  ![Kullanmak için hangi bağlayıcı davranışını seçin](linker-images/link-behavior.png "kullanmak için hangi bağlayıcı davranışını seçin")

4. Platform için tam bir hedef çerçeveyi bağlama IDE içinde bir güncelleştirme kadar görünmez. O zamana kadar eklemek `--linkplatform` için **ek mmp bağımsız değişkenleri** yerine.
5. Tıklatın **Tamam** yaptığınız değişiklikleri kaydetmek için düğmesi.


## <a name="related-links"></a>İlgili bağlantılar

- [İos'ta bağlama](~/ios/deploy-test/linker.md)
