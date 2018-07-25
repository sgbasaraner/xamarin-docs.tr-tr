---
title: Xamarin.iOS, görsel taslaklara giriş
description: Bu belge, Xamarin.iOS film şeritleri tanıtılmaktadır. Bir kullanıcı arabirimi tanımlamak için bir görsel taslak nasıl kullanıldığını açıklar, etkinleştirilir ve iOS Tasarımcısı görsel taslak dosyalarını düzenlemek için kullanma.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: bd8fee1b8f1941203bb0e6f00e261cbfbbccc9a7
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242348"
---
# <a name="introduction-to-storyboards-in-xamarinios"></a>Xamarin.iOS, görsel taslaklara giriş

Bu kılavuzda biz hangi görsel taslak açıklayacak olduğunu ve anahtar bileşenleri – geçişler Uyarlamasız gibi bazılarını inceleyin. Nasıl görsel Taslaklar oluşturulabilir ve kullanılan en inceleyeceğiz ve sağladığı avantajları bir geliştirici için vardır.

Görsel taslak dosyası biçimi görsel bir temsili kullanıcı arabiriminin bir iOS uygulamasının Apple tarafından sunulmadan önce geliştiriciler XIB dosyaları her görünüm denetleyicisi için oluşturulan ve her görünüm arasında gezinti el ile programlanır.  Görsel taslak kullanarak görünüm denetleyicisi hem tasarım yüzeyinde bunlar arasında gezinti tanımlayın Geliştirici sağlar ve WYSIWYG düzenleme uygulamanın kullanıcı arabirimini sunar.

Görsel taslak oluşturulabilir, açtığınız ve Xamarin iOS Designer ile düzenlenemez. Bu kılavuz size ayrıca izlenecek yol Tasarımcı Gezinti programlamak için C# kullanırken, film şeridi oluşturmak için nasıl kullanılacağını


## <a name="requirements"></a>Gereksinimler

Görsel Taslaklar yüklü Xamarin iş yükleri ile iOS Designer, Mac için Visual Studio veya Visual Studio 2015 ve 2017 ile kullanılabilir.

## <a name="what-is-a-storyboard"></a>Görsel taslak nedir?

