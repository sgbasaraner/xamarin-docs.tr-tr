---
title: "Belirli bazı lisans hataları"
ms.topic: article
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 80006ce594db5baef5d295537f198181fe0b0fe1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="some-specific-licensing-errors"></a>Belirli bazı lisans hataları

> [!IMPORTANT]
> Eski Xamarin lisansları belirli sorunları oldukları için aşağıdaki bilgileri MSDN kullanıcılara uygulanmaz. Bir MSDN kullanıcısıysanız benzer hatalar aşağıdaki görürseniz, lütfen deneyin [Xamarin en son sürüme güncelleştirmeyi](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) & oluşturun.%n%nBu için [lisans seçenekleri Kılavuzu](~/cross-platform/get-started/requirements.md).



## <a name="invalid-license"></a>"Geçersiz Lisans"

> Geçersiz Lisans. Lütfen Xamarin.Android (XA9999) belirlenemiyor lisans sürümü yeniden etkinleştirin. (XA9010)

Bu iletiler birkaç farklı koşullarda yer alabilir.

-   Diskteki geçerli lisans eşitleme olabilir bilgisayardaki geçerli kullanıcı bilgileriyle. [Lisans dosyalarını yenileme](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) çoğu durumda bu sorunu giderir.

-   Bilgisayar 2 çakışan yinelenen etkinleştirmeleri olabilir. Bu lisans türü artık Xamarin tarafından kullanılır. Bu hata için lütfen ilgili gibi görünen bir sorunla karşılaşırsanız [e-posta desteği](https://www.xamarin.com/support).

-   Xamarin hesap Xamarin lisansı henüz davet. Ayrıca bkz: [ekip Lisans Yönetimi](~/cross-platform/troubleshooting/legacy-licenses/team-management.md).

## <a name="failed-to-load-android-entitlements"></a>"Android yetkilendirmeler yüklenemedi"

> Mono.VisualStudio.Shell.ShellPackage hata: 0: Android yetkilendirmeler yüklenemedi: lisansı geçersiz. Lütfen Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage hata yeniden etkinleştirme: 0: Lisans güncelleştirilemedi: Geçersiz Lisans. Lütfen Xamarin.Android (XA9999) yeniden etkinleştirme

Yukarıda "Geçersiz Lisans" sorunu daha eski bir sürümü budur. Aynı sorun giderme adımlarını uygulayın.

## <a name="this-version-was-released-after-your-subscription-expired"></a>"Bu sürümü aboneliğinizin süresi sonra yayımlanmıştır"

> Hata XA9000: Bu sürümü aboneliğinizin süresi sonrasında yayımlanan (11/11/2014 5:11:41 PM). (XA9000) (Mobile.Android.Utilities) hata XA9010: Lisans sürümü belirlenemiyor. (XA9010) (Mobile.Android.Utilities)

Bu iletiler genellikle iki durumlarda görünür:

-   Disk üzerinde lisansları geçmiştir. [Lisans dosyalarını yenileme](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) çoğu durumda bu sorunu giderir.

-   Xamarin abonelik artık etkin olmadığını ve geçerli yüklü Xamarin abonelik sona erme tarihinden daha yeni sürümüdür. Bu durumda için doğru düzeltme olan [düşürmek](http://kb.xamarin.com/customer/portal/articles/1699777) abonelik süresi dolmadan önce yayımlanan Xamarin sürümüne. E-posta göndermek çekinmeyin [ hello@xamarin.com ](mailto:hello@xamarin.com) aboneliğiniz için geçerli en son sürüm istemek için. Hala eski sürüme düşürmeyi sonra hatasını görürseniz, lisans dosyalarını çok yenilemeyi deneyin emin olun.

## <a name="additional-references"></a>Ek başvurular

-   [Lisans yenileme](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [Xamarin düşürmek nasıl](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Xamarin.Android hata kodlarının listesi](~/android/troubleshooting/errors.md)
-   [MonoTouch (Xamarin.iOS) hata kodlarının listesi](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>Sonraki Adımlar
Bizimle iletişime geçin veya bile Yukarıdaki bilgilerin kullanılarak sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [için Xamarin hangi destek seçenekleri kullanılabilir?](~/cross-platform/troubleshooting/support-options.md) önerileri, iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Yeni bir hatanın gerekirse dosya.
