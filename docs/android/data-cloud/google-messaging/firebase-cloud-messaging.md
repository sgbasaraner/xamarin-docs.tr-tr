---
title: Firebase Cloud Messaging
description: Firebase Cloud Messaging (FCM) sunucu uygulamaları ve mobil uygulamalar arasında ileti kolaylaştıran bir hizmettir. Bu makalede FCM nasıl çalıştığına ilişkin genel bir bakış sağlar ve uygulamanızın FCM kullanabilmesi için Google hizmetlerin nasıl yapılandırılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: 92c402085627bbe67d4cd8ccaf60a68aa979f3e7
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514301"
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_Firebase Cloud Messaging (FCM) sunucu uygulamaları ve mobil uygulamalar arasında ileti kolaylaştıran bir hizmettir. Bu makalede FCM nasıl çalıştığına ilişkin genel bir bakış sağlar ve uygulamanızın FCM kullanabilmesi için Google hizmetlerin nasıl yapılandırılacağı açıklanmaktadır._

[![Firebase Cloud Messaging hero görüntüsü](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Bu konu, Xamarin.Android uygulamanız ile bir uygulama sunucusu arasında iletileri Firebase Cloud Messaging'in nasıl yönlendirdiğini bir üst düzey genel bakış sağlar ve uygulamanızın FCM hizmetleri kullanabilmesi için kimlik bilgilerini alma için adım adım bir yordam sağlar.

## <a name="overview"></a>Genel Bakış

Firebase Cloud Messaging (FCM) gönderme, Yönlendirme ve sunucu uygulamaları ve mobil istemci uygulamalar arasında ileti kuyruğa alma işleyen bir çoklu platform hizmetidir. FCM Google Cloud Messaging (GCM) için geçmiştir ve Google Play hizmetleri üzerinde oluşturulmuştur.

Aşağıdaki diyagramda gösterildiği gibi FCM ileti gönderenler ve istemcileri arasında bir aracı olarak görev yapar. A *istemci uygulaması* bir cihazda çalışan bir FCM özellikli uygulamadır. *Uygulama sunucusu* (siz veya şirketiniz tarafından sağlanan) istemci uygulamanızı FCM ile iletişim kuran FCM etkin sunucu. GCM, FCM, Firebase Konsolu bildirimleri GUI aracılığıyla doğrudan istemci uygulamaları için ileti göndermek kılar:

[![İstemci uygulaması ile bir uygulama sunucusu arasında FCM bulunur.](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM kullanarak, uygulama sunucuları iletiler tek bir cihaz grubuna veya bir konuya abone olduğu cihaz sayısı gönderebilirsiniz. Bir istemci uygulaması, uygulama sunucusundan (örneğin, uzak bildirimler almak için) için aşağı akış iletilerine abone olunmaya FCM kullanabilirsiniz. Firebase iletileri farklı türleri hakkında daha fazla bilgi için bkz. [FCM iletileri hakkında](https://firebase.google.com/docs/cloud-messaging/concept-options).

## <a name="fcm-in-action"></a>Firebase Cloud Messaging sürüyor

Bir istemci uygulaması için bir uygulama sunucusundan bir aşağı akış ileti gönderildiğinde, uygulama sunucusuna ileti gönderir. bir *FCM bağlantı sunucusu* ; Google tarafından sağlanan FCM bağlantı sunucusu çalıştıran bir cihaza ileti sırayla iletir. istemci uygulaması. HTTP üzerinden ileti gönderilebilir veya [XMPP](https://developers.google.com/cloud-messaging/ccs) (Genişletilebilir Mesajlaşma ve Durum Protokolü). İstemci uygulamalar her zaman bağlı değil çünkü ya da çalışan istemci uygulamaları için yeniden kullanılabilir olarak göndererek FCM bağlantı sunucusu kaybolmamasının ve depoları iletileri. Benzer şekilde, FCM yapmayacağınıza kuyruğa istemci uygulamasından Yukarı Akış iletileri uygulama sunucusuna uygulama sunucusu kullanılamıyor. FCM bağlantı sunucuları hakkında daha fazla bilgi için bkz. [hakkında Firebase Cloud Messaging sunucu](https://firebase.google.com/docs/cloud-messaging/server).

FCM uygulama sunucusu ve istemci uygulaması tanımlamak için aşağıdaki kimlik bilgilerini kullanır ve FCM ile ileti işlemlerini yetkilendirmek için bu kimlik bilgilerini kullanır:

-   <a name="fcm-in-action-sender-id"></a>**Gönderen Kimliği** &ndash; *Gönderen Kimliği* Firebase projenizi oluşturduğunuzda, atanan benzersiz bir sayısal değer. Gönderen Kimliği istemci uygulamasına iletileri gönderebilir her uygulama sunucusunu tanımlamak için kullanılır. Gönderen Kimliği Ayrıca, proje numarasıdır; Projenizi kaydettiğinizde Firebase konsolundan Gönderen Kimliği alın. Gönderen Kimliği örneğidir `496915549731`.

-   <a name="fcm-in-action-api-key"></a>**API anahtarı** &ndash; *API anahtarı* Firebase Hizmetleri; uygulama sunucusu erişimi verir FCM uygulama sunucunun kimliğini doğrulamak için bu anahtarı kullanır. Bu kimlik bilgileri, ayrıca olarak adlandırılır *sunucu anahtarı* veya *Web API anahtarı*. Bir API anahtarı örneğidir `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   <a name="fcm-in-action-app-id"></a>**Uygulama Kimliği** &ndash; uygulamanızın kimliği FCM iletileri almak için kaydeden istemci (belirli bir cihaz bağımsız olarak). Uygulama Kimliği örneğidir `1:415712510732:android:0e1eb7a661af2460`.

-   <a name="fcm-in-action-registration-token"></a>**Kayıt belirtecinizi** &ndash; *kayıt belirteci* (olarak da adlandırılan *örnek kimliği*) belirli bir cihaz istemcisi uygulamanıza FCM kimliğidir. Kayıt belirtecinizi çalışma zamanında oluşturulan &ndash; bir cihazda çalışırken ilk FCM ile kaydeder uygulama kayıt belirteci alır. Kayıt belirteci (Bu belirli bir cihaz üzerinde çalışan) istemci uygulamanızın bir örneğini FCM iletileri almak için yetkilendirir.
    Bir kayıt belirtecinizi örneğidir `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (çok uzun bir dize).

[Ayar yukarı Firebase Cloud Messaging](#setup_fcm) (Bu kılavuzun sonraki) bir proje oluşturmak ve bu kimlik bilgilerini oluşturmak için ayrıntılı yönergeler sağlar. Yeni bir proje oluşturduğunuzda, [Firebase konsolunda](https://console.firebase.google.com/), kimlik bilgileri dosyası adında **google-services.json** oluşturulan &ndash; içindeaçıklandığıgibibudosyayıXamarin.Androidprojenizeekleyin[ Uzak bildirimler FCM ile](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

Aşağıdaki bölümlerde, istemci uygulamalar uygulama sunucuları üzerinden FCM ile iletişim kurduğunda bu kimlik bilgilerini nasıl kullanıldığı açıklanmaktadır.


<a name="registration" />

### <a name="registration-with-fcm"></a>FCM ile kayıt

Mesajlaşma gerçekleşebilmesi için öncelikle bir istemci uygulaması ilk FCM ile kaydetmeniz gerekir. İstemci uygulaması, aşağıdaki diyagramda da görüldüğü kayıt adımlarını tamamlamanız gerekir:

[![Uygulama kayıt adımları diyagramı](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  İstemci uygulaması için FCM Gönderen Kimliği, API anahtarı ve uygulama Kimliğini geçirerek bir kayıt belirtecinizi almak için FCM ile iletişim kurar.

2.  FCM kayıt belirtecinizi istemci uygulamasına döndürür.

3.  İstemci uygulaması (isteğe bağlı olarak) kayıt belirtecinizi uygulama sunucusuna iletir.

Uygulama sunucusu, istemci uygulaması ile sonraki iletişim için kayıt belirtecinizi önbelleğe alır. Uygulama sunucusu kayıt belirtecinizi alındı belirtmek için geri istemci uygulamasına bir bildirim gönderebilirsiniz. Bu el sıkışması gerçekleştikten sonra istemci uygulaması iletileri alabileceğiniz (veya ileti göndermek) uygulama sunucusu. Eski belirteç tehlikedeyse istemci uygulaması yeni bir kayıt belirteci alabilirsiniz (bkz [FCM ile uzak bildirimler](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) nasıl bir uygulama kaydı belirteci güncelleştirmeleri alan bir örnek için).

İstemci uygulaması, uygulama sunucusundan iletileri almak artık istediği zaman, kayıt belirtecini silmek uygulama sunucusu için bir istek gönderebilirsiniz. İstemci uygulama bir CİHAZDAN kaldırılır, FCM bunu algılar ve uygulama kaydı belirtecini silmek sunucuya otomatik olarak bildirir.



### <a name="downstream-messaging"></a>Aşağı Akış Mesajlaşma

Aşağıdaki diyagramda, nasıl Firebase Cloud Messaging depolar ve aşağı akış iletilerini ileten gösterilmektedir:

[![Aşağı Akış Mesajlaşma için depola ve İlet FCM kullanır](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Uygulama sunucusu istemci uygulamasına bir aşağı akış ileti gönderdiğinde, yukarıdaki diyagramda listelenmiş olarak aşağıdaki adımları kullanır:

1.  Uygulama sunucusu için FCM iletiyi gönderir.

2.  İstemci cihaz kullanılabilir durumda değilse, FCM sunucu sonraki iletim için bir kuyruk iletisi depolar. 4 hafta en fazla depolama FCM iletileri korunur (daha fazla bilgi için [bir ileti ömrü ayarı](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  İstemci cihaz, kullanılabilir olduğunda bu cihaza istemci uygulamasında ileti FCM iletir.

4.  İstemci uygulaması FCM iletiyi alır, işler ve kullanıcıya görüntüler. Örneğin, uzak bir bildirim iletisi ise kullanıcıya bildirim alanında görüntülenir.

(Burada uygulama sunucusuna ileti gönderir tek istemci uygulamaları için), Mesajlaşma Bu senaryoda, ileti, 4 KB'lık uzunluğunda olabilir.

Android'de aşağı akış FCM iletileri alma hakkında ayrıntılı bilgi için bkz: [FCM ile uzak bildirimler](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

### <a name="topic-messaging"></a>Konu Mesajlaşma

*Konu ileti* içinde belirli bir konuya abone olanların birden fazla cihaza ileti göndermek bir uygulama sunucusu mümkün kılar. Ayrıca, oluşturma ve Firebase Konsolu bildirimleri GUI aracılığıyla konusuna ileti gönderme. Yönlendirme ve abone olunan istemciler konu iletileri teslim FCM işler. Bu özellik, hava durumu uyarıları, hisse senedi fiyatlarını ve haber başlıklarına gibi iletileri için kullanılabilir.

[![Konu Mesajlaşma diyagramı](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

(İstemci uygulama daha önce açıklandığı gibi bir kayıt belirtecinizi aldıktan sonra) konu mesajlaşmasından aşağıdaki adımlar kullanılır:

1.  İstemci uygulaması, FCM için abone ol ileti göndererek bir konuya abone olur.

2.  Uygulama sunucusu, dağıtım için FCM için konusuna iletiler gönderir.

3.  FCM konusu iletilerinde bu konuya abone olduğunuz istemcilere iletir.

Google Firebase konuyu Mesajlaşma hakkında daha fazla bilgi için bkz. [konu Mesajlaşma android'de](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Firebase Cloud Messaging ayarlama

Uygulamanızda FCM Hizmetleri kullanabilmeniz için önce yeni bir proje oluşturun (veya mevcut bir projeyi İçeri Aktar gerekir) aracılığıyla [Firebase konsolunda](https://console.firebase.google.com/). Uygulamanız için bir Firebase Cloud Messaging projesi oluşturmak için aşağıdaki adımları kullanın:

1.  Oturum [Firebase konsolunda](https://console.firebase.google.com/) tıklayın ve Google hesabı (yani, Gmail adresi) ile **yeni proje oluştur**:

    [![Yeni Proje düğme oluşturma](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Var olan bir proje varsa, tıklayın **bir Google projesini içeri**.

2.  İçinde **proje oluşturma** iletişim kutusunda, projenizin adını girin ve tıklayın **proje oluştur**. Aşağıdaki örnekte, yeni bir proje olarak adlandırılan **XamarinFCM** oluşturulur:

    [![Bir proje iletişim kutusu oluşturma](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Firebase konsolunda **genel bakış**, tıklayın **Firebase'i Android uygulamanıza ekleyin**:

    [![Android uygulamanıza Firebase ekleyin](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  Sonraki ekranda, uygulamanızın paket adı girin. Bu örnekte, paket adı olan **com.xamarin.fcmexample**. Bu değer, Android uygulamasının paket adı eşleşmelidir. Bir uygulama takma adı da girilebilir **uygulama takma ad** alan:

    [![FCM örnek app takma ad girme](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  Uygulamanızı dinamik bağlantılar, davetler veya Google kimlik doğrulaması kullanıyorsa, imzalama sertifikası, hata ayıklama da girmeniz gerekir. İmzalama sertifikanızı bulma hakkında daha fazla bilgi için bkz. [deponuzu'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md).
    Bu örnekte, imzalama sertifikası boş bırakılır.

6.  Tıklayın **uygulama Ekle**:

    [![Uygulama Ekle düğmesi](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Bir sunucu API anahtarını ve istemci kimliği için uygulama otomatik olarak oluşturulur. Bu bilgiler, paketlenmiş bir **google-services.json** tıkladığınızda otomatik olarak indirilen dosyayı **uygulama Ekle**.
    Bu dosyayı güvenli bir yere kaydettiğinizden emin olun.

Ekleme ilişkin ayrıntılı bir örnek **google-services.json** bir uygulama projesine android'de FCM anında iletme bildirimi iletileri almak için bkz: [FCM ile uzak bildirimler](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).



## <a name="for-further-reading"></a>Daha fazla bilgi için

-   Google'nın [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) Firebase Cloud Messaging'ın önemli özelliklerine genel bakış, nasıl çalıştığını ve kurulum yönergeleri bir açıklama sağlar.

-   Google'nın [uygulama sunucusu gönderme isteklerinin derleme](https://firebase.google.com/docs/cloud-messaging/send-message) ile uygulama sunucunuzdaki ileti göndermek nasıl açıklar.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) ve [RFC 6121](https://tools.ietf.org/html/rfc6121) açıklamak ve Genişletilebilir Mesajlaşma ve iletişim durumu Protokolü (XMPP) tanımlayın.

-   [FCM iletileri hakkında](https://firebase.google.com/docs/cloud-messaging/concept-options) gönderilebilecek ileti Firebase Cloud Messaging ile farklı türleri açıklanmaktadır.

## <a name="summary"></a>Özet

Bu makalede, Firebase Cloud Messaging (FCM), genel bakış sağlanır. Bu, tanımlamak ve uygulama sunucuları ve istemci uygulamalar arasında ileti yetkilendirmek için kullanılan çeşitli kimlik bilgilerini açıklanmıştır. Kayıt ve aşağı akış Mesajlaşma senaryoları gösterilen ve FCM kullanmanıza FCM ile uygulamanızı kaydetmek için adımları ayrıntılı.


## <a name="related-links"></a>İlgili bağlantılar

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
