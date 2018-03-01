---
title: Birim testi
ms.topic: article
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 0842ce24139da5d963e2c528b440e6592d5910f8
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="unit-testing"></a>Birim testi

Mobil uygulamaları, masaüstü ve web tabanlı uygulamalar hakkında endişelenmeniz gerekmez benzersiz sorunları vardır. Mobil kullanıcılar, ağ bağlantısı tarafından hizmetlerin kullanılabilirliğini ve diğer etkenlere bağlı bir dizi tarafından kullanırlar aygıtları göre farklılık gösterir. Bu nedenle, kendi kalitesini, güvenilirliğini ve performansını artırmak için gerçek dünyada bunlar kullanılacak gibi mobil uygulamaları test. Birim testi, tümleştirme testine ve kullanıcı arabirimi, birim sınama en yaygın form olan testi ile test de dahil olmak üzere, bir uygulama üzerinde gerçekleştirilmesi gereken sınama birçok türü vardır.

Birim testi uygulama, genellikle bir yöntem küçük birimi alır, kodun geri kalanı yalıtır ve beklendiği gibi davranır olduğunu doğrular. Böylece uygulama boyunca hataları yay yok işlevlerin her birimi beklendiği gibi performans denetlemek için kendi hedeftir. Nerede oluştuğunu bir hata algılama daha verimli bu hatayı dolaylı bir noktada ikincil hata etkisini gözlemleyebilirsiniz.

Yazılım geliştirme iş akışı'nın ayrılmaz bir parçası olduğunda birim testi kod kalitesini üzerinde en yüksek etkisi yoktur. Bir yöntem yazılmış olarak birim testleri kodu tarafından yapılan açık veya örtülü varsayımlar yanıt olarak standart, sınır ve giriş verileri hatalı örneklerini yöntemi ve bu onay davranışını doğrulayın yazılması gerekir. Alternatif olarak, geliştirme güdümlü test ile birim testleri kodundan önce yazılır. Bu senaryoda, birim testleri tasarım belgeleri ve işlevsel belirtimler davranır.

> [!NOTE]
> Birim testleri regresyon – çalışmak için kullanılan ancak hatalı bir güncelleştirmeyle dağıtılmış işlevselliği karşı çok etkili olur.

Birim testleri genellikle Yerleştir act assert deseni kullanın:

-   *Düzenleme* birim testi yöntemine bölümünü nesneleri başlatır ve test altındaki yöntemine geçirilen verileri değerini ayarlar.
-   *Hareket* bölüm gerekli bağımsız değişkenleriyle test altındaki yöntemi çağırır.
-   *Assert* bölüm doğrular eylem yönteminin test altındaki beklendiği gibi davranır.

Bu desen birim testleri okunabilir ve tutarlı olmasını sağlar.

## <a name="dependency-injection-and-unit-testing"></a>Bağımlılık ekleme ve birim testi

Gevşek bağlanmış bir mimari benimsenmesi için sözleri birim testi kolaylaştırır biridir. Autofac ile kayıtlı türlerinden biridir `OrderService` sınıfı. Aşağıdaki kod örneği, bu sınıfın bir özetini gösterir:

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

`OrderDetailViewModel` Sınıfı bir bağımlılığa sahip `IOrderService` yazın, ne zaman başlatır kapsayıcı çözümlenen bir `OrderDetailViewModel` nesnesi. Ancak, oluşturmak yerine bir `OrderService` birim testi nesnesine `OrderDetailViewModel` sınıfı, bunun yerine, yerine `OrderService` mock testler amacıyla nesnesiyle. Şekil 10-1, bu ilişkiyi gösterir.

![](unit-testing-images/unittesting.png "IOrderService arabirimini uygulayan sınıflar")

**Şekil 10-1:** IOrderService arabirimini uygulayan sınıflar

Bu yaklaşım sağlar `OrderService` uygulamasına geçirilecek nesne `OrderDetailViewModel` sınıfının çalışma zamanında ve Test Edilebilirlik, ilgi veren `OrderMockService` uygulamasına geçirilecek sınıfı `OrderDetailViewModel` test zaman sınıfı. Bu yaklaşımın ana avantajı, web hizmetleri veya veritabanları gibi yönetilmeleri kaynaklara gerek kalmadan yürütülmesi için birim testleri etkinleştirir ' dir.

