---
title: Android Platform Özellikleri
description: Bu makalede Xamarin.Forms uygulamalarına Android özgü işlevsellik ekleme açıklar ve malzeme tasarımına odaklanmıştır.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 2eada518586f222d200ec19aeddc65107d7603b3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242410"
---
# <a name="android-platform-features"></a>Android Platform Özellikleri

## <a name="platform-support"></a>Platform Desteği

Xamarin.Forms Android projesi varsayılan önce Android 5.0 ortak denetim renderering, daha eski olan bir stil kullanır. Şablon kullanılarak oluşturulan uygulamaların sahip `FormsApplicationActivity` ana etkinliklerini temel sınıf olarak.

## <a name="material-design-via-appcompat"></a>Uygulama aracılığıyla malzeme tasarım

Xamarin.Forms de sahip isteğe bağlı bir `FormsAppCompatActivity` kullanan **uygulama** malzeme tasarım temaları uygulamak için Android tarafından sağlanan özellikleri.

Malzeme tasarım temaları Xamarin.Forms Android projenize eklemek için izleyin [uygulama için yükleme yönergeleri desteği](appcompat.md)

Burada **Yapılacaklar** varsayılan örnek `FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "Yapılacaklar örnek uygulamayı uygulama olmadan")](images/before-appcompat.png#lightbox "uygulama olmadan Yapılacaklar örnek uygulama")

Ve bunlar aynı kodu kullanmak için proje yükselttikten sonra `FormsAppCompatActivity` (ve ek tema bilgileri ekleme):

[![](images/post-appcompat-sml.png "Uygulama ve tema ile Yapılacaklar örnek uygulaması")](images/post-appcompat.png#lightbox "uygulama ve tema ile Yapılacaklar örnek uygulaması")

> [!NOTE]
> Kullanırken `FormsAppCompatActivity`, [temel sınıflar için Android bazı özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) farklı olacaktır.


## <a name="related-links"></a>İlgili bağlantılar

- [Malzeme tasarım desteği ekleme](appcompat.md)
