---
title: Dış depolama Xamarin.Android ile dosya erişimi
description: Bu kılavuz, dış depolama alanında Xamarin.Android üzerinde dosya erişimi ele alınacaktır
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 07/23/2018
ms.openlocfilehash: 380100d38febf567fde94096455fd846d9d3d2d3
ms.sourcegitcommit: 9bb9e8297d3edd9a50585f4ba53c1b4f0bcd1d3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212163"
---
# <a name="external-storage"></a>Dış depolama

Dış depolama iç depolama alanına göre değil ve dosyayı sorumlu bir uygulamaya özel olarak erişilebilir olduğundan dosya depolama ifade eder. Dış depolama birincil amacı, uygulamalar arasında paylaşılacak yöneliktir veya iç depolama alanında sığmayacak kadar büyük olan dosyaları koyabileceğimiz bir yer sağlamaktır.

Tarihsel olarak konuşma, dış depolama başvurulan bir disk bölümüne SD kartı gibi çıkarılabilir medyada (olarak da bilinen _taşınabilir depolama_). Bu ayrım artık Android cihazları geliştirmiştir ve farklı Android cihazları çıkarılabilir depolama birimi artık desteklemiyor. ilgili değildir. Bunun yerine bazı cihazlar kendi iç geçici olmayan belleği bazıları aynı işlevi çıkarılabilir medyayı gerçekleştirmek için hangi Android ayırır. Bu olarak bilinir _benzetilmiş_ depolama ve harici depolama olarak kabul edilir. Alternatif olarak, bazı Android cihazları birden çok dış depolama bölüm olabilir. Örneğin, bir Android tablet (iç depolama) ek olarak, depolama benzetilmiş ve bir SD kart için bir veya daha fazla yuvası. Tüm bu bölümleri Android tarafından dış depolama alanı olarak kabul edilir.

Birden çok kullanıcıya sahip cihazlarda, her kullanıcı birincil dış depolama bölüm, dış depolama için ayrılmış bir dizine sahip olur. Bir kullanıcı olarak çalıştırılan uygulamalar dosyaları cihazdaki başka bir kullanıcı erişemeyecektir. Tüm kullanıcılar için hala dünya okunabilir ve yazılabilir dünya dosyalardır; Ancak, Android olacak korumalı alan her kullanıcı profilinizden diğerleri.

Diğer herhangi bir .NET uygulamasında olduğu için dosyaları okuma ve yazma Xamarin.Android neredeyse aynıdır. Xamarin.Android uygulaması değiştirilecek, ardından kullanımlar standart .NET deyimleri dosya erişimi için dosya yolunu belirler. Android sürümünden Android sürümüne veya cihazı başka bir cihaz gerçek iç ve dış depolama yolları farklılık gösterebileceğinden dosyalarının yolunu sabit kod için önerilmez. Bunun yerine, Xamarin.Android yerel Android dahili ve harici depolama dosyalarının yolunu belirleme ile yardımcı olacak API'lerini kullanıma sunar.

Bu kılavuz, kavramlar ve dış depolama birimine özgü Android API'leri ele alınacaktır.

## <a name="public-and-private-files-on-external-storage"></a>Ortak ve özel dosyaları dış depolama

Uygulama dış depolama alanında tutabilir dosyaları iki farklı tür vardır:

* **Özel** dosyaları &ndash; özel dosyaları, uygulama (ancak bunlar yine de world okunabilir ve yazılabilir world) özgü dosyalarıdır. Android özel dosyaları dış depolama üzerinde belirli bir dizinde depolanır bekliyor. Dosyalar "özel" olarak adlandırılır, ancak yine de görünür ve cihazdaki diğer uygulamalar tarafından erişilebilen, bunlar herhangi bir özel koruma Android tarafından gösterilen değil.

* **Genel** dosyaları &ndash; bunlar, uygulamaya özgü olmasını dikkate alınmaz ve özgürce paylaşılan yöneliktir dosyalardır.

