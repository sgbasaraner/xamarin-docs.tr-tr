---
title: "ApiDefinitions & StructsAndEnums dosyaları"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 0dedc6f574fbe2f2bf4ffaf4e70fb972670e9a8c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums dosyaları

Hedefi Sharpie başarıyla çalıştırıldı olduğunda oluşturur `Binding/ApiDefinitions.cs` ve `Binding/StructsAndEnums.cs` dosyaları.
Bu iki dosyayı Mac için Visual Studio'da bir bağlama projesi eklenir veya doğrudan geçirilen `btouch` veya `bmac` son bağlama oluşturmak için Araçlar.

İçinde *bazı* durumlarda bu oluşturulan dosyalar olabilir tüm yapmanız gereken, ancak daha sık geliştirici bu el ile değiştirmeniz gerekecektir oluşturulan dosyaları otomatik olarak (örneğin, bu bayrak eklenmiş aracı tarafından işlenemedi sorunları gidermek için ile bir [ `Verify` özniteliği](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Sonraki adımlar bazıları şunlardır:

- **Adları ayarlama**: bazen yöntemleri ve .NET Framework tasarım yönergeleri eşleşecek şekilde sınıfları adları Ayarla isteyeceksiniz.
- **Yöntemleri veya özellikleri**: bazen hedefi Sharpie tarafından kullanılan buluşsal bir özellikte açılması için bir yöntem alacaktır. Bu noktada, bu amaçlanan bir davranış olup olmadığını karar.
- **Olayları kanca**: sınıflarınızı temsilci sınıflarıyla bağlamak ve olaylar için otomatik olarak üret.
- **Bildirimleri kanca**: bildirimler API sözleşme saf üstbilgi dosyalarını ayıklamak mümkün değildir, bu API belgelerine seyahat gerektirir. Kesin türü belirtilmiş bildirimleri istiyorsanız, sonuç güncelleştirmeniz gerekir.
- **API iyileştirmesi**: Bu noktada, tercih edebileceğiniz fazladan oluşturucular sağlamak için yöntemler (C# yapım üzerinde Initialize sözdizimi için izin vermek için), İşleç aşırı yüklemesi ve uygulama Ekle fazladan tanımları dosyada kendi arabirimleri.

Bkz: [bir API bağlama](~/cross-platform/macios/binding/objective-c-libraries.md) aşağıdaki çizimde gösterildiği gibi bu dosyaları bağlama işlemine nasıl uyduğunu görmek için Açıklama:

![](apidefinitions-structsandenums-images/binding-flowchart.png "Bağlama işlemi bu diyagramda gösterildiği")

Başvurmak [türleri başvurusu bağlama](~/cross-platform/macios/binding/binding-types-reference.md) bu dosyaların içeriği hakkında daha fazla bilgi için.
