---
title: "Cihazda hata ayıklama"
description: "Bu makalede, bir Xamarin.Android uygulaması fiziksel Android cihazda hata ayıklama açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d340c3da220deacdb5606547a084e55d80c817e7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="debug-on-device"></a>Cihazda hata ayıklama

_Bu makalede, bir Xamarin.Android uygulaması fiziksel Android cihazda hata ayıklama açıklanmaktadır._

## <a name="debug-on-device-overview"></a>Aygıt bir genel bakış hata ayıklama

Mac için Visual Studio veya Visual Studio kullanarak Android cihazında bir Xamarin.Android hata ayıklamak mümkündür. Hata ayıklama bir aygıtta oluşabilmesi için öncelikle olmalıdır [geliştirme için Kurulum](~/android/get-started/installation/set-up-device-for-development.md) ve PC veya Mac için bağlı


## <a name="debug-application"></a>Uygulamanızın hatalarını ayıklama

Bir aygıt bilgisayarınıza bağlandıktan sonra bir Xamarin.Android uygulaması hata ayıklama herhangi bir Xamarin ürün veya .NET uygulaması aynı şekilde yapılır. Emin **hata ayıklama** yapılandırma ve dış aygıt IDE'de seçildiğinde, bu gerekli hata ayıklama simgeleri kullanılabilir olduğunu ve IDE çalışan bir uygulamaya bağlanabilir garanti eder: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Hata ayıklama yapılandırması seçili](debug-on-device-images/image1-vs.png)

Ardından, bir kesme noktası kodda ayarlanır:

![Kod satırında kesme](debug-on-device-images/image2-vs.png)

Cihaz seçildikten sonra Xamarin.Android cihaza bağlanın, uygulamayı dağıtın ve ardından çalıştırın. Kesme noktası ulaşıldığında, hata ayıklayıcı herhangi diğer C# uygulamaya benzer bir şekilde ayıklanacak uygulama izin vererek uygulama durduracak: 

![Kesme noktası ulaşıldı](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Hata ayıklama yapılandırması seçili](debug-on-device-images/image1-xs.png)

Ardından, bir kesme noktası kodda ayarlanır:

![Kod satırında kesme](debug-on-device-images/image2-xs.png)

Cihaz seçildikten sonra Xamarin.Android cihaza bağlanın, uygulamayı dağıtın ve ardından çalıştırın. Kesme noktası ulaşıldığında, hata ayıklayıcı herhangi diğer C# uygulamaya benzer bir şekilde ayıklanacak uygulama izin vererek uygulama durduracak: 

![Kesme noktası ulaşıldı](debug-on-device-images/image3-xs.png)

-----



## <a name="summary"></a>Özet

Bu belgede, bir Xamarin.Android uygulaması hata ayıklama kesme noktası ayarlama ve hedef cihaz seçilerek açıklanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Cihazı Dağıtım için Ayarlama](~/android/get-started/installation/set-up-device-for-development.md)
- [Debuggable özniteliğini ayarlama](~/android/deploy-test/debuggable-attribute.md)
