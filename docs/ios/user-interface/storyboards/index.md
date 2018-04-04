---
title: Film şeritleri giriş
description: Film şeridi görsel görünümünü ve uygulamanızın akış ' dir. Xamarin uygulaması ekranınızı görsel olarak tasarlamak ve görünümlere erişim film şeritleri, yararlanmak Xamarin.iOS uygulamaları izin vermek için bir tasarımcı kullanıma sunulan denetleyicileri ve C# ile daha fazla denetim için segues.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 647bd7d339dc56978752f7ab29de30cf8acb7e07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-storyboards"></a>Film şeritleri giriş

_Film şeridi görsel görünümünü ve uygulamanızın akış ' dir. Xamarin uygulaması ekranınızı görsel olarak tasarlamak ve görünümlere erişim film şeritleri, yararlanmak Xamarin.iOS uygulamaları izin vermek için bir tasarımcı kullanıma sunulan denetleyicileri ve C# ile daha fazla denetim için segues._

Bu kılavuz size hangi film şeridi anlatılmıştır olduğu ve bazı – Segues gibi anahtar bileşenlerini inceleyin. Biz nasıl film şeritleri oluşturulabilir ve kullanılan, en göreceğiz ve avantajları sahip oldukları için bir geliştirici.

Film şeridi dosya biçimi görsel bir sunumdur UI bir iOS uygulamasının Apple tarafından sunulmadan önce geliştiriciler XIB dosyaları her görünüm denetleyicisi için oluşturulan ve her görünüm arasında gezinti el ile programlanmış.  Film şeridi kullanarak görünüm denetleyicileri ve tasarım yüzeyi üzerlerinde arasında gezinti tanımlamak Geliştirici olanak tanır ve uygulamanın kullanıcı arabirimi WYSIWYG düzenleme sunar.

Film şeridi oluşturulan, açılabilir ve Xamarin iOS Tasarımcısı ile düzenlenemez. Bu kılavuz olur de izlenecek tasarımcının Gezinti programlamak için C# kullanırken, film şeritleri oluşturmak için nasıl kullanılacağı.


## <a name="requirements"></a>Gereksinimler

Film şeritleri Xamarin iş yükleri yüklü Visual Studio 2015 ve 2017 veya Mac için Visual Studio tasarımcısında iOS ile kullanılabilir.

## <a name="what-is-a-storyboard"></a>Film şeridi nedir?

