---
title: Sıkça Sorulan Sorular
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: f8264c48b3b4679d45d5603b5637df0016cd3d17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular

## <a name="general-questions"></a>Genel Sorular

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Bir Mac Sanal Makinesini Xamarin ile kullanabilir miyim?](mac-vm.md)
Evet, ancak yalnızca Mac donanımda.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Xcode’un eski sürümünü nasıl yükleyebilirim?](downgrade-xcode.md)
Bu kılavuzun yanı sıra en son sürüm Xcode önceki sürümlerini erişmek için bağlantılar sağlar.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[iOS SDK Konumlarımı nereden ayarlayabilirim?](ios-sdk.md)
Çoğu kullanıcı için bu uygun konumlara otomatik olarak ayarlanır. Bu kılavuz, varsayılan SDK konumların ve gerekirse nasıl değiştirileceğini listeler.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[Nasıl iOS güncelleştirdikten sonra ı Geliştirici seçenekleri etkinleştirebileceğiniz?](update-developer-options.md)
Bir iOS hata Geliştirici seçenekleri iOS sürümlerini güncelleştirdikten sonra kaybolmasına neden olabilir, bu iOS geçerken gözlenmiştir 8.x. Bu kılavuzda nasıl seçenekleri yeniden iler hale açıklanmaktadır.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[iOS 8’de Kullanıcı Konumu çalışmıyor](ios8-user-location.md)
Bu kılavuz, iOS 8 kullanıcı konumu etkinleştirmek için info.plist düzenlemek anlatır.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[iOS kilitlenme günlüklerini sembolleştirmek için .dSYM dosyasını nereden bulabilirim?](symbolicate-ios-crash.md)
Bu kılavuzda kilitlenme yardımcı olmak için iOS kilitlenme günlüklerini symbolicating için temel adımlar açıklanmaktadır. Ek kaynaklar için daha gelişmiş symbolication teknikleri & iOS kilitlenme günlüklerini yorumlama hakkında bilgi için ayrıca bağlar.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Xamarin Studio’da iOS projeleri için Mono Çalışma Zamanı ortam değişkenlerini nasıl ayarlayabilirim?](xs-mono-runtime.md)
Tüm çalışma zamanı Mono için ortam değişkenlerini ayarlama ihtiyacınız varsa, bunlar ayarlanabilir **proje Seçenekleri > Çalıştır > Genel** sayfası.

## <a name="publishing-questions"></a>Yayımlama sorular

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[Uygulama deposuna gönderirken hata: "Geçersiz paket - seçenekleri bitcode'u katıştırılacak izin verilmiyor gönderisine algılanır"](invalid-bundle-bitcode.md)

