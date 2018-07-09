---
title: Objective-C'yi bağlama
description: Bu belge, geliştiricilerin Xamarin uygulamaları kullanıma hazır kitaplıkları kullanmak C# bağlamaları Objective-C kodu oluşturmayı anlatan çeşitli kılavuzlara bağlantılar sağlar.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855162"
---
# <a name="binding-objective-c"></a>Objective-C'yi bağlama

Bu bölüm, Xamarin.iOS veya Xamarin.Mac ile oluşturulan C# uygulamalardan çağrılabilen böylece oluşturma Objective-C kitaplıklarını bağlama kapsayan belgeler çeşitli içerir.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Genel bakış](~/cross-platform/macios/binding/overview.md)

Bu belge, bazı nasıl bağlama gerçekleşmeden, iç işlevleri içerir. Bazı teknik bilgiler ile gelişmiş bir belgedir.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C Kitaplıklarını Bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)

Bu belgede, C# bağlamaları Objective-C API'leri ve. NET'te kullanılan deyimleri için Objective-c deyimleri nasıl eşleneceğini oluşturmak için kullanılan işlem açıklanır.
Yalnızca C API'lerini bağlanıyorsanız, bu, P/Invoke framework standart .NET mekanizmasını kullanmanız gerekir.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Bağlama tanımı Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md)

Tüm bağlama oluşturma işlemi sürücü bağlama yazarları için kullanılabilir özniteliklerini açıklayan Başvuru Kılavuzu budur.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Amaç Sharpie bağlama ilk geçişinde bootstrap yardımcı olmak için bir komut satırı aracıdır. Genel API uygulamasına eşlemek için yerel bir kitaplığı üstbilgi dosyaları ayrıştırarak çalıştığını [tanımı bağlama](~/cross-platform/macios/binding/objective-c-libraries.md) (el ile de yapılabilir bir işlem).

## <a name="ios"></a>iOS

[İOS bağlama sayfasını](~/ios/platform/binding-objective-c/index.md) bağlantıları geri ortak bu bağlama kaynaklarına ek olarak aşağıdaki örneklere.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[İzlenecek yol: bir Objective-C kitaplığını bağlama](~/ios/platform/binding-objective-c/walkthrough.md)

Bu makalede, açık kaynak kullanarak bir bağlama projesi oluşturma adım adım kılavuz sağlanır [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) örnek olarak Objective-C projesi. Renk seçim daha kullanıcı dostu yaparak HSB gösterimine alarak bir rengi seçmesini sağlayan bir yeniden kullanılabilir görünüm denetleyicisi InfColorPicker kitaplık sağlar. Amaç Sharpie bağlama işleminde yardımcı olmak için kullanılır.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Bağlama örnekleri](https://github.com/mono/monotouch-bindings)

Olabilen üçüncü taraf bağlamaları koleksiyonunu yeni bağlama projeleri oluştururken bir başvuru kullanılır.

## <a name="mac"></a>Mac

Tarihsel olarak [Mac bağlama](~/mac/platform/binding.md) çok el ile bir süreç olmuştur. Şu anda bir [indirilebilir Önizleme](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) Mac için Visual Studio'nun sonraki bir sürümü için Mac bağlama proje desteği



## <a name="related-links"></a>İlgili bağlantılar

- [iOS bağlama](~/ios/platform/binding-objective-c/index.md)
- [Mac bağlama](~/mac/platform/binding.md)
- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
