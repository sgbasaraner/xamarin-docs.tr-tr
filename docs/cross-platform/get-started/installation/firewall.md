---
title: Xamarin güvenlik duvarı yapılandırma yönergeleri
description: Bu belge kurumsal bir ortamda çalışmak Xamarin izin vermek için Güvenlik Duvarı'nda izin verilenler listesinde olması gereken ana bilgisayar listesini sağlar.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: asb3993
ms.author: amburns
ms.date: 10/05/2018
ms.openlocfilehash: 68689ce7d92a038d0724e1441f68fddcb1d0bba8
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "34781097"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin güvenlik duvarı yapılandırma yönergeleri

_Şirketiniz için Xamarin'in platform çalışmaya izin vermek için güvenlik duvarınızdan izin verilenler listesine gereken ana listesi._

Belirli uç noktaları sırada yükleyip düzgün çalışması Xamarin ürünleri için gerekli araçları ve yazılım güncelleştirmelerini indirmek için erişilebilir olmalıdır. Sizin veya şirketinizin katı güvenlik duvarı ayarları varsa, yükleme, lisans, bileşenleri ve daha fazlasıyla sorunlarla karşılaşabilirsiniz. Bu belge duvarınızda sırayla çalışması Xamarin için beyaz listeye alınması gereken bilinen uç bazıları özetlenmektedir. Bu liste, yüklemeye dahil herhangi bir üçüncü taraf araçları için gerekli uç noktaları içermez. Bu listede filtrelemesinden geçtikten sonra hala sorun yaşıyorsanız, Apple ya da sorun giderme kılavuzlarının Android yükleme bakın.

## <a name="endpoints-to-whitelist"></a>Beyaz liste uç noktaları

### <a name="xamarin-installer"></a>Xamarin yükleyici

Aşağıdaki bilinen adresleri Xamarin yükleyicisinin en son sürümünü kullanırken düzgün bir şekilde yüklemek yazılım teslimi eklenmesi gerekir:

- xamarin.com (Yükleyici bildirimleri)
- DL.xamarin.com (paket indirme konumu)
- dl.google.com (Android SDK'yı indirmek için)
- download.Oracle.com (JDK)
- VisualStudio.com (kurulum paketleri indirme konumu)
- go.microsoft.com (Kurulum URL çözümleme)
- aka.MS (Kurulum URL çözümleme)

Lütfen bir Mac kullanarak ve Xamarin.Android yükleme sorunlarını karşılaşıldığında, Macos'u Java indirebildiğini emin olun.

### <a name="nuget-including-xamarinforms"></a>NuGet (Xamarin.Forms dahil)

Aşağıdaki adresleri NuGet erişmeye eklenmesi gerekir (bir NuGet Xamarin.Forms paketlenen):

- www\.nuget.org (NuGet erişmek için)
- az320820.VO.msecnd.NET (NuGet yüklemeleri)
- DL-ssl.google.com (Android ve Xamarin.Forms için Google bileşenleri)

### <a name="software-updates"></a>Yazılım güncelleştirmeleri

Aşağıdaki adresler, yazılım güncelleştirmelerini düzgün bir şekilde indirebilirsiniz emin olmak için eklenmesi gerekir:

- Software.xamarin.com (updater hizmeti)
- download.VisualStudio.microsoft.com
- DL.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac aracı

Visual Studio için bir Mac derleme bağlanmak için Xamarin Mac Aracısı'nı kullanarak konağı SSH bağlantı noktası açık olmasını gerektirir. Varsayılan olarak **bağlantı noktası 22**.

## <a name="summary"></a>Özet

Bu kılavuz uç noktalar için Xamarin ürünleri yüklemek ve makinenizde düzgün bir şekilde güncelleştirmek izin vermek için beyaz liste kapsamdaki.