Bu dosyalar arasındaki farkları öncelikle kavramsal. Özel dosyaları anlamda özel, genel dosyaları dış depolama alanında bulunan diğer dosyaları durumdayken uygulamanın bir parçası olarak kabul edilir. Android özel ve genel dosyalara yolları çözümlemek için iki farklı API'ler sağlar, ancak aksi .NET API'leri için bu dosyaları okuma ve yazma için kullanılır. Bölümünde ele alınmıştır API'leri bunlar [okuma ve yazma](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage).

### <a name="private-external-files"></a>Özel bir dış dosyaları

Özel bir dış dosyaları belirli bir uygulamaya (iç dosyalara benzer) olarak değerlendirilir, ancak birkaç nedenden (örneğin, iç depolama için çok büyük olan) için dış depolama tutulur. Kullanıcı tarafından uygulama kaldırıldığında benzer şekilde iç dosyalar, bu dosyalar silinir.

Yöntemini çağırarak özel dış dosyaları için birincil konum bulundu `Android.Content.Context.GetExternalFilesDir(string type)`. Bu metodun döndüreceği bir `Java.IO.File` uygulaması için özel bir harici depolama dizini temsil eden nesne. Geçirme `null` bu yönteme, uygulama için kullanıcının depolama dizinine yolunu döndürür. Örneğin, paket adı ile bir uygulama için `com.companyname.app`, özel dış dosyaları "Kök" dizininin olacaktır:

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

Bu belge için özel dosyaları dış depolama depolama dizinine başvuracaktır _özel\_dış\_depolama_.


Parametresi için `GetExternalFilesDir()` belirten bir dize olan bir _uygulama dizini_. Bu dosyaların mantıksal bir kuruluş için standart bir konum sağlamaya yönelik bir dizindir. Dize değerleri sabitlerde aracılığıyla kullanılabilen `Android.OS.Environment` sınıfı:

| `Android.OS.Environment` | Dizin |
|-|-|
| DirectoryAlarms | **_ÖZEL\_dış\_depolama_  /alarmlar** |
| DirectoryDcim | **_ÖZEL\_DIŞ\_DEPOLAMA_/DCIM** |
| DirectoryDownloads | **_ÖZEL\_dış\_depolama_  /indirin** |
| DirectoryDocuments | **_ÖZEL\_dış\_depolama_  /belgeler** |
| DirectoryMovies | **_ÖZEL\_dış\_depolama_/Movies** |
| DirectoryMusic | **_ÖZEL\_dış\_depolama_/Music** |
| DirectoryNotifications | **_ÖZEL\_dış\_depolama_/Notifications** |
| DirectoryPodcasts | **_ÖZEL\_dış\_depolama_/Podcasts** |
| DirectoryRingtones | **_ÖZEL\_dış\_depolama_/Ringtones** |
| DirectoryPictures | **_ÖZEL\_dış\_depolama_  /resimler** |

Birden çok dış depolama bölümlere sahip cihazlar için her bölüm için özel dosyalara yöneliktir bir dizin olacaktır. Yöntem `Android.Content.Context.GetExternalFilesDirs(string type)` bir dizi döndürür `Java.IO.Files`. Her bir nesnenin özel bir uygulamaya özgü dizin temsil edecek burada uygulama yerleştirebileceğiniz dosyaları tüm paylaşılan harici depolama cihazlarında sahibi.

> [!IMPORTANT]
> Özel bir dış depolama dizinine tam yolun CİHAZDAN cihaz ve Android sürümleri arasında değişebilir. Bu nedenle, uygulamaları sabit olmayan bu dizine olan yolu kod ve bunun yerine Xamarin.Android API'leri gibi kullanmanız gerekir `Android.Content.Context.GetExternalFilesDir()`.

### <a name="public-external-files"></a>Genel dış dosyaları

Android için özel dosyaları ayırır dizine depolanmaz dış depolama alanında mevcut dosyaları ortak dosyalarıdır. Ortak dosyalar, uygulama kaldırıldığında silinmez. Okuma veya yazma tüm ortak dosyalar önce android uygulamaları iznine sahip olmanız gerekir. Genel dış depolama alanında herhangi bir mevcut dosyaya mümkündür, ancak kural gereği, genel özelliği tarafından tanımlanan dizine mevcut dosyaya Android bekliyor `Android.OS.Environment.ExternalStorageDirectory`. Bu özellik döndüreceği bir `Java.IO.File` birincil dış depolama dizini temsil eden nesne. Örneğin, `Android.OS.Environment.ExternalStorageDirectory` şu dizine başvurabilir:

