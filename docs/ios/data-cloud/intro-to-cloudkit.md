---
title: CloudKit
description: "iCloud API'leri bir kullanıcı hesabı otomatik eşitleme için desteğiyle iCloud verilerini depolamak iOS 8 uygulamaları etkinleştirir. CloudKit kullanarak kullanıcıların iCloud etkinleştirildiği aygıtlar arasında tutarlı ve sorunsuz bir deneyim sağlar. Bu makalede kolaylık API'sini kullanarak, iOS 8 uygulama CloudKit etkinleştirme kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/11/2016
ms.openlocfilehash: c4ee5c0457dd1faea74cbbc30dd2d0f42087a8d0
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="cloudkit"></a>CloudKit

_iCloud API'leri bir kullanıcı hesabı otomatik eşitleme için desteğiyle iCloud verilerini depolamak iOS 8 uygulamaları etkinleştirir. CloudKit kullanarak kullanıcıların iCloud etkinleştirildiği aygıtlar arasında tutarlı ve sorunsuz bir deneyim sağlar. Bu makalede kolaylık API'sini kullanarak, iOS 8 uygulama CloudKit etkinleştirme kapsar._

CloudKit framework bu erişim iCloud uygulamaların geliştirilmesini kolaylaştırır. Bu, uygulama verilerini ve varlık hakları yanı sıra uygulama bilgilerini güvenli bir şekilde erişebildiklerinden alınmasını içerir. Bu paketi kişisel bilgi paylaşımı olmadan kullanıcıların iCloud kimlikleri ile uygulamalara erişim sağlayarak kullanıcılara anonim bir katman sağlar.

Geliştiriciler, istemci-tarafı uygulamalarını odaklanmanıza ve sunucu tarafı uygulama mantığı yazmak gereğini ortadan iCloud izin verin. CloudKit kimlik doğrulaması, özel ve ortak veritabanları ve yapılandırılmış veri ve varlık depolama hizmetleri sağlar.

## <a name="requirements"></a>Gereksinimler

Aşağıda sunulan bu makaledeki adımları tamamlamak için gereklidir:

-  **Xcode ve iOS SDK'sı** – Apple'nın Xcode ve iOS 8 API'leri gereken yüklenmeli ve geliştirici bilgisayarda yapılandırılmış.
-  **Mac için Visual Studio** – Mac için Visual Studio en son sürümünü yüklenmeli ve kullanıcı cihazda yapılandırılmalıdır.
-  **iOS 8 aygıtı** – iOS 8 test etmek için en son sürümünü çalıştıran bir iOS cihazı.

## <a name="what-is-cloudkit"></a>CloudKit nedir?

