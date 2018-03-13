---
title: "Android .axml dosyalarında IntelliSense nasıl etkinleştirebilirim?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: c68ec5a6d369e8673c8764e01943e513489775b0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Android .axml dosyalarında IntelliSense nasıl etkinleştirebilirim?

Visual Studio'da Android IntelliSense uygun şekilde çalışması için iki XML şema dosyaları gerekir. Bu dosyaları bulmak için bu klasöre gidin:

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (Daha önce: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

İçinde iki .xsd dosyaları yer alır:

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio tüm XML şemaları ilgili klasörü içinde tutar:

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

Bu iki .xsd dosyalarını Yukarıdaki konuma taşıyın ya da yalnızca yalnızca bu şemaları Visual Studio içinde Ekle seçebilirsiniz. Ardından "Bu şemaları Visual Studio içinde ekleyebileceğiniz" **XML > şemaları > Ekle** iletişim.






