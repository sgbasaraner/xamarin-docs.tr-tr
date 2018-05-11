---
title: Özel Bağlayıcı yapılandırması
ms.prod: xamarin
ms.assetid: F8A99E3F-2197-4399-AC81-F1DBAB5729C9
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 333fe4163cea53217456f75249a191d66d181cb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="custom-linker-configuration"></a>Özel Bağlayıcı yapılandırması

Varsayılan seçenek kümesi varsa bağlayıcı istediğiniz tanımlayan bir XML dosyası ile bağlama işlemini sürücü yeterli değildir.

Bağlayıcıya türü emin olmak için fazladan tanımları sağlayabilir, uygulamanızdan yöntemleri ve/veya alanlar ortadan değil. Kendi kodunuzu tercih edilen yol kullanmaktır `[Preserve]` ele olarak özel öznitelik [İos'ta bağlama](~/ios/deploy-test/linker.md) ve [Android bağlama](~/android/deploy-test/linker.md) kılavuzları.
Ancak bazı tanımları gerekiyorsa bir XML dosyası kullanarak SDK veya ürün derlemeler (karşı bağlayıcı gerekenler ortadan olmaz sağlayacak kod ekleme), en iyi çözüm olabilir.

Bunu yapmak için en üst düzey öğesi içeren bir XML dosyası tanımlamak <linker> içeren *derleme* sırayla içeren düğümleri *türü* sırayla içeren düğümleri *yöntemi* ve *alan* düğümleri.

Bu bağlayıcı açıklama dosyası olduktan sonra projenize ekleyin ve:

-  **Android için** : ayarlamak **yapı eylemi** için **LinkDescription**
-  **İOS için** : ayarlamak **yapı eylemi** için **LinkDescription**


Aşağıdaki örnek, XML dosyasının nasıl göründüğünü gösterir:

```xml
<linker>
        <assembly fullname="mscorlib">
                <type fullname="System.Environment">
                        <field name="mono_corlib_version" />
                        <method name="get_StackTrace" />
                </type>
        </assembly>
        <assembly fullname="My.Own.Assembly">
                <type fullname="Foo" preserve="fields">
                        <method name=".ctor" />
                </type>
                <type fullname="Bar">
                        <method signature="System.Void .ctor(System.String)" />
                        <field signature="System.String _blah" />
                </type>
                <namespace fullname="My.Own.Namespace" />
                <type fullname="My.Other*" />
        </assembly>
</linker>
```

Yukarıdaki örnekte, bağlayıcı işlem okuyun ve yönergeleri uygulamak `mscorlib.dll` (Android için Mono ile birlikte gelen) ve `My.Own.Assembly` (kullanıcı kodu) derlemeler.

İlk bölümü için `mscorlib.dll`, şunları sağlar `System.Environment` türü adlı kendi alanı korumak `mono_corlib_version` ve kendi `get_StackTrace` yöntemi.
Alıcı unutmayın ve/veya bağlayıcı IL üzerinde çalışır ve C# özellikleri anlamıyor ayarlayıcı yöntemi adlar kullanılmalıdır.

İkinci bölümü için `My.Own.Assembly.dll`, şunları sağlar `Foo` türü, tüm alanları korumak (yani `preserve="fields"` özniteliği) ve tüm oluşturucular (adlı yani tüm yöntemleri `.ctor` IL içinde). `Bar` Türü belirli imzalar (adları değil) ve belirli dize alanı (tek bir dize parametresi kabul ettiğinde) bir oluşturucu için korumak `_blah`.
`My.Own.Namespace` Ad alanı, içerdiği tüm türleri korumak.
Son olarak, tam adı (ad alanı dahil) joker karakter deseni ile eşleşen hiçbir türü "My.Other\*" tüm alanları ve yöntemleri korur. Joker karakter `*` birden çok kez "fullname tür" desen içinde dahil edilebilir.



## <a name="related-links"></a>İlgili bağlantılar

- [İos'ta bağlama](~/ios/deploy-test/linker.md)
- [Android bağlama](~/android/deploy-test/linker.md)
