---
title: Şekilde eşlenen bileşenler arasında gevşek iletişim
description: 'Bu bölümde, hizmetine mobil uygulama yayımlama nasıl uyguladığını açıklanmaktadır-abone ol modeli, nesne ve tür başvurularını bağlamak uygun olan bileşenleri arasında ileti tabanlı iletişim izin verme '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ddc33d28aad4e00c9259893c0f8e7a1ab40ee429
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998550"
---
# <a name="communicating-between-loosely-coupled-components"></a>Şekilde eşlenen bileşenler arasında gevşek iletişim

Yayımla-abone olma, yayımcılar ileti gönderme aboneleri bilinen, tüm alıcılar bilgisi olmadan bir Mesajlaşma deseni modelidir. Benzer şekilde, abonelerin bilgi tüm yayımcılar zorunda kalmadan belirli iletiler için dinleme yapan.

. NET'te olayları uygulama Yayımla-abone ol modeli ve en sade ve basit yaklaşımdır iletişim katmanı için gevşek bağ modelini, Denetim ve içerdiği sayfa gibi gerekli değilse, bileşenler arasındaki. Ancak, yayımcısı ve abonesi ömürleri nesne başvuruları birbirine bağlı ve abone türü yayımcı türü bir başvuru olmalıdır. Özellikle statik veya uzun süreli bir nesne bir olaya abone kısa süreli nesneleri olduğunda bu bellek yönetim sorunlar oluşturabilir. Olay işleyicisi kaldırılmaz, aboneye yayımcı ona başvuru tarafından Canlı tutulacak ve bu engellemek veya çöp toplama abonenin gecikme.

## <a name="introduction-to-messagingcenter"></a>MessagingCenter giriş

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) sınıfın uyguladığı Yayımla-abone ol modeli, nesne ve tür başvurularını bağlamak uygun olan bileşenleri arasında ileti tabanlı iletişim sağlar. Bu mekanizma, Yayımcılar ve aboneler bir başvuru bağımsız olarak geliştirilen ve test bileşenlerini de verirken bileşenleri arasındaki bağımlılıkları azaltmaya yardımcı olur, birbiriyle olmadan iletişim kurmasına izin verir.

