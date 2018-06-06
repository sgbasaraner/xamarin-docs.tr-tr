---
title: Xamarin.iOS PassKit
description: Cüzdan depolayan ve barkodları ve müşteri hareketleri telefonlarını 'gerçek dünya' ile bağlamak için diğer bilgileri görüntüleyen bir sistem iOS uygulamasıdır.
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 0a4fd39e312cf96ac59eae97b1212f001c4ef799
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788354"
---
# <a name="passkit-in-xamarinios"></a>Xamarin.iOS PassKit

_Cüzdan depolayan ve barkodları ve müşteri hareketleri telefonlarını 'gerçek dünya' ile bağlamak için diğer bilgileri görüntüleyen bir sistem iOS uygulamasıdır._

Cüzdan İphone'lar için bir uygulamadır ve iPod iOS 6 dokunur. Depolar ve barkodları ve müşteri hareketleri telefonlarını 'gerçek dünya' ile bağlamak için diğer bilgileri görüntüler. Geçişleri satıcı tarafından oluşturulan ve müşteriye içinden veya e-posta, URL'ler aracılığıyla gönderilen bir satıcı'nın kendi iOS uygulaması. Cüzdan depolar ve bir Phone tüm geçişleri düzenler ve tarih/saat veya cihazın konumunu bağlı olarak kilit ekranında geçişi anımsatıcıları görüntüler.

