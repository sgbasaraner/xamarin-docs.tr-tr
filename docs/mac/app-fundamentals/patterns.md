---
title: Ortak desenleri ve deyimleri
description: "Bu belgede, model-view-controller düzeni, veri kaynağı ve temsilci desenleri ve iletişim kuralları açıklanır."
ms.topic: article
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/17/2016
ms.openlocfilehash: 4926670a0cd0960c9a390bd388d3530929634153
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="common-patterns-and-idioms"></a>Ortak desenleri ve deyimleri

C# kullanıma sunulan Apple API'leri belirli deyimleri ve desenler tekrar tekrar gündeme. Xamarin.iOS ile programlama deneyimi varsa, bunlar hakkında bilgi görünebilir. Bunları düz bir anlayış sahip bulduğunuz belgelerin anlamlı yardımcı olacak şekilde belgeleri genellikle bu desenleri ve deyimleri tekrar tekrar başvurur.

## <a name="mvc---model-view-controller"></a>MVC - Model View Controller

Model View Controller veya kısaca, MVC Cocoa bulunan çok yaygın bir düzeni değil. Ayrıntılı bir tartışma bu belgenin kapsamı dışındadır, ancak kısaca, bileşenleri uygulamanıza yapılandırma yolu:

- **Model** nesneleri temsil temel alınan veri okunduğunu görüntülenebilir ve (örneğin, bir adres defteri adresleri) yönetilebilir
- **Görünüm** nesneleri işlemek ekranında belirli nesne çizim ve kullanıcı etkileşimi (ekranda adresini gösteren bir metin alanı) işleme
- **Denetleyici** nesneleri işlemek modeli ve görünüm arasındaki etkileşim. Model değişiklikleri anında kullanıcılar kullanıcı Arabiriminde bir değişiklik yaptığınızda değişiklikleri "kapalı" görünümden gönderin ve Görünümü güncelleştirmek için "en".

(Model View ViewModel) MVVM ile WPF gibi diğer kitaplıklarından biliyorsanız, denetleyici ViewModel benzer davranır ancak genellikle belirli kullanıcı Arabirimi öğeleri daha yakından ilişkilidir.

Daha fazla bilgi şurada bulunabilir:

- [Apple'nın Web sitesinde MVC öğrenme](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller Objective-c](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Windows ile birlikte çalışma](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Veri kaynağı / temsilci / alt sınıf yapma

Kullanıcı Arabirimi öğeleri için verileri sağlayan ve Kullanıcı etkileşimlerine tepki ile başka bir yaygın deseninde Cocoa ile ilgilidir. Kullanarak `NSTableView` örnek olarak, her satır için veri şekilde vermeniz gerekir, bazı kullanıcı Arabirimi öğeleri, bazı satır temsil eden Ayarla kullanıcı etkileşimleri ve büyük olasılıkla özelleştirme miktar tepki vermek için davranışlar kümesi. Veri kaynağı ve temsilci desenleri için sınıflara çözümlemelere gerek kalmadan çoğu durumlarında olanak tanıyan `NSTableView` kendiniz.

- `DataSource` Özelliği, özel bir alt sınıfı örneği atandığında `NSTableViewDataSource` verilerle tabloyu doldurmak için adlı (aracılığıyla `GetRowCount` ve `GetObjectValue`).

- `Delegate` Özelliği, özel bir alt sınıfı örneği atandığında `NSTableViewDelegate` belirli model nesnesi için görünümü sağlar (aracılığıyla `GetViewForItem`) ve kullanıcı Arabirimi etkileşimleri işleme (aracılığıyla `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`, vb.).

Bazı durumlarda, bir denetim temsilci ya da veri kaynağında verilen kancaları ötesinde şekilde özelleştirmek istediğiniz ve alt görünümü doğrudan kurtarabilirsiniz. Ancak dikkatli olun, varsayılan geçersiz kılma birçok durumda davranışı ardından, tüm bu davranışı işlemek ihtiyaç duyacağınız (seçimi davranışını özelleştirme gerekebilir. tüm seçimi davranışları kendiniz uygulamak).

Xamarin.iOS, bazı API'ları içinde gibi `UITableView` temsilci ve veri kaynağı uygulayan bir özellik Genişletilmiş (`UITableViewSource`). Bu, tek bir C# sınıf yalnızca bir temel sınıf ve protokollerin görünmesini sağlayabilirsiniz ortak sınırlamaya geçici bir çözüm için temel sınıfları gerçekleştirilir.

Xamarin.Mac uygulamasında tablosu görünümleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Tablo görünümü](~/mac/user-interface/table-view.md) belgeleri.

## <a name="protocols"></a>protokolleri

Objective-C protokollerin C# arabirimleri karşılaştırıldığında ve çoğu durumda benzer durumlarda kullanılır. Örneğin `NSTableView` yukarıdaki örnekte, temsilci ve veri kaynağı olan gerçekte protokoller. Xamarin.Mac temel sınıflar olarak geçersiz kılabilirsiniz sanal yöntemleriyle kullanıma sunar. C# arabirimleri ve Objective-C protokoller arasındaki birincil fark bir protokol bazı yöntemleri uygulamak isteğe bağlı olmasıdır. Belgeleri ve/veya isteğe bağlı belirlemek için bir API tanımı aramak gerekir.

Daha fazla bilgi için lütfen bkz: bizim [Temsilciler, protokolleri ve olayları](~/ios/app-fundamentals/delegates-protocols-and-events.md) belgeleri.



## <a name="related-links"></a>İlgili bağlantılar

- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Windows ile birlikte çalışma](~/mac/user-interface/window.md)
- [Temsilciler, protokolleri ve olaylar](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
