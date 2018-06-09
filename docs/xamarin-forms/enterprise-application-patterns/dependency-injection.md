---
title: Bağımlılık ekleme
description: Bu bölümde, bağımlılık ekleme eShopOnContainers mobil uygulama bu türlerine bağlıdır kodundan somut türleri aynı şekilde nasıl kullandığı açıklanmıştır.
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fb225349b9ffb1c950486a817897b3c26c6ffbe4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242576"
---
# <a name="dependency-injection"></a>Bağımlılık ekleme

Genellikle, bir sınıf oluşturucu bir nesnesi başlatılırken çağrılır ve nesne gereken herhangi bir değeri oluşturucu bağımsız değişken olarak geçirilir. Bu, bağımlılık ekleme örneğidir ve özellikle olarak bilinen *Oluşturucu ekleme*. Nesne gereken bağımlılıkları oluşturucuya eklenmiş.

Bağımlılıklar arabirimi türleri olarak belirterek, bağımlılık ekleme somut türleri bu türlerine bağlıdır kodundan ayırma sağlar. Genellikle kayıtlar ve arabirimleri ve soyut türler arasındaki eşlemeleri listesi tutan bir kapsayıcı ve uygulama ya da bu tür genişletmek somut türleri kullanır.

Ayrıca diğer tür vardır, bağımlılık ekleme gibi *özellik ayarlayıcı ekleme*, ve *yöntemi çağrısı ekleme*, ancak daha az yaygın olarak görülen. Bu nedenle, bu bölüm, yalnızca bir bağımlılık ekleme kapsayıcısını ile Oluşturucu ekleme gerçekleştirme üzerinde odaklanır.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>Bağımlılık ekleme giriş

Bağımlılık ekleme bir özel denetimi tersine çevirme (IOC) düzeni ters sorunu gerekli bağımlılık alma işlemi olduğu sürümüdür. Bağımlılık ekleme ile çalışma zamanında bir nesneye bağımlılıkları injecting için başka bir sınıf sorumludur. Aşağıdaki örnekte gösterildiği kod nasıl `ProfileViewModel` sınıfı bağımlılık ekleme kullanırken yapılandırılmış:

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

`ProfileViewModel` Oluşturucusu alan bir `IOrderService` örneği başka bir sınıf tarafından eklenen bir bağımsız değişken olarak. Yalnızca bağımlılığı `ProfileViewModel` arabirimi türünde bir sınıftır. Bu nedenle, `ProfileViewModel` sınıfı örneği için sorumlu sınıfı bilgisi yok `IOrderService` nesnesi. Örnek oluşturma için sorumlu sınıfı `IOrderService` nesne ve içine ekleme `ProfileViewModel` sınıfı, olarak bilinen *bağımlılık ekleme kapsayıcısını*.

Bağımlılık ekleme kapsayıcıları sınıf örneklerini oluşturmak ve kapsayıcı yapılandırmasını temel alarak kendi ömürleri yönetmek için bir olanak sağlayarak nesneleri arasındaki ilişki azaltın. Nesne oluşturma sırasında kapsayıcı nesne gerektiren bağımlılıkları içine yerleştirir. Bu bağımlılıkların henüz oluşturulmadı, kapsayıcıyı oluşturur ve bağımlılıklarını ilk giderir.

> [!NOTE]
> Bağımlılık ekleme oluşturucuları kullanarak el ile de uygulanabilir. Ancak, bir kapsayıcı kullanılarak ömür yönetimi ve tarama derleme üzerinden kayıt gibi ek özellikler sağlar.

Bir bağımlılık ekleme kapsayıcısını kullanmanın bazı avantajları şunlardır:

-   Bir kapsayıcı bağımlılıklarını bulmak ve yaşam yönetmek bir sınıf gereksinimini ortadan kaldırır.
-   Bir kapsayıcı sınıfı etkilemeden uygulanan bağımlılıkları eşlenmesini sağlar.
-   Bir kapsayıcı mocked için bağımlılıkları vererek Test Edilebilirlik hızlandırır.
-   Bir kapsayıcı uygulamaya kolayca eklenecek yeni sınıflar vererek Bakımı artırır.

MVVM kullanan bir Xamarin.Forms uygulaması bağlamında, bir bağımlılık ekleme kapsayıcısını genellikle ve Hizmetleri kaydetme ve görünüm modellerini injecting kaydetme ve görünüm modelleri çözümleme için kullanılır.

