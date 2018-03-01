---
title: "Sekme çubukları ve sekme Çubuğu denetleyicileri"
description: "Sekme Gezinti kullanıcı arabirimini kullanarak iOS uygulamaları UITabBarController sınıfı kullanılarak oluşturulur. Bu makalede biz birkaç denetleyicileri ve görünümleri içeren bir sekmeli uygulama ayarlama konusunda size rehberlik. Biz, ardından kök denetleyicisi gibi bir oturum açma ekranı sonra olmadığında bir UITabBarController yükleme inceleyeceğiz."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 2a10c161c49e7cd0d45d29522a98c0dc78f7adb7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="tab-bars-and-tab-bar-controllers"></a>Sekme çubukları ve sekme Çubuğu denetleyicileri

_Sekme Gezinti kullanıcı arabirimini kullanarak iOS uygulamaları UITabBarController sınıfı kullanılarak oluşturulur. Bu makalede biz birkaç denetleyicileri ve görünümleri içeren bir sekmeli uygulama ayarlama konusunda size rehberlik. Biz, ardından kök denetleyicisi gibi bir oturum açma ekranı sonra olmadığında bir UITabBarController yükleme inceleyeceğiz._

Sekmeli uygulamalar, iOS, belirli bir sırada birden çok ekranlar burada erişilip kullanıcı arabirimleri desteklemek için kullanılır. Aracılığıyla `UITabBarController` sınıfı, uygulamaları kolayca gibi çok ekran senaryolar için destek içerir. `UITabBarController` Her ekran ayrıntılara odaklanmak uygulama geliştiricisi izin vererek çok ekran yönetim ilgilenir.

Sekmeli uygulamaları ile genellikle, oluşturulan `UITabBarController` olan `RootViewController` ana penceresinin. Ancak, ek kod bir bit sekmeli uygulamalar da art arda bazı diğer başlangıç ekranına, burada bir uygulama sekmeli arabirimi tarafından izlenen bir oturum açma ekranı ilk sunar senaryo gibi kullanılabilir.

Biz basit bir uygulama bir kılavuz ile çalışarak burada sekmeleri kullanarak inceleyeceğiz. Ardından, içinde olmayan sekmelerle çalışma konusunda inceleyeceğiz `RootViewController` senaryo.

## <a name="introducing-uitabbarcontroller"></a>UITabBarController Tanıtımı

`UITabBarController` Şunlarla destekler sekmeli uygulama geliştirme:

-  Birden çok denetleyicilerinin eklenmesi için izin verme.
-  Sekmeli kullanıcı arabirimi aracılığıyla sağlama `UITabBar` denetleyicileri ve bunların görünümleri arasında geçiş yapmak bir kullanıcı izin vermek için sınıf. 


Denetleyicileri eklenir `UITabBarController` aracılığıyla kendi `ViewControllers` olan özellik bir `UIViewController` dizi. `UITabBarController` Kendisini uygun denetleyicisi yükleme ve seçili sekme temel kendi görünümünü sunan işler.

Sekmeleri örnekleridir `UITabBarItem` bulunan sınıfı bir `UITabBar` örneği. Her `UITabBar` örneğidir üzerinden erişilebilir `TabBarItem` her sekme yer alan denetleyicisinin özelliği.

İle çalışmaya nasıl anlamak `UITabBarController`, şimdi bir kullanan basit bir uygulaması derleme konusunu inceleyin.

## <a name="tabbed-application-walkthrough"></a>Sekmeli uygulama gözden geçirme

Aşağıdaki uygulama oluşturmak için bu kılavuzda oluşturacağız:

[ ![](creating-tabbed-applications-images/00-app.png "Örnek sekmeli uygulama")](creating-tabbed-applications-images/00-app.png)

Olmasa zaten bir sekmeli uygulama şablonu bu örnek için kullanılabilir Mac Visual Studio, uygulama nasıl oluşturulur daha iyi anlamak için boş bir projeden çalışmaya yapacağız.

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>Uygulama oluşturma