Uygulamaları gönderme, _gerektiren_ watchOS ve tvOS uygulamalar gibi bitcode'u Xcode 9 ile yapılmalıdır.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[Çıkış yolu IPA dosyasının değiştirebilir miyim?](ipa-output-path.md)
Xamarin döngüsü 7 itibariyle, bunu başarmak için özelleştirilmiş MSBuild hedefleri kullanabilirsiniz.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[Nasıl ı TFS bırakma klasörüne IPA çıktı dosyalarını kopyalayabilirsiniz?](ipa-tfs.md)
Evet, bu kılavuzda açıklanmaktadır nasıl.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[I dosyalarını ekleyebilir veya Visual Studio'da oluşturduktan sonra bir IPA dosyasından dosyaları kaldırmak?](modify-ipa.md)
Evet, mümkündür, ancak genellikle yeniden imzalamak gerekir `.app` değişikliği yaptıktan sonra paketi. Bu değiştirme Not `.ipa` dosya normal kullanımda gerekli değildir. Bu makale yalnızca bilgilendirme amacıyla sağlanmıştır.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Visual Studio'dan .xcarchive arşivini oluşturmak mümkün mü?](create-xcarchive.md)
Xamarin 4'ten itibaren bunu şimdi oluşturmak mümkündür bir `.xcarchive` ayarlayarak Windows `ArchiveOnBuild` özelliğine `true`.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[Uygulama gönderme işlemim neden şu hatayla başarısız oluyor: "... adresinde izin verilmeyen yollar ( "iTunesMetadata.plist" ) bulundu”?](itunesmetadata-disallowed-paths.md)
Bu hata Apple App Store doğrulama işleminde bir değişiklik sonucudur. Bu belirli hata _değil_ belirli Xamarin yüklü olan sürümü için ilgili olarak, bu nedenle önceki sürüme indirme olur _değil_ yardımcı olur. Sorunu gidermek nasıl daha fazla bilgi için bu kılavuzu bağlantılar.


## <a name="diagnosing-specific-error-messages"></a>Özel hata iletileri tanılama

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[RegisterServicePort ile iOS Designer Hatası](error-registerserviceport.md)
Hatalarla `RegisterServicePort` ve yukarıdaki benzer hata iletileri gibi yaygın olarak casus yazılım/kötü amaçlı yazılım bilgisayardaki ile ilgili bir sorun. Tanılama ve casus yazılım/kötü amaçlı yazılım kaldırma hakkında bilgi onaylayan bu kılavuzu ayrıntıları.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[iOS derlemem neden şu hatayla başarısız oluyor: anahtarlıkta geçerli bir iPhone kod imzalama anahtarı bulunamadı?](no-codesigning-keys.md)
Bu hata iletisini söz konusu proje kod imzalama için geçerli kimlik bilgileri bakarken oluşur ancak bunları bulamıyor. Kod imzalama, test ve fiziksel iOS cihazlarda dağıtımlar için gereklidir; aynı zamanda derlemeleri deposundan geçici & uygulama.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[iOS 9 uygulamam neden şu hatayla başarısız oluyor: Sistem Özel Durumu: Objective-C nesnesi hazırlanamadı?](exception-marshal-obj-c.md)
API iOS 9 artık temel API yönetilmeyen kodu çağırma beklerken bir geri çağırma Oluşturucu kullanılmasını gerektirir.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[Çalışma zamanı hatası: mscorlib.dll bütünleştirilmiş kodu bulunamadı veya yüklenemedi](error-mscorlib-not-found.md)
Bu sorun oluşur zaman *gizli* `.monotouch-32` ve `.monotouch-64` klasörlerdir eksik `.xcarchive` imzalama / IPA oluşturma, çalışma zamanı hatası tetikleyen.

## <a name="deprecated"></a>Kullanım dışı

> [!IMPORTANT]
> Aşağıdaki makaleleri Xamarin en güncel sürümlerini çözümlenen sorunlar için geçerlidir. Ancak, en son sürümünü yazılım üzerinde gerçekleşirse, lütfen dosya bir [yeni hata](~/cross-platform/troubleshooting/questions/howto-file-bug.md) yapı bilgi ve tam bir günlük çıktısı, tam sürüm oluşturma.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA dosyası 0 bayt’tır](ipa-zero-bytes.md)
Önceki sürümlerinde 0 bayt olması için Windows IPA dosyada neden olabilecek Xamarin, bilinen bazı sorunlar vardı.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool Hatası: İşlem tamamlanamadı.](error-ibtool.md)
Apple [sabit](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) bu `ibtool` hatasıdır 6.1.1, bu nedenle Xcode 6.1.1 yükseltme veya daha yüksek xcode'da kolay düzeltme.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[MT1009 hatası: Bütünleştirilmiş kod kopyalanamadı](error-mt1009.md)
Xamarin.iOS 7.2.6 çalıştıran kullanıcılar etkiler. Bu sorunu Xamarin.iOS farklı bir kullanıcı hesabıyla yüklendiğinde, daha yüksek ayrıcalıklar gerektiren dosya izinleri nedeniyle sonra Geliştirici ana hesabıdır.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[AMDeviceNotificationSubscribe Sistem Özel Durumu döndürüldü ...](exception-amddevicenotificationsubscribe.md)
İlk Visual Studio veya Mac için başlattığınızda bir hata iletişim kutusunda Bu ileti görünebilir `mtbserver.log` dosya. Bu seyrek bir sorun olduğunu unutmayın. Visual Studio Mac yapı konağa bağlanırken sorun yaşıyor varsa görünür olasılığı yüksek olan diğer hataları `mtbserver.log` dosya.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe, rver.BaseCommand.OnRequest öğesini bulamadı](mdocarchivetomsxdocconverter-not-found.md)
Bu hata görüntülenebilir `Mac Server Log` Visual Studio.
