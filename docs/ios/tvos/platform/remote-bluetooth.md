---
title: Siri uzak ve Bluetooth denetleyicileri
description: "Bu makalede, yeni Siri uzak ve Bluetooth oyun denetleyicileri Xamarin.tvOS uygulamalarınızda destekleme kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: cef717a727b3b018b9eec3e8a402ae4f927f7cb8
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="siri-remote-and-bluetooth-controllers"></a>Siri uzak ve Bluetooth denetleyicileri

_Bu makalede, yeni Siri uzak ve Bluetooth oyun denetleyicileri Xamarin.tvOS uygulamalarınızda destekleme kapsar._


Xamarin.tvOS uygulamanızdaki kullanıcıların değil etkileşim arabirimiyle burada bunlar dokunun cihazın ekran görüntülerinde ancak dolaylı olarak gelen arasında yer kullanarak, doğrudan iOS [Siri uzaktan](#The-Siri-Remote).

Uygulamanızı bir oyun ise, isteğe bağlı olarak, yapılan için 3. taraf için destek iOS (MFI) oluşturabileceğiniz [Bluetooth oyun denetleyicileri](#Bluetooth-Game-Controllers) uygulamanızda de.

[![](remote-bluetooth-images/intro01.png "Bluetooth uzaktan ve oyun denetleyicisi")](remote-bluetooth-images/intro01.png#lightbox)

Bu makalede [Siri uzaktan](#The-Siri-Remote), [Touch yüzeyini hareketleri](#Touch-Surface-Gestures) ve [Siri uzak düğmeler](#Siri-Remote-Buttons) ve bunları çalışmak gösterilmektedir [hareketleri ve Film şeritleri](#Gestures-and-Storyboards), [hareketleri ve kod](#Gestures-and-Code) ve [alt düzey olay işleme](#Low-Level-Event-Handling). Son olarak, anlatılmaktadır [oyun denetleyicileri ile çalışma](#Working-with-Game-Controllers) Xamarin.tvOS uygulama.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri uzaktan

Kullanıcıların, Apple TV ve Xamarin.tvOS uygulamanız ile etkileşimde ana dahil Siri uzaktan yoludur. Apple uzaktan kanepenizde ve TV ekranında yer boyunca görüntülenen Apple TV's kullanıcı arabirimi durduğunu kullanıcı arasındaki uzaklığı köprülemek için tasarlanmıştır.

TvOS uygulama geliştiricisi olarak, sınama Siri uzaktan'ın dokunmatik yüzey, accelerometer, jiroskop ve düğmeleri yararlanır hızlı, kullanımı kolay ve görsel olarak ilgi çekici kullanıcı arabirimi oluşturma ' dir.

[![](remote-bluetooth-images/remote01.png "Siri uzaktan")](remote-bluetooth-images/remote01.png#lightbox)

Siri uzaktan tvOS uygulamanızı içinde beklenen kullanımları ve aşağıdaki özellikleri içerir:

<table width="100%" border="1px">
<tr>
    <td><b>Özelliği</b></td>
    <td><b>Genel uygulama kullanımı</b></td>
    <td><b>Oyun uygulaması kullanımı</b></td>
</tr>
<tr>
    <td valign="top"><b>Touch Surface</b><br/>Manyetik gidin, seçin ve bağlam menülerini tutmak için basın.</td>
    <td valign="top"><b>Dokunun/geçirme:</b><br/>UI Gezinti odaklanabilir öğeleri arasında.<br/><br/><b>' I tıklatın:</b><br/>Seçilen (odak) öğe etkinleştirir.</td>
    <td valign="top"><b>Dokunun/geçirme:</b><br/>Oyun tasarımına bağlıdır ve kenarlarında e dokunabilirsiniz D-Pad'i kullanılabilir.<br/><br/><b>' I tıklatın:</b><br/>Birincil düğme işlevi gerçekleştirir.</td>
</tr>
<tr>
    <td valign="top"><b>Menü</b><br/>Önceki ekrana veya menüye dönmek için basın.</td>
    <td valign="top">Apple TV giriş ekranına ana uygulama ekranından çıkar ve önceki ekrana döndürür.</td>
    <td valign="top">Duraklatma ve sürdürme oyunlar, önceki ekrana geri döner ve ana uygulama ekranından Apple TV giriş ekranına çıkar.</td>
</tr>
<tr>
    <td valign="top"><b>Siri/arama</b><br/>Ülkelerde Siri ile tuşuna basın ve tüm diğer ülkelerde, görüntüler arama ekran ses denetimi için basılı tutun.</td>
    <td valign="top">yok</td>
    <td valign="top">yok</td>
</tr>
<tr>
    <td valign="top"><b>Play/Pause</b><br/>Yürütmek ve medya duraklatmak veya uygulamalar, ikincil bir işlev sağlar.</td>
    <td valign="top">Medya kayıttan yürütme ve Duraklat/Sürdür kayıttan yürütme başlatır.</td>
    <td valign="top">Giriş video atlar veya ikincil düğmesi işlevini gerçekleştirir (varsa var).</td>
</tr>
<tr>
    <td valign="top"><b>Giriş</b><br/>Giriş ekranına geri dönmek için tuşuna basın, çalışan uygulamaları görüntülemek, basılı Aygıt uyku moduna için çift tıklayın.</td>
    <td valign="top">yok</td>
    <td valign="top">yok</td>
</tr>
<tr>
    <td valign="top"><b>Birim</b><br/>Denetimleri ses/video donanım birim eklendi.</td>
    <td valign="top">yok</td>
    <td valign="top">yok</td>
</tr>
</table>

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Dokunmatik yüzey hareketleri

Siri uzak bilgisayarın Touch yüzeyini Xamarin.tvOS uygulamanızda yanıt verebilirsiniz tek parmak hareketleri çeşitli algılayabilir:

<table width="100%">
<tr>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture01.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture02.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture03.png"></td>
</tr>
<tr>
    <td valign="top"><b>Geçirme:</b><br/>Kullanıcı Arabirimi öğeleri ekranında arasında seçim (odak) taşır (yukarı, aşağı sol, sağ). Geçirmeyi hızla eylemsizlik kullanarak içeriğin büyük listeleriyle kaydırmak için kullanılabilir.</td>
    <td valign="top"><b>' I tıklatın:</b><br/>Seçili (odak) öğeyi etkinleştiren veya oyun birincil düğmesini gibi davranır. Bağlam menüleri veya ikincil işlevleri tıklatıp tutarak etkinleştirebilirsiniz.</td>
    <td valign="top"><b>Dokunun:</b><br/>Hafifçe kenarları Touch yüzeyine dokunarak bir D-yukarı, aşağı, sola veya sağa dokunduğunuz alan bağlı olarak odağı taşıma Pad'i yön düğmeleri gibi davranır. Uygulamaya bağlı olarak, gizli denetimleri ortaya çıkarmak üzere kullanılabilir.</td>
</tr>
</table>

Apple Touch yüzeyini hareketleri ile çalışmak için aşağıdaki önerileri sağlar:

* **Tıklama ve dokunma arasında ayırt** -tıklayarak kullanıcı tarafından kasıtlı bir eylemdir ve seçim, etkinleştirme ve oyun birincil düğmesine için uygundur. Dokunma daha hafif ve kullanıcı Siri uzaktan kendi el ile genellikle tutarak ve yanlışlıkla bir dokunun olayı kolayca etkinleştirebilirsiniz için kullanılmamalıdır.
* **Yeniden tanımlama standart hareketleri yok** -belirli hareketleri belirli eylemleri gerçekleştirecek bir Beklenti kullanıcının sahip, anlamı veya bu hareketleri işlevini uygulamanızda yeniden tanımlamanız gerekir. Tek bir oyun uygulaması sırasında etkin oyunlar istisnası.
* **Gerektiğinde yeni hareketleri tanımlamak** -yeniden, kullanıcının belirli hareketleri belirli eylemleri gerçekleştirecek bir Beklenti sahip. Standart eylemleri gerçekleştirmek için özel hareketler tanımlama kaçınmalısınız. Ve yeniden oyunlar en sık kullanılan özel hareketler oyuna eğlenceli, derinlikli play, ekleyebileceğiniz istisnadır.
* **Uygunsa, D-Pad'i dokunur için yanıt** - hafifçe dokunarak Touch yüzey kenarlarına D-Pad'i bir oyun denetleyicisi taşıma odağını veya yön yukarı, aşağı, sol gibi tepki veya sağ köşede. Uygunsa, bu hareketleri uygulama ya da oyun yanıt.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri uzak düğmeler

Touch yüzeyinde hareketleri ek olarak, uygulamanızı Touch yüzeyini tıklatarak veya Yürüt/Duraklat düğmesine basarak kullanıcıya yanıt verebilir. Siri oyun denetleyicisi Framework kullanarak uzaktan erişiyorsanız, basılan menü düğmesi da algılayabilir. 

Ayrıca, menü düğmesine basarsa standart hareketi tanıyıcıyı algılanabilir `UIKit` öğeleri. Basılan menü düğmesi engellemek, geçerli görünümü ve görünüm denetleyicisini kapatma sorumlu ve önceki bir dönüş.

> [!IMPORTANT]
> **Not:** yapmanız gerekenler **her zaman** Yürüt/Duraklat düğmesini uzak işlev atayın. İşlev olmayan bir düğmeye sahip son kullanıcıya bozuk Ara uygulamanızı yapabilirsiniz. Bu düğme için geçerli bir işlev yoksa, birincil düğmeyi (yüzeyini tıklatın Touch) ile aynı işlevi atayın.




<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Hareketleri ve film şeritleri

Xamarin.tvOS uygulamanızda Siri uzaktan çalışmak için en kolay yolu hareketi tanıyıcıları görünümlerinizi arabirimi Tasarımcısı'nda eklemektir.

Hareketleri tanıyıcı eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve arabirimi tasarımcısı düzenlemek için açın.
2. Sürükleme bir **dokunun hareketi tanıyıcı** gelen **Kitaplığı** ve görünümünde bırak: 

    [![](remote-bluetooth-images/storyboard01.png "Dokunun hareketi tanıyıcı")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Denetleme **seçin** içinde **düğmesini** bölümünü **özniteliği denetçisi**: 

    [![](remote-bluetooth-images/storyboard02.png "Select denetleyin")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Seçin** hareketi kullanıcı tıklatmak yanıt verecektir anlamına gelir **Touch yüzeyini** Siri uzak. Ayrıca yanıt seçeneğiniz **menü**, **Yürüt/Duraklat**, **yukarı**, **aşağı**, **sol** ve **Sağ** düğmeler.
5. Ardından, yedeklemek wire bir **eylem** gelen **dokunun hareketi tanıyıcı** ve çağrısından `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "Dokunun hareketi tanıyıcı eylemden")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Yaptığınız değişiklikleri kaydedin ve Mac için Visual Studio'ya geri dönün

Görünüm denetleyicinizi Düzenle (örneğin `FirstViewController.cs`) dosya ve tetiklenen hareketi işlemek için aşağıdaki kodu ekleyin:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md). Özellikle [kullanıcı arabirimi oluşturma](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) ve [çıkışlar ve Eylemler ile kod yazma](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) bölümler.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Hareketleri ve kod

İsteğe bağlı olarak, doğrudan C# kodunda hareketleri oluşturun ve bunları görünümleri, kullanıcı arabiriminde ekleyin. Örneğin, manyetik hareketi tanıyıcıları bir dizi eklemek için Görünüm denetleyicinizi düzenleyebilir ve aşağıdaki kodu ekleyin:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();    

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>Alt düzey olay işleme

Temel bir özel tür oluşturuyorsanız `UIKit` Xamarin.tvOS uygulamanızda (örneğin `UIView`), alt düzey işlenmesini Düğme basma sağlama yeteneği de `UIPress` olayları. 

A `UIPress` olaydır tvOS için ne bir `UITouch` olaydır iOS için dışında `UIPress` düğmesi hakkında bilgi Siri uzak bastığında veya diğer Bluetooth cihazlara (örneğin, bir oyun denetleyicileri) bağlı döndürür. `UIPress` olayları basılan düğme ve durumuna (Began, iptal edildi, değiştirilen veya sona erdi) açıklanmaktadır. 

Cihazların Bluetooth oyun denetleyicileri gibi analog düğmeleri `UIPress` de düğme uygulanmakta zorla miktarını döndürür. `Type` Özelliği `UIPress` olay tanımlar hangi fiziksel düğmesi özelliklerini geri kalanı oluştu değişiklik açıklamak değiştirilen durumuna sahip.

Aşağıdaki kod örneği işleme alt düzey gösterir `UIPress` olaylar için bir `UIView`:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region 
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

İle `UITouch` herhangi birini uygulamanız gerekiyorsa olayları `UIPress` olayı geçersiz kılmaları, dört uygulamalıdır.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth oyun denetleyicileri

Apple TV, 3. taraf, yapılan için iOS ile birlikte gelen standart Siri uzaktan yanı sıra (MFI) Bluetooth oyun denetleyicileri Apple TV ile eşleştirilmiş ve değiştirebilirsiniz Xamarin.tvOS uygulamanızı denetlemek için kullanılır.

[![](remote-bluetooth-images/game01.png "Bluetooth oyun denetleyicileri")](remote-bluetooth-images/game01.png#lightbox)

Oyun denetleyicileri oyunlar geliştirmek ve oyun içinde tümüyle yoğunlaşabilmek duygusu sağlamak için kullanılabilir. Böylece kullanım uzaktan ve denetleyici arasında geçiş yapmak zorunda değil standart Apple TV arabirimi denetlemek için de kullanılabilir.

> [!IMPORTANT]
> **Not:** Bluetooth oyun denetleyicileri olan son kullanıcıların yapabileceğiniz isteğe bağlı bir satın alma, uygulamanızın satın almak için kullanıcı zorlayamaz. Oyun denetleyicileri uygulamanız destekliyorsa, oyun tüm Apple TV kullanıcılar tarafından kullanışlı olmasını sağlamak, ayrıca Siri uzaktan desteklemesi gerekir.


Oyun denetleyicileri tvOS uygulamanızı içinde beklenen kullanımları ve aşağıdaki özellikleri vardır:
<table width="100%" border="1px">
<tr>
    <td><b>Özelliği</b></td>
    <td><b>Genel uygulama kullanımı</b></td>
    <td><b>Oyun uygulaması kullanımı</b></td>
</tr>
<tr>
    <td valign="top"><b>D-Pad</b></td>
    <td valign="top">Kullanıcı Arabirimi öğeleri (değişiklikleri odak) gider.</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>A</b></td>
    <td valign="top">Seçilen (odak) öğe etkinleştirir.</td>
    <td valign="top">Birincil düğme işlevi gerçekleştirir ve iletişim eylemleri onaylar.</td>
</tr>
<tr>
    <td valign="top"><b>B</b></td>
    <td valign="top">Önceki ekrana döndürür veya uygulamanın ana ekranında, giriş ekranına çıkar.</td>
    <td valign="top">Önceki ekrana döndürür veya ikincil düğme işlevi gerçekleştirir.</td>
</tr>
<tr>
    <td valign="top"><b>X</b></td>
    <td valign="top">Medya kayıttan yürütme veya duraklatma/sürdürür kayıttan yürütme başlatır.</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>Y</b></td>
    <td valign="top">yok</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>Menü</b></td>
    <td valign="top">Önceki ekrana döndürür veya uygulamanın ana ekranında, giriş ekranına çıkar.</td>
    <td valign="top">Duraklat/oyunlar, önceki ekrana geri döner veya giriş ekranına çıkar uygulamanın ana ekranında değilse sürdürün.</td>
</tr>
<tr>
    <td valign="top"><b>Sol Kama düğmesi</b></td>
    <td valign="top">Sol gider.</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>Sol tetikleyici</b></td>
    <td valign="top">Sol gider.</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>Sağ Kama düğmesi</b></td>
    <td valign="top">Sağa gider.</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>Doğru tetikleyici</b></td>
    <td valign="top">Sağa gider</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>Sol Thumbstick</b></td>
    <td valign="top">Kullanıcı Arabirimi öğeleri (değişiklikleri odak) gider.</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
<tr>
    <td valign="top"><b>Sağ Thumbstick</b></td>
    <td valign="top">yok</td>
    <td valign="top">Oyun üzerinde bağlıdır.</td>
</tr>
</table>

Apple oyun denetleyicileri ile çalışmak için aşağıdaki önerileri sağlar:

- **Oyun denetleyicisi bağlantıları onaylayın** -tvOS uygulamanızı kullanmaya ve herhangi bir zamanda son kullanıcı tarafından durduruldu. Her zaman bir oyun denetleyicileri uygulama başlangıç veya uyanık kez varlığını denetlemek ve gerektiğinde harekete gerekir.
- **Siri uzak ve oyun denetleyicileri Your App çalışır olun** -kullanıcıların uygulamanızı kullanmaya Siri uzaktan ve oyun denetleyicileri arasında geçiş yapmak gerekmez. Uygulamanızı genellikle her şeyi gitmek kolay olduğundan ve beklendiği gibi çalıştığını sağlama denetleyicileri her iki tür sınayın.
- **Bir şekilde geri sağlamak** -menü düğmesine basarak önceki ekrana her zaman döndürmelidir. Kullanıcı ana uygulama ekranında ise menü düğmesi bunları Apple TV giriş ekranına döndürmelidir. Oyunlar sırasında kullanıcı oyunlar Duraklat/Sürdür veya ana menüye dönmek yeteneğini uyarı menü düğmesi görüntülemelidir.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Oyun denetleyicileri ile çalışma

Yukarıda belirtildiği gibi ek olarak Siri uzaktan kullanıcının Apple TV ile birlikte gelen isteğe bağlı olarak iliştirebilirsiniz standart bir 3. taraf, yapılan için iOS (MFI) Bluetooth oyun denetleyicileri ve Xamarin.tvOS uygulamanızı denetlemek için kullanın.

Uygulamanızı alt düzey denetleyicisi giriş gerekirse, Apple'nın kullanabilir [oyun denetleyicisi Framework](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) tvOS aşağıdaki değişiklikleri vardır:

- Mikro oyun denetleyicileri profil (`GCMicroGamepad`) Siri uzaktan hedeflemek için eklendi.
- Yeni `GCEventViewController` sınıfı, uygulamanızı aracılığıyla denetleyicisinin olaylarını yönlendirmek için kullanılabilir. Bkz: [belirleme oyun denetleyicisi giriş](#Determining-Game-Controller-Input) daha fazla ayrıntı için bölüm aşağıda.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Oyun denetleyicisi destek gereksinimleri

Apple oyun denetleyicileri Xamarin.tvOS uygulamanız destekliyorsa, karşılanması gereken birkaç belirli gereksinimlere sahiptir:

- **Siri uzaktan desteklemelidir** -Siri uzaktan her zaman desteklemesi gerekir. Oyununuzu 3. taraf bir oyun oynanabilir Denetleyicisi'ne gerekli kılamazsınız.
- **Genişletilmiş denetim düzenini desteklemelidir** -tüm tvOS oyun denetleyicileri olmayan-formfitting, denetleyicileri genişletilmiş.
- **Oyunlar, tek başına denetleyicileriyle Playable olmalıdır** -uygulamanızı bir genişletilmiş oyun denetleyicileri destekliyorsa, yalnızca o oyun denetleyicisiyle oynanabilir olmalıdır.
- **Yürüt/Duraklat düğmesini desteklemelidir** -oyunlar sırasında kullanıcı Yürüt/Duraklat düğmesine basarsa Duraklat/Sürdür oyunlar yeteneği kullanıcı vermiş bir uyarı görüntüler veya gerekir ana menüye dönmek.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Oyun denetleyicisi desteğini etkinleştirme

Xamarin.tvOS uygulamanızda oyun denetleyicileri desteğini etkinleştirmek için çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek üzere açmak için:

[![](remote-bluetooth-images/game02.png "Info.plist Düzenleyicisi")](remote-bluetooth-images/game02.png#lightbox)

Altında **oyun denetleyicileri** bölümünde, tarafından işaretleyin **etkinleştirmek oyun denetleyicileri**, tüm uygulama tarafından desteklenen oyun denetleyicileri türleri denetleyin.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Siri uzak bir oyun denetleyicisi olarak kullanma

Apple TV ile gelen Siri uzaktan sınırlı bir oyun denetleyicileri kullanılabilir. Diğer oyun denetleyicileri gibi oyun denetleyicisi Framework'teki görüntülersiniz bir `GCController` nesne ve destekler `GCMotion` ve `GCMicroGamepad` profilleri.

Siri uzaktan oyun denetleyicileri kullanılırken, aşağıdaki özelliklere sahiptir:

- Touch yüzeyini D-analog giriş verileri sağlayan panel kullanılabilir.
- Uzak bir dikey veya yatay yönde kullanılabilir ve profil nesnesi giriş verilerini otomatik olarak çevir, uygulamanızı karar verir.
- Touch yüzeyini tıklatarak davranır düğmesine basarak gibi **A** oyun denetleyicileri üzerinde.
- Yürüt/Duraklat düğmesini düğmesi gibi davranan **X** oyun denetleyicileri üzerinde.
- Menü düğmesi kullanıcı oyunlar Duraklat/Sürdür veya ana menüye dönmek yeteneğini uyarı görüntülemesi gerekir.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Oyun denetleyicisi giriş belirleme

Burada oyun denetleyicileri olayları alınan paralel dokunma olaylarla iOS, üst düzey teslim etmek için tüm alt düzey olayları tvOS işler `UIKit` olaylar. Sonuç olarak, en düşük düzey oyun denetleyicileri olayları erişmesi gerekiyorsa kapatmak gerekecektir `UIKit`ın varsayılan davranışı.

Kullanmanız gereken doğrudan denetleyicisinin girişini işlemek istediğinizde tvOS üzerinde bir `GCEventViewController` (veya bir alt kümesi) oyun görüntülemek için kullanıcının kullanıcı arabirimi. Her bir `GCEventViewController` olan *ilk Yanıtlayıcı*, oyun denetleyicileri giriş yakalanan ve uygulamanızı oyun denetleyicisi çerçevesi aracılığıyla teslim edilir.

Kullanabileceğiniz `UserInteractionEnabled` özelliği `GCEventViewController` olayları nasıl işlenir ve işlenmiş geçiş yapmak için sınıf.

Apple'nın oyun denetleyicileri destek uygulama hakkında daha fazla bilgi için lütfen bkz [oyun denetleyicileri ile çalışma](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) bölümüne [tvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) ve [oyun denetleyicileri Programlama Kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede Apple TV, Touch yüzeyini hareketleri ve Siri uzak düğmeler ile birlikte gelen yeni Siri uzaktan ele. Ardından, hareketleri ve film şeritleri, hareketleri kodu ve alt düzey olayları ile çalışma ele. Son olarak, oyun denetleyicileri ile çalışma ele alınan durumunda.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
