---
title: Bölüm 24 özeti. Sayfa gezintisi
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 24 özeti. Sayfa gezintisi'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 35130baac4025fe69dbc7aa9b6928f824b35c573
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156710"
---
# <a name="summary-of-chapter-24-page-navigation"></a>Bölüm 24 özeti. Sayfa gezintisi

Birçok uygulama birden çok sayfa yanı sıra kullanıcı gittiğinde oluşur. Uygulama her zaman bir *ana* sayfası veya *giriş* sayfasında ve oradan geri dönmek için bir yığın içinde tutulan diğer sayfalara kullanıcı gider. Ek gezinti seçenekleri kapsamdaki [ **bölüm 25. Sayfa çeşitleri**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Kalıcı sayfalar ve kalıcı olmayan sayfaları

`VisualElement` tanımlayan bir [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) türünün özelliği [ `INavigation` ](xref:Xamarin.Forms.INavigation), yeni bir sayfaya gitmek için aşağıdaki iki yöntemi içerir:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Her iki yöntem de kabul bir `Page` örneği bir bağımsız değişkeni ve dönüş olarak bir `Task` nesne. Aşağıdaki iki yöntemden önceki sayfaya gidin:

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Kullanıcı arabirimi kendi varsa **geri** düğmesine (Android ve Windows telefonlar gibi) bu yöntemleri çağırmak uygulama için gerekli değildir.

Bu yöntemlerin herhangi kullanılabilir, ancak `VisualElement`, gelen genel olarak adlandırılan `Navigation` özelliği geçerli `Page` örneği.

Kullanıcı önceki sayfaya geri dönmeden önce sayfada bazı bilgileri sağlamanız gerektiğinde uygulamalar genellikle kalıcı sayfalar kullanın. Kalıcı olmayan sayfaların adlandırılan kalıcı olmayan veya *hiyerarşik*. Sayfadaki bir şey kalıcı veya geçici olarak ayırır; Bunun yerine, gitmek için kullanılan yöntemi tarafından yönetilir. Tüm platformlar arası çalışması için kalıcı bir sayfa önceki sayfaya geri gezinme için kendi kullanıcı arabirimi sağlamalısınız.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) örnek geçici ve kalıcı sayfalar arasında fark olarak keşfetmenize olanak sağlar. Sayfa gezintisi kullanan herhangi bir uygulamanın giriş sayfası için geçmesi gereken [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) oluşturucuda, genellikle programın `App` sınıfı. Bir ödül olduğundan, artık yapmanız gerektiğini bir `Padding` sayfasında iOS için.

Kalıcı olmayan sayfaları için sayfa 's, keşfeder [ `Title` ](xref:Xamarin.Forms.Page.Title) özelliği görüntülenir. Önceki sayfaya gitmek için bir kullanıcı arabirimi öğesi, iOS, Android ve Windows tablet ve Masaüstü platformları belirtin. Kurs, Android ve Windows phone cihazları bir standart olan **geri** geri dönmek için düğme.

Kalıcı sayfalar, sayfanın için `Title` gösterilmez, ve herhangi bir kullanıcı arabirimi öğesinin önceki sayfaya dönmek için sağlanır. Android ve Windows phone standart kullanabilirsiniz, ancak **geri** düğmesi döndürmek için önceki sayfasına geri dönmek için kendi mekanizmasına kalıcı sayfanın diğer platformlarda sağlamalıdır.

### <a name="animated-page-transitions"></a>Animasyonlu sayfa geçişleri

Çeşitli gezinme yöntemlerinden farklı sürümlerini ayarlamak için ikinci bir Boole bağımsız değişkeniyle sağlanan `true` sayfa geçişi animasyon eklemek istiyorsanız:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

Ancak, standart sayfa gezintisi yöntemleri animasyon varsayılan olarak, bu yalnızca başlangıç belirli bir sayfada (Bu bölümün sonuna doğru açıklandığı gibi) giderek için değerli ve kendi giriş animasyon (bölümünde açıklandığı gibi sağlanırken içerir [ **Chapter22. Animasyon**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Görsel ve işlevsel farklılıkları

`NavigationPage` sınıfta başlattığınızda ayarlayabileceğiniz iki özelliklerini içeren, `App` yöntemi:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` Belirli sayfa üzerinde ayarlanırlar etkileyen dört ekli bağlanabilir özellikler de içerir:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) ve [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) ve [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) ve [ `GetBackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) iş yalnızca iOS
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) ve [ `GetTitleIcon` ](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) yalnızca iOS ve Android üzerinde çalışır

### <a name="exploring-the-mechanics"></a>Araştırma mekanizması

Sayfa Gezinti yöntemleri tüm zaman uyumsuzdur ve kullanılmalıdır `await`. Tamamlandığında, sayfa gezintisi, ancak yalnızca sayfa gezinme yığınında incelemek güvenli olan tamamlandığını göstermiyor.

