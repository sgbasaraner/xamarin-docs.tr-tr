---
title: Sorun giderme
description: "Bu makalede, Xamarin.iOS uygulamaları iOS 9 ile çalışmak için birkaç sorun giderme ipuçları verilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: ca3697b355a45e06f941a6dfd610cd19f922ca75
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshooting"></a>Sorun giderme

_Bu makalede, Xamarin.iOS uygulamaları iOS 9 ile çalışmak için birkaç sorun giderme ipuçları verilmektedir._

## <a name="there-was-a-problem-parsing-the-xml"></a>XML Ayrıştırma bir sorun oluştu

Xamarin iOS Tasarımcısı Xcode 7 özellikleri henüz desteklemiyor. Film şeritleri Tasarımcısı ile yüklemek başarısız olur _"XML Ayrıştırma bir sorun oluştu"_ yeni iOS 9 (Xcode 7) kullanmaya çalışırken StackView gibi tasarımcı öğeleri.

iOS Xcode 7 özellikleri tasarımcı desteği yakında döngüsü 6 özelliği sürüm için yöneliktir. Önizleme sürümü döngüsü 6 alfa kanal şu anda kullanılabilir değil ve yeni Xcode 7 özellikleri için destek sınırlıdır.

Mac için Visual Studio için kısmi geçici çözüm: film şeridi sağ tıklatın ve seçin **birlikte Aç** > **Xcode arabirimi Oluşturucu**.

## <a name="where-are-the-ios-8-simulators"></a>İOS 8 benzeticileri nerede?

Xcode 7 (veya üst sürümü) yüklü değilse, otomatik olarak tüm iOS 8 benzeticileri ile iOS 9 benzeticileri varsayılan olarak değiştirir. İOS 8 test etmek gerekiyorsa, Xcode, başlangıç sonra indirin ve iOS 8 benzeticileri yükleyin.

Xcode'da, seçin **Xcode** sonra menü **tercihleri...**   >  **İndirmeleri**:

