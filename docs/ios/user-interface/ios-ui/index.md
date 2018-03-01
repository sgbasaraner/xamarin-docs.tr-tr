---
title: "İOS kullanıcı arabirimi"
description: "Bir Xamarin.iOS uygulaması iOS kullanıcı arabirimi ile çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2B3E45FA-C30F-D708-0E8F-3EE02BD1A867
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 831ddfff7e05c391472b280095564f90528369ff
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface-in-ios"></a>İOS kullanıcı arabirimi

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Görünüm API](introduction-to-the-appearance-api.md)

iOS UIAppearance API'lerini kullanarak konulu olması için kullanıcı arabirimi denetimlerini birçok visual özniteliklerini sağlar.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Kullanıcı arabirimi nesneleri oluşturma](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple grupları "hangi Xamarin.iOS ad alanlarına eşitlemek çerçeveleri" işlevsellik parçalarını ilgili. `UIKit` iOS için tüm kullanıcı arabirimi denetimlerini içeren ad alanıdır.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Düzen Seçenekleri](~/ios/user-interface/ios-ui/layout-options.md)

Bir görünümü yeniden boyutlandırılmış veya Döndürülmüş düzenini denetlemek için iki farklı mekanizmalar vardır: otomatik boyutlandırma ve otomatik düzenleme.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Haptic geribildirim sağlama](~/ios/user-interface/ios-ui/haptic-feedback.md)

Bu makalede haptic geri bildirim iOS 10 ve Xamarin.iOS içinde uygulama bulunan yeni türleri yer almaktadır.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Kullanıcı Arabirimi iş parçacığı ile çalışma](~/ios/user-interface/ios-ui/ui-thread.md)

Kodunuzun yalnızca kullanıcı arabirimi denetimlerini ana (veya kullanıcı Arabirimi) yapılan değişiklikler iş parçacığı olmanız gerekir. Farklı bir iş parçacığında (örneğin, bir geri çağırma veya arka plan iş parçacığı) oluşan herhangi bir kullanıcı Arabirimi güncelleştirme ekrana çizilir değil veya bir kilitlenme bile neden olabilir.




