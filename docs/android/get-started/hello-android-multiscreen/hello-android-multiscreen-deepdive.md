---
title: "Merhaba, Android Multiscreen: Derinlemesine bakış"
description: "Bu iki parçalı Kılavuzu'nda (Merhaba, Android Kılavuzu oluşturulan) temel Phoneword uygulama ikinci ekranı işlemek için genişletilir. Yol boyunca temel Android uygulama yapı taşları sunulur. Derin Dalış Android mimarisi içine daha iyi anlamasına yardımcı Android uygulaması yapısı ve işlevleri geliştirmenize yardımcı olması için dahil edilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD3BAE9A-963C-4CF7-9733-111033034289
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a47dea43b1fb1e84a0cd3dffc07b483497edbe09
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="hello-android-multiscreen-deep-dive"></a>Merhaba, Android Multiscreen: Derinlemesine bakış

_Bu iki parçalı Kılavuzu'nda (Merhaba, Android Kılavuzu oluşturulan) temel Phoneword uygulama ikinci ekranı işlemek için genişletilir. Yol boyunca temel Android uygulama yapı taşları sunulur. Derin Dalış Android mimarisi içine daha iyi anlamasına yardımcı Android uygulaması yapısı ve işlevleri geliştirmenize yardımcı olması için dahil edilmiştir._

## <a name="hello-android-multiscreen-deep-dive"></a>Merhaba, Android Multiscreen derinlemesine bakış

İçinde [Hello, Android Multiscreen Hızlı Başlangıç](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), yerleşik ve ilk çok ekran Xamarin.Android uygulamanız çalıştırılmıştır.
Şimdi daha karmaşık uygulamaları oluşturabilmeleri Android gezinti ve mimari daha derin bir anlayış geliştirmek için zaman yapılır.

Keşfedin bu kılavuzda daha gelişmiş Android olarak Android mimarisi *uygulama yapı taşları* sunulur. Android gezinme *hedefleri* seçenekleri incelediniz anlatıldığı ve Android donanım Gezinti bölmesinde bulunuyor. Daha fazla ilişkin bütüncül bir işletim sistemi ve diğer uygulamalar uygulamanın ilişkisi geliştirdikçe Phoneword uygulama kümeye yeni eklenecekleri dissected.


## <a name="android-architecture-basics"></a>Android mimarisi temelleri

İçinde [Hello, Android derinlemesine](~/android/get-started/hello-android/hello-android-deepdive.md), tek giriş noktası bulunmadığından Android uygulamaları benzersiz programlar olduğunu öğrendiniz. Bunun yerine, işletim sistemi (veya başka bir uygulama) sırayla uygulama için işleminin başlatan herhangi bir uygulamanın kayıtlı etkinliklerinin başlatır. Bu derinlemesine Android mimarisi içine, Android uygulama yapı taşları ve bunların işlevlerini sunarak nasıl Android uygulamaları oluşturulan anlama genişletir.

<a name="AndroidApplicationBlocks" />

### <a name="android-application-blocks"></a>Android uygulaması blokları

Bir Android uygulaması olarak adlandırılan özel Android sınıfların koleksiyonu oluşur *uygulama blokları* uygulama kaynaklar - görüntüleri, temalar, yardımcı sınıfları, vb. herhangi bir sayıda birlikte paketlenebilir &ndash; bunlar tarafından düzenlenir bir XML dosyası olarak adlandırılan *Android derleme bildirimi*.

Normal bir sınıfla normalde gerçekleştirmek uygulanamadı şeyler izin verdiğinden uygulama blokları Android uygulamaları omurga oluşturur. Olanlar dahil en önemli iki _etkinlikleri_ ve _Hizmetleri_:

-   **Etkinlik** &ndash; bir etkinlik karşılık gelen bir kullanıcı arabirimi ile bir ekrana ve kavramsal olarak bir web uygulaması bir web sayfasında benzer. Örneğin, bir haber uygulamasında oturum açma ekranı ilk etkinlik olacaktır, haber öğeleri kaydırılabilir listesi başka bir etkinlik olabilir ve her öğe için Ayrıntılar sayfası üçüncü durumda olurdu. Etkinlikleri hakkında daha fazla bilgiyi [etkinlik yaşam döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md) Kılavuzu.

-   **Hizmet** &ndash; Android Hizmetleri uzun süre çalışan görevler alma ve arka planda çalışan tarafından etkinlikleri destekler. Hizmetleri kullanıcı arabirimi yoktur ve ekranlara bağlı olmayan görevler işlemek için kullanılan &ndash; Örneğin, arka planda tıklatın veya fotoğraf bir sunucuya yükleme. Hizmetler hakkında daha fazla bilgi için bkz: [Oluşturma Hizmetleri](~/android/app-fundamentals/services/index.md) ve [Android Hizmetleri](~/android/app-fundamentals/services/index.md) kılavuzları.


Bir Android uygulamasını blokları tüm türleri kullanamaz ve genellikle bir tür birkaç blokları sahiptir. Örneğin, Phoneword uygulamasından [Hello, Android Hızlı Başlangıç](~/android/get-started/hello-android/hello-android-quickstart.md) yalnızca bir etkinlik (ekran) ve bazı kaynak dosyaları oluşan oluştu. Bir basit müzik çalar uygulaması çeşitli etkinlikleri ve bir hizmet, uygulama arka planda olduğunda müzik yürütmek için sahip olabilir.

### <a name="intents"></a>Hedefleri

Android uygulamalarında başka bir temel kavram *hedefi*.
Android çevresinde tasarlanmıştır *en az ayrıcalık prensibi* &ndash; çalışmaya duyarlar bloklarına ve sınırlı işletim sistemini veya diğer oluşturan blokları erişime yalnızca uygulama erişimi uygulamalar. Benzer şekilde, taşlarıdır *birbirine sıkı şekilde bağlı* &ndash; çok az bilgiye sahip ve diğer blokları (aynı uygulamanın parçası olan bile engeller) sınırlı erişim sağlamak için tasarlanmıştır.

İletişim kurmak için uygulama blokları adlı zaman uyumsuz ileti göndermek *hedefleri* geri ve İleri. Hedefleri alıcı blok ve bazen bazı veriler hakkındaki bilgileri içerir. Amacına iki uygulama bileşenleri bağlama ve iletişim kurmalarına izin vererek gönderilen başka bir uygulama bileşeni olmasını bir şey bir uygulama bileşeni tetikleyicilerden. Hedefleri ve geriye göndererek almak ve kaydetmek için kamera uygulamasını başlatarak, konum bilgilerini toplama veya sonraki bir ekranından gezinme gibi karmaşık işlemleri koordine etmek için blokları elde edebilirsiniz.

<a name="AndroidManifestXML" />

### <a name="androidmanifestxml"></a>AndroidManifest.XML

Uygulamaya bir blok eklediğinizde, adlı özel bir XML dosyası ile kayıtlı olduğu **Android derleme bildirimi**. Bildirim, uygulamanın yanı sıra sürüm gereksinimleri, izinleri ve bağlantılı kitaplıklar tüm uygulama blokları izler &ndash; işletim sistemini çalıştırmak, uygulamanız için bilmeniz gereken her şeyi. **Android derleme bildirimi** etkinlikleri ve hangi eylemleri belirli bir etkinlik için uygun olduğunu denetlemek için amaçları ile de çalışır. Bu Android bildirim özelliklerini Gelişmiş ele alınmaktadır [Android derleme bildirimi ile çalışma](~/android/platform/android-manifest.md) Kılavuzu.

Phoneword uygulama, yalnızca bir etkinlik, bir hedefi tek ekran sürümünde ve `AndroidManifest.xml` simgeler gibi ek kaynaklar yanında kullanıldı. Phoneword çok ekran sürümünde, ek bir etkinlik eklendi; Amacına kullanarak ilk etkinliğinden başlatıldı. Sonraki bölümde hedefleri Gezinti Android uygulamaları oluşturmak için nasıl yardımcı araştırır.