Yeni bir uygulama oluşturarak başlayalım.

Seçin **Dosya > Yeni > Çözüm** Mac ve select için Visual Studio menü öğesi bir **iOS > Uygulama > Boş proje** şablonu, proje adı `TabbedApplication`, aşağıda gösterildiği gibi:

[ ![](creating-tabbed-applications-images/newsolution1.png "Boş proje şablonu seçin")](creating-tabbed-applications-images/newsolution1.png)

[ ![](creating-tabbed-applications-images/newsolution2.png "TabbedApplication proje adı")](creating-tabbed-applications-images/newsolution2.png)



### <a name="adding-the-uitabbarcontroller"></a>UITabBarController ekleme

Ardından, boş bir sınıf seçerek eklemek **Dosya > yeni dosya** ve seçme **genel: boş sınıfı** şablonu. Dosya adı `TabController` aşağıda gösterildiği gibi:

[ ![](creating-tabbed-applications-images/02-newclass.png "TabController sınıfı ekleme")](creating-tabbed-applications-images/02-newclass.png)

`TabController` Uygulanması içereceği sınıfı `UITabBarController` bir dizi yöneteceğiniz `UIViewControllers`. Kullanıcı bir sekme seçtiğinde `UITabBarController` uygun görünüm denetleyicisini görünümünü sunan ilgilenebilmek.

Uygulanacak `UITabBarController` aşağıdakileri yapmanız gerekir:

1.  Temel sınıfını ayarlamak `TabController` için `UITabBarController` . 
1.  Oluşturma `UIViewController` eklemek için örnekleri `TabController` . 
1.  Ekleme `UIViewController` atanan bir dizi örneklerine `ViewControllers` özelliği `TabController` . 


Aşağıdaki kodu ekleyin `TabController` sınıfı adımları elde etmek için:

```csharp
using System;
using UIKit;

namespace TabbedApplication {
        public class TabController : UITabBarController {

                UIViewController tab1, tab2, tab3;

                public TabController ()
                {
                        tab1 = new UIViewController();
                        tab1.Title = "Green";
                        tab1.View.BackgroundColor = UIColor.Green;

                        tab2 = new UIViewController();
                        tab2.Title = "Orange";
                        tab2.View.BackgroundColor = UIColor.Orange;

                        tab3 = new UIViewController();
                        tab3.Title = "Red";
                        tab3.View.BackgroundColor = UIColor.Red;

                        var tabs = new UIViewController[] {
                                tab1, tab2, tab3
                        };

                        ViewControllers = tabs;
                }
        }
}
```

Her biri için dikkat `UIViewController` örneği ayarlarız `Title` özelliği `UIViewController`. Ne zaman denetleyicileri eklenir `UITabBarController`, `UITabBarController` okuyacaksa `Title` her denetleyicisi ve aşağıda gösterildiği gibi ilişkili sekmenin etiketi görüntüleyin:

[ ![](creating-tabbed-applications-images/00-app.png "Örnek uygulamayı çalıştırma")](creating-tabbed-applications-images/00-app.png)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>TabController RootViewController ayarlama

Bunlar eklenme sırası denetleyicileri sekmeleri yerleştirilir sipariş karşılık `ViewControllers` dizi.

Alınacak `UITabController` ilk ekran olarak yüklemek için pencerenin yapmak ihtiyacımız `RootViewController`için aşağıdaki kodda gösterildiği gibi `AppDelegate`:

```csharp
[Register ("AppDelegate")]
        public partial class AppDelegate : UIApplicationDelegate
        {
                UIWindow window;
                TabController tabController;

                public override bool FinishedLaunching (UIApplication app, NSDictionary options)
                {
                        window = new UIWindow (UIScreen.MainScreen.Bounds);

                        var tabController = new TabController ();
                        window.RootViewController = tabController;

                        window.MakeKeyAndVisible ();
            
                        return true;
                }
        }
```

