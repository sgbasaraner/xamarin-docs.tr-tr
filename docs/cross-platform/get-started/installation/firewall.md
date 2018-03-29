---
title: Xamarin Güvenlik Duvarı'nı yapılandırma yönergeleri
description: Şirketiniz için Xamarin'ın platform çalışmaya izin vermek için Güvenlik Duvarı'nda beyaz liste ile gereken ana listesi.
ms.topic: article
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 5c6e850594e23d650dbe67126143ce7d58fcaa82
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin Güvenlik Duvarı'nı yapılandırma yönergeleri

_Şirketiniz için Xamarin'ın platform çalışmaya izin vermek için Güvenlik Duvarı'nda beyaz liste ile gereken ana listesi._

Yükleme ve düzgün çalışması Xamarin ürünleri için sırayla belirli uç noktaları gereken Araçlar ve, yazılım güncelleştirmelerini indirmek için erişilebilir olması gerekir. Sizin veya şirketinizin katı güvenlik duvarı ayarları varsa, yükleme, lisans, bileşenleri ve daha fazla ile sorunlarla karşılaşabilirsiniz. Bu belge, Güvenlik Duvarı'nda çalışmak xamarin sırayla güvenilir listesinde olması gereken bilinen uç noktalarını bazıları özetlenmektedir. Bu liste, yüklemeye dahil herhangi bir üçüncü taraf aracı için gerekli uç noktalarını içermez. Bu liste geçtikten sonra hala sorun yaşıyorsanız, Apple ya da Android yükleme sorun giderme kılavuzları bakın.

## <a name="endpoints-to-whitelist"></a>Güvenilir listeye uç noktaları

### <a name="xamarin-installer"></a>Xamarin Installer

Aşağıdaki bilinen adresleri yazılımının Xamarin Yükleyicisi'nin en son sürümünü kullanırken düzgün bir şekilde yüklenmesi sırayla eklenmesi gerekir:

-  xamarin.com (Yükleyici bildirimleri)
-  DL.xamarin.com (paket yükleme konumu)
-  dl.google.com (Android SDK yüklemek için)
-  download.oracle.com (JDK)
-  VisualStudio.com (Kurulum paketlerini indirmek konumu)
-  go.microsoft.com (Kurulum URL çözümleme)
-  aka.MS (Kurulum URL çözümleme)

Lütfen bir Mac kullanıyorsanız ve yükleme sorunlarla Xamarin.Android bu macOS Java indirebildiğini emin olun.


### <a name="components-store-and-nuget-including-xamarinforms"></a>Bileşenleri deposu ve NuGet (Xamarin.Forms dahil)

Aşağıdaki adresleri Xamarin bileşen Deposu'nda veya NuGet erişmek için eklenmesi gerekir (NuGet Xamarin.Forms paketlenmiş):

-  components.xamarin.com (Xamarin bileşenleri deposunu kullanmak üzere)
-  xampubdl.BLOB.Core.Windows.NET (ana bileşenler deposu indirmeleri)
-  www.nuget.org (NuGet erişim için)
-  az320820.vo.msecnd.net (NuGet downloads)
-  DL ssl.google.com (Google bileşenleri)


### <a name="software-updates"></a>Yazılım güncelleştirmeleri

Aşağıdaki adresler, yazılım güncelleştirmeleri düzgün indirebilirsiniz emin olmak için eklenecek gerekir:

-  Software.xamarin.com (güncelleştirici hizmetini)
-  download.visualstudio.microsoft.com
-  dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac arası

Mac derleme için Visual Studio bağlanmak için Xamarin Mac Aracısı'nı kullanarak ana bilgisayar SSH bağlantı noktası açık olmasını gerektirir. Varsayılan olarak **bağlantı noktası 22**.

## <a name="summary"></a>Özet

Bu kılavuz yüklemek ve düzgün şekilde makinenizde güncelleştirmek Xamarin ürünleri izin vermek için güvenilir listeye uç noktaları ele.