## <a name="testing-mvvm-applications"></a>MVVM uygulamalarını test etme

Modelleri ve MVVM uygulamalardan modelleri görüntüle sınama diğer sınıflar sınama için aynıdır ve aynı araçları ve teknikleri – sınama ve mocking, birim gibi kullanılabilir. Ancak, bazı modeline tipik desenleri ve belirli birim sınama teknikleri yararlanabilir görünüm model sınıfları, vardır.

> [!TIP]
> Her birim testi ile bir şeyi sınayın. Bir birimi birden fazla en boy biriminin davranış alıştırma test yapmak için çekinmeyin. Bunun yapılması, okuma ve güncelleştirme zor olan testleri için yol gösterir. Bir hata yorumlanırken Karışıklığı önlemek için de açabilir.

EShopOnContainers mobil uygulamanın kullandığı [xUnit](https://xunit.github.io/) birim testleri iki farklı türde destekleyen birim testi, gerçekleştirmek için:

-   Bulguları olan her zaman true, testleri değişmez koşullar test edin.
-   Kuramlarý yalnızca belirli bir veri kümesi için doğru olan testleri ' dir.

Olgu testleri eShopOnContainers mobil uygulamasına dahil birim testleri olduğunu ve bu nedenle her birim test yöntemi donatılmış ile `[Fact]` özniteliği.

> [!NOTE]
> xUnit testleri test Çalıştırıcısı tarafından yürütülür. Sınama Çalıştırıcısı yürütmek için gerekli platform eShopOnContainers.TestRunner proje çalıştırın.

### <a name="testing-asynchronous-functionality"></a>Zaman uyumsuz işlevselliğini test etme

MVVM deseni uygularken, görünüm modelleri genellikle hizmetler, işlemler çoğunlukla zaman uyumsuz olarak çağırır. Bu işlem genellikle çağırır kod için testleri mocks değişiklik gerçek hizmetler için kullanın. Aşağıdaki kod örneğinde, sahte bir hizmet bir görünüm modeline geçirerek zaman uyumsuz işlevselliğini test etme gösterilmektedir:

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

Bu birim testi denetleyen `Order` özelliği `OrderDetailViewModel` örneği, sonra bir değer olacaktır `InitializeAsync` yöntemi çağrılır. `InitializeAsync` Yöntemi için Görünüm modelinin karşılık gelen görünüm gittiğinizde olduğunda çağrılır. Gezinme hakkında daha fazla bilgi için bkz: [Gezinti](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Zaman `OrderDetailViewModel` bekliyor örneği oluşturulduğunda, bir `OrderService` örneği bağımsız değişken olarak belirtilmelidir. Ancak, `OrderService` bir web hizmetinden verileri alır. Bu nedenle, bir `OrderMockService` sahte bir sürümüdür örneği, `OrderService` sınıfı, bağımsız değişkeni olarak belirtilen `OrderDetailViewModel` Oluşturucusu. Sonra Görünüm modelinin `InitializeAsync` yöntemi çağrıldığında, hangi çağırır `IOrderService` işlemleri, sahte veriler alınan yerine bir web hizmeti ile iletişime.

### <a name="testing-inotifypropertychanged-implementations"></a>INotifyPropertyChanged uygulamaları test etme

Uygulama `INotifyPropertyChanged` arabirimi sağlayan görünümünden kaynaklanan değişikliklere tepki görünümleri modelleri ve modeller. Bu değişiklikler denetimlerinde gösterilen veriler bunlarla sınırlı değildir – bunlar da başlatılması animasyonları veya denetimleri devre dışı bırakılmasına neden görünüm model durumlarının gibi görünümü denetlemek için kullanılır.

Doğrudan birim testi tarafından güncelleştirilebilir özellikleri bir olay işleyicisi ekleyerek test edilmiş `PropertyChanged` olay ve olay özelliği için yeni bir değer ayarladıktan sonra tetiklenir olup olmadığı denetleniyor. Aşağıdaki kod örneği, bu tür bir test gösterir:

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

Bu birim testi çağırır `InitializeAsync` yöntemi `OrderViewModel` sınıfı, hangi nedenler kendi `Order` güncelleştirilmesi özelliği. Birim testi geçer, aşağıdaki koşullarda `PropertyChanged` olayı için `Order` özelliği.

### <a name="testing-message-based-communication"></a>İleti tabanlı iletişim test etme

Görünüm modeller kullanan [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) arasında sınıfları birbirine sıkı şekilde bağlı olması birim test aşağıdaki kod örneğinde gösterildiği şekilde abone olarak test kodu tarafından gönderilen iletiyi iletişim kurmak için sınıfı:

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

Bu birim testi denetleyen `CatalogViewModel` yayımlar `AddProduct` yanıt iletisinde kendi `AddCatalogItemCommand` yürütülmekte. Çünkü [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) sınıfı, çok noktaya yayın ileti abonelikleri destekler, birim testi için abone olabilirsiniz `AddProduct` iletisi ve bir geri çağırma temsilcisini alırken yanıt yürütün. Lambda ifadesi belirtilen bu geri çağırma temsilcisini ayarlar bir `boolean` tarafından kullanılan alan `Assert` deyimi test davranışını doğrulayın.

### <a name="testing-exception-handling"></a>Test özel durum işleme

Birim testleri aşağıdaki kod örneğinde gösterildiği gibi belirli geçersiz eylem veya giriş için özel durumlar bu onay de yazılabilir:

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

Bu birim testi için bir özel durum atar [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim adlı bir olay yok `OnItemTapped`. `Assert.Throws<T>` Yöntemdir genel yöntem nerede `T` beklenen özel durum türü değil. Geçirilen bağımsız değişken `Assert.Throws<T>` özel durum atar bir lambda ifadesi bir yöntemdir. Lambda ifadesi oluşturur koşuluyla, bu nedenle, birim testi geçecek bir `ArgumentException`.

>💡 **İpucu**: özel durum iletisi dizeleri inceleyin birim testleri yazma kaçının. Özel durum iletisi dizeleri zaman içinde değişebilir ve bu nedenle, kendi varlığına güvenir birim testleri hassas kabul edilir.

### <a name="testing-validation"></a>Doğrulama testi

Doğrulama uygulamadan test gerçekleştirilmesi gereken iki yön vardır: tüm doğrulama kuralları doğru şekilde uygulanan sınama ve sınama `ValidatableObject<T>` sınıfı, beklendiği gibi gerçekleştirir.

Genellikle, giriş, çıkış bağlıdır kendi içinde yer alan bir işlem olduğundan doğrulama mantığını sınamak, genellikle basit bir işlemdir. Çağırma sonuçları testleri olmalıdır `Validate` en az bir ilişkili doğrulama kuralı, aşağıdaki kod örneğinde gösterildiği şekilde sahip her bir özellik yöntemi:

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

Bu birim testi doğrulama ne zaman başarılı denetler iki `ValidatableObject<T>` özelliklerinde `MockViewModel` örneği iki verilere sahip.

Doğrulama başarılı olduğunu denetleme yanı sıra, doğrulama birim testleri de değerlerini denetlemelisiniz `Value`, `IsValid`, ve `Errors` her özellik `ValidatableObject<T>` sınıfı beklendiği gibi performans doğrulamak için örneği. Aşağıdaki kod örneğinde, bunu yapan birim testi gösterilmektedir:

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

Doğrulama başarısız olur, bu birim testi denetler `Surname` özelliği `MockViewModel` herhangi bir veri yok ve `Value`, `IsValid`, ve `Errors` her özellik `ValidatableObject<T>` örneği doğru olarak ayarlayın.

## <a name="summary"></a>Özet

Birim testi uygulama, genellikle bir yöntem küçük birimi alır, kodun geri kalanı yalıtır ve beklendiği gibi davranır olduğunu doğrular. Böylece uygulama boyunca hataları yay yok işlevlerin her birimi beklendiği gibi performans denetlemek için kendi hedeftir.

Test altındaki bir nesne davranışını bağımlı nesneler bağımlı nesneler davranışını benzetimini sahte nesneler ile değiştirerek yalıtılabilir. Bu web hizmetleri veya veritabanları gibi yönetilmeleri kaynaklara gerek kalmadan yürütülmesi için birim testleri sağlar.

Modelleri ve MVVM uygulamalardan modelleri görüntüle sınama diğer sınıflar sınama için aynıdır ve aynı araçları ve teknikleri kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
