---
title: Xamarin.iOS içinde katıştırılmış çerçeve
description: Bu belge, bir Xamarin.iOS uygulaması içinde katıştırılmış çerçeve kod paylaşmak açıklar. Bu mtouch aracı veya yerel başvuruları ile yapılabilir.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e42f0940fe3fc132c9d381907aad5afbe474c4ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787298"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Xamarin.iOS içinde katıştırılmış çerçeve

_Bu belgede, uygulama geliştiricilerin kendi uygulamalarında kullanıcı çerçeveler nasıl eklenebilir açıklanmaktadır._

İOS 8.0 Apple kod uygulama uzantıları xcode'da ana uygulama arasında paylaşmak için katıştırılmış bir çerçeve oluşturmak mümkün yapılan.

Xamarin.iOS 9.0 Xamarin.iOS uygulamaları (Xcode ile oluşturulan) Bu katıştırılmış çerçeveleri tüketimi için destek ekler. *İçinde **değil** herhangi bir Xamarin.iOS projeleri türünden katıştırılmış çerçeveler oluşturmak için yalnızca var olan yerel (Objective-C) çerçeveleri kullanmak mümkün olabilir.*

Xamarin.iOS içinde çerçeve kullanmak için iki yolu vardır:

- Projenin ek mtouch değişkenlerinde aşağıdakileri ekleyerek mtouch araca, framework geçirmek **iOS yapı** seçenekleri:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Bu, her proje yapılandırması için ayarlanması gerekir.

- Bağlam menüsünden yerel başvurular ekleyin

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Proje ve Gözat üzerinde yerel başvurular eklemek için sağ tıklatın

![](embedded-frameworks-images/xam-native-refs.png "Ekle yerel başvuruları Visual Studio'daki Mac için seçin.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Proje ve Gözat üzerinde yerel başvurular eklemek için sağ tıklatın

![](embedded-frameworks-images/vs-native-refs.png "Visual Studio'da Ekle yerel başvuruları seçin")

-----

  Bu, tüm yapılandırmalar için çalışır.

Gelecekteki sürümlerinde Mac için Visual Studio ve Xamarin araçları Visual Studio IDE içinden gelen çerçeveleri (proje dosyalarını el ile düzenleme olmadan) kullanmak mümkün olacaktır.

Birkaç örnek projelerinden bulunabilir [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Sınırlamalar

- İçinde yalnızca katıştırılmış çerçeveleri desteklenir [Unified](~/cross-platform/macios/unified/index.md) projeleri.
- Katıştırılmış çerçeveleri yalnızca desteklenen dağıtım hedefi ile projelerinde en az iOS 8.0.
- Uzantı katıştırılmış bir çerçeve gerektiriyorsa, kapsayıcı uygulama de framework uygulama paketine dahil edilmeyen framework başvuru, aksi durumda olmalıdır.

## <a name="the-mono-runtime"></a>Mono çalışma zamanı

Dahili olarak Xamarin.iOS Mono çalışma zamanı statik olarak her bir uzantı ve kapsayıcı uygulama halinde bağlama yerine bir çerçeve olarak Mono çalışma zamanı ile bağlamak için bu özelliği yararlanır.

Bu, kapsayıcı uygulama Unified uygulama, uzantılar içerir ve hedef dağıtım iOS 8.0 veya daha yüksek ise otomatik olarak gerçekleştirilir.

Kendisine başvuran yalnızca bir uygulama ise bir çerçeve kullanmak için bir boyut cezası olduğundan uygulamaları uzantısız hala Mono çalışma zamanı ile statik olarak, bağlantı içerir.

Bu davranış uygulama geliştiricisi tarafından projenin iOS derleme seçenekleri bir ek mtouch bağımsız değişken olarak aşağıdakileri ekleyerek geçersiz kılınabilir:

- `--mono:static`: Mono çalışma zamanı ile statik olarak bağlar.
- `--mono:framework`: Bir çerçeve Mono zamanıyla bağlantılar.

Uygulamaları uzantısız için bile bir çerçeve olarak Mono çalışma zamanı ile bağlama için bir Apple yürütülebilir dosyayı zorlar boyut kısıtlamaları aşmak için yürütülebilir boyutunu azaltmak için bir senaryodur. (Xamarin.iOS 8.12 ancak kaldırmalıdır sürümler arasında ve uygulamalar arasında bile değiştikçe) başvuru için Mono çalışma zamanı mimarisi başına yaklaşık 1,7 MB ekler. Mono framework yaklaşık 2.3 uygulamanın bağlantısı yapma başına MB tek mimarisi uygulamasının tüm uzantılar olmadan anlamına gelir, mimarisi ekler bir çerçeve yürütülebilir dosya ~1.7MB tarafından Küçült ancak ~2.3MB framework eklemek gibi Mono çalışma zamanı ile sonuçlanır bir ~0.6MB daha büyük uygulama birlikte içinde.

