---
title: Xamarin.Forms veri bağlama
description: Veri bağlama bir özelliğindeki değişiklikler otomatik olarak diğer özelliğinde yansıtılır bir iki nesnelerin özelliklerini bağlama, tekniğidir. Veri bağlama modelini görünümü ViewModel (MVVM) uygulama mimarisi ayrılmaz bir parçasıdır.
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a5ea5dcb5b108da52634f131fd36a91ba82f7da4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240359"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms veri bağlama

_Veri bağlama bir özelliğindeki değişiklikler otomatik olarak diğer özelliğinde yansıtılır bir iki nesnelerin özelliklerini bağlama, tekniğidir. Veri bağlama modelini görünümü ViewModel (MVVM) uygulama mimarisi ayrılmaz bir parçasıdır._

## <a name="the-data-linking-problem"></a>Veri bağlama sorunu

Her biri genellikle içeren birden çok kullanıcı arabirimi nesneleri olarak adlandırılan bir veya daha fazla sayfaları, bir Xamarin.Forms uygulaması oluşur *görünümleri*. Program, birincil görevleri görünümlerindeki eşitlenmiş halde tutmak ve çeşitli değerleri veya temsil ettikleri seçimleri izlemek için biridir. Genellikle görünümleri değerleri temel alınan bir veri kaynağından temsil eder ve bu verileri değiştirmek için bu görünümleri kullanıcı yönetir. Görünüm değiştiğinde, temel alınan veri bu değişikliği yansıtacak gerekir ve temel alınan veriler değiştiğinde, benzer şekilde, bu değişikliği görünümünde yansıtılması gerekir.

Bu işi başarılı bir şekilde işlemek için program bu görünümlere veya temel alınan veri değişiklikleri bildirilmelidir. Ortak bir değişiklik olduğunda sinyal olayları tanımlamak için çözümüdür. Olay işleyici, daha sonra bu değişiklikleri bildirimde yüklenebilir. Verileri bir nesneden diğerine aktararak yanıt verir. Ancak, çok sayıda görünümleri olduğunda, birçok olay işleyicileri bulunmalıdır ve çok fazla kod söz konusu.

## <a name="the-data-binding-solution"></a>Veri bağlama çözümü

Veri bağlama, bu iş otomatikleştirir ve olay işleyicileri gereksiz işler. (Veri bağlama altyapısı bunları kullandığından ancak hala gerekli olaylardır.) Veri bağlamaları kod veya XAML uygulanabilir, ancak burada arka plan kod dosyasının boyutunu azaltmak için yardımcı olurlar XAML'de çok daha yaygın. Olay işleyicileri yordam kodda bildirimsel kod veya işaretleme ile değiştirerek uygulamaya Basitleştirilmiş açıklığa kavuşturuldu ve.

Bir veri bağlamanın iki nesneler biri neredeyse her zaman türeyen bir öğe `View` ve forms görsel arabirimi bir sayfanın parçası. Diğer nesnesi kullanılıyor:

- Başka bir `View` genellikle aynı sayfada türetilmiş.
- Kod dosyasında bir nesne.

Tanıtım programlarda olanlar gibi [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) örnek, iki arasında veri bağlamaları `View` türevleri daha anlaşılır olması ve kolaylık amacıyla genellikle gösterilir. Ancak, aynı ilkeler arasında veri bağlamaları uygulanabilir bir `View` ve diğer nesneleri. Uygulamanın Model View ViewModel (MVVM) mimarisi kullanarak yapılandırıldığında, temel alınan veri sınıfıyla çoğunlukla bir ViewModel adı verilir.

Veri bağlamaları makaleleri aşağıdaki serisinde incelediniz:

## <a name="basic-bindingsbasic-bindingsmd"></a>[Temel Bağlamalar](basic-bindings.md)

Veri bağlama hedef ve kaynak arasındaki farkı öğrenin ve kod ve XAML basit veri bağlama bakın.

## <a name="binding-modebinding-modemd"></a>[Bağlama Modu](binding-mode.md)

Bağlama modunu iki nesne arasındaki veri akışını nasıl kontrol edebilir bulur.

## <a name="string-formattingstring-formattingmd"></a>[Dize Biçimlendirmesi](string-formatting.md)

Veri bağlama biçimlendirmek ve dize olarak nesneleri görüntülemek için kullanın.

## <a name="binding-pathbinding-pathmd"></a>[Bağlama Yolu](binding-path.md)

İçine inebilirsiniz `Path` alt özellikleri ve koleksiyon üyeleri erişmek için veri bağlama özelliği.

## <a name="binding-value-convertersconvertersmd"></a>[Bağlama Değeri Dönüştürücüleri](converters.md)

Veri bağlama içindeki değerleri değiştirmek için bağlama değer dönüştürücüler kullanın.

## <a name="the-command-interfacecommandingmd"></a>[Komut Arabirimi](commanding.md)

Uygulama `Command` veri bağlamaları özelliğiyle.



## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/markup-extensions/index.md)