Biz uygulamayı şimdi çalıştırırsanız `UITabBarController` ilk sekmesi varsayılan olarak seçili birlikte yüklenir. İlişkili denetleyicinin sonuçlarında tarafından sunulan görüntülemek diğer sekmeleri birini seçerek `UITabBarController,` son kullanıcı ikinci sekmesinde burada seçilmiş aşağıda gösterildiği gibi:

[ ![](creating-tabbed-applications-images/03-secondtab.png "Gösterilen ikinci sekmesi")](creating-tabbed-applications-images/03-secondtab.png)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>TabBarItems değiştirme

Biz sekmesinde uygulama, değiştirelim çalışan bir sahip olduğunuza `TabBarItem` görüntülenen metin ve görüntü değiştirmek gibi bir gösterge sekmelerden birine eklemek için.

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>Bir sistem öğe ayarlama

İlk olarak, bir sistem öğesini kullanmak için ilk sekme şimdi ayarlayın. Oluşturucusunun içinde `TabController`, denetleyicinin ayarlar satırı Kaldır `Title` için `tab1` örneği ve denetleyicinin ayarlamak için aşağıdaki kodla değiştirin `TabBarItem` özelliği:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Oluştururken `UITabBarItem` kullanarak bir `UITabBarSystemItem`, başlık ve görüntü otomatik olarak iOS tarafından ekran görüntüsü gösteren aşağıda görüldüğü gibi sağlanan **Sık Kullanılanlar** simgesi ve ilk sekmesindeki Başlık:

 ![](creating-tabbed-applications-images/04a-tabimage.png "Bir yıldız simgesiyle ilk sekmesi")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>Başlık ve görüntüyü ayarlama

Bir sistem öğesini, başlık ve görüntüsü kullanmanın yanı sıra bir `UITabBarItem` için özel değerler ayarlayabilirsiniz. Örneğin, ayarlar kodunu değiştirmek `TabBarItem` adlı denetleyicisi özelliğinin `tab2` gibi:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Yukarıdaki kod adlı bir resim varsayar `second.png` Mac için Visual Studio Proje kökündeki eklendi Gerçekte üç görüntüleri tüm aygıt çözümleri, aşağıda gösterildiği gibi kapsayacak şekilde bizim projeye ekledik:

 [ ![](creating-tabbed-applications-images/tabbedimages7new.png "Projeye eklenen görüntüleri")](creating-tabbed-applications-images/tabbedimages7new.png)

Sekme görüntünün saydamlık normal çözümlenmek 60 x 60 yüksek çözünürlüklü için ve 90 x 90 iPhone 6 ile 30 x 30 png olmalıdır çözümleme artı. Bizim kodda yalnızca adlı dosyayı yüklemek ihtiyacımız `second.png` ve iOS yüksek çözünürlüklü bir Retina görüntü ile cihazlarda otomatik olarak yükler. Daha fazla bilgiyi bu konuda [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) kılavuzları. Varsayılan olarak seçili olduğunda mavi TINT ile gri sekmesini çubuğu öğelerdir.

**Not**

Yukarıdaki görüntüleri de eklenemedi **kaynakları** içerikleri uygulama paket köküne otomatik olarak kopyalanacak özel bir dizin içinde dizini:

[ ![](creating-tabbed-applications-images/tabbedapplication8.png "Görüntü kaynakları olarak")](creating-tabbed-applications-images/tabbedapplication8.png)

Ayrıca, biz ayarlandığında `Title` özelliği doğrudan üzerinde `TabBarItem`, herhangi bir değer için ayarlanmış geçersiz kılarsınız `Title` denetleyicisinde kendisi.

Biz uygulamayı şimdi çalıştırdığınızda, ikinci sekmesi bizim Özel Başlık ve görüntü aşağıda gösterildiği gibi gösterir:

[ ![](creating-tabbed-applications-images/05-customtab.png "Kare bir simge olan ikinci sekmesi")](creating-tabbed-applications-images/05-customtab.png)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>Gösterge değeri ayarlama