Görsel Taslak, uygulamadaki tüm ekranların visual gösterimidir. Her Sahne temsil eden ile planda bir dizi içeren bir *görünüm denetleyicisi* ve kendi *görünümleri*. Bu görünümler nesneleri içerebilir ve [denetimleri](~/ios/user-interface/controls/index.md) kullanıcınızın uygulamayla etkileşmesi sağlayacaktır. Bu görünüm ve koleksiyonu (veya *subviews*) olarak bilinen bir *içerik hiyerarşisini görüntüle*. Sahneleri bağlı Görünüm denetleyicileri arasında bir geçiş temsil eden nesneler tarafından ü. Bu normalde bir segue başlangıç görünümü bir nesne ile bağlantı görünümü oluşturarak elde edilir. Tasarım yüzeyinde ilişkileri, aşağıdaki görüntüde gösterilmiştir:

 [![](images/storyboardsview.png "Tasarım yüzeyinde ilişkileri bu görüntüde gösterilmiştir.")](images/storyboardsview.png#lightbox)

Gösterildiği gibi film şeridi her, planda zaten işlenmiş içerikle düzenleme ve bunlar arasındaki bağlantıları gösterir.  Bu noktada, perde hakkında İphone'da konuşurken tek varsaymak güvenli olduğunu hatalarının ayıklanabileceğini belirtmekte yarar *Sahne* şeridinde birine eşit olup *ekran* cihazda içerik. Ancak, bir iPad birden çok Sahne tek bir seferde – görünür olması mümkündür, örneğin, Popover görünüm denetleyicisi kullanma.

Xamarin kullanırken özellikle uygulamanızın kullanıcı Arabirimi oluşturmak için görsel Taslaklar kullanarak birçok avantajı vardır. İlk olarak, kullanıcı Arabirimi, görsel bir temsili olarak dahil olmak üzere tüm nesneleri – olduğu [özel denetimler](~/ios/user-interface/designer/ios-designable-controls-overview.md) – tasarım zamanında işlenir. Bu derleme veya görünümünü ve akış görselleştirebilirsiniz Uygulamanızı dağıtmadan önce anlamına gelir. Yukarıdaki görüntüde, örneğin yararlanın. Biz hızlı bir bakış yüzey kaç sahneler var olan tasarım, her görünüm düzenini ve her şeyi nasıl ilişkili olduğunu anlayabilirsiniz. Görsel Taslaklar güçlü kılan budur.

İOS Designer kullanırken özellikle görsel Taslaklar ile daha kolay yönetilebilir olaylardır. Çoğu UI denetimleri, Özellikler panelinde olası olayların bir listesi gerekir. Olay işleyicisi buraya eklenen ve görünüm denetleyicileri sınıfı kısmi bir yöntemin içinde tamamlandı...

Görsel taslak içeriğini bir XML dosyası olarak depolanır. AT derleme zamanı, tüm `.storyboard` dosyaları nibs bilinen ikili dosyalarına derlenir. Çalışma zamanında bu nibs başlatılır ve yeni görünümler oluşturmak için örneği.

## <a name="segues"></a>Etkinleştirilir

A *Segue*, veya *ü nesne*, iOS geliştirme görünümler arasında geçiş temsil etmek için kullanılır. Bir segue oluşturmak için basılı **Ctrl** anahtarı ve başka bir click-sürükleyin bir Sahne gelen. Size sunduğumuz fare sürükleme sırasında aşağıdaki resimde gösterildiği gibi segue burada önünü açacak belirten mavi bir bağlayıcı görünür:

 [![](images/createsegue.png "Bu görüntüde gösterildiği gibi segue burada önünü açacak belirten mavi bir bağlayıcı görünür")](images/createsegue.png#lightbox)

Fare yukarı üzerinde bize bizim segue eylemi seçin izin vererek bir menü görünür. Aşağıdaki görüntülerin benzeyebilir: 

**Öncesi iOS 8 ve boyut sınıfları**:

[![](images/segue1.png "Boyut sınıflarını olmadan eylem ü açılır")](images/segue1.png#lightbox)

**Boyut sınıflarını ve Uyarlamalı etkinleştirilir kullanırken**:

[![](images/16new.png "Boyut sınıflarını eylem ü açılan listesi")](images/16new.png#lightbox)

> [!IMPORTANT]
> Windows sanal makineniz için VMWare kullanıyorsanız, Ctrl tuşunu olarak eşleştirilir _sağ_ varsayılan olarak, fare düğmesi. Bir Segue oluşturmak için klavye tercihlerinizi aracılığıyla Düzenle **tercihleri** > **klavye ve fare** > **Fare kısayolları** ve yeniden eşleme, **İkincil düğme** aşağıda gösterildiği gibi:
> 
> [![](images/image22.png "Klavye ve fare tercih ayarlarını")](images/image22.png#lightbox)
> 
> Artık normal olarak, görünüm denetleyicileri arasında bir segue eklemeniz mümkün olması gerekir.

Geçişler, her kullanıcı için yeni bir görünüm denetleyicisi nasıl sunulur ve nasıl diğer görsel taslak görünüm denetleyicileri ile etkileşim giving denetime farklı türde vardır. Bunlar aşağıda açıklanmıştır. Ayrıca, özel bir geçiş uygulamak için segue nesne alt sınıfı için de mümkündür:

-  **Göster / anında iletme** – bir anında iletme ü görünüm denetleyicisi için gezinme yığınında ekler. Bu anında iletme kaynaklanan görünüm denetleyicisi yığına eklenen görünüm denetleyicisi olarak aynı gezinti denetleyicisi bir parçası olduğunu varsayar. Bu aynı şeyi yapar `pushViewController` ve veriler ekranında arasındaki bazı ilişki olduğunda genel olarak kullanılır. Anında iletme kullanarak ü bir geri düğmesi ve başlık yığında Gezinti görünüm hiyerarşisi aracılığıyla detaya izin vererek her bir görünüme eklenmiş olan bir gezinti çubuğu yaşama lüks sağlar.
-  **Kalıcı** – bir kalıcı segue gösterilen bir animasyonlu geçiş seçeneği ile projenizdeki herhangi iki görünüm denetleyicileri arasında bir ilişki oluşturun. Alt Görünüm denetleyicisi tamamen görünüme duruma, üst görünüm denetleyicisi gizlememeniz. Bu bizim için geri düğmesi ekler bir anında iletme ü; ne zaman kalıcı bir kullanarak ü `DismissViewController` önceki görünüm denetleyiciye döndürmek için kullanılmalıdır.
-  **Özel** – herhangi bir özel öğesinin segue oluşturulabilir ` UIStoryboardSegue`.
-  **Geriye doğru izleme** geriye doğru izleme bir ü geri İtme veya kalıcı arasında gezinmek için kullanılan – Örneğin, kalıcı olarak sunulan görünüm denetleyicisi kapatarak ü. Bu, ek olarak yalnızca biri geriye doğru izleyebilirsiniz, ancak anında iletme ve kalıcı bir dizi etkinleştirilir ve tek bir gezinti hiyerarşinizdeki birden çok adım eylem bırakma geri dönün. Bir bırakma nasıl kullanılacağını anlamak için okuyun iOS ü [oluşturma, bırakma etkinleştirilir](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue) tarif.
-  **Sourceless** – ilk görünüm denetleyicisini içeren sahneye sourceless segue gösterir ve bu nedenle, kullanıcının görüntüleyebileceği önce görürsünüz. Aşağıda gösterilen segue tarafından temsil edilir:  

    [![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Uyarlamalı ü türleri

 iOS 8 sunulan [boyut sınıfları](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) geliştiricilerin tüm iOS cihazları için bir kullanıcı Arabirimi oluşturmak tüm kullanılabilir ekran boyutlarıyla çalışacak şekilde bir iOS görsel taslak dosyası izin vermek için. Varsayılan olarak, tüm yeni Xamarin.iOS uygulama boyut sınıfları kullanır. Boyut sınıfları daha eski bir projesinden kullanmak için başvurmak [birleşik görsel taslaklara giriş](~/ios/user-interface/storyboards/unified-storyboards.md) Kılavuzu. 
 
Boyut sınıflarını kullanan uygulamalar ayrıca yeni kullanacağı [ *Uyarlamalı etkinleştirilir*](~/ios/user-interface/storyboards/unified-storyboards.md). Boyut sınıfları kullanırken, biz doğrudan Unified belirtme değil olduğunu unutmayın. iPhone veya iPad kullanıyoruz. Diğer bir deyişle aynı şekilde çalışmak için sahip ne kadar Emlak bağımsız olarak her zaman görünür bir UI oluşturuyoruz. Ortam yüksek ve en iyi şekilde nasıl içerik sunmak belirleme Uyarlamalı geçişler Uyarlamasız çalışın. Uyarlamalı etkinleştirilir aşağıda gösterilmektedir: 

[![](images/adaptivesegue.png "Uyarlamalı etkinleştirilir açılır")](images/adaptivesegue.png#lightbox)

|Ü|Açıklama|
|--- |--- |
|Show|Bu çok benzer bir anında iletme ü ancak ekran içeriğini hesaba katar.|
|Ayrıntıları Göster|İçerik, uygulamada bir ana ve ayrıntı görünümü (örneğin, ipad'de bir Bölünmüş Görünüm denetleyicisi) görüntülenirse, ayrıntı görünümü yerini alır. Uygulamayı yalnızca ana veya ayrıntı görüntülerse, içerik görünümü denetleyicisi yığın üstüne yerini alır.|
|Sunum|Bu kalıcı segue için benzer ve sunu ve geçiş stilleri seçime olanak sağlar.|
|Popover sunu|Bu içerik bir popover sunar|

### <a name="transferring-data-with-segues"></a>İle veri aktarımı etkinleştirilir

Bir segue avantajlarını geçişleri ile sona ermez. Görünüm denetleyicileri arasında veri aktarımını yönetmek için de kullanılabilir. Bu geçersiz kılma tarafından sağlanır `PrepareForSegue` başlangıç görünümü denetleyicisi ve kendimize veri işleme yöntemi. Segue tetiklendiğinde – örneğin düğmesine basma ile – uygulama yeni görünüm denetleyicisini hazırlamak için bir fırsat sağlar, bu yöntemi çağırır *önce* herhangi bir gezinti gerçekleşir. Aşağıdaki kod öğesinden [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) örneği, bu gösterir: 


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

Bu örnekte, `PrepareForSegue` segue kullanıcı tarafından tetiklendiğinde yöntemi çağrılır. İlk 'alma' görünüm denetleyicisi örneği oluşturun ve bu görünüm denetleyicisi segue'nın hedefi olarak ayarlamak sahibiz. Bu, aşağıdaki kod satırı tarafından gerçekleştirilir:

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Yöntemi artık özellikleri ayarlamak için özelliğine sahiptir `DestinationViewController`. Bu örnekte Biz bu avantajı olarak adlandırılan bir liste geçirerek önlemlerin `PhoneNumbers` için `CallHistoryController` ve aynı ada sahip bir nesneye atama:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Geçiş tamamlandıktan sonra kullanıcının göreceği `CallHistoryController` ile doldurulmuş bir listesi.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Görsel taslağı bir görsel taslak olmayan projeye ekleniyor

Bazen bir görsel taslak önceden görsel taslak olmayan dosya eklemeniz gerekebilir. Visual Studio'da Mac için bir kez bunu aşağıdaki adımları izleyerek basitleştirilmesi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Göz atarak yeni bir görsel taslak dosyası oluşturma **Dosya > Yeni Dosya > iOS > Film şeridi**, aşağıda gösterildiği gibi: 
    
    [![](images/new-storyboard-xs.png "Yeni dosya iletişim kutusu")](images/new-storyboard-xs.png#lightbox)

2. Görsel taslak adınızı ekleme **ana arabirimi** bölümünü **Info.plist**, aşağıda gösterildiği gibi:
    
    [![](images/infoplist.png "Info.plist Düzenleyicisi")](images/infoplist.png#lightbox)
    
    Bunu, ilk görünüm denetleyicisi örnekleme eşit yapar `FinishedLaunching` yöntemi içinde uygulama temsilcisi. Bu seçenek kümesi ile uygulama bir pencere (aşağıya bakın) başlatır, ana görsel taslak yükler ve şeridinin ilk görünüm denetleyicisi (sourceless Segue yanında bir) örneği olarak atar `RootViewController` özellik penceresi ve ardından yapar pencereyi ekranda görünür.

3. İçinde `AppDelegate`, varsayılanın `Window` penceresi özelliği uygulamak için aşağıdaki kod ile yöntemi:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Projeye sağ tıklayarak yeni bir görsel taslak dosyası oluşturma **Ekle > Yeni Dosya > iOS > boş görsel taslak**, aşağıda gösterildiği gibi: 
    
    [![](images/new-storyboard-vs.png "Yeni öğe iletişim kutusu")](images/new-storyboard-vs.png#lightbox)

2. Görsel taslak adınızı ekleme **ana arabirimi** aşağıda gösterildiği gibi uygulama iOS bölümünü:
    
    [![](images/ios-app.png "Info.plist Düzenleyicisi")](images/ios-app.png#lightbox)
    
    Bunu, ilk görünüm denetleyicisi örnekleme eşit yapar `FinishedLaunching` yöntemi içinde uygulama temsilcisi. Bu seçenek kümesi ile uygulama bir pencere (aşağıya bakın) başlatır, ana görsel taslak yükler ve şeridinin ilk görünüm denetleyicisi (sourceless Segue yanında bir) örneği olarak atar `RootViewController` özellik penceresi ve ardından yapar pencereyi ekranda görünür.

3. İçinde `AppDelegate`, varsayılanın `Window` penceresi özelliği uygulamak için aşağıdaki kod ile yöntemi:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>İOS Designer ile görsel taslak oluşturma

Görsel Taslak, iOS, Mac ve Visual Studio için Visual Studio ile sorunsuz bir şekilde tümleştirilmiştir Xamarin Tasarımcısı'nı kullanarak oluşturulabilir.

Görsel taslakları oluşturmak için iOS Designer'ı kullanmaya başlamak için izleyin [iOS çok ekranlı Hello](~/ios/get-started/hello-ios-multiscreen/index.md) Kılavuzu. Bu izlenecek yolda geçişler Uyarlamasız ve denetimlerinizi olaylarına nasıl ele alınacağını kullanarak görünüm denetleyicileri arasında gezinti inceleyeceksiniz.

## <a name="instantiate-storyboards-manually"></a>Görsel Taslaklar el ile örneği

Görsel taslakları görsel taslak tek görünüm denetleyicileri kullanılarak yine de oluşturulabilir ancak tek XIB dosyaları projenizde, tamamen değiştirin `Storyboard.InstantiateViewController`.

Bazen uygulamalar, tasarımcı tarafından sağlanan yerleşik film şeridi geçişleri ile işlenemez özel gereksinimleri vardır. Aynı düğmesi, bir uygulamanın geçerli durumuna bağlı olarak farklı ekranları başlatan bir uygulama oluşturmak için olsaydık Örneğin, size geçişi kendimize programı ve görünüm denetleyicileri el ile oluşturmak isteyebilirsiniz.

Aşağıdaki ekran görüntüsünde, aralarında iki görünüm denetleyicisi bizim tasarım yüzeyinde olmayan ü gösterir. Sonraki bölümde bu geçiş kodunu nasıl ayarlanabilir yol gösterir.

 [![](images/viewcontrollerspink.png "Bu ekran görüntüsünde, aralarında iki görünüm denetleyicisi tasarım yüzeyinde olmayan ü gösterilmektedir.")](images/viewcontrollerspink.png#lightbox)

1. Ekleme bir _boş iPhone görsel taslağı_ mevcut projeler için:
    
    [![](images/add-storyboard1.png "Görsel taslak ekleme")](images/add-storyboard1.png#lightbox)

2. Yeni oluşturulan şeridinde açmak için çift tıklayın ve yeni bir **Gezinti denetleyicisi** tasarım yüzeyine bırakın. Gezinti denetleyicisi olduğundan bu gelen kök görünüm denetleyicisi ile aşağıda gösterildiği gibi varsayılan olarak UI olmayan:

    [![](images/uinavigationcontroller.png "Görünüm denetleyicileri ile etkinleştirilir")](images/uinavigationcontroller.png#lightbox)

3. Seçin _görünüm denetleyicisi_ altında siyah çubuğuna tıklayın. İçinde tasarımcının **özellik paneli**altında **kimlik** biz benzersiz bir kimliği yanı sıra özel bir sınıf için Görünüm denetleyicisi belirtebilirsiniz. Ayarlama **sınıf adı** ve **film şeridi kimliği** için `MainViewController`.

    [![](images/identitypanelnew.png "Özel bir sınıf belirtin")](images/identitypanelnew.png#lightbox)

4. Daha sonra bizim görsel taslak görünüm denetleyicilerini örneklemek ihtiyacımız ve bunları bizim kodda başvurmak için görsel taslak kimliği kullanır. Görsel taslak kimliği eşleştirmek için geri yükleme kimliği olarak ayarlanması durumunda geri yüklenmesi gerekiyorsa görünüm denetleyicisi doğru şekilde yeniden sağlar.

5. Şu anda yalnızca bir görünüm denetleyicisi sahibiz. Başka bir görünüm denetleyicisi tasarım yüzeyine sürükleyin. İçinde **özellik paneli**, kimliği altında sınıf ve görsel taslak Kimliğine ayarlayın `PinkViewController`, aşağıda gösterildiği gibi:

    [![](images/pinkvcnew.png "Özellik paneli")](images/pinkvcnew.png#lightbox)
    
    IDE görünüm denetleyicileri bu özel sınıflar oluşturur. Bunlar görüntülenebilir **çözüm bölmesi**aşağıdaki ekran görüntüsünde gösterildiği gibi:
    
    [![](images/solution-pad.png "Çözüm bölmesi")](images/solution-pad.png#lightbox)

6. İçinde `PinkViewController`, denetleyicinin çerçeve ortasına doğru tıklayarak görünümü seçin. Özellikler panelinde altında Görünüm değiştirme **arka plan** Eflatun için:
    
    [![](images/pinkcontroller.png "Arka plan rengini ayarla")](images/pinkcontroller.png#lightbox)

7. Son olarak, düğme sürükleyin **araç kutusu** üzerine `MainViewController`. İsteğe bağlı olarak özellikler panelinde adını verin `PinkButton` ve başlık aşağıda gösterildiği gibi GoToPink:

    [![](images/pinkbutton.png "Düğme adı ayarlayın")](images/pinkbutton.png#lightbox)

Görsel taslak tamamlandı, ancak biz proje artık dağıtırsanız, boş bir ekran elde edersiniz. Yine de bizim film şeridi kullanın ve ilk görünüm olarak görev yapacak bir kök görünüm denetleyicisini ayarlamak için IDE bildirmek ihtiyacımız olmasıdır. Normalde bu proje seçeneklerimiz, yukarıda gösterildiği gibi yapılabilir. Ancak bu örnekte olacak aşağıdakileri ekleyerek kodda, aynı sonucu elde **AppDelegate**:

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

Bu kod, ancak yalnızca birkaç satır bilginiz. İlk olarak biz bizim görsel taslakla kaydetmeden **AppDelegate** şeridinin adı geçirerek **MainStoryboard**. Ardından, biz çağırarak bir film şeridini başlangıç görünümü denetleyicisi oluşturmak için uygulama söyleyin `InstantiateInitialViewController` bizim şeridinde ve bu görünüm denetleyicisi bizim uygulamanın kök görünüm denetleyicisi ayarladık. Bu yöntem, kullanıcının gördüğü, ilk ekran belirler ve bu görünüm denetleyicisi yeni bir örneğini oluşturur.

IDE oluşturduğunuz çözüm bölmesinde bildirimi bir `MainViewcontroller.cs` sınıfı ve kendi `corresponding designer.cs` zaman ekledik sınıf adı için özellikler panelini 4. adımda. Bu sınıf bir taban sınıfı içeren özel bir oluşturucu oluşturulan görebiliriz:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Tasarımcı kullanarak görsel taslak oluşturma, IDE otomatik olarak ekler [[kaydetme]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) özniteliği en üstündeki `designer.cs` sınıfı ve görsel taslak belirtilen kimliği aynı olan bir dize tanımlayıcısı geçirin önceki adımı. Bu C# film şeridinde ilgili sahneye bağlayacaksınız.

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

Sınıflar ve yöntemler kaydetme ile ilgili daha fazla bilgi için [türü Kaydedici](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) belgeleri.

Bu sınıf son adımında, wire düğmesi ve pembe görünüm denetleyicisi geçiş. Örneği `PinkViewController` Film şeridinden; ardından, size bir anında iletme programlayacaksınız ile ü `PushViewController`gösterildiği örnek kod gibi:

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

Uygulamayı çalıştıran 2 ekranlı bir uygulama oluşturur:

![](images/finishedstoryboard.png "Örnek uygulama ekranları çalıştırın")

## <a name="conditional-segues"></a>Koşullu etkinleştirilir

Genellikle, bir görünüm denetleyicisinden diğerine taşıma sırasında belirli bir koşulu bağlıdır. Biz basit bir oturum açma ekranını yapıyorsanız Örneğin, biz yalnızca sonraki ekrana taşınır istersiniz *varsa* kullanıcı adı ve parola doğrulandı.

Sonraki örnekte Yukarıdaki örnek parola alanı ekleyeceğiz. Kullanıcı yalnızca erişmek mümkün olacaktır *PinkViewController* doğru parola girmeleri, aksi takdirde bir hata görüntülenir.

Başlamadan önce 1 – 8 yukarıdaki adımları izleyin. Bu adımda biz bizim görsel taslak oluşturma, kullanıcı Arabirimimizi oluşturmak ve kullanmak için hangi görünüm denetleyicisi bizim uygulama temsilci başlayın RootViewController olduğu gibi.

1. Şimdi, şimdi kullanıcı Arabirimimizi yapı ve listelenen ek görünümler eklemek `MainViewController` aşağıdaki ekran görüntüsünde gibi görünmesi için:

    - UITextField
        - Ad: PasswordTextField
        - Yer tutucu: 'gizli parolasını girin'
    - UILabel
        - Metin: ' hata: yanlış parola. Değil geçirdiğiniz!'
        - Renk: kırmızı
        - Hizalama: merkezi
        - Satırlar: 2
        - 'Hidden' onay kutusunu işaretli 
        
    [![](images/passwordvc.png "Merkezi satırları")](images/passwordvc.png#lightbox)
    
2. Pembe Git düğmesiyle Ctrl-sürükleyerek görünüm denetleyicisi arasında bir Segue oluşturma *PinkButton* için *PinkViewController*, seçerek **anında iletme** üzerinde fare yukarı . 

3. Üzerinde Segue tıklayın ve verin *tanımlayıcı* `SegueToPink`:

    [![](images/namesegue.png "Üzerinde Segue tıklayın ve tanımlayıcı SegueToPink verin")](images/namesegue.png#lightbox)  
    

4. Son olarak, aşağıdaki ShouldPerformSegue yöntemi ekleme `MainViewController` sınıfı:

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

Bu kodda biz için segueIdentifier sözleşmenizle bizim `SegueToPink` ü bir koşul; ardından sınayabiliriz şekilde bu durumda geçerli bir parola. Bizim koşul döndürürse `true`, Segue gerçekleştirir ve sunacaktır `PinkViewController`. Varsa `false`, yeni görünüm denetleyicisi değil sunulur.

Biz bu yaklaşım bu görünüm denetleyicisi üzerinde herhangi bir Segue ShouldPerformSegue yöntemi segueIdentifier bağımsız değişkeni kontrol ederek uygulayabilirsiniz. Bir Segue tanımlayıcısını – yalnızca bu durumda sahibiz `SegueToPink`.

Storyboards.Conditional çözümde başvurmak [el ile görsel Taslaklar örnek](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) çalışan bir örnek için.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Görsel taslak başvuruları kullanma

Bir görsel taslak başvuru büyük ve karmaşık bir görsel taslak tasarım alıp özgün başvurulan daha küçük görsel Taslaklar ile sonu sağlar, böylece kaldırma karmaşıklığı kaldırarak ve ortaya çıkan tek tek yapmak daha kolay tasarım film şeritleri ve korur.

Ayrıca, bir görsel taslak başvurusu sağlayabilir bir _bağlantı_ aynı görsel taslak veya belirli bir Sahne farklı bir şirket içinde başka bir sahneye.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Dış bir görsel taslağı başvurma

Dış bir görsel taslağı bir başvuru eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, proje adını sağ tıklatın ve seçin **Ekle** > **yeni dosya...**   >  **iOS** > **film şeridi**. Girin bir **adı** tıklayın ve yeni bir film şeridi için **yeni** düğmesi:
    
    [![](images/ref01.png "Yeni dosya iletişim kutusu")](images/ref01.png#lightbox)
    
2. Normalde olur ve değişikliklerinizi kaydedin, yeni şeridinin sahneler düzenini tasarlamak: 
    
    [![](images/ref02.png "Yeni bir Sahne düzeni")](images/ref02.png#lightbox)
    
3. İOS Designer'daki başvuru eklemek için uygulayacağınız film şeridini açın.

4. Sürükleme bir **film şeridi başvuru** gelen **araç kutusu** tasarım yüzeyine: 
    
    [![](images/ref03.png "Bir görsel taslak başvurusu")](images/ref03.png#lightbox)
    
5. İçinde **pencere öğesi** sekmesinde **özellikleri Gezgini**, adını seçin **film şeridi** yukarıda oluşturduğunuz: 

    [![](images/ref04.png "Pencere öğesi sekmesi")](images/ref04.png#lightbox)
    
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme) üzerinde var olan bir Sahne denetimini tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** oluşturduğunuz: 

    [![](images/ref05.png "Bir segue oluşturma")](images/ref05.png#lightbox) 
    
7. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](images/ref06.png "Segue tamamlanması seçme Göster")](images/ref06.png#lightbox) 
    
8. Görsel taslak için değişikliklerinizi kaydedin.

Uygulamayı çalıştırdığınızda ve dış film şeridi Başvurusu'nda belirtilen görsel taslağın ilk görünümü denetleyicisinden gelen Segue oluşturduğunuz kullanıcı kullanıcı Arabirimi öğesinde tıkladığında görüntülenir.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Belirli bir dış bir görsel taslağı sahnede başvurma

Belirli bir Sahne bir başvuru eklemek için dış bir görsel taslağı (ve ilk görünüm denetleyicisi değil), aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, dış film düzenlemek üzere açmak için çift tıklayın.

2. Yeni bir Sahne ekleme ve düzenini tasarlamak, normalde yaptığınız gibi: 

    [![](images/ref07.png "Yeni bir Sahne düzeni")](images/ref07.png#lightbox)
    
3. İçinde **pencere öğesi** sekmesinde **özellikleri Gezgini**, girin bir **film şeridi kimliği** yeni sahnenin görünüm denetleyicisi için: 

    [![](images/ref08.png "Yeni bir Sahne görünümü denetleyici için bir görsel taslak kimliği girin")](images/ref08.png#lightbox)
    
3. İOS Designer'daki başvuru eklemek için uygulayacağınız film şeridini açın.

4. Sürükleme bir **film şeridi başvuru** gelen **araç kutusu** tasarım yüzeyine: 

    [![](images/ref03.png "Bir görsel taslak başvurusu")](images/ref03.png#lightbox)
    
5. İçinde **pencere öğesi** sekmesinde **özellikleri Gezgini**, adını seçin **film şeridi** ve **başvuru kimliği** (görsel taslak kimliği), Yukarıda oluşturduğunuz Sahne: 

    [![](images/ref09.png "Pencere öğesi sekmesi ")](images/ref09.png#lightbox)
    
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme) üzerinde var olan bir Sahne denetimini tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** oluşturduğunuz: 

    [![](images/ref10.png "Bir segue oluşturma")](images/ref10.png#lightbox) 
    
7. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](images/ref06.png "Segue tamamlanması seçme Göster")](images/ref06.png#lightbox) 
    
8. Görsel taslak için değişikliklerinizi kaydedin.

Uygulamayı çalıştırma ve kullanıcı olduğunda tıkladığında Segue from, Sahne ile oluşturduğunuz kullanıcı Arabirimi öğesinde belirtilen **film şeridi kimliği** dış film şeridi Başvurusu'nda belirtilen görsel taslağın görüntülenir.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Aynı film şeridi belirli sahnede başvurma

Belirli bir Sahne aynı film şeridini bir başvuru eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, film şeridini düzenlemek üzere açmak için çift tıklayın.

2. Yeni bir Sahne ekleme ve düzenini tasarlamak, normalde yaptığınız gibi: 

    [![](images/ref11.png "Yeni bir Sahne düzeni")](images/ref11.png#lightbox)

3. İçinde **pencere öğesi** sekmesinde **özellikleri Gezgini**, girin bir **film şeridi kimliği** yeni sahnenin görünüm denetleyicisi için: 

    [![](images/ref12.png "Pencere öğesi sekmesi")](images/ref12.png#lightbox)
    
3. Sürükleme bir **film şeridi başvuru** gelen **araç kutusu** tasarım yüzeyine: 

    [![](images/ref03.png "Bir görsel taslak başvurusu")](images/ref03.png#lightbox)
    
5. İçinde **pencere öğesi** sekmesinde **özellikleri Gezgini**seçin **başvuru kimliği** (görsel taslak kimliği) yukarıda oluşturduğunuz sahnenin: 

    [![](images/ref13.png "Pencere öğesi sekmesi")](images/ref13.png#lightbox)
    
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme) üzerinde var olan bir Sahne denetimini tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** oluşturduğunuz: 

    [![](images/ref14.png "Bir segue oluşturma")](images/ref14.png#lightbox) 
    
7. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](images/ref06.png "Segue tamamlanması seçme Göster")](images/ref06.png#lightbox) 
    
8. Görsel taslak için değişikliklerinizi kaydedin.

Uygulamayı çalıştırma ve kullanıcı olduğunda tıkladığında Segue from, Sahne ile oluşturduğunuz kullanıcı Arabirimi öğesinde belirtilen **film şeridi kimliği** film şeridi Başvurusu'nda belirtilen aynı film şeridi görüntülenir.

## <a name="summary"></a>Özet

Bu makalede film şeritleri ve nasıl iOS uygulamaları geliştirmede daha avantajlı olabilir kavramını sunar. Planda, görünüm denetleyicileri, görünümleri ve görünüm hiyerarşileri ve sahneler geçişler Uyarlamasız farklı türde birlikte nasıl bağlı ele alır.  Bu da Görünüm denetleyicileri örnekleme el ile bir görsel taslak ve oluşturma koşullu geçişler Uyarlamasız keşfediyor.



## <a name="related-links"></a>İlgili bağlantılar

- [El ile görsel Taslak (örnek)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [İOS Tasarımcısı giriş](~/ios/user-interface/designer/introduction.md)
- [Görsel taslaklara dönüştürme](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard sınıf başvurusu](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
