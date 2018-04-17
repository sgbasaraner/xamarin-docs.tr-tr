---
title: Veri ve Bulut Hizmetleri
description: Xamarin.Forms uygulamalar çok çeşitli teknolojiler kullanılarak uygulanan web hizmetlerini kullanabilir ve bu kılavuzda bunun nasıl yapılacağı inceleyeceksiniz.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="data--cloud-services"></a>Veri ve Bulut Hizmetleri

_Xamarin.Forms uygulamalar çok çeşitli teknolojiler kullanılarak uygulanan web hizmetlerini kullanabilir ve bu kılavuzda bunun nasıl yapılacağı inceleyeceksiniz._

Platformlar arası web hizmet tüketimi Xamarin platformunda giriş için bkz: [Web hizmetlerine giriş](~/cross-platform/data-cloud/web-services/index.md).

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[Örneği Anlama](~/xamarin-forms/data-cloud/walkthrough.md)

Bu makalede farklı web Hizmetleri ile iletişim kurmak gösterilmiştir Xamarin.Forms örnek uygulamayı bir kılavuz sağlar. Kapsanan konular şunlardır uygulama sayfaları, veri modeli anatomisi ve web hizmeti işlemleri çağırma.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Web Hizmetlerini Kullanma](~/xamarin-forms/data-cloud/consuming/index.md)

Bu kılavuz sağlamak için farklı bir web Hizmetleri ile iletişim gösterilmiştir oluşturma, okuma, güncelleştirme ve silme (CRUD) işlevlerinin bir Xamarin.Forms uygulaması için. Kapsanan konular şunlardır kurduğu [ASMX Hizmetleri](consuming/asmx.md), [WCF hizmetleri](consuming/wcf.md), [REST Hizmetleri](consuming/rest.md), ve [Azure Mobile Apps](consuming/azure.md).

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Web Hizmetlerine Erişim için Kimlik Doğrulaması](~/xamarin-forms/data-cloud/authentication/index.md)

Bu kılavuz, kimlik doğrulama Hizmetleri kullanıcılarının yalnızca kendi verilerine erişim yaparken bir arka uç paylaşmalarını sağlamak için bir Xamarin.Forms uygulaması şekilde entegre açıklanmaktadır. Kapsanan konular şunlardır [REST hizmeti ile temel kimlik doğrulaması kullanarak](authentication/rest.md), [Xamarin.Auth bileşen OAuth kimlik sağlayıcısı karşı kimlik doğrulaması kullanmayı](authentication/oauth.md)ve yerleşik kimlik doğrulaması kullanma tarafından sunulan mekanizmaları [Azure Mobile Apps](authentication/azure.md).

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Web Hizmetleri ile Verileri Eşitleme](sync/index.md)

Bu makalede, bir Xamarin.Forms uygulaması çevrimdışı eşitleme işlevselliği eklemek açıklanmaktadır. Çevrimdışı eşitleme sağlayan bir mobil uygulama, görüntüleme, ekleme veya verileri değiştirme ile etkileşim kullanıcılar bile bulunmadığı durumlarda bir ağ bağlantısı. Değişiklikler yerel bir veritabanında depolanır ve cihaz çevrimiçi olduktan sonra web hizmetiyle değişiklikleri eşitlenen.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[Anında İletme Bildirimleri Gönderme](push-notifications/index.md)

Bu makalede, bir Xamarin.Forms uygulaması anında iletme bildirimleri ekleme gösterilmiştir. Azure bildirim hub'ları, mobil anında iletme bildirimleri herhangi arka ucundan mobil herhangi bir platform için farklı platform bildirim sistemleri ile iletişim kurmak zorunda kalmadan bir arka uç karmaşıklığını ortadan sırasında göndermek için ölçeklenebilir bir gönderim altyapısı sağlar.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[Dosyaları Bulutta Depolama](storage/index.md)

Bu makalede Xamarin.Forms metin ve ikili verileri Azure depolama alanında depolamak için nasıl kullanılacağı ve verilere nasıl erişileceğini gösterilmektedir. Azure Storage yapılandırılmamış ve yapılandırılmış verileri depolamak için kullanılan bir ölçeklenebilir bulut depolama çözümüdür.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[Bulutta Veri Arama](search/index.md)

Bu makalede, Microsoft Azure arama kitaplığı bir Xamarin.Forms uygulamasına Azure Search tümleştirmek için nasıl kullanılacağı gösterilmektedir. Azure Search dizin oluşturma ve karşıya yüklenen veriler için özellikleri sorgulama sağlayan bir bulut hizmetidir. Bu, geleneksel bir uygulamada arama işlevini uygulama ile ilişkili arama algoritması karmaşıklık ve altyapı gereksinimleri kaldırır.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[Verileri Bir Belge Veritabanında Depolama](cosmosdb/index.md)

Bu kılavuz Azure Cosmos DB standart .NET istemci kitaplığı Azure Cosmos DB belge veritabanı bir Xamarin.Forms uygulamasına tümleştirmek için nasıl kullanılacağını gösterir. Bir Azure Cosmos DB belge veritabanı sorunsuz ölçeklendirme ve genel çoğaltma gerektiren uygulamalar için hızlı, yüksek oranda kullanılabilir, ölçeklenebilir veritabanı hizmet sunumu, JSON belgelerini düşük gecikmeli erişim sağlayan bir NoSQL veritabanıdır.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Bilişsel Hizmetler ile Zeka Katma](cognitive-services/index.md)

Bu kılavuz, bazı Microsoft Bilişsel hizmetler API'leri bir Xamarin.Forms uygulaması nasıl kullanıldığını açıklar. Bilişsel hizmetler API'leri, SDK'ları ve Hizmetleri yüz tanıma, konuşma tanıma ve dil anlama gibi özellikler ekleyerek uygulamalarını daha akıllı hale geliştiricilerin kullanımına kümesidir.
