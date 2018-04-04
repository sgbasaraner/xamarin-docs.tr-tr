---
title: Hizmet Oluşturma
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/01/2018
ms.openlocfilehash: d1e0fdb1c4b159b6db283d7b9b3be673b73a0ee0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-service"></a>Hizmet Oluşturma

Xamarin.Android Hizmetleri Android Hizmetleri iki inviolable kurallara uyarlar gerekir:

* Genişletmeniz gerekir [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/).
* İle tasarlanmalıdır [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Başka bir Android Hizmetleri, bunlar içinde kaydedilmelidir gereksinimdir **AndroidManifest.xml** ve benzersiz bir ad verilir. Xamarin.Android otomatik olarak hizmet bildiriminde derleme zamanında gerekli XML özniteliği ile kaydeder.

Bu kod parçacığını iki bu gereksinimleri karşılayan Xamarin.Android içinde bir hizmet oluşturma için basit bir örnektir:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Derleme zamanında Xamarin.Android hizmeti aşağıdaki XML öğesi içine ekleyerek kaydedeceksiniz **AndroidManifest.xml** (Xamarin.Android rastgele bir ad hizmeti için oluşturulan dikkat edin):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Bir hizmet tarafından Android diğer uygulamalarla paylaşma mümkündür _dışarı aktarma_ onu. Bu ayar gerçekleştirilir `Exported` özelliği `ServiceAttribute`. Bir hizmet verirken `ServiceAttribute.Name` hizmeti için anlamlı bir ortak ad sağlamak için özelliği ayarlanmalıdır. Bu kod parçacığında, bir hizmet adı ve dışarı aktarmak gösterilmiştir:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

**AndroidManifest.xml** öğesi bu hizmet için sonra görünür aşağıdakine benzer:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Hizmetleri hizmet oluşturulduğunda çağrılan geri çağırma yöntemleri ile kendi yaşam döngüleri vardır. Tam olarak hangi yöntemlerin çağrılan hizmet türüne bağlıdır. Geri arama yöntemleri başlatılan bir hizmeti ve bağımlı bir hizmet için bir karma hizmet uygulamalıdır sırada başlatılan bir hizmet farklı yaşam döngüsü yöntemlerinden ilişkili hizmet daha uygulamalıdır. Bu yöntemler tüm üyeleri olan `Service` sınıf; hizmetin nasıl başlatılacağını yaşam döngüsü yöntemleri çağrılacak belirleyin. Bu yaşam döngüsü yöntemleri ayrıntılı olarak daha sonra açıklanacaktır.

Varsayılan olarak, bir Android uygulaması olarak aynı işlemde bir hizmetini başlatır. Ayarlayarak kendi işleminde bir hizmeti başlatmak mümkündür `ServiceAttribute.IsolatedProcess` özelliği true olarak:

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

Sonraki adım, bir hizmeti başlatmak ve Hizmetleri üç farklı türde uygulamak nasıl incelemek taşımak nasıl incelemektir.

> [!NOTE]
> Kullanıcı Arabirimi iş parçacığı üzerinde bir hizmeti çalıştıran herhangi bir iş olarak gerçekleştirilen hangi kullanıcı Arabirimi blokları ise, hizmet iş parçacığı çalışmayı gerçekleştirmek üzere kullanmalısınız.

## <a name="starting-a-service"></a>Bir hizmeti başlatılıyor

Gönderme Android bir hizmeti başlatmak için en temel yolu olan bir `Intent` hangi hizmetin başlatılması belirlemenize yardımcı olması için meta verileri içerir. Bir hizmeti başlatmak için kullanılan hedefleri iki farklı türlerde vardır:

-   **Açık hedefi** &ndash; bir _açık hedefi_ tam olarak hangi hizmetin belirli bir eylemi tamamlamak için kullanılması gerektiğini tanımlar. Açık bir amacı, belirli bir adresi olan bir harf düşünülebilir; Android amacını açıkça tanımlanmış hizmete yönlendirir. Bu kod parçacığında adlı bir hizmeti başlatmak için açık amacına kullanmanın bir örnektir `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Örtük hedefi** &ndash; hedefi bu tür kabaca tanımlayan eylemi, gerçekleştirilmelidir, ancak bu eylemi tamamlamak için tam hizmeti bilinmiyor. Bir harf "Kime Whom BT May sorunu..." olarak örtük amacına olarak düşünülebilir.
    Amaç eşleşen bir hizmetiniz varsa android amacını ve sözlüğünün içeriğini inceleyin.

    Bir _hedefi filtre_ kayıtlı hizmet örtük hedefle eşleşen yardımcı olmak için kullanılır. Eklenen bir XML öğesi hedefi bir filtredir **AndroidManifest.xml** gerekli meta örtük amacına sahip bir hizmet eşleşen yardımcı olmak için verileri içerir.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Android örtük amacına yönelik birden fazla olası eşleşme içeriyorsa, eylem işlemek için bileşenini seçmek için kullanıcı isteyebilir:

![Kesinleştirme iletişim kutusunun ekran görüntüsü](images/creating-a-service-01.png "Kesinleştirme iletişim kutusunun ekran görüntüsü")

> [!IMPORTANT]
> Android 5.0 (AP düzey 21) başlatma örtük amacına bir hizmeti başlatmak için kullanılamaz.

Mümkünse, uygulamalar bir hizmeti başlatmak için açık hedefleri kullanmalıdır. Örtük amacına başlatmak belirli bir hizmet için istemez &ndash; cihazda yüklü bazı hizmet isteği işlemek için bir istek olduğu. Bu belirsiz istek işleme isteği veya (cihazdaki kaynakları baskısı artıran) gereksiz yere başlangıç başka bir uygulama yanlış hizmeti neden olabilir.

Amaç nasıl gönderilir, hizmet türüne bağlıdır ve her türde bir hizmeti belirli kılavuzları ilerleyen bölümlerinde daha ayrıntılı olarak ele.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Örtük amaçlar için bir hedefi filtresi oluşturma

Bir hizmeti örtük bir hedefi ile ilişkilendirmek için bir Android uygulaması hizmet özelliklerini tanımlamak için bazı meta verileri sağlamanız gerekir. Bu meta veriler tarafından sağlanan _hedefi filtreleri_. Hedefi filtreler bir eyleme ya da bir hizmeti başlatmak için bir hedefi bulunması gereken veri türü gibi bazı bilgiler içerir. Hedefi filtre Xamarin.Android içinde kayıtlı **AndroidManifest.xml** bir hizmetle dekorasyon tarafından [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Örneğin, aşağıdaki kodu ilişkili eylemi ve hedefi bir filtre ekler `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Bu dahil bir giriş sonuçlanır **AndroidManifest.xml** dosya &ndash; aşağıdaki örneğe benzer şekilde bu uygulama ile paketlenmiştir bir giriş:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Xamarin.Android hizmet göz önünden temelleri ile inceleyelim farklı alt daha ayrıntılı Hizmetleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
