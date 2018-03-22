---
title: watchOS sorun giderme
description: "Bilinen sorunlar ve watchOS geliştirme sorunlara yönelik geçici çözümler."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4a6b916f991b337d8a28764f1482ddd837bad460
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="watchos-troubleshooting"></a>watchOS sorun giderme

Bu sayfa, ek bilgi ve halen geliştirilme özellikleri için geçici çözümler içerir. Bu çözümler bazıları yalnızca bizim Önizleme sürümleri için geçerlidir.

- [Bilinen Sorunlar](#knownissues)

- [Alfa kanal simgesi görüntülerden kaldırma](#noalpha)

- [Arabirim Denetleyicisi dosyaları el ile ekleme](#add) Xcode arabirimi Oluşturucu için.

- [Komut satırından WatchApp başlatma](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Bilinen Sorunlar

### <a name="general"></a>Genel

<a name="deploy" />

- Mac için Visual Studio'nun önceki sürümleri yanlış Göster birini **AppleCompanionSettings** sonuçlanır 88 x 88 piksel; olarak simgeleri bir **eksik simgesi hata** uygulamaya göndermek çalışırsanız Deposu.
    Bu simge 87 x 87 piksel olmalıdır (29 birimleri için  **@3x**  Retina ekranlar). Bu Mac - ya da Düzenle xcode'da görüntü varlığı için Visual Studio'da düzeltin veya el ile düzenlediğinizde **Contents.json** dosyası (eşleşecek şekilde [Bu örnek](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Varsa izleme uzantısı projenin **Info.plist > WKApp paket kimliği** değil [doğru olarak ayarlanmış](~/ios/watchos/get-started/project-references.md) izleme uygulamanın eşleşecek şekilde **paket kimliği**, hata ayıklayıcı bağlanamaz ve görsel Mac için Studio iletiyle bekleyecek *"bağlanmak için hata ayıklayıcı bekleniyor"*.

- Hata ayıklama içinde desteklenir **bildirimleri** modu ancak güvenilir olabilir. Yeniden deneniyor bazen çalışır. Onaylayın izleme uygulamanın **Info.plist** `WKCompanionAppBundleIdentifier` iOS üst/kapsayıcı uygulamanın paket tanımlayıcısı eşleşecek şekilde ayarlayın (IE. iPhone üzerinde çalışan bir).

- iOS Tasarımcısı bakışta veya bildirim arabirim denetleyicilerini entrypoint okları göstermez.

- İki ekleyemezsiniz `WKNotificationControllers` film şeridi için.
    Geçici çözüm: `notificationCategory` film şeridi XML öğesinde her zaman eklenir ile aynı `id`. İki (veya daha fazla) bildirim denetleyicileri ekleyebilir, film şeridi dosyasını bir metin düzenleyicisinde açın ve el ile değiştirmeniz bu sorunu çözmek için `id` öğesi benzersiz olmalıdır.

    [![](troubleshooting-images/duplicate-id-sml.png "Film şeridi dosya bir metin düzenleyicisinde açıp benzersiz olmasını id öğesi el ile değiştirmeniz")](troubleshooting-images/duplicate-id.png#lightbox)

- "Uygulama değil oluşturulduğunu" bir hata görebilirsiniz uygulamayı başlatmak çalışırken. Bundan sonra oluşan bir **temiz** izleme uzantısı projeye başlangıç projesi ayarlandığında.
    Düzeltme seçmektir **Yapı > yeniden tüm** ve uygulamayı yeniden başlatın.

### <a name="visual-studio"></a>Visual Studio

İOS Tasarımcısı için izleme paketi desteklemek *gerektirir* çözümü doğru şekilde yapılandırılmış. Proje başvurularını ayarlanmamışsa (bkz [başvuruları ayarlamak nasıl](~/ios/watchos/get-started/project-references.md)) sonra Tasarım yüzeyine düzgün çalışmaz.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Alfa kanal simgesi görüntülerden kaldırma

Simgeler (alfa kanal görüntünün saydam alanlarını tanımlayan) alfa kanalı içermemesi gerekir, aksi takdirde uygulama şuna benzer bir hata ile uygulama mağazası gönderme sırasında reddedilir:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Mac OS X kullanma alfa kanal kaldırmak daha kolaydır **Önizleme** uygulama:

1. Simge görüntüde açın **Önizleme** ve ardından **dosyası > dışarı aktarma**.

2. Görüntülenen iletişim kutusunu içerecek bir **alfa** alfa kanal varsa, onay kutusu.

    ![](troubleshooting-images/remove-alpha-sml.png "Alfa kanal varsa, açılan iletişim alfa onay kutusu içerir")

3. *Untick* **alfa** onay kutusunu ve **kaydetmek** dosyayı doğru konuma.

4. Simge görüntüsü artık Apple'nın doğrulama denetimlerini geçip.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Arabirim Denetleyicisi dosyaları el ile ekleme

> [!IMPORTANT]
> Tasarımcıda özetlenen adımları gerektirmeyen iOS (hem Mac için Visual Studio hem de Visual Studio), izleme film şeritleri tasarlama Xamarin'ın WatchKit destek içerir. Mac özellikleri pad için yalnızca bir arabirim denetleyicisi bir sınıf adı Visual Studio'da verin ve C# kod dosyaları otomatik olarak oluşturulur.


*Varsa* Xcode arabirimi Oluşturucusu'nu kullanarak, izleme uygulamanız için yeni arabirim denetleyicilerini oluşturmak ve çıkışlar ve eylemleri C# dilinde kullanılabilir olacak şekilde, Xcode ile eşitlemeyi etkinleştirmek için aşağıdaki adımları izleyin:

1. Gözcü uygulamanın açmak **Interface.storyboard** içinde **Xcode arabirimi Oluşturucu**.
    
    ![](troubleshooting-images/add-6.png "Film şeridi Xcode arabirimi Oluşturucusu'nda açma")

2. Yeni bir sürükleyin `InterfaceController` film şeridi üzerine:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. Şimdi denetimler arabirimi denetleyicide (ör. sürükleyebilirsiniz etiketleri ve düğmeleri) ancak olduğundan çıkışlar veya Eylemler henüz oluşturamazsınız hiçbir **.h** üstbilgi dosyası. Aşağıdaki adımlar gerekli neden olacak **.h** üstbilgi dosyası oluşturulacak.

    ![](troubleshooting-images/add-2.png "Düzeninde bir düğme")

4. Film şeridi kapatın ve Mac için Visual Studio'ya geri dönün Yeni bir C# dosyası oluşturma **MyInterfaceController.cs** (veya istediğiniz ad) içinde **izleme uygulaması uzantısı** proje (değil izleme uygulama film şeridi olduğu). (Ad, classname ve Oluşturucusu adı güncelleştirme) aşağıdaki kodu ekleyin:

        using System;
        using WatchKit;
        using Foundation;
        
        namespace WatchAppExtension  // remember to update this
        {
            public partial class MyInterfaceController // remember to update this
            : WKInterfaceController
            {
                public MyInterfaceController // remember to update this
                (IntPtr handle) : base (handle)
                {
                }
                public override void Awake (NSObject context)
                {
                    base.Awake (context);
                    // Configure interface objects here.
                    Console.WriteLine ("{0} awake with context", this);
                }
                public override void WillActivate ()
                {
                    // This method is called when the watch view controller is about to be visible to the user.
                    Console.WriteLine ("{0} will activate", this);
                }
                public override void DidDeactivate ()
                {
                    // This method is called when the watch view controller is no longer visible to the user.
                    Console.WriteLine ("{0} did deactivate", this);
                }
            }
        }

5. Başka bir yeni C# dosyası oluşturma **MyInterfaceController.designer.cs** içinde **izleme uygulaması uzantısı** proje ve aşağıdaki kodu ekleyin. Ad alanı, classname güncelleştirdiğinizden emin olun ve `Register` özniteliği:

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;
    
    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```
    
    İpucu: Bu diğer C# dosyasında Visual Studio üzerine Mac çözüm Pad için sürükleyerek ilk dosya alt düğümü dosya (isteğe bağlı) yapabilirsiniz. Bunu daha sonra şu şekilde görünür:
    
    ![](troubleshooting-images/add-5.png "Çözüm paneli")

6. Seçin **Yapı > Yapı tüm** Xcode eşitleme yeni sınıf tanıyabilmesi için (aracılığıyla `Register` özniteliği) kullanılan.

7. Film şeridi izleme uygulama film şeridi dosya üzerinde sağ tıklayıp seçerek yeniden açın **birlikte Aç > Xcode arabirimi Oluşturucu**:

    ![](troubleshooting-images/add-6.png "Film şeridi arabirimi Oluşturucusu'nda açma")

8. Yeni arabirimi denetleyicinizi seçin ve yukarıdaki ör tanımladığınız classname verin. `MyInterfaceController`.
Her şeyin doğru şekilde çalıştıysa otomatik görünmelidir **sınıfı:** açılan liste ve buradan seçebilirsiniz.

    ![](troubleshooting-images/add-4.png "Özel bir sınıf ayarlama")

9. Seçin **Yardımcısı Düzenleyicisi** görünümünde Xcode (iki örtüşen daire simgesiyle) böylece film şeridi ve kod yan yana görebilirsiniz:

    ![](troubleshooting-images/add-7.png "Yardımcısı Düzenleyicisi araç çubuğu öğesi")

    Odağı code bölmesinde olduğunda olduğunuz bakabilir olun **.h** üstbilgi dosyası ve içerik haritası çubuğunda sağ tıklatın ve doğru dosyayı seçin (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "MyInterfaceController seçin")

10. Artık çıkışlar ve eylemleri tarafından oluşturabilirsiniz **Ctrl + Sürükle** film şeridi gelen **.h** üstbilgi dosyası.

    ![](troubleshooting-images/add-9.png "Çıkışlar ve eylemleri oluşturma")

    Sürükle bıraktığınızda prizine veya bir eylem oluşturulup oluşturulmayacağını seçmek için istenir ve adını seçin:

    ![](troubleshooting-images/add-a.png "Çıkış ve bir eylem iletişim kutusu")

11. Film şeridi değişiklikler kaydedilir ve Xcode kapatıldıktan sonra Mac için Visual Studio'ya geri dönün Bu başlığı dosya değişiklikleri algılar ve otomatik olarak kod ekleme **. designer.cs** dosyası:


        [Register ("MyInterfaceController")]
        partial class MyInterfaceController
        {
            [Outlet]
            WatchKit.WKInterfaceButton myButton { get; set; }
        
            void ReleaseDesignerOutlets ()
            {
                if (myButton != null) {
                    myButton.Dispose ();
                    myButton = null;
                }
            }
        }


Şimdi denetimi başvuru (veya eylemi uygulamak) C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Komut satırından izleme uygulama başlatma

> [!IMPORTANT]
> Varsayılan olarak ve ayrıca normal uygulama modunda izleme uygulama başlatabilirsiniz **bakışta** veya **bildirim** kullanarak modları [özel yürütme parametreleri](~/ios/watchos/get-started/installation.md#custommodes) Mac için Visual Studio ve Visual Studio.


İOS simülatörü'nü denetlemek için komut satırını da kullanabilirsiniz. İzleme uygulamaları başlatmak için kullanılan komut satırı aracıdır **mtouch**.

(Terminal tek bir satır olarak yürütülen) tam bir örnek aşağıda verilmiştir:

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Uygulamanızı gösterecek şekilde güncellemeniz gerekir parametresi `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Ana uygulama paketi tam yolunu *izleme uygulama ve uzantısını içeren iOS uygulaması*.

> [!NOTE]
> Yolun sağlamanız gerekir içindir *iPhone uygulama .app dosyası*, yani iOS simülatörü dağıtılacak ve izleme uzantısı ve izleme uygulama içeren bir.

Örnek:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>Bildirim modu

Uygulamanın test etmek için [ **bildirim** modu](~/ios/watchos/platform/notifications.md)ayarlayın `watchlaunchmode` parametresi `Notification` ve test bildirim yükü içeren bir JSON dosyası için bir yol sağlayın.

Yükü parametresi *gerekli* bildirim modu.

Örneğin, bu bağımsız değişkenler mtouch komutu ekleyin:

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>Başka bir bağımsız değişken

Kalan bağımsız değişkenleri, aşağıda açıklanmıştır:

### <a name="--sdkroot"></a>--sdkroot

Gerekli. Xcode (6.2 veya üstü) yolunu belirtir.

Örnek:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--device

Yürütülecek simulator aygıt. Bu, belirli bir aygıt udid kullanarak veya bir çalışma zamanı ve cihaz türü kullanan iki yolla belirtilebilir.

Değerleri tam makineler arasında değişir ve Apple'nın aracılığıyla sorgulanabilir **simctl** aracı:

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Örnek:

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Çalışma zamanı ve cihaz türü**

Örnek:

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```



## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