Film şeridi bir uygulamadaki tüm ekranlar visual gösterimidir. Her Sahne temsil eden ile gerisinde, bir dizi içeren bir *View Controller* ve kendi *görünümleri*. Bu görünümler nesneleri içerebilir ve [denetimleri](~/ios/user-interface/controls/index.md) uygulamanız ile etkileşim kurmak kullanıcı izin. Bu koleksiyon görünümleri ve denetimlerin (veya *subviews*) olarak bilinen bir *içerik görünümü hiyerarşi*. Planda bağlı tarafından görünüm denetleyicileri arasında bir geçiş temsil eden nesneler ü. Bu normal olarak ilk görünümündeki bir nesne ve bağlantı görünümü arasında segue oluşturarak elde edilir. Tasarım yüzeyine ilişkilerde aşağıdaki görüntü gösterilmiştir:

 [![](images/storyboardsview.png "Tasarım yüzeyine ilişkileri bu görüntüde gösterilmiştir")](images/storyboardsview.png#lightbox)

Gösterildiği gibi film şeridi her, planda zaten işlenmiş içerikle düzenleme ve bunları arasındaki bağlantıları gösterir.  Bu noktada, planda hakkında bir İphone'da konuşurken bir varsaymak güvenli olduğunu belirtmeye değer olan *Sahne* şeridinde birine eşit olan *ekran* cihazda içerik. Ancak, birden çok Sahne aynı anda – görünür olması mümkündür iPad ile Örneğin, bir Popover görünümü denetleyicisi kullanarak.

Film şeritleri kullanarak uygulamanızın kullanıcı Arabirimi, özellikle Xamarin kullanırken oluşturmak için birçok avantajı vardır. İlk olarak, UI görsel gösterimi olarak dahil olmak üzere tüm nesneleri – ise [özel denetimler](~/ios/user-interface/designer/ios-designable-controls-overview.md) – tasarım zamanında işlenir. Bu derleme veya görünümünü ve akış görselleştirebilirsiniz Uygulamanızı dağıtmadan önce anlamına gelir. Yukarıdaki resmi gibi ele alın. Biz Hızlı Bakış yüzey kaç planda var olan tasarım, her görünüm düzenini ve her şeyi nasıl ilişkili olduğunu anlayabilirsiniz. Film şeritleri ne kadar güçlü yapan budur.

Olaylar, özellikle Tasarımcısı iOS kullanırken film şeritleri ile daha kolay yönetilebilir. Çoğu kullanıcı Arabirimi denetimlerini olası olayları listesini özellikleri defterinde gerekir. Olay işleyicisi buraya eklenen ve görünüm denetleyicileri sınıfı kısmi yönteminde tamamlandı...

Film şeridi içeriğini bir XML dosyası olarak depolanır. AT derleme zamanı, tüm `.storyboard` nibs bilinen ikili dosyalara derlenmiş dosyalar. Çalışma zamanında bu nibs başlatılamadı ve yeni görünümler oluşturmak için örneği.

## <a name="segues"></a>Segues

A *Segue*, veya *ü nesne*, iOS geliştirme planda arasında bir geçiş temsil etmek için kullanılır. Bir segue oluşturmak için basılı **Ctrl** anahtar ve tıklatıp sürükleme bir Sahne alanından diğerine. Biz bizim fareyi sürüklediğinizde, aşağıdaki resimde gösterildiği gibi segue burada götürür belirten mavi bağlayıcı görüntülenir:

 [![](images/createsegue.png "Bu görüntüde gösterildiği gibi segue burada götürür belirten mavi bağlayıcı görüntülenir")](images/createsegue.png#lightbox)

Fare yukarı üzerinde bize bizim segue eylemini seçin izin vererek bir menüsü görüntülenir. Görüntülere benzeyebilir: 

**Ön iOS 8 ve boyut sınıfları**:

[![](images/segue1.png "Boyutu sınıfları olmadan eylem ü açılır")](images/segue1.png#lightbox)

**Boyutu sınıfları ve Uyarlamalı Segues kullanırken**:

[![](images/16new.png "Boyut sınıfına sahip eylem ü açılır")](images/16new.png#lightbox)

> [!IMPORTANT]
> Windows sanal makineniz için VMWare kullanıyorsanız, CTRL tuşuna basıp tıklayın olarak eşlenmiş _sağ_ fare düğmesini varsayılan olarak. Bir Segue oluşturmak için klavye tercihlerinizi aracılığıyla düzenleyin **Tercihler** > **klavye ve fare** > **Fare kısayolları** ve yeniden eşleme, **İkincil düğme** aşağıda gösterildiği gibi:
> 
> [![](images/image22.png "Klavye ve fare tercih ayarları")](images/image22.png#lightbox)
> 
> Artık normal olarak, görünüm denetleyicileri arasında segue ekleyebilmek için olması gerekir.

Geçişler, yeni bir görünüm denetleyicisi kullanıcıya nasıl sunulur ve nasıl film şeridi alanındaki diğer görünüm denetleyicileriyle etkileşim giving her denetime farklı tür vardır. Bunlar, aşağıda açıklanmıştır. Ayrıca, bir alt kümesi için bir özel geçiş uygulamak için bir segue nesnesi de mümkündür:

-  **Göster / itme** – push ü Gezinti yığını görünüm denetleyicisini ekler. Bu, anında iletme kaynaklanan görünüm denetleyicisini yığınına eklenen görünüm denetleyicisini aynı gezinti denetleyicisine parçası olduğunu varsayar. Bu aynı şeyi yapar `pushViewController` ve veriler ekranında arasındaki bazı ilişki olduğunda genellikle kullanılır. Push kullanarak ü geri düğmesini ve başlığı her görünüm yığında detaya gitme hiyerarşisini görüntüleme gezinmeyi sağlayan eklenmiş olan bir gezinti çubuğu sahip olmanın lüks sağlar.
-  **Kalıcı** – bir kalıcı segue herhangi iki görünüm denetleyicileri gösterildikten animasyonlu geçişin seçeneğiyle projenizdeki arasında bir ilişki oluşturun. Alt Görünüm denetleyicisini tamamen görünüme duruma, üst görünüm denetleyicisini soyutlamaması. Bir itme ü, da bize geri düğmesini ekler; ne zaman kalıcı bir kullanarak ü `DismissViewController` önceki görünüm denetleyiciye dönebilmek için kullanılması gerekir.
-  **Özel** – her özel segue öğesinin bir alt kümesi oluşturulabilir ` UIStoryboardSegue`.
-  **Bırakma** – bırakma bir ü geri İtme veya kalıcı arasında gezinmek için kullanılan – Örneğin, kalıcı olarak sunulan görünüm denetleyicisini kapatarak ü. Bu, ek olarak yalnızca biri bırakma, ancak bir dizi itme ve kalıcı segues ve tek bir gezinti hiyerarşinizdeki birden çok adımı eylemi bırakma geri dönün. Bırakmayla nasıl kullanılacağını anlamak için okuyun iOS ü [oluşturma, bırakma Segues](https://developer.xamarin.com/recipes/ios/general/storyboard/unwind_segue/) tarif.
-  **Sourceless** – sourceless segue ilk görünüm denetleyicisini içeren Sahne gösterir ve bu nedenle, kullanıcının görüntüleyebileceği önce görürsünüz. Aşağıda gösterilen segue tarafından temsil edilen:  

    [![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Uyarlamalı ü türleri

 iOS 8 sunulan [boyutu sınıfları](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) tüm iOS cihazları için bir kullanıcı Arabirimi oluşturmak geliştiriciler etkinleştirilmesi, tüm kullanılabilir ekran boyutlarına çalışmak bir iOS film şeridi dosya izin vermek için. Varsayılan olarak, tüm yeni Xamarin.iOS uygulamaları boyutu sınıfları kullanır. Eski Proje boyutu sınıftan kullanmak için başvurmak [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) Kılavuzu. 
 
Boyutu sınıflarını kullanarak herhangi bir uygulama aynı zamanda yeni kullanacağı [ *Uyarlamalı Segues*](~/ios/user-interface/storyboards/unified-storyboards.md). Boyutu sınıfları kullanırken, biz doğrudan wether belirtme olmayan olduğunu unutmayın bir iPhone veya iPad kullanıyoruz. Diğer bir deyişle, bakılmaksızın aynı çalışmak için sahip ne kadar Gayrimenkul her zaman görünür bir UI oluşturuyoruz. Ortam yüksek ve en iyi şekilde nasıl içerik sunmak belirleme Uyarlamalı Segues çalışın. Uyarlamalı Segues aşağıda gösterilmektedir: 

[![](images/adaptivesegue.png "Uyarlamalı Segues açılır")](images/adaptivesegue.png#lightbox)

|Ü|Açıklama|
|--- |--- |
|Göster|Bu çok benzer bir itme ü ancak ekran içeriğini dikkate alır.|
|Ayrıntıları Göster|Uygulama bir ana ve ayrıntı görünümünde (örneğin, bir bölme görünüm denetleyicisinde iPad) görüntüler, içeriği ayrıntı görünümü değiştirir. Uygulamanın yalnızca ana veya ayrıntı gösterirse, içerik görünümü denetleyicisi yığınının tepesindeki yerini alır.|
|Sunum|Bu kalıcı segue benzer ve sunu ve geçiş stilleri için seçilmesine izin verir.|
|Popover sunu|Bu içerik popover gösterir|

### <a name="transferring-data-with-segues"></a>İle veri aktarma Segues

Bir segue yararları geçişleri ile sona ermez. Görünüm denetleyicileri arasında veri aktarımını yönetmek için de kullanılabilir. Bu geçersiz kılma tarafından sağlanır `PrepareForSegue` ilk görünüm denetleyicisini ve kendisini veri işleme yöntemi. Segue tetiklendiğinde – Örneğin, bir düğme basma ile – uygulama yeni görünüm denetleyicisini hazırlama olanağı sağlayarak bu yöntemi çağırır *önce* herhangi Gezinti oluşur. Aşağıdaki kod gelen [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) örnek, bu gösterir: 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
}
```

Bu örnekte, `PrepareForSegue` segue kullanıcı tarafından tetiklendiğinde yöntemi çağrılır. Biz öncelikle 'alıcı' görünümü denetleyici örneği oluşturun ve bu View Controller segue's hedef olarak ayarlamanız gerekir. Bu, aşağıdaki kod satırı tarafından gerçekleştirilir:

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Yöntemi şimdi özelliklerini ayarlamak için gönderebilen `DestinationViewController`. Bu örnekte, biz bu avantajlarından adlı bir liste geçirerek önlemlerin `PhoneNumbers` için `CallHistoryController` ve aynı ada sahip bir nesneye atama:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Geçiş tamamlandıktan sonra kullanıcının göreceği `CallHistoryController` doldurulan listesi.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Film şeridi film şeridi olmayan projeye ekleme

Bazen daha önce film şeridi olmayan dosya film şeridi eklemeniz gerekebilir. Visual Studio'da Mac için bir kez bunu aşağıdaki adımları izleyerek basitleştirilmesi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Göz atarak yeni bir film şeridi dosya oluşturma **Dosya > Yeni Dosya > iOS > Film şeridi**aşağıda gösterildiği gibi: 
    
    [![](images/new-storyboard-xs.png "Yeni dosya iletişim kutusu")](images/new-storyboard-xs.png#lightbox)

2. Film şeridi adınızı ekleme **ana arabirimi** bölümünü **Info.plist**, aşağıda gösterildiği gibi:
    
    [![](images/infoplist.png "Info.plist Düzenleyicisi")](images/infoplist.png#lightbox)
    
    Bunu ilk görünüm denetleyiciye başlatmasını eşdeğeri yapar `FinishedLaunching` yöntemi uygulama temsilci içinde. Bu seçenek kümesi ile uygulamayı bir pencere (aşağıya bakın) başlatır, ana film şeridi yükler ve film şeridi'nın ilk View Controller (sourceless Segue yanında bir) örneği olarak atar `RootViewController` özelliği penceresinin ve ardından yapar pencerenin ekranda görünür.

3. İçinde `AppDelegate`, varsayılan geçersiz kılma `Window` penceresi özelliği uygulamak için aşağıdaki kod ile yöntemi:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Projeye sağ tıklayarak yeni bir film şeridi dosya oluşturma **Ekle > Yeni Dosya > iOS > boş film şeridi**aşağıda gösterildiği gibi: 
    
    [![](images/new-storyboard-vs.png "Yeni öğe iletişim kutusu")](images/new-storyboard-vs.png#lightbox)

2. Film şeridi adınızı ekleme **ana arabirimi** iOS aşağıda gösterildiği gibi uygulama bölümü:
    
    [![](images/ios-app.png "Info.plist Düzenleyicisi")](images/ios-app.png#lightbox)
    
    Bunu ilk görünüm denetleyiciye başlatmasını eşdeğeri yapar `FinishedLaunching` yöntemi uygulama temsilci içinde. Bu seçenek kümesi ile uygulamayı bir pencere (aşağıya bakın) başlatır, ana film şeridi yükler ve film şeridi'nın ilk View Controller (sourceless Segue yanında bir) örneği olarak atar `RootViewController` özelliği penceresinin ve ardından yapar pencerenin ekranda görünür.

3. İçinde `AppDelegate`, varsayılan geçersiz kılma `Window` penceresi özelliği uygulamak için aşağıdaki kod ile yöntemi:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Film şeridi iOS Tasarımcısı ile oluşturma

Film şeridi, Mac ve Visual Studio için Visual Studio ile sorunsuz bir şekilde tümleştirilmiştir iOS için Xamarin Tasarımcısı'nı kullanarak oluşturulabilir.

Film şeritleri oluşturmak için iOS Tasarımcısı ile çalışmaya başlamak için izleyin [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) Kılavuzu. Bu kılavuzda görünüm Segues ve denetimlerinizi olaylarına nasıl ele alınacağını kullanarak denetleyicileri arasında gezinme inceleyeceksiniz.

## <a name="instantiate-storyboards-manually"></a>Film şeritleri el ile örneği

Film şeritleri film şeridi tek tek görünüm denetleyicileri hala kullanılarak oluşturulabilir ancak tek tek XIB dosyaları projenizdeki tamamen Değiştir `Storyboard.InstantiateViewController`.

Bazen uygulamaları tasarımcı tarafından sağlanan yerleşik film şeridi geçişleri ile işlenemez özel gereksinimleri vardır. Aynı düğmesi, bir uygulamanın geçerli durumunu bağlı olarak farklı ekranlar başlatan bir uygulama oluşturmak için olsaydı Örneğin, biz geçişi kendisini programı ve görünüm denetleyicileri el ile oluşturmak isteyebilirsiniz.

Aşağıdaki ekran görüntüsünde, iki görünüm denetleyicisinde bizim tasarım yüzeyi hiçbir aralarında ü gösterir. Sonraki bölümde, geçiş kodu nasıl ayarlanabilir size yol gösterecek.

 [![](images/viewcontrollerspink.png "Bu ekran görüntüsünde iki görünüm denetleyicisinde tasarım yüzeyine hiçbir aralarında ü gösterir")](images/viewcontrollerspink.png#lightbox)

1. Ekleme bir _iPhone film şeridi boş_ var olan bir proje proje için:
    
    [![](images/add-storyboard1.png "Film şeridi ekleme")](images/add-storyboard1.png#lightbox)

2. Yeni oluşturulan şeridinde açmak için çift tıklayın ve yeni bir ekleme **Gezinti denetleyicisi** tasarım yüzeyine. Gezinti denetleyicisi olduğu gibi gelen bir kök görünümü denetleyicisiyle aşağıda gösterildiği gibi varsayılan olarak UI olmayan:

    [![](images/uinavigationcontroller.png "Görünüm denetleyicileriyle Segues")](images/uinavigationcontroller.png#lightbox)

3. Seçin _View Controller_ altındaki siyah çubuğunda tıklatarak. Tasarımcıda 's **özelliği paneli**altında **kimlik** biz benzersiz bir kimliği yanı sıra özel bir sınıf için Görünüm denetleyicisini belirtebilirsiniz. Ayarlama **sınıf adı** ve **film şeridi kimliği** için `MainViewController`.

    [![](images/identitypanelnew.png "Özel bir sınıf belirtin")](images/identitypanelnew.png#lightbox)

4. Daha sonra biz bizim film şeridi görünümü denetleyicilerinden örneği oluşturmak ihtiyacınız olacak ve bizim kodda başvurmak görsel taslak haline getirme kimliği kullanır. Film şeridi kimliği eşleşecek şekilde geri yükleme kimliği ayarlama durumu geri yüklenmesi gerekiyorsa, görünüm denetleyicisini doğru yeniden sağlar.

5. Şu anda yalnızca bir görünüm denetleyicisi sahibiz. Başka bir görünüm denetleyicisi tasarım yüzeyine sürükleyin. İçinde **özelliği paneli**, sınıf ve film şeridi Kimliğine kimliği altında ayarlamak `PinkViewController`aşağıda gösterildiği gibi:

    [![](images/pinkvcnew.png "Özellik paneli")](images/pinkvcnew.png#lightbox)
    
    IDE görünümü denetleyicileri için özel bu sınıfları oluşturur. Bunlar görüntülenebilir **çözüm paneli**aşağıdaki ekran görüntüsünde gösterildiği gibi:
    
    [![](images/solution-pad.png "Çözüm paneli")](images/solution-pad.png#lightbox)

6. İçinde `PinkViewController`, denetleyicinin çerçeve merkezi tıklayarak görünümü seçin. Özellikler panelinde görünüm altında değiştirme **arka plan** macenta için:
    
    [![](images/pinkcontroller.png "Arka plan rengini ayarlama")](images/pinkcontroller.png#lightbox)

7. Son olarak, bir düğmesinden sürükleyin **araç** üzerine `MainViewController`. İsteğe bağlı olarak özellikleri defterinde adını verin `PinkButton` ve başlık aşağıda gösterildiği gibi GoToPink:

    [![](images/pinkbutton.png "Düğme adı ayarlama")](images/pinkbutton.png#lightbox)

Film şeridi tamamlandı, ancak biz projeyi şimdi dağıtırsanız, biz boş bir ekran alırsınız. Hala bizim film şeridi kullanın ve ilk görünüm olarak hizmet verecek bir kök görünümü denetimi ayarlamak için IDE bildirmek ihtiyacımız olmasıdır. Normalde bu bizim proje seçenekleri yukarıda gösterildiği gibi yapılabilir. Ancak bu örnekte, şunları yapacağız aşağıdakileri ekleyerek kodda, aynı sonucu elde **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

Çok fazla kod olan, ancak yalnızca birkaç satır bilginiz. İlk olarak, biz bizim film şeridi kaydetmek **AppDelegate** film şeridi'nın adını geçirerek **MainStoryboard**. Ardından, biz çağırarak film şeridi ilk görünüm denetleyicisinden örneği oluşturmak için uygulama söyleyin `InstantiateInitialViewController` bizim şeridinde ve bu görünüm denetleyicisini bizim uygulamanın kök görünümü denetleyicisi olarak ayarlar. Bu yöntem kullanıcı görür, ilk ekran belirler ve bu görünüm denetleyicisini yeni bir örneğini oluşturur.

Bildirim IDE oluşturdu çözüm bölmesinde bir `MainViewcontroller.cs` sınıfı ve onun `corresponding designer.cs` zaman eklediğimiz sınıf adı 4. adımda özellikleri paneli için. Bu sınıf bir taban sınıf içeren özel bir oluşturucu oluşturulmuş görebiliriz:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Tasarımcı kullanarak film şeridi oluştururken, IDE otomatik olarak Ekle [[Kaydet]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) özniteliği en üstündeki `designer.cs` sınıfı ve belirtilen film şeridi kimliği aynı olan bir dize tanımlayıcı geçirin önceki adımı. Bu C# film şeridi ilgili Sahne bağlar.

, Varolan bir sınıfa eklemek istediğiniz belirli bir noktada **değil** Tasarımcısı'nda oluşturulmuş. Bu durumda, bu sınıf normal olarak kaydetmeniz:

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

Sınıflar ve yöntemler kaydetme hakkında daha fazla bilgi için başvurmak [türü kayıt](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) belgeleri.

Bu sınıftaki son adım wire düğmesi ve pembe görünüm denetleyicisini geçişi. Örneği `PinkViewController` film şeridi görünümünden; daha sonra biz push programlayacaksınız ile ü `PushViewController`örnek kod gösterildiği gibi:

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

Uygulamayı çalıştırmak 2 ekran uygulama üretir:

![](images/finishedstoryboard.png "Çalışma ekranları örnek uygulaması")

## <a name="conditional-segues"></a>Koşullu Segues

Genellikle, bir görünüm denetleyicisinden diğerine taşıma belirli bir koşulun bağlıdır. Biz basit oturum açma ekranı yapıyorsanız Örneğin, biz yalnızca sonraki ekrana taşımak istersiniz *varsa* kullanıcı adı ve parola doğrulanmış.

Sonraki örnekte Yukarıdaki örnek parola alanı ekleyeceğiz. Kullanıcı yalnızca erişmek açabilirler *PinkViewController* doğru parolayı girin, aksi takdirde bir hata görüntülenir.

Başlamadan önce 1-8'i yukarıdaki adımları izleyin. Bu adımda biz bizim film şeridi oluşturma, kullanıcı arabirimimizi oluşturmak ve kullanmak için hangi görünüm denetleyicisini bizim uygulama temsilci söyleyin başlamadan RootViewController olduğu gibi.

1. Şimdi, şimdi bizim UI'yi oluşturmak ve listelenen ek görünümler ekleme `MainViewController` aşağıdaki ekran görüntüsünde gibi görünmesi için:

    - UITextField
        - Ad: PasswordTextField
        - Yer tutucu: 'gizli parolayı girin'
    - UILabel
        - Metin: ' hata: yanlış parola. Değil geçirdiğiniz!'
        - Renk: kırmızı
        - Hizalama: merkezi
        - Satırlar: 2
        - 'Hidden' onay kutusu işaretli 
        
    [![](images/passwordvc.png "Merkezi satırları")](images/passwordvc.png#lightbox)
    
2. Pembe git düğmesi ve görünüm denetleyici Ctrl-düşürmesini tarafından arasında Segue oluşturma *PinkButton* için *PinkViewController*, seçerek **anında** üzerinde fare yukarı . 

3. Üzerinde Segue'ı tıklatın ve bu verin *tanımlayıcısı* `SegueToPink`:

    [![](images/namesegue.png "Üzerinde Segue'ı tıklatın ve tanımlayıcı SegueToPink verin")](images/namesegue.png#lightbox)  
    

4. Son olarak, aşağıdaki ShouldPerformSegue yöntemine ekleyin `MainViewController` sınıfı:

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

Bu kodda segueIdentifier eşleştirdik bizim `SegueToPink` ü biz bir koşul; sınayabilir şekilde bu durumda geçerli bir parola. Bizim koşul döndürürse `true`, Segue gerçekleştirir ve sunacaktır `PinkViewController`. Varsa `false`, yeni görünüm denetleyicisini değil sunulur.

Biz bu yaklaşım tüm Segue bu görünüm denetleyicisindeki ShouldPerformSegue yöntemi segueIdentifier bağımsız değişkeni denetleyerek uygulayabilirsiniz. Bir Segue tanımlayıcısını – yalnızca bu durumda sahibiz `SegueToPink`.

Storyboards.Conditional çözümde başvurmak [el ile film şeritleri örnek](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) çalışan bir örnek için.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Film şeridi başvuruları kullanma

Film şeridi başvuru büyük ve karmaşık bir film şeridi tasarım alabilir ve özgün başvurulan küçük film şeritleri içine bölün olanak tanır, böylece kaldırma karmaşıklık kaldırma ve elde edilen tek tek yaparak daha kolay tasarım film şeritleri ve korur.

Ayrıca, bir film şeridi başvuru sağlayabilir bir _bağlantı_ aynı film şeridi veya farklı bir üzerinde belirli bir görünüm içindeki başka bir görünüm için.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Bir dış film şeridi başvurma

Bir dış film şeridi başvuru eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **Ekle** > **yeni dosya...**   >  **iOS** > **film şeridi**. Girin bir **adı** tıklatın ve yeni film şeridi için **yeni** düğmesi:
    
    [![](images/ref01.png "Yeni dosya iletişim kutusu")](images/ref01.png#lightbox)
    
2. Normalde olur ve değişikliklerinizi kaydetmek gibi yeni film şeridi'nın planda düzenini tasarım: 
    
    [![](images/ref02.png "Yeni Sahne düzeni")](images/ref02.png#lightbox)
    
3. Başvuru iOS Tasarımcısı eklemek zorunda kalacaklarını film şeridi açın.

4. Sürükleme bir **film şeridi başvuru** gelen **araç** tasarım yüzeyine: 
    
    [![](images/ref03.png "Film şeridi başvurusu")](images/ref03.png#lightbox)
    
5. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, adını seçin **film şeridi** yukarıda oluşturduğunuz: 

    [![](images/ref04.png "Pencere öğesi sekmesi")](images/ref04.png#lightbox)
    
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme gibi) var olan bir görünüm üzerinde denetim tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** yeni oluşturduğunuz: 

    [![](images/ref05.png "Bir segue oluşturma")](images/ref05.png#lightbox) 
    
7. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](images/ref06.png "Segue tamamlamak için Göster seçme")](images/ref06.png#lightbox) 
    
8. Film şeridi için yaptığınız değişiklikleri kaydedin.

Uygulamayı çalıştırın ve Kimden, dış film şeridi Başvurusu'nda belirtilen film şeridi ilk görünüm denetleyicisinden Segue oluşturulan UI öğesinde kullanıcı tıkladığında görüntülenir.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Bir dış film şeridi belirli Sahne başvurma

Belirli bir Sahne başvuru eklemek için bir dış film şeridi (ve değil ilk View Controller), aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, dış film şeridi düzenlemek üzere açmak için çift tıklayın.

2. Yeni bir görünüm ekleyin ve yerleşimi normal olarak tasarlamanız: 

    [![](images/ref07.png "Yeni Sahne düzeni")](images/ref07.png#lightbox)
    
3. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, girin bir **film şeridi kimliği** yeni Sahne ait görünüm denetleyicisi için: 

    [![](images/ref08.png "Yeni planda görünümü denetleyici için bir film şeridi kimliği girin")](images/ref08.png#lightbox)
    
3. Başvuru iOS Tasarımcısı eklemek zorunda kalacaklarını film şeridi açın.

4. Sürükleme bir **film şeridi başvuru** gelen **araç** tasarım yüzeyine: 

    [![](images/ref03.png "Film şeridi başvurusu")](images/ref03.png#lightbox)
    
5. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, adını seçin **film şeridi** ve **başvuru kimliği** (film şeridi kimliği), Yukarıda oluşturduğunuz Sahne: 

    [![](images/ref09.png "Pencere öğesi sekmesi ")](images/ref09.png#lightbox)
    
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme gibi) var olan bir görünüm üzerinde denetim tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** yeni oluşturduğunuz: 

    [![](images/ref10.png "Bir segue oluşturma")](images/ref10.png#lightbox) 
    
7. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](images/ref06.png "Segue tamamlamak için Göster seçme")](images/ref06.png#lightbox) 
    
8. Film şeridi için yaptığınız değişiklikleri kaydedin.

Uygulamayı çalıştırın ve kullanıcı olduğunda tıkladığında, Segue, Sahne ile oluşturulan UI öğesi üzerinde verilen **film şeridi kimliği** dış film şeridi Başvurusu'nda belirtilen film şeridi görüntülenir.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Aynı film şeridi belirli Sahne başvurma

Belirli bir Sahne aynı film şeridi başvuru eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, film şeridi düzenlemek üzere açmak için çift tıklayın.

2. Yeni bir görünüm ekleyin ve yerleşimi normal olarak tasarlamanız: 

    [![](images/ref11.png "Yeni Sahne düzeni")](images/ref11.png#lightbox)

3. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, girin bir **film şeridi kimliği** yeni Sahne ait görünüm denetleyicisi için: 

    [![](images/ref12.png "Pencere öğesi sekmesi")](images/ref12.png#lightbox)
    
3. Sürükleme bir **film şeridi başvuru** gelen **araç** tasarım yüzeyine: 

    [![](images/ref03.png "Film şeridi başvurusu")](images/ref03.png#lightbox)
    
5. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**seçin **başvuru kimliği** (film şeridi kimliği), yukarıda oluşturduğunuz Sahne: 

    [![](images/ref13.png "Pencere öğesi sekmesi")](images/ref13.png#lightbox)
    
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme gibi) var olan bir görünüm üzerinde denetim tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** yeni oluşturduğunuz: 

    [![](images/ref14.png "Bir segue oluşturma")](images/ref14.png#lightbox) 
    
7. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](images/ref06.png "Segue tamamlamak için Göster seçme")](images/ref06.png#lightbox) 
    
8. Film şeridi için yaptığınız değişiklikleri kaydedin.

Uygulamayı çalıştırın ve kullanıcı olduğunda tıkladığında, Segue, Sahne ile oluşturulan UI öğesi üzerinde verilen **film şeridi kimliği** film şeridi Başvurusu'nda belirtilen aynı film şeridi görüntülenir.

## <a name="summary"></a>Özet

Bu makalede film şeritleri ve nasıl iOS uygulamaları geliştirme faydalı olabilir kavramını sunmaktadır. Planda, görünüm denetleyicileri, görünümleri ve görünüm hiyerarşileri ve planda Segues farklı türleri ile birlikte nasıl bağlantılı anlatılmaktadır.  Bu ayrıca başlatmasını görünüm denetleyicileri el ile bir film şeridi ve oluşturma koşullu Segues araştırır.



## <a name="related-links"></a>İlgili bağlantılar

- [El ile film şeridi (örnek)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [İOS Tasarımcısı giriş](~/ios/user-interface/designer/introduction.md)
- [Film şeritleri için dönüştürme](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard sınıf başvurusu](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
