---
title: Mtouch paketini Xamarin.iOS uygulamaları için kullanma
description: Bu belgede mtouch, bir aracı adımlarının çoğunu bir paket bir Xamarin.iOS uygulamasına etkinleştirmek için gerekli sürücüler benzeticisinde başlatın ve bir fiziksel cihaza dağıtma açıklanır.
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 49507b6500755c617deacfb197ef089e3debd598
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351362"
---
# <a name="using-mtouch-to-bundle-xamarinios-apps"></a>Mtouch paketini Xamarin.iOS uygulamaları için kullanma

iPhone uygulamaları, uygulama paketleri gönderilir. Uzantı dizinlerle bunlar `.app` kodunuzu, verileri, yapılandırma dosyalarını ve iPhone uygulamanızı hakkında bilgi edinmek için kullandığı bir bildirim içerir.

Bir .NET çalıştırılabilir bir uygulamaya kapatma işlemi çoğunlukla tarafından yönlendirilen `mtouch` komutu, bir araç olan birçok bir paket uygulamaya etkinleştirmek için gerekli adımları tümleştirir. Bu araç, aynı zamanda simulator'da uygulamanızı başlatın ve bir gerçek iPhone veya iPod Touch cihaza yazılım dağıtmak için kullanılır.

## <a name="detailed-instructions"></a>Ayrıntılı yönergeler

Denetleme bizim [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) el kitabı sayfası tüm olası mtouch aracını kullanır.

## <a name="installation"></a>Yükleme

Bir Mac bilgisayarda `mtouch` Xamarin.iOS ile sağlanır. Şu dizinde bulunabilir:

**/Library/Frameworks/Xamarin.iOS.Framework/Versions/Current/bin**

Yapmak `mtouch` kullanmak, sisteminizin üst dizinini ekleyin kullanışlı `PATH` ortam değişkeni.  

Örneğin, bunu Bash'te yapmak için sonuna aşağıdaki satırı ekleyin, **~/.bash_profile** dosyası:

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> Kullanılacak `mtouch`, varlığını güvenmeyin **/Developer/MonoTouch/usr/bin**, işaret eden bir sembolik bağlantıyı **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**. Eski MonoTouch uyumluluğunu serbest korumak için de yüklenmedi yalnızca bu sembolik bağlantıyı var. **/Library/Frameworks/...** , ve gelecekteki bir sürümde kaybolabilir.

## <a name="building"></a>Oluşturma

`mtouch` Komut üç farklı yolla kodunuzu derlemek:

-  Simülatör test etmek için derleyin.
-  Cihazı dağıtım için derleyin.
-  Yürütülebilir dosyanın cihaza dağıtın.


### <a name="building-for-the-simulator"></a>Simülatör için oluşturma

Kullanmaya başlayın, en yaygın kullanılan senaryo kullanacaksınız böylece simulator'da, uygulamayı denemek için olacak `mtouch -sim` simülatör pakete Kodu derlemek için. Bu yapılır şöyle:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Cihaz için oluşturma

Cihaz için yazılım kullanarak uygulamanızı oluşturacağınız `mtouch -dev` seçeneği, ayrıca gereken uygulamanızı imzalamak için kullanılan sertifikanın adını sağlayın. Cihaz için uygulamanın nasıl oluşturulduğunu gösterir:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

Bu durumda kullanıyoruz "iPhone Geliştirici: Miguel de Icaza" uygulamayı imzalama sertifikası. Bu adım çok önemlidir ve fiziksel cihaz, uygulama yüklemek reddeder.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Uygulamanızı çalıştırma


### <a name="launching-on-the-simulator"></a>Simulator'da başlatılıyor

Bir uygulama paketi oluşturduktan sonra üzerinde simülatörü başlatılırken çok basittir:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Varsa `--sdkroot` bayrağı ayarlanmamışsa, varsayılan xcode-select yolu ve aşağıdaki uyarıyı neden olur:

> örn: MT0061 Uyarı: Hayır (--sdkroot kullanarak), belirtilen Xcode.app Xcode 'xcode-select tarafından--print-path' bildirilen sistemiyle: /Applications/Xcode.app/Contents/Developer 

Yukarıdaki komut satırı gibi bir çıktı oluşturur:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Ayrıca, hata ayıklamaya yardımcı olmak için standart çıktı ve standart hata dosyaları günlüğünü tutun önemle tavsiye edilir. Çıkışı `Console.WriteLine` gider `stdout`ve çıktısını `Console.Error.WriteLine` ve başka bir çalışma zamanı hata iletileri gider `stderr`.

Bunu yapmak için `--stdout` ve `--stderr` bayraklar:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Uygulamanızın başarısız olursa, sorunu tanılamak için hata ve çıkış görebilirsiniz.


### <a name="deploying-to-a-device"></a>Bir cihaza dağıtma

Cihazınıza dağıtmak için Apple içinde açıklandığı gibi Cihazınızı sağlamak gereken [cihazları yönetme](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) belge. Cihazınız düzgün sağlandıktan sonra cihazınızın içine derlenmiş ".app" dağıtılacağı mtouch komutu kullanabilirsiniz. Bu komutu kullanarak bunu yapabilirsiniz:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Varsa `--sdkroot` bayrağı ayarlanmamışsa, varsayılan xcode-select yolu ve aşağıdaki uyarıyı neden olur:

> örn: MT0061 Uyarı: Hayır (--sdkroot kullanarak), belirtilen Xcode.app Xcode 'xcode-select tarafından--print-path' bildirilen sistemiyle: /Applications/Xcode.app/Contents/Developer 

Bu adımlar genellikle Mac için Visual Studio tarafından gerçekleştirilir

## <a name="reference"></a>Başvuru

Bkz: [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) diğer komut satırı seçenekleri hakkında daha fazla ayrıntı için el kitabı sayfası.



## <a name="related-links"></a>İlgili bağlantılar

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
