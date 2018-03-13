---
title: Sorun giderme
description: "Bu makalede kapsar Xamarin'ın tvOS desteği ile çalışırken karşılaşabileceğiniz sorunları bildirin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7b6d0901f8b01668626fc3b6a70a091e99e2287e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshooting"></a>Sorun giderme

_Bu makalede kapsar Xamarin'ın tvOS desteği ile çalışırken karşılaşabileceğiniz sorunları bildirin._

<a name="Known-Issues" />

## <a name="known-issues"></a>Bilinen Sorunlar

Xamarin'ın tvOS desteği geçerli sürümünde aşağıdaki bilinen sorunlar vardır:

- **Mono Framework** – Mono 4.3 Cryptography.ProtectedData başarısız Mono 4.2 gelen verilerin şifresini çözmek. Sonuç olarak, NuGet paketlerini şu hata ile geri yükleme başarısız olur `Data unprotection failed` korumalı NuGet kaynağı yapılandırıldığında.
    - **Geçici çözüm** – herhangi bir NuGet paket kaynaklarını paketlerini geri yüklemek yeniden denemeden önce bu parola kimlik doğrulamasını kullan geri eklemeniz gerekiyor Mac için Visual Studio.
- **Mac içeren F # eklenti için Visual Studio** – F # Android şablonu Windows oluşturulurken hata oluştu. Bu hala düzgün Mac üzerinde çalışmalıdır
- **Xamarin.Mac** – Xamarin.Mac birleşik şablon proje hedef Framework çalıştıran kümesine `Unsupported`, açılan `Could not connect to the debugger` görünebilir.
    - **Olası geçici çözüm** – bizim kararlı kanalda kullanılabilir Mono framework sürüm düşürmek.
- **Xamarin Visual Studio & Xamarin.iOS** – Visual Studio'da hata WatchKit uygulamaları dağıtırken `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` görünebilir.

