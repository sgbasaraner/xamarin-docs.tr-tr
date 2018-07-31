---
title: Xamarin.iOS içinde gömülü çerçeveler
description: Bu belge, bir Xamarin.iOS uygulaması içinde gömülü çerçeveler ile kod paylaşın açıklar. Bu, mtouch aracı veya yerel başvurular ile yapılabilir.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: cce5356fd1d3d9a5cf16370a4843c3541b00a7c0
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351440"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Xamarin.iOS içinde gömülü çerçeveler

_Bu belgede, uygulama geliştiricilerinin uygulamalarında kullanıcı çerçeveleri nasıl katıştırabilirsiniz açıklanmaktadır._

İOS 8.0 Apple, uygulama uzantıları ve xcode'da ana uygulama arasında kod paylaşmak için bir katıştırılmış taslağı oluşturmak mümkün kıldı.

Xamarin.iOS 9.0 Xamarin.iOS uygulamalarında kullanma (Xcode ile oluşturulan) bu gömülü çerçeveler için destek ekler. *Götürür **değil** gömülü çerçeveler herhangi bir Xamarin.iOS projeleri türünden oluşturmak için yalnızca var olan yerel (Objective-C) çerçeveleri kullanmak mümkün olabilir.*

Xamarin.iOS işlemlerindeki çerçeveler kullanan iki yolu vardır:

- Diğer mtouch bağımsız değişkenleri projenin aşağıdakileri ekleyerek mtouch araca, framework geçirmek **iOS derleme** seçenekleri:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Bu, her bir proje yapılandırması için ayarlanması gerekir.

- Bağlam menüsünden yerel başvurular ekleme

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yerel başvurular eklemek için proje ve Gözat'a sağ tıklayın

![](embedded-frameworks-images/xam-native-refs.png "Mac için Visual Studio'da yerel başvurular ekleme seçin")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Yerel başvurular eklemek için proje ve Gözat'a sağ tıklayın

![](embedded-frameworks-images/vs-native-refs.png "Visual Studio'da yerel başvurular ekleme seçin")

-----

  Bu, tüm yapılandırmalar için çalışır.

Mac için Visual Studio ve Visual Studio için Xamarin araçları gelecek sürümleri (proje dosyalarını el ile düzenleme olmadan) IDE'nin içinden çerçevelerinden kullanmasını mümkün olacaktır.

Birkaç örnek projeler bulunabilir [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Sınırlamalar

- Gömülü çerçeveler de yalnızca desteklenmektedir [birleşik](~/cross-platform/macios/unified/index.md) projeleri.
- Gömülü çerçeveler yalnızca desteklenen bir dağıtım hedefi içeren projelerde en az iOS 8.0.
- Uzantı, katıştırılmış bir framework gerektiriyorsa, kapsayıcı uygulamasını da framework uygulama paketine dahil edilmemesi framework, başka bir başvuru olmalıdır.

## <a name="the-mono-runtime"></a>Mono çalışma zamanı

Dahili olarak Xamarin.iOS her uzantı ve kapsayıcı uygulamasını Mono çalışma zamanı statik bağlama yerine çerçeve, Mono çalışma zamanı ile bağlamak için bu özelliği yararlanır.

Bu, birleştirilmiş uygulama kapsayıcı uygulamasını, genişletmeler içeren, iOS 8.0 veya üzeri hedef dağıtım ise otomatik olarak gerçekleştirilir.

Kendisine başvuran yalnızca tek bir uygulama ise bir çerçeve kullanmak için bir boyut cezası olduğundan uygulamaları uzantısız hala Mono çalışma zamanı ile durağan bağlayacaksınız.

Bu davranış uygulama geliştiricisi tarafından aşağıdakileri ekleyerek projenin iOS derleme seçenekleri diğer mtouch bağımsız değişkeni olarak geçersiz kılınabilir:

- `--mono:static`: Mono çalışma zamanı ile statik olarak bağlar.
- `--mono:framework`: Bağlantılar çerçeve Mono çalışma zamanı ile.

Tüm boyutu kısıtlamaları, yürütülebilir dosyayı Apple zorlar yürütülebilir boyutunu azaltmak için çerçeve uzantıları olmayan uygulamalar için bile Mono çalışma zamanı ile bağlama için bir senaryodur. (Xamarin.iOS 8.12 ancak bu sürümler arasında ve hatta uygulamalar arasında değiştikçe) başvuru için Mono çalışma zamanı mimari başına yaklaşık 1.7 MB ekler. Mono framework ekler yaklaşık 2.3 uygulama bağlantısı yapma MB, herhangi bir uzantısı olmadan tek mimarisi uygulaması anlamına gelir, mimarisi, bir çerçeve yürütülebilir dosya tarafından ~1.7MB küçültme ancak ~2.3MB framework eklemek gibi Mono çalışma zamanı ile elde edilen bir ~0.6MB büyük uygulama birlikte içinde.

