---
title: Kurulum Windows projeleri
description: "Varolan bir Xamarin.Forms çözümü yeni Windows projelerine ekleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 2acabc68c595bfb3bbd5c3516f68cd777ba10327
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="setup-windows-projects"></a>Kurulum Windows projeleri

_Varolan bir Xamarin.Forms çözümü yeni Windows projelerine ekleme_

Eski Xamarin.Forms projeleri (veya Mac OS oluşturulanlar&nbsp;X) bu Windows projeleri Kurulum sahip olmaz.

Başka bir deyişle, Windows 8.1, Windows Phone 8.1 ve Windows 10 (UWP) uygulamaları oluşturmak için bu proje türleri el ile eklemeniz gerekir.

## <a name="add-a-windows-81-app"></a>Windows 8.1 uygulama Ekle

* PCL şablonu kullandıysanız [profilini güncelleştirmek](#pcl), ardından
* [Windows 8.1 uygulama Ekle](~/xamarin-forms/platform/windows/installation/tablet.md) tablet/Masaüstü form faktörleri için.

## <a name="add-a-windows-phone-81-app"></a>Ekleme bir Windows Phone 8.1 uygulama

* PCL şablonu kullandıysanız [profilini güncelleştirmek](#pcl), ardından
* [Ekleme bir Windows Phone 8.1 uygulama](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>Ekleme Evrensel Windows Platformu (UWP) uygulaması

* Derleme [UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx) uygulamaların Windows 10 çalıştıran Visual Studio 2015 gerektirir.
* PCL şablonu kullandıysanız [profilini güncelleştirmek](#pcl), ardından
* [Evrensel Windows eklemek Platform uygulama](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>PCL profilinizi güncelleştirme

Varolan Xamarin.Forms uygulamanızı taşınabilir sınıf kitaplığı (PCL) şablonu kullandıysanız, kendi profili güncelleştirmeniz gerekir.

1. **sağ tıklayın > Özellikler** (varolan ayarlarınızı farklı olabilir)

  ![](images/targets.png "PCL hedefleri")

2. Tıklayın **Değiştir...**  düğmesi

3. Olun **Windows 8** ve **Windows Phone 8.1** seçenekleri seçilidir (ve **Windows Phone Silveright** olan *XML'deki seçili*):

  ![](images/pcl.png "PCL hedef seçenekleri")

4. Tuşuna **Tamam** ve değişiklikleri kaydedin.

Bu karşılık gelir **profil 111** açılan listeyi kullanarak Mac için Visual Studio'da, PCL yapılandırıyorsanız.

  ![](images/pcl-xs.png "PCL profili 111")

**Not:** çözümünüzün bir Windows Phone 8 Silverlight projesi hala varsa, profil 259 PCL ayarlamanız gerekir. Bu sayfada gösterilen proje türleri ile değiştirildiğini önerilir, Windows Phone 8 Silverlight destek onaylanmaz.