## <a name="android-navigation"></a>Android gezinme

Hedefleri ekranları arasında gezinmek için kullanılmıştır. Bu kodu nasıl hedefleri çalışma ve Android Gezinti içindeki rollerine anlamak görmek için ıntune'un dalmaya geldi.


### <a name="launching-a-second-activity-with-an-intent"></a>Amacına sahip ikinci bir etkinlik başlatma

Phoneword uygulamada amacına ikinci bir ekran (etkinlik) başlatmak için kullanıldı. Başlangıç geçerli geçirme amacına, oluşturarak *bağlamı* (`this`geçerli başvuran **bağlamı**) ve aradığınız uygulama blok türü (`TranslationHistoryActivity`):

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

**Bağlamı** uygulama ortamı hakkında genel bilgi için bir arabirim &ndash; yeni oluşturulan nesneler uygulama ile neler olduğunu bilmeniz olanak sağlar. İleti olarak bir hedefinin düşünüyorsanız, ileti alıcısı adını sağlanmaktadır (`TranslationHistoryActivity`) ve alıcının adresi (`Context`).

Android için amacına basit veri eklemek için bir seçenek sunar (karmaşık veri farklı işlenir). Phoneword örnekte `PutStringArrayExtra` hedefi için bir telefon numarası listesinin eklemek için kullanılır ve `StartActivity` hedefi alıcıda olarak adlandırılır. Tamamlanan kodu şöyle görünür:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```


## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramlar

Phoneword uygulama bu kılavuzda ele alınmamaktadır birkaç kavramları sundu. Bu kavramları şunlardır:

**Dize Kaynakları** &ndash; içinde Phoneword uygulama, metnin `TranslationHistoryButton` ayarlandı `"@string/translationHistory"`. `@string` Sözdizimi anlamına gelir dizenin değer depolanır _dize kaynakları dosya_, **Strings.xml**. Şu değer `translationHistory` dize eklendiği **Strings.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

Dize kaynaklarını ve Android kaynaklar hakkında daha fazla bilgi için bkz [Android kaynaklar kılavuz](~/android/app-fundamentals/resources-in-android/index.md).

**ListView ve ArrayAdapter** &ndash; A _ListView_ kaydırma satır listesini sunmak için basit bir yöntem sunar UI bileşendir. A `ListView` örneği gerektirir bir _bağdaştırıcısı_ satır görünümlerinde yer alan verilerle akışına. Aşağıdaki kod satırını kullanıcı arabiriminin doldurmak için kullanılan `TranslationHistoryActivity`:

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

ListViews ve bağdaştırıcıları bu belgenin kapsamı dışındadır, ancak çok kapsamlı ele alınmıştır [ListViews ve bağdaştırıcıları](~/android/user-interface/layouts/list-view/index.md) Kılavuzu.
[ListView veri ile doldurma](~/android/user-interface/layouts/list-view/populating.md) yerleşik özellikle kullanarak ile ilgilenir `ListActivity` ve `ArrayAdapter` sınıfları oluşturmak ve doldurmak için bir `ListView` özel bir düzen tanımlamadan olarak yapıldı Phoneword örnekte.


## <a name="summary"></a>Özet

Tebrikler, ilk çok ekranın Android uygulaması tamamladınız. Bu kılavuzda sunulan *Android uygulama yapı taşları* ve *hedefleri* bunları çok filtrelenmiş bir Android uygulaması oluşturmak için kullanılır. Artık kendi Xamarin.Android uygulamaları geliştirmeye başlamak için gereken zemine sahipsiniz.

Ardından, içinde xamarin'le platformlar arası uygulamalar oluşturmak öğreneceksiniz [platformlar arası uygulamalar oluşturma sırasında size kılavuzluk eder](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
