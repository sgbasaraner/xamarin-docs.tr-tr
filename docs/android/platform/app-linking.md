---
title: "Android, uygulama olarak bağlama"
description: "Bu kılavuz, app-bağlama, Web sitelerinde URL'lere yanıtlamak mobil uygulamalar sağlayan bir teknik Android 6.0 nasıl desteklediği ele alınacaktır. Uygulama bağlama ne olduğu, nasıl bir Android 6.0 uygulamada uygulama bağlama uygulanacağını ve bir etki alanı için mobil uygulama izinleri vermek için bir Web sitesi yapılandırma ele alınacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: DDE54082-6E2B-9ED9-05FB-D9C1D1B1258E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 726890e48407dd26f52c5aeaecf4eab51dcc5182
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="app-linking-in-android"></a>Android, uygulama olarak bağlama

_Bu kılavuz, app-bağlama, Web sitelerinde URL'lere yanıtlamak mobil uygulamalar sağlayan bir teknik Android 6.0 nasıl desteklediği ele alınacaktır. Uygulama bağlama ne olduğu, nasıl bir Android 6.0 uygulamada uygulama bağlama uygulanacağını ve bir etki alanı için mobil uygulama izinleri vermek için bir Web sitesi yapılandırma ele alınacaktır._

## <a name="app-linking-overview"></a>Uygulama bağlama genel bakış

Mobil uygulamalar artık canlı bir silo &ndash; çoğu durumda bunlar bir önemli kendi Web sitesi birlikte işlerini bileşenleridir. Mobil uygulamalar başlatarak ve ilgili içeriği mobil uygulamaya görüntüleme bir Web sitesinde bağlantılarla kendi web sitenizin ve mobil uygulamalar, sorunsuz bir şekilde bağlanmak işletmeler için iyi bir şeydir. *Uygulama bağlama* (olarak da adlandırılan *derin bağlama*) bir URI ile yanıt verir ve bu URI karşılık gelen bir mobil uygulama başlatmak bir mobil cihaz izin veren bir tekniktir.

Android işleme uygulama bağlama aracılığıyla *hedefi sistem* &ndash; kullanıcı bir mobil tarayıcı bir bağlantıyı tıklattığında, mobil tarayıcı Android kayıtlı bir uygulama temsil edecek bir hedefi gönderme. Örneğin, bir pişirme Web sitesinde bir bağlantıyı tıklatarak bu Web sitesi ile ilişkilendirilmiş bir mobil uygulama açın ve belirli tarif kullanıcıya görüntüleyin. Varsa bu amacı işlemek için birden fazla uygulama kayıtlı sonra Android Yükselt olarak bilinen bir *Kesinleştirme iletişim* , sorar kullanıcı amacı için işlemesi gereken uygulama seçmek için hangi uygulama Örnek:

