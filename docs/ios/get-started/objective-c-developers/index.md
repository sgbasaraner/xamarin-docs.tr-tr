---
title: Objective-C geliştiricileri için Xamarin
description: Bu belge, Objective-C geliştiricileri için bir Xamarin.iOS açıklamasını sağlar. Bu, C# için Objective-C geçiş yapma, nasıl kullanılmak üzere C# bir Objective-C kitaplığını bağlama ve platformlar arası mobil uygulamanın nasıl oluşturulacağını açıklayan kılavuzları bağlar.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 574bd4c0b6bed639230dbfb6209eb78216e8e47b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351014"
---
# <a name="xamarin-for-objective-c-developers"></a>Objective-C geliştiricileri için Xamarin

İOS olmayan kullanıcı arabirimini taşımak hedefleyen geliştiriciler için bir yol, böylece kullanılabilir platform bağımsızlığı için C# kod Xamarin teklifler herhangi bir C# Xamarin.Android aracılığıyla Android ve Windows çeşitli türdeki dahil olmak üzere kullanılabilir. Yalnızca C# ile Xamarin kullanıyor ancak, mevcut becerilerini ve Objective-C kodunun yararlanamaz anlamına gelmez. Xamarin yerel iOS ve OS X platformunu bildiğiniz ve sevdiğiniz Uıkit, çekirdek animasyon, çekirdek Foundation ve temel grafikler gibi birkaç API'leri kullanıma sunduğundan, Objective-C bilerek, daha iyi bir Xamarin.iOS Geliştirici yapar. Aynı anda LINQ ve genel türler yanı sıra, zengin .NET native uygulamalarınızda kullanılmak üzere sınıf kütüphaneleri temel gibi özellikler dahil olmak üzere C# dili'nın gücünü edinin.

Ayrıca, Xamarin, varolan bir Objective-C yararlanmanıza olanak tanır varlıklar bir teknoloji aracılığıyla bağlamaları bildirin. Yalnızca Objective-C içinde statik kitaplık oluşturma ve bunu C# ' tan bir bağlama aracılığıyla Aşağıdaki diyagramda gösterildiği gibi kullanıma:

 [![](images/01-bindings.png "Objective-c C# ' tan bir bağlama aracılığıyla kullanıma sunulan bir statik kitaplık")](images/01-bindings.png#lightbox)

Bu kullanıcı Arabirimi olmayan kodu sınırlı olması gerekmez. Bağlamaları de Objective-C içinde geliştirilen bir kullanıcı arabirimi kodu kullanıma sunabilirsiniz.

## <a name="transitioning-from-objective-c"></a>Objective-C geçiş

Zaten bildiğiniz şeyleri ile C# kodu tümleştirmek nasıl gösteren xamarin, tranistion deseninizi oluşturmayı kolaylaştırmak amacıyla belgeleri sitemizi hakkında bilgi bulabilirsiniz. Başlamanıza yardımcı olmak için bazı önemli noktalar şunlardır:

-   [Objective-C geliştiricileri için temel C# bilgileri](primer.md) - bir Xamarin ve C# dili taşımak isteyen Objective-C geliştiricileri için temel bilgileri kısa. 
-   [İzlenecek yol: bir Objective-C kitaplığını bağlama](~/ios/platform/binding-objective-c/walkthrough.md) -Xamarin.iOS uygulamasının mevcut Objective-C kodu yeniden kullanmak için adım adım kılavuz. 


## <a name="binding-objective-c"></a>Objective-C'yi bağlama

Nasıl karşılaştırır C# Objective-C, bir kavrayın sahip ve yukarıdaki bağlama Kılavuzu çalıştıktan sonra geçiş için Xamarin platformunu iyi şeklinde olacaktır. Kapsamlı bağlama başvurusunu dahil olmak üzere yukarı Xamarin.iOS bağlaması teknolojileri hakkında ayrıntılı bilgi için bir izleme kullanıma sunulmuştur [bağlama Objective-C](~/ios/platform/binding-objective-c/index.md) bölümü.

## <a name="cross-platform-development"></a>Platformlar Arası Geliştirme

Son olarak, Xamarin.iOS için taşıdıktan sonra örnek olay incelemeleri devleoped, birlikte içindebulunanyenidenkullanılabilir,platformlararasıkodoluşturmakiçineniyiuygulamalarsahibizbaşvuruuygulamalarıdahilolmaküzeresahibizplatformlararasırehberikullanımaisteyebilirsiniz[ Çapraz Platform uygulamaları oluşturma bölümünde](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
