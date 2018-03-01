---
title: "I dosyalarını ekleyebilir veya Visual Studio'da oluşturduktan sonra bir IPA dosyasından dosyaları kaldırmak?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: be792c95de7aa66d64278e47b2ca6b354e611273
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>I dosyalarını ekleyebilir veya Visual Studio'da oluşturduktan sonra bir IPA dosyasından dosyaları kaldırmak?

Evet, mümkündür, ancak genellikle yeniden imzalamak gerekir `.app` değişikliği yaptıktan sonra paketi.

Bu değiştirme Not `.ipa` dosya normal kullanımda gerekli değildir. Bu makale yalnızca bilgilendirme amacıyla sağlanmıştır.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Örnek: bir dosyadan kaldırma bir `.ipa` arşiv

Bu örnek için Xamarin.iOS projesinin adı olduğunu varsayalım `iPhoneApp1` ve `generated session id` olduğu `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Yapı `.ipa` Visual Studio'dan normal olarak dosya.

2.  Mac yapı konağı geçebilir.

3.  Derleme Bul `~/Library/Caches/Xamarin/mtbs/builds` klasör. Bu yolun içine yapıştırabilirsiniz **Bulucu > Git > klasörüne gidin** Bulucu klasöre gidin. Proje adı ile eşleşen klasörü arayın. Bu klasör içinde eşleşen klasörü arayın `generated session id` yapı. Bu, büyük olasılıkla en son değiştirilme saati sahip alt klasör olacaktır.

4.  Yeni bir `Terminal.app` penceresi.

5.  Tür `cd ` Terminal.app penceresinde ve ardından sürükleyin & bırakma içine `generated session id` klasörüne `Terminal.app` penceresi:

    ![](modify-ipa-images/session-id-folder.png "Oluşturulan oturum kimliği klasörü Finder bulma")

6.  Dizine değiştirmek için return tuşuna yazın `generated session id` klasör.

7.  Unzip `.ipa` geçici bir dosyaya `old/` klasörü aşağıdaki komutu kullanarak. Ayarlama `Ad-Hoc` ve `iPhoneApp1` adları belirli projeniz için gerektiği gibi.

    > -xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa eski Denden /

8.  Tutmak `Terminal.app` penceresini açın.

9.  İstenen dosyalarını silin `.ipa`. Bulucu kullanarak çöp taşımak veya komut satırını kullanarak silme `Terminal.app`. İçeriğini görüntülemek için `Payload/iPhone` Bulucu, Denetim tıklatma dosyası ve seçin **paket içeriğini göster**.

10.  Adım 3, olduğu gibi aynı genel yaklaşım kullanarak günlük dosyasını bulun `~/Library/Logs/Xamarin/MonoTouchVS/` her iki proje adı olan ve `generated session id` adında: ![ ] (modify-ipa-images/build-log.png "Finder proje derleme günlüğünde bulun")

11.  Derleme günlüğünde, 10, adımından örneğin çift tıklatarak açın.

12.  İçeren satırı bulun `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Tür `/usr/bin/codesign ` adım 8'deki Terminal.app penceresine.

14.  Tüm ile başlayan bağımsız değişkenler kopyalayın `-v` satırında 12 adım ve Terminal.app penceresine yapıştırın.

15.  Son bağımsız değişken olarak değiştirmek `.app` paket içinde bulunan `old/Payload/` klasörü ve komutu çalıştırın.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Dönüştürme `old/` Terminal dizin:

```bash
cd old
```

17.  Yeni bir dizine içeriğini zip `.ipa` kullanarak dosya `zip` komutu. Değiştirebileceğiniz `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` çıktısını almak için bağımsız değişken `.ipa` istediğiniz yere dosya:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Genel hata iletileri

Görürseniz `Invalid Signature. A sealed resource is missing or invalid.`, genellikle anlamına bir şey içinde değiştirildi `.app` paket ve `.app` paket doğru yeniden imzalanmamış daha sonra. Oluşturmak isterseniz, ayrıca bir `.ipa` bir dağıtım profiliyle, _gerekir_ özgün yapı `.ipa` dağıtım profiline sahip. Aksi takdirde `Entitlements.xcent` yanlış olur.

Aşağıdaki çalıştırırsanız nasıl bu hata oluşabilir, somut bir örnek vermek için `codesign --verify` komutu 9. adım sonra Terminal penceresinde hatanın tam nedenini yanı sıra hatayı görürsünüz:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

Ve App Store doğrulama işlemi benzer bir hata iletisi rapor eder:

> HATA ITMS-90035: "imzası geçersiz. Korumalı bir kaynağı eksik veya geçersiz olur. İkili [iPhoneApp1.app/iPhoneApp1] yolundaki geçersiz bir imza içeriyor. Uygulama dağıtım sertifikası veya değil, geçici bir sertifika geliştirme sertifikası ile imzalanmış emin olun. Xcode kod imzalama ayarlarında (Proje düzeyindeki tüm değerleri geçersiz kılmak) hedef düzeyinde doğru olduğundan emin olun. Ayrıca, bir yayın hedef Xcode'da Simulator hedef kullanılarak karşıya yüklemekte olduğunuz paket oluşturuldu emin olun. Kod imzalama ayarlarınızın doğru olduğundan eminseniz, Xcode'da "Temiz tümünü"'i seçin, Bulucu "yapı" dizininde silin ve yayın hedef yeniden oluşturun. Daha fazla bilgi için lütfen bakın [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
