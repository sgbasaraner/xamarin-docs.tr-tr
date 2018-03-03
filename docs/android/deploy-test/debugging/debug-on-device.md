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
ms.openlocfilehash: 5e9874fba52b576494be5ac42ff13fdd0be4d9e7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="debug-on-device"></a>Cihazda hata ayıklama

_Bu makalede, bir Xamarin.Android uygulaması fiziksel Android cihazda hata ayıklama açıklanmaktadır._

## <a name="debug-on-device-overview"></a>Aygıt bir genel bakış hata ayıklama

Mac için Visual Studio veya Visual Studio kullanarak Android cihazında bir Xamarin.Android hata ayıklamak mümkündür. Hata ayıklama bir aygıtta oluşabilmesi için öncelikle olmalıdır [geliştirme için Kurulum](~/android/get-started/installation/set-up-device-for-development.md) ve PC veya Mac için bağlı

<a name="Debug_Application" />

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


<a name="Summary" />

## <a name="summary"></a>Özet

Bu belgede, bir Xamarin.Android uygulaması hata ayıklama kesme noktası ayarlama ve hedef cihaz seçilerek açıklanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Aygıtı geliştirme için ayarlama](~/android/get-started/installation/set-up-device-for-development.md)
- [Debuggable özniteliğini ayarlama](~/android/deploy-test/debuggable-attribute.md)