Lütfen herhangi hataları rapor bulmak için [Bugzilla](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki bölümlerde tvOS 9 Xamarin.tvOS ve bu sorunların çözümü ile kullanıldığında oluşabilir bazı bilinen sorunlar listelenmektedir:

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>Geçersiz çalıştırılabilir - bitcode'u yürütülebilir dosya içermiyor

Apple TV uygulama mağazası Xamarin.tvOS uygulama göndermeye çalışırken bir hata iletisi biçiminde alabilirsiniz _"Geçersiz çalıştırılabilir - yürütülebilir dosya içermiyor bitcode'u"_.

Bu sorunu çözmek için aşağıdakileri yapın:

1. Mac için Visual Studio'da Xamarin.tvOS proje dosyanızda sağ **Çözüm Gezgini** seçip **seçenekleri**.
2. Seçin **tvOS yapı** ve üzerinde olduğundan emin olun **sürüm** yapılandırma: 

    [![](troubleshooting-images/ts01.png "TvOS derleme seçenekleri seçin")](troubleshooting-images/ts01.png#lightbox)
3. Ekleme `--bitcode=asmonly` için **ek mtouch bağımsız değişkenleri** alanına gelin ve **Tamam** düğmesi.
4. Uygulamanızı yeniden **sürüm** yapılandırma.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>Doğrulama, tvOS uygulama Bitcode'u içerir

Xamarin.tvOS uygulamanızı Bitcode'u içerdiğini doğrulamak için Terminal uygulamasını açın ve aşağıdakileri girin:

```csharp
otool -l /path/to/your/tv.app/tv
```

Çıktıda aşağıdakiler için bakın:

```csharp
Section
  sectname __bundle
   segname __LLVM
      addr 0x0000000100001000
      size 0x000000000000124f
    offset 4096
     align 2^0 (1)
    reloff 0
    nreloc 0
     flags 0x00000000
 reserved1 0
 reserved2 0
```

`addr` ve `size` farklı olacaktır, ancak diğer alanlar aynı olmalıdır.

Emin olmak gerekir statik herhangi bir üçüncü taraf (`.a`) kullanmakta olduğunuz kitaplıkları tvOS kitaplıkları (IOS kitaplıkları değil) karşı yerleşik ve bunlar da, bitcode'u bilgileri içerir.

Uygulamalar veya geçerli bitcode'u dahil kitaplıkları `size` birden büyük olacaktır. Burada bir kitaplık bitcode'u işaret sahip, ancak geçerli bitcode'u içermemesi bazı durumlar vardır. Örneğin:

**Geçersiz Bitcode'u**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**Geçerli Bitcode'u** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

Fark Not `size` listelenen örnekte iki kitaplıkları arasında çalışan üstünde. Kitaplık Xcode arşiv derleme etkin bitcode'u ile oluşturulan gerekir (Xcode ayarı `ENABLE_BITCODE`) bu boyutu soruna bir çözüm olarak.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Yalnızca arm64 dilim içeren uygulamalar Info.plist UIRequiredDeviceCapabilities listesinde "arm64" de sahip olmalıdır

Apple TV App Store yayını için bir uygulamaya gönderilirken bir hata biçiminde alabilirsiniz:

_"Yalnızca arm64 dilim içeren uygulamalar ayrıca"arm64"Info.plist UIRequiredDeviceCapabilities listesinde olmalıdır"_

Bu gerçekleşirse, düzenleme, `Info.plist` dosya ve aşağıdaki anahtarları sahip olduğundan emin olun:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

Uygulamanız için yayın derlenir ve iTunes Bağlan yeniden gönderin.

### <a name="task-mtouch-execution----failed"></a>Görev "MTouch" yürütme--başarısız oldu

3 taraf kitaplığı (örneğin, MonoGame) kullanıyorsanız ve yayın derleme başarısız ile biten hata iletileri uzun bir dizi `Task "MTouch" execution -- FAILED`, eklemeyi deneyin `-gcc_flags="-framework OpenAL"` için **ek dokunma bağımsız değişkenleri**:

[![](troubleshooting-images/mtouch01.png "Görev MTouch yürütme")](troubleshooting-images/mtouch01.png#lightbox)

Ayrıca içermelidir `--bitcode=asmonly` içinde **ek dokunma bağımsız değişkenleri**, bağlayıcı seçeneklerini belirlemek **bağlantı tüm** ve temiz bir derleme yapın.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS-90471 error. Büyük simge eksik.

"ITMS 90471 hata. biçiminde bir ileti alırsanız Büyük simge Xamarin.tvOS uygulama sürümü için Apple TV uygulama mağazası göndermeye çalışırken eksik"Lütfen denetleyin aşağıdaki:

1. Büyük simge varlıkları dahil olmak, `Assets.car` kullanılarak oluşturulan dosya [uygulama simgeleri](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) belgeleri.
2. Bunun dahil olmak `Assets.car` dosya [simgeleri ve görüntüleri çalışma](~/ios/tvos/app-fundamentals/icons-images.md) son uygulama paket belgelerinde.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>Geçersiz paket – oyun denetleyicileri destekleyen bir uygulama aynı zamanda uzak Apple TV desteklemesi gerekir

veya 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>Uygulamanın Info.plist dosyasında geçersiz paket – Apple TV uygulamaları oyun denetleyicisi framework ile GCSupportedGameControllers anahtar içermelidir

Oyun denetleyicileri oyunlar geliştirmek ve oyun içinde tümüyle yoğunlaşabilmek duygusu sağlamak için kullanılabilir. Kullanıcının uzak ve denetleyici arasında geçiş yapmak yoksa şekilde standart Apple TV arabirimi denetlemek için de kullanılabilir.

Apple TV uygulama mağazası oyun denetleyicileri destek Xamarin.tvOS uygulamayla gönderiliyor ve bir hata iletisi biçiminde alma varsa:


_Biz, son teslim "uygulama adı" için bir veya daha fazla sorunlar bulundu. Teslim başarılı oldu, ancak sonraki teslim aşağıdaki sorunları düzeltmek isteyebilir:_

_Geçersiz paket – oyun denetleyicileri destekleyen bir uygulama aynı zamanda uzak Apple TV desteklemesi gerekir._

veya 

_Geçersiz paket – oyun denetleyicisi framework Apple TV uygulamalarla GCSupportedGameControllers anahtarı uygulamanın Info.plist dosyasında içermelidir._

Siri uzaktan desteği eklemek için çözümdür (`GCMicroGamepad`) için uygulamanızın `Info.plist` dosya. Mikro oyun denetleyicileri profili Siri uzaktan hedeflemek için Apple tarafından eklendi. Örneğin, aşağıdaki anahtarları şunlardır:

```xml
<key>GCSupportedGameControllers</key>  
  <array>  
    <dict>  
      <key>ProfileName</key>  
      <string>ExtendedGamepad</string>  
    </dict>  
    <dict>  
      <key>ProfileName</key>  
      <string>MicroGamepad</string>  
    </dict>  
  </array>  
<key>GCSupportsControllerUserInteraction</key>  
<true/>
```

> [!IMPORTANT]
> **Not:** Bluetooth oyun denetleyicileri olan son kullanıcıların yapabileceğiniz isteğe bağlı bir satın alma, uygulamanızın satın almak için kullanıcı zorlayamaz. Oyun denetleyicileri uygulamanız destekliyorsa, oyun tüm Apple TV kullanıcılar tarafından kullanışlı olmasını sağlamak, ayrıca Siri uzaktan desteklemesi gerekir.

Daha fazla bilgi için lütfen bkz bizim [oyun denetleyicileri ile çalışma](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) bölümünü bizim [Siri uzak ve Bluetooth denetleyicileri](~/ios/tvos/platform/remote-bluetooth.md) belgeleri.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Uyumsuz hedef framework:. NetPortable, sürümü v4.5, profil = Profile78 =

Taşınabilir sınıf kitaplığı (PCL) bir Xamarin.tvOS projeye dahil etmek çalışırken, bir ileti oluşturmak üzere alabilmeniz:

_Uyumsuz hedef framework:. NetPortable, sürümü v4.5, profil = Profile78 =_

Bu sorunu çözmek için adlandırılan bir XML dosyasına eklemek ` Xamarin.TVOS.xml` aşağıdaki içeriğe sahip:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

Aşağıdaki yola:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

Profil numarası yolunda PCL profil sayısı eşleşmesi gerektiğini unutmayın.

Bu dosyayı yerinde başarıyla PCL dosya Xamarin.tvOS projeye eklemek mümkün olması gerekir.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
