---
title: Xamarin.Mac – ilgili belgeler
description: "Bu belge Xamarin.Mac geliştiriciler için ilgili belgelere bağlantılar sağlar: Xamarin.iOS belgeler, Apple'nın Mac Geliştirme Merkezi ve kullanıcı arabirimleri Xamarin.Mac ile nasıl oluşturulacağını açıklayan çeşitli kılavuzları."
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 1bef756d89a92d082bd5ee29e18047bc4bed2498
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792534"
---
# <a name="xamarinmac--related-documentation"></a>Xamarin.Mac – ilgili belgeler

Ek olarak Mac bölümünü [developer.xamarin.com](~/mac/get-started/index.md) Xamarin.Mac sorularını Yardım olabilir belgelerin üç harika kaynağı vardır:

- [**Xamarin.iOS belgelerine** ](~/ios/get-started/index.md) - birçok API'leri (öncelikle AppKit/Uıkit dışında) iOS ve macOS sürümleri arasında yalnızca küçük farklılıklar vardır. Bir adı verilen bir iOS API sahip olduğu bazı durumlarda `UIFoo`, adlı benzer bir API `NSFoo` macOS üzerinde bulunabilir. Bu örnekler genellikle C# dilinde önceden olması gerekir.

- **Apple'nın [Mac Dev Center](https://developer.apple.com/devcenter/mac/)**  - Objective-C çağırmak için hangi API'leri örneği dönüştürülebilir C# için kolay bir şekilde birçok kez. Bkz: [anlama Mac API'leri](~/mac/app-fundamentals/mac-apis.md) bunun nasıl yapılacağı hakkında ayrıntılı bilgi için.

- [**Yığın Taşması** ](http://stackoverflow.com/) -basit bir gibi soruları kapatmak için harika bir kaynak ["nasıl t otomatik bir NSOutlineView tüm düğümleri genişletin"](http://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). Bu örnekler genellikle Objective-C ve C# dönüştürülmesi gerekiyorsa olur, ancak bir alt kümesini yanıtlar C# yok.

## <a name="user-interface"></a>Kullanıcı Arabirimi

Kullanıcı arabirimi Xamarin.Mac uygulama, geliştiriciler C# ve .NET ile çalışma erişimi aynı olduğunda, çalışan bir geliştirici denetleyen *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir olduğundan, geliştirici Xcode'nın kullanabilir _arabirimi Oluşturucu_ ve bir uygulamanın kullanıcı arabirimleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Aşağıda listelenen kılavuzları Xamarin.Mac uygulama macOS öğeleri ile çalışma hakkında ayrıntılı bilgi sağlar:

- [Windows](~/mac/user-interface/window.md)
- [İletişim Kutuları](~/mac/user-interface/dialog.md)
- [Uyarılar](~/mac/user-interface/alert.md)
- [Menüler](~/mac/user-interface/menu.md)
- [Araç Çubukları](~/mac/user-interface/toolbar.md)
- [Tablo Görünümleri](~/mac/user-interface/table-view.md)
- [Anahat Görünümleri](~/mac/user-interface/outline-view.md)
- [Kaynak Listeleri](~/mac/user-interface/source-list.md)
