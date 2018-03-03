---
title: "İTunesMetadata.plist dosyası"
description: "Bu makalede iTunes geçici dağıtım sınama veya kurumsal dağıtım için kullanılarak bir iOS uygulaması hakkında bilgi sağlamak için kullanılan iTunesMetadata.plist dosya kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 70676eba-6a99-4a3a-bccc-84359fe9c2c3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 21ae62ee0d43e688e63ca8b7feb6a8aebb227cd5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="the-itunesmetadataplist-file"></a>İTunesMetadata.plist dosyası

_Bu makalede iTunes geçici dağıtım sınama veya kurumsal dağıtım için kullanılarak bir iOS uygulaması hakkında bilgi sağlamak için kullanılan iTunesMetadata.plist dosya kapsar._

Bir iOS uygulaması iTune Bağlan (ya da satış veya iTunes uygulama mağazasından ücretsiz sürüm için) oluşturulduğunda, geliştirici uygulamanın Tarz, alt Tarz, telif hakkı uyarısı, desteklenen iOS cihazları ve gerekli cihaz gibi bilgileri belirtebilirsiniz yetenekleri. Ya da teslim sınayıcılar ya da kuruluş kullanıcıya geçici dağıtım aracılığıyla iOS uygulamaları için bu bilgileri eksik.

