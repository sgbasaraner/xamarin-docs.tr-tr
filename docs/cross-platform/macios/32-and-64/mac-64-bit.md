---
title: 64-bit uygulamaları Xamarin.Mac birleşik güncelleştiriliyor
description: Bu kılavuz, hedef 64-bit, Xamarin.Mac uygulamalarınızı güncelleştirmeye açıklar. Ayrıca, bu değişiklik yaparken karşılaşılan hataları tür örnekleri sağlar.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: aa97f9a68ea4acc4234233a22d10c99cde3e6d6c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780704"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>64-bit uygulamaları Xamarin.Mac birleşik güncelleştiriliyor

Ocak 2018 sürümünden itibaren bu yeni Apple gerekli [Mac App Store gönderimlerini hedef 64-bit](https://developer.apple.com/news/?id=06282017a). Mac App Store uygulamaları zaten kullanılabilir hedef 64-bit Haziran 2018 tarafından güncelleştirilmesi gerekir.

**Dosya** > **yeni** Xamarin.Mac proje şablonu oluşturur 64-bit uygulamalar varsayılan olarak, son oluşturduğunuz tüm uygulamalar zaten 64-bit uyumlu değildir ve herhangi bir değişiklik gerektirmez.

## <a name="targeting-64-bit"></a>64-bit hedefleme

1. Açık **proje seçenekleri** penceresi, olduğunuz Xamarin.Mac uygulamanızı:

   ![Projesi için bağlam menüsünde](mac-64-bit-images/1-contextual_menu-vsmac.png "projesi için bağlam menüsü")

2. Seçin **Mac yapı** ve **desteklenen mimariler** için **x86\_64**:

   [![Desteklenen mimariler için x86_64 ayarı](mac-64-bit-images/2-project_options-vsmac.png "x86_64 için desteklenen mimariler ayarlama")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Uygulamanızı yerel başvuruları veya bağlama projeleri gibi dış bağımlılıkları varsa, bunları hedef 64-bit güncelleştirin.

### <a name="errors"></a>Hatalar

Derleme veya 64-bit desteği ile uygulamanızı çalıştırın ilk kez clang veya çalışma zamanı sorunlardan bağlantı hatalarla karşılaşabilir. Bu hatalar üçüncü taraf oluşabilir bağımlılıkları — Örneğin, yerel başvuruları Xamarin.Mac veya bağlamaları projeleri veya el ile yüklenen sistem genelinde çerçeveler — 64 bit güncelleştirilmemiş.

> [!TIP]
> 64 bit projenizi dönüştürme önemli bir değişikliktir ve çeşitli programlama hatalar dolaylı olarak açığa. Özellikle boyutu ve hizalama p/Invoke imzalar ve yerel kodu projenizde bağlı etkileyeceği veri yapılarının değişebilir. Verilen herhangi bir derleme Uyarıları gözden geçirme göz önünde bulundurun ve uygulamanızı throughly daha sonra olası sorunları yakalamak için test edin.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64-bit hedeflemiyor dinamik olarak bağlı üçüncü taraf bağımlılığı elde edilen örnek hata:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Bu hata, çalışma zamanında tarafından izlenmesi `dlopen` döndüren `IntPtr.Zero` yerine bir beklenen tanıtıcısı.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64-bit hedeflemiyor statik olarak bağlantılı üçüncü taraf bağımlılığı elde edilen örnek hata:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Derleme ve başarılı bir şekilde çalıştırmak için 64-bit Bu bağımlılıklar güncelleştirin ve uygulamanızı yeniden derleyin.

