---
title: "Bölüm 24 özeti. Sayfa gezintisi"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b8eac45c52093dea23c08a19d219fa0bbd8d55ab
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-24-page-navigation"></a>Bölüm 24 özeti. Sayfa gezintisi

Birçok uygulama birden çok sayfa aralarında kullanıcı gider oluşur. Uygulama her zaman bir *ana* sayfa veya *ev* sayfasında, ve buradan kullanıcı geri dönme için yığındaki tutulan başka sayfalara gider. Ek gezinti seçenekleri ele alınmıştır [ **bölüm 25. Sayfa çeşit**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Kalıcı sayfaları ve kalıcı olmayan sayfaları

`VisualElement` tanımlayan bir [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) türündeki özelliği [ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/), yeni bir sayfaya gitmek için aşağıdaki iki yöntem içerir:

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

Her iki yöntem kabul bir `Page` örneği bir bağımsız değişken ve return olarak bir `Task` nesnesi. Aşağıdaki iki yöntemden önceki sayfaya gidin:

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

Kullanıcı arabirimi kendi varsa **geri** düğmesini (Android ve Windows telefonlar gibi) bu yöntemleri çağırmak uygulama için gerekli değildir.

Bu yöntemlerin herhangi kullanılabilir, ancak `VisualElement`, gelen genellikle adlı `Navigation` özelliği geçerli `Page` örneği.

Önceki sayfaya geri dönmeden önce bazı bilgiler sayfasında sağlamak için kullanıcının gerektiğinde uygulamalar genellikle kalıcı sayfalarını kullanın. Kalıcı olmayan sayfaları adlandırılan kalıcı olmayan veya *hiyerarşik*. Sayfasında hiçbir şey kalıcı veya geçici olarak ayırt; Bunun yerine, gitmek için kullanılan yöntemi tarafından yönetilir. Tüm platformlar arası çalışması için kalıcı bir sayfa kendi kullanıcı arabirimi önceki sayfaya geri gezinmek için sağlamanız gerekir.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) örnek geçici ve kalıcı sayfaları arasındaki farkı keşfedin olanak tanır. Sayfa gezintisi kullanan herhangi bir uygulama için giriş sayfasını geçmelidir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) oluşturucuda, genellikle programın `App` sınıfı. Bir ödül olduğundan, artık ayarlamak gerektiği bir `Padding` iOS sayfasında.

Kalıcı olmayan sayfalar için sayfanın 's keşfeder [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) özelliği görüntülenir. İOS, Android ve Windows tablet ve Masaüstü platformları önceki sayfaya gitmek için bir kullanıcı arabirimi öğesi sağlar. İndirmelere, Android ve Windows phone cihazları bir standart olan **geri** düğmesi geri dönün.

Kalıcı sayfaları, sayfa için `Title` görüntülenen değil ve hiç kullanıcı arabirimi öğesi önceki sayfaya dönmek için sağlanır. Android ve Windows phone standart kullanabilirsiniz ancak **geri** dönmek için önceki sayfaya diğer platformlarda kalıcı sayfasına geri dönmek için kendi mekanizmasını sağlamalıdır düğmesi.

### <a name="animated-page-transitions"></a>Animasyonlu sayfa geçişleri

Çeşitli Gezinti yöntemlerinin alternatif sürümlerini ayarlamak için ikinci bir Boolean bağımsız değişkeniyle sağlanan `true` animasyon dahil etmek için sayfa geçişi istiyorsanız:

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