Sekme bir gösterge de görüntüleyebilirsiniz. Örneğin, bir gösterge üçüncü sekmesinde belirlemek için kod aşağıdaki satırı ekleyin:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Bu çalıştığını kırmızı bir etiket aşağıda gösterildiği gibi sekmesini sol üst köşesindeki "Hi" dizesi ile sonuçlanır:

[ ![](creating-tabbed-applications-images/06-badge.png "Hi rozet olan ikinci sekmesi")](creating-tabbed-applications-images/06-badge.png)

Gösterge genellikle bir numara göstergesi okunmamış, görüntülemek için kullanılan yeni öğeler. Gösterge kaldırmak için ayarlanmış `BadgeValue` aşağıda gösterildiği gibi null olarak:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>RootViewController olmayan senaryolar sekmeleri

Yukarıdaki örnekte, biz çalışmak nasıl oluşturulacağını gösterir bir `UITabBarController` da `RootViewController` penceresinin. Bu örnekte inceleyeceğiz nasıl kullanılacağını bir `UITabBarController` olmadığı `RootViewController` ve bu nasıl oluşturulur Göster film şeritleri kullanın.

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>Başlangıç ekranı örneği

Bu senaryo için ilk ekran değil bir denetleyicisinden yükleyen bir `UITabBarController`. İçine bir düğmeye dokunarak ekranla kullanıcı etkileşim kurduğunda, aynı View Controller yüklenecek bir `UITabBarController`, hangi sonra sunulur kullanıcıya. Aşağıdaki ekran görüntüsünde uygulama akışı gösterilmektedir:

[ ![](creating-tabbed-applications-images/inital-screen-application.png "Bu ekran uygulama akışı gösterilmektedir.")](creating-tabbed-applications-images/inital-screen-application.png)