[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Sınıfı, çok noktaya yayın Yayımla-abone ol işlevselliği sağlar. Bu, tek bir ileti yayımlayın birden çok yayımcı olabilir ve birden çok abone aynı ileti için dinleme olabilir anlamına gelir. Şekil 4-1, bu ilişkiyi göstermektedir:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Çok noktaya yayın Yayımla-abone ol işlevi")

**Şekil 4-1:** çok noktaya yayın Yayımla-abone ol işlevi

Yayımcılar, kullanılarak ileti gönderme [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) aboneleri kullanarak iletileri dinlemek sırada yöntemi [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) yöntemi. Ayrıca, aboneleri de ileti aboneliklerden gerekirse abonelikten çıkabilirsiniz ile [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) yöntemi.

Dahili olarak [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) sınıfı, zayıf başvurular kullanır. Başka bir deyişle, bu nesneleri canlı tutmak değil ve çöp olarak toplanacak izin verir. Bu nedenle, bu yalnızca bir ileti, ileti almak bir sınıf artık istediğinde, aboneliğinizi iptal etmek gerekli olmalıdır.

Hizmetine mobil uygulama kullandığı [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) eşleşmiş bileşenleri arasında gevşek iletişim kurmak için sınıf. Uygulamayı üç iletileri tanımlar:

-   `AddProduct` İletisi tarafından yayımlanmış `CatalogViewModel` alışveriş sepetine bir öğe eklendiğinde sınıfı. Buna karşılık, `BasketViewModel` sınıfı iletiyi abone olur ve yanıt alışveriş sepetine öğe sayısını artırır. Ayrıca, `BasketViewModel` sınıfı da bu ileti aboneliğini kaldırır.
-   `Filter` İletisi tarafından yayımlanmış `CatalogViewModel` kullanıcı catalogue görüntülenen öğeler için bir marka veya türü filtre uygulandığında sınıfı. Buna karşılık, `CatalogView` sınıfı iletiyi abone olur ve filtre ölçütleriyle eşleşen öğeler görüntülenir, böylece kullanıcı arabirimini güncelleştirir.
-   `ChangeTab` İletisi tarafından yayımlanmış `MainViewModel` sınıfı `CheckoutViewModel` gider `MainViewModel` başarılı oluşturulmasını ve yeni bir sipariş teslimini izleyen. Buna karşılık, `MainView` sınıfı abone olan iletiyi ve kullanıcı Arabirimi güncelleştirmeleri bu nedenle, **Profilimi** sekmedir kullanıcının siparişleri göstermek için etkin.

> [!NOTE]
> Sırada [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) sınıfı verir zamanı gevşek bağlanmış sınıfları arasındaki iletişim, bu sorun yalnızca mimari çözüme sunmaz. Örneğin, bir görünüm modeli ve bir görünüm arasında iletişim bağlama altyapısı ve özellik değişikliği bildirimleri aracılığıyla gerçekleştirilebilir. Ayrıca, iki görünüm modelleri arasındaki iletişimi de gezinti sırasında veri geçirerek gerçekleştirilebilir.

Hizmetine mobil uygulamasında[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) başka bir sınıf içinde gerçekleşen bir eyleme yanıt kullanıcı arabirimini güncelleştirmek için kullanılır. Bu nedenle, iletileri UI iş parçacığı üzerinde aynı iş parçacığında iletiyi almak için abone yayımlanır.

> [!TIP]
> Kullanıcı arabirimini gerçekleştiren güncelleştirdiğinde UI iş parçacığı için hazırlama. Kullanıcı arabirimini güncelleştirmek için arka plan iş parçacığından gönderilen ileti gereklidir, iletiyi abone UI iş parçacığı üzerinde harekete geçirerek işleme [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) yöntemi.

Hakkında daha fazla bilgi için [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), bkz: [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Bir ileti tanımlama

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) iletiler iletiler tanımlamak için kullanılan dizelerdir. Aşağıdaki kod örneği, iletileri hizmetine mobil uygulama içinde tanımlı gösterir:

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

Bu örnekte, iletilerin sabitler kullanılarak tanımlanır. Bu yaklaşımın avantajı derleme zamanı tür güvenliği ve yeniden düzenleme desteği sağlamasıdır.

## <a name="publishing-a-message"></a>İleti yayımlama

Yayımcılar bildirim aboneleri birini içeren bir ileti [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) aşırı yüklemeleri. Aşağıdaki kod örneği yayımlama gösterir `AddProduct` ileti:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

Bu örnekte, [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) yöntemi üç bağımsız değişken belirtir:

-   İlk bağımsız değişken gönderen sınıfını belirtir. Gönderen sınıfı iletisi isteyen tüm aboneler tarafından belirtilmesi gerekir.
-   İkinci bağımsız değişkeni, iletiyi belirtir.
-   Üçüncü bağımsız değişken aboneye gönderilmesi yük verisi belirtir. Bu durumda yük verisi, bir `CatalogItem` örneği.

[ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Yöntemi, ileti ve Başlat ve unut bir yaklaşım kullanarak yükü verisini yayımlar. İleti almak için kayıtlı hiçbir aboneleri olsa bile, bu nedenle, ileti gönderilir. Bu durumda, gönderilen iletiyi göz ardı edilir.

> [!NOTE]
> [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Yöntemi, iletilerin nasıl teslim denetlemek için genel parametreleri kullanabilirsiniz. Bu nedenle, bir ileti kimliği paylaşır, ancak farklı yük türleri gönderme birden çok ileti farklı aboneler tarafından alınabilir.

## <a name="subscribing-to-a-message"></a>Bir iletiyi abone olma

Aboneleri birini kullanarak ileti alma kaydedebilir [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) aşırı yüklemeleri. Aşağıdaki kod örneğinde nasıl hizmetine mobil uygulama abone olur ve işler, gösterir `AddProduct` ileti:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

Bu örnekte, [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) yöntemi aboneliği `AddProduct` iletisi ve ileti almak için yanıt geri çağırma temsilcisini yürütür. Kullanıcı Arabirimi güncelleştirmeleri kod bir lambda ifadesi belirtilen bu geri çağırma temsilcisini yürütür.

> [!TIP]
> Sabit yük verisi kullanmayı düşünün. Birkaç iş parçacığı alınan verileri aynı anda erişim çünkü bir geri çağırma temsilcisini içindeki yük verileri değiştirmeyi denemeyin. Bu senaryoda, yük verisi eşzamanlılık hatalarını önlemek için sabit olmalıdır.

Bir abonenin yayımlanan ileti'nin her örneğinin işlemek gerekmeyebilir ve bunun üzerinde belirtilen genel tür bağımsız değişkenleri tarafından denetlenebilir [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) yöntemi. Bu örnekte, yalnızca abonelik alacak `AddProduct` uygulamasından gönderilen iletileri `CatalogViewModel` yükü verisini olan sınıfı, bir `CatalogItem` örneği.

## <a name="unsubscribing-from-a-message"></a>Bir ileti aboneliklerini iptal etme

Bunlar artık almak istediğiniz iletileri aboneleri abonelikten çıkabilirsiniz. Bu, biri ile sağlanır [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) , aşağıdaki kod örneğinde gösterildiği gibi aşırı:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

Bu örnekte, [ `Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) yöntem sözdizimi almak için abone olurken belirtilen tür bağımsız değişkenleri yansıtan `AddProduct` ileti.

## <a name="summary"></a>Özet

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) sınıfın uyguladığı Yayımla-abone ol modeli, nesne ve tür başvurularını bağlamak uygun olan bileşenleri arasında ileti tabanlı iletişim sağlar. Bu mekanizma, Yayımcılar ve aboneler bir başvuru bağımsız olarak geliştirilen ve test bileşenlerini de verirken bileşenleri arasındaki bağımlılıkları azaltmaya yardımcı olur, birbiriyle olmadan iletişim kurmasına izin verir.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