İlk sayfasının bir sayfa diğerine geçtiğinde, genellikle bir çağrı alır. kendi [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) yöntemi ve ikinci sayfasında alır bir çağrı, [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) yöntemi. Benzer şekilde, başka bir sayfa geri döndüğünde, ilk sayfa için bir çağrı alır, `OnDisappearing` yöntemi ve ikinci sayfasında genellikle bir çağrı alır, `OnAppearing` yöntemi. Bu çağrılar (ve gezinti çağıran tamamlanmasını zaman uyumsuz yöntemler) sırasını bağımlı platformudur. İki yukarıdaki ifadeyi "Genel" sözcüğü, bu yöntem çağrılarının oluşmaz Android kalıcı sayfa gezintisi nedeniyle kullanılır.

Ayrıca, çağrılar `OnAppearing` ve `OnDisappearing` yöntemleri sayfa gezintisi gerekmeyen gösteren yok.

`INavigation` Arabirimi, gezinme yığınında incelemenize olanak tanıyan iki koleksiyon özellikleri içerir:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) tür `IReadOnlyList<Page>` için geçici yığın
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) tür `IReadOnlyList<Page>` kalıcı yığınının

Bu yığınlar erişmek güvenli `Navigation` özelliği `NavigationPage` (olacağı `App` sınıfın [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) özelliği). Yalnızca zaman uyumsuz sayfa gezintisi yöntemleri tamamladıktan sonra bu yığınları incelemek güvenlidir. [ `CurrentPage` ](xref:Xamarin.Forms.NavigationPage.CurrentPage) Özelliği `NavigationPage` geçerli sayfa geçerli sayfa kalıcı bir sayfa ise, ancak belirtir yerine son kalıcı olmayan sayfayı göstermez.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) örnek sayfa gezintisi ve Yığınlar ve sayfa gezintiler yasal türlerini keşfetmenize olanak sağlar:

- Kalıcı olmayan bir sayfaya başka bir geçici veya kalıcı sayfası için gidebilirsiniz.
- Kalıcı bir sayfa yalnızca başka bir kalıcı sayfasına gidebilirsiniz.

### <a name="enforcing-modality"></a>Şekil zorlama

Kullanıcıdan bazı bilgiler elde etmek gerekli olduğunda uygulama kalıcı bir sayfa kullanır. Kullanıcı, önceki sayfaya döndürmesini bu bilgileri sağlanmadıkça engellenmelidir. İos'ta sağlamak kolay bir **geri** düğmesine tıklayın ve yalnızca kullanıcının sayfayla sona erdiğinde etkinleştirin. Ancak Android ve Windows phone cihazları için uygulama geçersiz kılmalıdır [ `OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) yöntemi ve dönüş `true` program işleniyorsa **geri** kendisini gösterildiği şekilde düğmesi [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) örnek.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) örnek, bir MVVM senaryoda bunun nasıl çalıştığını gösterir.

## <a name="navigation-variations"></a>Gezinti farklılıkları

Belirli bir kalıcı sayfa için birden çok kez gittiğinizde, böylece kullanıcı, yazmak yerine bilgileri düzenleyebilirsiniz. Bu bilgi korurlar tümü yeniden. Kalıcı sayfasında belirli bir örneğini koruyarak bu işleyebilir, ancak daha iyi bir yaklaşım (özellikle iOS üzerinde), bir görünüm modelinde bilgi koruma.

### <a name="making-a-navigation-menu"></a>Gezinti Menüsü yapma

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) örnek gösterir kullanarak bir `TableView` menü öğeleri listelemek için. Her bir öğe ile ilişkili bir `Type` nesne için belirli bir sayfa. Bu öğe seçildiğinde, program sayfası oluşturur ve ona gider.

[![Üç ekran görünümü galeri türü](images/ch24fg21-small.png "Tablo görünümü listeleme menü öğelerini")](images/ch24fg21-large.png#lightbox "Tablo görünümü menü öğelerini listeleme")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) menüsünün türleri yerine her sayfa örneklerini içerdiğinden, örnek biraz farklı. Bu bilgilerin her sayfadaki korumaya yardımcı olur, ancak tüm sayfaları, program başlangıcında örneği gerekir.

### <a name="manipulating-the-navigation-stack"></a>Gezinme yığınında düzenleme

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) çeşitli işlevler tarafından tanımlanan gösterir `INavigation` sağlayan gezinme yığınında ile yapılandırılmış bir biçimde düzenleme:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) ve [ `PopToRootAsync` ](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) isteğe bağlı animasyon ile

### <a name="dynamic-page-generation"></a>Dinamik sayfa oluşturma

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) örnek, çalışma zamanında kullanıcı girişini temel alarak bir Web sayfası oluşturmak gösterir.

## <a name="patterns-of-data-transfer"></a>Veri aktarımı

Sayfaları arasında veri paylaşımı genellikle gereklidir &mdash; taşınabilecek sayfasına ve veri kendisini çağıran sayfasına dönmek bir sayfa için veri aktarmak için. Bunu yapmak için çeşitli teknikler vardır.

### <a name="constructor-arguments"></a>Oluşturucu bağımsız değişkenleri

