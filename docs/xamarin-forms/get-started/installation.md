---
title: Xamarin.Forms gereksinimleri
description: Xamarin.Forms Platform ve geliştirme sistem gereksinimleri.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: 75e6d25f95a0a3f18c83fe73f67ad4a7797f0924
ms.sourcegitcommit: c024f29ff730ae20c15e99bfe0268a0e1c9d41e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2018
ms.locfileid: "34470336"
---
# <a name="xamarinforms-requirements"></a>Xamarin.Forms gereksinimleri

_Xamarin.Forms Platform ve geliştirme sistem gereksinimleri._

Başvurmak [yükleme](~/cross-platform/get-started/installation/index.md) platformlarında uygulama yükleme ve Kurulum yöntemler genel bir bakış için makale.

## <a name="target-platforms"></a>Hedef platformlar

Xamarin.Forms uygulamaları aşağıdaki işletim sistemleri için yazılabilir:

- iOS 8 veya sonraki sürümler
- Android 4.0.3 (API 15) veya üzeri ([daha fazla ayrıntı](#android))
- Windows 10 Evrensel Windows Platformu ([daha fazla ayrıntı](#windows10))

Geliştiriciler alışkanlığına sahip olduğunuz varsayılır [.NET standart](~/cross-platform/app-fundamentals/net-standard.md) ve [paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md).

### <a name="additional-platform-support"></a>Ek platform desteği

Bu platformlar durumunu kullanılabilir [Xamarin.Forms GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

- Samsung Tizen
- MacOS
- GTK#
- WPF

### <a name="platforms-from-earlier-versions"></a>Önceki sürümlerinden platformları

Bu platformlar Xamarin.Forms 3.0 kullanılırken desteklenmez:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*

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

## <a name="development-system-requirements"></a>Geliştirme sistem gereksinimleri

Xamarin.Forms uygulamaları macOS ve Windows geliştirilebilir. Ancak, Windows ve Visual Studio uygulamasının Windows sürümleri oluşturmak için gereklidir.

## <a name="mac-system-requirements"></a>Mac sistem gereksinimleri

Xamarin.Forms uygulamaları OS X El Capitan'üzerinde (10.11 sürümünü) geliştirmek için Visual Studio Mac için kullanabileceğiniz ya da daha yeni. İOS uygulamaları geliştirmek için en az 10 SDK'sı ve Xcode yüklü 8 iOS sahip olmanızı öneririz.

> [!NOTE]
>  Windows uygulamaları üzerinde macOS geliştirilmiş olamaz.

## <a name="windows-system-requirements"></a>Windows sistem gereksinimleri

Xamarin.Forms uygulamalar iOS ve Android için Xamarin geliştirmesini destekleyen herhangi bir Windows yüklemesinde oluşturulabilir. Bu, Visual Studio 2017 ya da daha yeni çalışan Windows 7 veya üstü gerektirir. Ağa bağlı bir Mac, iOS geliştirme için gereklidir.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

Xamarin.Forms uygulamalar için UWP geliştirme gerektirir:

- Windows 10 (sonbaharda oluşturucuları güncelleştirme önerilir)

- Visual Studio 2017

- [Windows 10 SDK'sı](https://dev.windows.com/downloads/windows-10-sdk)

UWP projeleri Visual Studio 2017 oluşturulan Xamarin.Forms çözümlerin ancak Mac için Visual Studio'da oluşturulan çözümleri içinde yer alan
Yapabilecekleriniz [bir evrensel Windows Platformu (UWP) uygulamasını eklemek](~/xamarin-forms/platform/windows/installation/index.md) herhangi bir anda bir Xamarin.Forms çözüme.