```bash
/storage/emulated/0/
```

Bu belge, dış depolama ortak dosyaları için depolama dizinine başvuracaktır _genel\_dış\_depolama_.


Android ayrıca desteklediğine uygulama dizinleri kavramını _genel\_dış\_depolama_. Bu dizinler tam olarak için uygulama diretories aynıdır `_PRIVATE\_EXTERNAL\_STORAGE_` ve önceki bölümde yer alan tabloda açıklanmıştır. Yöntem `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` döndüreceği bir `Java.IO.File` genel uygulama dizinine karşılık gelen nesne. `directoryType` Parametre zorunlu bir parametredir ve olamaz `null`.

Örneğin, çağırma `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` benzer bir dizeyi döndürür:

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> Genel dış depo dizininin tam yolunu CİHAZDAN cihaz ve Android sürümleri arasında değişebilir. Bu nedenle, uygulamaları sabit olmayan bu dizine olan yolu kod ve bunun yerine Xamarin.Android API'leri gibi kullanmanız gerekir `Android.OS.Environment.ExternalStorageDirectory`.

## <a name="working-with-external-storage"></a>Harici depolama ile çalışma

Bir Xamarin.Android uygulaması bir dosyanın tam yolunu elde edilen sonra herhangi bir oluşturma, okuma, yazma veya dosyaları silme için standart .NET API'lerini kullanmanız gerekir. Bu miktarı çapraz platform uyumlu kod bir uygulama için en üst düzeye çıkarır. Ancak, bir dosyaya erişmeye çalışmadan önce bir Xamarin.Android uygulaması bu dosyaya erişmek mümkün olduğundan emin olmalısınız.

1. **Dış depolama alanını doğrulama** &ndash; dış depolama doğasına bağlı olarak, bu olası değil bağlı ve uygulama tarafından kullanılabilir olur. Tüm uygulamalar, kullanmaya çalışmadan önce dış depolama durumunu denetlemeniz gerekir.
2. **Bir çalışma zamanı izni denetimi gerçekleştirmek** &ndash; bir Android uygulaması istemeniz gerekir izni kullanıcıdan dış depolama erişebilmek için. Bu, izin isteği herhangi bir dosya erişim önce gerçekleştirilmesi gereken bir çalışma süresi anlamına gelir. Kılavuzu [izinleri de Xamarin.Android](~/android/app-fundamentals/permissions.md) Android izinler hakkında daha fazla ayrıntı içerir.

Bu iki görevlerin her biri aşağıda açıklanmıştır.

### <a name="verifying-that-external-storage-is-available"></a>Dış depolama kullanılabilir olduğu doğrulanıyor

Dış depolama birimine yazmadan önce ilk adımı, okunabilir veya yazılabilir olduğundan emin olun sağlamaktır. `Android.OS.Environment.ExternalStorageState` Dış depolama durumunu tanımlayan bir dize özelliği içerir. Bu özellik durumu temsil eden bir dize döndürür. Bu tabloda listesidir `ExternalStorageState` tarafından döndürülen değerler `Environment.ExternalStorageState`:

| ExternalStorageState | Açıklama  |
|----------------------|---|
| MediaBadRemoval      | Medyanın düzgün bir şekilde bağlı olmayan olmadan aniden kaldırıldı. |
| MediaChecking        | Ortamı var ancak bir disk aşamasında denetleyin.  |
| MediaEjecting        | Medya takılamadı çıkarılabilir ve sürecinde ' dir.  |
| MediaMounted         | Medya bağlanır ve okunabilir veya yazılır.  |
| MediaMountedReadOnly | Medya bağlı, ancak yalnızca okunabilir. |
| MediaNofs            | Medya var, ancak Android için uygun bir dosya sistemi içermiyor. |
| MediaRemoved         | Mevcut ortam yok. |
| MediaShared          | Medya var, ancak takılı değil. Başka bir cihaz ile USB üzerinden paylaşılıyor.|
| MediaUnknown         | Ortam durumunu Android tarafından tanınmıyor. |
| MediaUnmountable     | Ortamı var ancak Android tarafından oluşturulmuş. |
| MediaUnmounted       | Ortamı var ancak takılı değil. |


