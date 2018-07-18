---
title: Hello, iOS çok ekranlı – derinlemesine bakış
description: Bu belge, genişletilmiş Phoneword uygulama, model-view-controller dikkate yapılacaktır, iOS gezinti ve diğer iOS geliştirme kavramları daha derinlikli bir bakış alır.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: eaf77dd68895a3fbf677e1d0aa68125d81d709c1
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111231"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Hello, iOS çok ekranlı – derinlemesine bakış

Hızlı Başlangıç Kılavuzu'nda yerleşik olarak ve ilk çoklu ekran Xamarin.iOS uygulamamız çalıştı. Artık iOS gezinti ve mimari daha derin bir anlayış geliştirmek için zamanı geldi.

Bu kılavuzda biz tanıtmak *Model, görünüm denetleyicisi (MVC)* deseni ve iOS mimarisi ve gezinti rolü.
Daha sonra Gezinti denetleyicisi ile inceleyin ve bir iOS tanıdık Gezinti deneyimi sağlamak için kullanmayı öğrenin.

<a name="Model_View_Controller" />

## <a name="model-view-controller-mvc"></a>Model-View-Controller (MVC)

İçinde [Hello, iOS](~/ios/get-started/hello-ios/index.md) öğreticide edindiğimiz iOS uygulamaları yalnızca biri olduğunu *penceresi* görünüm denetleyicisi yükleme sorumlu olan kendi *içerik görünümü hiyerarşileri* içine Pencere. İkinci Phoneword izlenecek yolda, uygulamamız için ikinci bir ekran eklenir ve aşağıdaki diyagramda gösterildiği gibi iki ekran arasındaki bazı veriler – telefon numaralarının listesini – geçirildi:

 [![](hello-ios-multiscreen-deepdive-images/08.png "Bu diyagramda iki ekranlar arasında veri geçirme gösterilir.")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

Bizim örneğimizde, ikinci ilk görünüm denetleyicisi geçirilen ve ikinci ekran tarafından görüntülenen ilk ekranda veri toplanamadı. Ekranlar, görünüm denetleyicilerini ve veri ayrımı izleyen *Model, görünüm denetleyicisi (MVC)* deseni. Sonraki birkaç bölümde, deseni, bileşenlerinin ve Phoneword uygulamamız kullandığımız nasıl ele alır.

### <a name="benefits-of-the-mvc-pattern"></a>MVC düzenin avantajları

Model-View-Controller olduğu bir *tasarım deseni* – kodundaki yaygın bir sorun veya kullanım örneği için yeniden kullanılabilir bir mimari çözümü. MVC uygulamaları için bir mimari olduğunu bir *grafik kullanıcı arabirimi (GUI)*. Uygulamadaki üç rollerden - nesne atar *modeli* (mantıksal), verileri veya uygulaması *görünümü* (kullanıcı arabirimi) ve *denetleyicisi* (arkasındaki kodu). Aşağıdaki diyagramda, MVC örüntüsü, üç parça ve kullanıcı arasındaki ilişkiler gösterilmektedir:

 [![](hello-ios-multiscreen-deepdive-images/00.png "Bu diyagram, MVC örüntüsü, üç parça ve kullanıcı arasındaki ilişkileri gösterir")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

MVC örüntüsü yararlıdır, çünkü kod ve görünümleri yeniden kullanmak ABD kolaylaştırır ve GUI uygulamanın farklı kısımlarını arasında mantıksal ayrım sağlar. Şimdi hemen ve her biri üç rol daha ayrıntılı olarak bakalım.

> [!NOTE]
> MVC örüntüsü, gevşek ASP.NET sayfaları ya da WPF uygulamaları yapısına benzer. Bu örneklerde, UI açıklamak için gerçekten sorumludur ve XAML içinde WPF uygulaması veya ASP.NET ASPX (HTML) sayfasına karşılık gelen bileşenin bir görünümüdür. Denetleyici, arka plan kod ASP.NET ya da WPF karşılık gelen görünüm yönetmekten sorumludur bileşendir.

### <a name="model"></a>Model

Model nesnesinin genellikle bir uygulamaya özgü görüntülenebilir veya görünüme girilen verileri gösterimidir. Model genellikle gevşek - Örneğin, tanımlanan bizim **Phoneword_iOS** uygulama, telefon numaralarını (bir dize listesi gösterilir) listesi, Model. Biz bir çoklu platform uygulaması oluşturuyorsanız, biz paylaşmayı seçebilirsiniz **PhonewordTranslator** iOS ve Android uygulamaları arasında kod. Biz o paylaştırılmış kodu modeli olarak düşünün.

MVC, tamamen belirsiz *veri kalıcılığı* ve *erişim* modeli. Nasıl verilerin yalnızca diğer bir deyişle, MVC bizim veri göründüğüne veya nasıl depolandığını, care değil *temsil*. Örneğin, biz kullanmanız yeterlidir verilerimizi bir SQL veritabanında depolayın veya bazı bulut depolama mekanizması kalıcı seçebilir bir `List<string>`. MVC amacıyla, yalnızca veri temsilini kendisini düzende dahil edilir.

Bazı durumlarda, MVC Model kısmı boş olabilir. Örneğin, biz telefon translator nasıl çalıştığını, oluşturduğumuz neden ve nasıl rapor hataları için bizle temasa açıklayan uygulamamız için bazı statik sayfaları eklemeyi seçebilirsiniz. Bu uygulama ekranları hala görünümleri ve denetleyicileri kullanarak oluşturulması, ancak herhangi bir gerçek Model veri bulunmaz.

> [!NOTE]
> Bazı belgelerinde MVC örüntüsü Model kısmı tüm uygulama arka ucunu yalnızca kullanıcı Arabirimi üzerindeki görüntülenen verileri başvurabilir. Bu kılavuzda modelin modern bir yorumu kullanıyoruz ancak fark özellikle önemli değildir.

### <a name="view"></a>Görüntüle

Bir görünümü kullanıcı arabirimi çizmek için sorumlu bileşendir. MVC düzeni kullanan neredeyse tüm platformlarda, kullanıcı arabirimi görünüm hiyerarşisi oluşur. Biz bir MVC görünümü hiyerarşisini görüntüle - hiyerarşideki üst ve alt görünümleri herhangi bir sayıda kök görünümü olarak bilinen tek bir görünümde – olarak düşünebilirsiniz (bilinen veya subviews) altındaki. İOS, ekran içerik hiyerarşisini görüntüle MVC görünümü bileşen karşılık gelir.

### <a name="controller"></a>Denetleyici

Denetleyici nesnesi, iOS tarafından temsil edilir ve her şeyi birbirine bağlayan bileşendir `UIViewController`. Biz denetleyicisini yedekleme kodunun bir ekran veya Görünüm kümesi olarak düşünebilirsiniz. Denetleyici, kullanıcıdan gelen istekler için dinleyen ve uygun görünüm hiyerarşisi döndürmek için sorumludur. Bu görünümden (düğme tıklamaları, metin girişi, vb.) isteklerini dinleyen ve uygun görünümü değiştirme işlemleri ve görünümünü yeniden yüklemeyi gerçekleştirir. Denetleyici oluşturma veya veri deposu uygulamada var. hangi önlemeniz modelini alırken ve görünümü, verilerle doldurmak için de sorumludur.

Denetleyicileri, ayrıca diğer denetleyicileri yönetebilirsiniz. Örneğin, farklı bir ekran görüntülemek veya düzenlerine ve bunlar arasında geçiş izlemek için denetleyicileri yığınını yönetmek gerekiyorsa bir denetleyicisi başka bir denetleyici yükleyebilir. Sonraki bölümde, biz iOS görünüm denetleyicisi adı verilen özel türde kulak diğer denetleyicilerini yöneten bir denetleyici örneği görüyoruz bir *Gezinti denetleyicisi*.

## <a name="navigation-controller"></a>Gezinti denetleyicisi

Phoneword uygulamada birden fazla ekranlar arasında gezintiyi yönetmenize yardımcı olmak için bir gezinti denetleyicisi kullanılır. Gezinti denetleyicisi özelleştirilmiş olan `UIViewController` tarafından temsil edilen `UINavigationController` sınıfı. Tek bir içerik görünümü hiyerarşi yönetmek yerine, kendi özel içerik görünümü hiyerarşi biçiminde bir başlık, geri düğmesi ve diğer isteğe bağlı özellikler içeren bir gezinme araç yanı sıra diğer görünüm denetleyicileri Gezinti denetleyicisi yönetir.

Gezinti denetleyicisi iOS uygulamalarında yaygındır ve gezinti gibi Zımba iOS uygulamaları için tanır **ayarları** ekran aşağıda gösterildiği gibi uygulama:

 [![](hello-ios-multiscreen-deepdive-images/01.png "Ayarlar uygulamasında aşağıda gösterildiği gibi iOS uygulamaları için Gezinti Gezinti denetleyicisi sağlar")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Gezinti denetleyicisi üç birincil işlev sunar:

-  **İleri gezinme için hooks sağlayan** – Gezinti denetleyicisi kullanan Cerberus Gezinti benzetimini içerik görünümü hiyerarşileri nerede *gönderilen* üzerine bir *gezinme yığınında* . Gezinme yığını bir yığın Yürütülüyor yalnızca en üstteki çoğu kartı Aşağıdaki diyagramda gösterildiği gibi görünür durumda olduğu kartların düşünebilirsiniz:  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Bu diyagramda gezinme yığını kartlar olarak gösterilir.")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **İsteğe bağlı olarak bir geri düğmesi sağlar** - biz gezinme yığınında üzerine yeni bir öğe gönderdiğinizde başlık çubuğunda otomatik olarak görüntülemek bir *geri düğmesini* geriye doğru gezinmek kullanıcının izin verir. Geri düğmesine basarak *POP* geçerli görünüm denetleyicisi gezinme yığınında ve önceki içerik hiyerarşisini görüntüle penceresine yükler:  

    [![](hello-ios-multiscreen-deepdive-images/03.png "'Bir kart yığından pencerelerinin' Bu şemada gösterilmektedir.")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Başlık çubuğu sağlar** – Gezinti denetleyicisi üst kısmını adlı *başlık çubuğu* . Bu, aşağıdaki diyagramda gösterildiği gibi görünüm denetleyicisi başlığı görüntülemek için sorumlu değil:  

    [![](hello-ios-multiscreen-deepdive-images/04.png "Başlık çubuğunda Görünüm denetleyicisi başlığı görüntülemek için sorumludur")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)

### <a name="root-view-controller"></a>Kök görünümü denetleyicisi

Bir gezinti denetleyicisi içerik bir görünüm hiyerarşisi yönetmeyen, kendi başına görüntülenecek öğe yok sahiptir.
Bunun yerine, bir gezinti denetleyicisi ile eşleştirilmiş bir *kök görünüm denetleyicisi*:

 [![](hello-ios-multiscreen-deepdive-images/05.png "Bir gezinti denetleyicisi kök görünüm denetleyicisi ile eşleştirilir.")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Kök görünüm denetleyicisi Gezinti denetleyicinin yığın ilk görünüm denetleyicisini temsil eder ve kök görünümü denetleyicinin içerik hiyerarşisini görüntüle ilk içerik Görünümü penceresine yüklenen hiyerarşidir. Biz Gezinti denetleyicinin yığınındaki tüm uygulamamız koymak istiyorsanız, biz Sourceless ü taşımak için Gezinti denetleyicisi ve Phoneword uygulamada yaptığımız gibi bizim ilk ekranın görünüm denetleyicisi kök görünümü denetleyicisi olarak ayarlayın:

 [![](hello-ios-multiscreen-deepdive-images/06.png "Sourceless ü ilk ekranlar görünüm denetleyicisi kök görünümü denetleyicisi olarak ayarlar")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Ek Gezinti Seçenekleri

Gezinti denetleyicisi iOS gezintiyi işleme genel bir yoludur, ancak tek seçenek değildir. Örneğin, bir [Sekme çubuğu denetleyicisi](~/ios/user-interface/controls/creating-tabbed-applications.md) farklı işlevsel bir uygulamaya bölebilir ve bir [Bölünmüş Görünüm denetleyicisi](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) ana/ayrıntı görünümleri oluşturmak için kullanılabilir. Gezinti denetleyicisi bu başka gezinme paradigmalarını ile birleştirerek iOS içeriğinde gitmek ve mevcut birçok esnek bir yol sağlar.

## <a name="handling-transitions"></a>İşleme geçişleri

Phoneword izlenecek yolda, biz iki farklı yolla – iki görünüm denetleyicisi arasındaki geçiş ilk ü görsel taslak ile işlenir ve ardından program aracılığıyla. Bu seçeneklerin daha ayrıntılı olarak inceleyelim.

### <a name="prepareforsegue"></a>PrepareForSegue

İle bir Segue eklediğimiz ne zaman bir **Göster** eylem film şeridi için biz isteyin iOS Gezinti denetleyicinin yığın üzerine ikinci görünüm denetleyicisi göndermek için:

 [![](hello-ios-multiscreen-deepdive-images/09.png "Açılan listeden ayar segue türü")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Bir Segue film şeridi eklemek, ekranlar arasında basit bir geçiş oluşturmak için yeterli olur. Biz görünüm denetleyicileri arasında veri geçirmek istiyorsanız, geçersiz kılmak sahibiz `PrepareForSegue` yöntemi ve verileri kendimize işler:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

iOS çağrıları `PrepareForSegue` geçiş oluşur ve içinde bir film şeridini yönteme oluşturduğumuz Segue geçirmeden önce sağ.
Bu noktada, biz Segue'nın hedef görünüm denetleyicisi el ile ayarlamanız gerekir. Aşağıdaki kod, hedef görünüm denetleyicisi için bir tanıtıcı alır ve doğru sınıfı - CallHistoryController, bu durumda çevirir:

```csharp
CallHistoryController callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Son olarak, biz telefon numaralarını (modeli) listesini geçirmek `ViewController` için `CallHistoryController` ayarlayarak `PhoneHistory` özelliği `CallHistoryController` çevrilen telefon numaraları listesi:

```csharp
callHistoryContoller.PhoneNumbers = PhoneNumbers;
```

Bir Segue kullanarak verileri geçirmek için tam kod aşağıdaki gibidir:

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

### <a name="navigation-without-segues"></a>Gezinti olmadan etkinleştirilir

İlk Görünüm denetleyicisi kod ikinci geçiş bir Segue ile aynı işlemde olduğu halde el ile yapılması birkaç adım vardır.
İlk olarak kullandığımız `this.NavigationController` olan yığın Gezinti denetleyicisi bir başvuru almak için şu anda açık duyuyoruz. Ardından, gezinti denetleyicinin kullanıyoruz `PushViewController` el ile görünüm denetleyicisi ve geçiş animasyon uygulamak için bir seçenek geçirerek yığına sonraki görünüm denetleyicisi itme yöntemini (biz bunu `true`).

Aşağıdaki kod, arama geçmişi ekran Phoneword ekran durumundan işler:

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Sonraki görünüm denetleyiciye geçiş önce çağırarak el ile Film şeridinden örneklemek sahibiz `this.Storyboard.InstantiateViewController` ile film şeridi kimliği geçirme `CallHistoryController`:

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Son olarak, biz telefon numaralarını (modeli) listesini geçirmek `ViewController` için `CallHistoryController` ayarlayarak `PhoneHistory` özelliği `CallHistoryController` biz geçişte bir Segue işlenen, yaptığımız gibi çevrilen telefon numaraları listesi:

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Programlı geçişi için tam kod aşağıdaki gibidir:

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

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramları

Bu kılavuzda ele alınmamaktadır birkaç kavram Phoneword uygulama sunmuştur. Bu kavramlar şunları içerir:

-  **Otomatik oluşturma, görünüm denetleyicileri** – biz görünüm denetleyicisi için bir sınıf adı girdiğinizde **özellikler panelinde** , iOS Tasarımcısı o sınıfın var ve ardından sınıf bizim için yedekleme görünüm denetleyicisi oluşturur, denetler. Bu ve diğer iOS designer özellikler hakkında daha fazla bilgi için başvurmak [iOS Designer giriş](~/ios/user-interface/designer/introduction.md) Kılavuzu.
-  **Tablo görünümü denetleyicisi** – `CallHistoryController` bir tablo görünümü denetleyicisi. Tablo görünümü bir tablo görünümü denetleyicisi içeriyor, en yaygın düzen ve veri araç iOS görüntüler. Bu kılavuzun kapsamı dışındadır tablolarıdır. Tablo görünümü Denetleyicisi hakkında daha fazla bilgi için bkz [tablolar ve hücreler çalışma](~/ios/user-interface/controls/tables/index.md) Kılavuzu.
-   **Film şeridi kimliği** – film şeridi kimliği ayarı oluşturur bir görünüm denetleyicisi sınıfını içeren görsel taslak görünüm denetleyicisi için gerideki Objective-C. Objective-C sınıfı bulun ve görsel taslak görünüm denetleyicisi oluşturmak için görsel taslak kimliği kullanın. Görsel taslak kimlikleri hakkında daha fazla bilgi için bkz [görsel taslaklara giriş](~/ios/user-interface/storyboards/index.md) Kılavuzu.

## <a name="summary"></a>Özet

Tebrikler, ilk çoklu ekran iOS uygulamanızı tamamladınız!

Bu kılavuzda sunulan MVC örüntüsü ve çok filtrelenmiş bir uygulama oluşturmak için kullanılan. Biz de gezinti denetleyicisi ve iOS Gezinti destekleyen kendi rol incelediniz. Artık sağlam altyapısı kendi Xamarin.iOS uygulamaları geliştirmeye başlamak için ihtiyacınız vardır.

Ardından, Xamarin ile platformlar arası uygulamaları oluşturmak öğrenelim [mobil geliştirmeye giriş](~/cross-platform/get-started/introduction-to-mobile-development.md) ve [platformlar arası uygulamalar oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Kılavuzlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Hello, iOS (örnek)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS İnsan Arabirimi yönergelerine](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS sağlama portalı](https://developer.apple.com/ios/manage/overview/index.action)
