---
title: Neden Xamarin Visual Studio veya Visual Studio uygulamasına Mac için oturum olamaz?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: cb750a7c282ecab6e2193bb554e470086868e018
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>Neden Xamarin Visual Studio veya Visual Studio uygulamasına Mac için oturum olamaz?

> [!IMPORTANT]
> Bu kılavuz sahibi veya Xamarin hesaplara günlük kullanmadığınız sürece olması gerekmez çünkü çoğu MSDN kullanıcılara uygulanmaz [Xamarin bileşenleri depolamak](https://components.xamarin.com/) veya [Mac (Mac) için Visual Studio](~/cross-platform/get-started/requirements.md). MSDN lisans sahipleri için başvurmak [lisans seçenekleri Kılavuzu](~/cross-platform/get-started/requirements.md) yerine.



## <a name="overview"></a>Genel Bakış
IDE içinde Xamarin hesabınızda oturum açmasını engelleyebilir birkaç ortak nedenleri vardır. Bilinen sorunlar ve düzeltmeleri aşağıda açıklanmıştır.

### <a name="finding-the-login-screen"></a>Oturum açma ekranı bulma

Başvuru için oturum açma ekranları burada bulundu:

- Mac için Visual Studio
   - Hoş Geldiniz ekranında sağ üst köşesinde
   - **Mac için Visual Studio > Hesap** (Mac)
   - **Araçlar > Hesap** (Windows)
- Visual Studio
   - **Araçlar > Xamarin hesabı**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>IDE bağlanma, ancak hesap ekran doğru oturum açma bilgilerini gösteren değil

Bu sorun genellikle el ile lisans yeniden eşitleme tarafından çözümlenir.
Lütfen bu makaledeki yönergeleri izleyin: [nasıl yedeklerim el ile resyncronize Xamarin lisansları?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>Geçersiz hesap bilgisi

Xamarin Web sitesine gidin, [oturum açma sayfasına](https://store.xamarin.com/Login?from=%2faccount%2f), geçerli hesap kimlik bilgilerinizi kullanarak günlük deneyebilirsiniz.
Sayfasında, bağlantılar, parolanızı sıfırlamak için ve yeni bir hesap oluşturmak için de vardır.

## <a name="account-is-valid-but-the-ide-cant-connect"></a>Hesap geçerli ancak IDE bağlanamıyor

Bu, genellikle güvenlik duvarı veya diğer güvenlik ayarları gerekli uç noktaları erişimini IDE engellediğinizde kaynaklanır.
Etkinleştirme sunucularını aşağıdaki erişmeniz gerekir:

> activation.xamarin.com store.xamarin.com auth.xamarin.com

Ancak, diğer birkaç uç noktaları güncelleştirmeleri, Başlarken NuGet paketleri, vb. gibi genel geliştirme süreçleri için önemlidir. Bu nedenle, emin olmak için önermiştir *tüm* uç noktalarına gelen eklenen [Xamarin Güvenlik Duvarı Yapılandırma Kılavuzu'nda](~/cross-platform/get-started/installation/firewall.md).

### <a name="ios-in-xamarin-studio-windows"></a>Xamarin Studio Windows iOS
Windows için Xamarin Studio'da iOS geliştirme desteklenmiyor. Oturum açma ekranı burada erişirken iOS lisans görüntülenmez.

Bunun yerine, Visual Studio veya Xamarin Studio'yu (Mac) aracılığıyla oturum açma eski lisansa sahip. MSDN kullanıcıların Windows üzerinde oturum açmak gerekmiyorsa unutmayın.

## <a name="additional-support"></a>Ek destek

Yukarıdaki senaryoların değil durumunuza açıklamak veya sorunu çözmek için bu başvurun [destek seçenekleri](https://www.xamarin.com/support).
