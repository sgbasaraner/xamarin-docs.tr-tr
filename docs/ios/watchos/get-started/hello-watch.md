---
title: Merhaba, izleme
description: "Xamarin ve watchOS ile çalışmaya başlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 732fd9808ded4bf97e99e7ab0e6ab63b989452d1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="hello-watch"></a>Merhaba, izleme

Bir çözüm adımlarını izleyerek oluşturduktan sonra [Kurulum ve yükleme](~/ios/watchos/get-started/installation.md), 3 proje gerekir:

- Kurulum veya diğer cihaz üzerinde yönetim görevleri için kullanılan iOS üst uygulaması. (İOS uzantıları diğer türleri ile bu genellikle "Kapsayıcı" uygulama olarak adlandırılır.) Gözcü uygulamalarla olmadan izleme uygulamayı çalıştıran başlatmak kullanıcılar için mümkün olacaktır **hiç** üst uygulama; çalışan
- İzleme uygulama program kodunu içeren izleme uzantısı; ve
- Gözcü saatin işlendiğini film şeridi ve görüntü kaynakları tutan uygulama.

Denetleyin, [başvuruları doğru](~/ios/watchos/get-started/project-references.md): üst uygulama uzantısı bir başvuru içeriyor ve uzantı izleme uygulama için bir başvuru içeriyor.

Paket tanımlayıcıları uymanızı onaylayın \*.watchkitextension \*.watchkitapp kuralı ve bilgilerinizi uzantının Info.plist dosyasını olduğunu sahip **WKApp paket kimliği** değerini ayarlamak için paket tanıtıcısı Gözcü uygulamanızı.

Gözcü uygulamanız Şimdi Çalıştır gerekir, ancak film şeridi dosya İzle uygulamanızı içinde boş olduğundan, söyleyin bağlanamayacak.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-watch-images/projectstructure.png "Çözüm Gezgini")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "Çözüm Gezgini")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
Xamarin iOS Tasarımcısı başlatmak için izleme uygulamanızda Interface.storyboard çift tıklayın (Mac üzerinde olması durumunda da sağ tıklayarak ve **birlikte Aç > Xcode arabirimi Oluşturucu**)


1.  Olun **araç** ve **özellikleri** klavye takımı görünür
1.  Arabirim Denetleyicisi seçmek için tıklatın
1.  Tanımlayıcı ve arabirimi denetleyicisinin başlığını ayarla **interfaceController** ve **Hi izleme**,
1.  Doğrulama **sınıfı** ayarlanır **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "Tanımlayıcı ve başlık arabirimi denetleyicisinin interfaceController ve yüksek izleme ayarlayın")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin iOS Visual Studio'da Tasarımcısı ile düzenlemek için izleme uygulamanızda Interface.storyboard çift tıklayın:

1.  Özellikler bölmesinde açın;
1.  Sınıfa değiştirme **InterfaceController**;
1.  Arabirim Denetleyicisi tıklayın; ve
1.  Tanımlayıcı ve arabirimi denetleyicisinin başlığını ayarla **interfaceController** ve **Hi izleme**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Tanımlayıcı ve başlık arabirimi denetleyicisinin interfaceController ve yüksek izleme ayarlayın")

-----


UI oluşturun:

1. Gelen **araç** paneli
1. Sürükleme ve bırakma bir **düğmesini** ve **etiket** Sahne üzerine ve
1. Metin ve gösterildiği gibi denetimleri özniteliklerini ayarlayın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-watch-images/draganddrop.png "Metin ve gösterildiği gibi denetimleri özniteliklerini ayarlama")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "Metin ve gösterildiği gibi denetimleri özniteliklerini ayarlama")

-----

1. Ayarlama **adı** içinde **özellikleri** paneli her denetim için. Bu örnekte kullandığımız `myButton` ve `myLabel`.

1. Şeridinde düğmesini seçin ve Git **özellikleri** paneli'nın **olayları** listesinde, ardından

1. Yeni bir **eylem** yazarak `OnButtonPress` tuşuna basarak **Enter**.
  Eylem listesinde görünür ve kısmi yöntemi otomatik olarak C# içinde oluşturulur.

![](hello-watch-images/buttonaction.png "Bir düğme OnButtonPress eylem eklendi")

Film şeridi kaydettikten sonra **InterfaceController.designer.cs** denetim adlarının ve eylemleri güncelleştirilir... Bu güncelleştirilmiş sonra bu dosyayı açarsanız, gördüğünüz nasıl `RegisterAttribute` denetleyici ve kullanıcı Arabirimi denetimlerini örnek ile işaretlenen C# değişkenleri nasıl eşleştiğini karşılık gelen `OutletAttribute` ve kısmi yöntemler nasıl Harita eylemleri ile etiketlenmiş `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

Şimdi açmak **InterfaceController.cs** (*değil* InterfaceController.designer.cs) ve aşağıdaki kodu ekleyin:

```csharp
int clickCount = 0;

partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}

```

Bu kodu oldukça saydam olmalıdır: örnek değişkeni `clickCount` olduğu işlevi her zaman artar `OnButtonPress` olarak adlandırılır. Metnin `myLabel` bu sayı; yansıtacak şekilde değiştirildi `myLabel`, doğal olarak, Xcode'da oluşturduğunuz çıkışlar birini adıdır. `partial` İşlevi, belirtilen eylem adı ile ilişkili işlevi uygulamasıdır.

Zaten başlangıç projesi değilse,

1. İzleme uzantısı projenize sağ tıklayın ve seçin **başlangıç projesi olarak ayarla**,

1. Gözcü Seti uyumlu simulator görüntüsüne (örneğin, iPhone 6 iOS 8.2), dağıtım hedefini ayarlama

1. Tuşuna **hata ayıklama** yapı ve simulator başlatma tetiklemek için düğmesi.

    [![](hello-watch-images/readytodebug-sml.png "Visual Studio arabirim öğeleri")](hello-watch-images/readytodebug.png#lightbox)

Simulator başlattığında, etiket artırılacağını düğmesine basın.
Tebrikler, kendinize bir izleme uygulaması var!

![](hello-watch-images/running.png "Benzetici çalışan uygulama")


## <a name="related-links"></a>İlgili bağlantılar

- [Başlarken (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Kurulum ve Yükleme](~/ios/watchos/get-started/installation.md)
- [İlk izleme uygulama video](http://blog.xamarin.com/your-first-watch-kit-app/)
