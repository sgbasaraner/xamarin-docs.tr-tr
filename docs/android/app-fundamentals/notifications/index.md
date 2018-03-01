---
title: Notifications in Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0dbf8c32ca7b7565105c01cfaa077fe792b09b18
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="notifications-in-xamarinandroid"></a>Notifications in Xamarin.Android

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Bu bölümde Xamarin.Android bildirimleri uygulamak gösterilmektedir.
Bir Android bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API anlatılmaktadır oluşturma ve bir bildirim görüntüleme ile söz konusu.

<a name="Sections" />

## <a name="sections"></a>Bölümler

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Android yerel bildirimler](local-notifications.md)

Bu bölümde, Xamarin.Android içinde yerel bildirimleri uygulamak açıklanmaktadır. Bir Android bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API ele oluşturma ve bir bildirim görüntüleme ile söz konusu. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[İzlenecek yol - yerel bildirimler Xamarin.Android içinde kullanma](local-notifications-walkthrough.md)  
 
Bu kılavuz bir Xamarin.Android uygulaması'nda yerel bildirimler kullanmayı kapsar. Oluşturma ve bir bildirim yayımlama temellerini gösterir. Bildirim çekmecesini bildiriminde kullanıcı tıkladığında ikinci bir faaliyeti başlatır. 


## <a name="for-further-reading"></a>Daha Fazla Bilgi İçin

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase bulut Mesajlaşma (FCM), mobil uygulamaları ve sunucu uygulamaları Mesajlaşma kolaylaştıran bir hizmetidir. Firebase Cloud Messaging (anında iletme bildirimleri olarak da bilinir) uzaktan bildirimleri uygulamak için Xamarin.Android uygulamalarda kullanılabilir.

[Bildirimleri](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; bu Android Geliştirici konudur Android bildirimleri için kesin Kılavuzu. Yardımcı olan bölümünde konuları bildirimlerinizi Android kullanıcı arabirimi yönergelerine uygun şekilde tasarlamanız bir tasarım içerir. Bir etkinlik başlatırken preserviing Gezinti hakkında daha fazla arka plan bilgileri sağlar ve kilit ekranında bildirim ve denetim medya kayıttan ilerleme durumunu görüntülemek nasıl açıklanmaktadır. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; bu Android hizmet dinlemek (ve etkileşim), uygulamanız için mümkün kılar tüm bildirimleri gönderilen Android cihazda yalnızca uygulamanız için kayıtlı bildirim alırsınız. Kullanıcının açıkça cihazda bildirimleri dinler mümkün olması için uygulamanıza izin vermesi gerekir unutmayın.





## <a name="related-links"></a>İlgili bağlantılar

- [Yerel bildirimler (örnek)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Uzak bildirimler (örnek)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
