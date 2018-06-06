---
title: Objective-C geliştiriciler için Xamarin
description: Bu belge, Objective-C geliştiriciler için Xamarin.iOS açıklamasını sağlar. C# Objective-C geçiş yapma, C# kullanmak için bir Objective-C Kitaplığı bağlamak nasıl ve platformlar arası mobil uygulamasının nasıl oluşturulacağını açıklayan kılavuzlara bağlar.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 027ef8cabc55ace41bcf201b8355aab8ca6ec625
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786057"
---
# <a name="xamarin-for-objective-c-developers"></a>Objective-C geliştiriciler için Xamarin

Kendi kullanıcı olmayan arabirimi taşımak için iOS hedefleyen geliştiriciler için bir yol, böylece kullanılabilmesi için platform belirsiz için C# kod Xamarin teklifler herhangi bir yere C# Xamarin.Android aracılığıyla Android ve Windows çeşitli özellikleri dahil olmak üzere kullanılabilir. Ancak, yalnızca, C# Xamarin ile kullanmak için var olan becerileri ve Objective-C kodunu yararlanamaz anlamına gelmez. Xamarin tüm yerel iOS ve OS X platformunu tanıyor ve ona Uıkit, çekirdek animasyon, çekirdek Foundation ve çekirdek grafikleri gibi birkaçıdır memnuniyet API'leri gösterir çünkü aslında, Objective-C bilerek, daha iyi bir Xamarin.iOS Geliştirici hale getirir. Aynı anda LINQ ve genel türler yanı sıra, zengin .NET sınıf kitaplıkları yerel uygulamalarınızda kullanmak için temel gibi özellikler içeren C# dili gücünü alın.

Ayrıca, Xamarin, varolan Objective-C yararlanmanızı sağlar varlıklar bir teknoloji aracılığıyla bilmeniz bağlar. Sadece Objective-C statik kitaplık oluşturma ve onu C# için bir bağlama aracılığıyla aşağıdaki çizimde gösterildiği gibi kullanıma:

 [![](images/01-bindings.png "Objective-C C# için bir bağlama aracılığıyla kullanıma sunulan statik kitaplığa")](images/01-bindings.png#lightbox)

Bu kullanıcı Arabirimi olmayan koda sınırlı olması gerekmez. Bağlamaları Objective-C da geliştirilmiş kullanıcı arabirimi kodu getirebilir.

## <a name="transitioning-from-objective-c"></a>Objective-C ' geçiş

C# kodu zaten bilmeniz ile tümleştirmek nasıl gösteren tranistion Xamarin için kullanılan çok sayıda kolaylığı yardımcı olmak için belgeleri sitemizi hakkında bilgi bulabilirsiniz. Başlamanıza yardımcı olmak için bazı önemli noktalar:

-   [C# Primer Objective-C geliştiriciler için](primer.md) - bir Xamarin ve C# dili taşımak isteyen Objective-C geliştiriciler için primer kısa. 
-   [İzlenecek yol: bir Objective-C Kitaplığı bağlama](~/ios/platform/binding-objective-c/walkthrough.md) -Xamarin.iOS uygulamasının varolan Objective-C kodu yeniden kullanma için adım adım kılavuz. 


## <a name="binding-objective-c"></a>Objective-C bağlama

Nasıl karşılaştırır C# Objective-C için bir kavrayın bağlama izlenecek yol yukarıdaki çalıştıktan sonra Xamarin platformuna geçiş için iyi şeklinde olacaktır. İzleme, Xamarin.iOS bağlama teknolojileri hakkında daha ayrıntılı bilgi, kapsamlı bağlama başvuru dahil olmak üzere kullanılabilir [bağlama Objective-C](~/ios/platform/binding-objective-c/index.md) bölümü.

## <a name="cross-platform-development"></a>Platformlar Arası Geliştirme

Son olarak, Xamarin.iOS için taşıdıktan sonra örnek olay incelemeleri bulunanyenidenkullanılabilir,platformlararasıkoduoluşturmakiçineniyiuygulamalarilebirliktedevleopedsahibizbaşvuruuygulamalardahilolmaküzeresahibizplatformlararasıkılavuzukullanımaistediğiniz[ Çapraz Platform uygulamaları oluşturma bölümüne](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
