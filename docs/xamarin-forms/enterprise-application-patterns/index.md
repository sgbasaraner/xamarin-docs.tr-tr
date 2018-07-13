---
title: Xamarin.Forms e-kitap kullanarak kurumsal uygulama desenleri
description: Bu e-kitap uyarlanabilir, sürdürülebilir ve test edilebilir Xamarin.Forms Kurumsal uygulamaları geliştirmek için Mimari Rehber sağlar.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ecfe99f66e16eafabc3117036ff065e3a35259c3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994354"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.Forms e-kitap kullanarak kurumsal uygulama desenleri

_Uyarlanabilir, sürdürülebilir ve test edilebilir Xamarin.Forms Kurumsal uygulamaları geliştirmek için Mimari kılavuzluğu_

![](images/cover-sml.png "Xamarin.Forms e-kitap kullanarak kurumsal uygulama desenleri")

Bu e-kitap, gevşek bağ modelini korurken, Model-View-ViewModel (MVVM) desenini, bağımlılık ekleme, gezinti, doğrulama ve yapılandırma yönetimi, uygulama hakkında yönergeler sağlanır. Ayrıca, de mevcuttur kılavuzu üzerinde kimlik doğrulaması ve yetkilendirme ile kapsayıcılı mikro hizmetler ve birim testi verilerine erişme, IdentityServer gerçekleştirme.

## <a name="prefaceprefacemd"></a>[Önsöz](preface.md)

Bu bölümde, amacını ve kapsamını Kılavuzu ve en hedefler açıklanmaktadır.

## <a name="introductionintroductionmd"></a>[Giriş](introduction.md)

Kurumsal uygulama geliştiriciler, geliştirme sırasında uygulama mimarisinden değiştirebilirsiniz birkaç sorunlarla karşılaşıyor. Bu nedenle, değiştiren veya genişletilmiş zaman içinde bir uygulama oluşturmak önemlidir. Bu tür uyumluluk için tasarlama zor olabilir, ancak genellikle bir uygulamayı kolayca birlikte uygulamalarla tümleştirilebilir ayrık, gevşek bileşenlere bölümleme içerir.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Model-View-ViewModel (MVVM) desenini düzgün bir şekilde iş ve bir sunu mantıksal uygulamanın, kullanıcı arabiriminden (UI) ayrı yardımcı olur. Uygulama mantığı ve UI arasında NET bir ayrım koruma çok sayıda geliştirme sorunları gidermeye yardımcı olur ve uygulama sınamak için korumak ve geliştirmek daha kolay hale getirebilirsiniz. Kodu yeniden kullanma fırsatlarını büyük ölçüde geliştirebilir ve geliştiricilerin sağlar ve daha fazla bilgi için kullanıcı Arabirimi tasarımcıları, ilgili bölümleri Uygulama geliştirirken kolayca işbirliği yapın.

## <a name="dependency-injectiondependency-injectionmd"></a>[Bağımlılık Ekleme](dependency-injection.md)

Bağımlılık ekleme, bu türlerine bağlıdır koddan somut tür ayırma sağlar. Genellikle, kayıtlar ve arabirimleri ile soyut türler arasındaki eşlemeleri listesini tutan kapsayıcı ve uygulamak ya da bu türleri genişleten somut türler de kullanır.

Bağımlılık ekleme kapsayıcıları sınıf örneklerini oluşturmak ve kapsayıcıya yapılandırmasına bağlı olarak kendi ömürlerine yönetmek için bir olanağı sağlayarak nesneler arasında bağ azaltın. Nesne oluşturma sırasında kapsayıcı nesne gerektiren herhangi bir bağımlılığın içine yerleştirir. Bu bağımlılıkların henüz oluşturulmadı, kapsayıcı oluşturur ve ilk bağımlılıklarını çözümler.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Gevşek Bir Şekilde Eşlenen Bileşenler Arasında İletişim](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) sınıfın uyguladığı Yayımla-abone ol modeli, nesne ve tür başvurularını bağlamak uygun olan bileşenleri arasında ileti tabanlı iletişim sağlar. Bu mekanizma, Yayımcılar ve aboneler bir başvuru bağımsız olarak geliştirilen ve test bileşenlerini de verirken bileşenleri arasındaki bağımlılıkları azaltmaya yardımcı olur, birbiriyle olmadan iletişim kurmasına izin verir.

## <a name="navigationnavigationmd"></a>[Gezinti](navigation.md)