Pek çok bağımlılık ekleme kapsayıcıları Autofac kullanarak görünüm modeli örneklemesi yönetmek ve uygulama sınıflarda hizmet eShopOnContainers mobil uygulama ile kullanılabilir. Autofac geniş bağlı uygulamalar oluşturmaya kolaylaştırır ve tüm yaygın olarak türü eşlemeleri ve nesne örneklerini kaydetmeye yönelik yöntemler de dahil olmak üzere bağımlılık ekleme kapsayıcılarında bulunan özellikler nesnelerini, nesne yaşam süresi yönetmek ve ekleme sağlar Oluşturucular çözümlediği nesnelerin bağımlı nesneleri. Autofac hakkında daha fazla bilgi için bkz: [Autofac](http://autofac.readthedocs.io/en/latest/index.html) readthedocs.io.

Autofac içinde `IContainer` arabirimi bağımlılık ekleme kapsayıcısını sağlar. Şekil 3-1 gösterir bağımlılıkları başlatır bu kapsayıcı kullanırken bir `IOrderService` nesne ve içine yerleştirir `ProfileViewModel` sınıfı.

![](dependency-injection-images/dependencyinjection.png "Bağımlılık ekleme kullanırken bağımlılıkları örneği")

**Şekil 3-1:** bağımlılık ekleme kullanırken bağımlılıkları

Çalışma zamanında hangi uyarlamasını kapsayıcı bilmelisiniz `IOrderService` arabirimi, örneği, örneği oluşturmak önce bir `ProfileViewModel` nesnesi. Bu içerir:

-   Uygulayan bir nesne örneği oluşturmak nasıl karar kapsayıcı `IOrderService` arabirimi. Bu olarak bilinir *kayıt*.
-   Uygulayan nesnenin başlatmasını kapsayıcı `IOrderService` arabirimi ve `ProfileViewModel` nesne. Bu olarak bilinir *çözümleme*.

Sonuç olarak, uygulamayı kullanarak bitirecek `ProfileViewModel` nesne ve olacak çöp koleksiyonu için kullanılabilir. Bu noktada, atık toplayıcı elden `IOrderService` diğer sınıflar aynı örneğini paylaşmıyorsa örneği.

> [!TIP]
> Kapsayıcı belirsiz kod yazın. Her zaman kullanılan belirli bağımlılık kapsayıcı uygulamadan aynı şekilde kapsayıcı belirsiz kod yazmayı deneyin.

## <a name="registration"></a>Kayıt

Bağımlılıklar nesneyi yerleştirilebilir önce bağımlılıkları türlerini ilk kapsayıcı ile kayıtlı olması gerekir. Genellikle bir tür kaydı bir arabirim ve arabirimini uygulayan somut bir türde kapsayıcı geçirme içerir.

Kod aracılığıyla kapsayıcısında türleri ve nesneleri kaydı iki yolu vardır:

-   Bir tür veya eşlemesi kapsayıcıyla kaydedin. Gerektiğinde, kapsayıcı belirtilen türün bir örneğini oluşturun.
-   Varolan bir nesne kapsayıcısında bir singleton olarak kaydedin. Gerektiğinde, kapsayıcı mevcut nesnesine bir başvuru döndürür.

> [!TIP]
> Bağımlılık ekleme kapsayıcıları her zaman uygun değildir. Bağımlılık ekleme karmaşıklık ve küçük uygulamalara uygun veya faydalı olmayabilir gereksinimleri sunar. Bir sınıf bağımlılıkları yok veya diğer türleri için bağımlılık değil, bu kapsayıcıda koymak için anlamlı olmayabilir. Ayrıca, bir sınıfın varsa tek bir türü için tam sayı bağımlılıkları kümesini ve hiçbir zaman değiştirir, bu kapsayıcıda koymak için anlamlı olmayabilir.

Bir uygulama içinde tek bir yöntemde bağımlılık ekleme gereksinim türlerini kaydını gerçekleştirilmelidir ve uygulama sınıflarından arasındaki bağımlılıkları farkında olduğundan emin olmak için erken uygulamanın yaşam döngüsü, bu yöntem çağrılır. EShopOnContainers mobil uygulamasında bu tarafından gerçekleştirilen `ViewModelLocator` sınıfı, hangi derlemeleri `IContainer` nesne ve bu nesneye bir başvurusu tutan uygulama yalnızca bir sınıftır. Aşağıdaki kod örneğinde eShopOnContainers mobil uygulamayı nasıl bildirir gösterir `IContainer` nesnesinde `ViewModelLocator` sınıfı:

```csharp
private static IContainer _container;
```

Türlerinin ve örneklerinin kayıtlı içinde `RegisterDependencies` yönteminde `ViewModelLocator` sınıfı. Bu ilk oluşturarak elde edilir bir `ContainerBuilder` aşağıdaki kod örneğinde gösterildiği örnek:

```csharp
var builder = new ContainerBuilder();
```

Türlerinin ve örneklerinin sonra kayıtlı ile `ContainerBuilder` nesne ve aşağıdaki kod örneğinde en yaygın formun türü kayıt gösterir:

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

`RegisterType` Burada gösterilen yöntemi somut bir türde bir arabirim türü eşler. Örneği oluşturmak için kapsayıcı söyleyen bir `RequestProvider` bir eklenmesine gerektiren bir nesne başlattığında nesne bir `IRequestProvider` aracılığıyla bir oluşturucu.

Somut türleri de bir arabirim türüne bir eşlemeyi olmadan doğrudan aşağıdaki kod örneğinde gösterildiği gibi kaydedilebilir:

```csharp
builder.RegisterType<ProfileViewModel>();
```

Zaman `ProfileViewModel` türdür çözülmüş, kapsayıcı gerekli bağımlılıkları ekleme.

Autofac kapsayıcı türü tek bir örneğini başvuru bakımından sorumlu olduğu örneği kayıt de sağlar. Örneğin, aşağıdaki kod örneğinde eShopOnContainers mobil uygulamayı nasıl kullanmak üzere somut türde kaydeder gösterir bir `ProfileViewModel` örneği gerektirir bir `IOrderService` örneği:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

`RegisterType` Burada gösterilen yöntemi somut bir türde bir arabirim türü eşler. `SingleInstance` Yöntemi her bağımlı nesne aynı paylaşılan örnek alması kayıt yapılandırır. Bu nedenle, yalnızca tek bir `OrderService` örneği var, bir ekleme gerektiren nesneler tarafından paylaşılan kapsayıcısındaki bir `IOrderService` aracılığıyla bir oluşturucu.

Örnek kayıt de yapılabilir ile `RegisterInstance` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

`RegisterInstance` Burada gösterilen yöntemi oluşturur Yeni bir `OrderMockService` örneği ve kapsayıcı ile kaydeder. Bu nedenle, yalnızca tek bir `OrderMockService` örneği varsa, bir ekleme gerektiren nesneler tarafından paylaşılan kapsayıcısındaki bir `IOrderService` aracılığıyla bir oluşturucu.

Kayıt türü ve örnek aşağıdaki `IContainer` nesne gerekir oluşturulur, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
_container = builder.Build();
```

Çağırma `Build` yöntemi `ContainerBuilder` örneği yapılmıştır kayıtlar içeren yeni bir bağımlılık ekleme kapsayıcısını oluşturur.

>💡 **İpucu**: göz önünde bulundurun bir `IContainer` olarak değişmez olacak. Autofac sağlarken bir `Update` yöntemi bu yöntemi çağırmadan var olan bir kapsayıcıyı kayıtları güncelleştirmek için mümkün olduğunda kaçınılmalıdır. Yerleşik olan sonra kapsayıcı bir özellikle kullanılması durumunda için bir kapsayıcı değiştirme riskleri vardır. Daha fazla bilgi için bkz: [kapsayıcı Immutable olarak göz önünde bulundurun](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) readthedocs.io.

<a name="resolution" />

## <a name="resolution"></a>Çözüm

Bir tür kaydedildikten sonra Çözüldü veya bağımlılık olarak eklenmiş. Yeni bir örneğini oluşturmak kapsayıcı türü çözümlendi ve gerektiğinde örneğine bağımlılıkları yerleştirir.

Genellikle, bir tür çözümlendiğinde işlemlerden birini olur:

1.  Tür kayıtlı taşınmadığından, kapsayıcı bir özel durum oluşturur.
1.  Türü bir singleton olarak kayıtlı olup olmadığını kapsayıcı singleton örneğini döndürür. Türü için çağrılır ilk kez kullanıyorsanız, kapsayıcı gerekirse oluşturur ve ona başvuru korur.
1.  Türü bir singleton olarak kayıtlı taşınmadığından, kapsayıcı yeni bir örneğini döndürür ve kendisine bir başvuru bilgisini korumaz.

Aşağıdaki örnekte gösterildiği kod nasıl `RequestProvider` Autofac ile daha önceden kaydedilen türü çözümlenen olabilir:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

Bu örnekte, Autofac için somut türü çözümlenemedi istenir `IRequestProvider` tüm bağımlılıklarla birlikte türü. Genellikle, `Resolve` yöntemi, belirli bir türün bir örneği gerektiğinde çağrılır. Çözümlenen nesnelerin ömrü denetleme hakkında daha fazla bilgi için bkz: [ömrü çözülmüş nesnelerin yönetme](#managing_the_lifetime_of_resolved_objects).

Aşağıdaki kod örneği, görünüm modeli türlerini ve bunların bağımlılıklarını eShopOnContainers mobil uygulamayı nasıl başlatır gösterir:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

Bu örnekte, Autofac görünüm model türü için istenen görünüm modeli çözmek istenir ve kapsayıcı bağımlılıkları da çözer. Çözülürken `ProfileViewModel` çözümlemek için bağımlılık türüdür bir `IOrderService` nesnesi. Bu nedenle, Autofac ilk oluşturan bir `OrderService` nesne ve oluşturucusuna geçirir `ProfileViewModel` sınıfı. Modeller ve görünümlere ilişkilendirir nasıl eShopOnContainers mobil uygulama görünümü oluşturur hakkında daha fazla bilgi için bkz [otomatik olarak bir görünüm modeli bulucuyla görünüm Model oluşturma](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

> [!NOTE]
> Kaydetme ve bir kapsayıcı türleriyle çözme özellikle bağımlılıkları uygulamadaki her sayfa gezinti için yeniden, her tür oluşturmak için kapsayıcının kullanımını yansıma nedeniyle bir performans vardır. Birçok veya derin bağımlılıkları varsa, oluşturma maliyetini önemli ölçüde artırabilir.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>Çözümlenen nesnelerin ömrü yönetme

Bir tür kaydolduktan sonra varsayılan için Autofac türü her zaman kayıtlı türün yeni bir örneğini oluşturmak için çözümlenmiş ya da zaman bağımlılık mekanizması örnekleri diğer sınıflara yerleştirir davranıştır. Bu senaryoda, kapsayıcı çözümlenmiş nesneye bir başvurusu tutun değil. Ancak, bir örnek kaydedilirken Autofac için varsayılan davranış singleton olarak nesnesinin ömrü yönetmektir. Bu nedenle, örnek kapsayıcı kapsamında olduğunu ve kapsayıcının kapsamı dışında gider ve atık toplanan atıldı çalışırken ya da kod açıkça kapsayıcı siler kapsam içinde kalır.

Bir Autofac örneği kapsamı, kayıtlı bir türden Autofac oluşturan bir nesne için singleton davranışı belirtmek için kullanılabilir. Kapsayıcı tarafından örneği nesne yaşam süresi Autofac örneği kapsamlarını yönetme. Varsayılan örneği kapsamın `RegisterType` yöntemi `InstancePerDependency` kapsam. Ancak, `SingleInstance` kapsam ile kullanılabilir `RegisterType` yöntemi, böylece kapsayıcı oluşturur veya çağrılırken bir tür tek örneğini döndüren `Resolve` yöntemi. Aşağıdaki kod örneğinde tek örneğini oluşturmak için Autofac nasıl talimat gösterir `NavigationService` sınıfı:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

İlk kez `INavigationService` arabirimi çözülmüş, yeni bir kapsayıcı oluşturur `NavigationService` nesne ve kendisine bir başvuru korur. Sonraki tüm çözümler üzerinde `INavigationService` arabirimi, kapsayıcı bir başvuru döndürür `NavigationService` daha önce oluşturulmuş nesne.

> [!NOTE]
> Kapsayıcı çıkarıldığından daıtmaktadır kapsam oluşturulan nesneleri siler.

Autofac ek örnek kapsamları içerir. Daha fazla bilgi için bkz: [örneği kapsam](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) readthedocs.io.

## <a name="summary"></a>Özet

Bağımlılık ekleme, bu türlerine bağlıdır kodundan somut türleri ayırma sağlar. Genellikle kayıtlar ve arabirimleri ve soyut türler arasındaki eşlemeleri listesi tutan bir kapsayıcı ve uygulama ya da bu tür genişletmek somut türleri kullanır.

Autofac geniş bağlı uygulamalar oluşturmaya kolaylaştırır ve tüm yaygın olarak türü eşlemeleri ve nesne örneklerini kaydetmeye yönelik yöntemler de dahil olmak üzere bağımlılık ekleme kapsayıcılarında bulunan özellikler nesnelerini, nesne yaşam süresi yönetmek ve ekleme sağlar Oluşturucular çözümlediği nesnelerin bağımlı nesneleri.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
