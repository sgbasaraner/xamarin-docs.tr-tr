---
title: ApiDefinitions ve StructsAndEnums dosyaları
description: Bu belgenin amacı Sharpie oluşturan ApiDefinitions.cs ve StructsAndEnums.cs dosyalarını açıklar. Bu dosyalar daha sonra Objective-C kodunun C# ' tan erişmek için kullanılır.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855188"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions ve StructsAndEnums dosyaları

Hedefi Sharpie başarıyla çalıştırıldığında, ürettiği `Binding/ApiDefinitions.cs` ve `Binding/StructsAndEnums.cs` dosyaları.
Bu iki dosyayı Mac için Visual Studio'da bir bağlama projesi eklenir veya doğrudan `btouch` veya `bmac` son bağlamayı oluşturmak için Araçlar.

İçinde *bazı* çalışmaları üretilen bu dosyaları olabilir tüm yapmanız gereken, ancak daha geliştirici bu el ile değiştirmeniz gerekecektir oluşturulan dosyaları otomatik olarak (örneğin bu bayrak aracı tarafından işlenemedi sorunları düzeltmek için ile bir [ `Verify` özniteliği](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Sonraki adımlardan bazıları şunlardır:

- **Adları ayarlama**: bazen yöntemleri ve .NET Framework tasarım yönergeleri eşleştirilecek sınıflarının adları Ayarla isteyeceksiniz.
- **Yöntemleri veya özellikleri**: bazen hedefi Sharpie tarafından kullanılan buluşsal bir özellikte açılması için bir yöntem seçer. Bu noktada, bu amaçlanan bir davranış olup olmadığını karar.
- **Olayları bağlama**: sınıflarınızı temsilci sınıflarınızı ile bağlayın ve olayları için otomatik olarak oluşturun.
- **Bildirimleri kanca**: bildirimler API sözleşmesine saf üstbilgi dosyalarından ayıklamak mümkün değildir, bu API belgelerine bir seyahat gerektirir. Kesin türü belirtilmiş bildirimleri isterseniz sonuç güncelleştirme gerekir.
- **API Seçkisi**: Bu noktada, arasından seçim ek oluşturucular sağlamak için yöntemler (C# yapı üzerinde başlatma sözdizimi için izin vermek için), İşleç aşırı yüklemesi ve uygulama ekleyebileceğiniz ek tanımları dosya çubuğunda kendi arabirimleri.

Bkz: [API bağlama](~/cross-platform/macios/binding/objective-c-libraries.md) Aşağıdaki diyagramda gösterildiği gibi bu dosyalar bağlama işlemine nasıl uyduğunu görmek için Açıklama:

![](apidefinitions-structsandenums-images/binding-flowchart.png "Bağlama işlemi bu diyagramda gösterilmiştir.")

Başvurmak [türleri başvurusu bağlama](~/cross-platform/macios/binding/binding-types-reference.md) bu dosyaların içeriğini hakkında daha fazla bilgi için.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
