---
title: Xamarin.Forms veri bağlama
description: Veri bağlama, bir özellik değişiklikleri otomatik olarak diğer özelliğinde yansıtılır bir bağlama iki nesnelerin özelliklerini, tekniğidir. Veri bağlama Model-View-ViewModel (MVVM) uygulama mimarisi önemli bir parçasıdır.
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2018
ms.openlocfilehash: def97ab77781c7a7156d4c4178097184614f3e8b
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "35240359"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms veri bağlama

_Veri bağlama, bir özellik değişiklikleri otomatik olarak diğer özelliğinde yansıtılır bir bağlama iki nesnelerin özelliklerini, tekniğidir. Veri bağlama Model-View-ViewModel (MVVM) uygulama mimarisi önemli bir parçasıdır._

## <a name="the-data-linking-problem"></a>Veri bağlama sorunu

Her biri genellikle içeren birden çok kullanıcı arabirimi nesneleri olarak adlandırılan bir veya daha fazla sayfaları, bir Xamarin.Forms uygulaması oluşur *görünümleri*. Program birincil görevlerini görünümlerindeki eşitlenmiş halde tutmak ve çeşitli değerleri veya temsil ettikleri seçimleri izlemek için biridir. Genellikle görünümleri değerler temel alınan bir veri kaynağından temsil eder ve bu verileri değiştirmek için bu görünümleri kullanıcı yönetir. Görünüm değiştiğinde, temel alınan verileri bu değişikliği yansıtacak gerekir ve temel alınan veriler değiştiğinde, benzer şekilde, bu değişikliği Görünümü'nde yansıtılır gerekir.

Bu işi başarılı bir şekilde işlemek için program bu görünümleri ya da temel alınan veri değişikliklerinin açıklaması bildirilmelidir. Ortak çözüm, bir değişiklik olduğunda sinyal olayları tanımlamaktır. Bir olay işleyicisi, ardından, bu değişikliklerin bildirilmesini yüklenebilir. Verileri bir nesneden diğerine aktarılmasını tarafından yanıt verir. Ancak, çok sayıda görünümleri olduğunda, ayrıca birçok olay işleyicisi de olmalıdır ve bir sürü kod söz konusu.

## <a name="the-data-binding-solution"></a>Veri bağlama çözümü

Veri bağlama, bu işlemi otomatik hale getirir ve olay işleyicileri gereksiz işler. (Veri bağlama altyapısı bunları kullandığından olayları ancak hala gerekli değildir.) Veri bağlamaları kodda veya XAML uygulanabilir, ancak bunlar burada, arka plan kod dosyasının boyutunu azaltmak için Yardım XAML içinde çok daha yaygındır. Olay işleyicileri yordam kodunda bildirim temelli bir kod veya biçimlendirme ile değiştirerek, uygulama Basitleştirilmiş açıklığa kavuşturuldu ve.

Bir veri bağlamaya dahil edilen iki nesne biri neredeyse her zaman türetilen bir öğe `View` ve görsel arabirim sayfasının bir parçasını oluşturur. Diğer nesne ya da verilmiştir:

- Başka bir `View` türetme genellikle aynı sayfa üzerinde.
- Bir kod dosyasında bir nesne.

Tanıtım programlarda olanlar gibi [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) örnek, ikisi arasında veri bağlamaları `View` türevleri genellikle açıklık ve kolaylık olması amacıyla gösterilir. Ancak, aynı ilkeler arasında veri bağlamaları uygulanabilir bir `View` ve diğer nesneleri. Model-View-ViewModel (MVVM) mimarisini kullanarak uygulama oluşturulduğunda, temel alınan veri sınıfıyla genellikle bir ViewModel olarak adlandırılır.

Veri bağlamaları aşağıdaki dizi makale incelenmektedir:

## <a name="basic-bindingsbasic-bindingsmd"></a>[Temel Bağlamalar](basic-bindings.md)

Kod ve XAML basit veri bağlamaları görmek ve veri bağlama hedef ve kaynak arasındaki farkı öğrenin.

## <a name="binding-modebinding-modemd"></a>[Bağlama Modu](binding-mode.md)

Bağlama modu iki nesne arasındaki veri akışını nasıl kontrol edebilir keşfedin.

## <a name="string-formattingstring-formattingmd"></a>[Dize Biçimlendirmesi](string-formatting.md)

Veri bağlama biçimlendirmek ve dizeleri nesneleri görüntülemek için kullanın.

## <a name="binding-pathbinding-pathmd"></a>[Bağlama Yolu](binding-path.md)

Yakından bakış ayrıntılı incelemesi `Path` alt özellikleri ve koleksiyon üyelerine erişmek için veri bağlama özelliği.

## <a name="binding-value-convertersconvertersmd"></a>[Bağlama Değeri Dönüştürücüleri](converters.md)

Bağlama değeri dönüştürücüleri içinde veri bağlamayı değerleri değiştirmek için kullanın.

## <a name="binding-fallbacksbinding-fallbacksmd"></a>[Bağlama geri dönüşler](binding-fallbacks.md)

Veri bağlamaları daha sağlam bağlama işlemi başarısız olursa kullanmak için geri dönüş değerleri tanımlayarak olun.

## <a name="the-command-interfacecommandingmd"></a>[Komut Arabirimi](commanding.md)

Uygulama `Command` veri bağlamaları ile özelliği.

## <a name="compiled-bindingscompiled-bindingsmd"></a>[Derlenmiş bağlamaları](compiled-bindings.md)

Veri bağlama performansını artırmak için derlenmiş bağlamaları kullanın.

## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölümden Xamarin.Forms kitabı](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/markup-extensions/index.md)
