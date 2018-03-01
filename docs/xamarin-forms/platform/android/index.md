---
title: "Android platformu özellikleri"
description: "Xamarin.Forms uygulamalarına Android özel işlevsellik ekleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: d68d84671028ded14b4b885f2c134656fc639f9e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="android-platform-features"></a>Android platformu özellikleri

## <a name="platform-support"></a>Platform Desteği

Xamarin.Forms Android projesi varsayılan önce Android 5.0 ortak denetim renderering, daha eski olan bir stil kullanır. Şablon kullanılarak oluşturulan uygulamaların sahip `FormsApplicationActivity` ana etkinliklerini temel sınıf olarak.

## <a name="material-design-via-appcompat"></a>Uygulama aracılığıyla malzeme tasarım

Xamarin.Forms de sahip isteğe bağlı bir `FormsAppCompatActivity` kullanan **uygulama** malzeme tasarım temaları uygulamak için Android tarafından sağlanan özellikleri.

Malzeme tasarım temaları Xamarin.Forms Android projenize eklemek için izleyin [uygulama için yükleme yönergeleri desteği](appcompat.md)

Burada **Yapılacaklar** varsayılan örnek `FormsApplicationActivity`:

[ ![](images/before-appcompat-sml.png "Yapılacaklar örnek uygulamayı uygulama olmadan")](images/before-appcompat.png "uygulama olmadan Yapılacaklar örnek uygulama")

Ve bunlar aynı kodu kullanmak için proje yükselttikten sonra `FormsAppCompatActivity` (ve ek tema bilgileri ekleme):

[ ![](images/post-appcompat-sml.png "Uygulama ve tema ile Yapılacaklar örnek uygulaması")](images/post-appcompat.png "uygulama ve tema ile Yapılacaklar örnek uygulaması")

> [!NOTE]
> **Not**: kullanırken `FormsAppCompatActivity`, [temel sınıflar için Android bazı özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) farklı olacaktır.


## <a name="related-links"></a>İlgili bağlantılar

- [Malzeme tasarım desteği ekleme](appcompat.md)