Bu belge Xamarin.iOS ile geçirmek Seti API kullanarak Cüzdan tanıtır ve nasıl sunucunuzda geçişleri uygulanacağını anlatır.

 [![](passkit-images/image1.png "Cüzdan depolar ve bir Phone tüm geçişleri düzenler")](passkit-images/image1.png#lightbox)


## <a name="requirements"></a>Gereksinimler

Bu belgede ele alınan deposu Seti özellikleri, iOS 6 ve Xamarin.iOS 6.0 birlikte Xcode 4.5 gerektirir.


## <a name="introduction"></a>Giriş

Kit geçirmek çözebilecek anahtar barkodları yönetimini ve dağıtımı sorunudur. Nasıl barkodları şu anda kullanılmakta olan bazı gerçek örnekler şunlardır:

-   **Film biletleri çevrimiçi satın alma** – müşteriler genellikle biletlerini temsil eden bir barkod postayla. Bu barkodu yazdırılabilir ve giriş için taranacak sinema gerçekleştirilecek.
-   **Bağlılık kartları** – müşteriler kendi Cüzdan veya çantanızda farklı deposu özgü kartlarının sayısını taşımak, görüntüleme ve ne zaman tarama için mal satın.
-   **Kuponlar** – kuponlar yazdırılabilir web sayfaları, letterboxes aracılığıyla ve gazete ve dergi barkodları olarak olarak e-posta aracılığıyla dağıtılır. Müşteriler bunları mal, hizmetleri ve indirim almak için taramak için bir depolama alanına getirin.
-   **Geçişleri binan** – film bilet satın almak için benzer.


Geçişi Seti bu senaryoların her biri için bir alternatif sunar:

-   **Film biletleri** – satınalma sonra müşterinin bir olay anahtarı geçişi (aracılığıyla, e-posta veya Web sitesi bağlantısı) ekler. Film yaklaşımlar süre olarak geçişi bir anımsatıcı olarak kilit ekranı otomatik olarak görünür ve sinema vardığında üzerinde geçişi kolayca alınır ve Tarama için Cüzdan içinde görüntülenir.
-   **Bağlılık kartları** – yerine (veya ek olarak) fiziksel bir kart sağlayarak, depoları (e-posta yoluyla veya bir Web sitesi oturum açma sonra) depolama kartı geçişi verebilir. Müşteri depolama konumu olduğunda coğrafi konum hizmetlerini kullanarak geçişi otomatik olarak Kilitle ekranda görünebilir ve depolama geçişi anında iletme bildirimleri aracılığıyla hesabında bakiyesini güncelleştirme gibi ek özellikler sağlayabilirsiniz.
-   **Kuponlar** – kupon geçişleri kolayca izlemeyle yardımcı olması için benzersiz özelliklere sahip oluşturulan ve e-posta veya Web sitesi bağlantılar yoluyla dağıtılmış. Kullanıcı belirli bir konuma yakın ve/veya belirli bir tarihte (örneğin, ne zaman sona erme tarihi yaklaştığını) olduğunda indirilen kuponlar üzerinde kilit ekranı otomatik olarak görünebilir. Kuponlar kullanıcının telefonda depolandığından, her zaman kullanışlı olan ve olmayan yanlış yerleştirilmesi. Kuponlar müşterilerin uygulama mağazası bağlantıları müşteriyle katılım artırma geçişi, içine birleştirilebilir çünkü yardımcı uygulamaları karşıdan teşvik.
-   **Geçişleri binan** – bir çevrimiçi iade işleminden sonra e-posta veya bir bağlantı üzerinden kendi ortamınızın geçişi müşteri alacaksınız. Aktarım sağlayıcısı tarafından sağlanan bir yardımcı uygulaması, iade etme işlemi içerir ve ayrıca kendi bilgisayar başına veya yemek seçme gibi ek işlevleri gerçekleştirmek müşteri izin. Aktarım Sağlayıcısı anında iletme bildirimleri aktarım Gecikmeli veya iptal edilirse geçişi güncelleştirmek için kullanabilirsiniz. Binme sürede yaklaşımlar geçişi bir anımsatıcı olarak ve hızlı erişim geçişi sağlamak için kilit ekranında görüntülenir.


Özünde, geçirmek Seti depolamak ve iPhone veya iPod touch Cihazınızda barkodları görüntülemek için basit ve kolay bir yol sağlar. Ek zaman ve konum kilit ekranı tümleştirme ile anında iletme bildirimleri ve yardımcı uygulama, teklifleri foundation çok karmaşık satış için bir anahtar ve Hizmetleri faturalama tümleştirin.


## <a name="pass-kit-ecosystem"></a>Geçişi Seti ekosistemi

Geçişi Seti bir API CocoaTouch içinde değil, yerine daha büyük bir ekosistemi uygulamalar, veri ve güvenli paylaşım kolaylaştırmak Hizmetleri ve barkodları yönetimi ve diğer verileri, bir parçasıdır. Bu üst düzey diyagramı yer alabilir farklı varlıkları oluşturma ve geçişleri kullanarak gösterir:

 [![](passkit-images/image2.png "Kullanıcı hikayesinin oluşturma ve geçişleri kullanarak bu üst düzey diyagramı varlıkları söz konusu'e gösterir.")](passkit-images/image2.png#lightbox)

Her bir parçası ekosistemi açıkça tanımlanmış rol vardır:

-   **Cüzdan** – depolayan ve geçişleri görüntüleyen Apple'nın yerleşik iOS uygulaması (iPhone ve iPod touch için). Bu geçiş (IE Barkod, yerelleştirilmiş veri geçişi ile birlikte görüntülenir) gerçek dünya kullanmak için işlenen tek yerdir.
-   **Uygulamaları Yahoo! Companion** – bunlar sorunu, bir depolama kartına bilgisayar başına binme geçişi veya diğer iş özgü işlevi değiştirme değeri ekleme gibi geçişleri işlevselliğini genişletmek için geçişi sağlayıcıları tarafından oluşturulan iOS 6 uygulamaları. Yardımcı uygulamalar kullanışlı olması bir geçiş için gerekli değildir.
-   **Sunucunuz** – burada geçişleri oluşturulabilir ve dağıtım için imzalanmış güvenli bir sunucu. Yardımcı uygulamanızı yeni geçişleri oluşturmak veya mevcut geçişleri güncelleştirmelerini istemek için sunucunuza bağlanabilir. İsteğe bağlı olarak Cüzdan geçişleri güncelleştirmek için çağırırdı web hizmeti API'sine uygulayabilir.
-   **APNS sunucuları** – sunucunuz Cüzdan APNS kullanarak belirli bir aygıt geçişte güncelleştirmeleri bildirmek için özelliğine sahiptir. Bir bildirim sunucunuz değişiklik ayrıntılarını sonra başvururlar Cüzdan iletin. Yardımcı uygulamaları APNS için bu özelliği uygulamanız gerekmez (dinlemek `PKPassLibraryDidChangeNotification` ).
-   **Conduit uygulamaları** –, yok doğrudan yönlendirme (yardımcı uygulamaları gibi) geçirir, ancak hangi geliştirebilir kendi yardımcı programı geçişleri algılamayı ve Cüzdan için eklenmesine izin vermeden uygulamaları. Posta istemcileri, sosyal ağ tarayıcılar ve diğer veri toplama uygulamalar tüm ekler veya geçişleri bağlantılar karşılaşabilirsiniz.


Bazı bileşenler isteğe bağlıdır ve çok daha kolaydır geçirmek Seti uygulamaları mümkün olması için tüm ekosistemi karmaşık arar.

## <a name="what-is-a-pass"></a>Bir geçişi nedir?

Bir seferde bir bilet, kupon veya kartı temsil eden veri koleksiyonudur. Bir tek bir kişi tarafından kullanılmaya (ve bu nedenle uçuş numarası ve bilgisayar başına ayırma gibi ayrıntıları içerir) veya herhangi bir sayı (örneğin, bir indirim kupon) kullanıcı tarafından paylaşılan bir birden çok kullanımı belirteci olabilir. Ayrıntılı bir açıklama Apple içinde kullanılabilir [hakkında dosyaları geçirmek](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) belge.


### <a name="types"></a>Türler

Şu anda beş Cüzdan uygulamada geçişi düzeni ve üst kenarından ayırt edilebilir türleri desteklenir:

-  **Olay bileti** – küçük Yarım Daire kesme.
-   **Ortamınızın geçişi** – düğüm yan, aktarım özel simge (ör belirtilebilir. veri yolu, eğitme, uçak).
-   **Depolama kartı** – yuvarlak bir kredi veya ATM kartı gibi üst.
-  **Kupon** – delikli olması üstünde.
-  **Genel** – depolama kartı, yuvarlatılmış üst ile aynı.


Beş geçişi türleri bu ekran görüntüsünde gösterilen (sırayla: kupon, genel, depolama kartı, binme geçişi ve olay bileti):

 [![](passkit-images/image3.png "Bu ekran görüntüsünde gösterilen beş geçişi türleri")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>Dosya yapısı

Gerçekte ZIP arşivini ile geçişi dosyasıdır bir **.pkpass** (gerekli) bazı belirli JSON dosyaları, görüntü çeşitli içeren uzantısı dosyaları (isteğe bağlı) yanı sıra yerelleştirilmiş dizeleri (Ayrıca isteğe bağlı).

-   **Pass.JSON** – gerekli. Geçişi için tüm bilgileri içerir.
-   **manifest.JSON** – gerekli. Her dosya imza dosyası dışında geçişindeki ve bu dosyayı (manifest.json) SHA1 karma değerlerini içerir.
-   **İmza** – gerekli. İmzalama tarafından oluşturulan `manifest.json` iOS sağlama portalı oluşturulan sertifika dosyasıyla.
-  **Logo.PNG** – isteğe bağlıdır.
-  **Background.PNG** – isteğe bağlıdır.
-  **icon.PNG** – isteğe bağlıdır.
-  **Yerelleştirilebilir dizeler dosyaları** – isteğe bağlıdır.


Geçişi dosyasının dizin yapısını aşağıda gösterilen (ZIP arşivini içeriğini budur):

 [![](passkit-images/image4.png "Geçişi dosyasının dizin yapısını burada gösterilen")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>Pass.JSON

JSON biçimi geçişleri genellikle bir sunucu üzerinde oluşturulduğundan – oluşturma koduna platform belirsiz olduğu anlamına gelir sunucusunda. Üç temel bilgiler her geçişinde şunlardır:

-   **teamIdentifier** – bu uygulama mağazası hesabınızı oluşturduğunuz tüm geçişleri bağlar. Bu değer iOS sağlama portalı görünür olur.
-   **passTypeIdentifier** – sağlama portalı grubuna kayıttaki geçirir birlikte (birden çok tür üretmek değilse). Örneğin, bir kafeterya bir depo kartı geçirmek bağlılık krediler kazanmak müşterilerine izin vermek için türü, aynı zamanda oluşturmak ve indirim kuponlar dağıtmak için bir ayrı kupon geçirmek türü oluşturabilirsiniz. Bu aynı kafeterya bile Canlı müzik olayları basılı tutun ve olay bileti geçişleri için sorun.
-   **serialNumber** – bu içinde benzersiz bir dize `passTypeidentifier` . Değeri için Cüzdan opak, ancak sunucunuzla iletişim kurarken belirli geçişleri izlemek için önemlidir.


Çok sayıda diğer JSON anahtarlar bir örneği aşağıda gösterilen her geçişinde vardır:

``` 
{
   "passTypeIdentifier":"com.xamarin.passkitdoc.banana",  //Type Identifier (iOS Provisioning Portal)
   "formatVersion":1,                                     //Always 1 (for now)
   "organizationName":"Xamarin",                          //The name which appears on push notifications
   "serialNumber":"12345436XYZ",                          //A number for you to identify this pass
   "teamIdentifier":"XXXAAA1234",                         //Your Team ID
   "description":"Xamarin Demo",                          //
   "foregroundColor":"rgb(54,80,255)",                    //color of the data text (note the syntax)
   "backgroundColor":"rgb(209,255,247)",                  //color of the background
   "labelColor":"rgb(255,15,15)",                         //color of label text and icons
   "logoText":"Banana ",                                  //Text that appears next to logo on top
   "barcode":{                                            //Specification of the barcode (optional)
      "format":"PKBarcodeFormatQR",                       //Format can be QR, Text, Aztec, PDF417
      "message":"FREE-BANANA",                            //What to encode in barcode
      "messageEncoding":"iso-8859-1"                      //Encoding of the message
   },
   "relevantDate":"2012-09-15T15:15Z",                    //When to show pass on screen. ISO8601 formatted.
  /* The following fields are specific to which type of pass. The name of this object specifies the type, e.g., boardingPass below implies this is a boarding pass. Other options include storeCard, generic, coupon, and eventTicket */
   "boardingPass":{
/*headerFields, primaryFields, secondaryFields, and auxiliaryFields are arrays of field object. Each field has a key, label, and value*/
      "headerFields":[          //Header fields appear next to logoText
         {
            "key":"h1-label",   //Must be unique. Used by iOS apps to get the data.
            "label":"H1-label", //Label of the field
            "value":"H1"        //The actual data in the field
         },
         {
            "key":"h2-label",
            "label":"H2-label",
            "value":"H2"
         }
      ],
      "primaryFields":[       //Appearance differs based on pass type
         {
            "key":"p1-label",
            "label":"P1-label",
            "value":"P1"
         }
      ],
      "secondaryFields":[     //Typically appear below primaryFields
         {
            "key":"s1-label",
            "label":"S1-label",
            "value":"S1"
         }
      ],
      "auxiliaryFields":[    //Appear below secondary fields
         {
            "key":"a1-label",
            "label":"A1-label",
            "value":"A1"
         }
      ],
      "transitType":"PKTransitTypeAir"  //Only present in boradingPass type. Value can
                                        //Air, Bus, Boat, or Train. Impacts the picture
                                        //that shows in the middle of the pass.
   }
}
```

### <a name="barcodes"></a>Barkodları

Yalnızca 2B biçimleri desteklenir: PDF417, Aztek, QR. Apple 1 D barkodları arkadan aydınlatma telefon ekranda taramaya unsuited talepleri.

Barkod görüntülenen alternatif metin isteğe bağlı – bazı tüccarların okuma/türü el ile kullanabilmek ister.

ISO 8859 1 kodlama hangi kodlama, geçişleri okuma yapacak tarama sistemleri tarafından kullanılan en yaygın olup olmadığını denetler.

### <a name="relevancy-lock-screen"></a>İlgi (kilit ekranı)

Kilit ekranında görüntülenecek bir geçişi neden olan veri iki tür vardır:

 **Konum**

Bir geçiş örneğin bir müşteri sık ziyaret depoları veya sinema veya havaalanı konumunu 10 konumları için belirtilebilir. Bir müşteri bu konumları yardımcı uygulama aracılığıyla ayarlayabilirsiniz veya sağlayıcı bunları kullanım verilerini (Müşteri'nin izniyle toplanan varsa) belirlenemedi.

Geçişi üzerinde kilit ekranı gösterildiğinde kullanıcı alanı ayrıldığında geçişi kilit ekranından gizlenmesi bir dilimi hesaplanır. RADIUS kötüye önlemek için stil geçirmek için bağlıdır.

 **Tarih ve Saat**

Bir seferde yalnızca bir tarih/saat belirtilebilir. Tarih ve saat binme geçişleri ve olay biletleri için kilit ekranı anımsatıcılar tetiklemek için yararlıdır.

Tarih/saat (örneğin, bir theatre veya spor karmaşık sezonu bilet) çoklu kullanım bilet durumunda güncelleştirilmesi böylece itme aracılığıyla veya PassKit API aracılığıyla güncelleştirilebilir.

### <a name="localization"></a>Yerelleştirme

Bir seferde birden çok dillere çevirme benzer bir iOS uygulaması yerelleştirme için – belirli dizinlerle dil oluşturmak `.lproj` uzantısı ve yerelleştirilmiş öğeleri içine yerleştirin. Metin çevirileri içine girilmelidir bir `pass.strings` dosya sırada yerelleştirilmiş görüntüleri bunlar Değiştir geçişi kök dizininde görüntü adıyla aynı olmalıdır.

## <a name="security"></a>Güvenlik

Geçişleri iOS sağlama portalı oluşturmak özel bir sertifika ile imzalanmış. Geçişi imzalamak için adımlar şunlardır:

1.  Geçişi dizindeki her bir dosyanın SHA1 karma hesaplama (içermeyen `manifest.json` veya `signature` kümelesini var Bu aşamada yine de dosya).
1.  Yazma `manifest.json` her dosya karması ile JSON anahtar/değer listesi olarak.
1.  Sertifika imzalamak için kullanmak `manifest.json` dosya ve sonuç olarak adlandırılan bir dosyaya yazma `signature` .
1.  Her şeyi ZIP ve sonuçta elde edilen dosyaya verin bir `.pkpass` dosya uzantısı.


Özel anahtarınızı geçişi imzalamak için gerekli olduğundan bu işlem yalnızca denetim güvenli bir sunucuda yapılmalıdır. Anahtarlarınızı deneyin ve bir uygulamada geçişleri oluşturmak için dağıtmayın.

 
## <a name="configuration-and-setup"></a>Yapılandırması ve Kur

Bu bölüm sağlama ayrıntılarınızı Kurulum ve ilk geçiş oluşturun yardımcı olacak yönergeler içerir.

### <a name="provisioning-passkit"></a>PassKit sağlama

Uygulama mağazası girmek bir geçiş için sırayla bir geliştirici hesabını bağlı olması gerekir. Bu iki adımı gerektirir:

1.  Geçişi geçirmek türü kimliği adlı benzersiz bir tanımlayıcıyı kullanarak kayıtlı olması gerekir
1.  Geçişi Geliştirici dijital imza ile imzalamak için geçerli bir sertifika oluşturulması gerekir.

Tür kimliği geçirmek aşağıdaki oluşturmak için.


<a name="create-passid"/>

#### <a name="create-a-pass-type-id"></a>Bir geçiş türü kimliği oluşturur

Geçirmek türü kimliği için her farklı ayarlamak için ilk adımdır _türü_ geçişi desteklenmez. Geçirmek kimliği (veya geçirmek tür tanımlayıcısı) geçişi için benzersiz bir tanımlayıcı oluşturur. Bir sertifika kullanılarak Geliştirici hesabınızla geçişi bağlamak için bu kimliği kullanacağız.

1. İçinde [iOS sağlama Portalı'nın sertifikaları, tanımlayıcılarını ve profilleri bölümüne](https://developer.apple.com/account/overview.action), gitmek **tanımlayıcıları** seçip **geçirmek tür kimlikleri** . Ardından **+** yeni bir geçiş türü oluşturmak için düğmeye: [ ![ ] (passkit-images/passid.png "yeni bir geçiş türü oluşturma")](passkit-images/passid.png#lightbox)

2.   Sağlayan bir **açıklama** (ad) ve **tanımlayıcısı** (benzersiz bir dize) geçişi için. Tüm geçirmek tür kimlikleri dizesi ile başlamalıdır Not `pass.` kullandığımız Bu örnekte `pass.com.xamarin.coupon.banana` : [ ![ ] (passkit-images/register.png "tanımlayıcısı ve açıklama sağlayın")](passkit-images/register.png#lightbox)


3.   Tuşlarına basarak geçirmek Kimliğini onaylayın **kaydetmek** düğmesi.


<a name="generate" />

#### <a name="generate-a-certificate"></a>Bir sertifika oluşturma

Bu geçirmek türü kimliği için yeni bir sertifika oluşturmak için aşağıdakileri yapın:

1.  Listeden yeni oluşturulan geçirmek kimliği seçin ve'ı tıklatın **Düzenle** : [ ![ ] (passkit-images/pass-done.png "yeni geçirmek kimliği listeden seçin")](passkit-images/pass-done.png#lightbox)

    Ardından, seçin **sertifika oluştur...** :

    [![](passkit-images/cert-dist.png "Sertifika Seç oluşturun")](passkit-images/cert-dist.png#lightbox)


2.  Bir sertifika imzalama isteği (CSR) oluşturmak için aşağıdaki adımları izleyin.
  
3. Tuşuna **devam** düğmesi Geliştirici portalında ve sertifikanızı oluşturmak için CSR dosyasını karşıya yükleyin.

4. Sertifikayı indirin ve, Anahtarlıkta yüklemek için çift tıklayın.


Bu geçirmek türü kimliği için bir sertifika oluşturduk, sonraki bölümde geçiş el ile nasıl oluşturulacağını açıklar.

Cüzdan sağlama hakkında daha fazla bilgi için başvurmak [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Kılavuzu.

 <a name="Create_a_Pass_Manually" />

### <a name="create-a-pass-manually"></a>Bir geçiş el ile oluşturun

Biz geçirmek türü oluşturduğunuza göre biz benzeticisi veya bir aygıt üzerinde test etmek için bir geçiş elle hazırlayabilirsiniz. Bir geçiş oluşturmaya yönelik adımlar şunlardır:

-  Geçişi dosyaları içerecek bir dizin oluşturun.
-  Tüm gerekli verileri içeren bir pass.json dosyası oluşturun.
-  Görüntüler klasörüne (gerekliyse) ekleyin.
-  Klasördeki her dosya için SHA1 karma değerleri hesaplamak ve manifest.json için yazma.
-  Manifest.JSON indirilen sertifika .p12 dosyasını ile oturum açın.
-  Dizinin içeriğini ZIP ve .pkpass uzantısı ile yeniden adlandırın.


Bir geçiş oluşturmak için kullanılan bu makaledeki örnek kodda bazı kaynak dosyaları vardır. Dosyaları kullanmak `CouponBanana.raw` CreateAPassManually dizininin dizin. Aşağıdaki dosyalar mevcuttur:

 [![](passkit-images/image18.png "Bu dosyaları yok")](passkit-images/image18.png#lightbox)

Pass.JSON açın ve JSON düzenleyin. En az güncelleştirmelisiniz `passTypeIdentifier` ve `teamIdentifer` Apple Geliştirici hesabınızda eşleşecek şekilde.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Ardından her dosya için karma değerleri hesaplamak ve oluşturmanız `manifest.json` dosya. İşiniz bittiğinde şunun gibi görünür:

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

Sonraki imza Bu geçiş türü kimliği için oluşturulan sertifikası (.p12 dosyası) kullanarak bu dosya için oluşturulmuş olması gerekir

 <a name="Signing_On_a_Mac" />


#### <a name="signing-on-a-mac"></a>Mac üzerinde imzalama

Karşıdan **Cüzdan çekirdek destek malzemeleri** gelen [Apple indirmeleri](https://developer.apple.com/downloads/index.action?name=Passbook) site. Kullanım `signpass` klasörünüze (Bu da hesaplar SHA1 karma hale getirir ve çıktı .pkpass dosyasına ZIP) geçişi dönüştürmek için aracı.

 <a name="Signing_On_a_PC" />


#### <a name="signing-on-a-pc"></a>Bir bilgisayarda imzalama

Aşağıdaki örnekte bu makalede var. adlı bir proje kodudur `signpassnet` Windows .NET içinde çalışır. Ancak, daha az doğrulama kodunu içeren Apple'nın aracı taklit edecek şekilde çalışır.

 <a name="Testing" />


#### <a name="testing"></a>Sınama

Bu araçları çıktısını (dosya adı .zip olarak ayarlama ve onu açarak) incelemek için olsaydı, aşağıdaki dosyaları görür (eklenmesi Not `manifest.json` ve `signature` dosyaları):

 [![](passkit-images/image19.png "Bu araçları çıktısını inceleniyor")](passkit-images/image19.png#lightbox)

İmzalı, ZIPped ve dosya (ör. yeniden adlandırılmış bir kez için `BananaCoupon.pkpass`) test etmek için simulator sürükleyin veya bunu kendinize gerçek bir aygıtta almak için e-posta. Bir ekran görürsünüz **Ekle** geçişi, şuna benzer:

 [![](passkit-images/image20.png "Geçişi ekranına ekleyin")](passkit-images/image20.png#lightbox)

Normal olarak bu işlem ancak el ile geçişi oluşturma yalnızca bir arka uç sunucusu desteği gerektirmez kuponlar oluşturmakta olduğunuz küçük ölçekli işletmeler için bir seçenek olabilir, bir sunucuda otomatik.

 <a name="Wallet" />

## <a name="wallet"></a>Cüzdan

Cüzdan geçirmek Seti ekosistemi merkezi bir parçasıdır. Bu ekran, boş Cüzdan ve geçişi listesi ve tek tek geçişleri nasıl göründüğünü gösterir:

 [![](passkit-images/image21.png "Bu ekran boş Cüzdan ve geçişi listesi ve tek tek geçişleri nasıl göründüğünü gösterir")](passkit-images/image21.png#lightbox)

Cüzdan özellikleri:

-  Geçişleri kendi barkod taramak için ile işlenen tek yerdir.
-  Kullanıcı güncelleştirme ayarlarını değiştirebilirsiniz. Etkinleştirilirse, anında iletme bildirimleri veri geçişi güncelleştirmeleri tetikleyebilir.
-  Kullanıcı etkinleştirebilir veya ekran kilitleme tümleştirme devre dışı bırakın. Etkinleştirilirse, bu otomatik olarak kendi kilit ekranında görünmesini geçişi sağlar geçişinde katıştırılmış ilgili zaman ve yer verileri temel.
-  Web-server-URL geçirmek JSON'de sağlandıysa çekme yenileme, geçişi ters tarafının destekler.
-  Yardımcı uygulamaları açılabilen (veya indirdiğiniz) uygulamanın Kimliğini geçirmek JSON'de sağlandıysa.
-  Geçişleri (bir şirin shredding ile animasyonu) silinebilir.


 <a name="Getting_Passes_into_Wallet" />

## <a name="adding-passes-into-wallet"></a>Cüzdan geçişe ekleme

Geçişleri için Cüzdan aşağıdaki yollarla eklenebilir:

* **Conduit uygulamaları** – bu geçişleri doğrudan değiştiremez, sadece geçişi dosyalarını yüklemek ve kullanıcı için Cüzdan ekleme seçeneği sunar. 

* **Uygulamaları Yahoo! Companion** – bu geçişleri dağıtmak ve göz atın veya bunları düzenlemek için ek işlevler sunmak için sağlayıcıları tarafından yazılır. Xamarin.iOS uygulamaları oluşturma ve geçişleri geçirmek Seti API tam erişimi. Geçişleri sonra eklenebilir Cüzdan kullanarak `PKAddPassesViewController`. Bu işlem daha ayrıntılı olarak açıklanmıştır **yardımcı uygulamaları** bu belgenin bölüm.

### <a name="conduit-applications"></a>Conduit uygulamaları

Conduit uygulamalar, bir kullanıcı adına geçişleri alabilir ve kendi içerik türünü tanır ve Cüzdan eklemek için işlevselliği sağlamak üzere programlanmış Ara uygulamalardır. Conduit uygulamaları örnekleri şunlardır:

-   **Posta** – ek bir geçişi olarak tanır.
-   **Safari** – bir URL geçirmek bağlantısı tıklatıldığında geçirmek Content-Type tanır.
-   **Diğer özel uygulamaları** – eklerini alırsınız veya bağlantılar (sosyal medya istemcileri, posta okuyucular, vb.) herhangi bir uygulama.


Bu ekran gösterir nasıl **posta** iOS 6 geçişi eki tanır ve (işlemdeki olduğunda) sunar **Ekle** Cüzdan için.

 [![](passkit-images/image22.png "Bu ekran görüntüsü, iOS 6 Mail'de geçişi ek nasıl tanıdığını gösterir.")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "Bu ekran görüntüsü, posta Cüzdan bir geçişi ek eklemek nasıl sağladığını gösterir.")](passkit-images/image23.png#lightbox)

Geçiş için bir iletken olabilir bir uygulama geliştiriyorsanız, bunlar tarafından tanınan:

-  **Dosya uzantısı** -.pkpass
-  **MIME türü** -application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


Temel bir conduit uygulamasının geçişi dosyasını alın ve geçirmek Seti'nın çağırmak için bir işlemdir `PKAddPassesViewController` kullanıcıya kendi Cüzdan geçişi ekleme seçeneğini sunmak için. Bu görünüm denetleyicisini uyarlamasını sonraki bölümde yer alan **yardımcı uygulamaları**.

Conduit uygulamalar belirli bir geçirmek türü kimliği için yardımcı uygulamalar aynı şekilde sağlanması gerekmez.

## <a name="companion-applications"></a>Yardımcı uygulamalar

Bir geçişi oluşturma, geçişi ile ilişkili bilgileri güncelleştirmek ve aksi durumda geçişleri yönetme gibi geçişleri ile çalışmak için ek işlevler uygulama ile ilişkili bir yardımcı uygulaması sağlar.

Yardımcı uygulamaları Cüzdan özelliklerini yinelenen denememeniz gerekir. Tarama için geçişleri görüntülenecek amaçlanmamıştır.

Bu bölümde bu kalanı etkileşim temel bir yardımcı uygulama geçirmek Seti ile derlemeyi açıklar.

### <a name="provisioning"></a>Sağlama

Cüzdan depolama teknolojisi olduğundan, uygulama ayrı olarak hazırlanması gerekir ve takım sağlama profili veya joker uygulama kimliği kullanamazsınız. Başvurmak [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Kılavuzu Cüzdan uygulama için benzersiz bir uygulama kimliği ve sağlama profili oluşturun.

### <a name="entitlements"></a>Yetkilendirmeler

**Entitlements.plist** dosya tüm yeni Xamarin.iOS projesinde bulunması gerekir. Yeni bir Entitlements.plist dosyası eklemek için adımları [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

Ayarlanacak yetkilendirmeler aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Çift **Entitlements.plist** Entitlements.plist Düzenleyicisi'ni açmak için çözüm defteri dosyasında:

![](passkit-images/image31.png "Entitlements.plst Düzenleyicisi")

Cüzdan bölümü altında seçin **etkinleştirmek Cüzdan** seçeneği

![](passkit-images/image32.png "Cüzdan yetkilendirme etkinleştir")


Tüm türleri geçirmek izin vermek uygulamanız için varsayılan seçenektir. Ancak, uygulamanızı kısıtlamak ve yalnızca bir alt kümesini team geçişi türleri izin vermek mümkündür. Bu seçim etkinleştirmek için **izin alt ekibinin geçirmek türleri** ve izin vermek istediğiniz alt geçişi türü tanımlayıcısını girin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Çift **Entitlements.plist** dosyayı XML kaynak dosyasını açın.

Cüzdan yetkilendirme eklemek için ayarlayın **özelliği** için `Passbook Identifiers` açılır listede, otomatik olarak ayarlayacak **türü** `Array`. Ardından, dize ayarlanmasına **değeri** için `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Cüzdan yetkilendirme etkinleştir")

Bu, tüm türleri geçirmek izin vermek, uygulamanızın olanak tanır. Uygulamanızı kısıtlamak ve yalnızca bir alt kümesini team geçişi türleri izin vermek için dize değeri ayarlayın:

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Burada `pass.$(CFBundleIdentifier)` oluşturuldu geçirmek kimliği [yukarıda](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>Hata Ayıklama

Uygulamanızı dağıtma sorunları yaşıyorsanız, doğru kullanıyorsanız denetleyin **sağlama profili** ve `Entitlements.plist` olarak seçilen **özel yetkilendirmeler** dosyasında**iPhone paket imzalama** seçenekleri.

Bu hata dağıtırken karşılaşırsanız:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

ardından `pass-type-identifiers` yetkilendirmeler dizidir yanlış (veya eşleşmeyen **sağlama profili**). Geçirmek tür kimlikleri ve takım Kimliğinizi doğru olduğunu doğrulayın.

 <a name="Classes" />

## <a name="classes"></a>Sınıflar

Aşağıdaki geçirmek Seti sınıflar uygulamaların geçişleri erişmek için kullanılabilir:

-  **PKPass** – bir geçişi ait bir örnek.
-  **PKPassLibrary** – cihazda geçişleri erişmek için API sağlar.
-  **PKAddPassesViewController** – kullanıcının kendi Cüzdan kaydetmek bir geçiş görüntülemek için kullanılır.
-  **PKAddPassesViewControllerDelegate** – Xamarin.iOS developers


## <a name="example"></a>Örnek

Bu makale için örnek kod PassLibrary projesinde bakın. Bir Cüzdan Yardımcısı uygulamasında gerekli olacak aşağıdaki ortak işlevleri göstermektedir:

### <a name="check-that-wallet-is-available"></a>Cüzdan kullanılabilir olup olmadığını denetleyin

Cüzdan iPad cihazında kullanılabilir olmadığından uygulamaları geçirmek Seti özellikleri erişmeyi denemeden önce denetlemeniz gerekir.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Geçişi kitaplık örneği oluşturma

Geçirmek Seti kitaplığı tek değil, uygulamaları oluşturmak ve depolamak ve geçirmek Seti API erişmek için örneği.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Geçişleri listesini al

Uygulama geçişleri listesini Kitaplığı'ndan talep edebilir. Yetkilendirmeler takım Kimliğiniz ile oluşturulan ve hangi listelenen geçişleri yalnızca görebilmeniz için bu listeyi geçirmek paketi tarafından otomatik olarak filtrelenir.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Bu yöntem her zaman gerçek cihazlarda test şekilde simulator döndürülen geçişleri listesi filtre uygulamadığında olduğunu unutmayın. İki kuponlar eklendikten sonra bu liste bir UITableView örnek uygulama görünümlü bu görüntülenebilir:

 [![](passkit-images/image29.png "Örnek uygulama görünüm aşağıdaki gibi iki kuponlar eklendikten sonra")](passkit-images/image29.png#lightbox)


### <a name="displaying-passes"></a>Geçişleri görüntüleme

Bilgi sınırlı sayıda geçiş Yardımcısı uygulamaların içindeki işleme için kullanılabilir.

Örnek kod yaptığı gibi standart özellikler bu kümesi geçişleri, listesini görüntülemek için seçin.

```csharp
string passInfo =
                "Desc:" + pass.LocalizedDescription
                + "\nOrg:" + pass.OrganizationName
                + "\nID:" + pass.PassTypeIdentifier
                + "\nDate:" + pass.RelevantDate
                + "\nWSUrl:" + pass.WebServiceUrl
                + "\n#" + pass.SerialNumber
                + "\nPassUrl:" + pass.PassUrl;
```

Bu dize bir uyarı örnek olarak gösterilmiştir:

 [![](passkit-images/image30.png "Örnek kupon seçili uyarıda")](passkit-images/image30.png#lightbox)

Aynı zamanda `LocalizedValueForFieldKey()` tasarlanmış geçişleri alanlarındaki verileri almak üzere yöntemi (şunları öğrenmiş olacaksınız bu yana hangi alanlar mevcut olması). Kod örneği, bu göstermez.

### <a name="loading-a-pass-from-a-file"></a>Bir seferde bir dosyasından yükleniyor

Bir seferde yalnızca Cüzdan için kullanıcının izniyle eklenebilir olduğundan, bir görünüm denetleyicisini karar vermenize olanak sağlayacak biçimde sunulmalıdır. Bu kod içinde kullanılan **Ekle** (size değiştirin Bu, oturum açmış başka biriyle) uygulamasında katıştırılmış önceden oluşturulmuş bir geçişi yüklemek için bu örnekteki düğme:

```csharp
NSData nsdata;
using ( FileStream oStream = File.Open (newFilePath, FileMode.Open ) ) {
        nsdata = NSData.FromStream ( oStream );
}
var err = new NSError(new NSString("42"), -42);
var newPass = new PKPass(nsdata,out err);
var pkapvc = new PKAddPassesViewController(newPass);
NavigationController.PresentModalViewController (pkapvc, true);
```

Geçişi ile sunulan **Ekle** ve **iptal** seçenekleri:

 [![](passkit-images/image20.png "Ekle ve iptal seçenekleriyle sunulan geçişi")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>Var olan bir parola Değiştir

Geçişi zaten yoksa, başarısız olur ancak var olan bir parola değiştirme kullanıcının izni gerektirmez.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Bir seferde düzenleme

Kodunuzda geçişi nesneleri güncelleştiremez PKPass değişken değil. Bir seferde bir uygulama verileri değiştirmek için geçiş kaydını tutun ve uygulama yükleyebilirsiniz güncelleştirilmiş değerlerle yeni bir geçişi dosya oluşturmak bir web sunucusuna erişimi olmalıdır.

Geçişleri özel ve güvenli tutulması gereken bir sertifikayla imzalanması gerekir çünkü geçiş dosya oluşturma bir sunucuda yapılmalıdır.

Güncelleştirilmiş geçişi dosyası oluşturulduktan sonra kullanmak `Replace` cihazdaki eski verilerin üzerine yazmak için yöntem.

### <a name="display-a-pass-for-scanning"></a>Bir geçiş taramak için görüntüleme

Cüzdan taramak için bir geçiş görüntüleyebilirsiniz yalnızca, yukarıda belirtildiği. Bir geçişi kullanarak görüntülenebilen `OpenUrl` gösterildiği gibi yöntemi:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Değişiklik bildirimleri alma

Uygulamaları geçirmek Kitaplığı'na yapılan değişiklikleri dinler kullanarak `PKPassLibraryDidChangeNotification`. Bunlar için uygulamanızda dinlemek için iyi bir uygulamadır şekilde güncelleştirmeleri arka planda tetikleme bildirimleri değişiklikleri kaynaklanabilir.

```csharp
noteCenter = NSNotificationCenter.DefaultCenter.AddObserver (PKPassLibrary.DidChangeNotification, (not) => {
    BeginInvokeOnMainThread (() => {
        new UIAlertView("Pass Library Changed", "Notification Received", null, "OK", null).Show();
        // refresh the list
        var passlist = library.GetPasses ();
        table.Source = new TableSource (passlist, library);
        table.ReloadData ();
    });
}, library);  // IMPORTANT: must pass the library in
```

Bir kitaplık örneği PKPassLibrary tek olmadığından bildirim için kaydolurken geçirmek önemlidir.

## <a name="server-processing"></a>Sunucu işleme

Kit geçirmek desteklemek için bir sunucu uygulaması oluşturmanın ayrıntılı bir tartışma giriş bu makalenin kapsamı dışındadır bulunmaktadır.

Sağlanan .NET kodu *signpassnet* örnek kullanılan geçişleri oluşturabilen bir sunucu tarafı yöntemi için temel olarak.

Görünüm [WWDC Video: Tanıtımı Passbook, Kısım 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) 27:00 dakikadan daha fazla bilgi için.

### <a name="other-resources"></a>Diğer kaynaklar

Bkz: [dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook) açık kaynak C# sunucu tarafı kodu.

## <a name="push-notifications"></a>Anında iletme bildirimleri

Geçişleri güncelleştirmek için anında iletme bildirimleri kullanımının ayrıntılı bir tartışma giriş bu makalenin kapsamı dışındadır bulunmaktadır.

Güncelleştirmeler gerekli olduğunda Cüzdan web isteklerini yanıtlamak için Apple tarafından tanımlanan REST benzeri API uygulamak için gerekli. Sağlanan .NET kodu *signpassnet* örnek kullanılabilirdi temel olarak bu istekleri sonucunda yeni geçiş oluşturmak için.

Görünüm [WWDC Video: Tanıtımı Passbook, Kısım 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) 27:00 dakikadan daha fazla bilgi için.

## <a name="summary"></a>Özet

Bu makalede geçirmek Seti sunulan, neden yararlıdır nedenlerinden bazıları ana hatlarıyla ve tam geçirmek Seti Çözüm uygulanmalı farklı bölümleri açıklanmıştır. Geçişleri, el ile hem de Seti geçirmek API'ları bir Xamarin.iOS uygulamasından erişmek nasıl geçiş yapma işlemi oluşturmak için Apple Geliştirici hesabınızda yapılandırmak için gereken adımlar açıklanmaktadır.

## <a name="related-links"></a>İlgili bağlantılar

- [CreateAPassManually (örnek)](https://developer.xamarin.com/samples/PassKit/)
- [PassKit örnek](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [iOS 6’ya Giriş](~/ios/platform/introduction-to-ios6/index.md)
- [Passbook Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Conceptual/PassKit_PG/Chapters/Introduction.html)
- [Passbook geliştiricileri için](https://developer.apple.com/passbook/)
- [Geçişi dosyaları hakkında](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [Kit Framework başvurusu geçirin](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Kit Framework başvurusu geçirin](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passbook Web hizmeti başvurusu](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