Bu örnek için yeni bir uygulama başlayalım. Yeniden kullanacağız **iPhone > Uygulama > Boş proje (C#)** şablonu, proje adlandırma bu kez `InitialScreenDemo`.


Bu örnekte, biz bizim görünüm denetleyicilerinin tutmak için film şeridi gerekir. Film şeridi eklemek için:

- Proje adına sağ tıklayın ve seçin **Ekle > yeni dosya**.

- Yeni dosya iletişim kutusu belirdiğinde gidin **iOS > iPhone film şeridi boş**.

Şimdi bu yeni film şeridi çağrısı **MainStoryboard** aşağıda gösterildiği gibi: 

[ ![](creating-tabbed-applications-images/new-file-dialog.png "Bir MainStoryboard dosyası projeye ekleyin")](creating-tabbed-applications-images/new-file-dialog.png)

Film şeridi olarak ele alınan önceden film şeridi olmayan dosyasına eklerken dikkat edilecek bazı önemli adım vardır [film şeritleri giriş](~/ios/user-interface/storyboards/index.md) Kılavuzu. Bunlar:

 
1. Film şeridi adınızı ekleme **ana arabirimi** bölümünü `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Ana arabirimi MainStoryboard için ayarlama")](creating-tabbed-applications-images/project-options.png)
1. İçinde `App Delegate`, aşağıdaki kod ile penceresi yöntemini geçersiz kılın:

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

Bu örnek için üç görünüm denetleyicisi ihtiyacınız olacak. Adlı bir `ViewController1`, ilk bizim View Controller olarak ve ilk sekme kullanılır. Adlı diğer iki, `ViewController2` ve `ViewController3`, hangi kullanılacak ikinci ve üçüncü sekmeleri sırasıyla.

Tasarımcı tarafından çift MainStoryboard.storyboard dosya tıklatarak açın ve üç görünüm denetleyicisi açın tasarım yüzeyine sürükleyin. Yukarıdaki adına karşılık gelen kendi sınıfı sağlamak için bu görünümü denetleyicilerinden her birinin istiyoruz altında bunu **kimliği > sınıfı**, aşağıdaki ekran görüntüsünde gösterildiği gibi adını yazın:

[ ![](creating-tabbed-applications-images/class-name.png "ViewController1 için set sınıfı")](creating-tabbed-applications-images/class-name.png)

Mac için Visual Studio sınıfları ve gerekli designer dosyaları otomatik olarak oluşturur, bu çözüm defterinde, aşağıda gösterildiği gibi görülebilir:

[ ![](creating-tabbed-applications-images/solution-pad2.png "Projedeki otomatik olarak oluşturulan dosyalar")](creating-tabbed-applications-images/solution-pad2.png)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>Kullanıcı Arabirimi oluşturma

Ardından, basit bir kullanıcı arabirimi her ViewController'ın görünümleri için Xamarin iOS Tasarımcısı kullanarak oluşturacağız.

Sürükleme istiyoruz bir `Label` ve `Button` ViewController1 üzerine **araç** sağ taraftaki üzerinde. Sonraki adını ve aşağıdaki denetimlere metnini düzenlemek için özellikleri paneli kullanacağız:

-  **Etiket** : `Text`  =  **bir**
-  **Düğme** : `Title`  =  **kullanıcı bazı ilk eylemi alır**


Biz bizim düğmesinin görünürlüğünü denetleme bir `TouchUpInside` olay ve biz arkasındaki kodda başvurduğu gerekir. Şimdi tanımlamak **adı** `aButton` özellikler aşağıdaki ekran görüntüsünde gösterildiği gibi panelinde:

[ ![](creating-tabbed-applications-images/abutton-properties.png "Özellikler panelinde aButton kümesinin adı")](creating-tabbed-applications-images/abutton-properties.png)

Tasarım yüzeyi aşağıdaki ekran görüntüsüne benzer görünmelidir:

[ ![](creating-tabbed-applications-images/design-surface1.png "Tasarım yüzeyi bu ekran görüntüsüne benzer görünmelidir")](creating-tabbed-applications-images/design-surface1.png)

Biraz daha fazla ayrıntı için ekleyelim `ViewController2` ve `ViewController3`, her bir etiket ekleyerek ve metin 'İki' ve 'Üç' sırasıyla değiştirme. Bu kullanıcıya adresindeki arıyoruz hangi sekmesini/görünüm vurgular.

#### <a name="wiring-up-the-button"></a>Düğmeyi yukarı bağlantı kabloları

Yük oluşturacağız `ViewController1` uygulamayı ilk kez başlatıldığında. Kullanıcı düğmesi dokunur, biz düğmesini gizle ve yük bir `UITabBarController` ile `ViewController1` ilk sekme örneği.

Kullanıcı ne zaman serbest `aButton`, tetiklenmesi TouchUpInside olay istiyoruz. Şimdi düğmesini seçin ve **olaylar sekmesi** özellikleri ofisinizde, olay işleyicisi – bildirme `InitialActionCompleted` – bu kodda başvurulabilen şekilde. Bu ekran görüntüsünde gösterilmiştir:

[ ![](creating-tabbed-applications-images/event-handler.png "Kullanıcı aButton bıraktığında TouchUpInside olay tetikler")](creating-tabbed-applications-images/event-handler.png)

Artık olay başlatıldığında düğmesini gizlemek için Görünüm denetleyicisini bildirmek ihtiyacımız `InitialActionCompleted`. İçinde `ViewController1`, kısmi aşağıdaki yöntemi ekleyin:

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

Dosyayı kaydedin ve uygulamayı çalıştırın. Ekran birer görüntülenir ve dokunma yukarı üzerinde kayboluyor düğmesi görmeniz gerekir.

#### <a name="adding-the-tab-bar-controller"></a>Sekme çubuğu ekleyerek denetleyicisi

Şimdi beklendiği gibi çalıştığını bizim ilk görünüm sunuyoruz. Ardından, eklemek istiyoruz bir `UITabBarController`, görünümleri 2 ve 3 ile birlikte. Film şeridi Tasarımcısı'nda açalım.

İçinde **araç**, arama **sekmesini çubuğu denetleyicisi** denetleyicileri & nesneleri altında ve bu tasarım yüzeyine sürükleyin. Aşağıdaki ekran görüntüsünde görüldüğü gibi Sekme çubuğu denetleyicisidir UI olmayan ve bu nedenle varsayılan olarak iki görünüm denetleyicileri onunla getirir:

[ ![](creating-tabbed-applications-images/tabbarcontroller.png "Düzenine bir sekme çubuğu denetleyicisi ekleme")](creating-tabbed-applications-images/tabbarcontroller.png)

Bu yeni görünüm denetleyicileri siyah bir çubuk altındaki seçip delete tuşuna basarak silin.

Bizim film şeridi biz Segues TabBarController ve bizim görünüm denetleyicileri arasındaki geçişler işlemek için kullanabilirsiniz. İlk görünümü ile etkileşim sonra kullanıcıya sunulan TabBarController içine yük istiyoruz. Şimdi bu tasarımcıda ayarlayın.

**CTRL tuşuna basıp tıklayın** ve **sürükleme** TabBarController için düğmesinden. Fare yukarı üzerinde bir bağlam menüsü görüntülenir. Kalıcı segue kullanmak istiyoruz. 
 
Her bizim sekmeleri ayarlamak için **CTRL tuşuna basıp tıklayın** her bizim görünüm denetleyicilerini sırayla TabBarController üç ve seçin ilişki birinden gelen **sekmesini** bağlam menüsünden Aşağıda gösterildiği gibi:

[ ![](creating-tabbed-applications-images/context-menu.png "Sekme ilişki seçin")](creating-tabbed-applications-images/context-menu.png)

Aşağıdaki ekran görüntüsünde Şeridinizin benzemelidir:

[ ![](creating-tabbed-applications-images/segue-layout.png "Film şeridi bu ekran görüntüsüne benzer olmalıdır")](creating-tabbed-applications-images/segue-layout.png)

Sekme çubuğu öğelerden birini tıklatın ve Özellikler paneli keşfetmek, aşağıda gösterildiği gibi bir dizi farklı seçenekler görebilirsiniz:

[ ![](creating-tabbed-applications-images/properties-panel.png "Özellikler Explorer'ın sekmesini seçeneklerini ayarlama")](creating-tabbed-applications-images/properties-panel.png)

Biz bu rozet, başlığı ve iOS gibi belirli öznitelikleri düzenlemek için kullanabileceğiniz [tanımlayıcısı](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), diğerlerinin yanı sıra

Biz kaydedin ve uygulamayı şimdi çalıştırırsanız, biz ViewController1 örneği TabBarController yüklendiğinde düğmesi görüntülenir bulabilirsiniz. Şimdi bu geçerli görünümü üst görünümü denetleyicisi olup olmadığını kontrol ederek düzeltin. Aşması durumunda biliyoruz içinde TabBarController duyuyoruz ve bu nedenle düğmesi gizlenmelidir. Şimdi ViewController1 sınıfa aşağıdaki kodu ekleyin:

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

Ne zaman uygulama çalıştığında ve kullanıcı dokunur UITabBarController ilk ekranda düğmesi, aşağıda gösterildiği gibi ilk sekmesindeki yerleştirilen ilk ekran görünümünden ile yüklenir:

[ ![](creating-tabbed-applications-images/first-view.png "Örnek uygulama çıktısı")](creating-tabbed-applications-images/first-view.png)

<!--Save the files and run the application:

[ ![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png)-->

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağını kapsanan bir `UITabBarController` bir uygulamada. Biz bu başlık, görüntü ve rozet sekmelerinde özelliklerini ayarlamak nasıl yanı sıra her bir sekmeyi denetleyicileri yükleme aracılığıyla gitti. Biz sonra film şeridi, kullanarak incelenmesi nasıl yükleneceğini bir `UITabBarController` olmadığında çalışma zamanında `RootViewController` penceresinin.


## <a name="related-links"></a>İlgili bağlantılar

- [Sekmeli uygulamaları (örnek) oluşturma](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
