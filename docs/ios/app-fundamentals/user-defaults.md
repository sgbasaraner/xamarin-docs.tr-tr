---
title: Kullanıcı varsayılanları ile çalışma
description: Bu makalede, bir Xamarin iOS uygulama veya uzantı varsayılan ayarları kaydetmek için NSUserDefault çalışmak yer almaktadır.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: aa28e7d5636b06c8ab1e46457537431b5d1c7f1a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-user-defaults"></a>Kullanıcı varsayılanları ile çalışma

_Bu makalede, bir Xamarin.iOS uygulaması veya uzantısı varsayılan ayarları kaydetmek için NSUserDefault çalışmak yer almaktadır._


`NSUserDefaults` Sınıfı iOS uygulamaları ve uzantıları varsayılan olarak sistem genelinde sistemiyle program aracılığıyla etkileşim kurmak için bir yol sağlar. Varsayılan olarak sistem kullanarak, kullanıcı bir uygulamanın davranışı veya (uygulama tasarımını göre) tercihlerini karşılamak için stil oluşturma yapılandırabilirsiniz. Örneğin, ölçüm vs verilerde İngiliz ölçümleri sunan veya belirli bir kullanıcı Arabirimi tema seçin.

Uygulama grupları ile kullanıldığında `NSUserDefaults` Ayrıca belirli bir grubunda uygulamalar arasındaki (veya Uzantılar) iletişim kurmak için bir yol sağlar.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>Kullanıcı Varsayılanları hakkında

Kullanıcı varsayılan olarak, yukarıda belirtildiği gibi (`NSUserDefaults`) bir uygulama (veya uzantısı) eklenir ve son kullanıcı görünümünü veya işlemi çalışma zamanında uygulamanın ayarlamak için değiştirebileceğiniz yapılandırılabilir seçeneklerin sağlamak için kullanılır.

Uygulamanızı ilk kez yürütüldüğünde, `NSUserDefaults` anahtarları ve değerleri uygulamanın kullanıcı Varsayılanları veritabanından okur ve onları belleğe açma ve veritabanı her zaman okunurken önlemek için bir değer gereklidir önbelleğe alır. 

> [!IMPORTANT]
> Apple artık önerir Geliştirici çağrısı `Synchronize` bellek içi önbellek veritabanı ile doğrudan eşitleme yöntemi. Bunun yerine, otomatik olarak bellek içi önbellek kullanıcının varsayılan veritabanı ile eşitlenmiş tutmak için düzenli aralıklarla çağrılır.

