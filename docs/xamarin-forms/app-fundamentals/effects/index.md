---
title: Etkiler
description: Xamarin.Forms kullanıcı arabirimleri, her platform için uygun görünüm ve kullanımında korumak Xamarin.Forms uygulamaların izin verecek şekilde hedef platformu yerel denetimleri kullanarak işlenir. Efektler yerel denetimler için özel Oluşturucu uygulama çözümlemelere gerek kalmadan özelleştirilmek üzere her platformda izin verir.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 63d7750294321a28dbde833f72fe6b7277ada397
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="effects"></a>Etkiler

_Xamarin.Forms kullanıcı arabirimleri, her platform için uygun görünüm ve kullanımında korumak Xamarin.Forms uygulamaların izin verecek şekilde hedef platformu yerel denetimleri kullanarak işlenir. Efektler yerel denetimler için özel Oluşturucu uygulama çözümlemelere gerek kalmadan özelleştirilmek üzere her platformda izin verir._

## <a name="introduction-to-effectsintroductionmd"></a>[Giriş etkileri](introduction.md)

Efektler izin yerel denetimlere özelleştirilmek üzere her platformda ve genellikle küçük stil değişiklikler için kullanılır. Bu makalede etkileri tanıtılmaktadır, etkiler ve özel Oluşturucu arasında sınır özetler ve açıklar `PlatformEffect` sınıfı.

## <a name="creating-an-effectcreatingmd"></a>[Efekt oluşturma](creating.md)

Bir denetimin özelleştirme etkileri basitleştirin. Bu makalede, arka plan rengini değiştirir bir efekt oluşturmak gösterilmiştir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) zaman denetim odağı kazanır denetim.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Bir etkili olması için parametreleri geçirme](passing-parameters/index.md)

Parametreler ile yapılandırılmış bir efekt oluşturarak yeniden etkisi sağlar. Bu makaleler, efekt parametreleri geçirmek için özelliklerini kullanarak göstermek ve çalışma zamanında bir parametre değiştirme.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Efekt olaylarından çağırma](touch-tracking.md)

Efektler olayları çağırabilirsiniz. Bu makalede, alt düzey çok dokunma parmak izleme uygular ve dokunma basarsa, taşır ve sürümler için uygulama sinyalleri bir olay oluşturulacağını gösterir.