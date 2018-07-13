---
title: Birim testi kurumsal uygulamalar
description: Bu bölümde, birim testi hizmetine mobil uygulamada nasıl gerçekleştirileceğini açıklar.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 02aeedd5498c47950e2fbc0d218de05bc0bb3204
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998689"
---
# <a name="unit-testing-enterprise-apps"></a>Birim testi kurumsal uygulamalar

Mobil uygulamaları, masaüstü ve web tabanlı uygulamalar hakkında endişelenmek zorunda olmadığınız benzersiz sorunları vardır. Mobil kullanıcılar, ağ bağlantısı tarafından hizmetlerin kullanılabilirliğini ve bir dizi etkene kullandıkları cihazları göre farklılık gösterir. Gerçek dünyada, kendi kalitesini, güvenilirliğini ve performansını geliştirmek için kullanılacağından, bu nedenle, mobil uygulamalar edilmelidir. Bir uygulamada kullanıcı arabirimi test, test en yaygın formu olan test birimi ile birim testi ve tümleştirme testine dahil olmak üzere gerçekleştirilmesi gereken test birçok türü vardır.

Bir birim testini bir yöntem genellikle uygulamanın küçük bir birim alır, kodun geri kalanını yalıtır ve beklendiği gibi çalıştığını doğrular. Böylece uygulama boyunca hataları yay yoksa her bir işlevsellik birimi beklendiği gibi performans denetlemek için hedefi sağlamaktır. Bir hatanın nerede oluştuğunu algılama daha verimli gözleme dolaylı olarak ikincil bir hata noktası konumunda bir hatanın etkisini.

Yazılım geliştirme iş akışını bir parçası olduğunda birim testi kod kalitesini en büyük etkisi. Bir yöntem yazılan hemen sonra birim testlerini kod tarafından yapılan açık veya örtülü varsayımlar yöntemi yanıt olarak standart, sınır ve giriş verisi hatalı durumda ve bu onay davranışını doğrulayın yazılmalıdır. Alternatif olarak, temelli geliştirme testi, birim testlerini kod önce yazılır. Bu senaryoda, birim testleri tasarım belgeleri ve işlevsel özellikleri davranır.

> [!NOTE]
> Birim testleri, regresyon – çalışmak için kullanılan, ancak hatalı bir güncelleştirmeyle dağıtılmış işlevselliği karşı çok etkili olur.

Birim testleri genellikle Yerleştir Yasası onaylama deseni kullanır:

-   *Düzenleme* birim sınaması metodunda bölümünü nesneleri başlatır ve yöntemi testten geçirilen verileri değerini ayarlar.
-   *Hareket* bölümünde gerekli bağımsız değişkenleriyle test altındaki yöntemi çağırır.
-   *Assert* bölümü, test altındaki yöntem eylem beklendiği gibi çalıştığını doğrular.

Bu örneği izlemeniz, birim testleri okunabilir ve tutarlı olmasını sağlar.

## <a name="dependency-injection-and-unit-testing"></a>Bağımlılık ekleme ve birim testi

Motivasyonlardan zamanı gevşek bağlanmış bir mimarisini benimsemenin için birim testi kolaylaştırır biridir. Autofac ile kayıtlı türlerinden biridir `OrderService` sınıfı. Aşağıdaki kod örneği, bu sınıfın bir özetini gösterir:

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

`OrderDetailViewModel` Sınıfı bağımlıdır `IOrderService` yazın zaman örneğini oluşturduğunda kapsayıcı gideren bir `OrderDetailViewModel` nesne. Ancak oluşturmak yerine bir `OrderService` birim testi nesnesine `OrderDetailViewModel` sınıfı, bunun yerine, değiştirin `OrderService` testler amacıyla sahte nesne. Şekil 10-1, bu ilişkiyi göstermektedir.

![](unit-testing-images/unittesting.png "IOrderService arabirimi uygulayan sınıflar")

**Şekil 10-1:** IOrderService arabirimi uygulayan sınıflar

Bu yaklaşım sağlar `OrderService` uygulamasına geçirilecek nesne `OrderDetailViewModel` sınıfı zamanında ve Test Edilebilirlik, ilgi veren `OrderMockService` uygulamasına geçirilecek sınıfı `OrderDetailViewModel` test zaman sınıfı. Ana avantajı, bu yaklaşım zahmetli kaynakları web hizmetleri veya veritabanları gibi gerek kalmadan yürütülecek birim testleri sunmasıdır.