![Kesinleştirme iletişim örnek ekran görüntüsü](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 bu otomatik bağlantı işleme kullanarak artırır. Otomatik olarak bir uygulama için bir URI varsayılan işleyici olarak kaydetmek Android mümkündür &ndash; uygulama otomatik olarak başlatmak ve doğrudan ilgili etkinlik gidin. Android 6.0 URI tıklatın işlemek nasıl karar aşağıdaki ölçütlere bağlıdır:

1. **Mevcut bir uygulamayı zaten URI'sı ile ilişkilendirilen** &ndash; kullanıcı zaten var olan bir uygulamaya bir URI'sı ile ilişkili. Bu durumda, Android, bu uygulamayı kullanmaya devam eder.
2. **Var olan bir uygulamayı URI'sı ile ilgili değildir, ancak destekleyen bir uygulama yüklü** &ndash; Android yüklü destekleyen uygulama isteği işlemek için kullanılacak şekilde bu senaryoda, kullanıcı var olan bir uygulamaya belirtilmedi.
3. **Var olan bir uygulamayı URI'sı ile ilgili değildir, ancak birçok destekleyen uygulamalar yüklü** &ndash; URI destekleyen birden çok uygulama olduğundan, kesinleştirme iletişim kutusu görüntülenir ve kullanıcının hangi uygulamanın olacak seçmeniz gerekir URI işleyin.

URI destekleyen hiçbir uygulamaları kullanıcı varsa ve daha sonra yüklüyse, ardından Android uygulama varsayılan işleyici olarak URI'sini URI'sı ile ilişkili Web sitesi ile ilişkilendirme doğruladıktan sonra ayarlar.

Bu kılavuz, bir Android 6.0 uygulamasının nasıl yapılandırılacağını ve oluşturma ve yayımlama Android 6. 0'uygulama bağlama desteklemek için dijital varlık bağlantılar dosyası ele alınacaktır.

## <a name="requirements"></a>Gereksinimler

Bu kılavuz Xamarin.Android 6.1 ve Android 6.0 (API düzeyi 23) hedefleyen uygulama gerektirir ya da daha yüksek.

Uygulama bağlama Android önceki sürümlerde mümkün kullanarak [Rivets NuGet paketi](https://www.nuget.org/packages/Rivets/) Xamarin bileşeni deposundan. Perçinler paket uyumlu olmayan uygulama bağlama Android 6.0; ile Android 6.0 uygulama bağlama desteklemez.

## <a name="configuring-app-linking-in-android-60"></a>Uygulama bağlama Android 6. 0'yapılandırma

App-Android 6.0 bağlantılar kurma iki ana adımdan oluşur:

1. **Bir veya daha fazla amacı filtreleri Web sitesi URI için ekleme** &ndash; hedefi filtreleri bir mobil tarayıcı bir URL tıklatın nasıl ele alınacağını Android kılavuzunda.
2. **Yayımlama bir *dijital varlık bağlantıları JSON* Web sitesinde dosya** &ndash; bu Web sitesine yüklenir ve Android tarafından mobil uygulama ve Web sitesinin etki alanı arasındaki ilişkiyi doğrulamak için kullanılan bir dosyadır. Bu, Android uygulama varsayılan işleyici URI'si için yükleyemezsiniz; kullanıcı bunu el ile yapmanız gerekir.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Hedefi filtresi yapılandırma

Bir Android uygulamasını bir etkinlikte bir Web sitesinden bir URI (veya olası bir dizi URI'ler) eşleşen bir hedefi filtresi yapılandırmak gereklidir. Xamarin.Android içinde bir etkinlik eklemek tarafından bu ilişki kurulur [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Hedefi filtre aşağıdaki bilgileri bildirmeniz gerekir:

* **`Intent.ActionView`** &ndash; Bu bilgileri görüntülemek için isteklerini yanıtlamasını hedefi filtre kaydeder
* **`Categories`** &ndash;  Hedefi filtre hem kaydedeceğini  **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)**  ve  **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)**  düzgün kaldırabilmek için URI web işleyin.
* **`DataScheme`** &ndash; Hedefi filtre bildirmelidir `http` ve/veya `https`. Bu, yalnızca iki geçerli şemaları geçerlidir.
* **`DataHost`** &ndash; Bu URI kaynaklanan etki alanıdır.
* **`DataPathPrefix`** &ndash; Bu Web sitesinde kaynakları isteğe bağlı bir yoludur.
* **`AutoVerify`** &ndash; `autoVerify` Özniteliği Android uygulaması ve Web sitesi arasındaki ilişkiyi doğrulamak için söyler. Bu daha aşağıda açıklanmıştır.

Aşağıdaki örnekte nasıl kullanılacağını gösterir [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) bağlantılarından işlemek için `https://www.recipe-app.com/recipes` ve `http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe"),
              AutoVerify=true]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android uygulama varsayılan işleyici olarak için URI kaydetmeden önce tanımlanan dijital varlıklar dosyasını Web sitesinde karşı hedefi filtreler tarafından her ana bilgisayarın doğrular. Android uygulama varsayılan işleyici olarak kurmadan önce tüm hedefi filtreleri doğrulama geçmesi gerekir.

### <a name="creating-the-digital-assets-link-file"></a>Dijital varlıklar bağlantı dosyası oluşturma

Uygulama bağlama android 6.0 Android uygulaması için URI varsayılan işleyici olarak ayarlamadan önce uygulama ve Web sitesi arasındaki ilişkiyi doğrulamanız gerekiyor. Uygulamayı ilk kez yüklendiğinde bu doğrulamayı meydana gelir. *Dijital varlıklar bağlantılar* ilgili webdomain(s) tarafından barındırılan bir JSON dosyası bir dosyadır.

> [!NOTE]
> **Not:** `android:autoVerify` hedefi Filtresi tarafından özniteliği ayarlanmalıdır &ndash; aksi Android doğrulaması gerçekleştirmez.

Dosya konumunda etki alanı yayımlanması tarafından yerleştirilen **https://domain/.well-known/assetlinks.json**.

Dijital varlık dosya ilişkilendirme doğrulamak Android için meta veri gerekli içerir. Bir **assetlinks.json** dosyası aşağıdaki anahtar-değer çiftleri sahiptir:

* `namespace` &ndash; Android uygulama ad alanı.
* `package_name` &ndash; (uygulama bildirimde belirtilen) Android uygulama paketi adı.
* `sha256_cert_fingerprints` &ndash; İmzalı uygulama SHA256 parmak izi. Lütfen kılavuzuna bakın [Keystore'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md) SHA1 parmak izi uygulamanın edinme hakkında daha fazla bilgi için.

Aşağıdaki kod parçacığında örneğidir **assetlinks.json** ile tek bir uygulama listelenen:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

Farklı sürümleri desteklemek için birden fazla SHA256 parmak izi kaydedilebilir veya uygulamanızın veya oluşturur. Bu sonraki **assetlinks.json** dosyası birden çok uygulama kaydı bir örnek verilmiştir:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

[Google dijital varlık bağlantılar Web sitesi](https://developers.google.com/digital-asset-links/tools/generator) oluşturma ve dijital varlıklar dosyayı test ile yardımcı olabilir bir çevrimiçi araç vardır.

### <a name="testing-app-links"></a>Uygulama bağlantıları sınama

Uygulama bağlantılar uyguladıktan sonra çeşitli parçaları bunlar beklendiği gibi çalıştığından emin olmak için test edilmelidir.

Dijital varlıklar dosyası doğru biçimlendirilmiş ve Google dijital varlık bağlantıları API'sini kullanarak bu örnekte gösterildiği gibi barındırılan onaylamak mümkündür:

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Hedefi filtreleri düzgün yapılandırıldığını ve uygulama için bir URI varsayılan işleyici olarak ayarlandığından emin olmak için gerçekleştirilen iki testleri vardır:

1.  Dijital varlık dosyası, yukarıda açıklandığı gibi düzgün barındırılıyor. İlk test, Android mobil uygulamaya yönlendirmelidir amacına gönderir. Android uygulama başlatın ve URL'sini kayıtlı etkinlik görüntüler. Bir komut isteminde şunu yazın:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  Belirli bir aygıtta yüklü uygulamalar için ilkeler işleme varolan bağlantıyı görüntüler. Aşağıdaki komutu aşağıdaki bilgilerle cihazdaki her kullanıcı için bağlantı ilkelerin listesini döküm. Komut satırında, aşağıdaki komutu yazın:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; Uygulama paketinin adıdır.
    * **`Domain`** &ndash; Web bağlantıları uygulama tarafından işlenecek (boşlukla ayrılmış olarak) etki alanları
    * **`Status`** &ndash; Bu uygulama için geçerli bağlantı işleme durumudur. Değerini **her zaman** uygulaması anlamına gelir `android:autoVerify=true` bildirilir ve sistem doğrulama geçti. Tercih Android sistemin kaydını temsil eden bir onaltılık sayı tarafından izlenir.

    Örneğin:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Özet

Bu kılavuzda ele alınan Android 6.0 nasıl uygulama bağlama çalışır. Ardından, bir Android 6.0 uygulamasını desteklemek ve uygulama bağlantılar için yanıtlama nasıl ele. Ayrıca, bir Android uygulamasını Uygulama bağlama test etme açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Keystore'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md)
- [Etkinlikleri ve amaçları](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google dijital varlıklar bağlantılar](https://developers.google.com/digital-asset-links/)
- [Deyimi listesi oluşturucu ve Tester](https://developers.google.com/digital-asset-links/tools/generator)
