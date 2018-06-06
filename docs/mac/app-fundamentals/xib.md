---
title: Xamarin.Mac .xib dosyaları
description: Bu makalede, Xcode'nın arabirimi Oluşturucu'da oluşturmak ve Xamarin.Mac uygulama için kullanıcı arabirimleri korumak için oluşturulan .xib dosyaları ile çalışma kapsar.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3ef536ddb19ed60975368bd022e57c34c6f473dc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792280"
---
# <a name="xib-files-in-xamarinmac"></a>Xamarin.Mac .xib dosyaları

_Bu makalede, Xcode'nın arabirimi Oluşturucu'da oluşturmak ve Xamarin.Mac uygulama için kullanıcı arabirimleri korumak için oluşturulan .xib dosyaları ile çalışma kapsar._

> [!NOTE]
> Film şeritleri ile Xamarin.Mac uygulama için kullanıcı arabirimi oluşturmak için önerilen yöntemdir. Bu belge, geçmiş nedeniyle ve eski Xamarin.Mac projelerle çalışmak için yerinde bırakıldı. Daha fazla bilgi için lütfen bkz bizim [film şeritleri giriş](~/mac/platform/storyboards/index.md) belgeleri.

## <a name="overview"></a>Genel Bakış

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı kullanıcı arabirimi öğeleri erişiminiz ve çalışan bir geliştirici araçları *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve kullanıcı arabirimleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

.Xib dosya macOS tarafından oluşturulmuş ve korunan uygulamanızın kullanıcı arabirimi (örneğin, menüler, Windows, görünümler, etiketler, metin alanları) öğelerini grafik Xcode'nın arabirimi Oluşturucusu'nda tanımlamak için kullanılır.