Çoğu Android uygulamaları dış depolama takılı denetlemek yeterlidir. Aşağıdaki kod parçacığı, salt okunur veya okuma-yazma erişimi için dış depolama bağlandığını doğrulamak gösterilmektedir:

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>Dış depolama izinleri

Android göz önünde bulundurur olması için dış depolama alanına erişilirken bir _tehlikeli izni_, kaynağa erişmek için kendi izin vermek kullanıcının genellikle gerektirir. Kullanıcının bu izni dilediğiniz zaman iptal edebilirsiniz.  Bu, izin isteği herhangi bir dosya erişim önce gerçekleştirilmesi gereken bir çalışma süresi anlamına gelir. Uygulamaları otomatik olarak kendi özel dosyalarını okuma ve yazma izni verilir. Uygulamaların kaldıktan sonra diğer uygulamalara ait özel dosyalarını okuma ve yazma mümkündür [izni](~/android/app-fundamentals/permissions.md) kullanıcı tarafından.

Tüm Android uygulamaları için dış depolama alanında iki izinlerinden birini belirtmesi gerekir **AndroidManifest.xml** . İzinler, aşağıdaki iki birini belirlemek için `uses-permission` öğeleri eklemek, için **AndroidManifest.xml**:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> Kullanıcı verirse `WRITE_EXTERNAL_STORAGE`, ardından `READ_EXTERNAL_STORAGE` de örtülü olarak verilir. Her iki izinleri istemek gerekli değildir **AndroidManifest.xml**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

İzinlerini kullanarak da eklenebilir **Android bildirim** sekmesinde **çözüm özellikleri**:

![Çözüm Gezgini - Visual Studio 2017 için gerekli izinleri](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

İzinlerini kullanarak da eklenebilir **Android bildirim** sekmesinde **çözüm özellikleri bölmesi**:

[![Çözüm bölmesi - Mac için Visual Studio için gerekli izinleri](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

Genel olarak bakıldığında, tüm tehlikeli izinler kullanıcı tarafından onaylanması gerekir. Var olan uygulamayı çalıştıran Android sürümüne bağlı olarak bu kuralın istisnası, dış depolama izinlerini bir anomali şunlardır:

![Dış depolama izin denetimleri akış çizelgesi](./images/external-permission-check-flowchart.png)

Çalışma zamanı izni istekleri gerçekleştirme hakkında daha fazla bilgi için lütfen kılavuzuna başvurun [izinleri de Xamarin.Android](~/android/app-fundamentals/permissions.md). **Monodroid örnek** [YerelDosyalar](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) ayrıca çalışma zamanı izni denetimleri gerçekleştirmenin bir yolunu gösterir.

#### <a name="granting-and-revoking-permissions-with-adb"></a>Verme ve ADB ile izinleri iptal etme

Bir Android uygulaması geliştirme sırasında çalışma zamanı izni denetimleri ile ilgili çeşitli iş akışlarını sınamak için izinlerini iptal edin ve vermek gerekli olabilir. Komut isteminde ADB kullanarak bunu mümkündür. Aşağıdaki komut satırı kod parçacıkları vermek veya ADB paket adı olan bir Android uygulaması kullanarak izinlerini iptal işlemini göstermek **com.companyname.app**:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>Dosyaları silme

C# API kullanılabilir bir dosya gibi dış depolama alanından silmek için standart [ `System.IO.File.Delete` ](xref:System.IO.File.Delete*). Java API kod taşınabilirliği çoğaltamaz kullanmak mümkündür. Örneğin:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>İlgili bağlantılar

* [Yerel dosyaları Xamarin.Android örnek üzerinde **monodroid örnekleri**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin.Android izinleri](~/android/app-fundamentals/permissions.md)
