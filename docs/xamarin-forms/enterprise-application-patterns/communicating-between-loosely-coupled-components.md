---
title: Bileşenleri arasında kabaca iletişim eşleşmiş
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 01669573f215c5a13bb918c9f9ba80aa5ca528c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="communicating-between-loosely-coupled-components"></a>Bileşenleri arasında kabaca iletişim eşleşmiş

Yayımla-abone olma deseni olduğundan, yayımcılar ileti gönderme aboneleri bilinen tüm alıcıları bilgisi olmadan bir Mesajlaşma düzeni. Benzer şekilde, aboneler, tüm yayımcılar bilgi gerek kalmadan belirli iletiler için dinleme.

.NET olayları uygulama Yayımla-düzeni abone olmak ve en basit ve kolay bir yaklaşım iletişim katmanı gevşek bağlantı, bir denetim ve içerdiği sayfa gibi gerekli değilse bileşenler arasındaki şunlardır. Ancak, yayımcısı ve abonesi yaşam süreleri nesne başvuruları birbirlerine bağlı değildir ve abone türü yayımcı türüne bir başvuru olması gerekir. Özellikle statik veya uzun süreli bir nesneyi bir olaya abone kısa süreli nesneleri olduğunda bu bellek yönetimi sorunlarını oluşturabilirsiniz. Olay işleyicisi kaldırılmaz, abone tarafından kendisine başvuru Publisher'da Canlı tutulacak ve bu engellemek veya abonenin çöp toplama gecikme.

## <a name="introduction-to-messagingcenter"></a>MessagingCenter giriş

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) sınıfı uygulayan Yayımla-abone nesne ve türü başvurularını bağlamak kullanışsız bileşenleri arasındaki iletişim ileti tabanlı olanak deseni. Bu mekanizma Yayımcılar ve aboneleri birbirlerine bağımsız olarak geliştirilen ve test bileşenleri de verirken bileşenleri arasındaki bağımlılıkları azaltmaya yardımcı başvuru gerek kalmadan iletişim kurmasına izin verir.

