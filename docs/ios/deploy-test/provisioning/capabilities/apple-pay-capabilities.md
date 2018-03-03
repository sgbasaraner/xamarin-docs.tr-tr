---
title: "Apple Pay özellikleri"
description: "Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, Apple Pay özellikleri için gereken kurulum açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 735CC916-16A4-471B-87F7-0535E24288D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 88db45135104f14ca3a4b18e466e95288853a6df
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="apple-pay-capabilities"></a>Apple Pay özellikleri

_Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz, Apple Pay özellikleri için gereken kurulum açıklar._

Apple Pay kullanıcıların iOS cihazlarını aracılığıyla fiziksel mal ödeme olanak tanır. Bu bölümde, Apple pay Apple Geliştirici Merkezi'ndeki için gereken tüm gerekli bileşenleri oluşturmayı açıklar.

Geliştirici Merkezi aracılığıyla yeni bir uygulama sağlama, yapılması gereken üç adım vardır:

1.  Ticari bir kimlik oluşturmak
2.  Geçerli ödeme özelliğine sahip bir uygulama kimliği oluşturmanız ve satıcı ekleyin.
3.  Satıcı Kimliği için bir sertifika oluşturma

Aşağıdaki adımlar yukarıdaki öğelerin oluşturmada size yol gösterecek:

<a name="merchantid" />

## <a name="create-merchant-id"></a>Satıcı kimliği oluşturur

Ödemeler kabul edebilir ve PassKit için 's geçirilen bilmeniz Apple Pay izin vermek için kullanılan bir satıcı kimliği `PaymentRequest` yöntemi ve Apple Pay yetki verme kullanılır:

