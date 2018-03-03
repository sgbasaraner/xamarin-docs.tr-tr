---
title: "Kurumsal uygulama Xamarin.Forms eBook kullanarak düzenleri"
description: "Uyarlanabilir, sürdürülebilir ve sınanabilir Xamarin.Forms Kurumsal uygulamaları geliştirmek için Mimari Kılavuzu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 7ed546ac975ce1956d94d509486e4cfb25d28100
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Kurumsal uygulama Xamarin.Forms eBook kullanarak düzenleri

_Uyarlanabilir, sürdürülebilir ve sınanabilir Xamarin.Forms Kurumsal uygulamaları geliştirmek için Mimari Kılavuzu_

![](images/cover-sml.png "Kurumsal uygulama Xamarin.Forms eBook kullanarak düzenleri")

Bu e-kitap gevşek bağlantı korurken Model View ViewModel (MVVM) deseni, bağımlılık ekleme, gezinti, doğrulama ve yapılandırma yönetimi, uygulama hakkında yönergeler sağlar. Ayrıca, aynı zamanda bir yönerge yoktur gerçekleştirdiği kimlik doğrulama ve yetkilendirme kapsayıcılı mikro ve birim testi verilere IdentityServer ile.

## <a name="prefaceprefacemd"></a>[Preface](preface.md)

Bu bölümde, amacını ve kapsamını Kılavuzu ve kimlerin adresindeki hedefler açıklanmaktadır.

## <a name="introductionintroductionmd"></a>[Giriş](introduction.md)

Kurumsal uygulamaları, geliştiricilerin uygulama mimarisi geliştirme sırasında değiştirebilir birkaç güçlüklerle karşılaşır. Bu nedenle, böylece değiştiren veya zamanla genişletilmiş bir uygulama oluşturmak önemlidir. Böyle uyumluluk için tasarlama zor olabilir, ancak genellikle uygulama kolayca birlikte uygulamaya tümleştirilebilir ayrık, geniş eşleşmiş bileşenlere bölümleme ilgilidir.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Model-View-ViewModel (MVVM) deseni, uygulamanın kendi kullanıcı arabiriminden (UI) iş ve sunu mantığı düzgün bir şekilde ayırmak için yardımcı olur. Uygulama mantığı ve UI arasında temiz bir ayrım bakımı çok sayıda geliştirme sorunları gidermek için yardımcı olur ve bir uygulama sınamak için korumak ve gelişmesi daha kolay hale getirebilir. Kodu yeniden kullanın fırsatlar büyük ölçüde artırabilir ve geliştiricilerin sağlar ve bunların ilgili bölümleri Uygulama geliştirirken UI tasarımcıları daha kolayca işbirliği.

## <a name="dependency-injectiondependency-injectionmd"></a>[Bağımlılık ekleme](dependency-injection.md)

Bağımlılık ekleme, bu türlerine bağlıdır kodundan somut türleri ayırma sağlar. Genellikle kayıtlar ve arabirimleri ve soyut türler arasındaki eşlemeleri listesi tutan bir kapsayıcı ve uygulama ya da bu tür genişletmek somut türleri kullanır.

Bağımlılık ekleme kapsayıcıları sınıf örneklerini oluşturmak ve kapsayıcı yapılandırmasını temel alarak kendi ömürleri yönetmek için bir olanak sağlayarak nesneleri arasındaki ilişki azaltın. Nesne oluşturma sırasında kapsayıcı nesne gerektiren bağımlılıkları içine yerleştirir. Bu bağımlılıkların henüz oluşturulmadı, kapsayıcıyı oluşturur ve bağımlılıklarını ilk giderir.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Bileşenleri arasında kabaca iletişim eşleşmiş](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) sınıfı uygulayan Yayımla-abone nesne ve türü başvurularını bağlamak kullanışsız bileşenleri arasındaki iletişim ileti tabanlı olanak deseni. Bu mekanizma Yayımcılar ve aboneleri birbirlerine bağımsız olarak geliştirilen ve test bileşenleri de verirken bileşenleri arasındaki bağımlılıkları azaltmaya yardımcı başvuru gerek kalmadan iletişim kurmasına izin verir.

## <a name="navigationnavigationmd"></a>[Gezinme](navigation.md)