[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Sınıfı, çok noktaya yayın yayımlama-abone olma işlevselliği sağlar. Bunun anlamı, tek bir ileti yayımlama birden çok yayımcılar olabilir ve aynı ileti için dinleme birden çok abone olabilir. Şekil 4-1, bu ilişkiyi göstermektedir:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Çok noktaya yayın yayımlama-abone olma işlevi")

**Şekil 4-1:** çok noktaya yayın yayımlama-abone olma işlevi

Yayımcılar kullanarak ileti gönderme [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) aboneleri kullanarak iletiler için dinleme sırada yöntemi [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) yöntemi. Ayrıca, aboneleri de ileti aboneliklerden gerekirse, aboneliğinizi iptal edebilirsiniz ile [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) yöntemi.

Dahili olarak, [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) sınıfı, zayıf başvurular kullanır. Bu, nesneleri canlı tutmayacaktır ve toplanacak imkan tanıyacak anlamına gelir. Bu nedenle, bu yalnızca bir sınıf artık iletisi istediğinde iletiden aboneliğinizi iptal etmek gerekli olması gerekir.

EShopOnContainers mobil uygulamanın kullandığı [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) arasında kabaca iletişim kurmak için sınıf bağlı bileşenler. Uygulama üç iletileri tanımlar:

-   `AddProduct` İleti tarafından yayımlanır `CatalogViewModel` alışveriş sepetine bir öğe eklendiğinde sınıfı. Buna karşılık, `BasketViewModel` sınıfı için ileti abone olur ve yanıt alışveriş sepetine öğe sayısını artırır. Ayrıca, `BasketViewModel` sınıfı ayrıca bu iletiden aboneliğini kaldırır.
-   `Filter` İleti tarafından yayımlanır `CatalogViewModel` sınıfı kullanıcı kataloğunda görüntülenen öğelerin bir marka veya türü filtrenin uygulandığı zaman. Buna karşılık, `CatalogView` sınıfı için ileti abone olur ve böylece yalnızca filtre ölçütüyle eşleşen öğeleri görüntülenir kullanıcı arabirimini güncelleştirir.
-   `ChangeTab` İleti tarafından yayımlanır `MainViewModel` sınıfı `CheckoutViewModel` gider `MainViewModel` başarılı oluşturulması ve yeni bir sipariş teslimini aşağıdaki. Buna karşılık, `MainView` sınıfı abone ileti ve güncelleştirmeleri UI bu nedenle, **Profilim** sekmesinde kullanıcının siparişleri göstermek için etkin olduğunu.

> [!NOTE]
> Sırada [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) sınıfı sınıfları birbirine sıkı şekilde bağlı iletişimine izin verir, bu sorun yalnızca mimari çözüme sağlamaz. Örneğin, bir görünüm modeli bir görünüm arasında iletişimi bağlama altyapısı ve özellik değişikliği bildirimlerini aracılığıyla gerçekleştirilebilir. Ayrıca, iki görünüm modeli arasındaki iletişim de gezinti sırasında veri ileterek yararlanılabilir.

EShopOnContainers mobil uygulamaya[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) UI yanıt başka bir sınıf oluşan bir eylem olarak güncelleştirmek için kullanılır. Bu nedenle, iletileri kullanıcı Arabirimi iş parçacığı üzerinde iş parçacığı mesaj alarak aboneleri yayımlanır.

> [!TIP]
> UI gerçekleştirme güncelleştirdiğinde için kullanıcı Arabirimi iş parçacığı sıralama. Arka plan iş parçacığından gönderilen bir ileti UI güncelleştirmek için gerekliyse, harekete geçirerek ileti abone UI iş parçacığı üzerinde işlem [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) yöntemi.

Hakkında daha fazla bilgi için [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), bkz: [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Bir ileti tanımlama

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) iletileri iletileri tanımlamak için kullanılan dizelerdir. Aşağıdaki kod örneğinde eShopOnContainers mobil uygulama içinde tanımlanan iletilerini gösterir:

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

Bu örnekte, iletilerin sabitleri kullanılarak tanımlanır. Bu yaklaşımın avantajı derleme zamanı tür güvenliği ve Destek yeniden düzenleme sağlamasıdır.

## <a name="publishing-a-message"></a>Bir ileti yayımlama

Yayımcılar bildirim aboneleri birini içeren bir ileti [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) aşırı. Aşağıdaki kod örneğinde yayımlama gösterilmektedir `AddProduct` ileti:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

Bu örnekte, [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) yöntemi üç bağımsız değişken belirtir:

-   İlk bağımsız değişken gönderen sınıfı belirtir. Gönderen sınıfı ileti almak isteyen herhangi aboneler tarafından belirtilmesi gerekir.
-   İkinci bağımsız değişkeni iletiyi belirtir.
-   Üçüncü bağımsız değişken aboneye gönderilmek üzere yük verilerinin belirtir. Bu durumda yükü veridir bir `CatalogItem` örneği.

[ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Yöntemi, ileti ve yangın ve unut bir yaklaşım kullanarak yükü verilerini yayımlar. Bu nedenle, ileti almak için kayıtlı hiçbir aboneleri olsa bile iletisi gönderilir. Bu durumda, gönderilen iletiyi göz ardı edilir.

> [!NOTE]
> [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Yöntemi iletilerin nasıl teslim denetlemek için genel parametreleri kullanabilirsiniz. Bu nedenle, bir ileti kimliği paylaşır, ancak farklı yükü veri türleri Gönder birden fazla ileti farklı aboneler tarafından alınabilir.

## <a name="subscribing-to-a-message"></a>İleti abone olma

Aboneler birini kullanarak bir ileti almak için kaydolabilir [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) aşırı. Aşağıdaki kod örneğinde eShopOnContainers mobil uygulama abone olur ve nasıl işlediği gösterilmektedir `AddProduct` ileti:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

Bu örnekte, [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) yöntemi abone olduğu `AddProduct` iletisi ve yanıt iletisini almak için bir geri çağırma temsilcisini yürütür. Lambda ifadesi belirtilen bu geri çağırma temsilcisini UI güncelleştirmeleri kodu yürütür.

> [!TIP]
> Değişmez yükü veri kullanmayı düşünün. Birkaç iş parçacığı alınan verilerin aynı anda erişiyor çünkü bir geri çağırma temsilcisini içindeki yükü verileri değiştirme girişimi yok. Bu senaryoda, yükü veri eşzamanlılık hatalarını önlemek için sabit olmalı.

Bir abonenin yayımlanan ileti her örneğinin işlemek gerekmeyebilir ve bunun üzerinde belirtilen genel tür bağımsız değişkenleri tarafından denetlenebilir [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) yöntemi. Bu örnekte, abone yalnızca alacak `AddProduct` den gönderilen iletileri `CatalogViewModel` yükü verisini olduğu sınıfı, bir `CatalogItem` örneği.

## <a name="unsubscribing-from-a-message"></a>Bir ileti aboneliklerini

Aboneler artık almak istediği iletilerden aboneliğinizi iptal edebilirsiniz. Bu, biri ile sağlanır [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) , aşağıdaki kod örneğinde gösterildiği şekilde overloads:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

Bu örnekte, [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) yöntem sözdizimi yansıtır almak için abone olurken belirtilen tür bağımsız değişkenleri `AddProduct` ileti.

## <a name="summary"></a>Özet

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) sınıfı uygulayan Yayımla-abone nesne ve türü başvurularını bağlamak kullanışsız bileşenleri arasındaki iletişim ileti tabanlı olanak deseni. Bu mekanizma Yayımcılar ve aboneleri birbirlerine bağımsız olarak geliştirilen ve test bileşenleri de verirken bileşenleri arasındaki bağımlılıkları azaltmaya yardımcı başvuru gerek kalmadan iletişim kurmasına izin verir.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