[![Çalışan uygulama örneği](xib-images/intro01.png "çalışan uygulama örneği")](xib-images/intro01-large.png#lightbox)

Bu makalede, biz Xamarin.Mac uygulama .xib dosyaları ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri kapsar gibi ilk olarak, makalesi.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve kullanıcı Arabirimi öğeleri, C# sınıfları wire için kullanılır.


## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode ve arabirim Oluşturucu giriş

Xcode bir parçası olarak, Apple Kullanıcı Arabiriminizin Tasarımcısı'nda görsel olarak oluşturmanıza olanak tanıyan arabirimi Oluşturucu adlı bir araç oluşturdu. Xamarin.Mac fluently Objective-C kullanıcıların aynı araçları ile UI oluşturmanızı sağlayan arabirim Oluşturucusu ile tümleştirir.


### <a name="components-of-xcode"></a>Xcode bileşenleri

Bir .xib dosyasını Visual Studio'dan xcode'da Mac için açtığınızda, ile açılır bir **Proje Gezgini** sol taraftaki **arabirimi hiyerarşi** ve **arabirimi Düzenleyicisi** ortasında ve bir **özellikleri & yardımcı programları** sağdaki bölümü:

[![Xcode UI bileşenlerinin](xib-images/xcode03.png "Xcode UI bileşenleri")](xib-images/xcode03-large.png#lightbox)

Bir göz atalım hangi bunların her biri Xcode yapar ve Xamarin.Mac uygulamanız için arabirim oluşturmak için bunları nasıl kullanacağını bölümler.


#### <a name="project-navigation"></a>Proje gezinme

Xcode'da düzenlemek için bir .xib dosyayı açtığınızda, Mac için Visual Studio değişiklikleri kendisi ve Xcode arasında iletişim kurmak için arka planda bir Xcode projesi dosyası oluşturur. Xcode Mac için Visual Studio'ya geri döndüğünüzde, daha sonra bu proje için yapılan tüm değişiklikler Xamarin.Mac projenizi ile Visual Studio tarafından Mac için eşitlenir

**Proje Gezinti** bölümü sağlar, bu hale dosyaların tamamını arasında gezinmek _dolgusu_ Xcode projesi. Genellikle, yalnızca bu listeyi gibi .xib dosyalarında ilgilenecek **MainMenu.xib** ve **MainWindow.xib**.


#### <a name="interface-hierarchy"></a>Hiyerarşi arabirimi

**Arabirimi hiyerarşi** bölümü kolayca kullanıcı arabiriminin birkaç anahtar özellikleri gibi erişmenize olanak sağlar, **yer tutucuları** ve ana **penceresi**. Bu bölümde, bunlar etrafında hiyerarşide sürükleyerek yerleştirilir yolu, kullanıcı arabirimi ve Ayarla olun ayrı ayrı öğeler (görünümler) erişmek için de kullanabilirsiniz.


#### <a name="interface-editor"></a>Arabirim Düzenleyicisi

**Arabirimi Düzenleyicisi** bölümde yüzey üzerinde düzen grafik kullanıcı arabirimi. Öğelerden sürükleyin **Kitaplığı** bölümünü **özellikleri & yardımcı programları** tasarımınızı oluşturmak için bölüm. Tasarım yüzeyine kullanıcı arabirimi öğeleri (görünümler) eklemek, bunlar için eklenecektir **arabirimi hiyerarşi** bölüm içinde göründükleri sırada **arabirimi Düzenleyicisi**.


#### <a name="properties--utilities"></a>Özellikleri & yardımcı programları

**Özellikleri & yardımcı programları** bölüm biz ile çalışacaksınız iki ana bölümlere ayrılmıştır **özellikleri** (denetçiler olarak da adlandırılır) ve **Kitaplığı**:

![Özellik denetçisi](xib-images/xcode04.png "Özellik denetçisi")

Başlangıçta bu bölümde ancak neredeyse boş bir öğedeki seçerseniz **arabirimi Düzenleyicisi** veya **arabirimi hiyerarşi**, **özellikleri** bölüm doldurulmuş Verilen öğe ve ayarlayabileceğiniz özellikleri hakkında daha fazla bilgi ile.

İçinde **özellikleri** bölümünde, 8 farklı *denetçisi sekmeleri*, aşağıdaki çizimde gösterildiği gibi:

[![Tüm denetçiler genel bir bakış](xib-images/xcode05.png "tüm denetçiler genel bakış")](xib-images/xcode05-large.png#lightbox)

Soldan sağa, aşağıdaki sekmelerden şunlardır:

-   **Inspector dosya** – dosya denetçisi düzenlenmekte olan Xib dosyasının konumunu ve dosya adı gibi dosya bilgileri gösterir.
-   **Hızlı Yardım** – hızlı Yardım sekmesini Xcode'da seçili üzerinde temel Bağlamsal Yardım sağlar.
-   **Kimlik denetçisi** – kimlik denetçisi Seçili denetim/görünüm hakkında bilgi sağlar.
-   **Öznitelikleri denetçisi** – öznitelikleri denetçisi Seçili denetim/görünüm çeşitli özniteliklerini özelleştirmenizi sağlar.
-   **Boyut denetçisi** – boyutu denetçisi boyutu ve Seçili denetim/görünüm davranışını yeniden boyutlandırma denetlemenize olanak verir.
-   **Bağlantıları denetçisi** – bağlantıları denetçisi seçili denetimlerin çıkışı ve eylem bağlantıları gösterir. Biz çıkışlar ve eylemleri yalnızca birazdan inceleyeceğiz.
-   **Bağlamaları denetçisi** – bağlamaları denetçisi değerlerine veri modelleri için otomatik olarak bağlı şekilde denetimleri yapılandırmak sağlar.
-   **Görüntüleme etkileri denetçisi** – View etkileri denetçisi animasyonları gibi denetimleri etkileri belirtmenize olanak verir.

İçinde **Kitaplığı** bölümünde denetimleri bulabilir ve Designer'a grafik yerleştirin nesnelere yapı Kullanıcı Arabiriminizin:

![Kitaplık denetçisi örneği](xib-images/xcode06.png "kitaplığı denetçisi örneği")

Xcode IDE ve arabirim Oluşturucu sahibiyseniz, bir kullanıcı arabirimi oluşturmak için kullanma konumundaki bakalım.


## <a name="creating-and-maintaining-windows-in-xcode"></a>Oluşturma ve windows xcode'da koruma

Film şeritleri ile Xamarin.Mac uygulamanın kullanıcı arabirimini oluşturmak için tercih edilen yöntem olduğu (Lütfen bakın bizim [film şeritleri giriş](~/mac/platform/storyboards/index.md) daha fazla bilgi için) ve sonuç olarak, herhangi bir yeni projeye çalışmaya Xamarin.Mac olacaktır Film şeritleri varsayılan olarak kullanın.

Geçiş yapmak için bir .xib kullanarak UI tabanlı, aşağıdakileri yapın:

1. Mac için Visual Studio'yu açın ve yeni bir Xamarin.Mac proje başlatın.
2. İçinde **çözüm paneli**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...**
3. Seçin **Mac** > **Windows denetleyicisi**:

    ![Yeni bir pencere denetleyicisi ekleme](xib-images/setup00.png "yeni bir pencere denetleyicisi ekleme")
4. Girin `MainWindow` tıklayın ve ad için **yeni** düğmesi:

    ![Yeni bir ana pencere ekleyerek](xib-images/setup01.png "yeni bir ana penceresi ekleme")
5. Projeye tekrar sağ tıklayın ve seçin **Ekle** > **yeni dosya...**
6. Seçin **Mac** > **ana menü**:

    ![Yeni bir ana menü ekleme](xib-images/setup02.png "yeni bir ana menü ekleme")
7. Adı olarak bırakın `MainMenu` tıklatıp **yeni** düğmesi.
8. İçinde **çözüm paneli** seçin **Main.storyboard** dosya, sağ tıklatın ve seçin **kaldırmak**:

    ![Ana film şeridi seçme](xib-images/setup03.png "ana film şeridi seçme")
9. Kaldır iletişim kutusu, tıklatın **silmek** düğmesi:

    ![Silme onayı](xib-images/setup04.png "silme onayı")
10. İçinde **çözüm paneli**, çift **Info.plist** dosyayı düzenlemek için açın.
11. Seçin `MainMenu` gelen **ana arabirimi** açılır:

    [![Ana menü ayarı](xib-images/setup05.png "ana menüye ayarlama")](xib-images/setup05-large.png#lightbox)
12. İçinde **çözüm paneli**, çift **MainMenu.xib** dosyayı Xcode'nın arabirimi Oluşturucusu'nda düzenlemek için açın.
13. İçinde **kitaplığı denetçisi**, türü `object` arama alanına sonra yeni bir sürükleyin **nesne** tasarım yüzeyine:

    [![Ana menü düzenleme](xib-images/setup06.png "ana menüye düzenleme")](xib-images/setup06-large.png#lightbox)
14. İçinde **kimlik denetçisi**, girin `AppDelegate` için **sınıfı**:

    [![Uygulama temsilci seçme](xib-images/setup07.png "uygulama temsilci seçme")](xib-images/setup07-large.png#lightbox)
15. Seçin **dosyanın sahibi** gelen **arabirimi hiyerarşi**, geçiş **bağlantı denetçisi** ve temsilci atamak için bir satır sürükleyin `AppDelegate` **Nesne** yalnızca projeye eklendi:

    [![Uygulama temsilci bağlanma](xib-images/setup08.png "uygulama temsilci bağlanma")](xib-images/setup08-large.png#lightbox)
16. Değişiklikleri kaydetmek ve Mac için Visual Studio'ya geri dönün

Tüm bu değişikliklerle yerinde, düzenleme **AppDelegate.cs** dosya ve şu şekilde görünür yapın:

```csharp
using AppKit;
using Foundation;

namespace MacXib
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public MainWindowController mainWindowController { get; set; }

        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
            mainWindowController = new MainWindowController ();
            mainWindowController.Window.MakeKeyAndOrderFront (this);
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

İçinde tanımlanmış bir uygulamanın ana penceresi artık bir **.xib** otomatik olarak bir pencere denetleyicisi eklerken projeye dahil dosyası. Windows tasarımınızı düzenlemek için **çözüm paneli**, çift tıklayarak **MainWindow.xib** dosyası:

![MainWindow.xib dosya seçme](xib-images/edit01.png "MainWindow.xib dosya seçme")

Bu pencere tasarım Xcode'nın arabirimi Oluşturucusu'nda açın:

[![MainWindow.xib düzenleme](xib-images/edit02.png "MainWindow.xib düzenleme")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>Standart pencere iş akışı

Oluşturun ve Xamarin.Mac uygulamanızda çalışmak herhangi penceresi için işlem temelde aynıdır:

1. Projenize otomatik olarak eklenen varsayılan olmayan yeni windows için yeni bir pencere tanımı projeye ekleyin.
2. Xcode'nın arabirimi Oluşturucusu'nda düzenleme penceresi tasarımı .xib dosyasına çift tıklayarak açın.
3. Tüm gerekli pencere özellikleri kümesinde **özniteliği denetçisi** ve **boyutu denetçisi**.
4. Arabiriminizin oluşturmak ve bunları yapılandırmak için gerekli denetimleri sürükleyin **özniteliği denetçisi**.
5. Kullanım **boyutu denetçisi** yeniden boyutlandırma için kullanıcı Arabirimi öğeleri işlemek için.
6. Pencerenin kullanıcı Arabirimi öğeleri için C# kodu çıkışlar ve eylemleri aracılığıyla kullanıma sunar.
7. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.


### <a name="designing-a-window-layout"></a>Bir pencere düzeni tasarlama

Bir kullanıcı arabirimi arabirimi Oluşturucusu'nda uğradı yerleştirme işlemi eklediğiniz her öğe için aynıdır:

1. İstenen denetiminde Bul **kitaplığı denetçisi** ve içine sürükleyin **arabirimi Düzenleyicisi** ve konumlandırın.
2. Tüm gerekli pencere özellikleri kümesinde **özniteliği denetçisi**.
3. Kullanım **boyutu denetçisi** yeniden boyutlandırma için kullanıcı Arabirimi öğeleri işlemek için.
4. Özel bir sınıf kullanıyorsanız, kümesinde **kimlik denetçisi**.
5. C# kodu çıkışlar ve eylemleri aracılığıyla için kullanıcı Arabirimi öğeleri kullanıma sunar.
6. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

Örneğin:

1. Xcode'da, sürükleyin bir **düğme** gelen **kitaplığı bölüm**:

    [![Kitaplıktan bir düğmesini seçerek](xib-images/xcode07.png "kitaplıktan bir düğme seçme")](xib-images/xcode07-large.png#lightbox)
2. Düğmeyi üzerine bırakın **penceresi** içinde **arabirimi Düzenleyicisi**:

    [![Düğme penceresine ekleme](xib-images/xcode08.png "penceresine düğme ekleme")](xib-images/xcode08-large.png#lightbox)
3. Tıklayın **başlık** özelliğinde **özniteliği denetçisi** düğmenin başlık değiştirip `Click Me`:

    ![Düğme özniteliklerini ayarlama](xib-images/xcode09.png "düğmesi özniteliklerini ayarlama")
4. Sürükleme bir **etiket** gelen **kitaplığı bölüm**:

    [![Kitaplıkta bir etiket seçme](xib-images/xcode10.png "kitaplıkta bir etiket seçme")](xib-images/xcode10-large.png#lightbox)
5. Etiketin üzerine bırakma **penceresi** düğmesinin yanındaki **arabirimi Düzenleyicisi**:

    [![Bir etiket penceresine ekleme](xib-images/xcode11.png "pencereyi bir etiket ekleme")](xib-images/xcode11-large.png#lightbox)
6. Etiket sağ tutamacı alın ve pencerenin kenarına olana kadar sürükleyin:

    [![Etiket yeniden boyutlandırma](xib-images/xcode12.png "etiket yeniden boyutlandırma")](xib-images/xcode12-large.png#lightbox)
7. Halen seçili etiketin **arabirimi Düzenleyicisi**, geçiş **boyutu denetçisi**:

    ![Boyutu denetçisi seçme](xib-images/xcode13.png "boyutu denetçisi seçme")
8. İçinde **otomatik boyutlandırma kutusuna** tıklatın **Dim kırmızı köşeli ayraç** sağ ve **Dim kırmızı yatay ok** Center'da:

    ![Otomatik boyutlandırma özelliklerini düzenleme](xib-images/xcode14.png "otomatik boyutlandırma özelliklerini düzenleme")
9. Bu etiket büyütmek ve içinde çalışan uygulama penceresi boyutlandırıldığında küçültmek için uzatılır sağlar. **Kırmızı köşeli** ve üst ve sol tarafındaki **otomatik boyutlandırma kutusuna** kutusu bildirmek kendi verilen X ve Y konumları takılmış etiketi.
10. Değişikliklerinizi kaydetmek için kullanıcı arabirimi

Yeniden boyutlandırma ve taşıma geçici denetimleri gibi arabirim Oluşturucu, temel alan yararlı ek ipuçları sunar için fark etmiş [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Bu yönergeleri Mac kullanıcıları için tanıdık bir görünüm ve kullanımında içeren yüksek kaliteli uygulamalar oluşturmanıza yardımcı olur.

Bakarsanız **arabirimi hiyerarşi** bölümünde, Düzen ve hiyerarşi bizim kullanıcı arabirimi öğelerinin nasıl gösterileceğini dikkat edin:

![Arabirim hiyerarşisinde bir öğeyi seçerek](xib-images/xcode15.png "arabirimi hiyerarşi içinde bir öğe seçme")

Buradan, düzenlemek veya gerekirse kullanıcı Arabirimi öğeleri yeniden sıralamak için sürükleyin öğe seçebilirsiniz. Örneğin, bir kullanıcı Arabirimi öğesi başka bir öğe tarafından kapsanan, pencerenin en üst öğede yapmak için listenin altına sürükleyebilirsiniz.

Xamarin.Mac uygulamasında Windows ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Windows](~/mac/user-interface/window.md) belgeleri.


## <a name="exposing-ui-elements-to-c-code"></a>C# kod için UI öğeleri gösterme

Kullanıcı Arabiriminizin arabirimi Oluşturucu Görünüm ve yapısını düzenlemeyi tamamladığınızda, böylece C# kodundan erişilen UI öğelerini göstermek gerekir. Bunu yapmak için eylemleri ve çıkışlar kullanırsınız.


### <a name="setting-a-custom-main-window-controller"></a>Özel ana penceresi denetleyicisini ayarlama

Çıkışlar ve eylemleri C# kod için UI öğeleri göstermek için oluşturabilecek, Xamarin.Mac uygulama özel pencere denetleyicisi kullanılmasını gerekir.

Aşağıdakileri yapın:

1. Uygulamanın film şeridi Xcode'nın arabirimi Oluşturucu'da açın.
2. Seçin `NSWindowController` tasarım yüzeyine içinde.
3. Geçiş **kimlik denetçisi** görüntüleyin ve girin `WindowController` olarak **sınıf adı**:

    [![Sınıf adı düzenleme](xib-images/windowcontroller01.png "sınıf adını düzenleme")](xib-images/windowcontroller01-large.png#lightbox)
4. Değişikliklerinizi kaydetmek ve Visual Studio eşitlemek için Mac için geri dönün.
5. A **WindowController.cs** dosya projenizde eklenecek **çözüm paneli** Mac için Visual Studio'da:

    ![Visual Studio'da yeni sınıf adı Mac için](xib-images/windowcontroller02.png "yeni Mac sınıf adı Visual Studio'da")
6. Xcode'nın arabirimi Oluşturucu film şeridi yeniden açın.
7. **WindowController.h** dosya kullanılabilir olacaktır:

    [![Xcode eşleşen .h dosyasında](xib-images/windowcontroller03.png "Xcode eşleşen .h dosyasında")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Çıkışlar ve eylemleri

Bu nedenle çıkışlar ve Eylemler nedir? Geleneksel .NET kullanıcı arabirimi programlamada eklendiğinde kullanıcı arabiriminde bir denetim bir özellik olarak otomatik olarak sunulur. Şeyler farklı Mac içinde çalışır, yalnızca bir görünüme denetim ekleme, kod erişilebilir değil. Geliştirici açıkça kod için UI öğesi kullanıma gerekir. Sırayla Bunu yapmak için Apple bize iki seçenek sunar:

-  **Çıkışlar** – çıkışlar özelliklerine benzer. Bir çıkış denetimini oluşturan wire, kodunuza bir özelliği aracılığıyla gösterilir, olay işleyicileri ekleme gibi şeyler yapabilirsiniz yöntemlerini çağırın, vb. üzerinde.
-  **Eylemler** – Eylemler WPF komutu desende benzer. Örneğin, bir eylem denetim gerçekleştirildiğinde deyin bir düğmeye tıkladığınızda, Denetim kodunuzda otomatik olarak bir yöntemi çağırır. Birçok denetimleri aynı eylemi wire olduğundan Eylemler güçlü ve uygun değil.

Xcode'da, doğrudan kodda eklenen çıkışlar ve eylemleri *denetimini sürükleyerek*. Daha belirgin olarak bir çıkış veya eylem oluşturmak için istediğiniz bir çıkış veya eylem ekleme, basılı hangi denetim öğesi seçin, yani **denetim** düğmesini klavyede ve doğrudan kodunuza denetleyen sürükleyin.

Xamarin.Mac geliştiriciler için bu eylem ve çıkış oluşturmak istediğiniz C# dosyasına karşılık gelen Objective-C saplama dosyalarıyla sürükleyin anlamına gelir. Mac için Visual Studio'nun oluşturduğu adlı bir dosya **MainWindow.h** arabirimi Oluşturucusu'nu kullanmak için oluşturulan bir Xcode projesi dolgusu bir parçası olarak:

[![Xcode'da .h dosyası örneği](xib-images/xcode16.png "xcode'da .h dosyası örneği")](xib-images/xcode16-large.png#lightbox)

Bu saplama .h dosyası yansıtan **MainWindow.designer.cs** Xamarin.Mac projeye yeni bir otomatik olarak eklenen `NSWindow` oluşturulur. Bu dosya arabirimi Oluşturucu tarafından yapılan değişiklikleri eşitlemek için kullanılır ve böylece kullanıcı Arabirimi öğeleri için C# kodu gösterilen burada çıkışlar ve eylemleri oluşturacağız.


#### <a name="adding-an-outlet"></a>Prizine ekleme

Çıkışlar ve Eylemler nedir temel bilgilere sahip bir kullanıcı Arabirimi öğesi, C# kodunuzun kullanıma sunmak için prizine oluşturma sırasında bakalım.

Aşağıdakileri yapın:

1. Ekranın üst eldeki sağda köşesindeki Xcode'da tıklatın **çift daire** açmak için düğmeye **Yardımcısı Düzenleyicisi**:

    [![Yardımcısı Düzenleyicisi'ni seçerek](xib-images/outlet01.png "Yardımcısı Düzenleyici seçme")](xib-images/outlet01-large.png#lightbox)
2. Xcode ile bölünmüş görünüm moduna geçiş yapar **arabirimi Düzenleyicisi** bir tarafında ve **Kod düzenleyicisinde** diğer.
3. Xcode otomatik olarak çekilen bildirimi **MainWindowController.m** dosyasını **Kod düzenleyicisinde**, hatalı olduğu. Çıkışlar ve eylemleri yukarıda nelerdir bizim bilgi gelen unutmayın, biz gerek **MainWindow.h** seçili.
4. Üstündeki **Kod düzenleyicisinde** tıklayın **otomatik bağlantı** seçip **MainWindow.h** dosyası:

    [![Doğru .h dosyası seçme](xib-images/outlet02.png "doğru .h dosyası seçme")](xib-images/outlet02-large.png#lightbox)
5. Xcode şimdi seçili doğru dosya olmalıdır:

    [![Seçili doğru dosya](xib-images/outlet03.png "seçili doğru dosya")](xib-images/outlet03-large.png#lightbox)
6. **Son adım çok önemlidir!** Seçili doğru dosya yoksa, çıkışlar oluşturmak mümkün olmayacaktır ve C# yanlış sınıf için bir eylem veya bunlar sunulur!
7. İçinde **arabirimi Düzenleyicisi**, basılı **denetim** anahtar klavyede ve tıklatıp sürükleme oluşturduğumuz Yukarıdaki kod düzenleyicisinde etiketi yalnızca aşağıda `@interface MainWindow : NSWindow { }` kod:

    [![Yeni bir çıkış oluşturmak için sürükleme](xib-images/outlet04.png "yeni çıkışı oluşturmak için sürükleme")](xib-images/outlet04-large.png#lightbox)
8. Bir iletişim kutusu görüntülenir. Bırakın **bağlantı** çıkışı için ayarlayabilir ve girin `ClickedLabel` için **adı**:

    [![Çıkış özelliklerini ayarlama](xib-images/outlet05.png "çıkışı özelliklerini ayarlama")](xib-images/outlet05-large.png#lightbox)
9. Tıklatın **Bağlan** düğmesi çıkış oluşturmak için:

    ![Tamamlanan çıkışı](xib-images/outlet06.png "tamamlanmış çıkışı")
10. Değişiklikleri dosyaya kaydedin.


#### <a name="adding-an-action"></a>Bir eylem ekleme

Ardından, kullanıcı Arabirimi öğesi, C# kodunuzun olan bir kullanıcı etkileşimi kullanıma sunmak için bir eylem oluşturma sırasında bakalım.

Aşağıdakileri yapın:

1. Biz yine olduğundan emin olun **Yardımcısı Düzenleyicisi** ve **MainWindow.h** dosyasıdır görünür **Kod düzenleyicisinde**.
2. İçinde **arabirimi Düzenleyicisi**, basılı **denetim** anahtar klavyede ve oluşturduğumuz Yukarıdaki kod düzenleyicisinde düğmesini tıklatıp sürükleme yalnızca aşağıda `@property (assign) IBOutlet NSTextField *ClickedLabel;` kod:

    [![Bir eylem oluşturmak için sürükleme](xib-images/action01.png "bir eylem oluşturmak için sürükleme")](xib-images/action01-large.png#lightbox)
3. Değişiklik **bağlantı** eylem türü:

    [![Bir eylem türü seçin](xib-images/action02.png "bir eylem türü seçin")](xib-images/action02-large.png#lightbox)
4. Girin `ClickedButton` olarak **adı**:

    [![Eylem yapılandırma](xib-images/action03.png "eylemi yapılandırma")](xib-images/action03-large.png#lightbox)
5. Tıklatın **Bağlan** düğmesi eylemi oluşturmak için:

    ![Tamamlanan eylem](xib-images/action04.png "tamamlanan eylem")
6. Değişiklikleri dosyaya kaydedin.

Kullanıcı arabirimiyle kablolu yukarı ve C# kodundaki için Visual Studio'ya geri Mac için geçiş ve onu Xcode ve arabirim Builder değişikliklerden eşitleme izin verin.


### <a name="writing-the-code"></a>Kod yazma

Oluşturulan, kullanıcı arabirimi ve kodu çıkışlar ve eylemleri aracılığıyla kullanıma sunulan kullanıcı Arabirimi öğeleri ile programınızı hayata geçirin için kodu yazmaya hazırsınız. Örneğin, açık **MainWindow.cs** dosyasını çift tıklatarak düzenleme için **çözüm paneli**:

[![MainWindow.cs dosya](xib-images/code01.png "MainWindow.cs dosyası")](xib-images/code01-large.png#lightbox)

Ve aşağıdaki kodu ekleyin `MainWindow` sınıfı, yukarıda oluşturduğunuz örnek çıkış çalışmak için:

```csharp
private int numberOfTimesClicked = 0;
...

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Unutmayın `NSLabel` C# ' ta doğrudan adıyla erişilir kendi çıkışı Xcode'da oluşturduğunuz zaman, Xcode'da atanmış, bu durumda, adlı `ClickedLabel`. Normal bir C# sınıfı olduğu gibi herhangi bir yöntemi veya özelliği sunulan nesnenin erişebilir.

> [!IMPORTANT]
> Kullanmanız gereken `AwakeFromNib`, gibi başka bir yöntem yerine `Initialize`, çünkü `AwakeFromNib` denir _sonra_ işletim sistemi yüklenir ve .xib dosya kullanıcı arabiriminden örneği. Çalıştığınız .xib dosya önce etiket denetimi erişmek için tam olarak yüklenen örneği ve yapılandırıldı, elde edebileceğiniz bir `NullReferenceException` hata nedeniyle etiket denetimi henüz oluşturulmamış.

Ardından, aşağıdaki kısmi sınıfına ekleyin `MainWindow` sınıfı:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Bu kod, Xcode ve arabirim Builder oluşturulur ve kullanıcı düğmesine tıklar dilediğiniz zaman çağrılacağı eylem ekler.

Bazı kullanıcı Arabirimi öğeleri otomatik olarak Eylemler, örneğin, varsayılan menü çubuğunda öğeleri gibi yerleşik **Aç...**  menü öğesi (`openDocument:`). İçinde **çözüm paneli**, çift **AppDelegate.cs** dosyasını düzenlemek için açın ve aşağıdaki kodu ekleyin `DidFinishLaunching` yöntemi:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Burada anahtar satır `[Export ("openDocument:")]`, bunu belirten `NSMenu` , **AppDelegate** bir yönteme sahip `void OpenDialog (NSObject sender)` , yanıt verir `openDocument:` eylem.

Menülerle çalışma hakkında daha fazla bilgi için lütfen bkz bizim [menüleri](~/mac/user-interface/menu.md) belgeleri.


## <a name="synchronizing-changes-with-xcode"></a>Xcode ile değişiklikler eşitleniyor

Xcode Mac için Visual Studio'ya geri döndüğünüzde, Xcode'da yaptığınız tüm değişiklikler Xamarin.Mac projenizi ile otomatik olarak eşitlenir.

Seçerseniz **MainWindow.designer.cs** içinde **çözüm paneli** nasıl bizim çıkışı ve eylem yukarı bizim C# kodunda kablolu görmeye devam:

[![Xcode ile değişiklikler eşitleniyor](xib-images/sync01.png "Xcode ile değişiklikler eşitleniyor")](xib-images/sync01-large.png#lightbox)

Bildirim nasıl iki tanımlarında **MainWindow.designer.cs** dosyası:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Tanımlarıyla hizaya **MainWindow.h** Xcode dosyasında:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Gördüğünüz gibi Mac için Visual Studio .h dosyada yapılan değişiklikler dinler ve ilgili değişiklikleri otomatik olarak eşitleyen **. designer.cs** uygulamanıza göstermek için dosya. Ayrıca, fark edebilirsiniz **MainWindow.designer.cs** bir parçalı sınıf, böylelikle Mac için Visual Studio değiştirmek yok **MainWindow.cs** hangi sınıfa yapmış olduğunuz değişiklikleri üzerine.

Normalde hiçbir zaman açmanız gerekecek **MainWindow.designer.cs** yalnızca eğitim amacıyla kendiniz burada verildi.

> [!IMPORTANT]
> Çoğu durumda, Mac için Visual Studio otomatik olarak Xcode'da yapılan değişiklikleri görmek ve bunları Xamarin.Mac projenizi eşitleyin. Eşitleme otomatik olarak gerçekleşmez kapalı örneği içinde geri Xcode ve bunları Visual Studio'ya geri Mac için yeniden geçin. Bu, normalde bir eşitleme döngüsü tetiklersiniz.


## <a name="adding-a-new-window-to-a-project"></a>Bir projeye yeni bir pencere ekleme

Ana belge penceresine yanı sıra Xamarin.Mac uygulama kullanıcıya Tercihler veya Inspector paneller gibi diğer windows türlerini görüntülemek gerekebilir. Projeniz için yeni bir pencere eklerken, her zaman kullanmalısınız **Cocoa penceresi denetleyicisiyle** seçeneği bu daha kolay dosya penceresi .xib yükleme işleminin getirir.

Yeni bir pencere eklemek için aşağıdakileri yapın:

1. İçinde **çözüm paneli**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** .
2. Yeni dosya iletişim kutusunda seçin **Xamarin.Mac** > **Cocoa penceresi denetleyicisiyle**:

    ![Yeni bir pencere denetleyicisi ekleme](xib-images/new01.png "yeni bir pencere denetleyicisi ekleme")
3. Girin `PreferencesWindow` için **adı** tıklatıp **yeni** düğmesi.
4. Çift **PreferencesWindow.xib** dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın:

    [![Xcode penceresinde düzenleme](xib-images/new02.png "Xcode penceresinde düzenleme")](xib-images/new02-large.png#lightbox)
5. Arabiriminizin tasarım:

    [![Windows düzeni tasarlama](xib-images/new03.png "windows düzeni tasarlama")](xib-images/new03-large.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Aşağıdaki kodu ekleyin **AppDelegate.cs** yeni penceresini görüntülemek için:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

`var preferences = new PreferencesWindowController ();` Satır pencerenin .xib dosyasından yükler ve onu Şişir penceresi denetleyicisi yeni bir örneğini oluşturur. `preferences.Window.MakeKeyAndOrderFront (this);` Satırı kullanıcıya yeni pencerede görüntüler.

Kodu çalıştırmak ve seçmek **tercihleri...**  gelen **uygulama menüsü**, penceresi görüntülenir:

![Örnek uygulamayı çalıştıran](xib-images/new04.png "örnek uygulamayı çalıştırma")

Xamarin.Mac uygulamasında Windows ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Windows](~/mac/user-interface/window.md) belgeleri.


## <a name="adding-a-new-view-to-a-project"></a>Bir projeye yeni bir görünüm ekleme

Birkaç, daha kolay yönetilebilir .xib dosyalarıyla, pencerenin tasarım bölünme daha kolay olduğunda zamanlar vardır. Örneğin, bir araç çubuğu öğesi seçerken ana penceresinin içeriğini değiştirme gibi bir **Tercihler penceresi** veya içeriği yanıt olarak takas bir **kaynağı listesi** seçim.

Projeniz için yeni bir görünüm eklerken, her zaman kullanmalısınız **Cocoa görünüm denetleyicisiyle** seçeneği bu daha kolay dosya görünümü .xib yükleme işleminin getirir.

Yeni bir görünüm eklemek için aşağıdakileri yapın:

1. İçinde **çözüm paneli**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** .
2. Yeni dosya iletişim kutusunda seçin **Xamarin.Mac** > **Cocoa görünüm denetleyicisiyle**:

    ![Yeni bir görünüm ekleme](xib-images/view01.png "yeni bir görünüm ekleme")
3. Girin `SubviewTable` için **adı** tıklatıp **yeni** düğmesi.
4. Çift **SubviewTable.xib** arabirimi Oluşturucusu'nda düzenlemek için açın ve kullanıcı arabirimini tasarlamak için dosya:

    [![Xcode'da yeni görünüm tasarlama](xib-images/view02.png "xcode'da yeni görünümü tasarlama")](xib-images/view02-large.png#lightbox)
5. Tüm gerekli eylemleri ve çıkışlar bağlayın.
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Sonraki düzenleme **SubviewTable.cs** ve aşağıdaki kodu ekleyin **AwakeFromNib** dosya yüklendiğinde yeni görünüm doldurmak için:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));
    DataSource.Sort ("Title", true);

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);

    // Auto select the first row
    ProductTable.SelectRow (0, false);
}
```

Enum hangi görünüm şu anda görünen yüklenmekte olan izlemek için projeye ekleyin. Örneğin, **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Görünüm harcayan ve görüntülemeden pencerenin .xib dosyasını düzenleyin. Ekleme bir **Özel Görünüm** , davranacağı kapsayıcı olarak görünüm için C# kodu ve prizine kendisine adlı sunmaya tarafından belleğe yüklenmiş bir kez `ViewContainer`:

[![Gerekli çıkışı oluşturma](xib-images/view03.png "gerekli çıkışı oluşturma")](xib-images/view03-large.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Ardından, yeni görünüm görüntüleme penceresi .cs dosyasını düzenleyin (örneğin, **MainWindow.cs**) ve aşağıdaki kodu ekleyin:

```csharp
private SubviewType ViewType = SubviewType.None;
private NSViewController SubviewController = null;
private NSView Subview = null;
...

private void DisplaySubview(NSViewController controller, SubviewType type) {

    // Is this view already displayed?
    if (ViewType == type) return;

    // Is there a view already being displayed?
    if (Subview != null) {
        // Yes, remove it from the view
        Subview.RemoveFromSuperview ();

        // Release memory
        Subview = null;
        SubviewController = null;
    }

    // Save values
    ViewType = type;
    SubviewController = controller;
    Subview = controller.View;

    // Define frame and display
    Subview.Frame = new CGRect (0, 0, ViewContainer.Frame.Width, ViewContainer.Frame.Height);
    ViewContainer.AddSubview (Subview);
}
```

Ne zaman yeni bir görünüm gösterileceğini ihtiyacımız yüklenen pencerenin kapsayıcısında .xib dosyasından ( **Özel Görünüm** tıklar), varolan herhangi bir görünümü kaldırarak ve onu için yeni bir takas bu kodu tanıtıcısı. Bu nedenle, onu ekranından kaldırırsa görüntülenen bir görünüm zaten görmek için görünüyor. Sonraki geçirildi görünümü alır (bir görünüm denetleyicisinden yüklendiği gibi) içerik alanında uyacak şekilde yeniden boyutlandırır ve görüntü içeriğini ekler.

Yeni bir görüntülemek için aşağıdaki kodu kullanın:

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Bu yeni görünüm için Görünüm görüntülenecek denetleyicisi yeni bir örneğini oluşturur, türünü (projeye eklenen enum tarafından belirtildiği şekilde) ayarlar ve kullandığı `DisplaySubview` gerçekten görüntülemek için pencerenin sınıfına eklediğiniz yöntemi. Örneğin:

[![Örnek uygulamayı çalıştıran](xib-images/view04.png "örnek uygulamayı çalıştırma")](xib-images/view04-large.png#lightbox)

Xamarin.Mac uygulamasında Windows ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Windows](~/mac/user-interface/window.md) ve [iletişim kutularını](~/mac/user-interface/dialog.md) belgeleri.


## <a name="summary"></a>Özet

Bu makalede Xamarin.Mac uygulamasında .xib dosyalarıyla çalışma ayrıntılı bir bakış sürdü. Farklı bağlantı türleri ve .xib dosyaları kullanımları, uygulamanızın kullanıcı arabirimi oluşturmak için dosyaları Xcode'da 's oluşturmak ve .xib korumak için nasıl oluşturucusu ve C# kodunda .xib dosyalarıyla çalışmak nasıl arabirim gördük.


## <a name="related-links"></a>İlgili bağlantılar

- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Menüler](~/mac/user-interface/menu.md)
- [İletişim Kutuları](~/mac/user-interface/dialog.md)
- [İmajlarla çalışma](~/mac/app-fundamentals/image.md)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