CloudKit sunucuları İcloud'a Geliştirici erişmenizi sağlayacak bir yoludur. İCloud sürücü ve İcloud'a Fotoğraf Kitaplığı için temeli sağlar. CloudKit Mac OS X ve Apple iOS aygıtları üzerinde desteklenir.

 [![](intro-to-cloudkit-images/image1.png "CloudKit hem Mac OS X hem de Apple iOS cihazları nasıl desteklenir")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit iCloud hesabı altyapısı kullanır. İCloud hesapla oturum açmış bir kullanıcı varsa, CloudKit kullanıcıyı tanımlamak için kendi Kimliğini kullanır. Herhangi bir hesabı varsa, sınırlı salt okunur erişim sağlanacaktır.

CloudKit kavramı, ortak ve özel veritabanlarını destekler. Ortak veritabanları kullanıcı erişimi olan tüm verilerin "Çorbası" sağlar. Belirli bir kullanıcıya bağlı özel verileri depolamak için özel veritabanları yöneliktir.

CloudKit yapılandırılmış ve toplu veri destekler. Büyük dosya aktarımları sorunsuz bir şekilde işleyebilir. CloudKit verimli bir şekilde büyük dosyaları arka planda sunucuları iCloud gelen ve giden aktarma diğer görevlere odaklanmasını Geliştirici boşaltma mvc'deki.

> [!NOTE]
> CloudKit olduğunu dikkate almak önemlidir bir _aktarım teknolojisi_. Tüm Kalıcılık sağlamaz; Bu yalnızca bilgi göndermek ve sunuculardan verimli bir şekilde almak için bir uygulama sağlar.

Bu makalenin yazıldığı sırada Apple başlangıçta CloudKit ücretsiz yüksek sınırına bant genişliği ve depolama kapasitesine sahip sağlamaktadır. Büyük projeler veya büyük bir kullanıcı tabanı uygulamaları için Apple uygun maliyetli bir fiyatlandırma ölçek sağlanacak İpuçlu.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Xamarin uygulamasında CloudKit etkinleştirme

Uygulama, bir Xamarin uygulaması CloudKit Framework kullanabilmeniz için önce doğru sağlanmalıdır içinde ayrıntılı olarak [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) ve [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) kılavuzları

1.  Proje, Mac veya Visual Studio için Visual Studio'da açın.
2.  İçinde **Çözüm Gezgini**, açık **Info.plist** dosya ve olun **paket tanımlayıcısı** tanımlanan bir eşleşen **uygulama kimliği**sağlama parçası ayarlanmış olarak oluşturulan:
 
    [![](intro-to-cloudkit-images/image26a.png "Paket tanımlayıcısı girin")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  Listenin sonuna kaydırın **Info.plist** dosya ve seçin **etkin arka plan modları**, **konumu güncelleştirmeleri** ve **uzak bildirimler**:

    [![](intro-to-cloudkit-images/image27a.png "Etkin arka plan modları, konum güncelleştirmeler ve uzak bildirimler seçin")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  Çözüm seçip iOS projesine sağ tıklayın **seçenekleri**.
5.  Seçin **iOS paket imzalama**seçin **Geliştirici kimlik** ve **sağlama profili** yukarıda oluşturduğunuz.
6.  Olun **Entitlements.plist** içeren **etkinleştirmek iCloud** , **anahtar-değer depolama** ve **CloudKit** .
7.  Olun **Yaygınlığıyla kapsayıcı** (yukarıda oluşturduğunuz gibi) uygulama için bulunmaktadır. Örnek: `iCloud.com.your-company.CloudKitAtlas`
8.  Değişiklikleri dosyaya kaydedin.


Yerinde bu ayarlarla uygulama CloudKit Framework API'lerine erişmek hazır.

## <a name="cloudkit-api-overview"></a>CloudKit API genel bakış

Bir Xamarin iOS uygulaması'nda CloudKit uygulamadan önce bu makalede aşağıdaki konuları içerir CloudKit Framework temelleri kapsayacak şekilde geçiyor:

1.  **Kapsayıcıları** – yalıtılmış siloları iCloud iletişim.
2.  **Veritabanlarını** – ortak ve özel uygulama için kullanılabilir.
3.  **Kayıtları** – içinde yapılandırılmış veri taşındığı CloudKit gelen ve giden mekanizması.
4.  **Kayıt bölgeleri** – kayıtları gruplarıdır.
5.  **Kayıt tanımlayıcıları** – tam olarak normalleştirilmiş ve kayıt belirli konumunu temsil eder.
6.  **Başvuru** – üst-alt verilen bir veritabanı içinde ilgili kayıtlar arasındaki ilişkileri sağlayın.
7.  **Varlıklar** – İcloud'a karşıya ve belirli bir kaydıyla ilişkili büyük, yapılandırılmamış verileri dosyası izin verir.


### <a name="containers"></a>Kapsayıcılar

Bir iOS cihazında çalışan belirli bir uygulama her zaman çalışması bunların tarafı diğer uygulama ve hizmetlerin cihazdaki. İstemci cihazda uygulama siloed veya bazı şekilde korumalı olacak. Bazı durumlarda, sabit bir korumalı alan budur ve bazı durumlarda uygulamanın kendi bellek alanı içinde yalnızca çalışan.

Bir istemci uygulaması alma ve diğer istemcilerinden ayrılmış çalışan kavramı çok güçlü bir özelliktir ve aşağıdaki avantajları sağlar:

1.  **Güvenlik** – diğer istemci uygulamaları veya OS bir uygulama müdahale edemez.
1.  **Kararlılık** – istemci uygulama çökerse işletim sisteminin diğer uygulamalarını alamıyor.
1.  **Gizlilik** – her istemci uygulaması cihaz içinde depolanan kişisel bilgilere erişim sınırlıdır.


CloudKit, yukarıda listelenen olarak aynı avantajları sağlar ve bulut tabanlı bilgileriyle çalışmaya uygulamak için tasarlanmıştır:

 [![](intro-to-cloudkit-images/image31.png "Kapsayıcıları kullanma CloudKit uygulamaları iletişim")](intro-to-cloudkit-images/image31.png#lightbox)

Yalnızca uygulama olması gibi çok bir cihazda çalışan, iCloud çok bir uygulamanın iletişimlerinizde dolayısıyla taşır. Bu farklı iletişim siloları her kapsayıcıları çağrılır.

Kapsayıcıları CloudKit Framework'teki açığa `CKContainer` sınıfı. Varsayılan olarak, bu uygulama için verileri bir kapsayıcı ve bu kapsayıcı için bir uygulama konuşmaları ayırır. Bu, birkaç uygulama aynı iCloud hesabıyla bilgilerini depolamak henüz bu bilgileri hiçbir zaman birbirine anlamına gelir.

İCloud verilerin kapsayıcıyla taşıma kullanıcı bilgileri yalıtan CloudKit de sağlar. Bu şekilde uygulama bazı sınırlı iCloud hesabıyla erişebilir ve kullanıcı bilgilerini içinde tüm hala kullanıcının güvenlik ve gizlilik koruma sırasında depolanır.

Kapsayıcıları tam olarak WWDR Portalı aracılığıyla uygulama geliştiricisi tarafından yönetilir. Kapsayıcı yalnızca belirli bir geliştiricinin uygulamalara, benzersiz olmamalıdır tüm Apple geliştiriciler arasında genel ad alanı kapsayıcısının olduğundan ancak tüm Apple geliştiriciler ve uygulamalar için.

Uygulama kapsayıcıları için ad alanı oluştururken, ters DNS gösterimini kullanarak Apple önerir. Örnek:

```csharp
iCloud.com.company-name.application-name
```

Kapsayıcılar, bağlı varsayılan olarak, belirli bir uygulamaya birebir olsa da, uygulamalar arasında paylaşılabilir. Bu nedenle birden çok uygulama tek bir kapsayıcıda koordine edebilir. Tek bir uygulama için birden çok kapsayıcı de iletişim kurabilirsiniz.

### <a name="databases"></a>Veritabanları

Birincil işlevlerinden CloudKit biri uygulamanın veri modeli ve çoğaltma iCloud sunucuları kadar bu modelin almaktır. Bazı bilgiler oluşturan kullanıcıya yönelik, diğer bilgileri (örneğin, bir restoran incelemesi) genel kullanım için bir kullanıcı tarafından oluşturulmuş genel verilerdir veya uygulama için geliştirici yayımladıktan bilgi olabilir. Her iki durumda da İzleyici yalnızca tek bir kullanıcı değil, ancak kişiler bir topluluktur.

 [![](intro-to-cloudkit-images/image32.png "CloudKit kapsayıcı diyagramı")](intro-to-cloudkit-images/image32.png#lightbox)

Bir kapsayıcı içinde öncelikle ortak veritabanıdır. Burada tüm genel bilgileri yaşar ve birlikte mingles budur. Ayrıca, uygulamanın her kullanıcı için birkaç tekil özel veritabanları vardır.

Bir iOS cihazında çalıştırıldığında, uygulamanın yalnızca bilgileri şu anda oturum açmış iCloud kullanıcı için erişebilir. Bu nedenle uygulamanın görünümü kapsayıcısının şu şekilde olacaktır:

 [![](intro-to-cloudkit-images/image33.png "Kapsayıcı uygulamaları görünümü")](intro-to-cloudkit-images/image33.png#lightbox)

Yalnızca ortak veritabanı ve şu anda oturum açmış iCloud kullanıcıyla ilişkili özel veritabanı görebilirsiniz.

Veritabanları CloudKit Framework'teki açığa `CKDatabase` sınıfı. Her uygulama iki veritabanı erişimi: Genel veritabanını ve özel bir.

Kapsayıcı CloudKit içine ilk giriş noktasıdır. Aşağıdaki kod, uygulamanın varsayılan kapsayıcı ortak ve özel veritabanına erişmek için kullanılabilir:

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

Veritabanı türleri arasındaki farklar şunlardır:

||Genel veritabanı|Özel veritabanı|
|---|--- |--- |
|**Veri türü**|Paylaşılan veri|Geçerli kullanıcının veri|
|**Kota**|İçin geliştiricinin kota hesaba|İçin kullanıcının kota hesaba|
|**Varsayılan izinleri**|Dünya okunabilir|Kullanıcı okunabilir|
|**Düzenleme izinleri**|bir kayıt sınıfı düzeyi aracılığıyla iCloud Pano rolleri|Yok|

### <a name="records"></a>Kayıtlar

Veritabanlarını ve kayıtları veritabanlarıdır içinde kapsayıcıları basılı tutun. Kayıtları yapılandırılmış veri CloudKit gelen ve giden taşınır mekanizması şunlardır:

 [![](intro-to-cloudkit-images/image34.png "Kapsayıcılar, veritabanlarını ve kayıtları veritabanlarıdır içinde tutun")](intro-to-cloudkit-images/image34.png#lightbox)

Kayıtları CloudKit Framework'teki açığa `CKRecord` anahtar-değer çiftleri sarmalar sınıfı. Bir uygulamada bir nesnenin örneğine eşdeğerdir bir `CKRecord` CloudKit içinde. Ayrıca, her `CKRecord` sahip bir nesne sınıfı için eşdeğer bir kayıt türü.

Veri CloudKit için devre dışı işleme için teslim önce anlatılan şekilde kayıtların tam zamanı şeması vardır. Bu noktadan itibaren CloudKit bilgilerini yorumlamak ve depolama ve kayıt alma Lojistik tanıtıcı.

`CKRecord` Sınıfı, çok çeşitli meta verileri de destekler. Örneğin, bir kayıt oluşturulma ve onu oluşturan kullanıcı hakkındaki bilgileri içerir. Bir kayıt en son değiştirildiği ve üzerinde değişiklik kullanıcı hakkındaki bilgileri de içerir.

Bir değişiklik etiketi kavramı kayıtları içerir. Verilen kayıt düzeltilmesi önceki bir sürümünü budur. Değişiklik etiketi istemci ve sunucu verilen kayıt aynı sürümüne sahipseniz, belirlemek hafif şekilde kullanılır.

Yukarıda belirtildiği gibi `CKRecords` anahtar-değer çiftleri kaydırma ve bu nedenle, aşağıdaki veri türlerini bir kayıtta depolanabilir:

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


Tek değer türlerine ek olarak, yukarıda listelenen türlerinden herhangi birini homojen bir dizi bir kaydı içerebilir.

Aşağıdaki kod, yeni bir kayıt oluşturmak ve bir veritabanında depolamak için kullanılabilir:

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>Kayıt bölgeleri

Kayıtları kendileri tarafından verilen bir veritabanı içinde mevcut olmayan – kayıt bölge içinde kayıt gruplarının birlikte mevcut. Kayıt bölgeleri, tablo olarak geleneksel ilişkisel veritabanlarında değerlendirilebilir:

 [![](intro-to-cloudkit-images/image35.png "Kayıt grupları birlikte bir kayıt bölge içinde mevcut")](intro-to-cloudkit-images/image35.png#lightbox)

Belirli bir kayıt bölge içinde birden çok kayıt ve verilen bir veritabanı içinde birden çok kayıt bölgeleri olabilir. Her veritabanı varsayılan kayıt bölge içerir:

 [![](intro-to-cloudkit-images/image36.png "Her veritabanı bir varsayılan kayıt hem de özel bölgesine içerir")](intro-to-cloudkit-images/image36.png#lightbox)

Varsayılan olarak kayıtları depolandığı budur. Ayrıca, özel kayıt bölgeleri oluşturulabilir. Hangi atomik işlemeleri ve değişiklik izleme temel bazda yapılır kayıt bölgeleri temsil eder.

## <a name="record-identifiers"></a>Kayıt tanımlayıcıları

Kayıt tanımlayıcıları bir tanımlama grubu temsil edilir, her ikisini de içeren bir istemci kaydı sağlanan adı ve kayıt bulunduğu bölge. Kayıt tanımlayıcıları aşağıdaki özelliklere sahiptir:

-  Bunlar istemci uygulaması tarafından oluşturulur.
-  Bunlar tam olarak normalleştirilmiş ve kayıt belirli konumunu temsil eder.
-  Kayıt adı yabancı veritabanındaki bir kaydı benzersiz kimliği atayarak bunların CloudKit içinde depolanmaz yerel veritabanlarını köprülemek için kullanılabilir.


Geliştiricilerin yeni kayıtlar oluşturduğunuzda, bir kayıt tanımlayıcısı geçirmek seçebilirsiniz. Bir kayıt tanımlayıcısı belirtilmezse, bir UUID otomatik olarak oluşturulur ve kayda atanmış.

Geliştiricilerin yeni kayıt tanımlayıcıları oluşturduğunuzda, her kayıt ait bir kayıt bölge belirtmek seçebilirsiniz. Belirtilmemişse, varsayılan kayıt bölge kullanılır.

Kayıt tanımlayıcıları CloudKit Framework'teki açığa `CKRecordID` sınıfı. Aşağıdaki kod, yeni bir kayıt tanımlayıcısı oluşturmak için kullanılabilir:

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>Referanslar

Başvuruları verilen bir veritabanı içinde ilgili kayıtlar arasındaki ilişkileri sağlar:

 [![](intro-to-cloudkit-images/image37.png "Verilen bir veritabanı içinde ilgili kayıtlar arasındaki ilişkileri başvuruları sağlayın")](intro-to-cloudkit-images/image37.png#lightbox)

Böylece alt üst kaydının bir alt kayıt yukarıdaki örnekte, üst Çocuklar kendi. İlişki alt kaydının üst kayda gider ve olarak adlandırılır bir *geri başvuru*.

Başvuruları CloudKit Framework'teki açığa `CKReference` sınıfı. Bunlar kayıtları arasındaki ilişkiyi anlamanız iCloud sunucu izin vererek, bir yoludur.

Başvuruları basamaklı siler arkasında mekanizma sağlar. Veritabanından bir üst kaydı silinirse, veritabanından herhangi bir alt kayıt (belirtildiği gibi bir ilişki) otomatik olarak silinir.

> [!NOTE]
> Sallantıdaki işaretçileri olasılığı CloudKit kullanılırken edilir. Örneğin, uygulama olan kayıt işaretçileri listesini getirilen, bir kayıt seçili ve kayıt için sorulan saatle kaydı artık veritabanında bulunmuyor olabilir. Bu durum kolaylıkla idare edecek bir uygulama kodlanmış olmalıdır.

Gerekli olmasa da, geri başvuruları CloudKit Framework ile çalışırken tercih edilir. Apple bu başvuru en verimli türü yapmak için sistem ince ayar.

Bir başvuru oluştururken, geliştirici bellekte zaten bir kayıt belirtebilir veya bir kayıt tanımlayıcı başvuru oluşturmak olabilir. Bir kayıt tanımlayıcısı ve belirtilen başvuru kullanarak veritabanında bulunamadı, sarkan işaretçi oluşturulması.

Bilinen bir kayıt karşı bir başvuru oluşturmanın bir örneği verilmiştir:

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>Varlıklar

Varlıklar İcloud'a karşıya ve belirli bir kaydıyla ilişkili büyük ve yapılandırılmamış veri dosyası için izin ver:

 [![](intro-to-cloudkit-images/image38.png "Varlıklar İcloud'a karşıya ve belirli bir kaydıyla ilişkili büyük ve yapılandırılmamış veri dosyası için izin ver")](intro-to-cloudkit-images/image38.png#lightbox)

İstemci üzerindeki bir `CKRecord` oluşturulur iCloud sunucuya karşıya yükleneceği dosya açıklar. A `CKAsset` dosyasını içerecek şekilde oluşturulur ve bu açıklayan kaydıyla ilişkili.

Dosya sunucusuna yüklendiğinde, kayıt veritabanında yerleştirilir ve dosya özel toplu depolama veritabanına kopyalanır. Yüklenen dosya ve kayıt işaretçi arasında bir bağlantı oluşturulur.

Varlıklar CloudKit Framework'teki açığa `CKAsset` sınıfı ve büyük ve yapılandırılmamış veri depolamak için kullanılır. Geliştirici hiçbir zaman büyük ve yapılandırılmamış veri belleğiniz istediğinden varlıklar diskteki dosyaları kullanılarak uygulanır.

Varlıklar, kaydı bir işaretçi olarak kullanarak iCloud alınacağı varlıklar verir kayıtları sahibi olur. Varlık sahibi kaydı silindiğinde, bu şekilde sunucu atık toplama varlıklar olabilir.

Çünkü `CKAssets` tasarlanmıştır büyük veri dosyalarını işlemek, Apple CloudKit verimli bir şekilde karşıya yükleme ve varlıkları indirmek için tasarlanmıştır.

Aşağıdaki kod, bir varlık oluşturun ve kaydıyla ilişkilendirmek için kullanılabilir:

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

Biz şimdi tüm CloudKit temel nesnelerinde kapsamına. Kapsayıcı uygulamaları ilişkili ve veritabanlarını içerir. Veritabanlarını kayıt bölgelere gruplandırılmış ve kayıt tanımlayıcılarla işaret kayıtları içerir. Üst-alt ilişkisi başvurular kullanarak kayıtları arasında tanımlanır. Son olarak, büyük dosyaları karşıya ve varlıkları kullanarak kayıtları için ilişkili.

## <a name="cloudkit-convenience-api"></a>CloudKit Convenience API

Apple CloudKit ile çalışmak için iki farklı API kümesi sunar:

-  **İşletimsel API** – CloudKit tek her özelliğini sunar. Daha karmaşık uygulamalar için bu API CloudKit üzerinde ayrıntılı denetim sağlar.
-  **Kolaylık API** – ortak, önceden yapılandırılmış özelliklerinin bir alt kümesi CloudKit sunar. Rahat ve kolay erişim çözümü CloudKit işlevselliği bir iOS uygulamasına dahil etmek için sağlar.


Kolaylık API genellikle çoğu iOS uygulamaları için en iyi seçimdir ve Apple ile başlayan önerir. Bu bölümde rest aşağıdaki kolaylık API konuları ele alınacaktır:

-  Bir kayıt kaydediliyor.
-  Bir kayıt getiriliyor.
-  Bir kaydı güncelleştirilemedi.


### <a name="common-setup-code"></a>Genel Kurulum kodu

CloudKit kolaylık API ile başlamadan önce gereken bazı standart kurulum kodu yok. Başlangıç uygulamanın değiştirerek `AppDelegate.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
        #endregion
    }
}
```

Yukarıdaki kod, uygulamanın geri kalanına çalışmak kolaylaştırmak için kısayollar olarak ortak ve özel CloudKit veritabanlarını kullanıma sunar.

Ardından, herhangi bir görünüm veya CloudKit kullanarak görünüm kapsayıcısını aşağıdaki kodu ekleyin:

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Bu almak için bir kısayol ekler `AppDelegate` ve yukarıda oluşturduğunuz ortak ve özel veritabanı kısayolları erişebilirsiniz.

Bu kod yerinde bir Xamarin iOS 8 uygulamasında CloudKit kolaylık API uygulama bir bakalım.

### <a name="saving-a-record"></a>Bir kayıt kaydetme

Yukarıdaki kayıtları ele alırken sunulan desenini kullanarak, aşağıdaki kod yeni bir kayıt oluşturmak ve kolaylık API ortak veritabanına kaydetmek için kullanın:

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Yukarıdaki kod hakkında dikkat edilecek noktalar üç:

1.  Çağırarak `SaveRecord` yöntemi `PublicDatabase`, geliştirici nasıl veri gönderilir, bunu yazılmakta kadar vb. hangi bölgeyi belirtmeniz gerekmez. Kolaylık API alma ayrıntılarını tümünün kendisini önemli.
1.  Arama zaman uyumsuz olarak çağrılır ve bir geri arama yordamı, başarı veya başarısızlık çağrısı tamamlandığında sağlar. Çağrı başarısız olursa bir hata iletisi sağlanacaktır.
1.  CloudKit yerel depolama/Kalıcılık sağlamaz; Bu yalnızca bir aktarım Orta olur. Bir kayıt kaydetmek için bir istek yapıldığında, bu nedenle, hemen iCloud sunucularına gönderilir.


> [!NOTE]
> Bağlantıları sürekli bırakılma veya kesintiye, CloudKit çalışmak hata işleme olduğunda Geliştirici olmalısınız ilk ile ilgili olduğu mobil ağ iletişimleri, "kayıplı" doğası nedeniyle.

### <a name="fetching-a-record"></a>Bir kaydı getirme

Oluşturulan ve başarıyla iCloud sunucusunda depolanan bir kayıtla kaydı almak için aşağıdaki kodu kullanın:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Kaydın kaydedilmesi gibi Yukarıdaki kod zaman uyumsuz, basit ve harika hata işleme gerekir.

### <a name="updating-a-record"></a>Kayıt güncelleştirme

Bir kayıt iCloud sunucularından alınan sonra aşağıdaki kodu kaydı değiştirebilir ve değişiklikleri veritabanına kaydetmek için kullanılabilir:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

`FetchRecord` Yöntemi `PublicDatabase` döndüren bir `CKRecord` çağrı başarılı ise. Uygulama kaydı ve çağrıları değiştirir `SaveRecord` yeniden değişiklikleri veritabanına yazmak için.

Bu bölümde, bir uygulama CloudKit kolaylık API ile çalışırken kullanacak tipik döngüsü göstermiştir. Uygulama İcloud'a kayıtları kaydetme, iCloud kayıtları almak, kayıtlarını değiştirme ve bu değişiklikleri geri İcloud'a kaydedin.

## <a name="designing-for-scalability"></a>Ölçeklenebilirlik için tasarlama

Şu ana kadar bu makale ile çalışılacak geçiyor her zaman bir uygulamanın tüm nesne modelini iCloud sunucularından alma ve depolamada baktığı. Bu yaklaşım iyi küçük miktarda veri ve temel çok küçük bir kullanıcı ile çalışırken, onu iyi zaman artar temel bilgileri ve/veya kullanıcı miktarını ölçeklenmez.

### <a name="big-data-tiny-device"></a>Büyük veri, küçük aygıt

Daha yaygın bir uygulama olur, veritabanında daha fazla veri ve bu cihazdaki tüm verilerin bir önbelleğe sahip az uygun değil. Bu sorunu çözmek için şu teknikler kullanılabilir:

-  **Bulutta büyük verileri tutmak** – CloudKit büyük veri verimli bir şekilde işlemek için tasarlanmıştır.
-  **İstemci bir dilim bu verilerin yalnızca görüntülemek** – belirli bir zamanda herhangi bir görev işlemek için gereken verileri tam en az kapalı duruma getirin.
-  **İstemci görünümleri değiştirebilir** – çünkü her kullanıcının farklı Tercihler vardır, görüntülenmesini veri dilimini kullanıcı başka bir kullanıcı değiştirebilir ve tek tek bir kullanıcının belirli bir dilim görünümünü farklı olabilir.
-  **İstemci görüş odaklanmaya sorguları kullanan** – sorguları, küçük bir alt kümesine var. daha büyük bir veri kümesi içinde bulut görüntülemek kullanıcı izin verir.


### <a name="queries"></a>Sorgular

Yukarıda belirtildiği gibi sorguları bulutta bulunan büyük veri kümesi küçük bir alt seçmek Geliştirici izin verir. Sorguları CloudKit Framework'teki açığa `CKQuery` sınıfı.

Bir sorgu üç farklı işlemler birleştirir: bir kayıt türü ( `RecordType`), bir koşul ( `NSPredicate`) ve isteğe bağlı olarak, bir sıralama tanımlayıcısı ( `NSSortDescriptors`). CloudKit çoğunu destekler `NSPredicate`.

#### <a name="supported-predicates"></a>Desteklenen koşulları

CloudKit aşağıdaki türlerini destekleyen `NSPredicates` sorgularıyla çalışırken:


1. Eşleşen adlı bir değişkende depolanan bir değere eşit olduğu kayıtlar:
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. Böylece anahtar olması öğrenmek derleme zamanında yok dinamik anahtar değerine dayanması eşleşen sağlar:
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. Kaydın değeri belirtilen değerden daha büyük olduğu kayıtları eşleşen:
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. Kaydın konum içinde 100 metre verilen konumunun olduğu kayıtları eşleşen:
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit parçalanmış arama destekler. Bu çağrı için iki belirteçleri oluşturacak `after` için başka bir `session`. Bu iki belirteci içeren bir kayıt döndürür:
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. CloudKit destekler bileşik kullanan katılmış koşulları `AND` işleci.
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>Sorgular oluşturma

Aşağıdaki kodu oluşturmak için kullanılan bir `CKQuery` bir Xamarin iOS 8 uygulamasında:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

İlk olarak, belirli bir ada uyan kayıtları seçmek için bir koşul oluşturur. Ardından koşulu ile eşleşen kayıtlar verilen kayıt türünün seçecektir bir sorgu oluşturur.

#### <a name="performing-a-query"></a>Bir sorgu gerçekleştirme

Bir sorgu oluşturulduktan sonra sorguyu gerçekleştirmek ve döndürülen kayıtlarını işlemek için aşağıdaki kodu kullanın:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

Yukarıdaki kod, yukarıda oluşturduğunuz sorgu alır ve ortak veritabanına karşı yürütür. Kayıt bölge belirtilen olduğundan, tüm bölgeler aranır. Herhangi bir hata oluştuysa, bir dizi `CKRecords` Sorgu parametrelerinin eşleşen döndürülür.

Sorgular hakkında düşünmek bunlar yoklamalar olan ve büyük veri kümelerinin üstünden dilimleme en iyi yoludur. Sorguları, ancak, aşağıdaki nedenlerden dolayı büyük, çoğunlukla statik veri kümeleri için uygun değildir:

-  Bunlar için cihaz pil ömrünün bozuk.
-  Bunlar ağ trafiği için hatalı.
-  Uygulama veritabanı ne sıklıkta sorgulanır tarafından göreceği bilgi sınırlı olduğundan kullanıcı deneyimi için hatalı. Bir şey değiştiğinde, kullanıcılar bugün anında iletme bildirimleri bekler.


### <a name="subscriptions"></a>Abonelikler

Büyük, çoğunlukla statik kümeleriyle ilgilenirken, sorgu istemci cihazında gerçekleştirilmelidir değil, istemci adına bir sunucuda çalıştırmanız gerekir. Sorgu arka planda çalışması gerektiğini ve tek tek her kayıt kaydettikten sonra geçerli aygıtı veya başka bir aygıt aynı veritabanını temas yürütülmelidir.

Son olarak, sunucu tarafı sorgu çalıştığında veritabanına bağlı her cihazın bir anında iletme bildirimi gönderilmelidir.

Abonelikler CloudKit Framework'teki açığa `CKSubscription` sınıfı. Bir kayıt türü birleştirmek ( `RecordType`), bir koşul ( `NSPredicate`) ve bir Apple anında iletilen bildirim ( `Push`).

> [!NOTE]
> CloudKit olmasını itme neyin neden olduğunu gibi belirli bilgileri içeren bir yükü içerdiğinden CloudKit iter biraz engagement'ta.

#### <a name="how-subscriptions-work"></a>Abonelikleri nasıl çalışır

C# kodunda abonelik uygulamadan önce nasıl abonelikleri işe hızlı bir genel bakış atalım:

 [![](intro-to-cloudkit-images/image39.png "Abonelikleri nasıl genel bakış")](intro-to-cloudkit-images/image39.png#lightbox)

Yukarıdaki grafikte tipik abonelik işlemi aşağıdaki gibi gösterir:

1.  İstemci aygıtı abonelik ve bir itme tetikleyici gerçekleştiğinde gönderilecek bildirim tetikleyecek koşullar kümesini içeren yeni bir abonelik oluşturur.
2.  Abonelik, burada mevcut abonelikleri koleksiyonuna eklenen veritabanına gönderilir.
3.  İkinci bir cihaz yeni bir kayıt oluşturur ve bu kaydı veritabanına kaydeder.
4.  Veritabanı, yeni kayıttaki kendi koşullardan herhangi biri eşleşip eşleşmediğini Aboneliklerin listesini arar.
5.  Bir eşleşme bulunamazsa, anında iletilen bildirim aboneliği tetiklenmesi neden kaydıyla ilgili bilgileri kayıtlı cihaza gönderilir.


Bu bilgiyle yerinde abonelikleri bir Xamarin iOS 8 uygulaması oluşturma sırasında bakalım.

#### <a name="creating-subscriptions"></a>Abonelik oluşturma

Aşağıdaki kod, bir abonelik oluşturmak için kullanılabilir:

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

İlk olarak, abonelik tetiklemek için koşulu sağlayan bir koşul oluşturur. Ardından, belirli bir kayıt türü karşı abonelik oluşturur ve ne zaman tetikleyici test seçeneğini ayarlar. Son olarak, abonelik tetiklenir ve abonelik iliştirir olduğunda meydana gelir bildirim türünü tanımlar.

### <a name="saving-subscriptions"></a>Abonelikleri kaydetme

Abonelik oluşturuldu, aşağıdaki kod, veritabanına kaydeder:

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

Kolaylık API'sini kullanarak, arama, zaman uyumsuz, basit ve kolay hata işleme sağlar.

#### <a name="handling-push-notifications"></a>Anında iletme bildirimlerini işleme

Geliştirici Apple anında iletme bildirimleri (AP'ler) daha önce kullandıysa CloudKit tarafından oluşturulan bildirimleri postalarla işlem bilgi sahibi olmanız gerekir.

İçinde `AppDelegate.cs`, geçersiz kılma `ReceivedRemoteNotification` gibi sınıfı:

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

Yukarıdaki kod CloudKit bildirim kullanıcı bilgisi ayrıştırmak için CloudKit sorar. Ardından, bilgi hakkında uyarı ayıklanır. Son olarak, bildirim türü sınanır ve bildirim uygun şekilde gerçekleştirilir.

Bu bölümde büyük veri, sorgular ve abonelikleri kullanarak yukarıdaki sunulan küçük aygıt sorunu yanıtlamak nasıl göstermiştir. Uygulama bulutta büyük veri bırakın ve görünümler bu veri kümesine sağlamak için bu teknolojiler kullanın.

## <a name="cloudkit-user-accounts"></a>CloudKit kullanıcı hesapları

Bu makalenin başlangıcında belirtildiği gibi CloudKit varolan iCloud altyapısının üzerinde oluşturulmuştur. Aşağıdaki bölümde, ayrıntılı olarak hesapları CloudKit API kullanarak bir geliştirici gösterilen nasıl ele alınacaktır.

### <a name="authentication"></a>Kimlik doğrulaması

Kullanıcı hesapları ile ilgilenirken, ilk kimlik doğrulaması noktadır. CloudKit kimlik doğrulaması yoluyla iCloud geçerli oturum açma kullanıcı cihazda destekler. Kimlik doğrulaması arka planda gerçekleşir ve iOS tarafından işlenir. Bu şekilde, geliştiriciler hiçbir zaman kimlik doğrulaması uygulama ayrıntılarını hakkında endişelenmeye gerek yoktur. Yalnızca bir kullanıcı oturum açtıysa görmek için test ederler.

### <a name="user-account-information"></a>Kullanıcı hesabı bilgileri

CloudKit geliştiriciler için aşağıdaki kullanıcı bilgileri sağlar:

-  **Kimlik** – kullanıcının benzersiz olarak tanımlayan bir yoludur.
-  **Meta veri** – kaydetmek ve kullanıcılar hakkında bilgi alma olanağı.
-  **Gizlilik** – tüm bilgileri bir gizlilik bilinçli manor ele alınır. Kullanıcı için kabul sürece hiçbir şey açıktır.
-  **Bulma** – kullanıcılar aynı uygulamayı kullanarak kullanıcıların arkadaşlarını bulma olanağı sağlar.


Ardından, bu konularda ayrıntılı ele alacağız.

#### <a name="identity"></a>Kimlik

Yukarıda belirtildiği gibi CloudKit uygulamanın belirli bir kullanıcının benzersiz şekilde tanımlamak bir yol sağlar:

 [![](intro-to-cloudkit-images/image40.png "Benzersiz olarak identifing belirli bir kullanıcının")](intro-to-cloudkit-images/image40.png#lightbox)

Bir kullanıcının cihazları ve tüm CloudKit kapsayıcı içindeki belirli kullanıcı özel veritabanları üzerinde çalışan bir istemci uygulaması yoktur. İstemci uygulaması belirli kullanıcılarla birine bağlanması geçiyor. Bu iCloud cihazda yerel olarak oturum açmış kullanıcının temel alır.

Bu iCloud geliyor olmadığından kullanıcı bilgilerini deposu yedekleme zengin yoktur. Ve iCloud gerçekte kapsayıcıyı barındıran olduğundan, kullanıcılar ilişkilendirebilirsiniz. Yukarıdaki grafikte, kullanıcının iCloud hesabı `user@icloud.com` geçerli istemci bağlandı.

Bir kapsayıcı tarafından kapsayıcı temelinde benzersiz, rastgele oluşturulan bir kullanıcı kimliği oluşturulur ve (e-posta adresi) kullanıcının iCloud hesabıyla ilişkilendirilmiş. Bu kullanıcı Kimliğini uygulamaya döndürülür ve geliştirici uygun herhangi bir şekilde kullanılabilir.

> [!NOTE]
> Farklı CloudKit kapsayıcılara bağlandığından aynı iCloud kullanıcı için aynı cihazda çalışan farklı uygulamalar farklı kullanıcı kimlikleri sahip olur.

Aşağıdaki kod alır CloudKit kullanıcı kimliği için şu anda oturum açmış iCloud kullanıcı cihazda:

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

Yukarıdaki kod şu anda oturum açmış olan kullanıcının Kimliğini sağlamak için CloudKit kapsayıcı istiyor. Bu bilgiler iCloud sunucu geliyor olduğundan, çağrı uyumsuzdur ve hata işleme gereklidir.

#### <a name="metadata"></a>Meta Veriler

Her kullanıcı CloudKit bunları açıklayan belirli meta veriler var. Bu meta veriler CloudKit kayıt olarak temsil edilir:

 [![](intro-to-cloudkit-images/image41.png "CloudKit her kullanıcı bunları açıklayan belirli meta veriler içeriyor")](intro-to-cloudkit-images/image41.png#lightbox)

Özel veritabanı içinde bir kapsayıcı var. belirli bir kullanıcı için Arayan Kullanıcının tanımlayan bir kayıttır. Genel veritabanı, her bir kullanıcının kapsayıcısının içinde çok sayıda kullanıcı kayıtlarını vardır. Bunlardan biri şu anda oturum açmış kullanıcının kayıt kimliği eşleşen bir kayıt kimliği gerekir

Kullanıcının ortak veritabanında world okunabilir kayıtlarıdır. Bunlar, çoğunlukla, normal bir kayıt olarak kabul edilir ve bir türüne sahip `CKRecordTypeUserRecord`. Bu kayıtlar sistem tarafından ayrılmış ve sorgular için kullanılabilir değil.

Bir kullanıcı kaydı erişmek için aşağıdaki kodu kullanın:

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

Yukarıdaki kod ortak biz yukarıda erişilen kimliği olan bir kullanıcı için kullanıcı kayıt döndürmek için veritabanı istiyor. Bu bilgiler iCloud sunucu geliyor olduğundan, çağrı uyumsuzdur ve hata işleme gereklidir.

#### <a name="privacy"></a>Gizlilik

Şu anda oturum açmış kullanıcı gizliliğini korumak için tasarım, varsayılan olarak, CloudKit oluştu. Kullanıcıyla ilgili hiçbir kişisel olarak tanımlayıcı bilgileri varsayılan olarak açıktır. Uygulamanın kullanıcı hakkındaki sınırlı bilgiler nerede gerektirecektir bazı durumlar vardır.

Bu durumlarda, kullanıcı bu bilgileri ifşa uygulama isteyebilir. Kullanıcıya, hesap bilgilerini gösterme için katılımı için bunları soran bir iletişim kutusu sunulur.

#### <a name="discovery"></a>Bulma

Kullanıcı olarak seçti-uygulama izin vermek için kullanıcı hesabı bilgilerini erişimi sınırlı varsayılarak, uygulamanın diğer kullanıcılara bulunabilirlik olabilir:

 [![](intro-to-cloudkit-images/image42.png "Bir kullanıcı uygulama diğer kullanıcılara bulunabilirlik olabilir")](intro-to-cloudkit-images/image42.png#lightbox)

İstemci uygulaması için bir kapsayıcı Bahsediyor ve kapsayıcı erişim kullanıcı bilgilerini İcloud'a Bahsediyor. Kullanıcı bir e-posta adresi sağlayabilir ve bulma geri kullanıcı hakkında bilgi almak için kullanılabilir. İsteğe bağlı olarak, kullanıcı kimliği kullanıcı hakkındaki bilgileri bulmak için de kullanılabilir.

CloudKit de şu anda oturum açmış arkadaşların olabilecek herhangi bir kullanıcı hakkında bilgi bulmak için bir yol sağlar tüm adres defteri sorgulamak iCloud kullanıcı. CloudKit işlemi kullanıcının Contact rehberi çekme ve diğer kullanıcının bu eşleşen uygulamanın bulup bulamayacağını anlamak için e-posta adresi kullanın.

Bu kullanıcının Contact rehberi erişimi sağlama veya kişilere erişimi onaylamak için kullanıcıya soruluyor olmadan yararlanmak uygulama sağlar. Hiçbir zaman CloudKit işlem erişimi yalnızca ilgili kişi bilgilerini uygulama için kullanılabilir hale getirilir.

Olduðunu için girişleri üç farklı türde kullanıcı bulma için kullanılabilen vardır:

-  **Kullanıcı kaydı kimliği** – bulma şu anda oturum açmış olan kullanıcının Kimliğine karşı yapılabilir CloudKit kullanıcı.
-  **Kullanıcı e-posta adresi** – kullanıcı bir e-posta adresi sağlayabilir ve bulma için kullanılabilir.
-  **Kişi kitap** – kullanıcının adres defteri uygulamanın kendi kişiler listelendiği gibi aynı e-posta adresine sahip kullanıcıları bulmak için kullanılabilir.


Kullanıcı keşfi aşağıdaki bilgileri döndürür:

-  **Kullanıcı kaydı kimliği** -ortak veritabanında bir kullanıcı benzersiz kimliği.
-  **İlk ve son adı** - ortak veritabanında depolandığı şekliyle.


Bu bilgiler yalnızca seçti içine bulma sahip kullanıcılar için döndürülür.

Aşağıdaki kod iCloud cihazda şu anda oturum açmış kullanıcı hakkındaki bilgileri keşfeder:

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

Kişi kitaptaki tüm kullanıcıları sorgulamak için aşağıdaki kodu kullanın:

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

Bu bölümde biz dört temel alanları uygulamaya CloudKit sağlayan bir kullanıcı hesabına erişim kapsamına. Kullanıcının kimliği ve meta veri bulaşmasından gizlilik ilkeleri, CloudKit ve son olarak, diğer kullanıcıları uygulamanın bulma olanağı oluşturulmuştur.

## <a name="the-development-and-production-environments"></a>Geliştirme ve üretim ortamları

CloudKit, bir uygulamanın kayıt türleri ve verileri için ayrı geliştirme ve üretim ortamları sağlar. Geliştirme ortamı yalnızca geliştirme ekibi üyeleri için kullanılabilir olan daha esnek ortamıdır. Bir uygulamanın yeni bir alan için bir kayıt ekler ve bu kaydı geliştirme ortamında kaydeder, sunucu şema bilgileri otomatik olarak güncelleştirir.

Geliştirici, zaman kazandırır geliştirme sırasında bir şema değişiklikleri yapmak için bu özelliği kullanabilirsiniz. Bir uyarı, bir alan için bir kayıt eklendikten sonra o alanıyla ilişkili veri türü'nin programlı olarak değişiklik yapılamaz ' dir. Bir alanın türünü değiştirmek için geliştirici alanında silmelisiniz [CloudKit Pano](https://icloud.developer.apple.com/dashboard/) ve yeni türüyle tekrar ekleyin.

Uygulamayı dağıtmadan önce geliştirici kendi şeması ve verisi üretim ortamı kullanarak geçirebilirsiniz **CloudKit Pano**. Üretim ortamında çalıştırırken, sunucu şema programlı bir uygulamanın engeller. Geliştirici değişikliklerle hale getirebilir **CloudKit Pano** ancak hataları üretim ortamına sonucunda kaydındaki alanları eklemek çalışır.

> [!NOTE]
> Simulator çalışır yalnızca iOS **geliştirme ortamı**. Geliştirici olduğunda bir uygulamayı test etmek hazır bir **üretim ortamında**, fiziksel bir iOS cihazına gereklidir.


## <a name="shipping-a-cloudkit-enabled-app"></a>Uygulama etkin bir CloudKit aktarma

CloudKit kullanan bir uygulamayı göndermeden önce yapılandırılması gerekli hedefine **üretim CloudKit ortam** veya uygulama Apple tarafından reddedilir.

Aşağıdakileri yapın:

1. Uygulama için Ma için Visual Studio'da derleme **sürüm** > **iOS aygıtı**: 

    [![](intro-to-cloudkit-images/shipping01.png "Uygulama sürümü için derleme")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. Gelen **yapı** menüsünde, select **arşiv**: 

    [![](intro-to-cloudkit-images/shipping02.png "Arşiv seçin")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. **Arşiv** oluşturulur ve Mac için Visual Studio'da görüntülenir: 

    [![](intro-to-cloudkit-images/shipping03.png "Arşiv oluşturulur ve görüntülenir")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. Başlat **Xcode**.
5. Gelen **penceresi** menüsünde, select **düzenleyici**: 

    [![](intro-to-cloudkit-images/shipping04.png "Düzenleyici'yi seçin")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. Uygulamanın Arşivi'ni seçin ve'ı tıklatın **dışarı aktar...**  düğmesi: 

    [![](intro-to-cloudkit-images/shipping05.png "Uygulamanın arşiv")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. Dışa aktarma için bir yöntem seçin ve tıklatın **sonraki** düğmesi: 

    [![](intro-to-cloudkit-images/shipping06.png "Dışa aktarma için bir yöntem Seç")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. Seçin **geliştirme ekibi** tıklatın ve açılır listeden **Seç** düğmesi: 

    [![](intro-to-cloudkit-images/shipping07.png "Geliştirme ekibi açılır listeden seçin")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. Seçin **üretim** tıklatın ve açılır listeden **sonraki** düğmesi: 

    [![](intro-to-cloudkit-images/shipping08.png "Üretim açılır listeden seçin")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. ' I tıklatın ve ayarı gözden **verme** düğmesi: 

    [![](intro-to-cloudkit-images/shipping09.png "Ayarları gözden geçirin")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. Sonuçta elde edilen uygulama oluşturmak için bir konum seçin `.ipa` dosya.

İşlem iTunes Bağlan uygulamaya doğrudan göndermek için benzer, yalnızca tıklatın **Gönder...**  Düzenleyicisi penceresinde bir arşiv seçtikten sonra... dışa aktarma düğmesi.

## <a name="when-to-use-cloudkit"></a>Ne zaman CloudKit kullanılır

Bu makalede anlatıldığı gibi CloudKit depolamak ve iCloud sunucularından bilgileri almak bir uygulama için kolay bir yol sağlar. Başka bir deyişle, CloudKit artık kullanılmıyor ya da mevcut araçlar ya da çerçeveleri herhangi birini alanı onaylanamadı.

### <a name="use-cases"></a>Kullanım örnekleri

Aşağıdaki kullanım örnekleri Geliştirici belirli iCloud framework veya teknolojisini kullanan karar vermenize yardımcı olmalıdır:

-  **iCloud anahtar değeri deposu** – zaman uyumsuz olarak küçük miktarda veri güncel tutar ve uygulama tercihlerini ile çalışmak için mükemmeldir. Ancak, çok az miktarda bilgi için sınırlı değildir.
-  **iCloud sürücü** – yerleşik en üstünde mevcut iCloud belgeleri API'leri ve dosya sisteminden yapılandırılmamış verileri eşitlemek için basit bir API sağlar. Mac OS X üzerinde tam çevrimdışı bir önbellek sağlar ve belge merkezli uygulamaları için mükemmeldir.
-  **iCloud çekirdek veri** – tüm kullanıcı aygıtları arasında çoğaltılması veri sağlar. Tek kullanıcı ve tutma özel, yapılandırılmış veri eşitleme için harika bir veridir.
-  **CloudKit** – genel veriler hem yapısı sağlar ve toplu ve büyük veri kümesi ve büyük yapılandırılmamış dosyalarını işleyebilir. Kullanıcı kendi bağlı iCloud hesap adı ve sağlar istemci yönlendirilmiş veri aktarımı.


Bunlar tutma göz önünde kullanım örnekleri, geliştirici hem bir geçerli gerekli uygulama işlevleri sağlamak ve gelecekteki büyümeyi de iyi ölçeklenebilirlik sağlamak için doğru iCloud teknolojisi seçmeniz gerekir.

## <a name="summary"></a>Özet

Bu makalede CloudKit API giriş kapsamına. Sağlama ve CloudKit kullanmak için bir Xamarin iOS uygulaması yapılandırma göstermiştir. CloudKit kolaylık API özelliklerini ele. Göster sahip bir CloudKit tasarlamak nasıl uygulama sorgular ve abonelikleri kullanarak ölçeklenebilirlik için etkinleştirilmiş. Ve son olarak bir uygulamaya CloudKit tarafından kullanıma sunulan kullanıcı hesabı bilgileri göstermiştir.

## <a name="related-links"></a>İlgili bağlantılar

- [CloudKitAtlas (sample)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [iOS 8’e Giriş](~/ios/platform/introduction-to-ios8.md)
- [Bir sağlama profili oluşturma](~/ios/get-started/installation/device-provisioning/index.md)
