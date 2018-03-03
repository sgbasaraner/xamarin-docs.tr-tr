---
title: Microsoft Azure Mobile Apps
description: "Örnekler ve kod için Azure portal belgeleri indirir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fb3c26b7d090ca42328c61192c794dec1544d1d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
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


Kullanılabilir Xamarin belgeleri için bu bağlantıları olan [Azure Mobile Apps](https://azure.microsoft.com/en-us/documentation/services/app-service/mobile/) Web sitesi.
Azure işlevleri için Xamarin ekleme uygulama indiriliyor olarak kadar basittir [Azure mobil istemci](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Xamarin Azure bileşeni ile çalışma

Genel belgeler [Xamarin istemci kitaplığı (Bileşen) çalışma](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/) Azure Mobile Apps ile çeşitli görevleri gerçekleştirmek için. Bu sayfa ayrıntılı açıklamalar ve örnekler her aşağıda listelenen kılavuz makaleleri kullanılabilir olmadan örnek kod parçacıkları çok sayıda içerir.

## <a name="getting-started"></a>Başlarken

Bu makalede, ilk Xamarin Azure uygulamanızı çalışır almak için adım adım yönergeler sağlar.
Portalda yeni bir Azure mobil uygulaması oluşturma ve ardından indirme ve önceden yapılandırılmış uygulamayı çalıştıran kapsar.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-forms-get-started/)

## <a name="validate-modify-and-augment-data-in-scripts"></a>Doğrulama, değiştirmek ve komut veri büyütmek

Sunucu tarafında doğrulama ve diğer işlevleri uygulamak için Azure Mobile Services veri tablolarına sunucu tarafı komut dosyası eklemek gösterilmiştir.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)


## <a name="add-paging-to-data"></a>Disk belleği veri ekleme

Büyük Skip() ve Take() kullanarak veri kümelerine yönelik disk belleği hızlı bir örnek.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)


## <a name="get-started-with-users"></a>Kullanıcılar ile çalışmaya başlama

Yapılandırma ve Azure Mobile Services'ı kullanarak bir oturum açma ekranı kodlama için tam yönergeler sağlar. Desteklenen kimlik doğrulama sağlayıcıları, Microsoft, Google, Facebook ve Twitter içerir.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Komut dosyalarında Kullanıcıları yetkilendirmek

Javascript arka uçlarını için bazı örnek kod

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Anında iletme ile çalışmaya başlama

Apple ve Google sitelerinde anında iletme bildirimleri yapılandırın, ardından Azure Mobile Services bir aygıta bir anında iletme bildirimi göndermek için yönergeleri tamamlayın.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-push/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-push/)


## <a name="get-started-with-notification-hubs"></a>Notification Hubs ile çalışmaya başlama

Apple ve Google sitelerinde anında iletme bildirimleri yapılandırmak, Azure bildirim hub'ı yapılandırmak ve cihazlara anında iletme bildirimleri oluşturmak için yönergeler tamamlayın.

-  [iOS](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-ios-get-started/)
-  [Android](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-android-get-started/)



## <a name="related-links"></a>İlgili bağlantılar

- [GettingStarted (örnek)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
- [NotificationHubs (örnek)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure PCL örnek (tarafından @paulbatum) (örnek)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure mobil istemci](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps öğrenme yolu](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)
