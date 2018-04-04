---
title: Xamarin.Forms gereksinimleri
description: Xamarin.Forms Platform ve geliştirme sistem gereksinimleri.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2017
ms.openlocfilehash: e62c82b351bab759192a4fe879a3b63754cdf0af
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-requirements"></a>Xamarin.Forms gereksinimleri

_Xamarin.Forms Platform ve geliştirme sistem gereksinimleri._

Başvurmak [yükleme](~/cross-platform/get-started/installation/index.md) platformlarında uygulama yükleme ve Kurulum yöntemler genel bir bakış için makale.

## <a name="target-platforms"></a>Hedef Platformlar

Xamarin.Forms uygulamaları aşağıdaki işletim sistemleri için yazılabilir:

-  iOS 8 veya sonraki sürümler
-  Android 4.0.3 (API 15) veya üzeri ([daha fazla ayrıntı](#android))
-  Windows 10 Evrensel Windows Platformu ([daha fazla ayrıntı](#windows10))
-  Windows 8.1 / Windows Phone 8.1 WinRT ([daha fazla ayrıntı](#windows))
-  *Windows Phone 8 Silverlight (kullanım dışı)*

Geliştiriciler alışkanlığına sahip olduğunuz varsayılır [taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md) ve [paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md).

<a name="android" />

### <a name="android"></a>Android

Yüklü en son Android SDK Araçları ve Android API platformunu olması gerekir. Kullanarak en son sürümleri güncelleştirebilirsiniz [Android SDK Manager](~/android/get-started/installation/android-sdk.md).

Ayrıca, Android projeleri için hedef/derleme sürümü **gerekir** ayarlanabilir *son yüklenmiş platform*. Böylece Android 4.0.3 kullanan cihazları desteklemek devam edebilirsiniz ancak en düşük sürüm API 15'e ayarlanabilir ve daha yeni. Bu değerleri ayarlayın **proje seçenekleri**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Proje Seçenekleri > Uygulama > uygulama özellikleri**

![](installation-images/options-android-vs-sml.png "Visual Studio'da Android derleme seçenekleri")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

**Derleme > Genel**

![](installation-images/options-general-sml.png "Derleme > Genel")

**Derleme > Android uygulaması**

![](installation-images/options-android-sml.png "Derleme > Android uygulaması")

-----


<a name="windows10" />

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Bir çözüm üzerinde macOS oluşturulduğunda, Windows 10 UWP projeleri eklenmez. Varolan bir çözümü bu projelerine ekleme hakkında daha fazla yönerge için bkz: [bir evrensel Windows Platformu (UWP) uygulamasını ekleme](~/xamarin-forms/platform/windows/installation/universal.md).


<a name="windows" />

### <a name="windows-81--windows-phone-81-winrt"></a>Windows 8.1 / Windows Phone 8.1 WinRT

Windows 8.1 / Windows Phone 8.1 WinRT projeleri çözüm üzerinde macOS oluşturulduğunda eklenmez. Varolan bir çözümü bu projelerine ekleme hakkında daha fazla yönerge için bkz: [bir Windows Phone uygulama ekleme](~/xamarin-forms/platform/windows/installation/phone.md) ve [bir Windows uygulaması ekleme](~/xamarin-forms/platform/windows/installation/tablet.md).


## <a name="development-system-requirements"></a>Geliştirme sistem gereksinimleri

Xamarin.Forms uygulamaları macOS ve Windows geliştirilebilir. Ancak, Windows ve Visual Studio uygulamasının Windows sürümleri oluşturmak için gereklidir.

## <a name="mac-system-requirements"></a>Mac sistem gereksinimleri

Xamarin.Forms uygulamaları OS X El Capitan'üzerinde (10.11 sürümünü) geliştirmek için Visual Studio Mac için kullanabileceğiniz ya da daha yeni. İOS uygulamaları geliştirmek için en az 10 SDK'sı ve Xcode yüklü 8 iOS sahip olmanızı öneririz.

> [!NOTE]
>  Windows uygulamaları üzerinde macOS geliştirilmiş olamaz.

## <a name="windows-system-requirements"></a>Windows sistem gereksinimleri

Xamarin.Forms uygulamalar iOS ve Android için Xamarin geliştirmesini destekleyen herhangi bir Windows yüklemesinde oluşturulabilir. Bu, Visual Studio 2015 ya da daha yeni çalışan Windows 7 veya üstü gerektirir. Ağa bağlı bir Mac, iOS geliştirme için gereklidir.

### <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

Xamarin.Forms uygulamalar için UWP geliştirme gerektirir:

* Windows 10 (sonbaharda oluşturucuları güncelleştirme önerilir)

* Visual Studio 2017 önerilir.

* [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

UWP projeleri, Visual Studio 2015 ve Visual Studio 2017'de oluşturulan Xamarin.Forms çözümlerinde dahil edilir.
Ayrıca [bir evrensel Windows Platformu (UWP) uygulamasını eklemek](~/xamarin-forms/platform/windows/installation/universal.md) varolan Xamarin.Forms çözümünü için.

