---
title: Merhaba, iOS Multiscreen – derinlemesine bakış
description: Bu belgede daha derinlikli bir bakış genişletilmiş Phoneword uygulama, diğer model-view-controller dikkate alarak, iOS gezinti ve diğer iOS geliştirme kavramları alır.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: cdeea6d78ec1262a0b5b613b4f483012c9df2c19
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785664"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Merhaba, iOS Multiscreen – derinlemesine bakış

Hızlı Başlangıç Kılavuzu içinde oluşturulan ve ilk çok ekran Xamarin.iOS uygulamamız çalıştırılmıştır. Artık iOS gezinti ve mimari daha derin bir anlayış geliştirmek için zaman yapılır.

Bu kılavuzda biz tanıtmak *Model, görünüm, denetleyici (MVC)* düzeni ve iOS mimarisi ve gezinti rolü.
Daha sonra Gezinti Denetleyicisi'nde daha yakından inceleyin ve bir iOS tanıdık gezinme deneyimi sağlamak için kullanmak üzere bilgi edinin.

<a name="Model_View_Controller" />

## <a name="model-view-controller-mvc"></a>Model View Controller (MVC)

İçinde [Hello, iOS](~/ios/get-started/hello-ios/index.md) öğretici, biz öğrenilen iOS uygulamaları tek sahip *penceresi* görünüm denetleyicileri yükleme sorumlu olan kendi *içerik görünümü hiyerarşileri* içine Pencere. İkinci Phoneword örneklerde, uygulamamız için ikinci bir ekran eklendi ve aşağıdaki diyagramda gösterildiği gibi iki ekranda arasında bazı veriler – telefon numarası listesinin – geçirilen:

 [![](hello-ios-multiscreen-deepdive-images/08.png "Bu diyagramda iki ekranda arasında veri geçirme gösterilir")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

Bizim örneğimizde, ikinci ilk görünüm denetleyicisinden geçirilen ve ikinci ekranı tarafından görüntülenen ilk ekranında veri toplanmıştır. Bu ayrım ekranlar, görünüm denetleyicileri ve veri izleyen *Model, görünüm, denetleyici (MVC)* düzeni. Sonraki birkaç bölümlerde deseni, bileşenleri ve Phoneword uygulamamız kullanırız nasıl avantajları tartışın.

### <a name="benefits-of-the-mvc-pattern"></a>MVC örüntüsü yararları

Model-View-Controller olan bir *tasarım deseni* – kodda ortak bir sorun veya kullanım örneği yeniden kullanılabilir bir mimari çözüme. MVC uygulamaları için bir mimari olan bir *grafik kullanıcı arabirimi (GUI)*. Uygulama üç rollerini - nesneleri atar *modeli* (veri veya uygulama mantığını) *Görünüm* (kullanıcı arabirimi) ve *denetleyicisi* (arkasında kodu). Aşağıdaki diyagram, MVC örüntüsü üç parça ve kullanıcı arasındaki ilişkileri gösterir:

 [![](hello-ios-multiscreen-deepdive-images/00.png "Bu diyagram MVC örüntüsü üç parça ve kullanıcı arasındaki ilişkileri gösterir")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

MVC örüntüsü, bir GUI uygulamasının farklı bölümleri arasında mantıksal ayrım sağlar ve bize kod ve görünümlerde yeniden kolaylaştırır için yararlıdır. Şimdi hemen ve üç rollerinin her biri daha ayrıntılı olarak bakalım.

> [!NOTE]
> MVC örüntüsü, geniş ASP.NET sayfaları ve WPF uygulamaları yapısına benzerdir. Bu örneklerde, görünüm, gerçekte kullanıcı arabirimini açıklamak için sorumludur ve ASP.NET ASPX (HTML) sayfası veya bir WPF uygulamasında XAML karşılık gelen bileşenidir. Arka plan kod ASP.NET veya WPF karşılık gelen görünümü yönetmekten sorumlu bileşeni denetleyicisidir.


### <a name="model"></a>Model

Model nesnesi genellikle bir uygulamaya özgü görüntülenmesine veya görünüme girilen verileri gösterimidir. Model genellikle geniş - Örneğin, tanımlanan bizim **Phoneword_iOS** uygulama, telefon numaralarını (dizelerinin listesini gösterilir) listesi olan Model. Platformlar arası uygulama de oluşturuyorsanız, biz paylaşmak seçebilir **PhonewordTranslator** , iOS ve Android uygulamalar arasında kodu. Biz bu paylaşılan kodu Model olarak düşünün.

MVC, tamamen belirsiz *veri kalıcılığını* ve *erişim* modeli. Nasıl verilerin yalnızca diğer bir deyişle, MVC bizim veri görülüyor veya nasıl depolanacağını, önemli değil *temsil*. Örneğin, biz yalnızca kullanmak verilerimizi bir SQL veritabanında depolamak veya bazı bulut depolama mekanizmasını kalıcı tercih edebilirsiniz bir `List<string>`. MVC amacıyla, yalnızca veri temsili kendisini düzende dahil edilir.

Bazı durumlarda, MVC Model kısmı boş olabilir. Örneğin, biz Telefon Çeviricisi nasıl çalıştığını, oluşturduğumuz neden ve nasıl bizimle iletişime rapor hataları alınacağı açıklayan uygulamamıza bazı statik sayfaları eklemek isteyebilirsiniz. Bu uygulama ekranlar hala görünümler ve denetleyiciler kullanılarak oluşturulması, ancak herhangi bir gerçek Model veri bulunmaz.

> [!NOTE]
> Bazı belgeleri, MVC örüntüsü modeli kısmı tüm uygulama arka uç, yalnızca kullanıcı Arabirimi üzerindeki görüntülenen verileri başvurabilirsiniz. Model modern yorumu kullandığımız bu kılavuzda, ancak bir ayrım özellikle önemli değildir.


### <a name="view"></a>Görüntüle

Bir görünümü, kullanıcı arabirimi işlenmesinden sorumludur bileşenidir. MVC modelini kullanan neredeyse tüm platformlarda, kullanıcı arabirimi görünümleri hiyerarşisini oluşur. Biz MVC görünümünde kök görünümü - hiyerarşideki üst ve alt görünümlerinin herhangi bir sayı olarak bilinen tek bir görünüm – view hiyerarşisiyle olarak düşünebilirsiniz (olarak bilinen veya subviews) altındaki. İOS, ekran içerik görünümü hiyerarşi MVC görünümü bileşeni karşılık gelir.

### <a name="controller"></a>Denetleyici

Denetleyici nesnesinin her şeyi birbirine bağlayan ve iOS tarafından temsil edilen bir bileşendir `UIViewController`. Bir ekran veya Görünüm kümesi için yedekleme kodu olarak biz denetleyicisini düşünebilirsiniz. Denetleyici, kullanıcı isteklerini dinleyen ve uygun görünümü hiyerarşiyi döndürüyor sorumludur. (Düğme tıklamaları, metin girişi, vb.) görünümünden isteklerini dinleyen ve uygun görünüm değiştirme işlemleri ve görünümünü yeniden yükleme gerçekleştirir. Denetleyici oluşturma veya veri deposu uygulamada var ne olursa olsun yedekleme modeli alınırken ve görünümü verileri ile doldurmak için de sorumludur.

Denetleyicileri ayrıca diğer denetleyicileri yönetebilirsiniz. Örneğin, farklı bir ekranı veya sıralarına ve bunları arasındaki geçişler izlemek için denetleyicilerinin yığınını yönetmek gerekiyorsa bir denetleyicisi başka bir denetleyici yük. Sonraki bölümde, biz özel türde bir görünüm denetleyicisini adlı iOS tanıtmak gibi diğer denetleyicilerini yöneten bir denetleyici örneği göreceğiz bir *Gezinti denetleyicisi*.

## <a name="navigation-controller"></a>Gezinti denetleyicisi

Phoneword uygulamada kullandık bir *Gezinti denetleyicisi* birden çok ekranları arasında gezinmeyi yönetmenize yardımcı olmak için. Gezinti denetleyicisidir özelleştirilmiş `UIViewController` tarafından temsil edilen `UINavigationController` sınıfı. Tek bir içerik görünümü hiyerarşi yönetmek yerine, kendi özel içerik görünümü hiyerarşisinde bir başlık, geri düğmesini ve diğer isteğe bağlı özellikler içeren bir gezinme araç form yanı sıra diğer görünüm denetleyicileri Gezinti denetleyicisi yönetir.

Gezinti denetleyicisi iOS uygulamalarında yaygın bir durumdur ve gezinti gibi Zımba iOS uygulamaları için sağlar **ayarları** aşağıdaki ekran görüntüsüne gösterildiği gibi uygulama:

 [![](hello-ios-multiscreen-deepdive-images/01.png "Burada gösterilen ayarları uygulaması gibi iOS uygulamaları için Gezinti Gezinti denetleyici sağlar")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Üç birincil işlev Gezinti denetleyicisi sunar:

-  **İleri gezinti için kancalar sağlar** – Gezinti denetleyicisi içerik görünümü hiyerarşileri nerede hiyerarşik Gezinti benzetimini kullanır *gönderilen* üzerine bir *Gezinti yığını* . Yalnızca en üstteki çoğu kartı Aşağıdaki diyagramda gösterildiği gibi görünür, olduğu, oyun kart yığınını olarak bir gezinti yığını düşünebilirsiniz:  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Bu diyagramda Gezinti kart yığınını olarak gösterilir")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **İsteğe bağlı olarak bir geri düğmesini sağlar** - biz Gezinti yığına yeni bir öğe bastığınızda başlık çubuğunu otomatik olarak görüntüleyebilirsiniz bir *geri düğmesini* geriye doğru gidin kullanıcıya izin verir. Geri düğmesine basarak *POP* geçerli görünümü denetleyicisi Gezinti yığını ve yükleri önceki içerik görünümü hiyerarşiye pencereyi kapat:  

    [![](hello-ios-multiscreen-deepdive-images/03.png "Bu diyagramda 'yığından bir kart pencerelerinin' gösterilir")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Başlık çubuğu sağlar** – üst kısmını **Gezinti denetleyicisi** çağrılır *başlık çubuğu* . Bunu, aşağıdaki diyagramda gösterildiği gibi View Controller başlık görüntülemede sorumlu olan:  

    [![](hello-ios-multiscreen-deepdive-images/04.png "Başlık çubuğu View Controller başlık görüntülemek için sorumludur")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)




### <a name="root-view-controller"></a>Kök görünümü denetleyicisi

A **Gezinti denetleyicisi** , kendi görüntülenecek bir şey sahiptir içerik görünümü hiyerarşi yönetmeyen.
Bunun yerine, bir **Gezinti denetleyicisi** ile eşleştirilmiş bir *kök View Controller*:

 [![](hello-ios-multiscreen-deepdive-images/05.png "Bir gezinme denetleyicisi kök View Controller ile eşleştirilmiş")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Kök View Controller ilk görünüm denetleyicisini temsil eden **Gezinti denetleyicinin** yığını ve kök görünümü denetleyicinin içerik görünümü hiyerarşisi olan ilk içerik penceresine yüklenmesi hiyerarşisini görüntüleme. Biz Gezinti denetleyicinin yığında tüm uygulamamız getirmek isterseniz, biz Sourceless ü taşıyabilirsiniz **Gezinti denetleyicisi** ve biz olduğu gibi ilk ekran View Controller kök View Controller ayarlayın Phoneword uygulama:

 [![](hello-ios-multiscreen-deepdive-images/06.png "Sourceless ü ilk ekranlar View Controller kök View Controller ayarlar")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Ek Gezinti Seçenekleri

**Gezinti denetleyicisi** iOS Gezinti işleme, genel bir yoludur, ancak yalnızca seçenek değildir. A [sekmesini çubuğu denetleyicisi](~/ios/user-interface/controls/creating-tabbed-applications.md) farklı işlevsel; bir uygulamaya bölebilirsiniz bir [bölme görünüm denetleyicisini](https://developer.xamarin.com/recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) ana/ayrıntı görünümler; oluşturabilirsiniz ve [çıkma Gezinti denetleyicisi](http://components.xamarin.com/view/flyoutnavigation) kullanıcı içinde doğru çekin Gezinti taraftan oluşturur. Bunların tümü ile birleştirilebilir bir **Gezinti denetleyicisi** içerik sunmak sezgisel bir yol.

## <a name="handling-transitions"></a>İşleme geçişleri

Phoneword kılavuzda biz iki farklı yolla – iki görünüm denetleyicileri arasında geçiş ilk film şeridi ü ile işlenen ve ardından program aracılığıyla. Her iki bu seçenekleri daha ayrıntılı inceleyelim.

### <a name="prepareforsegue"></a>PrepareForSegue

Biz Segue ile eklediğinizde bir **Göster** film şeridi eyleme, biz istemeniz iOS ikinci Gezinti denetleyicinin yığın görünümü denetleyicide göndermek için:

 [![](hello-ios-multiscreen-deepdive-images/09.png "Açılır listeden ayar segue türü")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Film şeridi için bir Segue ekleme ekranları arasında basit bir geçiş oluşturmak için yeterlidir. Biz görünüm denetleyicileri arasında veri geçirmek istiyorsanız, geçersiz kılmak sahibiz `PrepareForSegue` yöntemi ve verileri kendisini işler:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

iOS çağrıları `PrepareForSegue` geçiş oluşur ve yönteme film şeridi oluşturduğumuz Segue geçirmeden önce sağ.
Bu noktada, biz Segue'nin hedef View Controller el ile ayarlamanız gerekir. Aşağıdaki kod, hedef görünümü denetleyicisi için bir tanıtıcı alır ve uygun sınıfta - CallHistoryController, bu durumda çevirir:

```csharp
CallHistoryController callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Son olarak, biz telefon numarası (modeli) listesinin geçirmek `ViewController` için `CallHistoryController` ayarlayarak `PhoneHistory` özelliği `CallHistoryController` çevrilen telefon numarası listesinin:

```csharp
callHistoryContoller.PhoneNumbers = PhoneNumbers;
```

Bir Segue kullanarak veri geçirme için tam kod aşağıdaki gibidir:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

    if (callHistoryContoller != null) {
         callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
 }
```

### <a name="navigation-without-segues"></a>Gezinti olmadan Segues

İkinci kodu ilk görünüm denetleyicisinden koddan bir Segue ile aynı süreci, ancak çeşitli adımları el ile yapılması sahip.
İlk olarak, kullandığımız `this.NavigationController` bir başvuru Gezinti denetleyicisi, yığın almak için şu anda açık duyuyoruz. Sonra Gezinti denetleyicinin kullanırız `PushViewController` görünüm denetleyicisini ve geçiş animasyon seçeneği geçirme yığına sonraki görünüm Denetleyicisi'ni el ile göndermeyi yöntemi (Bu ayarlar `true`).

Aşağıdaki kod, arama geçmişi ekran Phoneword ekranından geçişi gerçekleştirir:

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Sonraki görünüm denetleyiciye geçiş yapabileceğini önce çağırarak el ile film şeridi görünümünden örneklemek sahibiz `this.Storyboard.InstantiateViewController` ve içinde film şeridi Kimliğini geçirme `CallHistoryController`:

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Son olarak, biz telefon numarası (modeli) listesinin geçirmek `ViewController` için `CallHistoryController` ayarlayarak `PhoneHistory` özelliği `CallHistoryController` biz geçişi bir Segue ile işlenen zaman komutlarında yaptığımız gibi çevrilen telefon numarası listesinin:

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Programsal geçişi için tam kod aşağıdaki gibidir:

```csharp
CallHistoryButton.TouchUpInside += (object sender, EventArgs e) => {
    // Launches a new instance of CallHistoryController
    CallHistoryController callHistory = this.Storyboard.InstantiateViewController ("CallHistoryController") as CallHistoryController;
    if (callHistory != null) {
     callHistory.PhoneNumbers = PhoneNumbers;
     this.NavigationController.PushViewController (callHistory, true);
    }
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramlar

Phoneword uygulama bu kılavuzda ele alınmamaktadır birkaç kavramları sundu. Bu kavramları şunlardır:

-  **Otomatik oluşturma, görünüm denetleyicileri** – biz görünümü denetleyici için bir sınıf adı girdiğinizde **özellikleri paneli** , o sınıfın var ve görünüm sınıfı bize yedekleme denetleyicisi oluşturur, iOS Tasarımcısı'nı denetler. Bu ve diğer iOS Tasarımcısı özellikleri hakkında daha fazla bilgi için bkz [Tasarımcısı iOS giriş](~/ios/user-interface/designer/introduction.md) Kılavuzu.
-  **Tablo View Controller** – `CallHistoryController` bir tablo görünümü denetleyicisidir. Bir tablo görünümü denetleyicisi içeren bir tablo görünümü, en yaygın düzeni ve verileri aracı iOS görüntüler. Bu kılavuzun kapsamı dışındadır tablolardır. Tablo görünümü denetleyicileri hakkında daha fazla bilgi için lütfen [tabloları ve hücreleriyle çalışma](~/ios/user-interface/controls/tables/index.md) Kılavuzu.
-   **Film şeridi kimliği** – film şeridi kimliği ayarı oluşturur görünümü denetleyici sınıfını Objective-C arka plan koduna film şeridi görünüm denetleyiciye içeren. Film şeridi kimliği Objective-C sınıfı bulmak ve film şeridi görünüm denetleyiciye örneği için kullanırız. Film şeridi kimlikleri hakkında daha fazla bilgi için lütfen [film şeritleri giriş](~/ios/user-interface/storyboards/index.md) Kılavuzu.


## <a name="summary"></a>Özet

Tebrikler, ilk çok ekran iOS uygulamanızı tamamladınız.

Bu kılavuzdaki MVC örüntüsü sunulan ve çok denetlenen bir uygulama oluşturmak için kullanılır. Biz de gezinti denetleyicileri ve iOS Gezinti başlatırken kendi rol incelediniz. Artık kendi Xamarin.iOS uygulamaları geliştirmeye başlamak için gereken zemine sahipsiniz.

Ardından, Xamarin ile platformlar arası uygulamaları oluşturmak öğrenelim [mobil geliştirme giriş](~/cross-platform/get-started/introduction-to-mobile-development.md) ve [platformlar arası uygulamalar oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) kılavuzları.


## <a name="related-links"></a>İlgili bağlantılar

- [Merhaba, iOS (örnek)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS İnsan Arabirimi yönergelerine](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS sağlama portalı](https://developer.apple.com/ios/manage/overview/index.action)