`NSUserDefaults` Sınıfı okumak ve tercih değerleri için ortak veri türleri gibi yazmak için birkaç kullanışlı yöntemler içerir: dize, tamsayı, kayan nokta, Boole ve URL'leri. Diğer veri türleri kullanılarak arşivlenebilir `NSData`, ardından okuma veya varsayılan olarak kullanıcı veritabanına yazılır. Daha fazla bilgi için lütfen Apple'nın bkz [tercihlerini ve ayarlarını Programlama Kılavuzu](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Paylaşılan NSUserDefaults örneği erişme 

Paylaşılan kullanıcı Varsayılanları örneği erişim kullanıcı varsayılan olarak cihaz geçerli kullanıcı için sağlar. Paylaşılan varsayılan olarak nesne yoksa, ilk kez erişilen ve aşağıdaki bilgilerle başlatılan oluşturulur:

- Bir `NSArgumentDomain` geçerli uygulamadan Ayrıştırılan Varsayılanları oluşan.
- Uygulamanın paket tanımlayıcısı etki alanı.
- Bir `NSGlobalDomain` tüm uygulamalar tarafından paylaşılan Varsayılanları oluşan.
- Her kullanıcının tercih edilen dilleri için ayrı bir etki.
- Bir `NSRegistrationDomain` aramaları her zaman başarılı olduğundan emin olmak için uygulama tarafından değiştirilebilir geçici Varsayılanları kümesiyle.

Paylaşılan kullanıcı Varsayılanları örneğine erişmek için aşağıdaki kodu kullanın:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Bir uygulama grubu NSUserDefaults örneği erişme

Uygulama grupları kullanarak, yukarıda belirtildiği gibi `NSUserDefaults` uygulamalar arasındaki (veya Uzantılar) iletişim kurmak için belirli bir grup içinde kullanılabilir. Öncelikle, uygulama grubu ve gerekli uygulama kimlikleri doğru olarak yapılandırıldığından emin olmak gerekir **tanımlayıcıları & profilleri, sertifikaları** bölümünde [iOS Geliştirme Merkezi](https://developer.apple.com/devcenter/ios/) ve yüklendi geliştirme ortamında.

Ardından, uygulama ve/veya uzantı projelerinizi, yukarıda oluşturduğunuz geçerli uygulama kimlikleri biri olması gerekir ve `Entitlements.plist` dosya sahip uygulama etkinleştirilmiş ve belirtilen grupları ile uygulama paketi dahil edilecek.

Yerinde tüm ile bu paylaşılan uygulama grubu kullanıcı varsayılan olarak aşağıdaki kodu kullanarak erişilebilir:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Burada `group.com.xamarin.todaysharing` uygulama grubu oluşturulan **tanımlayıcıları & profilleri, sertifikaları** erişmek istediğiniz. Daha fazla bilgi için lütfen bkz [uygulama grubu özellikleri](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) belgeleri.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Varsayılan değerleri okuma

İstenen kullanıcı varsayılan veritabanı eriştiğiniz sonra anahtar/değer çiftleri ve okunan veri türüne göre birkaç kullanışlı yöntemler kullanarak varsayılandan değerleri okuyabilirsiniz:

- `ArrayForKey` -Bir dizi döndürür `NSObjects` verilen anahtarı değeri.
- `BoolForKey` -Belirtilen anahtar için bir Boole değeri döndürür.
- `DataForKey` -Döndüren bir `NSData` nesnesi belirtilen anahtar için.
- `DictionaryForKey` -Döndüren bir `NSDictionary` verilen anahtar için.
- `DoubleForKey` -Belirtilen anahtar için bir çift değer döndürür.
- `FloatForKey` -Belirtilen anahtar için bir float değeri döndürür.
- `IntForKey` -Belirtilen anahtar için bir Integer değeri döndürür.
- `StringArrayForKey` -Bir dizi döndürür `String` nesneleri belirli anahtar değeri.
- `StringForKey` -Belirtilen anahtar için bir string değeri döndürür.
- `URLForKey` -Döndüren bir `NSUrl` verilen anahtar için değer.

Örneğin, aşağıdaki kod, kullanıcı Varsayılanları bir Boole değeri okuduğunuz:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>Varsayılan değerleri yazılıyor

İstenen kullanıcı varsayılan veritabanı eriştikten sonra yalnızca okuma değerleri yukarıdaki gibi değerleri anahtar/değer çiftleri ve yazılan veri türüne göre birkaç kullanışlı yöntemler kullanarak varsayılanlarına yazabilirsiniz:

- `SetBool` -Belirli Boole değeri için belirtilen anahtar yazar.
- `SetDouble` -Belirtilen anahtar için verilen çift değer yazar.
- `SetFloat` -Belirtilen float değeri için belirtilen anahtar yazar.
- `SetString` -Belirtilen dize değeri için belirtilen anahtar yazar.
- `SetURL` -Verilen URL'nin yazar (`NSUrl`) verilen anahtar değeri.

Örneğin, aşağıdaki kod, kullanıcı varsayılan olarak bir Boole değeri yazarsınız:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> Uygulamanızı ilk kez yürütüldüğünde, `NSUserDefaults` anahtarları ve değerleri uygulamanın kullanıcı Varsayılanları veritabanından okur ve onları belleğe açma ve veritabanı her zaman okunurken önlemek için bir değer gereklidir önbelleğe alır.



<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ele alınan `NSUserDefaults` sınıfı ve nasıl bu son kullanıcı Xamarin.iOS Uygulamanızı yapılandırmak için kullanabileceğiniz seçenekleri kümesi sağlamak için kullanılabilir. Ayrıca, bir uzantı ve kendi üst uygulama veya bir grup içindeki uygulamalar arasında iletişim kurmak için uygulama gruplarını kullanarak ele.


## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [Tercihlerini ve ayarlarını Programlama Kılavuzu](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
