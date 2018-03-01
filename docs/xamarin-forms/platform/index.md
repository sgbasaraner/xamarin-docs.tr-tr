---
title: "Platform özellikleri"
description: "Xamarin.Forms ile platforma özgü özelliklerden yararlanabilir"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/20/2017
ms.openlocfilehash: 950cc4a8611b05c22825ef89a85827fa0d3e5f7b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="platform-features"></a>Platform özellikleri

Xamarin.Forms genişletilebilir ve birleştirme platforma özgü özellikleri kullanarak olanak tanır [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md), [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)ve daha fazlası.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

Bu kılavuz, mevcut Xamarin.Forms Android uygulamaları güncelleştirerek malzeme tasarımını uygulamak açıklar.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[Uygulama dizin oluşturma ve derin bağlama](deep-linking.md)

Uygulama dizin arama sonuçlarında görünmesini ilgili kalmak için birkaç kullandıktan sonra Aksi takdirde unutulursa uygulamaları sağlar. Derin bağlama uygulamaların uygulama verilerini içeren, genellikle derin bir bağlantıdan başvurulan sayfasına giderek arama sonucu yanıt izin verir.

## <a name="device-classdevicemd"></a>[Aygıt sınıfı](device.md)

Nasıl kullanılacağını `Device` platforma özgü davranış paylaşılan kodu ve (XAML kullanılarak dahil) kullanıcı arabirimini oluşturmak için sınıfı. Ayrıca kapsayan `BeginInvokeOnMainThread` arka plan iş parçacıkları kullanıcı Arabirimi denetimlerini değiştirirken gerekli olduğu.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

Bazı iOS stil aracılığıyla gerçekleştirilebilir **Info.plist** ve `UIAppearance` API. Bu kılavuz, çekirdek Spotlight arama da dahil olmak üzere bir Xamarin.Forms çözümünü iOS uygulamaya iOS 9 özellikleri içerecek şekilde nasıl örnekleri içerir.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms artık macOS uygulamaları için Önizleme destek sahiptir.

## <a name="native-formsnative-formsmd"></a>[Yerel formlar](native-forms.md)

Yerel Forms izin Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-yerel Xamarin.iOS, Xamarin.Android ve evrensel Windows Platformu (UWP) projeler tarafından tüketilen sayfalarına türetilmiş.

## <a name="native-viewsnative-viewsindexmd"></a>[Yerel görünümleri](native-views/index.md)

İOS, Android ve evrensel Windows platformu yerel görünümleri doğrudan Xamarin.Forms başvurulabilir. Xamarin.Forms görünümleri ile etkileşim kurabilir ve özellikleri ve olay işleyicileri yerel görünümler üzerinde ayarlanabilir.

## <a name="platform-specificsplatform-specificsindexmd"></a>[Platform-Specifics](platform-specifics/index.md)

Platform özellikleri yalnızca özel oluşturucu veya efektler gerektirmeden belirli bir platformda kullanılabilir olan işlevsellik kullanmasına olanak sağlar.

## <a name="pluginspluginsmd"></a>[Eklentileri](plugins.md)

Çok çeşitli açık kaynak eklentileri Github, Nuget ve Xamarin bileşen Deposu'nda Xamarin.Forms uygulamaları uzatmaya yardımcı olmak için kullanılabilir.

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms dört farklı türde Windows projesi için destek içerir:

* Windows Phone 8 Silverlight (Xamarin.Forms tarafından desteklenen orijinal Windows platformuna)
* Windows Phone 8.1 (WinRT),
* Windows 8.1 (WinRT) ve
* Evrensel Windows Platformu (Windows 10).

Bu bölümde, bunları ve varolan Xamarin.Forms çözümünü ekleme arasındaki farklar açıklanmaktadır.
