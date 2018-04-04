---
title: Yetkilendirmeler ile çalışma
description: Yetkilendirmeler özel uygulama özellikleri ve onları kullanmak üzere doğru şekilde yapılandırılmış uygulamalar için güvenlik izinler ' dir.
ms.prod: xamarin
ms.assetid: 8A3961A2-02AB-4228-A41D-06CB4108D9D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 6ced541ca9df6fcae1643dc14c2e19807e972822
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-entitlements"></a>Yetkilendirmeler ile çalışma

_Yetkilendirmeler özel uygulama özellikleri ve onları kullanmak üzere doğru şekilde yapılandırılmış uygulamalar için güvenlik izinler ' dir._

İOS uygulamaları çalıştırmak bir _korumalı alan_, uygulama ve bazı sistem kaynakları veya kullanıcı verileri arasında erişimi sınırlayan kurallar kümesi sağlar. _Yetkilendirmeler_ sistem uygulamanızı ek yetenekler vermek için korumalı alan genişletin istemek için kullanılır.

Uygulamanızı yeteneklerini artırmak için uygulamanızın Entitlements.plist dosyasında bir yetkilendirme sağlanmalıdır. Yalnızca belirli özellikleri genişletilebilir ve bunlar içinde listelenen [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu ve açıklanan [aşağıda](#keyreference). Yetkilendirmeler sisteme bir anahtar/değer çifti olarak geçirilir ve genellikle tek yetenek gereklidir. Özel anahtarları ve değerleri açıklanan [yetkilendirme anahtar başvurusu](#keyreference) bu kılavuzda daha sonra bölüm.
Mac için Visual Studio ve Visual Studio bir Xamarin.iOS uygulaması Entitlements.plist Düzenleyicisi aracılığıyla yetkilendirmeler eklemek için açık bir arabirim sağlar.
Bu kılavuz, Entitlements.plist Düzenleyicisi'ni ve nasıl kullanılacağını tanıtır. Ayrıca, her özellik için bir iOS projesine eklenen tüm yetkilendirmeler bir başvuru sağlar.

## <a name="entitlements-and-provisioning"></a>Yetkilendirmeler ve sağlama


Entitlements.plist dosya yetkilendirmeler belirtmek için kullanılır ve uygulama paketi imzalamak için kullanılır.

Ancak, bazı ek sağlama uygulama kodu doğru şekilde imzalanmış olduğundan emin olmak için gereklidir. Kullanılan sağlama profili etkin gerekli özelliğine sahip bir uygulama kimliği içermelidir. Bunun nasıl yapılacağı hakkında daha fazla bilgi için başvurmak [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu.

> [!IMPORTANT]
> Entitlements.plist dosya özelliklerini kullanarak bir uygulama için doğru özelliklerini doldurun yardımcı olur, ancak bir Apple Geliştirici hesabını bağlı değil olarak bir sağlama profili oluşturulamıyor. Dağıtma ve uygulama dağıtmak için Geliştirici Portalı'nı kullanarak bir sağlama profili oluştur gerekecektir.

## <a name="set-entitlements-in-a-xamarinios-project"></a>Bir Xamarin.iOS projesi yetkilendirmeleri ayarlama

Seçme ve uygulama kimliği tanımlarken, gerekli uygulama hizmetlerini yapılandırma yanı sıra yetkilendirmeleri de Xamarin.iOS projesinde düzenleyerek yapılandırılmalıdır **Info.plist** ve  **Entitlements.plist** dosyaları.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da yetkilendirmeleri yapılandırmak için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift **Info.plist** dosyayı düzenlemek için açın.
2. İçinde **iOS uygulaması hedefi** bölümünde, uygulama için bir ad girin ve girin **paket tanımlayıcısı** uygulama kimliği tanımlandığında oluşturuldu:

  ![](entitlements-images/servicexs01.png "Bir paket tanımlayıcısı girin")

3. Değişiklikleri kaydetmek **Info.plist** dosya.
4. İçinde **Çözüm Gezgini**, çift **Entitlements.plist** dosyayı düzenlemek için açın:

    ![](entitlements-images/servicexs02.png "Yetkilendirmeler düzenleme")

5. Seçin ve Xamarin.iOS uygulaması için uygulama kimliği oluştururken tanımlandığına Kurulum eşleşmesi gereken yetkilendirmeler yapılandırın.
6. Değişiklikleri kaydetmek **Entitlements.plist** dosya.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da yetkilendirmeleri yapılandırmak için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, sağ **Info.plist**seçin **birlikte Aç...** ve **özellik listesi düzenleyici** dosyayı düzenlemek için açın.
2. İçinde **iOS uygulaması hedefi** bölümünde, uygulama için bir ad girin ve girin **paket tanımlayıcısı** uygulama kimliği tanımlandığında oluşturuldu:

  ![](entitlements-images/servicevs01.png "Paket tanımlayıcısı ayarlama")

3. Değişiklikleri kaydetmek **Info.plist** dosya.
4. İçinde **Çözüm Gezgini**, sağ tıklayın **Entitlements.plist** dosyası **birlikte Aç...** ve **özellik listesi düzenleyici** düzenlemek üzere açmak için:

    ![](entitlements-images/servicevs02.png "Yetkilendirmeler düzenleme")

    Alternatif olarak, çift **Entitlements.plist** dosya içinde ayrıntılı olarak yetkilendirme özelliği ve anahtar değeri ayarlamanıza olanak tanıyan XML kaynak Düzenleyicisi açılır [yetkilendirmeler başvuru](#keyreference)bölümüne bakın.

5. Seçin ve Xamarin.iOS uygulaması için uygulama kimliği oluştururken tanımlandığına Kurulum eşleşmesi gereken yetkilendirmeler yapılandırın.
6. Değişiklikleri kaydetmek **Entitlements.plist** dosya.


-----

<a name="add-new" />

## <a name="adding-a-new-entitlementsplist-file"></a>Yeni bir Entitlements.plist dosyası ekleme

Yetkilendirmeler Entitlements.plist dosyası aracılığıyla uygulama eklenir. Bu dosya Xamarin.iOS projelerinde varsayılan olarak dahil ancak eski projelerden eksik olabilir.

Xamarin.iOS Yapılacaklar aşağıdaki Entitlements.plist dosya eklemek için:

1.  Proje dosyasını sağ tıklatın ve Gözat **Ekle > yeni dosya...** :

    ![Dosya bağlam menüsü ekleme](entitlements-images/image1.png)
2.  Yeni dosya iletişim kutusunda seçin **iOS > özellik listesi** ve yetkilendirmeler adlandırın:

    ![Yeni dosya iletişim kutusu](entitlements-images/image2.png)

<a name="keyreference" />

## <a name="entitlement-key-reference"></a>Yetkilendirme anahtar başvurusu

Yetkilendirme anahtarları Entitlements.plist Düzenleyicisi'ni kaynağı paneli eklenebilir. Gerekli anahtarlarla, kullanırken normalde aşağıda Entitlements.plist Düzenleyicisi ancak olan başvuru için listelenen eklenir.

### <a name="wallet"></a>Wallet

*   **Açıklama**: resmi olarak Passbook da bilinen Cüzdan depolar ve geçişleri yöneten bir uygulamadır. Bu geçiş, kredi kartı, depolama kartları, binme geçişleri veya biletleri olabilir.

    - **Geçiş türü tanımlayıcısı**
        * **Anahtarları**: com.apple.developer.pass tür tanımlayıcıları
        * **Dize**: `$(TeamIdentifierPrefix)*`

* **Notlar**:
    - Bu, tüm türleri geçirmek izin vermek, uygulamanızın olanak tanır. Uygulamanızı kısıtlamak ve yalnızca bir alt kümesini team geçişi türleri izin vermek için dize değeri ayarlayın: `$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

    Pass.$(CFBundleIdentifier) oluşturuldu geçirmek kimliği olduğu [yukarıda](~/ios/platform/passkit.md)

<a name="icloud" />

### <a name="icloud"></a>iCloud

*   **Açıklama**: iCloud içeriklerini depolamak ve cihazlar arasında paylaşmak için iOS kullanıcılarla kullanışlı ve basit bir yol sağlar. Geliştiriciler kendi kullanıcılar için depolama sağlamak için iCloud kullanabilir dört yolu vardır: anahtar-değer depolama, UIDocument depolama, CoreData ve CloudKit doğrudan tek dosyalar ve dizinler için depolama sağlamak için kullanma. Bunlar hakkında daha fazla bilgi için giriş iCloud Kılavuzu'na bakın.

    - **İcloud'a belge & CloudKit**
        - **Anahtarları**: com.apple.developer.ubiquity kapsayıcı tanımlayıcıları
        - **Dize**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`
    - **iCloud KeyValue Storage**
        - **Anahtar**: com.apple.developer.ubiquity kvstore tanımlayıcısı
        - **Dize**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`

* **Notlar**:
    - `$(TeamIdentifierPrefix)` Dize developer.apple.com ve ziyaret açarak bulunması **üye merkezi > hesabınız > Geliştirici hesabı Özet** Takım Kimliği (veya tek geliştiriciler için tek tek kimliği) almak için. 10 karakter dizesi olacaktır (örneğin A93A5CM278).
    - `$(CFBundleIdentifier)` Dize başlıyorsa `iCloud` ve iCloud kapsayıcı içindeki adımları göredir crated ayarlanır [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) Kılavuzu.
    - $`(TeamIdentifierPrefix)` Ve `$(CFBundleIdentifier)` yer tutucuları kullanılabilir ve doğru değerlerin için derleme zamanında değiştirilecektir.

### <a name="app-groups"></a>Uygulama grupları

- **Açıklama**: farklı uygulamaları (veya bir uygulama ve uzantılarını) bir paylaşılan dosya depolama konumu erişmek bir uygulama grubu sağlar.

    - **Anahtar**: com.apple.security.application grupları
    - **String**: group.$(CFBundleIdentifier)

<a name="apple-pay" />

### <a name="apple-pay"></a>Apple Pay

- **Açıklama**: Apple pay kullanıcıların iOS cihazlarını aracılığıyla fiziksel mal ödeme sağlar.
    - **Anahtar**: com.apple.developer.in uygulama ödemeler
    - **Dize**: merchant.your.mechantid

### <a name="push-notifications"></a>Anında iletme bildirimleri

- **Anahtar**: AP'ler ortamı
- **Dize**: `production` veya `development`

### <a name="siri"></a>Siri

- **Açıklama**: SiriKit, uygulama uzantıları, yeni hedefleri ve hedefleri UI kullanarak iOS cihazında Siri tarafından erişilebilen hizmetler sağlamak için bir iOS uygulaması ve haritalar uygulama sağlar çerçeveleri. Daha fazla bilgi için giriş SiriKit Kılavuzu'na bakın.
    - **Key**: com.apple.developer.siri

### <a name="personal-vpn"></a>Kişisel VPN

- **Key**: com.apple.developer.networking.vpn.api
- **String**: allow-vpn

### <a name="keychain-sharing"></a>Anahtarlık paylaşımı

- **Açıklama**: Anahtarlık paylaşımı, uygulama geliştiriciler aynı ekibiniz tarafından geliştirilen diğer uygulamalarla depolanan aygıt Anahtarlıkta parolaları paylaşabilmesini sağlar. Anahtarlık erişim grubu tanımlayıcısı dizesi içinde geçirerek erişimi kısıtlanabilir.
    - **Anahtar**: Anahtarlık erişim grupları
    - **String**: $(AppIdentifierPrefix) $(CFBundleIdentifier)

### <a name="inter-app-audio"></a>Uygulamalar Arası Ses

- **Açıklama**: uygulamalar arasında akış ses geliştiricilerine arası uygulama ses sağlar.
    - **Anahtar**: arası-app-ses
    - **Boolean**: Evet

### <a name="associated-domains"></a>İlişkili etki alanları

- **Açıklama**: Evrensel bağlantıları bu yetkilendirme ile iletilmesi gereken şekilde, işlenmesi gereken etki alanları ilişkili. Evrensel bağlantılar, uygulama ve Web sitesi arasında bağlama derin izin vermek için uygulanabilir. Uygulama destekler ve her bir girdi ile başlaması gereken her etki alanı için bir girdi sağlanması `applinks:`
    - **Key**: com.apple.developer.associated-domains
    - **String**: webcredentials:example.com

### <a name="data-protection"></a>Veri koruma

- **Açıklama**: veri korumayı etkinleştirmek yerleşik şifreleme donanımını şifrelenmiş biçimde uygulamanızda kullanılan hassas verileri depolamak için kullanır. Varsayılan olarak, koruma düzeyini (cihaz kilitli değil sonra dosyaları yalnızca erişilebilir olduğunda) koruma tamamlamak için ayarlanır.
    - **Key**: com.apple.developer.default-data-protection
    - **String**: NSFileProtectionComplete

### <a name="homekit"></a>HomeKit

- **Açıklama**: HomeKit framework yukarı, yapılandırma ayarı için bir platform sağlar ve desteklenen ev Otomasyon aygıtlardan – tüm iOS cihazı yönetme. HomeKit kullanma hakkında daha fazla bilgi için giriş HomeKit Kılavuzu'na bakın.
    - **Key**: com.apple.developer.homekit
    - **Boolean**: Evet

### <a name="healthkit"></a>HealthKit

- **Açıklama**: HealthKit, sistem durumu ile ilgili bilgiler için merkezi, Eşgüdümlü ve güvenli veri deposu sağlayan iOS 8'de sunulan bir çerçevedir. HealthKit kullanma hakkında daha fazla bilgi için giriş HealthKit Kılavuzu'na bakın.
    - **Key**: com.apple.developer.healthkit
    - **Boolean**: Evet

### <a name="wireless-accessory-configuration"></a>Kablosuz Aksesuar Yapılandırması

- **Açıklama**: kablosuz aksesuar Yapılandırması'nı kullanarak uygulamanızın MFi Wi-Fi aksesuarlarını yapılandırmasına izin verir
    - **Anahtar**: com.apple.external accessory.wireless yapılandırma
    - **Boolean**: Evet

## <a name="summary"></a>Özet

Bu kılavuz, yetkilendirmeler ve Mac için Visual Studio ve Visual Studio kullanma sunulur. Her yetenek için bir başvuru anahtar/değer çiftleri de sağlanır.