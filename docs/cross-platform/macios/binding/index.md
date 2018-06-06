---
title: Objective-C bağlama
description: Bu belge, geliştiricilerin piyasada kitaplıklarında Xamarin uygulamaları kullanmak C# bağlamalar Objective-C kodunu oluşturmak nasıl tanımlayan çeşitli kılavuzlara bağlantılar sağlar.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 55c28387424dd7397280ffa255d94a950618d5ab
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781013"
---
# <a name="binding-objective-c"></a>Objective-C bağlama

Bu bölümde uygulamalardan Xamarin.iOS veya Xamarin.Mac ile oluşturulan C# çağrılabilir böylece Objective-C kitaplıklara oluşturma bağlamaları kapsayan belgeler, çeşitli içerir.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Genel bakış](~/cross-platform/macios/binding/overview.md)

Bu belgede bazı nasıl bağlama gerçekleşmeden, dahili bileşenleri içerir. Bazı teknik bilgilerle Gelişmiş bir belgedir.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C Kitaplıklarını Bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)

Bu belgede, C# Objective-C API'lerini ve Objective C'de deyimleri .NET içinde kullanılan deyimleri için nasıl eşlendiğini bağlamalarını oluşturmak için kullanılan işlem açıklanır.
Yalnızca C API'lerini bağlanıyorsanız, bunun için P/Invoke framework standart .NET mekanizmasını kullanmanız gerekir.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Bağlama tanımı Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md)

Tüm bağlama oluşturma işlemi sürücü bağlama yazarları için kullanılabilir özniteliklerini açıklar Başvuru Kılavuzu budur.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Amaç Sharpie bağlaması ilk geçişi bootstrap yardımcı olmak için bir komut satırı aracıdır. Genel API uygulamasına eşlemek için yerel kitaplık üstbilgi dosyaları ayrıştırarak çalışır [tanımı bağlama](~/cross-platform/macios/binding/objective-c-libraries.md) (da el ile yapılabilir bir işlem).

## <a name="ios"></a>iOS

[İOS bağlama sayfa](~/ios/platform/binding-objective-c/index.md) Bağlantılar geri bu ortak bağlama kaynaklara ek olarak aşağıdaki örneklere.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[İzlenecek yol: bir Objective-C Kitaplığı bağlama](~/ios/platform/binding-objective-c/walkthrough.md)

Bu makalede açık kaynak kullanarak bir bağlama proje oluşturma bir adım adım kılavuz sağlar [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C projesi bir örnek olarak. InfColorPicker kitaplığı, kullanıcının kendi HSB gösterimi üzerinde renk seçim yaparak daha kullanıcı dostu dayalı olarak renk seçmesine izin yeniden kullanılabilir görünüm denetleyicisi sağlar. Amaç Sharpie bağlama işleminde yardımcı olmak için kullanılır.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Bağlama örnekleri](https://github.com/mono/monotouch-bindings)

Olabilecek üçüncü taraf bağlama koleksiyonuna bir başvuru yeni bağlama projeleri oluşturulurken kullanılır.

## <a name="mac"></a>Mac

Geçmişte [Mac bağlama](~/mac/platform/binding.md) çok manuel bir işlem denendi. Şimdilik bir [indirilebilir Önizleme](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) Mac için Visual Studio sonraki bir sürümü için Mac Binding projesini desteği



## <a name="related-links"></a>İlgili bağlantılar

- [iOS bağlama](~/ios/platform/binding-objective-c/index.md)
- [Mac bağlama](~/mac/platform/binding.md)