Xamarin.Forms genellikle kullanıcı Arabirimi ile kullanıcı etkileşimi veya uygulama, iç mantık temelli durumu yapılan değişiklikler sonucunda sonuçları sayfa gezintisi için destek içerir. Ancak, gezinti MVVM desenini kullanan uygulamalarda uygulamak için karmaşık olabilir.

Bu bölümde sunan bir `NavigationService` görünüm modeli ilk Gezinti görünümü modellerinden gerçekleştirmek için kullanılan sınıf. Gezinti mantığı görünümde yerleştirme modeli sınıfları gelir mantık otomatikleştirilmiş testleri denenmesi. Ayrıca, görünüm modeli sonra belirli iş kuralları zorunlu tutulmaz emin olmak için Denetim Gezinti mantığı uygulayabilirsiniz.

## <a name="validationvalidationmd"></a>[Doğrulama](validation.md)

Kullanıcı girişi kabul eden herhangi bir uygulama girişi geçerli olduğundan emin olmanız gerekir. Doğrulama olmadan bir kullanıcı uygulama başarısız olmasına neden olan veri sağlayabilir. Doğrulama iş kurallarını uygular ve bir saldırgan kötü amaçlı veriler injecting önler.

Bağlamında, Model ViewModel modeli (MVVM) deseni, bir görünüm modeli veya model genellikle veri doğrulaması yapmak ve böylece kullanıcı düzeltmenize görüntülemek için herhangi bir doğrulama hatası sinyal gerekecektir.

## <a name="configuration-managementconfiguration-managementmd"></a>[Yapılandırma Yönetimi](configuration-management.md)

Ayarlar, uygulama derlenmeden değiştirilecek davranışı izin vererek uygulama kodundan davranışını yapılandırır veri ayrımı sağlar. Uygulama ayarları, bir uygulama oluşturur ve yönetir verileri ve kullanıcı ayarları, uygulamanın davranışı etkiler ve sık sık yeniden ayarlama gerektirmeyen özelleştirilebilir bir uygulamanın ayarlardır.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Kapsayıcılı mikro](containerized-microservices.md)

Mikro uygulama geliştirme ve modern bulut uygulamaları çeviklik, ölçek ve güvenilirlik gereksinimlerine uygun dağıtım bir yaklaşım sunmaktadır. Ana avantajları mikro, bunlar bağımsız olarak, belirli bir işlevsel alan isteğe bağlı olarak gereksiz yere alanlarının ölçeklendirmeden desteklemek için daha fazla işlem gücü veya ağ bant genişliği gerektiren genişletilebilir, yani ölçeklendirilmiş olabileceğini biridir artan talep etkilenmeyen uygulama.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Kimlik doğrulama ve yetkilendirme](authentication-and-authorization.md)

Bir ASP.NET MVC web uygulamasıyla iletişim kuran bir Xamarin.Forms uygulamada kimlik doğrulama ve yetkilendirme tümleştirmek için birçok yaklaşım vardır. Burada, kimlik doğrulama ve yetkilendirme IdentityServer 4 kullanan kapsayıcılı kimlik mikro hizmet ile gerçekleştirilir. IdentityServer, taşıyıcı belirteci kimlik doğrulaması gerçekleştirmek için ASP.NET Core kimliği ile tümleşir ASP.NET Core bir açık kaynak Openıd Connect ve OAuth 2.0 çerçevedir.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Uzak verilere erişme](accessing-remote-data.md)

Birçok modern web tabanlı çözüm olun işlevselliği için Uzak istemci uygulamaları sağlamak için web sunucuları tarafından barındırılan bir web Hizmetleri kullanın. Bir web hizmeti sunan operations web API'si oluşturur ve istemci uygulamaları verileri veya API sunan işlemlerini nasıl uygulandığını bilmeden web API'si verecek olması gerekir.

## <a name="unit-testingunit-testingmd"></a>[Birim Testi](unit-testing.md)

Modelleri ve MVVM uygulamalardan modelleri görüntüle sınama diğer sınıflar sınama için aynıdır ve aynı araçları ve teknikleri kullanılabilir. Ancak, bazı modeline tipik desenleri ve belirli birim sınama teknikleri yararlanabilir görünüm model sınıfları, vardır.

## <a name="feedback"></a>Geribildirim

Bu proje üzerinde sorularınızı ve geri bildirim sağlamak bir topluluk site içeriyor. Topluluk sitesi bulunan [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Alternatif olarak, e-kitap hakkında geri bildirim için e-posta gönderilip [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