Xamarin.Forms, genellikle kullanıcı Arabirimi ile kullanıcı etkileşimi veya iç mantık temelli durumu yapılan değişiklikler sonucunda uygulama içinden sonucunda sayfa gezintisi için destek içerir. Ancak, gezinti MVVM düzenini kullanan uygulamalarda uygulamak için karmaşık olabilir.

Bu bölümde sunan bir `NavigationService` görünümü modellerinden görünüm model-first Gezinti gerçekleştirmek için kullanılan sınıf. Gezinti mantığı görünümde yerleştirilmesi, model sınıfları anlamına gelir mantık otomatik testler uygulanabilecek. Ayrıca, görünüm modeli ardından belirli iş kuralları uygulanmasını sağlamak için denetimi Gezinti mantığı uygulayabilir.

## <a name="validationvalidationmd"></a>[Doğrulama](validation.md)

Kullanıcıların girişi kabul eden herhangi bir uygulama, giriş geçerli olduğundan emin olmanız gerekir. Doğrulama, bir kullanıcı, uygulamanın başarısız olmasına neden olan veri sağlayabilirsiniz. Doğrulama, iş kuralları zorunlu kılar ve bir saldırgan kötü amaçlı veri ekleme öğesinden engeller.

Bağlam, Model ViewModel Model (MVVM) deseni, bir görünüm modeli veya model genellikle veri doğrulaması gerçekleştir ve böylece kullanıcı bunları düzeltmek için kullanabileceğiniz tüm doğrulama hatalarını görünümüne sinyal gerekecektir.

## <a name="configuration-managementconfiguration-managementmd"></a>[Yapılandırma Yönetimi](configuration-management.md)

Ayarlar, uygulamanın derlenmeden değiştirilecek davranış sağlayan bir kodu uygulamadan davranışını yapılandıran veri ayrımı sağlar. Uygulama ayarları, bir uygulama oluşturur ve yönetir verilerinin ve uygulama davranışını etkiler ve sık sık yeniden ayarlama gerektirmeyen özelleştirilebilir ayarları bir uygulamanın kullanıcı ayarlarıdır.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Kapsayıcılı Mikro Hizmetler](containerized-microservices.md)

Mikro hizmetler, uygulama geliştirme ve modern bulut uygulamaları çeviklik, ölçek ve güvenilirlik gereksinimleri için uygun olan dağıtım için bir yaklaşım sunar. Mikro hizmetlerin başlıca avantajlarından biri, bağımsız olarak, belirli bir işlevsel alan talep alanları gereksiz yere yeniden ölçeklendirmeden desteklemek için daha fazla işleme güç veya ağ bant genişliği gerektiren ölçeklendirilebilir, yani ölçeği genişletilen yardımcı olması açısından önemlidir artan talebi yaşamadığınızdan uygulama.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Kimlik Doğrulaması ve Yetkilendirme](authentication-and-authorization.md)

Kimlik doğrulama ve yetkilendirme, bir ASP.NET MVC web uygulaması ile iletişim kuran bir Xamarin.Forms uygulamasına tümleştirmek için birçok yaklaşım vardır. Burada, kimlik doğrulama ve yetkilendirme IdentityServer 4 kullanan bir kimlik kapsayıcılı mikro gerçekleştirilir. IdentityServer bir açık kaynak Openıd Connect ve OAuth 2.0 taşıyıcı belirteci kimlik doğrulaması gerçekleştirmek için ASP.NET Core kimliği ile tümleşen bir ASP.NET Core çerçevesidir.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Uzak Verilere Erişme](accessing-remote-data.md)

Birçok modern web tabanlı çözümlerine işlevselliği için Uzak istemci uygulamaları sağlamak için web sunucuları tarafından barındırılan web hizmetleri. Bir web API'sini bir web hizmetini gösteren işlemleri oluşturur ve istemci uygulamaları veri veya API kullanıma sunduğu işlemleri nasıl uygulandığını farkında olmadan web API'sini kullanmaya başlayabilirsiniz olmalıdır.

## <a name="unit-testingunit-testingmd"></a>[Birim Testi](unit-testing.md)

Test modelleri ve görünüm modelleri MVVM uygulamalardan herhangi diğer sınıfları test aynıdır ve aynı araçları ve teknikleri kullanılabilir. Ancak, bazı model için tipik olan desenleri ve belirli bir birim test teknikleri yararlanabilir görünüm modeli sınıfları, vardır.

## <a name="feedback"></a>Geribildirim

Bu proje üzerinde sorularınızı ve geri bildirim sağlayan bir topluluk sitesine sahiptir. Topluluk sitesine bulunan [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Alternatif olarak, e-kitap hakkında geri bildirim için gönderilebilir [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
