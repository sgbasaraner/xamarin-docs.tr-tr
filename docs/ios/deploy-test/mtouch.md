---
title: mtouch
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b1b61e7ce1bae413f132cfe1e6c051a53b786f98
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="mtouch"></a>mtouch


iPhone uygulamalar, uygulama paketleri gönderilir. Dizinleri uzantılı bunlar `.app` kodunuzu, veriler, yapılandırma dosyalarını ve iPhone uygulamanız hakkında bilgi edinmek için kullandığı bir bildirimi içerir.

Bir uygulamaya bir .NET yürütülebilir dosyayı açma işlemi çoğunlukla tarafından yönlendirilen `mtouch` komutu, birçok bir paket uygulamaya açmak için gerekli adımları tümleştirir bir araç. Bu araç, uygulamanızı simulator başlatın ve gerçek iPhone veya iPod Touch cihazına yazılım dağıtmak için de kullanılır.


## <a name="detailed-instructions"></a>Ayrıntılı yönergeler

Denetleme bizim [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) tüm olası el ile sayfa mtouch aracını kullanır.


## <a name="building"></a>Oluşturma

`mtouch` Komutu üç farklı yolla kodunuzu derleyin:

-  Simulator test etmek için derleyin.
-  Aygıt dağıtımı için derleme.
-  Yürütülebilir dosyanın cihaza dağıtabilirsiniz.


### <a name="building-for-the-simulator"></a>Simülatörü oluşturma

Başlatıldığında en yaygın kullanılan senaryo Simulator uygulamada kullanacağınız şekilde denemek için olacaktır `mtouch -sim` simulator pakete Kodu derlemek için. Bu yapılır şöyle:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Aygıt için oluşturma

Cihaz için yazılım oluşturmak için kullanarak uygulamanızı oluşturacaksınız `mtouch -dev` seçeneği, ayrıca, uygulamanızı imzalamak için kullanılan sertifikanın adı sağlamanız gereken. Aşağıdaki uygulama aygıt için nasıl yapılandırıldığını gösterir:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

Bu örnekte kullanıyoruz "iPhone Geliştirici: Miguel de Icaza" uygulamayı imzalama sertifikası. Bu adım çok önemlidir ve fiziksel cihaz uygulamayı yüklemek reddeder.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Uygulamanızı çalıştırma


### <a name="launching-on-the-simulator"></a>Simulator'da başlatma

Bir uygulama paketini olduktan sonra simulator'da başlatma çok basittir:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Varsa `--sdkroot` bayrağı ayarlı değil, xcode seçin yolu ve aşağıdaki uyarı sonucu varsayılanlara olur:

> Örneğin: MT0061 Uyarı: Hayır (--sdkroot kullanarak), belirtilen Xcode.app Xcode 'xcode-göre Seç--yazdırma yolu' bildirilen sistemiyle: /Applications/Xcode.app/Contents/Developer 

Yukarıdaki komut satırı şuna benzer bir çıktı üretir:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Ayrıca, hata ayıklamaya yardımcı olması için standart çıktı ve standart hata dosyaları günlüğü tutun önerilir. Çıktısını `Console.WriteLine` gider `stdout`ve çıktısını `Console.Error.WriteLine` ve başka bir çalışma zamanı hata iletileri gider `stderr`.

Bunu yapmak için kullanın `--stdout` ve `--stderr` bayrakları:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Uygulamanızı başarısız olursa, sorunu tanılamak için hata ve çıktı görebilirsiniz.


### <a name="deploying-to-a-device"></a>Bir cihaza dağıtma

Aygıtınıza dağıtmak için Apple içinde açıklandığı gibi Cihazınızı sağlamak gerekir [aygıtları yönetme](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) belge. Cihazınızı düzgün bir şekilde sağlanmış sonra derlenmiş ".app" Cihazınızı dağıtmak için mtouch komutunu kullanabilirsiniz. Bu komutu kullanarak bunu yapabilirsiniz:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Varsa `--sdkroot` bayrağı ayarlı değil, xcode seçin yolu ve aşağıdaki uyarı sonucu varsayılanlara olur:

> Örneğin: MT0061 Uyarı: Hayır (--sdkroot kullanarak), belirtilen Xcode.app Xcode 'xcode-göre Seç--yazdırma yolu' bildirilen sistemiyle: /Applications/Xcode.app/Contents/Developer 

Bu adımlar genellikle Mac için Visual Studio tarafından gerçekleştirilir

## <a name="reference"></a>Başvuru

Bkz: [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) diğer komut satırı seçenekleri hakkında ayrıntılar için el ile sayfa.



## <a name="related-links"></a>İlgili bağlantılar

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
