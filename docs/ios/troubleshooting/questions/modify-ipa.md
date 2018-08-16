---
title: Ben dosyalarını ekleyebilir veya Visual Studio'da derledikten sonra bir IPA dosyasındaki dosyaları kaldırma?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 366308774a7302e54b0d47753256638e89d97b82
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350988"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Ben dosyalarını ekleyebilir veya Visual Studio'da derledikten sonra bir IPA dosyasındaki dosyaları kaldırma?

Evet, mümkündür, ancak genellikle yeniden imzalama gerektirecek `.app` değişiklik yaptıktan sonra paketi.

Not Bu değiştirme `.ipa` dosya normal kullanımda gerekli değildir. Bu makalede, yalnızca bilgilendirici amaçlarla sağlanmıştır.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Örnek: bir dosyadan kaldırarak bir `.ipa` arşiv

Bu örnek için bir Xamarin.iOS projesi adı olduğunu varsayalım `iPhoneApp1` ve `generated session id` olduğu `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Derleme `.ipa` Visual Studio'dan normal olarak dosya.

2.  Mac derleme konağı geçebilir.

3.  Derleme Bul `~/Library/Caches/Xamarin/mtbs/builds` klasör. Bu yolun içine yapıştırabilirsiniz **Bulucu > gidin > klasörüne gidin** Bulucu klasöre gidin. Proje adı ile eşleşen klasörünü arayın. Bu klasörde eşleşen klasörü arayın `generated session id` derleme. Bu, büyük olasılıkla en son değiştirilme zamanına sahip alt klasör olacaktır.

4.  Yeni bir `Terminal.app` penceresi.

5.  Tür `cd ` Terminal.app penceresini ve ardından sürükle ve bırak içine `generated session id` klasörüne `Terminal.app` penceresi:

    ![](modify-ipa-images/session-id-folder.png "Oluşturulan oturum kimliği klasörü Finder'da bulma")

6.  Dizine değiştirmek için dönüş anahtar türü `generated session id` klasör.

7.  Unzip `.ipa` geçici bir dosyaya `old/` aşağıdaki komutu kullanarak klasör. Ayarlama `Ad-Hoc` ve `iPhoneApp1` belirli projeniz için gerektiği şekilde adlandırır.

    > -xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa eski Denden /

8.  Tutun `Terminal.app` penceresini açın.

9.  İstenen dosyaları Sil `.ipa`. Bunları kullanarak Bulucu çöp kutusuna taşınacak veya kullanarak komut satırı Sil `Terminal.app`. İçeriğini görüntülemek için `Payload/iPhone` seçin ve dosyayı Finder'da Control tuşuna tıklama dosya **paket içeriğini göster**.

10.  Adım 3, olduğu gibi aynı genel yaklaşım kullanarak Bul günlük dosyasını altında `~/Library/Logs/Xamarin/MonoTouchVS/` proje adı olan ve `generated session id` adında: ![](modify-ipa-images/build-log.png "Finder'da projesi oluşturma günlüğünü bulun")

11.  Yapı günlüğüne, adımda 10, örneğin çift tıklayarak açın.

12.  İçeren satırı Bul `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Tür `/usr/bin/codesign ` 8. adımdaki Terminal.app penceresine.

14.  Tüm bağımsız değişkenler ile başlayan kopyalayın `-v` satırında 12 adım ve bunları Terminal.app penceresine yapıştırın.

15.  Olmasını son bağımsız değişkenin `.app` paket içinde bulunan `old/Payload/` klasörünü açın ve ardından komutu çalıştırın.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Dönüştürme `old/` terminalde dizin:

```bash
cd old
```

17.  Yeni bir dizine içeriğini zip `.ipa` kullanarak dosya `zip` komutu. Değiştirebileceğiniz `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` çıktısını almak için bağımsız değişken `.ipa` istediğiniz yerde dosya:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Genel hata iletileri

Görürseniz `Invalid Signature. A sealed resource is missing or invalid.`, genellikle anlamına bir şey içinde değiştirildiğini `.app` paket ve `.app` paket doğru şekilde yeniden imzalanmamış daha sonra. Oluşturmak istediğiniz da unutmayın bir `.ipa` dağıtım profiliyle, _gerekir_ orijinal derleme `.ipa` dağıtım profili ile. Aksi takdirde `Entitlements.xcent` yanlış olur.

Aşağıdaki çalıştırırsanız nasıl bu hata ortaya çıkabilecek, somut bir örnek vermek `codesign --verify` komut sonra 9. adım Terminal penceresinde, hatanın tam nedenini birlikte hatayı görürsünüz:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

Ve App Store doğrulama işlemi, benzer bir hata iletisi rapor eder:

> HATA ITMS-90035: "geçersiz imza. Korumalı bir kaynağı eksik veya geçersiz. İkili dosya yolunda [iPhoneApp1.app/iPhoneApp1], geçersiz bir imza içeriyor. Uygulamanızın dağıtım sertifikası, değil geçici bir sertifika veya bir geliştirme sertifikası ile imzalanmış olduğundan emin olun. Xcode kod imzalama ayarlarında hedef düzeyinde (Bu proje düzeyindeki tüm değerleri geçersiz kılmak) doğru olduğundan emin olun. Ayrıca, Xcode, simülatör hedef yayın hedef kullanarak karşıya yüklemekte olduğunuz paket oluşturulmuş emin olun. Kod imzalama ayarlarınızın doğru olduğundan eminseniz, Xcode'da "Temiz tümünü"'i seçin, Finder'da "derleme" dizinini Sil ve yayın hedef yeniden oluşturun. Daha fazla bilgi için lütfen başvurun [ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
