---
title: Microsoft Azure
description: Belgeler ve örnek kod için Azure indirir.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: ab449a58cc87699b97a1ade7721a08f771c4f55d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="microsoft-azure"></a>Microsoft Azure

_Belgeler ve örnek kod için Azure indirir._

[ ![](images/evolve-mikej-azure-sml.png "Azure App Services özellikleri eklemek bulut veri depolama ve platformlar arası anında iletme bildirimleri de dahil olmak üzere Xamarin uygulamaları için kolaydır")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[2016 gelişmesi: Azure ve Xamarin kullanarak bağlı uygulamaları geliştirme](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Mac için Visual Studio'da Hizmetleri bağlı

Yeni [bağlantılı Hizmetler](connected-services.md) özelliği Mac için Visual Studio, geliştiricilerin hızla ve kolayca mobil uygulamalarından IDE içinden Azure işlevsellik eklemek için yardımcı olur. Şu anda alfa kanal test etmek için kullanılabilir.


## <a name="azure-app-services"></a>Azure uygulama hizmetleri

Bir koleksiyonu [Azure Mobile Apps belge](~/cross-platform/data-cloud/mobile-apps.md) , uygulama işleminde size rehberlik eder [Azure mobil istemci](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin sunduğu bir Azure Mesajlaşma NuGet paketleri için [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) ve [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) platformda anında iletme bildirimleri uygulamak yardımcı olacak.

Uygulamalarınızı yapılandırın [Azure App Services portal](https://portal.azure.com/) mobil uygulamalar, Web API'leri, depolama ve daha fazlasını erişmek için. Hakkında bilgi edinin [nasıl uygulama hizmetleri farklı](http://azure.microsoft.com/updates/whats-new-with-azure-app-service/) ve içinde izleme [bu videolar Microsoft'un](http://azure.microsoft.com/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Active Directory kimlik doğrulaması

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) Xamarin uygulamaları oturum açma kullanıcılar için kullanılabilir [Xamarin.Auth bileşen](https://www.nuget.org/packages/Xamarin.Auth/).
Sonra Office 365 gibi ek hizmetlerine erişebilir.

## <a name="webapi"></a>Webapı

Microsoft'un Web API kolayca Xamarin uygulamaları tarafından kullanılabilecek bir REST benzeri arabirimi sunar.
Kolayca döndürme yukarı bir [Azure Web sitesi](https://trywebsites.azurewebsites.net/) ve Xamarin uygulamaları bağlamak için Webapı tabanlı bir uygulama oluşturun.


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Web hizmetlerine giriş](~/cross-platform/data-cloud/web-services/index.md)

Bu öğretici, REST tümleştirme tanıtır WCF ve SOAP web hizmeti teknolojileri Xamarin mobil uygulamaları ile. Çeşitli hizmet uygulamalarını inceler, araçları ve bunları birleştirmek için kitaplıkları değerlendirir ve hizmet verinin tüketimi için örnek desenleri sağlar. Son olarak, bir Xamarin mobil uygulama ile bir RESTful web hizmetini oluşturma temel bir genel bakış sağlar.

## <a name="samples"></a>Örnekler

Ek olarak [belgelerine örnekleri](https://github.com/xamarin/mobile-samples/tree/master/Azure), Xamarin uygulamaları dahil çeşitli Azure özelliklerinin aşağıdaki tam uygulamalar gösterecek:

- [Spor](https://github.com/xamarin/Sport) – kolay Spor ligi izleme veri depolama ve anında iletme bildirimleri kullanan bir uygulama.
- [Dakika](https://github.com/pierceboggan/Moments) –, paylaşımı anlık fotoğraf görüntüler için Azure Storage kullanır.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – için arka uç Web API'sini kullanır.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -Azure mobil uygulamalar.

- [Elektronik Mağaza](https://github.com/dotnet-architecture/eShopOnContainers) – için örnek [mimarisi serisi](https://www.microsoft.com/net/learn/architecture) e-kitapları.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IOT örnek yapı 2016'dan.


## <a name="related-links"></a>İlgili bağlantılar

- [Azure PCL örnek (tarafından @paulbatum) (örnek)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure portalı](http://azure.microsoft.com/)
- [Mobil istemci xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