Yeni bir sayfasına gittiğinizde, sayfanın kendisi başlatmak izin veren bir oluşturucu bağımsız değişkeni ile sayfası sınıfı örneğini oluşturmak mümkündür. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) örnek bunu gösterir. Ayrıca taşınabilecek sayfanın olması daha olasıdır, `BindingContext` ızgaranın kendisine sayfası tarafından ayarlayın.

### <a name="properties-and-method-calls"></a>Özellik ve yöntem çağrıları

Bir sayfa başka bir sayfaya gittiğinde sayfaları ve arka arasında bilgi geçirme sorununu kalan veri aktarımı örnekleri keşfedin. Bu tartışmalarında *giriş* sayfasına gider *bilgisi* sayfasında ve başlatılmış bilgileri aktarmanız gerekir *bilgisi* sayfası. *Bilgisi* sayfası kullanıcıdan ek bilgileri alır ve bilgilerini aktarır *giriş* sayfası.

*Giriş* sayfası genel yöntemleri ve özellikleri de kolayca erişebileceği *bilgisi* sayfayı başlatır hemen sonra sayfa. *Bilgisi* sayfasına da erişebilirsiniz genel yöntemleri ve özellikleri *giriş* sayfa, ancak bu zor olabilir için iyi bir zaman seçme. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) örnek bunu yapar, `OnDisappearing` geçersiz kılar. Bir eksisi ise *bilgisi* sayfa türünü bilmeniz gerekir *giriş* sayfası.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) sınıfı birbirleri ile iletişim kurmak iki sayfa için başka bir yol sağlar. Bir metin dizesi aracılığıyla tanımlanır ve herhangi bir nesne tarafından eşlik iletileri.

Belirli bir türden iletileri almak isteyen bir program kendilerine abone olmalısınız kullanarak [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) ve bir geri çağırma işlevini belirtin. Çağırarak daha sonra abonelikten çıkabilirsiniz [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*). Geri çağırma işlevi aracılığıyla gönderilen belirtilen adla belirtilen türden gönderilen tüm iletiler alır [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) yöntemi.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) programı ileti merkezi aracılığıyla veri aktarmak nasıl gösterir, ancak bu, yeniden gerektirir *bilgisi* sayfa türünübilmeniz*giriş* sayfası.

### <a name="events"></a>Olaylar

Olay, bu sınıfın türü farkında olmadan başka bir sınıf için bilgi göndermek bir sınıf için beri bir yaklaşımdır. İçinde [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) örnek *bilgisi* bilgileri hazır olduğunda harekete bir olay sınıfını tanımlar. Ancak, için uygun bir yer yoktur *giriş* olay işleyicisi ayırmak için sayfa.

### <a name="the-app-class-intermediary"></a>Uygulama sınıf aracı

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) örnek tanımlanan özelliklere nasıl erişileceğini göstermektedir `App` sınıf her ikisi tarafından *giriş* sayfası ve *bilgisi*sayfası. Bu iyi bir yöntemdir, ancak daha iyi bir sonraki bölümde açıklanmaktadır.

### <a name="switching-to-a-viewmodel"></a>Bir ViewModel için değiştirme

Bilgi sağlayan bir ViewModel kullanılması *giriş* sayfası ve *bilgisi* bilgi sınıfının örneğini paylaşmak için sayfa. Bu gösterilmiştir [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) örnek.

### <a name="saving-and-restoring-page-state"></a>Kaydetme ve sayfa durumu geri yükleme

`App` Sınıfı aracı veya ViewModel yaklaşım ideal program uyku moduna geçer, uygulama bilgileri kaydettiğinizde, ancak *bilgileri* sayfasıdır etkin. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) örnek bunu gösterir.

## <a name="saving-and-restoring-the-navigation-stack"></a>Kaydetme ve gezinme yığınında geri yükleme

Bu geri yüklendiğinde genel durumunda uyku moduna sayfalı bir program aynı sayfasına giderek. Başka bir deyişle, böyle bir program gezinme yığınında içeriğini kaydetmeniz gerekir. Bu bölümde, bu amaca yönelik olarak tasarlanmış bir sınıf içinde bu işlemi otomatikleştirmek gösterilmektedir. Bu sınıf, ayrıca kaydetme ve geri yükleme sayfası durumlarına izin vermek üzere her bir sayfayı çağırır.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı adlı bir arabirim tanımlar [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) sınıfları kaydetme ve öğelerdegeriyüklemeiçinuygulayabileceğiniz`Properties`sözlüğü.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığı türetilir `Application`. Ardından türetebilirsiniz, `App` gelen sınıfı `MultiPageRestorableApp` ve bazı temizlik gerçekleştirebilirsiniz.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) kullanımını gösteren `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Şuna benzer bir gerçek yaşam uygulaması

[ **Not tutucu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) örnek ayrıca kullanır `MultiPageRestorableApp` ve girme ve kaydedilen notları düzenlenmesine izin veren `Properties` sözlüğü.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 24 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Bölüm 24 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hiyerarşik Gezinme](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Kalıcı Sayfalar](~/xamarin-forms/app-fundamentals/navigation/modal.md)