## <a name="testing-mvvm-applications"></a>MVVM uygulamalarını test etme

Test modelleri ve görünüm modelleri MVVM uygulamalardan herhangi diğer sınıfları test aynıdır ve aynı araçları ve teknikleri – test etme ve sahte işlem, birim gibi kullanılabilir. Ancak, bazı model için tipik olan desenleri ve belirli bir birim test teknikleri yararlanabilir görünüm modeli sınıfları, vardır.

> [!TIP]
> Bir şey her bir birim testi ile test edin. Bir birim, birden fazla boyut biriminin davranış alıştırma testi yapmak için çekinmeyin. Bunun yapılması, okumak ve güncellemek zor olan testleri yol açar. Bir hata yorumlarken karışıklığa da açabilir.

Hizmetine mobil uygulama kullandığı [xUnit](https://xunit.github.io/) birim testleri iki farklı türde destekleyen birim testi, gerçekleştirmek için:

-   Her zaman true, testleri şunlardır sabit koşulların test edin.
-   Teoriler yalnızca belirli bir veri kümesi için doğru olan testlerdir.

Hizmetine mobil uygulamasına dahil birim testleri olgu testlerdir işareti ve bu nedenle her birim sınaması metodunda düzenlenmiş ile `[Fact]` özniteliği.

> [!NOTE]
> xUnit testleri bir test Çalıştırıcısı tarafından yürütülür. Test Çalıştırıcısı yürütmek için gerekli bir platform için eShopOnContainers.TestRunner proje çalıştırın.

### <a name="testing-asynchronous-functionality"></a>Zaman uyumsuz işlevselliğini test etme

MVVM düzenini hayata geçirirken, görünüm modelleri genellikle işlem Hizmetleri, genellikle zaman uyumsuz olarak çağırır. Bu işlem genellikle çağıran kod için testleri mocks değişiklik gerçek hizmetler için kullanın. Aşağıdaki kod örneğinde, sahte bir hizmet bir görünüm modeline geçirerek zaman uyumsuz işlevler test gösterilmektedir:

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

Bu birim testi denetleyen `Order` özelliği `OrderDetailViewModel` örneğine sonraki bir değere sahip `InitializeAsync` yöntemi çağrılır. `InitializeAsync` Yöntemi için karşılık gelen görünüm görünüm modelinin görüntülendiği zaman çağrılır. Gezintisi hakkında daha fazla bilgi için bkz. [Gezinti](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Zaman `OrderDetailViewModel` bekliyor örneği oluşturulduğunda bir `OrderService` örneği bağımsız değişken olarak belirtilmelidir. Ancak, `OrderService` bir web hizmetinden veri alır. Bu nedenle, bir `OrderMockService` sahte bir sürümüne olan örneği, `OrderService` sınıfı, bağımsız değişkeni olarak belirtilen `OrderDetailViewModel` Oluşturucusu. Sonra Görünüm modelinin `InitializeAsync` yöntemi çağrılır, hangi çağırır `IOrderService` işlemleri sahte veri alınan yerine bir web hizmeti ile iletişim kuran.

### <a name="testing-inotifypropertychanged-implementations"></a>INotifyPropertyChanged uygulamaları test etme

Uygulama `INotifyPropertyChanged` görünümleri görünümünden kaynaklanan değişikliklere tepki vermek arabirim sağlar modelleri ve modeller. Bu değişiklikler denetimlerinde gösterilen verileri sınırlı değildir: Bunlar animasyonları başlatılması veya denetimleri devre dışı bırakılmasına neden görünüm model durumlarının gibi görünüm denetimi için de kullanılır.

Birim testi tarafından doğrudan güncelleştirilebilir özellikleri, bir olay işleyici ekleyerek edilebilirler `PropertyChanged` olay ve olay özelliği için yeni bir değere ayarladıktan sonra oluşturup oluşturmadığını denetleniyor. Aşağıdaki kod örneği, bu tür bir test gösterilir:

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

Bu birim testi çağırır `InitializeAsync` yöntemi `OrderViewModel` sınıfı, neden olur, `Order` güncelleştirilecek özelliği. Birim testi geçer, aşağıdaki koşullarda `PropertyChanged` olayı için oluşturulur `Order` özelliği.

### <a name="testing-message-based-communication"></a>İleti tabanlı iletişim test etme

Görünüm modelleri kullanan [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) zamanı gevşek bağlanmış sınıflar arasında birim test edilebilirler aşağıdaki kod örneğinde gösterildiği gibi abone olarak test edilen kod tarafından gönderilen ileti için iletişim kurmak için sınıfı:

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

Bu birim testi denetleyen `CatalogViewModel` yayımlar `AddProduct` yanıt iletisinde kendi `AddCatalogItemCommand` yürütülmekte. Çünkü [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) sınıfı, çok noktaya yayın ileti abonelikleri destekler, birim testi için abone olabilirsiniz `AddProduct` iletisi ve alırken yanıt geri çağırma temsilcisini yürütün. Bir lambda ifadesi belirtilen bu geri çağırma temsilcisini ayarlar bir `boolean` tarafından kullanılan alanı `Assert` test davranışını doğrulamak için deyimi.

### <a name="testing-exception-handling"></a>Test bir özel durum işleme

Birim testleri aşağıdaki kod örneğinde gösterildiği gibi belirli özel durumları için geçersiz eylem veya girişleri, oluşturulan denetlemenin da yazılabilir:

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

Bu birim test bir özel durum oluşturmaz, çünkü [ `ListView` ](xref:Xamarin.Forms.ListView) denetim adlı bir olay yok `OnItemTapped`. `Assert.Throws<T>` Yöntemdir genel yöntem burada `T` beklenen özel durum türüdür. Geçirilen bağımsız değişken `Assert.Throws<T>` yöntemi olan bir lambda ifadesi bir özel durum oluşturur. Bu nedenle, lambda ifadesi atar koşuluyla birim testi geçer bir `ArgumentException`.

>💡 **İpucu**: özel durum iletisi dizeleri inceleyin birim testleri yazma kaçının. Özel durum iletisi dizeleri zaman içinde değişebilir ve bu nedenle, varlıklarını kullanan birim testleri kırılır kabul edilir.

### <a name="testing-validation"></a>Doğrulama sınaması

Doğrulama uygulaması test gereken iki unsur vardır: tüm doğrulama kurallarının doğru olarak uygulandığından test ve test `ValidatableObject<T>` beklendiği gibi sınıf gerçekleştirir.

Doğrulama mantığını genellikle burada çıkış girişine bağlı kendi içinde bir işlem olduğu için test etmek genellikle basit bir işlemdir. Çağırma sonuçları üzerinde testleri olmalıdır `Validate` yöntemini aşağıdaki kod örneğinde gösterildiği gibi en az bir ilişkili doğrulama kuralı, bulunan her bir özellik:

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

Bu birim testi doğrulama ne zaman başarılı olduğunu denetleyen iki `ValidatableObject<T>` özelliklerinde `MockViewModel` veri örneği her iki sahiptir.

Doğrulama başarılı denetlemenin yanı sıra, doğrulama birim testleri de değerini denetlemeniz gerekir `Value`, `IsValid`, ve `Errors` her özellik `ValidatableObject<T>` sınıfı beklendiği gibi performans doğrulamak için örneği. Aşağıdaki kod örneği bunu yapan birim testi gösterir:

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

Doğrulama başarısız olur, bu birim testi denetler `Surname` özelliği `MockViewModel` herhangi bir veri yoktur ve `Value`, `IsValid`, ve `Errors` her özellik `ValidatableObject<T>` örneği doğru şekilde ayarlayın.

## <a name="summary"></a>Özet

Bir birim testini bir yöntem genellikle uygulamanın küçük bir birim alır, kodun geri kalanını yalıtır ve beklendiği gibi çalıştığını doğrular. Böylece uygulama boyunca hataları yay yoksa her bir işlevsellik birimi beklendiği gibi performans denetlemek için hedefi sağlamaktır.

Bağımlı nesneler bağımlı nesnelerin davranışını benzetmekte sahte nesneler ile değiştirerek test edilen nesnenin davranışını yalıtılabilir. Bu, web hizmetleri ve veritabanları gibi kullanışsız kaynakları gerek kalmadan yürütülecek birim testleri sağlar.

Test modelleri ve görünüm modelleri MVVM uygulamalardan herhangi diğer sınıfları test aynıdır ve aynı araçları ve teknikleri kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