[![](troubleshooting-images/ios8.png "iOS 8 benzeticileri indirir")](troubleshooting-images/ios8.png#lightbox)

Tıklatın **onay ve Şimdi Yükle** düğmesini iOS 8 benzeticileri yeniden yükleyin.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Sol/sağ özniteliği hatalarla yerleşim kısıtlaması

İOS 8 (ve önceki), kullanıcı Arabirimi öğeleri film şeritleri içinde hem de bir karışımını kullanabilirsiniz **sağ** & **sol** öznitelikleri (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) ve  **Önde gelen** & **sağ** öznitelikleri (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) aynı düzeni.

Aynı film şeridi iOS 9 çalıştırırsanız, aşağıdaki biçimde bir özel duruma neden olur:

> Uygulama yakalanmayan 'NSInvalidArgumentException' özel durum nedeniyle sonlandırılıyor, nedeni: ' *** + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: başında/bitmiyorsa arasında bir kısıtlama kurulamaz özniteliği ve sağa/sola özniteliği. Önde gelen/hem ya da hiçbirini sondaki kullanın.'

iOS 9 zorlar kullanın ya da düzenleri **sağ** & **sol** _veya_ **önde gelen**  &   **Sondaki** öznitelikleri ancak *değil* her ikisi de. Bu sorunu gidermek için film şeridi dosya içinde aynı özniteliği kullanmak için tüm düzeni kısıtlamalarını değiştirin.

Daha fazla bilgi için lütfen bkz [iOS 9 kısıtlama hatası](http://stackoverflow.com/questions/32692841/ios-9-constraint-error) yığın taşması tartışma.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>HATA ITMS-90535: Beklenmeyen CFBundleExecutable anahtarı

İOS 9 değiştirdikten sonra bir uygulamadan derlenmiş ve iOS 8 (veya öncesi), iTunes Bağlan biçiminde bir hata iletisi için yeni derleme göndermeye çalışırken çalıştırılmıştır 3 taraf bileşenleri (özellikle sunduğumuz mevcut Google haritalar bileşeni) kullanır:

> HATA ITMS-90535: Beklenmeyen CFBundleExecutable anahtarı. Paket 'Payload/app-name.app/component.bundle' konumundaki paket yürütülebilir dosya içermiyor...

Bu sorunları genellikle adlandırılmış paket projede bulma tarafından çözüldü sonra kullanabilirsiniz - yalnızca hata iletisi anlaşılacağı gibi - düzenlenen `Info.plist` olan paketteki kaldırarak `CFBundleExecutable` anahtarı. `CFBundlePackageType` Anahtar ayarlanmalıdır `BNDL` de.

Bu değişiklikleri yaptıktan sonra temiz yapın ve tüm projeyi derleyin. Bu değişiklikleri yaptıktan sonra sorun olmadan iTunes Bağlan gönderme olması gerekir.

Daha fazla bilgi için lütfen bu bakın [yığın taşması](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) tartışma.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake (-9824) hatası ile başarısız oldu.

Doğrudan veya bir web görünümü iOS 9'internet ' e bağlanmaya çalışılırken bir hata biçiminde alabilirsiniz:

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Veya formunda:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

İOS9 içinde uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulamanız arasındaki güvenli bağlantılar zorlar. Ayrıca, ATS iletişimi kullanılması `HTTPS` protokolü ve TLS sürüm 1.2 ile iletme gizliliği kullanılarak şifrelenmesi için üst düzey API iletişimi.

Varsayılan olarak tüm bağlantılar kullanarak iOS 9 ve OS X 10.11 sürümünü (El Capitan) için oluşturulan uygulamaların ATS etkin olduğundan `NSURLConnection`, `CFURL` veya `NSURLSession` ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantılarınızı bu gereksinimi karşılamıyorsa, bir özel durum ile başarısız olur.

Lütfen bakın [Opting-ATS dışı](~/ios/app-fundamentals/ats.md) bölümünü bizim [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) Kılavuzu bu sorunu çözmek hakkında bilgi için.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Varolan uygulamalarım 9 İos'ta çalıştırma

Bkz: bizim [iOS 9 uyumluluk bilgileri](~/ios/platform/introduction-to-ios9/ios9.md) iOS 9 çalıştırmak yeniden oluşturma ve mevcut uygulamalarınızı yeniden dağıtma hakkında yönergeler için.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView is Null in Constructors

**Neden:** iOS 9 içinde `initWithFrame:` Oluşturucusu olan artık iOS 9 davranış değişiklikleri nedeniyle gerekli [UICollectionView belgelerine durumları](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Belirtilen tanımlayıcı için bir sınıf kayıtlı ve yeni bir hücreye oluşturulmalıdır, hücre şimdi çağrılarak başlatılır, `initWithFrame:` yöntemi.

**Düzeltme:** Ekle `initWithFrame:` Oluşturucusu şuna benzer:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

İlgili örnekleri: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Bir görünüm Xib/Nib yüklerken Kodlayıcı ile başlatma için UIView başarısız

**Neden:** `initWithCoder:` Oluşturucusu olan bir görünümü bir arabirim Oluşturucu Xib dosyasından yüklenirken adlı bir. Bu oluşturucu değil verildiyse, yönetilmeyen kod yönetilen bizim sürümü çağrılamaz. Daha önce (ör.) iOS 8 içinde) `IntPtr` Oluşturucusu görünüm başlatmak için çağrıldığı.

**Düzeltme:** oluşturma ve verme `initWithCoder:` Oluşturucusu şuna benzer:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

İlgili örnek: [sohbet](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld iletisi: Hiçbir önbellek ada sahip bir görüntü...

Günlükteki şu bilgileri içeren bir kilitlenme karşılaşabilirsiniz:

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Neden:** bu özel framework ortak yaptıklarında gerçekleşen Apple'nın yerel bağlayıcı, bir hata olduğunu (JavaScriptCore yapıldı iOS 7, ortak önce özel bir çerçeve olan), ve uygulamanın dağıtım hedefi için bir iOS sürümü olduğunda Framework'te özel. Bu durumda Apple'nın bağlayıcı çerçevesi ortak sürümü yerine özel sürümü ile bağlantı içerir.

**Düzeltme:** bu iOS 9 ele alınacaktır ancak uygulayabileceğiniz kendiniz zarfında kolay bir geçici çözüm yoktur: yalnızca (deneyebilirsiniz iOS 7 bu durumda) sonraki bir iOS sürümünü projenizdeki hedefleyin. Diğer çerçeveler benzer sorunlar sergileyen, örneğin da WebKit framework iOS 8 genel yapıldı (ve iOS 7 hedefleme bu hata; neden olacak şekilde iOS 8 uygulamanızda WebKit kullanmasını hedeflemelidir).

## <a name="untrusted-enterprise-developer"></a>Güvenilmeyen Kurumsal Geliştirici

Xamarin.iOS uygulamanızı iOS 9 sürümü gerçek iOS donanımda çalıştırılmaya çalışılırken Geliştirici hesabınızda cihazda güvenilmeyen olduğunu belirten bir ileti alabilirsiniz. Örneğin:

[![](troubleshooting-images/untrusted01.png "Güvenilmeyen Enterprise Developer Uyarısı")](troubleshooting-images/untrusted01.png#lightbox)

Bu sorunu çözmek için aşağıdakileri yapın:

1. Xcode (en son beta sürümü) geliştirme Mac üzerinde Başlat
2. Seçin **aygıtları** gelen **penceresi** menü aygıtları penceresini açmak için: 

    [![](troubleshooting-images/untrusted02.png "Aygıtları penceresi")](troubleshooting-images/untrusted02.png#lightbox)
3. Altında **AYGITLARI** yan paneli, aygıt, sağ tıklatın ve seçin seçin **sağlama profilleri göster...** : 

    [![](troubleshooting-images/untrusted03.png "SShow sağlama profilleri")](troubleshooting-images/untrusted03.png#lightbox)
4. Her sağlama profilinde şu anda tıklatın ve aygıt seçin  **-**  düğmesi silmek için: 

    [![](troubleshooting-images/untrusted04.png "Bir sağlama profili silme")](troubleshooting-images/untrusted04.png#lightbox)
5. Gelen **Xcode** menüsünde, select **tercihleri...**  ve **hesapları**: 

    [![](troubleshooting-images/untrusted05.png "Xcode hesap tercihleri")](troubleshooting-images/untrusted05.png#lightbox)
6. Tıklatın **ayrıntıları görüntüle...**  düğmesine ve ardından **tüm indirin** düğmesi: 

    [![](troubleshooting-images/untrusted06.png "Tüm profillerin indirin")](troubleshooting-images/untrusted06.png#lightbox)
7. Listenin güncelleştirme tamamlandığında tıklatın **Bitti** düğmesine tıklayın ve tercihleri penceresini kapatın.
8. İOS cihazının test etmek için çalıştığınız Xamarin.iOS uygulamasının var olan sürümü kaldırın.
9. Mac için Visual Studio'ya geri dönün, temiz bir yapı yapın ve uygulamayı cihazda yeniden çalıştırmayı deneyin.

Durdurun ve Visual Studio, Xcode tarafından yüklenen yeni sağlama profilleri görülen önce Mac için yeniden başlatmanız gerekebilir. Ayarlamanız gerekebilir **iOS paket imzalama** seçeneklerini Xamarin.iOS uygulamanızı yeni sağlama profili seçin.

## <a name="launch-screen-issues"></a>Ekran sorunları başlatma

Böylece aynı başlatma görüntü artık farklı arabirimi yönler desteklemek için yeniden kullanılabilir iOS 9 Şimdi Başlat ekranında gereksinimleri zorunlu tutar. Apple'nın bkz [UILanchImage başvuru](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) daha fazla bilgi için.

İsteğe bağlı olarak, bir dizi kullanılarak aksine, uygulamanızın başlatma ekran sunmak için film şeridi dosya kullanabilirsiniz **.png** görüntü dosyaları. Apple şekilde başlatma ekranlar sunmak tercih edilen artık budur. Lütfen bakın bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) daha fazla bilgi için Kılavuzu.

Son olarak, uygulamanız için başlatma, ekranı film şeridi dosya kullanın ve bu tüm dört arabirimi yönler (dikey, görüntülemediğini dikey, yatay sola ve yatay sağ) bir slayt üzerinden panelinde veya bölme görünüm modunda çalıştırmak için kabul edilmesi için destek gerekir. İOS 9 yeni görevli yeteneklerini hakkında daha fazla bilgi için lütfen bkz bizim [iPad için çoklu](~/ios/platform/multitasking.md) Kılavuzu.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException özel durumu

Derleme ve iOS 9 için var olan bir Xamarin.iOS uygulaması çalıştırırken bir hata biçiminde alabilirsiniz:

> Objective-C özel durum oluşturuldu.  Ad: NSInternalInconsistencyException neden: uygulama windows kök görünüm denetleyicisini uygulama başlatma işleminin sonunda olması beklenir

Bu hatadır uygulama Windows kök View Controller uygulama başlatma işleminin sonunda olması beklenir ve varolan uygulamanızı değil çünkü gerçekleştirilen.

Bu sorun için en az iki olası geçici çözümler vardır:

1. Film şeridi dosya yerine kullanılacak güncelleştirme uygulamasını `xib` kendi kullanıcı arabirimi tanımlamak için dosyaları. Bu bir düzen film şeritleri için oldukça çok zaman uygulamanızı boyutunu ve iOS Tasarımcısı (veya Xcode'nın arabirimi Oluşturucu) kullanmak bilgiye bağlı olarak gerektirir. Daha fazla bilgi için bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) belgeleri.
2. Kurulum `RootViewController` uygulama penceresi özelliği de `FinishedLaunching` yönteminde `AppDelegate` View Controller uygulamanızın kullanıcı arabiriminde işaret edecek şekilde sınıfı.

## <a name="when-to-initialize-views-and-view-controllers"></a>Initialize görünümleri ve görünüm denetleyicileri zamanı

Xamarin.iOS ile bir şey yönetilen koda sunulan ancak iOS tasarım keser olarak adlandırılan oluşturucular içinde görünüm veya Görünüm denetleyicisini başlatma yapmak mümkündür.

Genel olarak, her şeyi emin olamayacağından Objective-C kodunu oluşturucudan geri çağırabilirsiniz başlatmalıdır değil zaman çağrılır. Ayrıca anlamına daha iyi bir yerde (diğer .ctor) yok ya da (Objective-C hiçbir olay olduğu gibi) geçersiz kılmak için çağrıları burada bu başlatma yapılması.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [Yeni iOS 9.0 nedir](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Xamarin.iOS uygulamaları iOS9 (video) güncelleştiriliyor](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
