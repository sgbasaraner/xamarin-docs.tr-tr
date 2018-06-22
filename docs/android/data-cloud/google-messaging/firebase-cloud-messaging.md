---
title: Firebase bulut Mesajlaşma
description: Firebase bulut Mesajlaşma (FCM), mobil uygulamaları ve sunucu uygulamaları Mesajlaşma kolaylaştıran bir hizmettir. Bu makalede FCM nasıl çalıştığını genel bir bakış sağlar ve uygulamanızı FCM kullanabilmesi için Google hizmetlerin nasıl yapılandırılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: ef2c23d16545d03dc267054a96f8b0f8883afcf1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769825"
---
# <a name="firebase-cloud-messaging"></a>Firebase bulut Mesajlaşma

_Firebase bulut Mesajlaşma (FCM), mobil uygulamaları ve sunucu uygulamaları Mesajlaşma kolaylaştıran bir hizmettir. Bu makalede FCM nasıl çalıştığını genel bir bakış sağlar ve uygulamanızı FCM kullanabilmesi için Google hizmetlerin nasıl yapılandırılacağı açıklanmaktadır._

[![Firebase Cloud Messaging kahramanı görüntüsü](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Bu konuda nasıl Firebase bulut Mesajlaşma iletileri Xamarin.Android uygulamanıza bir uygulama sunucusu arasındaki yönlendiren bir üst düzey genel bakış sunar ve uygulamanızı FCM hizmetleri kullanabilmesi için kimlik bilgileri alınırken için adım adım bir yordam sağlar.



## <a name="overview"></a>Genel Bakış

Firebase bulut Mesajlaşma (FCM) gönderme, Yönlendirme ve sunucu uygulamaları ve mobil istemci uygulamalar arasında iletilerinin çağrısı işleyen platformlar arası bir hizmettir. Google Cloud Messaging (GCM) için ardıl FCM olduğu ve Google Play hizmetleri üzerinde oluşturulmuştur.

Aşağıdaki çizimde gösterildiği gibi FCM ileti gönderenler ve istemciler arasında bir aracı gibi davranır. A *istemci uygulaması* bir cihazda çalışan FCM özellikli bir uygulama olur. *Uygulama sunucusu* (siz veya şirketiniz tarafından sağlanan) istemci uygulamanızı FCM iletişim kuran FCM etkin sunucusudur. GCM FCM Firebase konsol bildirimleri GUI yoluyla doğrudan istemci uygulamaları için ileti göndermenizi sağlar:

[![İstemci uygulaması ve bir uygulama sunucusu arasında FCM bulunur](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM kullanarak, uygulama sunucuları iletileri tek bir cihazı, aygıtların bir gruba veya çok sayıda bir konuya abone olduğunuz cihaz gönderebilirsiniz. Bir istemci uygulaması FCM (örneğin, uzak bildirimleri almak için) bir uygulama sunucudan aşağı akış iletileri abone olmak için kullanabilirsiniz. Firebase iletileri farklı uygulama türleri hakkında daha fazla bilgi için bkz: [FCM iletileri hakkında](https://firebase.google.com/docs/cloud-messaging/concept-options).



## <a name="firebase-cloud-messaging-in-action"></a>Eylem Mesajlaşma firebase bulut

Bir uygulama sunucusundan istemci uygulamaları için bir aşağı akış ileti gönderildiğinde, uygulama sunucusu iletisi gönderir. bir *FCM bağlantı sunucusu* Google tarafından; sağlanan FCM bağlantı sunucusu çalıştıran bir cihaza ileti sırayla iletir. istemci uygulaması. İletileri HTTP üzerinden gönderilebilir veya [XMPP](https://developers.google.com/cloud-messaging/ccs) (Genişletilebilir Mesajlaşma ve varlığı Protokolü). İstemci uygulamaları her zaman bağlı olduğundan veya çalıştıran, yeniden bağlanın ve kullanılabilir hale gibi istemci uygulamalarını göndererek FCM bağlantı sunucu enqueues ve depoları iletileri. Benzer şekilde, FCM olacak enqueue istemci uygulamasından Yukarı Akış iletileri uygulama sunucusuna uygulama sunucusu kullanılamıyorsa. FCM bağlantı sunucuları hakkında daha fazla bilgi için bkz: [hakkında Firebase bulut Mesajlaşma sunucusu](https://firebase.google.com/docs/cloud-messaging/server).

Uygulama sunucusu ve istemci uygulaması tanımlamak için aşağıdaki kimlik bilgilerini FCM kullanır ve FCM aracılığıyla ileti işlemlerini yetkilendirmek için bu kimlik bilgilerini kullanır:

-   **Gönderen Kimliği** &ndash; *Gönderen Kimliği* Firebase projenizi oluşturduğunuzda, atanmış benzersiz bir sayısal değer. Gönderen Kimliği istemci uygulamasına iletiler gönderebilir her uygulama sunucusunu tanımlamak için kullanılır. Gönderen Kimliği, ayrıca, proje numarası olan; Projenizi kaydettiğinizde Firebase konsolundan Gönderen Kimliği alın. Gönderen Kimliği örneğidir `496915549731`.

-   **API anahtarı** &ndash; *API anahtarı* uygulama sunucusu erişmenizi Firebase Hizmetleri; FCM uygulama sunucusunun kimliğini doğrulamak için bu anahtarı kullanır. Bu kimlik bilgisi olarak da adlandırılan *sunucu anahtarı* veya *Web API anahtarı*. Bir API anahtarı örneğidir `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   **Uygulama Kimliği** &ndash; kimliğini FCM iletileri almak için kaydeden istemci uygulamanızın (herhangi belirli bir aygıt bağımsız olarak). Bir uygulama kimliği örneğidir `1:415712510732:android:0e1eb7a661af2460`.

-   **Kayıt belirtecinizi** &ndash; *kayıt belirteci* (olarak da adlandırılan *örnek kimliği*) istemci uygulamanızı belirli bir aygıttaki FCM kimliğidir. Kayıt belirtecinizi çalışma zamanında oluşturulan &ndash; bir cihazda çalıştığı sırada ilk FCM ile kaydeder, uygulamanızı bir kayıt belirtecini alır. Kayıt belirteci (belirli bu cihazda çalışan) istemci uygulamanızı örneği FCM iletileri almak için yetkilendirir.
    Bir kayıt belirtecinizi örneğidir `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (çok uzun bir dize).

[Ayar yukarı Firebase Cloud Messaging](#setup_fcm) (Bu kılavuzun sonraki) bir proje oluşturma ve bu kimlik bilgileri oluşturmak için ayrıntılı yönergeler sağlar. Yeni bir proje oluşturduğunuzda [Firebase konsol](https://console.firebase.google.com/), bir kimlik bilgileri dosyası adlı **google services.json** oluşturulan &ndash; içindeaçıklandığıgibibudosyayıXamarin.Androidprojenizeekleyin[ Uzak bildirimleri FCM ile](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

Aşağıdaki bölümlerde istemci uygulamaları FCM aracılığıyla uygulama sunucuları ile iletişim kurduğunda bu kimlik bilgilerini nasıl kullanıldığı açıklanmaktadır.


<a name="registration" />

### <a name="registration-with-fcm"></a>FCM kaydı

Mesajlaşma gerçekleşebilmesi için öncelikle bir istemci uygulaması önce FCM ile kaydetmeniz gerekir. İstemci uygulaması, aşağıdaki çizimde gösterilen kayıt adımları tamamlamanız gerekir:

[![Uygulama kayıt adımları diyagramı](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  İstemci uygulaması Gönderen Kimliği, API anahtarı ve uygulama kimliği için FCM geçirme bir kayıt belirtecinizi almak için FCM iletişim kurar.

2.  FCM istemci uygulamasına bir kayıt belirtecini döndürür.

3.  İstemci uygulaması (isteğe bağlı) kayıt belirtecinizi uygulama sunucusuna iletir.

Uygulama sunucusu, istemci uygulaması ile sonraki iletişim için kayıt belirtecinizi önbelleğe alır. Uygulama sunucusu, kayıt belirtecinizi alındı göstermek için istemci uygulamaya tekrar bir bildirim gönderebilirsiniz. Bu anlaşma gerçekleştikten sonra istemci uygulaması iletilerden alabileceğiniz (veya ileti göndermek) uygulama sunucusu. Eski belirteci aşılıp aşılmadığını istemci uygulaması yeni bir kayıt belirteci alabilirsiniz (bkz [FCM ile uzaktan bildirimleri](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) nasıl bir uygulama kaydı belirteci güncelleştirmeleri alır, bir örnek için).

İstemci uygulaması artık uygulama sunucusundan iletileri almak istediğinde kayıt belirtecini silmek için uygulama sunucusuna bir istek gönderebilirsiniz. İstemci uygulaması bir aygıttan kaldırılırsa, FCM bu algılar ve uygulama kayıt belirtecini silmek sunucuya otomatik olarak bildirir.



### <a name="downstream-messaging"></a>Aşağı Akış Mesajlaşma

Aşağıdaki diyagramda, nasıl Firebase Cloud Messaging depolar ve aşağı akış iletileri iletir gösterilmektedir:

[![Aşağı Akış Mesajlaşma deposu ve İleri FCM kullanır](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Uygulama sunucusu istemci uygulamasına bir aşağı akış ileti gönderdiğinde, yukarıdaki diyagramda numaralandırılmış olarak aşağıdaki adımları kullanır:

1.  Uygulama sunucusu için FCM iletisi gönderir.

2.  İstemci aygıtı kullanılamıyorsa FCM sunucu ileti sırasındaki sonraki iletim için depolar. İleti en fazla 4 hafta için FCM depolama birimindeki korunur (daha fazla bilgi için bkz: [bir ileti Sysprep'in ayarlama](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  İstemci aygıtı kullanılabilir olduğunda, FCM bu cihaza istemci uygulamanın iletiyi iletir.

4.  İstemci uygulaması FCM ileti alır, işler ve kullanıcıya görüntüler. Örneğin, uzak bir bildirim iletisiyse bildirim alanında kullanıcıya sunulur.

(Burada uygulama sunucusuna ileti gönderir tek istemci uygulamaları için) Bu senaryoda ileti, ileti uzunluğu en fazla 4kB olabilir.

Android aşağı akış FCM iletileri alma hakkında ayrıntılı bilgi için bkz: [FCM ile uzaktan bildirimleri](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

### <a name="topic-messaging"></a>Konu Mesajlaşma

*Konu ileti* belirli bir konuyu çevirdiniz birden çok aygıt bir ileti göndermek bir uygulama sunucusu için mümkün kılar. Ayrıca, oluşturabilir ve Firebase konsol bildirimleri GUI aracılığıyla konu iletileri gönderebilirsiniz. Yönlendirme ve abone olunan istemcilere konu iletilerin teslim FCM işler. Bu özellik, hava durumu uyarıları, borsa bilgileri ve başlık haber gibi iletileri için kullanılabilir.

[![Konu ileti diyagramı](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

(İstemci uygulamasını daha önce açıklandığı gibi bir kayıt belirteci aldıktan sonra) aşağıdaki adımları konu Mesajlaşma kullanılır:

1.  İstemci uygulaması, FCM için bir abonelik iletisi göndererek bir konuya abone olur.

2.  Uygulama sunucusu için dağıtım için FCM konu iletileri gönderir.

3.  Bu konuya abone olan istemcilere konu iletileri FCM iletir.

Google Firebase konu ileti hakkında daha fazla bilgi için bkz: [konu mesajlaşmayı Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Firebase ayarlama bulut Mesajlaşma

Uygulamanızda FCM Hizmetleri kullanabilmeniz için önce yeni bir proje oluşturun (veya varolan projeyi içeri gerekir) aracılığıyla [Firebase konsol](https://console.firebase.google.com/). Uygulamanız için bir Firebase Cloud Messaging projesi oluşturmak için aşağıdaki adımları kullanın:

1.  Oturum [Firebase konsol](https://console.firebase.google.com/) tıklatın ve Google hesabı (yani, Gmail adresiniz) ile **yeni proje oluştur**:

    [![Yeni Proje düğmesi oluşturma](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Varolan projeyi varsa tıklatın **Google proje alma**.

2.  İçinde **proje oluşturma** iletişim kutusunda, projenizin adını girin ve tıklayın **proje oluştur**. Aşağıdaki örnekte, yeni bir proje adı verilen **XamarinFCM** oluşturulur:

    [![Bir projesi oluştur iletişim kutusu](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Firebase konsolunda **genel bakış**, tıklatın **Android uygulamanıza eklemek Firebase**:

    [![Android uygulamanızı Firebase Ekle](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  Sonraki ekranda, uygulamanızı paket adını girin. Bu örnekte, paket adı olan **com.xamarin.fcmexample**. Bu değer, Android uygulamanızın paket adı eşleşmelidir. Bir uygulama takma adı da girilebilir **uygulama takma ad** alan:

    [![FCM örnek uygulama takma ad girme](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  Uygulamanızı dinamik bağlantılar, davetiye veya Google kimlik doğrulaması kullanıyorsa, imzalama sertifikası, hata ayıklama girmeniz gerekir. İmzalama sertifikanızın bulma hakkında daha fazla bilgi için bkz: [Keystore'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md).
    Bu örnekte, imzalama sertifikası boş bırakılır.

6.  Tıklatın **Ekle uygulama**:

    [![Uygulama Ekle düğmesi](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Bir sunucu API anahtarı ve bir istemci kimliği uygulama için otomatik olarak oluşturulur. Bu bilgiler, paketlenmiş bir **google services.json** tıkladığınızda, otomatik olarak karşıdan dosya **uygulama Ekle**.
    Bu dosyayı güvenli bir yerde kaydettiğinizden emin olun.

Ekleme konusunda ayrıntılı bir örnek için **google services.json** android'de FCM anında bildirim iletileri almak için bir uygulama projesi için bkz: [FCM ile uzaktan bildirimleri](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).



## <a name="for-further-reading"></a>Daha Fazla Bilgi İçin

-   Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) Firebase Cloud Messaging'ın anahtar özelliklerine genel bakış, nasıl çalıştığı ve kurulum yönergeleri hakkında bir açıklama sağlar.

-   Google [uygulama sunucu gönderme istekleri yapı](https://firebase.google.com/docs/cloud-messaging/send-message) uygulama sunucunuzla iletileri göndermek açıklanmaktadır.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) ve [RFC 6121](https://tools.ietf.org/html/rfc6121) açıklayabilir ve Genişletilebilir ileti ve iletişim durumu Protokolü (XMPP) tanımlayın.



## <a name="summary"></a>Özet

Bu makalede Firebase bulut Mesajlaşma (FCM), genel bakış sağlanır. Tanımlamak ve uygulama sunucuları ve istemci uygulamalar arasında Mesajlaşma yetkilendirmek için kullanılan çeşitli kimlik bilgilerini açıklanmıştır. Kayıt ve aşağı akış Mesajlaşma senaryoları gösterilmiştir ve FCM hizmetleri kullanmaya FCM ile uygulamanızı kaydetme adımları ayrıntılı.


## <a name="related-links"></a>İlgili bağlantılar

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
