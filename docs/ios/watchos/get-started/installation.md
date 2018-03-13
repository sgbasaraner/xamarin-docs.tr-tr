---
title: "Kurulum ve yükleme"
description: "İçin watchOS geliştirmek için ayarlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: c423c9bf49c735673793f8e61134f7e705816d54
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="installation"></a>Yükleme

watchOS 4 macOS Xcode 9 Sierra (10,12) gerektirir.

watchOS 1 başlangıçta Xcode 7 ile OS X Yosemite (10.10) gereklidir.

> [!WARNING]
> [watchOS 1 güncelleştirmeleri 1 Nisan 2018 sonra kabul edilmeyecek](https://developer.apple.com/news/?id=11162017a). Gelecekteki güncelleştirmeleri watchOS 2 kullanmalıdır SDK veya üzeri; ile watchOS derleme 4 SDK önerilir.

## <a name="project-structure"></a>Proje yapısı

İzleme uygulama üç projelerin oluşur:

- **Xamarin.iOS iPhone uygulama projesi** -bu normal iPhone projesidir, Xamarin.iOS şablonlardan herhangi biri olabilir. İzleme uygulaması ve uzantısını bu ana proje içinde paketlenmiş.

- **Gözcü uzantı projesi** -bu izleme uygulaması için kod (örneğin, denetleyici sınıflar) içerir.

- **İzleme uygulaması projesi** -bu izleme uygulama için kullanıcı arabirimi film şeridi dosya tüm UI kaynaklarla içerir.

[İzleme Seti katalog örnek](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) çözüm şu şekilde Xamarin.Studio içinde görünür:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](installation-images/catalog-solution.png "Visual Studio'da Çözüm")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "Visual Studio'da Çözüm")

-----

İndirme ve çalıştırma [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) başlamak için örnek.
Örnek ekranlarından bulunabilir [denetimleri](~/ios/watchos/user-interface/index.md) sayfası.


## <a name="creating-a-new-project"></a>Yeni proje oluşturma

Yeni bir "izleme Çözüm"... oluşturulamıyor yerine var olan bir iOS uygulaması için bir izleme uygulaması ekleyebilirsiniz. İzleme uygulaması oluşturmak için aşağıdaki adımları izleyin:

1. Varolan projeyi yoksa, öncelikle seçin **Dosya > Yeni bir çözüm** ve bir iOS uygulaması oluşturma (örneğin, bir **Single View uygulaması**):

    [![](installation-images/cycle8-2-sml.png "Dosya > Yeni bir çözüm ve bir iOS uygulaması oluşturma")](installation-images/cycle8-2.png#lightbox)

2. İOS uygulaması oluşturulur (veya var olan iOS uygulamanızı kullanmayı planlıyorsanız sonra), çözüm üzerinde sağ tıklatın ve seçin **Ekle > Yeni Proje Ekle...** . İçinde **yeni proje** penceresi seçin **watchOS > Uygulama > WatchKit uygulama**:

    [![](installation-images/cycle8-6-sml.png "WatchOS seçin > Uygulama > WatchKit uygulama")](installation-images/cycle8-6.png#lightbox)

3. Sonraki ekranda hangi iOS uygulaması projesi izleme uygulama içermelidir seçmenize olanak sağlar:

    [![](installation-images/cycle8-7-sml.png "İOS uygulaması proje izleme uygulama içermelidir seçin")](installation-images/cycle8-7.png#lightbox)

4. Son olarak, projeyi kaydetmek istediğiniz konumu seçin (ve isteğe bağlı olarak, kaynak denetimi etkin):

    [![](installation-images/cycle8-8-sml.png "Proje kaydetmek istediğiniz konumu seçin")](installation-images/cycle8-8.png#lightbox)

5. Mac için Visual Studio otomatik olarak yapılandırır [proje başvuruları ve **Info.plist** ayarları](~/ios/watchos/get-started/project-references.md) sizin için.

## <a name="creating-the-watch-user-interface"></a>İzleme kullanıcı arabirimi oluşturma

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS Tasarımcısı kullanma

Gözcü uygulamanın üzerinde çift **Interface.storyboard** iOS Tasarımcısı kullanarak düzenlemek için. Film şeridi görünümünden üzerine arabirim denetleyicilerini ve kullanıcı Arabirimi denetimlerini sürükleyebilirsiniz **araç** ve bunları yapılandırma kullanarak **özellikleri** paneli:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "Tasarımcıda film şeridi")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "Tasarımcıda film şeridi")](installation-images/iosdesigner-vs.png#lightbox)

-----

Her yeni Arabirim Denetleyicisi vermelidir bir **sınıfı** seçerek ve ardından adı girerek **özellikleri** paneli (Bu oluşturacak gerekli C# arkasındaki koda dosyaları otomatik olarak):

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "Bir sınıf her yeni Arabirim Denetleyicisi verin")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "Bir sınıf her yeni Arabirim Denetleyicisi verin")

-----

Oluşturma tarafından segues **Ctrl + sürükleyerek** bir düğme, tablo veya arabirim denetleyicisinden başka bir arabirim denetleyicisine.


### <a name="using-xcode-on-the-mac"></a>Mac Xcode kullanarak

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Interface.storyboard dosyayı sağ tıklayıp seçerek, kullanıcı arabirimini oluşturmak için Xcode kullanmaya devam edebilirsiniz **birlikte Aç > Xcode arabirimi Oluşturucu**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio kullanıcıları, Xcode yapı Mac konağı doğrudan kullanmak için geçiş tarafından kendi kullanıcı arabirimini oluşturmak için de kullanabilirsiniz.
Mac için Visual Studio çözümünüzü açın ve ardından Interface.storyboard dosyasını sağ tıklatın ve seçin **birlikte Aç > Xcode arabirimi Oluşturucu**:

-----

![](installation-images/openwith-xcode.png "Xcode arabirimi Oluşturucusu'nda Interface.storyboard açın")

Xcode kullanıyorsanız, normal ettirilmesi izleme uygulamalar için aynı adımları izlemelisiniz [iOS app film şeritleri](~/ios/user-interface/storyboards/index.md) (çıkışlar ve eylemleri tarafından oluşturma gibi **Ctrl + sürükleyerek** içine **.h**üstbilgi dosyası).

Kaydettiğinizde film şeridi, otomatik olarak ekleyecek Xcode arabirimi Oluşturucusu'nda çıkışlar ve oluşturduğunuz Eylemler C# **. designer.cs** izleme uzantısı projedeki dosyaları.


### <a name="adding-additional-screens-in-xcode"></a>Xcode'da ek ekranlar ekleme

Xcode arabirimi oluşturucu kullanılarak şeridinizin (neler şablonda varsayılan olarak dahildir) ötesinde ek ekranlar eklediğinizde **C# kod dosyaları el ile eklemelisiniz** her yeni arabirimi denetleyicisi.

Başvurmak [görsel taslak haline getirme için yeni arabirim denetleyicilerini ekleme hakkında yönergeler Gelişmiş](~/ios/watchos/troubleshooting.md#add).

*Xamarin iOS Tasarımcısı bunu otomatik olarak yapar, el ile yapılan hiçbir adım gerekli değildir.*


## <a name="building"></a>Oluşturma

Diğer iOS projeleri gibi izleme uygulamasını içeren bir proje oluşturur. Yapı işlem sırayla kodu daha az izleme uygulaması (.app) içeren bir izleme uzantısı (.appex) içeren bir iPhone uygulamada (.app) neden olur.


## <a name="launching"></a>Başlatma

Studio (Mac yapı konağı başlar) ya da Visual Studio veya Visual Mac için kullanarak simulator izleme uygulamaları başlatabilir.

WatchKit uygulamayı başlatmak için iki modu vardır:

 - Normal uygulama modunu (varsayılan) ve
- [Bildirimleri](~/ios/watchos/platform/notifications.md) (test bildirim yükü JSON biçiminde gerektiren).

### <a name="xcode-8-support"></a>Xcode 8 desteği

Xcode 8 (veya üstü) yüklendikten sonra Apple Watch benzeticileri iOS benzeticileri ayrı (aksine [Xcode 6](#xcode6), olarak göründüğü burada bir *dış görüntü*).
İzleme uygulama projesini seçin ve başlangıç projesi olun simulator listesi gösterecektir *iOS benzeticileri* (aşağıda gösterildiği gibi) aralarından seçim yapabileceğiniz.

[![](installation-images/xs-xcode8-watchos3-sml.png "Simulator türü seçme")](installation-images/xs-xcode8-watchos3.png#lightbox)

Hata ayıklama, başlattığınızda *iki* benzeticileri başlamalıdır - iOS simülatörü'nü *ve* Apple Watch Simulator. Kullanmak **komutu + Shift + H** için izleme menü ve saat yüz; gidin ve kullanmak için **donanım** ayarlamak için menü **zorla Touch baskısı**. İzleme paneli veya fare kaydırma dijital Dama yapma kullanarak benzetimini yapacaksınız.

#### <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki hata görünür **uygulama çıktısı** eşleştirilmiş bir izleme sahip olmayan bir simulator başlatmaya çalışırsanız:

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Başvurmak [Apple'nın forumları](https://forums.developer.apple.com/thread/7783) Varsayılanları işe yaramazsa benzeticileri yapılandırma hakkında yönergeler için.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 ve watchOS 1

Yapmanız gereken *İzleme uzantı projesi* **başlangıç projesi** çalıştıran veya uygulama hata ayıklama önce. "İzleme uygulama başlatılamıyor" ve iOS uygulamasını seçin, sonra iOS Simulator'da normal olarak başlatılır.

Varsayılan olarak, normal bir izleme uygulaması başlatır **uygulama** modu (değil tek bakışta veya bildirimleri) Mac'ın Visual Studio'dan **çalıştırmak** veya **hata ayıklama** komutları.

Xcode 6 kullanırken yalnızca iPhone 5, iPhone 5s'dir, iPhone 6 ve iPhone 6 Plus ya da dış görüntü etkinleştirebilir **Apple Watch - 38mm** veya **Apple Watch - 42mm** izleme uygulamaları burada olacaktır görüntülenir.

**Not:** izleme ekranın otomatik olarak iOS Simulator'da Xcode 6 kullanırken görünmez unutmayın.
Kullanım **donanım > Dış görüntüler** izleme ekran göstermek için menüsü.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Bildirim modu başlatılıyor

Başvurmak [bildirim sayfası](~/ios/watchos/platform/notifications.md) bilgi kodu bildirimlerini nasıl ele alınacağını.


Mac için Visual Studio izleme uygulama içeren bir bildirim başlayabileceğini _başlangıç modları_ bildirimler için:



İzleme uygulaması projesine sağ tıklayıp seçin **çalıştırmak ile > özel yapılandırma...** :


[![](installation-images/runwith-customparams-sml.png "Özel yapılandırma çalıştırma")](installation-images/runwith-customparams.png#lightbox)


Açılır **özel parametreler** seçebileceğiniz penceresi **bildirim** (ve bir JSON yükü sağlamak), tuşuna basarak **çalıştırmak** benzeticisinde izleme uygulamasını başlatmak için:


[![](installation-images/runwith-execargs-sml.png "Bildirim ve yükü ayarlama")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>Hata Ayıklama

Hata ayıklama Mac için Visual Studio ve Visual Studio içinde desteklenir.
Bildirimleri modda hata ayıklama sırasında bildirim JSON dosyasını sağlamayı unutmayın. Bu ekran görüntüsü, bir izleme uygulamasında isabet bir hata ayıklama kesme gösterir:

![](installation-images/debug-sml.png "Bu ekran, bir izleme uygulamasında isabet bir hata ayıklama kesme gösterir")

Başlatma yönergeleri izledikten sonra izleme uygulamanızın üzerinde çalışan ile karşılaşırsınız **iOS simülatörü'nü (İzleme)**.
Bildirim modu için seçtiğiniz **hata ayıklama > açık sistem günlüğüne** (**CMD + /**) ve `Console.WriteLine` kodunuzda.

### <a name="debugging-lifecycle-event-handlers"></a>Hata ayıklama yaşam döngüsü olay işleyicileri

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

WatchOS şablon dosyalarını (gibi `InterfaceController`, `ExtensionDelegate`, `NotificationController`, ve `ComplicationController`) zaten uygulanmış gerekli yaşam döngüsü yöntemleriyle gelir. Ekleme `Console.WriteLine` çağrıları ve okuma **uygulama çıktısı** olay yaşam döngüsü daha iyi anlamak için.



## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [İlk izleme uygulama video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple'nın WatchKit ipuçları](https://developer.apple.com/watchkit/tips/)
