---
title: Google Cloud Messaging'i
description: Google Cloud Messaging (GCM), mobil uygulamaları ve sunucu uygulamaları Mesajlaşma kolaylaştıran bir hizmettir. Bu makalede GCM nasıl çalıştığını genel bir bakış sağlar ve uygulamanızı GCM kullanabilmeniz için Google hizmetlerin nasıl yapılandırılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: 29cccf414759a79a8ba74dfc35b7ba9f6a1cc5d6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044749"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging'i

_Google Cloud Messaging (GCM), mobil uygulamaları ve sunucu uygulamaları Mesajlaşma kolaylaştıran bir hizmettir. Bu makalede GCM nasıl çalıştığını genel bir bakış sağlar ve uygulamanızı GCM kullanabilmeniz için Google hizmetlerin nasıl yapılandırılacağı açıklanmaktadır._

[![Google Cloud Messaging logosu](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

Bu konu, uygulamanızı ve uygulama sunucusu arasında iletileri Google Cloud Messaging nasıl yönlendiren bir üst düzey genel bakış sağlar ve uygulamanızı GCM hizmetleri kullanabilmesi için kimlik bilgileri alınırken için adım adım bir yordam sağlar.

> [!NOTE]
> GCM kılınan tarafından [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM sunucu ve istemci API'leri [kullanım dışı bırakılmış](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) ve artık 11 Nisan 2019 olan en kısa sürede kullanılabilir olacaktır.

## <a name="overview"></a>Genel Bakış

Google Cloud Messaging (GCM) gönderme, Yönlendirme ve sunucu uygulamaları ve mobil istemci uygulamalar arasında iletilerinin çağrısı işleyen bir hizmettir. A *istemci uygulaması* bir cihazda çalışan GCM özellikli bir uygulama olur. *Uygulama sunucusu* (siz veya şirketiniz tarafından sağlanan) istemci uygulamanızı GCM iletişim kuran GCM etkin sunucu:

[![İstemci uygulaması ve uygulama sunucusu arasında GCM bulunan](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

GCM kullanarak, uygulama sunucuları, tek bir aygıt, aygıtlar bir grup veya bir konuya abone olduğunuz cihaz sayısı iletileri gönderebilir. İstemci uygulamanızı GCM (örneğin, uzak bildirimleri almak için) bir uygulama sunucudan aşağı akış iletileri abone olmak için kullanabilirsiniz. Ayrıca, GCM uygulama sunucusuna geri Yukarı Akış iletileri göndermek istemci uygulamaları için mümkün kılar.

Bir uygulama sunucusu için GCM uygulama hakkında daha fazla bilgi için bkz: [hakkında GCM bağlantı sunucusu](https://developers.google.com/cloud-messaging/server).



## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging'i eylemi

Aşağı Akış iletileri istemci uygulamaları için bir uygulama sunucusundan gönderildiğinde, uygulama sunucusu iletisi gönderir. bir *GCM bağlantı sunucusu*; GCM bağlantı sunucusu istemci uygulamanızı çalıştıran bir cihaza iletiyi sırayla iletir. İletileri HTTP üzerinden gönderilebilir veya [XMPP](https://developers.google.com/cloud-messaging/ccs) (Genişletilebilir Mesajlaşma ve varlığı Protokolü). İstemci uygulamaları her zaman bağlı olduğundan veya çalıştıran, yeniden bağlanın ve kullanılabilir hale gibi istemci uygulamalarını göndererek GCM bağlantı sunucu enqueues ve depoları iletileri. Benzer şekilde, GCM olacak enqueue istemci uygulamasından Yukarı Akış iletileri uygulama sunucusuna uygulama sunucusu kullanılamıyorsa.

GCM uygulama sunucusu ve istemci uygulamanızı tanımlamak için aşağıdaki kimlik bilgilerini kullanır ve ileti işlemleri GCM aracılığıyla yetkilendirmek için bu kimlik bilgilerini kullanır:

-   **API anahtarı** &ndash; *API anahtarı* Google Hizmetleri; uygulama sunucusuna erişimi sağlar GCM uygulama sunucunuzun kimliğini doğrulamak için bu anahtarı kullanır.
    GCM hizmeti kullanabilmek için önce bir API anahtarı edinmeniz gerekir [Google Developer konsolunda](https://console.developers.google.com/) oluşturarak bir *proje*. API anahtarını güvenli tutulması; API anahtarınızı koruma hakkında daha fazla bilgi için bkz: [en iyi uygulamalar API anahtarlarını güvenli bir şekilde kullanmak için](https://support.google.com/cloud/answer/6310037?hl=en).

-   **Gönderen Kimliği** &ndash; *Gönderen Kimliği* istemci uygulamanızı uygulama sunucusuna yetkilendirir &ndash; , istemci uygulamanızı iletileri göndermek için izin verilen uygulama sunucusunu tanımlayan benzersiz bir sayıdır.
    Gönderen Kimliği, ayrıca, proje numarası olan; Projenizi kaydettiğinizde Google geliştiriciler konsolundan Gönderen Kimliği alın.

-   **Kayıt belirtecinizi** &ndash; *kayıt belirteci* istemci uygulamanızı belirli bir aygıttaki GCM kimliğidir. Kayıt belirtecinizi çalışma zamanında oluşturulan &ndash; bir cihazda çalıştığı sırada ilk GCM'ye kaydedilir, uygulamanızı bir kayıt belirtecini alır. Kayıt belirteci (belirli bu cihazda çalışan) istemci uygulamanızı örneği GCM'den iletileri almak için yetkilendirir.

-   **Uygulama Kimliği** &ndash; kimliğini GCM'den iletileri almak için kaydeden istemci uygulamanızın (herhangi belirli bir aygıt bağımsız olarak). Android, uygulama kimliği kaydedilen paket adıdır **AndroidManifest.xml**, gibi `com.xamarin.gcmexample`.

[Ayar yukarı Google Cloud Messaging](#settingup) (Bu kılavuzun sonraki) bir proje oluşturma ve bu kimlik bilgileri oluşturmak için ayrıntılı yönergeler sağlar.

Aşağıdaki bölümlerde istemci uygulamaları GCM aracılığıyla uygulama sunucuları ile iletişim kurduğunda bu kimlik bilgilerini nasıl kullanıldığı açıklanmaktadır.



### <a name="registration-with-gcm"></a>GCM ile kayıt

Mesajlaşma gerçekleşebilmesi için öncelikle bir cihazda yüklü bir istemci uygulaması önce GCM ile kaydetmeniz gerekir. İstemci uygulaması, aşağıdaki çizimde gösterilen kayıt adımları tamamlamanız gerekir:

[![Uygulama kayıt adımları](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  İstemci uygulaması için GCM Gönderen Kimliği geçirme bir kayıt belirtecinizi almak için GCM ile iletişim kurar.

2.  GCM kayıt belirtecinizi istemci uygulamasına döndürür.

3.  İstemci uygulama kaydı belirteci uygulama sunucusuna iletir.

Uygulama sunucusu, istemci uygulaması ile sonraki iletişim için kayıt belirtecinizi önbelleğe alır. İsteğe bağlı olarak, uygulama sunucusu kayıt belirtecinizi alındı göstermek için istemci uygulamaya tekrar bir bildirim gönderebilirsiniz. Bu anlaşma gerçekleştikten sonra istemci uygulaması iletilerden alabileceğiniz (veya ileti göndermek) uygulama sunucusu.

İstemci uygulaması artık uygulama sunucusundan iletileri almak istediğinde kayıt belirtecini silmek için uygulama sunucusuna bir istek gönderebilirsiniz. İstemci uygulaması (Bu makalenin sonraki bölümlerinde açıklanmıştır) konu iletileri alıyorsa konusundan aboneliğinizi iptal edebilirsiniz.
İstemci uygulaması bir aygıttan kaldırılırsa, GCM bu algılar ve uygulama kayıt belirtecini silmek sunucuya otomatik olarak bildirir.

Google [kaydetme istemci uygulamaları](https://developers.google.com/cloud-messaging/registration) daha ayrıntılı; kayıt işlemini açıklar kayıt silme ve aboneliği kaldırma açıklar ve bir istemci uygulama kaldırıldığında kayıt silme işlemini açıklar.



### <a name="downstream-messaging"></a>Aşağı Akış Mesajlaşma

Uygulama sunucusu istemci uygulamasına bir aşağı akış ileti gönderdiğinde, aşağıdaki çizimde gösterilen adımları izler:

[![Aşağı Akış Mesajlaşma deposu ve iletme diyagramı](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  Uygulama sunucusu için GCM iletisi gönderir.

2.  İstemci aygıtı kullanılabilir durumda değilse, GCM server ileti sırasındaki sonraki iletim için depolar.

3.  İstemci aygıtı kullanılabilir olduğunda, GCM bu cihaza istemci uygulamanın iletiyi gönderir.

4.  İstemci uygulama GCM'den iletiyi alır ve buna göre işler. Örneğin, uzak bir bildirim iletisiyse kullanıcıya sunulur.

(Burada uygulama sunucusuna ileti gönderir tek istemci uygulamaları için) Bu senaryoda ileti, ileti uzunluğu en fazla 4kB olabilir.

Android aşağı akış GCM iletileri alma hakkında (kod örnekleri dahil) ayrıntılı bilgi için bkz: [uzak bildirimler](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).


#### <a name="topic-messaging"></a>Konu Mesajlaşma

*Konu ileti* aşağı akış Mesajlaşma burada uygulama sunucusunun tek ileti gönderir (örneğin, bir hava tahmini) bir konuya abone birden çok istemci uygulamasını cihazlarına türüdür. Konu iletileri uzunluğu en çok 2 KB olabilir ve uygulama başına en fazla bir milyon abonelik konu Mesajlaşma destekler. GCM Mesajlaşma konu için kullanılıyorsa, istemci uygulama kaydı belirteci uygulama sunucusuna göndermek için gerekli değildir. Google [uygulama konu ileti](https://developers.google.com/cloud-messaging/topic-messaging) belirli bir konuya abone birden çok aygıt için bir uygulama sunucusundan iletileri göndermeye açıklanmaktadır.



#### <a name="group-messaging"></a>Grup Mesajlaşma

*Grup ileti* aşağı akış Mesajlaşma burada uygulama sunucusunun tek ileti gönderir (örneğin, tek bir kullanıcıya ait cihazların bir grubu) bir gruba ait birden çok istemci uygulamasını cihazlarına türüdür. Grup iletileri iOS cihazları için uzunluğu en çok 2KB olabilir ve Android cihazlar için uzunluğu 4KB'a kadar. En fazla 20 üyeleri için bir grup sınırlıdır. Google [Grup Device Messaging](https://developers.google.com/cloud-messaging/notifications) uygulama sunucuları bir gruba ait cihazlarda çalışan birden çok istemci uygulama örneği için tek bir ileti nasıl gönderebilir açıklar.


### <a name="upstream-messaging"></a>Yukarı Akış Mesajlaşma

İstemci uygulamanızı destekleyen bir sunucuya bağlanır, [XMPP](https://developers.google.com/cloud-messaging/ccs), onu iletileri uygulama sunucusuna aşağıdaki çizimde gösterildiği gibi gönderebilirsiniz:

[![Yukarı Akış Mesajlaşma diyagramı](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  İstemci uygulaması GCM XMPP bağlantı sunucusuna bir ileti gönderir.

2.  Uygulama sunucusu bağlantısı kesilirse, GCM server iletiyi daha sonra iletmek için bir sıra depolar.

3.  Uygulama sunucusu yeniden bağlı olduğunda, GCM iletiyi uygulama sunucusuna iletir.

4.  Uygulama sunucusu istemci uygulamanın kimliğini doğrulamak için ileti ayrıştırır ve ardından "ack" iletisi giriş onaylamak için GCM'ye gönderir.

5.  Uygulama sunucusu yapılacak işler.

Google [Yukarı Akış iletileri](https://developers.google.com/cloud-messaging/ccs#upstream) JSON olarak kodlanmış iletileri yapısı ve Google XMPP tabanlı bulut bağlantı sunucusu çalıştıran uygulama sunucuları için Gönder açıklanmaktadır.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Google Cloud Messaging'i ayarlama

Uygulamanızda GCM Hizmetleri kullanabilmeniz için önce Google GCM sunuculara erişim için kimlik bilgilerini edinmeniz gerekir. Aşağıdaki bölümlerde bu işlemi tamamlamak için gereken adımlar açıklanmaktadır:



### <a name="enable-google-services-for-your-app"></a>Google Hizmetleri için uygulamanızı etkinleştirme

1.  Oturum [Google geliştiriciler konsol](https://developers.google.com/mobile/add?platform=android) (yani, gmail adresinizi) ile Google hesabı ve yeni bir proje oluşturun. Varolan projeyi varsa, GCM etkin olmasını istediğiniz projesini seçin. Aşağıdaki örnekte, yeni bir proje adı verilen **XamarinGCM** oluşturulur:

    [![XamarinGCM projesi oluşturma](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  Ardından, uygulamanız için paket adını girin (Bu örnekte, paket adı olan **com.xamarin.gcmexample**) tıklatıp **Seç devam et ve Hizmetleri Yapılandırma**:

    [![Paket adı girme](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    Bu paket adı, uygulamanız için uygulama kimliği olduğuna dikkat edin.

3.  **Seçin ve Hizmetleri Yapılandırma** bölümü uygulamanıza eklediğiniz Google hizmetleri listeler. Tıklatın **bulut Mesajlaşma**:

    [![Bulut seçin Mesajlaşma](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  Bundan sonra öğesini **etkinleştirmek GOOGLE CLOUD MESSAGING**:

    [![Google Cloud Messaging'i etkinleştirme](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A **Server API anahtarını** ve **Gönderen Kimliği** uygulamanız için oluşturulur. Bu değerleri kaydedin ve tıklatın **Kapat**:

    [![Sunucu API anahtarı ve görüntülenen gönderen Kimliği](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    API anahtarını korumak &ndash; ortak kullanılmak üzere tasarlanmamıştır. API anahtarını aşılıp aşılmadığını yetkisiz sunucuları iletileri istemci uygulamalar yayımlayabilirsiniz.
    [En iyi uygulamalar API anahtarlarını güvenli bir şekilde kullanmak için](https://support.google.com/cloud/answer/6310037?hl=en) API anahtarınızı korumak için yararlı bir kılavuz bilgiler verilmektedir.



### <a name="view-your-project-settings"></a>Proje ayarlarınızı görüntüleyin

İçine oturum açarak proje ayarlarınızı istediğiniz zaman görüntüleyebilirsiniz [Google Cloud Console](https://console.cloud.google.com/) ve projenizin seçme. Örneğin, görüntüleyebileceğiniz **Gönderen Kimliği** menüsünü çekme sayfanın üst kısmındaki içinde projenizi seçerek (Bu örnekte, proje denir **XamarinGCM**). Bu ekran görüntüsünde gösterildiği gibi proje numarası gönderen kimliğidir (Gönderen Kimliği burada **9349932736**):

[![Gönderen Kimliği görüntüleme](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

Görüntülemek için **API anahtarı**, tıklatın **API Yöneticisi** ve ardından **kimlik bilgileri**:

[![API anahtarını görüntüleme](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>Daha Fazla Bilgi İçin

-   Google [kaydetme istemci uygulamaları](https://developers.google.com/cloud-messaging/registration) daha fazla ayrıntı, istemci kayıt işlemini açıklar ve otomatik yeniden yapılandırma ve kayıt durumu eşitlenmiş şekilde kalmasının hakkında bilgi sağlar.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) ve [RFC 6121](https://tools.ietf.org/html/rfc6121) açıklayabilir ve Genişletilebilir ileti ve iletişim durumu Protokolü (XMPP) tanımlayın.



## <a name="summary"></a>Özet

Bu makalede, Google Cloud Messaging (GCM), genel bakış sağlanır. Tanımlamak ve uygulama sunucuları ve istemci uygulamalar arasında Mesajlaşma yetkilendirmek için kullanılan çeşitli kimlik bilgilerini açıklanmıştır. En yaygın ileti senaryolar gösterilmiştir ve GCM Hizmetleri'ni GCM ile uygulamanızı kaydetme adımları ayrıntılı.


## <a name="related-links"></a>İlgili bağlantılar

- [Mesajlaşma bulut](https://developers.google.com/cloud-messaging/)
