---
title: Microsoft Azure Mobile Apps
description: Örnekler ve kod için Azure portal belgeleri indirir.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: 14ae6bdb3419bac13aebe6b5a9d8346ed5163de5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

_Örnekler ve kod için Azure portal belgeleri indirir._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Kullanılabilir Xamarin belgeleri için bu bağlantıları olan [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) Web sitesi.
Yükleyerek bir Xamarin uygulaması Azure işlevsellik ekleme [Azure mobil istemci](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Xamarin Azure bileşeni ile çalışma

Genel belgeler [Xamarin istemci kitaplığı (Bileşen) çalışma](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) Azure Mobile Apps ile çeşitli görevleri gerçekleştirmek için. Bu sayfa ayrıntılı açıklamalar ve örnekler her aşağıda listelenen kılavuz makaleleri kullanılabilir olmadan örnek kod parçacıkları çok sayıda içerir.

## <a name="getting-started"></a>Başlarken

Bu makalede, ilk Xamarin Azure uygulamanızı çalışır almak için adım adım yönergeler sağlar.
Portalda yeni bir Azure mobil uygulaması oluşturma ve ardından indirme ve önceden yapılandırılmış uygulamayı çalıştıran kapsar.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Kullanıcılar ile çalışmaya başlama

Yapılandırma ve Azure Mobile Services'ı kullanarak bir oturum açma ekranı kodlama için tam yönergeler sağlar. Desteklenen kimlik doğrulama sağlayıcıları, Microsoft, Google, Facebook ve Twitter içerir.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Komut dosyalarında Kullanıcıları yetkilendirmek

Javascript arka uçlarını için bazı örnek kod

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Anında iletme ile çalışmaya başlama

Apple ve Google sitelerinde anında iletme bildirimleri yapılandırın, ardından Azure Mobile Services bir aygıta bir anında iletme bildirimi göndermek için yönergeleri tamamlayın.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Notification Hubs ile çalışmaya başlama

Apple ve Google sitelerinde anında iletme bildirimleri yapılandırmak, Azure bildirim hub'ı yapılandırmak ve cihazlara anında iletme bildirimleri oluşturmak için yönergeler tamamlayın.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>İlgili bağlantılar

- [GettingStarted (örnek)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (örnek)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure mobil istemci](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps öğrenme yolu](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->