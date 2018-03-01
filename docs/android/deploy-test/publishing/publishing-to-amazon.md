---
title: "Amazon uygulama mağazası yayımlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: A3E9EAC7-2968-8891-CDF2-B73FC0013EC9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 6b4958c6a82b824f19cc041b124e79034eba4c86
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-to-the-amazon-app-store"></a>Amazon uygulama mağazası yayımlama

Amazon mobil uygulama dağıtım programı uygulamalarını Amazon yayımlamak mobil uygulama geliştiricileri sağlar. Bu bölümde, Android için Amazon App Store kısaca ele alınmaktadır. 

[![Amazon App Store ekranında](publishing-to-amazon-images/amazon-app-store.png)](publishing-to-amazon-images/amazon-app-store.png)

Amazon APKs boyutunu sınırlamaz. Bir APK 30 MB'den büyük olursa, ancak daha sonra FTP Amazon mobil uygulama dağıtım portalı yerine dağıtım için kullanır.

<a name="Submitting_Apps:_Binary_Info" />

## <a name="submitting-apps-binary-info"></a>Gönderen uygulamalar: İkili bilgisi

Amazon uygulama mağazası uygulama gönderme, Google Play uygulamaya göndermek için benzer bir işlemdir. Amazon tarafından dağıtılan uygulamaları şu varlıkları gerektirir: 

-   **Simge** &ndash; 114 x 114 .png dosyası saydam arka plan ile budur. Gerekli değildir.
-   **Küçük resim** &ndash; Yukarıdaki simgeyi daha büyük bir sürümü budur. Bu, 512 x 512 piksel saydam arka plana sahip olur. Bu simge da zorunludur.
-   **Ekran görüntüleri** &ndash; Amazon üç en az ve en fazla 10 ekran görüntüleri gerektirir. Ekran görüntüleri x 600 h piksel 1024w veya 800w x 480 h piksel olmalıdır. .Png veya .jpg biçimleri kabul edilir.
-   **Promosyon görüntü** &ndash; sırada tanıtım yerleşimi giriş sayfası gibi özel bir uygulama için bir tanıtım görüntüsü isteğe bağlı olarak gönderilebilir. X 500 h piksel .png veya .jpg dosyası, yatay yönde 1024w olmalıdır. Hiçbir animasyon olmayabilir.
-  Beş videolar güncelleştirmeleri sağlanabilir.


<a name="Approval_Process" />

## <a name="approval-process"></a>Onay işlemi

Bir uygulama gönderildikten sonra bir onay süreci devam ettiği.
Amazon, ürün açıklamasında özetlendiği gibi çalışır, müşteri verilerini tehlikeye atabilir değil ve cihaz işlemi bozacak değil emin olmak için uygulamanızın incelenecektir. Onay işlemi tamamlandığında, Amazon bildirim göndermek ve uygulamayı dağıtın.