1.  Gözat [Apple Geliştirici Merkezi](https://developer.apple.com/account/) ve sertifikalar, tanımlayıcı ve profilleri bölümüne gidin: 
 
    ![Geliştirici Merkezi satıcı kimliği seçimi](apple-pay-capabilities-images/image57.png)

2.  Altında **tanımlayıcıları**seçin **satıcı kimlikleri**ve ardından  **+**  yeni bir satıcı oluşturmak için kimliği:  

3.  Aşağıda, yeni bir açıklama ve tanımlayıcı ile gösterilen formu doldurun. Açıklama kimliği tanımlanabilen sunar ve daha sonra değiştirilebilir. Tanımlayıcı için benzersiz olmalıdır ve dizesi ile başlamalıdır `merchant`. Apple önerir tanımlayıcısı şu biçimde olmalıdır: `merchant.com.[Your-App-Name]`:
   
    ![Yeni satıcı kimliği ayrıntıları](apple-pay-capabilities-images/image58.png)

4.  Ayrıntılarınızı doğrulayın ve **kaydetmek** Kimliğinizi: 
    
    ![Satıcı Kimliği onayı](apple-pay-capabilities-images/image59.png)

<a name="appid" />

## <a name="create-an-app-id-with-the-apple-pay-capability-that-includes-the-merchant-id"></a>Satıcı Kimliği içeren Apple Pay özelliğine sahip bir uygulama kimliği oluşturma

1.  İçinde [Geliştirici Merkezi](https://developer.apple.com/account/) tıklayın **uygulama kimlikleri** altında **tanımlayıcıları**: 
    
    ![Geliştirici Merkezi'nde uygulama Kimliğini seçin](apple-pay-capabilities-images/image6.png)

2.  Seçin  **+**  düğmesi yeni bir uygulama kimliği eklemek için: 
   
    ![Yeni uygulama kimliği düğme ekleme](apple-pay-capabilities-images/image27.png)

3.  Uygulama kimliği için bir ad girin ve açık bir uygulama kimliği verin:    
   
    ![Uygulama Kimliği ayrıntıları ekranı ](apple-pay-capabilities-images/image35.png)

4.  Uygulama Hizmetleri altında Apple Pay seçin:    
  
    ![Uygulama Hizmetleri Apple ödeme](apple-pay-capabilities-images/image36.png)

5.  Seçin **devam** ve ardından **kaydetmek**. Apple Pay onay ekranında, sarı bir simgeyle seçili yapılandırılabilir ile görüntüler unutmayın: 
   
    ![Apple Pay onay ekranı](apple-pay-capabilities-images/image37.png)

6.  Uygulama kimlikleri listesine dönmek ve az önce oluşturduğunuz birini seçin:  
   
    ![Uygulama Kimliği Düzenle](apple-pay-capabilities-images/image38.png)

7.  Bu genişletilmiş bölüm en altına kadar kaydırın ve tıklayın **Düzenle**.
8.  Apple Pay listesine aşağı kaydırın ve tıklayın **Düzenle** düğmesi:  
    
    
    ![Apple ödeme uygulama kimliği Ayrıntıları Düzenle](apple-pay-capabilities-images/image39.png)
9.  Bu uygulama kimliği ile kullanın ve'ı tıklatın satıcı Kimliğini seçin **devam**:  
    
    ![Satıcı uygulama kimliği için kullanmak üzere bir kimlik seçin](apple-pay-capabilities-images/image40.png)

10. Satıcı Kimliği onaylayın atamaları ve tuşuna **atamak**:  
    
    ![Onay ekranı](apple-pay-capabilities-images/image41.png)

Bu uygulama kimliği artık oluşturmak ya da, yeni bir sağlama profili yeniden oluşturmak için açıklandığı gibi kullanılabilir [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu. 

<a name="certificate" />

## <a name="create-a-certificate-for-your-merchant-id"></a>Satıcı Kimliği için bir sertifika oluşturun

Bir sertifika, Apple tarafından işlemle ilişkili hassas verileri şifrelemek için gereklidir. Oluşturulan her satıcı kimliği, kendi sertifika olması gerekir. 

Bir sertifika oluşturmak için aşağıdaki adımları izleyin:

1.  Yukarıda oluşturulan satıcı kimliği seçin ve basın **Düzenle**: 
    
    ![Satıcı Kimliği iletişim Düzenle](apple-pay-capabilities-images/image42.png)

2.  İOS satıcı kimliği ayarları ekranında tıklatın **oluşturduğunuz sertifika**: 
   
    ![Ödeme işleme sertifika oluştur](apple-pay-capabilities-images/image43.png)

3.  Aşağıdaki soruyu yanıtlayın: 

    ![özel olarak Çin'de ödemeler işlenecekse adresi](apple-pay-capabilities-images/image44.png)

4.  Bu noktada oluşturmak için istenir bir _sertifika imzalama isteği_: 

    ![Sertifika imzalama isteği oluşturma](apple-pay-capabilities-images/image45.png)
    
    > [!IMPORTANT]
> Apple Pay, böyle bir JudoPay veya Stripe için ödeme sağlayıcısını kullanıyorsanız, bu noktada kullanmak için düzgün şekilde biçimlendirilmiş bir CSR sağlayabilirsiniz. Bu isteyen bilgi bulundu [JudoPay](https://www.judopay.com/docs/version-52/apple-pay/getting-started/#create-an-apple-pay-certificate) ve [Stripe](https://stripe.com/docs/apple-pay/apps#csr) siteleri. Kendi CSR oluşturmak için 5-8 aşağıdaki adımları izleyin. Bulduktan sonra CSR 9. adıma gidin.

5.  Anahtarlık erişimi uygulamayı açın ve gidin **Anahtarlık erişimi > sertifika Yardımcısı > bir sertifika yetkilisinden bir sertifika isteği:** 

     ![Anahtarlık bir Mac kullanarak bir CSR oluşturma](apple-pay-capabilities-images/image46.png)

6.  E-posta adresinizi girin, özel anahtar için bir ad girin, CA e-posta adresi boş seçin bırakın **diske kaydedin** seçeneğini ve **anahtar çifti bilgilerini belirtmek izin ver**:

     ![Sertifika bilgileri iletişim kutusu](apple-pay-capabilities-images/image47.png)

7.  CSR uygun bir konuma kaydedin: 

     ![Yerel makineye CSR kaydetme](apple-pay-capabilities-images/image48.png)

8.  Anahtar çiftini bilgi ekran ayarlamak **anahtar boyutu** için **256 bit** ve **algoritması** için **ECC** tıklatıp **devam**:

     ![Anahtar çifti bilgi iletişim girin](apple-pay-capabilities-images/image49.png)

9.  Geliştirici Merkezi'nde tıklatın **devam** CSR karşıya yüklemek için: 

     ![Geliştirici Merkezi'nde CSR karşıya yüklemek hazırlama](apple-pay-capabilities-images/image50.png)

10. Önceki adımda oluşturduğunuz .pem dosyasını yüklemek için **Dosya Seç…** tuşuna basın ve CSR seçmek için **devam** Geliştirici portalına karşıya yüklemek için: 

     ![Geliştirici Merkezi'nde CSR dosyasını karşıya yükleyin](apple-pay-capabilities-images/image51.png)

11. Sertifika oluşturulduktan sonra indirin ve çift Anahtarlığa da yüklemek için tıklayın.

Apple Pay kullanma hakkında daha fazla bilgi için aşağıdaki Kılavuzu'na bakın:

*   [Apple Pay giriş](~/ios/platform/apple-pay.md)

## <a name="next-steps"></a>Sonraki Adımlar
 
Aşağıdaki liste yapılması gereken ek adımları açıklar:

* Uygulamanızda framework ad kullanın.
* Gerekli yetkilendirmeler uygulamanıza ekleyin. Gerekli yetkilendirmeler ve bunları ekleme hakkında bilgi ayrıntılı olarak [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.
* Uygulamasının **iOS paket imzalama**, emin **özel yetkilendirmeler** ayarlanır **Entitlements.plist**. Bu _değil_ hata ayıklama ve iOS simülatörü varsayılan ayarı oluşturur.

Uygulama Hizmetleri ile ilgili sorunlarla karşılaşırsanız, başvurmak [sorun giderme](~/ios/deploy-test/provisioning/capabilities/index.md) ana Kılavuzu'nun bölümünde.