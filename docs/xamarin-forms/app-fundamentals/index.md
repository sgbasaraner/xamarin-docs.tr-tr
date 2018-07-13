---
title: Xamarin.Forms uygulaması temelleri
description: Xamarin.Forms, tüm gereken temel kavramlar aracılığıyla Son dokunuşları erişilebilirlik ve yerelleştirme gibi de dahil olmak üzere uygulama geliştirme temelleri keşfederken eğlenmeyi unutmayın.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 515dbd2683619cfcfb7a6c8ecac6bc147265ef7d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995618"
---
# <a name="xamarinforms-application-fundamentals"></a>Xamarin.Forms uygulaması temelleri

## <a name="accessibilityaccessibilityindexmd"></a>[Erişilebilirlik](accessibility/index.md)

Xamarin.Forms ile (ekran okuyucu araçları desteği gibi) erişilebilir Özellikleri birleştirmek için ipuçları.

## <a name="app-classapplication-classmd"></a>[Uygulama Sınıfı](application-class.md)

`Application` Sınıfı Xamarin.Forms için başlangıç noktası – her uygulama öğesinin alt sınıfı uygulamak gereken `App` ilk sayfayı ayarlamanızı sağlar. Ayrıca sağlar `Properties` basit veri depolama için koleksiyon. C# veya XAML içinde tanımlanabilir.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Uygulama Kullanım Ömrü](app-lifecycle.md)

`Application` Sınıfı `OnStart`, `OnSleep`, ve `OnResume` kalıcı Gezinti olayları yanı sıra, yöntemleri, özel kod ile uygulama yaşam döngüsü olaylarını işleme sağlar.

## <a name="behaviorsbehaviorsindexmd"></a>[Davranışlar](behaviors/index.md)

Kullanıcı arabirimi denetimleri, olmadan sınıflara işlevsellik eklemek için davranışları kullanarak kolayca genişletilebilir.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Özel Oluşturucular](custom-renderer/index.md)

Özel işleme 'görünümlerini ve davranışlarını (isterseniz yerel SDK'ları kullanılarak) her platformda özelleştirmek için Xamarin.Forms denetimleri varsayılan işlenmesi override' geliştiricilerin olanak sağlar.

## <a name="data-bindingdata-bindingindexmd"></a>[Veri Bağlama](data-binding/index.md)

Veri bağlama iki nesnelerin özelliklerini otomatik olarak diğer özelliğinde yansıtılmasını bir özellikte değişiklik yapılmasına olanak tanıma bağlar. Veri bağlama, Model-View-ViewModel ayrılmaz bir parçası olan ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) uygulama mimarisi.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Bağımlı hizmet](dependency-service/index.md)

`DependencyService` Basit bir Bulucu sağlar, böylece arabirimlerine paylaşılan kodunuzda kodu ve platforma özgü işlevselliği Xamarin.Forms başvuru kolaylaştıran otomatik olarak çözüldü, platforma özel uygulamalar sağlar.

## <a name="effectseffectsindexmd"></a>[Etkiler](effects/index.md)

Etkileri izin özelleştirilmek üzere her platformda yerel denetimleri ve genellikle küçük stil değişiklikler için kullanılır.

## <a name="filesfilesmd"></a>[Dosyalar](files.md)

Dosya işleme Xamarin.Forms ile kod kullanarak bir .NET Standard Kitaplığı'nda veya katıştırılmış kaynakları kullanarak gerçekleştirilebilir.

## <a name="gesturesgesturesindexmd"></a>[Hareketler](gestures/index.md)

Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) sınıfı, kullanıcı arabirimi denetimlerini dokunun, tabletinizde ve kaydırma hareketlerini destekler.

## <a name="localizationlocalizationindexmd"></a>[Yerelleştirme](localization/index.md)

Yerleşik .NET yerelleştirme framework Xamarin.Forms ile platformlar arası çok dilli uygulamalar oluşturmak için kullanılabilir.

## <a name="local-databasesdatabasesmd"></a>[Yerel Veritabanları](databases.md)

Xamarin.Forms, yüklemek ve nesneleri paylaşılan kodu kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak veritabanı odaklı uygulamaları destekler.

## <a name="messaging-centermessaging-centermd"></a>[İleti Merkezi](messaging-center.md)

Xamarin.Forms `MessagingCenter` görünüm modelleri ve diğer bileşenleri arasındaki ilişki hakkında her şeyi basit bir ileti anlaşması yanı sıra bilmek zorunda kalmadan ile iletişim kurmak için etkinleştirir.

## <a name="navigationnavigationindexmd"></a>[Gezinti](navigation/index.md)

Xamarin.Forms sağlar bağlı olarak farklı sayfa gezinti deneyimleri sayısı `Page` kullanılan yazın.

## <a name="templatestemplatesindexmd"></a>[Şablonlar](templates/index.md)

Veri şablonları desteklenen denetimlere verilerini sunumu tanımlama olanağı sunarken denetim şablonları, çalışma zamanında uygulama sayfaları kolayca teması ve re-theme olanağı sağlar.

## <a name="triggerstriggersmd"></a>[Tetikleyiciler](triggers.md)

Özellik değişiklikleri ve XAML içindeki olaylara yanıt vererek denetimleri güncelleştirin.


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