Bu, isteğe bağlı bir geçici bir dağıtım için eksik bilgileri girmeniz `iTunesMetadata.plist` dosya oluşturulur ve uygulamaları IPA dosyasında bulunur. Özel olarak biçimlendirilmiş bir XML dosyası bu plist dosyasıdır (Apple'nın bkz [özellikleri listesi Programlama Kılavuzu](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html) daha fazla bilgi için) belirli iOS uygulamayla ilgili bilgileri tanımlayan anahtar/değer çiftleri içerir.

<a name="iTunesMetadata_contents" />

## <a name="the-itunesmetadataplist-contents"></a>İTunesMetadata.plist içeriği

Aşağıdaki tipik örneğidir `iTunesMetadata.plist` iTunes bilgi geçici bir dağıtım için tanımlamak için kullanılan dosya:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>UIRequiredDeviceCapabilities</key>
    <dict>
        <key>armv7</key>
        <true/>
        <key>front-facing-camera</key>
        <true/>
    </dict>
    <key>artistName</key>
    <string>Company, Inc.</string>
    <key>bundleDisplayName</key>
    <string>App Name</string>
    <key>bundleShortVersionString</key>
    <string>1.5.1</string>
    <key>bundleVersion</key>
    <string>1.5.1</string>
    <key>copyright</key>
    <string>© 2015 Company, Inc.</string>
    <key>drmVersionNumber</key>
    <integer>0</integer>
    <key>fileExtension</key>
    <string>.app</string>
    <key>gameCenterEnabled</key>
    <false/>
    <key>gameCenterEverEnabled</key>
    <false/>
    <key>genre</key>
    <string>Games</string>
    <key>genreId</key>
    <integer>6014</integer>
    <key>itemName</key>
    <string>App Name</string>
    <key>kind</key>
    <string>software</string>
    <key>playlistArtistName</key>
    <string>Company, Inc.</string>
    <key>playlistName</key>
    <string>App Name</string>
    <key>releaseDate</key>
    <string>2015-11-18T03:23:10Z</string>
    <key>s</key>
    <integer>143441</integer>
    <key>softwareIconNeedsShine</key>
    <false/>
    <key>softwareSupportedDeviceIds</key>
    <array>
        <integer>9</integer>
    </array>
    <key>softwareVersionBundleId</key>
    <string>com.company.appid</string>
    <key>subgenres</key>
    <array>
        <dict>
            <key>genre</key>
            <string>Puzzle</string>
            <key>genreId</key>
            <integer>7012</integer>
        </dict>
        <dict>
            <key>genre</key>
            <string>Word</string>
            <key>genreId</key>
            <integer>7019</integer>
        </dict>
    </array>
    <key>versionRestrictions</key>
    <integer>16843008</integer>
</dict>
</plist>

```

Tek tek tuşları için değer aşağıda ayrıntılı olarak ele alınacaktır.

### <a name="uirequireddevicecapabilities"></a>UIRequiredDeviceCapabilities

`UIRequiredDeviceCapabilities` Anahtar hangi cihaz bir iOS uygulaması gerektiren belirli iOS cihazında yüklenmeden önce belirli özellikleri bilmeniz iTunes olanak sağlar. Bir sözlük olarak sağlanan (`<dict>...</dict>`) özellikleri (`<key>...</key>`) ve her bir özellik için bir boolean değeri. Bir özellik değeri geçerliyse `true`, bu özelliği mevcut olması gerekir. Eğer öyleyse `false` özelliği cihazda mevcut olmaması gerekir. Örneğin:

```xml
<key>UIRequiredDeviceCapabilities</key>
<dict>
    <key>armv7</key>
    <true/>
    <key>front-facing-camera</key>
    <true/>
</dict>
```
İOS cihazı yönerge belirlemek ve cihazda bu uygulama yüklenmeden önce bir ön dönük kamera ARM7 desteklemelidir belirtir. Apple'nın izin verilen değerlerin tam listesi için lütfen bkz [UIRequiredDeviceCapabilities](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) belgeleri.

### <a name="artistname-and-playlistartistname"></a>artistName ve playlistArtistName

Kullanım `artistName` ve `playlistArtistName` iOS uygulamayı oluşturan şirket adını tanımlamak için anahtarları iTunes görüntülenir. Örnek:

```xml
<key>artistName</key>
<string>Company, Inc.</string>
...
<key>playlistArtistName</key>
<string>Company, Inc.</string>
```

### <a name="bundledisplayname-itemname-and-playlistname"></a>bundleDisplayName, ItemName ve playlistName

Kullanım `bundleDisplayName`, `itemName`, ve `playlistName` iTunes içinde görüntülenen iOS uygulamasının adını tanımlamak için anahtarları. Örnek:

```xml
<key>bundleDisplayName</key>
<string>App Name</string>
...
<key>itemName</key>
<string>App Name</string>
...
<key>playlistName</key>
<string>App Name</string>
```

### <a name="bundleshortversionstring-and-bundleversion"></a>bundleShortVersionString ve bundleVersion

Kullanım `bundleShortVersionString` ve `bundleVersion` anahtarları iTunes görüntülenir iOS uygulamanın sürüm numarasını tanımlayın. Örnek:

```xml
<key>bundleShortVersionString</key>
<string>1.5.1</string>
<key>bundleVersion</key>
<string>1.5.1</string>
```

### <a name="softwareversionbundleid"></a>softwareVersionBundleId

Kullanım `softwareVersionBundleId` anahtar iOS uygulama paketi kimliği belirtin. Örnek:

```xml
<key>softwareVersionBundleId</key>
<string>com.company.appid</string>
```

### <a name="copyright"></a>telif hakkı

Kullanım `copyright` iTunes görüntülenen telif hakkı bildirimi tanımlamak için anahtar. Örnek:

```xml
<key>copyright</key>
<string>© 2015 Company, Inc.</string>
```

### <a name="releasedate"></a>releaseDate

Kullanım `releaseDate` anahtar iTunes görüntülenen iOS uygulaması için bir yayın tarihi sağlayın. Örnek:

```xml
<key>releaseDate</key>
<string>2015-11-18T03:23:10Z</string>
```

### <a name="softwareiconneedsshine"></a>softwareIconNeedsShine

Kullanım `softwareIconNeedsShine` iOS uygulamanın simgesini gerektirip gerektirmediğini iTunes bildirmek için anahtar bir _bekliyoruz Vurgu_ iOS 6 (ve önceki). Örnek:

```xml
<key>softwareIconNeedsShine</key>
<false/>
```

### <a name="gamecenterenabled-and-gamecentereverenabled"></a>gameCenterEnabled ve gameCenterEverEnabled

Kullanım `gameCenterEnabled` ve `gameCenterEverEnabled` iTunes olup bu iOS uygulama anahtarları Apple'nın Game Center destekler. Örnek:

```xml
<key>gameCenterEnabled</key>
<false/>
<key>gameCenterEverEnabled</key>
<false/>
```

### <a name="genre-genreid-and-subgenres"></a>genre, genreId ve subgenres

Kullanım `genre` ve `genreId` anahtarları iTunes iOS uygulamasına ait hangi Tarz söyleyin. Örnek:

```xml
<key>genre</key>
<string>Games</string>
<key>genreId</key>
<integer>6014</integer>
```

İsteğe bağlı olarak `subgenres` anahtar, daha fazla iOS uygulaması için en fazla iki alt türler tanımlamak için kullanılabilir. Örnek:

```xml
<key>subgenres</key>
<array>
    <dict>
        <key>genre</key>
        <string>Puzzle</string>
        <key>genreId</key>
        <integer>7012</integer>
    </dict>
    <dict>
        <key>genre</key>
        <string>Word</string>
        <key>genreId</key>
        <integer>7019</integer>
    </dict>
</array>
```

İOS uygulamaları için Apple şu anda aşağıdaki türler ve Tarz kimlikleri tanımlar:

[!include[](~/ios/includes/table-appstore.html)]

Daha fazla bilgi için lütfen Apple'nın bkz [Tarz kimlikleri ek](http://www.apple.com/itunes/affiliates/resources/documentation/genre-mapping.html) belgeleri.

### <a name="softwaresupporteddeviceids"></a>softwareSupportedDeviceIds

Kullanım `softwareSupportedDeviceIds` iTunes bu iOS uygulamasının desteklediği hangi iOS cihazları bildirmek için anahtar. Örnek:

```xml
<key>softwareSupportedDeviceIds</key>
<array>
    <integer>9</integer>
</array>
```

Burada aşağıdaki değerler kullanılabilir:

- 1 – Klasik iPhone
- 2 – iPod Touch
- 4 – iPad
- 9 – Modern iPhone

### <a name="standard-keys"></a>Standart anahtarları

Aşağıdaki anahtarları tümünde dahil edilen `iTunesMetadata.plist` dosyaları iOS uygulamaları için ve her zaman aynı değerlere sahiptir:

```xml
<key>drmVersionNumber</key>
<integer>0</integer>
<key>fileExtension</key>
<string>.app</string>
...
<key>kind</key>
<string>software</string>
...
<key>s</key>
<integer>143441</integer>
...
<key>versionRestrictions</key>
<integer>16843008</integer>
```

<a name="iTunesMetadata_creating" />

## <a name="creating-an-itunesmetadataplist-file"></a>Bir iTunesMetadata.plist dosyası oluşturma

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 İle çalışırken bir `iTunesMetadata.plist` dosya Mac için Visual Studio'da, iki seçeneğiniz vardır:

- Oluşturun ve Visual Studio için Mac'ın visual plist düzenleyicisi kullanarak dosyasını güncelleştirin.
- Oluşturun ve düz metin düzenleyicide dosyasını güncelleştirin.

 Her iki seçenek aşağıda ayrıntılı olarak ele alınacaktır.

### <a name="using-the-visual-plist-editor"></a>Visual Plist Düzenleyicisi'ni kullanarak

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, Xamarin.iOS projesi dosyasını sağ tıklatın ve seçin **Ekle** > **yeni dosya...**
2. Yeni dosya iletişim kutusundan seçin **iOS** > **özellik listesi**:

    ![](itunesmetadata-images/image01.png "İOS özellik listesi seçin")
3. Girin `iTunesMetadata` için **adı** tıklatıp **yeni** düğmesi.
4. Çift `iTunesMetadata.plist` dosyasını **Çözüm Gezgini** düzenlemek üzere açmak için:

    ![](itunesmetadata-images/image02.png "İTunesMetadata.plist Düzenleyicisi")
5. Yeşil tıklatın  **+**  yeni bir giriş oluşturun ve girmek için `UIRequiredDeviceCapabilities` anahtar adı:

    ![](itunesmetadata-images/image03.png "Yeni bir giriş oluşturabilir ve UIRequiredDeviceCapabilities anahtar adı girin")
6. Tıklayın **dize** değer türü ve select **sözlük** açılan listeden:

    ![](itunesmetadata-images/image04.png "Sözlük açılan listeden seçin")
7. Özelliğin adı sözlüğün girişleri ortaya solundaki turndown tıklatın:

    ![](itunesmetadata-images/image05.png "Dictionary girişlerinin Göster")
8. Tıklayın **yeni giriş Ekle** metin yeşil ardından  **+**  sözlüğe bir giriş eklemek için:

    ![](itunesmetadata-images/image06.png "Sözlüğe bir giriş ekleyin")
9. Girin `armv7` anahtar adını, türünü **Boolean** ve girin **Evet** değeri olarak:

    ![](itunesmetadata-images/image07.png "Armv7 anahtar adı, bir Boolean türü seçin ve değeri olarak Evet girin")
10. Doldurduğunuz kadar yukarıdaki adımları yineleyin `iTunesMetadata.plist` tüm gerekli anahtar/değer çiftleri dosyasıyla (bkz [iTunesMetadata.plist içeriği](#iTunesMetadata_contents) daha fazla ayrıntı için bölüm yukarıda).

11. Plist dosyasındaki değişiklikleri kaydedin.

### <a name="using-a-plain-text-editor"></a>Bir düz metin düzenleyicisi kullanarak

Aşağıdakileri yapın:

1. Bir düz metin Düzenleyicisi'nde yeni bir metin dosyası oluşturun ve adlandırın `iTunesMetadata.plist`.
2. Örnek içeriği Kopyala [iTunesMetadata.plist içeriği](#iTunesMetadata_contents) yukarıdaki bölümde.
3. İçeriği dosyaya yapıştırın ve bunları gerektiği gibi düzenleyin.
4. Dosyayı kaydedin ve Mac için Visual Studio'ya geri dönün
5. İçinde **Çözüm Gezgini**, Xamarin.iOS projesi dosyasını sağ tıklatın ve seçin **Ekle** > **var olan dosyaları...** .
6. Açık dosya iletişim kutusunda seçin `iTunesMetadata.plist` yukarıda oluşturduğunuz ve tıklatın dosyası **Tamam** düğmesi.
7. Bırakın **yapı eylemi** ayarlamak bu dosyanın **hiçbiri**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio için Xamarin eklentisi için görsel bir düzenleyici yalnızca destekler `Info.plist` ve `Entitlement.plist` dosyaları oluşturmanız gerekecek şekilde, `iTunesMetadata.plist` standart bir metin Düzenleyicisi'nde dosya ve el ile Xamarin.iOS projenize ekleyin.

Aşağıdakileri yapın:

1. Bir düz metin Düzenleyicisi'nde yeni bir metin dosyası oluşturun ve adlandırın `iTunesMetadata.plist`.
2. Örnek içeriği Kopyala [iTunesMetadata.plist içeriği](#iTunesMetadata_contents) yukarıdaki bölümde.
3. İçeriği dosyaya yapıştırın ve bunları gerektiği gibi düzenleyin.
4. Dosyayı kaydedin ve Visual Studio'ya geri dönün.
5. İçinde **Çözüm Gezgini**, Xamarin.iOS projesi dosyasını sağ tıklatın ve seçin **Ekle** > **var olan dosyaları...** .
6. Açık dosya iletişim kutusunda seçin `iTunesMetadata.plist` , yukarıda oluşturduğunuz dosya ve tıklayın **açık** düğmesi.
7. Bırakın **yapı eylemi** ayarlamak bu dosyanın **hiçbiri**.

-----

Daha sonra bunu seçin `iTunesMetadata.plist` IDE içinde IPA oluşturmak hazırlanırken dosya.

## <a name="summary"></a>Özet

Bu makalede ele alınan `iTunesMetadata.plist` iTunes bir geçici hakkında bildirmek için kullanılan dosya teslim iOS uygulaması. Standart anahtar plist dosyası ve oluşturmak ve Mac için Visual Studio ve Visual Studio dosyayı korumak nasıl ele

## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama mağazası dağıtım](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Uygulama iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Uygulama mağazası yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Şirket içi dağıtım](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Geçici dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [IPA desteği](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
