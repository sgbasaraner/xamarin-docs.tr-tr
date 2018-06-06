---
title: TvOS sekmesini Çubuğu denetleyicileri Xamarin ile çalışma
description: Bu belge, Xamarin ile oluşturulan bir tvOS uygulamasında sekmesini çubuğu denetleyicileriyle iş açıklar. Bir üst düzey sekmesini çubukları görünüm sağlar ve sekme çubuğu öğeleri, film şeridi tümleştirme ve sekme çubuğu öğeleri açıklanır.
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ea782fc8d6a2ccef2cdd687ec467be6d49793fc0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789326"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>TvOS sekmesini Çubuğu denetleyicileri Xamarin ile çalışma

Birçok tvOS uygulama türleri için birincil gezinti ekranın üst çalışan sekme çubuğunu sunulur. Kullanıcı sağa ve sola olası kategorileri ve değişiklikleri aşağıda içerik alanının kullanıcının seçimini yansıtmak için listesi arasında swipes.

[![](tab-bars-images/tab01.png "Örnek Sekme çubuğu")](tab-bars-images/tab01.png#lightbox)

Sekme çubuğunu varsayılan olarak saydam ve her zaman ekranın üstünde görünür. Odakta olduğunda, bir sekme çubuğunu ekranın üst 140 pikselleri kapsayacağı ancak odak aşağıdaki içerik alanının geçtiğinde hızla hemen slayt.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>TvOS çubuklarında sekmesi

`UITabViewController` Benzer şekilde çalışır ve iOS, aşağıdaki anahtar farklarla birlikte, yaptığı gibi benzer bir amaçla tvOS üzerinde sunar:

- Ekranın alt kısmında görüntülenen iOS sekmesini çubuğunda aksine tvOS sekmesini çubuklarında ekranın üst 140 piksel kaplar ve varsayılan olarak saydam.
- Odağı aşağıdaki içerik alanının sekme çubuğunu ayrıldığında, sekme çubuğunu ekranın üst hızla kaydırın ve gizli olamaz. Kullanıcı bir kez menü düğmesine dokunun veya yukarı doğru çekin üzerinde [Siri uzaktan](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) sekme çubuğunu yeniden göstermek için.
- Aşağı Siri uzak geçirmeyi taşınır odak sekmesini çubuğunun altında ilk içerik alanına [odaklanabilir öğesi](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) gösterildikten içerik. Yeniden odağını kaydırır sonra bu sekme çubuğunu gizler.
- Sekme çubuğunda görüntülenen bir kategori seçebilmek için için kategori içerik ve odak görünümdeki ilk odaklanabilir öğesine bırakılacak geçiş yapar.
- Sekme çubuğunda görüntülenen kategori sayısı düzeltilmesi gereken ve tüm kategorileri her zaman erişilebilir olmalıdır, belirtilen kategori hiçbir zaman devre dışı bırakılmalıdır.
- Sekme çubuklarını özelleştirme üzerinde tvOS desteklemez. Ayrıca, bunlar gösterme **daha fazla** kategorisi (örneğin, iOS) değerinden daha fazla kategori varsa sığacak sekmesini çubuğunda.

Apple sekmesini çubukları ile çalışmak için aşağıdaki önerileri vardır:

- **İçerik mantıksal olarak düzenlemek için kullanım sekmesini çubukları** -sekme çubuğunu tvOS uygulamanızı çalışır içeriği mantıksal olarak düzenlemek için kullanın. Örneğin, öne çıkan, üst grafikler, satın alınan ve arama.
- **Rozetleri bildirmek kullanıcıların, yeni içerik için ekleme** -yeni içerik bir kategorideki kullanıcı bildirmek için isteğe bağlı olarak bir gösterge (kırmızı bir beyaz sayı veya ünlem işareti olan bir oval) görüntüleyebilirsiniz.
- **Rozetleri tutumlu kullanmak** - yoksa rozetleri ile sekme çubuğunu karmaşıklığa ve yalnızca bunları burada sağladıkları kritik bilgileri kullanıcıya görüntüler.
- **Bu sayı kategorileri sınırlandırmak** - karmaşıklığını azaltmak ve uygulama yönetilebilir tutmak, yok, sekme çubuğunu kategorilerle aşırı yükleme ve kategorilerin tümünü görünür ve değil kalabalık olduğundan emin olun. Basit, kısa başlıkları en iyi çalışır.
- **Bir kategori devre dışı bırakmayın** -tüm sekmeler (kategori) her zaman görünür ve etkin her zaman. Belirli bir sekme içerik varsa, bir açıklama kullanıcıya neden sağlayın. Örneğin, satın alma sekmesi kullanıcı hiçbir satın alma işlemleri yaptıysanız boş olur.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>Sekme çubuğu öğeleri

Sekme çubuğunu her kategoride (sekmesi) Sekme çubuğu öğesi tarafından temsil edilen (`UITabBarItem`). Apple Sekme çubuğu öğeleri ile çalışmak için aşağıdaki önerileri vardır:

- **Metin tabanlı sekmeleri kullanma** -sırada Sekme çubuğu öğesi simge olarak gösterilemeyecek mümkün, Apple yalnızca kısa bir başlık simge yorumlamak daha kolay olduğundan metin kullanılmasını önerir.
- **Kısa, anlamlı isimleri veya fiilleri kullanın** -A Sekme çubuğu öğesi içerir ve basit bir isim (örneğin, fotoğraflar, filmler veya müzik) ya da (örneğin, arama veya Play) fiiller olduğunda iyi içerik açıkça geçiş.

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>Sekme çubukları ve film şeritleri

Xamarin.tvOS uygulamada sekmesini çubukları ile çalışmak için en kolay yolu, onları iOS Tasarımcısı kullanarak uygulamanın UI eklemektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
1. Yeni bir Xamarin.tvOS uygulamayı başlatın ve seçin **tvOS** > **uygulama** > **sekmeli uygulama**: 

    [![](tab-bars-images/tab02.png "Sekmeli uygulama seçin")](tab-bars-images/tab02.png#lightbox)
1. Tüm yeni bir Xamarin.tvOS çözüm oluşturmak için istemleri izleyin.
1. İçinde **çözüm paneli**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Değiştirmek için **simgesi** veya **başlık** için belirli bir kategori seçin **Sekme çubuğu öğesi** için **View Controller** içinde  **Belge Anahattı**:

    [![](tab-bars-images/tab03a.png "Sekme çubuğu öğesi için Belge Anahattı görünüm denetleyiciye")](tab-bars-images/tab03a.png#lightbox)
1. Ardından gerekli özellikleri kümesinde **pencere öğesi sekmesini** , **özellikleri Explorer**: 

    [![](tab-bars-images/tab03.png "Pencere öğesi sekmesi")](tab-bars-images/tab03.png#lightbox)
1. Yeni bir kategori (sekmesi) eklemek için doğrudan bir **View Controller** , tasarım yüzeyine: 

    [![](tab-bars-images/tab04.png "Bir görünüm denetleyicisi")](tab-bars-images/tab04.png#lightbox)
1. Denetim tıklatın ve sürükleyin **sekmesini View Controller** yeni **View Controller**.
1. Açılır penceresinden seçin **görüntülemek denetleyicileri** Yeni Görünüm sekmesinde (kategori) eklemek için: 

    [![](tab-bars-images/tab05.png "Sekmesini seçin")](tab-bars-images/tab05.png#lightbox)
1. Her bir Caterogies içerik alan normal, olarak UI düzenini iOS Tasarımcısı kullanıcı Arabirimi öğeleri ekleyerek tasarlayın.
1. C# kodunda UI denetimlerinizi çalışmak için gerekli tüm olayları gösterir.
1. C# kodunda kullanıma sunmak istediğiniz herhangi bir kullanıcı Arabirimi denetimlerini olarak adlandırın.
1. Değişikliklerinizi kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Yeni bir Xamarin.tvOS uygulamayı başlatın ve seçin **tvOS** > **uygulama** > **sekmeli uygulama**: 

    [![](tab-bars-images/tab02vs.png "Sekmeli uygulama seçin")](tab-bars-images/tab02vs.png#lightbox)
1. Tüm yeni bir Xamarin.tvOS çözüm oluşturmak için istemleri izleyin.
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Değiştirmek için **simgesi** veya **başlık** için belirli bir kategori seçin **Sekme çubuğu öğesi** için **View Controller** içinde  **Belge Anahattı**:

    [![](tab-bars-images/tab03avs.png "Belge Anahattı görünüm denetleyiciye")](tab-bars-images/tab03avs.png#lightbox)
1. Ardından gerekli özellikleri kümesinde **pencere öğesi sekmesini** , **özellikleri Explorer**: 

    [![](tab-bars-images/tab03vs.png "Pencere öğesi sekmesi")](tab-bars-images/tab03vs.png#lightbox)
1. Yeni bir kategori (sekmesi) eklemek için sürükleyin bir **View Controller** gelen **araç** ve tasarım yüzeyine bırak: 

    [![](tab-bars-images/tab04vs.png "Bir görünüm denetleyicisi")](tab-bars-images/tab04vs.png#lightbox)
1. Denetim tıklatın ve sürükleyin **sekmesini View Controller** yeni **View Controller**.
1. Açılır penceresinden seçin **görüntülemek denetleyicileri** Yeni Görünüm sekmesinde (kategori) eklemek için: 

    [![](tab-bars-images/tab05vs.png "Sekmesini seçin")](tab-bars-images/tab05vs.png#lightbox)
1. Her bir Caterogies içerik alan normal, olarak UI düzenini iOS Tasarımcısı kullanıcı Arabirimi öğeleri ekleyerek tasarlayın.
1. C# kodunda UI denetimlerinizi çalışmak için gerekli tüm olayları gösterir.
1. C# kodunda kullanıma sunmak istediğiniz herhangi bir kullanıcı Arabirimi denetimlerini olarak adlandırın.
1. Değişikliklerinizi kaydedin.
    
-----

> [!IMPORTANT]
> Olayları gibi atamak mümkün olmakla birlikte `TouchUpInside` bir kullanıcı Arabirimi öğesine (gibi bir `UIButton`) Apple TV ekranında veya dokunma olaylarını destekleyen bir touch sahip olmadığından iOS Tasarımcısı, hiçbir zaman çağrılır. Her zaman kullanmalısınız `Primary Action ` olay işleyicileri tvOS için kullanıcı arabirimi öğeleri oluştururken olay.

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>Sekme çubukları ile çalışma

Kullanım `Items` özelliği `UITabBar` koleksiyonunu erişmek için `UITabBarItems` bir sıfır (0) oluşturulmuş dizisi olarak içerir. `SelectedItem` Özelliği olarak seçili olan sekme (kategori) döndürecektir bir `UITabBarItem`.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>Sekme çubuğu öğeleri ile çalışma

Verilen sekmesinde (beyaz metinle kırmızı bir oval) bir gösterge görüntülemek için aşağıdaki kodu kullanın:

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

Hangi çalıştırdığınızda, aşağıdaki sonuçlar:

[![](tab-bars-images/tab06.png "Gösterge ile sekme çubuğu öğesi")](tab-bars-images/tab06.png#lightbox)

Kullanım `Title` özelliği `UITabBarItem` başlığını değiştirmek için ve `Image` simgeyi değiştirme özelliği.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve sekmesini çubuğu denetleyicisiyle Xamarin.tvOS uygulama içinde çalışma ele.




## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