Ancak, standart sayfa gezinti yöntemlerini animasyonun varsayılan olarak, bunlar yalnızca başlangıç belirli bir sayfada (Bu bölümün sonunda anlatıldığı gibi) gezinme için değerlidir veya kendi giriş animasyon (anlatıldığı gibi sağlanırken içerir [ **Chapter22. Animasyon**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Görsel ve işlevsel farklılıkları

`NavigationPage` sınıf örneği yükleyen ayarlayabileceğiniz iki özellik içerir, `App` yöntemi:

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` Ayrıca üzerinde ayarlandıktan belirli sayfa etkileyen dört ekli bağlanabilir özellikleri içerir:

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) Ve [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) Ve [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) ve [ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/) yalnızca ios'ta çalışma
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) ve [ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/) iOS ve Android yalnızca çalışma

### <a name="exploring-the-mechanics"></a>Mekanizması keşfetme

Sayfa gezinti yöntemlerini tüm zaman uyumsuz ve kullanılmalıdır `await`. Tamamlandığında, sayfa gezintisi, ancak yalnızca sayfa gezintisi yığınını incelemek güvenli olan tamamlandığını göstermez.

Bir sayfadan diğerine gittiğinde, ilk sayfa genellikle bir çağrı alır, [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) yöntemi ve ikinci sayfasında yapılan bir çağrı alır, [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) yöntemi. Benzer şekilde, bir sayfadan diğerine geri döndüğünde, ilk sayfa için bir çağrı alır, `OnDisappearing` yöntemi ve ikinci sayfasında genellikle bir çağrı alır, `OnAppearing` yöntemi. Bu çağrılar (ve gezinti çağırır zaman uyumsuz yöntemleri tamamlanmasından) sırası bağımlı platformudur. Bu yöntem çağrılarını ortaya olmayan Android kalıcı sayfa gezintisi nedeniyle iki önceki deyimleri "Genel" sözcüğü kullanılır.

Ayrıca, çağrılar `OnAppearing` ve `OnDisappearing` yöntemlerini sayfa gezintisi mutlaka belirtmek yok.

`INavigation` Arabirimi Gezinti yığını inceleyin olanak tanıyan iki koleksiyon özellikleri içerir:

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) tür `IReadOnlyList<Page>` kalıcı olmayan yığınının
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) tür `IReadOnlyList<Page>` kalıcı yığınının

Bu yığınlar erişim güvenlisidir `Navigation` özelliği `NavigationPage` (olacağı `App` sınıfının [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) özelliği). Yalnızca zaman uyumsuz sayfa gezinti yöntemlerini tamamladıktan sonra bu yığınları incelemek güvenli değildir. [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/) Özelliği `NavigationPage` geçerli sayfa geçerli sayfa kalıcı bir sayfa ise, ancak belirtir yerine son kalıcı olmayan sayfayı göstermez.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) örnek sayfa gezintisi ve yığınları ve sayfa erişimlerinizde yasal türlerini keşfetmenize olanak sağlar:

- Kalıcı olmayan bir sayfa kalıcı olmayan başka bir sayfaya veya kalıcı sayfasına gidebilirsiniz
- Kalıcı bir sayfa yalnızca başka bir kalıcı sayfasına gidebilirsiniz

### <a name="enforcing-modality"></a>Şekil zorlama

Kullanıcıdan bazı bilgiler almak gerekli olduğunda uygulama kalıcı bir sayfa kullanır. Kullanıcı bu bilgileri sağlanan kadar önceki sayfaya geri döndürme engellenmelidir. İOS üzerinde sağlamak kolay bir **geri** düğmesini ve yalnızca kullanıcı sayfasıyla tamamladığında etkinleştirin. Ancak Android ve Windows phone cihazlar için uygulama kılmalıdır [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/) yöntemi ve return `true` program işleniyorsa **geri** kendisi, örnekte gösterildiği şekilde düğmesi [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) örnek.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) örneği, bunun bir MVVM senaryoda nasıl çalıştığını gösterir.

## <a name="navigation-variations"></a>Gezinti farklılıkları

Belirli bir kalıcı sayfa için birden çok kez gittiğinizde, böylece kullanıcı yazmak yerine bilgileri düzenleyebilirsiniz bu bilgileri korurlar tümü yeniden. Bu kalıcı sayfa belirli bir örneği koruyarak işlemek, ancak daha iyi bir yaklaşım (özellikle de iOS) bir görünüm modeli bilgileri koruma.

### <a name="making-a-navigation-menu"></a>Gezinti Menüsü yapma

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) örnek gösterilmektedir kullanarak bir `TableView` menü öğelerini listelemek. Her bir öğe ile ilişkili bir `Type` belirli bir sayfa için nesne. Bu öğe seçildiğinde, program sayfası oluşturur ve ona gider.

[![Üçlü ekran görünümü galeri türü](images/ch24fg21-small.png "Tablo görünümü listeleme menü öğeleri")](images/ch24fg21-large.png "Tablo görünümü listeleme menü öğeleri")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) menü türleri yerine her sayfa örneklerini içerdiğinden, örnek biraz farklı. Bu her sayfasındaki bilgileri korumaya yardımcı olur, ancak tüm sayfaları program başlangıcında örneğinin oluşturulması gerekir.

### <a name="manipulating-the-navigation-stack"></a>Gezinti yığını düzenleme

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) tarafından tanımlanan çeşitli işlevleri göstermektedir `INavigation` imkan sağlayan Gezinti yığınını yapılandırılmış bir şekilde düzenlemek:

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) ve [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/) isteğe bağlı animasyon ile

### <a name="dynamic-page-generation"></a>Dinamik sayfa oluşturma

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) örneği, çalışma zamanında kullanıcı girdisi tabanlı bir Web sayfası oluşturmak gösterir.

## <a name="patterns-of-data-transfer"></a>Veri aktarımı desenleri

Genellikle, veri sayfaları & #x 2014 arasında paylaşmak gereklidir; navigated sayfasına ve veri çağrılması sayfasına dönmek için bir sayfa için veri aktarmak için. Bunu yapmak için birçok tekniği vardır.

### <a name="constructor-arguments"></a>Oluşturucu bağımsız değişkenleri

Yeni bir sayfaya giderken kendisini başlatmak için sayfaya izin veren bir oluşturucu bağımsız değişkeni ile sayfa sınıfının örneği mümkündür. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) örnek bu gösterir. Ayrıca navigated sayfanın olması mümkündür, `BindingContext` ona gider sayfası tarafından ayarlayın.

### <a name="properties-and-method-calls"></a>Özellik ve yöntem çağrıları

Geri kalan veri aktarımı örnekleri bir sayfa başka bir sayfaya gittiğinde sayfaları ve geri arasında bilgi geçirme sorun keşfedin. Bu tartışma içinde *ev* sayfasına gider *bilgisi* sayfasında ve başlatılmış bilgileri aktarmanız gerekir *bilgisi* sayfası. *Bilgisi* sayfa kullanıcıdan ek bilgileri alır ve bilgilerini aktarır *ev* sayfası.

*Ev* sayfa kolayca erişebileceği ortak yöntemler ve Özellikler *bilgileri* bu sayfayı başlatır hemen sonra sayfa. *Bilgisi* sayfa genel yöntemler ve özellikler de erişebilir *ev* sayfa, ancak bu hassas olabilir için iyi bir zamandır seçme. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) yapar örnek bu kendi `OnDisappearing` geçersiz kılar. Bir dezavantajı olduğundan *bilgisi* sayfa türünü bilmeniz gereken *ev* sayfası.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) sınıfı birbirleri ile iletişim kurmak iki sayfa için başka bir yol sağlar. Bir metin dizesi tarafından tanımlanır ve herhangi bir nesne tarafından eşlik iletileri.

Belirli bir türden iletileri almak isteyen bir program bunlara abone olmalısınız kullanarak [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/) ve bir geri çağırma işlevi belirtin. Daha sonra onu çağırarak sona erdirebilirsiniz [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/). Geri çağırma işlevi aracılığıyla gönderilen belirtilen ada sahip belirtilen türden gönderilen tüm iletiler aldığı [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) yöntemi.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) program ileti Merkezi'ni kullanarak veri aktarmak nasıl gösterir, ancak yeniden bu gerektiren *bilgisi* sayfa türünübilmeniz*ev* sayfası.

### <a name="events"></a>Olaylar

Olay, o sınıfın türü bilmeden başka bir sınıf bilgileri göndermek bir sınıf için beri bir yaklaşımdır. İçinde [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) örnek *bilgisi* bilgileri hazır olduğunda harekete bir olay sınıfı tanımlar. Ancak, için kullanışlı bir yer yok *ev* olay işleyicisi ayırmak için sayfa.

### <a name="the-app-class-intermediary"></a>Uygulama sınıfı aracı

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) örnek göstermektedir tanımlanan özelliklere erişmek nasıl `App` sınıfı her ikisi tarafından *ev* sayfa ve *bilgisi*sayfası. Bu iyi bir yöntemdir, ancak daha iyi bir şey sonraki bölümde açıklanmaktadır.

### <a name="switching-to-a-viewmodel"></a>Bir ViewModel değiştirme

Bilgi sağlayan için bir ViewModel kullanarak *ev* sayfa ve *bilgisi* bilgi sınıfının örneğini paylaşmayı sayfası. Bu, gösterilmiştir [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) örnek.

### <a name="saving-and-restoring-page-state"></a>Kaydetme ve sayfa durumu geri yükleme

`App` Sınıfı aracı ViewModel yaklaşım mı ideal program uyku moduna kalırsa uygulamanın bilgileri kaydettiğinizde, sırada *bilgisi* sayfa etkin olduğunu. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) örnek bu gösterir.

## <a name="saving-and-restoring-the-navigation-stack"></a>Kaydetme ve gezinti yığını geri yükleme

Bunu geri yüklendiğinde genel durumda uyku moduna sayfalı bir program aynı sayfaya gitmek. Başka bir deyişle, böyle bir program Gezinti yığını içeriğini kaydetmeniz gerekir. Bu bölümde, bu işlemi bu amaç için tasarlanmış bir sınıftaki otomatikleştirmek gösterilmiştir. Bu sınıf Ayrıca bunları kaydetmek ve sayfa durumlarına geri yüklemek izin vermek için tek tek sayfaları çağırır.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı adlı bir arabirim tanımlar [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) sınıfları kaydetme ve öğelerigeriyüklemeiçinuygulayabileceğiniz`Properties`sözlük.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığı türer `Application`. Ardından türetilemeyeceğini, `App` sınıfıyla `MultiPageRestorableApp` ve bazı temizlik gerçekleştirin.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) kullanımını gösteren `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Şuna benzer bir gerçekçi uygulaması

[ **Not tutucu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) örnek de kullanır `MultiPageRestorableApp` ve girme ve kaydedilir notlarının düzenleme izin veren `Properties` sözlük.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 24 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Bölüm 24 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hiyerarşik gezinme](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Kalıcı sayfaları](~/xamarin-forms/app-fundamentals/navigation/modal.md)
