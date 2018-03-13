---
title: "Gelişmiş iletisi uygulama uzantıları"
description: "Bu makalede iletileri uygulamayla tümleştirilen ve kullanıcıya yeni işlevselliği sunan bir Xamarin.iOS çözümüne iletisi uygulama uzantıları ile çalışmak için Gelişmiş yöntemlerini gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fcfd1fd2ec9271bb5e8d9e09b43b7dc4cf3b3f12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="advanced-message-app-extensions"></a>Gelişmiş iletisi uygulama uzantıları

_Bu makalede iletileri uygulamayla tümleştirilen ve kullanıcıya yeni işlevselliği sunan bir Xamarin.iOS çözümüne iletisi uygulama uzantıları ile çalışmak için Gelişmiş yöntemlerini gösterir._


Yeni bir ileti uygulama uzantısı bütünleşir iOS 10, **iletileri** kullanıcı yeni işlevsellik uygulama ve sunar. Uzantı, metin, etiketler, ortam dosyaları ve etkileşimli iletilerin gönderebilirsiniz.

## <a name="about-message-app-extensions"></a>İleti Uygulama uzantıları hakkında

Yukarıda belirtildiği gibi bir ileti uygulama uzantısı ile tümleştirilir **iletileri** kullanıcı yeni işlevsellik uygulama ve sunar. Uzantı, metin, etiketler, ortam dosyaları ve etkileşimli iletilerin gönderebilirsiniz. İleti Uygulama Uzantısı iki tür mevcuttur:

- **Etiket paketleri** -kullanıcı bir iletiye ekleyebilirsiniz etiketler koleksiyonunu içerir. Etiket paketleri hiçbir kod yazmadan oluşturulabilir.
- **iMessage uygulama** -etiketler seçerek, metin girerek, ortam dosyalarıyla (isteğe bağlı tür dönüşümleri) dahil olmak üzere ve oluşturma, düzenleme ve etkileşim iletileri göndermek için Messages uygulamasının içinde özel bir kullanıcı arabirimi sunabilir.

İleti uygulamaları uzantıları üç ana içerik türlerini sağlar:

- **Etkileşimli iletilerin** -bir uygulamanın oluşturduğu özel ileti içerik türünü, kullanıcı uygulama ileti üzerinde ne zaman dokunur ön planda başlatılacak.
- **Etiketler** -olan kullanıcılar arasında gönderilen iletileri eklenebilir ve uygulama tarafından üretilen görüntüler. Bkz: bizim [dondurma Oluşturucu](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) bir etiket paketi uygulaması örnek uygulaması için örnek uygulama.
- **Desteklenen diğer içeriği** -uygulama içeriği fotoğrafları, videoları, metin ya da iletileri uygulama tarafından bir zaman desteklenen tür bağlantıları gibi sağlayabilir.

Yeni iOS 10, ileti uygulama artık kendi özel ve yerleşik uygulama mağazası içerir. İleti uygulamaları uzantıları dahil tüm uygulamalar görüntülenir ve bu depolama alanındaki yükseltildi. Yeni iletiler uygulama bölümü kullanıcılara hızlı erişim sağlamak için iletileri uygulama mağazasından indirilmiş uygulamaları görüntüler.

Ayrıca iOS 10'da, kullanıcının bir uygulamayı kolay bulmasına olanak veren satır içi uygulama Attribution Apple yeni ekledi. Bir kullanıcı içeriği başka bir 2 kullanıcının yoksa bir uygulamadan gönderirse, örneğin, (bir etiket gibi örneğin), yüklü adı gönderen uygulama ileti geçmişi içeriği altında listelenir. Uygulamanın kullanıcı dokunur varsa adı, ileti uygulama Mağazası'nin biz açılması ve Mağazası'nda seçilen uygulama.

İleti uygulamaları uzantıları Geliştirici oluşturmaya alışkın olduğu ve tüm standart çerçeveler ve standart iOS uygulaması özelliklerini erişimi vardır var olan iOS uygulamaları benzerdir. Örneğin:

- Uygulama içi satın alma erişimi.
- Apple Pay erişime sahip.
- Aygıt donanım kamera gibi erişimi.

İletisi uygulamaları uzantıları yalnızca 10 İos'ta desteklenir, ancak bu uzantılar Gönder içeriği watchOS ve macOS cihazlarda görüntülenebilir. Yeni _Recents sayfa_ watchOS 3 eklenen, ileti uygulamaları uzantılardan dahil olmak üzere Phone gönderilen son etiketler görüntüler ve izleme bu etiketler Gönder izin verin.

## <a name="about-interactive-messages"></a>Etkileşimli iletileri hakkında

Özel ileti Kabarcık sunmak ve ileti uygulama uzantısı tarafından sağlanan etkileşimli iletileri. İleti giriş alanında Ekle etkileşimli ileti içeriği oluşturmak verin ve gönderebilirsiniz.

[![](advanced-message-app-extensions-images/interactive01.png "Etkileşimli ileti içeriği oluşturma")](advanced-message-app-extensions-images/interactive01.png#lightbox)

Alıcı kullanıcı, kendi ileti Kabarcık oluşturulduğu ileti uygulamasının uzantısı yüklenemedi ileti geçmişinde dokunarak etkileşimli iletisine yanıt verebilir. Uzantı başlatılan tam ekran olmalı ve bir yanıt oluşturan ve gönderen kullanıcıya geri göndermek izin verin.

[![](advanced-message-app-extensions-images/interactive02.png "Tam ekran uzantısı başlattı")](advanced-message-app-extensions-images/interactive02.png#lightbox)


Aşağıdaki konular aşağıda ayrıntılı olarak ele alınacaktır:

- İletileri API genel bakış
- Uzantı yaşam döngüsü
- Bir ileti oluşturma
- İleti gönderme

## <a name="messages-api-overview"></a>İletileri API genel bakış

Kullanıcı tarafından çağrıldığında bir ileti uygulama uzantısı compact görünüm modunda ileti geçmişi sonundaki görüntülenir:

[![](advanced-message-app-extensions-images/interactive03.png "İletileri API genel bakış")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController` Nesnesidir ileti uygulama uzantısı'nda uzantının görünüm kullanıcıya görüntülendiğinde çağrılan ana sınıfı.
2. Konuşma olarak kullanıcıya sunulan bir `MSConversation` nesne örneği.
3. `MSMessage` Sınıfı, belirli bir ileti Kabarcık konuşmadaki temsil eder.
4. `MSSession` bir ileti nasıl gönderileceğini denetler.
5. `MSMessageTemplateLayout` ileti nasıl görüntüleneceğini denetler

## <a name="the-extension-lifecycle"></a>Uzantı yaşam döngüsü

Bir ileti uygulama etkin hale uzantısı işlemi göz atın:

[![](advanced-message-app-extensions-images/interactive04.png "Bir ileti uygulama etkin hale uzantısı işlemi")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. Bir uzantı (örneğin uygulama bölümü) başlatıldığında, ileti uygulamasını bir işlem başlatır.
2. `DidBecomeActive` Yöntemi çağrılır ve geçirilen bir `MSConversation` ileti uygulama uzantısının çalıştığı konuşma temsil eder.
3. Uzantı kapatarak bağlı olduğu bir `UIViewController` her ikisi de `ViewWillAppear` ve `ViewDidAppear` olarak adlandırılır.

Ardından, bir ileti uygulama devre dışı olma uzantısı işlemi göz atın:

[![](advanced-message-app-extensions-images/interactive05.png "Bir ileti uygulama devre dışı olma uzantısı işlemi")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. İleti uygulaması uzantısını devre dışı bırakıldığında, `ViewWillDisappear` yöntemi önce çağrılır.
2. Ardından `ViewDidDisappear` yöntemi çağrılır.
3. `WillResignActive` Yöntemi çağrılır ve geçirilen bir `MSConversation` ileti uygulama uzantısının çalıştığı konuşma temsil eder. Bu noktada, mesajlar uygulamasının uzantısı arasındaki bağlantı olan yaklaşık yayımlanacak.
4. Daha sonraki bir noktada işlem iletileri uygulama tarafından sonlandırıldı.

Uzantı kısa süreli bir işlem olduğundan, işleme ve pil gücünden tasarruf etmek için sistem tarafından titizlikle sonlandırılır. Geliştirici bu tasarlarken ve bir ileti uygulama uzantısı uygulama göz önünde bulundurmanız.

## <a name="composing-a-message"></a>Bir ileti oluşturma

İleti uygulaması uzantısı bir işlemde çalışan ve kendi kullanıcı arabirimi girildikten sonra aşağıdaki kod yeni bir ileti oluşturmak için kullanılabilir:

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

Bu kod yeni bir oluşturur `MSMessage` ve çeşitli özelliklerini ayarlar (gibi `Url`). İletinin yalnızca İos'ta oluşturulabilir olsa da, hem iOS hem de macOS görüntülenecek gönderilebileceği.

Kullanıcı ileti Kabarcık macOS üzerinde konuşmadaki üzerinde tıklarsa, Mac open web tarayıcısı URL'de belirtilen adresi dener. Sonuç olarak, geliştiricinin Web sitesi bazı macOS web tarayıcısında iletinin gösterimini makineler dayalı Göster yapabiliyor olmanız gerekir.

`AccessibilityLabel` Özelliği, kullanıcıya konuşma dökümü okumak için ekran okuyucular tarafından kullanılır. `Layout` Özellik belirtir nasıl iletisi, şu anda yalnızca görüntülenir `MSMessageTemplateLayout` desteklenir ve aşağıdaki gibi görünür:

[![](advanced-message-app-extensions-images/interactive06.png "MSMessageTemplateLayout şablonu")](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image` Özelliği `MSMessageTemplateLayout` ekranında MessageBubble ana gövdesi için içerik sağlar. `MediaFileUrl` Özelliği de ileti Kabarcık gövdesi için içerik sağlar ancak tarafından desteklenmeyen içerik için verir `UIImage` (örneğin, arka planda döngü bir video dosyası). Her iki `Image` ve `MediaFileUrl` özellikleri sağlanır, `Image` özelliği öncelik alır. `MediaFileUrl` PNG, JPEG, GIF ve videoda (Media Player çerçevesi tarafından yürütülen herhangi bir formatta) desteklediği medya biçimleri.

Önerilen ortam boyutu, 3 x çözünürlükte 300 x 300 pikseldir. Biraz daha büyük ve küçük varlıklar de kabul edilir ve en iyi sonuçları almak için birkaç farklı boyutlarda ile test Apple önerir. İleti Uygulama aşağı örnek ve bu ortam gerekli olarak ölçeklendirin.

Varlıkları alıcıya gönderildiğinde bağlı herhangi bir ortam otomatik olarak kod çevrimi bunları aktarımı ağlar üzerinden iyileştirmek için Messages uygulamasının tarafından olacaktır. Bu nedenle, Apple medya ölçeklendirilmiş ve aktarım için sıkıştırılır çünkü iletisine medya metinde dahil böylece metin okunmaz işleme olası zorlaştırır.

`ImageTitle` Ve `ImageSubtitle` özelliklerini ileti Kabarcık sunulan medya için bir açıklama sağlayın. Bu özellikler metin olarak burada bunlar crisply sol alt köşesinde görüntünün işlenir alıcı cihaza gönderilir.

`Caption`, `SubCaption`, `TrailingCaption` Ve `TrailingSubcaption` özellikleri daha fazla görüntü açıklar ve görüntüsünün altındaki bir bölümdeki çizilir. Tüm bu özellikler ayarlanması `null` resim yazısı alan olmadan ileti Kabarcık oluşturacak:

[![](advanced-message-app-extensions-images/interactive07.png "Bir ileti Kabarcık Resim yazısı alan olmadan")](advanced-message-app-extensions-images/interactive07.png#lightbox)

Not etmek için son Messages uygulamasının ileti Kabarcık üst sol alt köşesindeki ileti uygulama uzantının simgesi çizin şeydir.

## <a name="sending-a-message"></a>İleti gönderme

Bir kez bir `MSMessage` , aşağıdaki kod, göndermek için kullanılabilir, oluşan bırakıldı:

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

`ActiveConversation` Özelliği `MSMessagesAppViewController` ileti uygulama uzantısı içinde başlatılan geçerli konuşma tutacaktır.

Çağrı `InsertMessage` , `MSConversation` ileti konuşmadaki içerir ve ortaya çıkabilecek hataları işlemek için. İletiyi başarıyla eklenirse, ileti Kabarcık giriş alanında görüntülenir.

Ayrıca, uzantısı gibi konuşmaya farklı veri türlerini gönderebilir:

- **Metin** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Ekleri** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Etiketler**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` nerede `sticker` olan bir `MSSticker`.

Yeni içerik giriş alanı eklendiğinde, kullanıcı mavi dokunarak ileti gönderebildiğini **Gönder** (tipik bir ileti yaptıkları gibi) düğmesine tıklayın. İçeriği otomatik olarak göndermek ileti uygulama uzantısı için bir yolu yoktur, bu işlem tamamen kullanıcı denetimi altında.

## <a name="handling-the-compact-and-expanded-modes"></a>Sıkıştırılmış ve genişletilmiş modları işleme

Bir ileti uygulama uzantısı iki farklı görünüm modlarından birini görüntülenebilir:

[![](advanced-message-app-extensions-images/interactive08.png "İki farklı görünüm modda görüntülenen bir ileti uygulama uzantısı: Sıkıştır & Genişletilmiş")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -burada ileti uygulama uzantısı alt % 25'ini ileti görünümünü alır varsayılan mod budur. Compact modunda uygulama klavye, yatay kaydırma veya sağdan hareketi tanıyıcıları erişimi yok. Uygulama giriş alanı için erişime sahip ve çağrılar `InsertMessage` vardır kullanıcıya anında görüntülenir.
- **Genişletilmiş** -ileti uygulama uzantısı tüm ileti görünümü doldurur. Giriş alanı için erişime sahip değil, ancak klavye, yatay kaydırma ve manyetik hareketi tanıyıcıları erişimi yok.

Bir ileti uygulama uzantısı Bu modlar arasında program aracılığıyla veya el ile kullanıcı tarafından herhangi bir zamanda değiştirilebilir ve yanıt veren herhangi bir görünüm modu değişir instantly olmalıdır.

Aşağıdaki örnekte, iki farklı görünüm modlar arasında geçiş yapma işleme göz atın. İki farklı görünümü denetleyicileri her durum için gerekli olacaktır. `StickerBrowserViewController` Tanıtıcıları **Compact** Görünüm ve `AddStickerViewController` işleyecek **Genişletilmiş** görünümü:

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
}
```

`DidTransition` İki modlar arasında geçiş yapma işlemek için yöntem geçersiz:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
```

İsteğe bağlı olarak, uygulama kullanabilirdik `WillTransition` (Icecream Oluşturucu yukarıdaki örnekte gerçekleştirilir gibi) kullanıcıya sunulan önce görünüm modu değişikliği işlemeye yöntemi. Daha fazla bilgi için lütfen bkz bizim [daha fazla etiket özelleştirme](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) belgeleri.

## <a name="replying-to-a-message"></a>Bir iletiyi yanıtlarken

Bir ileti uygulama uzantısı bir ileti yanıtlarken işlemesi gereken iki durum vardır:

[![](advanced-message-app-extensions-images/interactive09.png "Etkin ve etkin modda ileti uygulama uzantısı")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **Uzantı devre dışı olduğundan** -bir ileti uygulama uzantının ileti Kabarcık kullanıcı uzantıları etkinleştirmek ve etkileşimli konuşma devam etmek için dokunabilirsiniz ileti dökümü içinde.
- **Uzantısıdır etkin** -kullanıcı ileti uygulama uzantının ileti Kabarcık Genişletilmiş Görünüm modu girin ve etkileşimli sürecin bıraktığı yerden devam etmek için bu ileti dökümü içinde dokunabilirsiniz.

### <a name="the-extension-is-inactive"></a>Uzantı etkin değil

İleti dökümü kullanıcı tarafından bir ileti Kabarcık dokunduğunuz ve ileti uygulaması uzantısının etkin olduğunda, aşağıdaki süreç gerçekleşir:

[![](advanced-message-app-extensions-images/interactive10.png "Etkin olmayan bir ileti Kabarcık işleme")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. Kullanıcı uzantının ileti Kabarcık dokunur.
2. Uzantı başlatıldığında, ileti uygulamasını bir işlem başlatır.
3. `DidBecomeActive` Yöntemi çağrılır ve geçirilen bir `MSConversation` ileti uygulama uzantısının çalıştığı konuşma temsil eder.
4. Uzantı kapatarak bağlı olduğu bir `UIViewController` her ikisi de `ViewWillAppear` ve `ViewDidAppear` olarak adlandırılır.

İşlem tamamlandığında, ileti uygulama uzantısı Genişletilmiş Görünüm modunda sunulur.

### <a name="the-extension-is-active"></a>Etkin uzantısıdır

İleti dökümü kullanıcı tarafından bir ileti Kabarcık dokunduğunuz ve ileti uygulaması uzantısının etkin olduğunda, aşağıdaki süreç gerçekleşir:

[![](advanced-message-app-extensions-images/interactive11.png "Etkin bir ileti Kabarcık işleme")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. Kullanıcı uzantının ileti Kabarcık dokunur.
2. İleti uygulama uzantısı zaten etkin olduğundan `WillTransition` yöntemi `MSMessagesAppViewController` sıkıştırma kaynağı Genişletilmiş Görünüm moda geçişi işlemek üzere çağrılır.
3. `DidSelectMessage` Yöntemi `MSMessagesAppViewController` çağrılır ve geçirilen `MSMessage` ve `MSConversation` ileti Kabarcık ait.
4. `DidTransition` Yöntemi `MSMessagesAppViewController` sıkıştırma kaynağı Genişletilmiş Görünüm moda geçişi işlemek üzere çağrılır.

Yeniden işlemi tamamlandığında, ileti uygulama uzantısı Genişletilmiş Görünüm modunda sunulur.

### <a name="accessing-the-selected-message"></a>Seçili iletiyi erişme

Kullanıcı bir ileti için ileti uygulama uzantısı ait Kabarcık dokunur zaman her iki durumda da, bu erişim gerekir. `MSMessage` , dokunduğunuz kullanarak `SelectedMessage` özelliği `MSConversation`.

Örneğin:

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

Seçili iletiyi ileti uygulama uzantının kullanıcı Arabiriminde gösterilecek ve kullanıcı bir yanıt oluşturan izin.

## <a name="removing-partially-completed-messages"></a>Kısmen kaldırma iletilerini tamamlandı

Etkileşimli bir konuşma farklı adımları iki kullanıcının konuşmadaki arasında gönderme sürecinde kısmen tamamlanmış ileti balonları ileti dökümü dağıtmayı başlatmak için:

[![](advanced-message-app-extensions-images/interactive12.png "Kısmen tamamlanmış ileti balonları ileti dökümü alanınızda karışıklık olabilir")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Bunun yerine, ileti uygulama uzantısı ileti dökümü kısa bir açıklama içine önceki iletiyi balonları daraltın:

[![](advanced-message-app-extensions-images/interactive13.png "İleti dökümü içinde önceki iletiyi balonları daraltma")](advanced-message-app-extensions-images/interactive13.png#lightbox)

Bu kullanarak işlenir bir `MSSession` tüm mevcut adımlar daraltmak için. Bu nedenle `DidSelectMessage` yöntemi `MSMessagesAppViewController` sınıfı şu şekilde görünür için değiştirilmesi:

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

Bir çıkış seçili iletiyi zaten varsa, `MSSession`, başka bir yeni kullanıldığı `MSSession` oluşturulur. `SummaryText` Özelliği `MSMessage` resim yazısı daraltılmış önceki adımı eklemek için kullanılır. Varsa `SummaryText` özelliği ayarlanmış `null`, konuşma önceki adımlarda konuşma dökümü tamamen kaldırılacak.

## <a name="advanced-message-api-features"></a>Gelişmiş ileti API özellikleri

Yukarıdaki ayrıntılı olarak ele yeni ileti API temel özellikleri ile sonraki Apple Framework'e yerleşik olan daha gelişmiş özelliklerden bazıları inceleyin.

İlk olarak, diğer birkaç geçersiz kılma yöntemleri vardır `MSMessagesAppViewController` konuşma daha derin erişimi sağlamak sınıfı:

- `DidStartSendingMessage` -Bu, kullanıcı Gönder düğmesi dokunur olduğunda çağrılır. Bu, ileti gönderme işlemi başlatıldı yalnızca alıcıya teslim için gerçekten bırakıldı gelmez.
- `DidCancelSendingMessage` -Bu, kullanıcı dokunur ortaya çıkar *X* konuşma dökümü içinde ileti Kabarcık üst sağ alt köşesindeki düğmesini.
- `DidReceiveMessage` -İleti uygulaması uzantısının etkin olduğunda bu yöntem çağrılır bir konuşma katılımcılar birinden yeni bir ileti alındı.

### <a name="group-conversations"></a>Grup sohbet

Bir ileti uygulama uzantısı Kullanıcıları grubu görüşmeleri (kişilerle 3 veya daha fazla) oynayan ve bu tasarlarken ve bir ileti uygulama uzantısı uygulama dikkate alınması gerekir kullanılabilir.

Aşağıdaki etkileşim üç kullanıcılarla Grup konuşmada göz atın:

[![](advanced-message-app-extensions-images/interactive14.png "Bir grup konuşma üç kullanıcılarla etkileşimi")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. 1 kullanıcı grubu etkileşimli ileti gönderir kullanıcı 2 ve kullanıcı 3 belirtti burger seçmenizi isteyen.
2. Kullanıcı 2 tomatoes seçer.
3. Kullanıcı 3 pickles seçer.
4. Hem kullanıcı 2'in hem de kullanıcı 3'ın seçimler kullanıcı 1'e neredeyse aynı anda ulaşır. Sonuç olarak, kullanıcı 2'in seçim bir Özet satırı daraltılmış ve kullanılamıyor. Bu durumda da çevrilmiş, kullanıcı 2'in seçimi ile gösterildikten ve kullanıcı 3 daraltılmış.

Kullanıcı 1 kullanıcı 2'in ve kullanıcı 3'ın seçeneklerini erişebilir olması gerektiği kadar her iki durumda da, bu istenmeyen bir davranıştır. Bu durumu yönetmek için Apple ileti uygulama uzantısı ileti durumu bulutta depolayan ve URL özelliğini kullanın öneren `MSMessage` (gönderdiği kullanıcılar arasında) bu durum erişmek için.

Kullanıcı bir ileti gönderdiğinde, bir oturum belirteci oluşturulur ve geçerli ileti durumu ile bulut gönderilir. Konuşma dökümü içinde ileti Kabarcık üzerinde bir kullanıcı dokunur, oturum belirteci buluttan geçerli oturum durumunu almak için kullanılır.

### <a name="sender-identifiers"></a>Gönderen tanımlayıcısı

Bir ileti gönderen tanımlayıcısını erişme tartışmak için yukarıda verilen bir grup konuşma örneği alın:

[![](advanced-message-app-extensions-images/interactive15.png "Grup konuşma tanımlayıcıları gönderme")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Kullanıcı 1 bir grup etkileşimli ileti yeniden gönderir kullanıcı 2 ve kullanıcı 3 belirtti burger seçmenizi isteyen.
2. Kullanıcı 3 pickles seçer.
3. Kullanıcı 3'ın seçeneği kullanıcı 1'e ulaştığında ve kullanıcı 2 değil henüz vermiş.
4. Apple çok kullanıcı gizliliği ile ilgili olduğu için ileti uygulama uzantısı yalnızca hakkında benzersiz bir tanımlayıcı bilir (olarak bir `NSUUID`) konuşma her Katılımcısı atanır. Yerel cihazda, yalnızca geçerli kullanıcının tanımlayıcı adı verilir.
5. `MSMessage` Sahip bir `SenderIdentifier` kullanıcı biriyle eşleşen özellik katılımcı listesinde bilinen uzantısı.
6. Her kullanıcı cihazına burada yalnızca yerel kullanıcı tanımlayıcısı yeniden bilinir Katılımcı listesi kopyasını sahiptir.
7. Bir ileti gönderildiğinde, kendi `SenderIdentifier` özelliği, yerel kullanıcı olarak da bilinir.

Gönderen tanımlayıcıları aşağıdaki şekillerde kullanılabilir:

- Katılımcı listesini bakarak uzantısı kullanıcı sayısını konuşmada elde edebilirsiniz.
- Uzantısı bir kullanıcıdan bir ileti aldığında, bunu gönderen tanımlayıcısını izlemek. Aynı gönderen tanımlayıcısını içeren başka bir ileti alır, uzantıyı aynı kullanıcıdan olduğunu bilir.
- Belirli bir kullanıcı konuşmadaki tanımlamaya yardımcı olmak için kullanılabilir.

Gönderen tanımlayıcısını metin alanlarının hiçbirinde kullanılabilir `MSMessageTemplateLayout` dolar işareti önek olarak (`$`). Örneğin:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Messages uygulamasının bu biçimlendirme türü ile bir ileti Kabarcık görüntülediğinde, yerini alacak `$uuid...` iletiyi gönderen konuşma Katılımcısı ilgili kişi adı.

Yeniden Not Yukarıdaki diyagramda bu kullanıcı 1 aygıt ve kullanıcı 3'ın aygıt bakarak her katılımcı konuşmadaki için farklı bir benzersiz gönderen tanımlayıcısı sahiptir gönderen her cihazda benzersiz tanımlayıcısıdır.

Gönderen tanımlayıcısı ileti uygulaması uzantısının yüklenmesi kapsamlıdır. Bu nedenle kullanıcı kaldırır ve ileti uygulama uzantısı yeniden yükler yeni gönderen tanımlayıcısı yeni yükleme için oluşturulur.

Gönderen tanımlayıcısı erişmek için uzantı aşağıdaki kodu kullanabilirsiniz:

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>Desteklenen Platformlar

Etkileşimli bir ileti uygulama uzantısı tarafından oluşturulan iletileri aşağıdaki Apple platformlarda teslim edilecek:

- watchOS 3
- macOS Sierra
- iOS 10

Üç platformları yalnızca iOS 10 kullanıcının etkileşimli iletisine oluşturmasına olanak sağlar. Kullanıcı etkileşimli bir ileti Kabarcık üzerinde tıklarsa macOS Sierra, URL bağlı `MSMessage` Safari ile açılacak ve iletinin gösterimini var. görüntülenmesi gerekir.

Messages uygulamasının watchOS üzerinde aktarmadan etkileşimli bir ileti kullanıcının yanıt nerede oluşturabilirsiniz bir bağlı iOS cihazına olabilir.

Etkileşimli iletisi eski Apple platformlarda aldıysanız yeni iletiler API geri dönüş için destek vardır:

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

Bunlar bir geri dönüş biçiminde iki ayrı iletiler teslim edilir:

- Şablon düzeni tarafından sağlanan bir görüntüyü olacaktır.
- Sağlanan diğer URL olacaktır `MSMessage`.

## <a name="summary"></a>Özet

Bu makale ile tümleşen bir Xamarin.iOS çözümüne iletisi uygulama uzantıları ile çalışmak için gelişmiş teknikleri sunulan **iletileri** uygulama ve kullanıcıya mevcut yeni işlevsellik.


## <a name="related-links"></a>İlgili bağlantılar

- [Dondurma Oluşturucusu (örnek)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [İletileri başvurusu](https://developer.apple.com/reference/messages)
- [Uygulama Uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
