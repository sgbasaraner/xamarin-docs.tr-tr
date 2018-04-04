---
title: Dondurma Sandwich özellikleri
description: Bu makalede Android 4 API - dondurma Sandwich uygulama geliştiricileri için kullanılabilir yeni özelliklerin bazıları açıklanmaktadır. Birkaç yeni kullanıcı arabirimi teknolojilerini kapsar ve çeşitli uygulamalar ve cihazlar arasında veri paylaşımı için Android 4 sunan yeni özellikler inceler.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 3c5412629f6dcd31fb0a82634ef530569eef459a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="ice-cream-sandwich-features"></a>Dondurma Sandwich özellikleri

_Bu makalede Android 4 API - dondurma Sandwich uygulama geliştiricileri için kullanılabilir yeni özelliklerin bazıları açıklanmaktadır. Birkaç yeni kullanıcı arabirimi teknolojilerini kapsar ve çeşitli uygulamalar ve cihazlar arasında veri paylaşımı için Android 4 sunan yeni özellikler inceler._

## <a name="overview"></a>Genel Bakış

Android işletim sistemi sürüm 4.0 (API düzeyi 14) bir ana Android işletim sistemi denetçiye yeni biçim temsil eder ve bir dizi önemli değişiklikler ve yükseltmede içerir:

-   **Güncelleştirilmiş kullanıcı arabirimi** – geliştiriciler daha fazla güç birkaç yeni kullanıcı Arabirimi özelliklerini verin ve uygulama kullanıcısı oluşturduğunuzda esneklik arabirimleri. Bu yeni özellikler şunlardır: `GridLayout` , `PopupMenu` , `Switch` pencere, ve `TextureView` . 
-   **Daha iyi donanım hızlandırmasını** – şimdi tüm Android denetimleri için GPU üzerinde gerçekleşmesi işleme 2B. Ayrıca, donanım hızlandırmasını, Android 4.0 için geliştirilen tüm uygulamalarda varsayılan olarak açıktır. 
-   **Yeni veri API'leri** – yeni önceden resmi olarak erişilebilir, Takvim verileri ve cihaz sahibinin kullanıcı profili gibi değildi verilere erişimi yoktur. 
-   **Uygulama veri paylaşımı** – uygulamalar ve cihazlar arasında veri paylaşımı şimdi teknolojileri her zamankinden daha kolay gibi `ShareActionProvider` , hangi kolaylaştırır bir eylem çubuğundan paylaşım bir eylem oluşturun ve *Android ışını* için *yakın alan iletişimi (NFC)* , hangi kılar yakın birbirlerine cihazlar arasında veri paylaşmak için ek. 


Bu makalede, bu özellikler ve Android 4.0 API yapılan diğer değişiklikler keşfetmek için oluşturacağız ve her bir özelliğin Xamarin.Android ile kullanmak nasıl açıklayacağız.

## <a name="user-interface-features"></a>Kullanıcı Arabirimi Özellikleri

Android 4 dahil olmak üzere, çeşitli yeni kullanıcı arabirimi teknolojiler mevcuttur:

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – denetimlerin 2B kılavuz düzenini destekler. 
-   **[Geçiş pencere öğesi](~/android/user-interface/controls/switch.md)**  – arasında geçiş verir açık veya kapalı. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – video ve bir görünüm içindeki OpenGL içeriği sağlar. 
-   **[Gezinti çubuğu](~/android/user-interface/controls/navigation-bar.md)**  – geri, ev ve çoklu görev sanal düğmelerini içerir. 


Ayrıca, diğer kullanıcı Arabirimi öğeleri, gibi geliştirilmiştir `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, artık çalışmak kolaydır ve sekmeler, daha zarif bir görünümü olan.

## <a name="sharing-features"></a>Paylaşım Özellikleri

Android 4 cihazlar ve uygulamalar arasında veri paylaşma bize birkaç yeni teknolojileri içerir. Ayrıca, çeşitli Takvim bilgileri ve cihaz sahibinin kullanıcı profili gibi daha önce kullanılabilir olmadığından veri türlerine erişim sağlar. Biz inceleyeceğiz Bu bölümde çeşitli özellikler dahil olmak üzere bu alanlar adresi Android 4 tarafından sunulan:

-  **[Android ışını](~/android/platform/android-beam.md)**  – verir veri NFC paylaşımı.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – geliştiricilerinin eylem çubuğunda paylaşım eylemleri belirtmenizi sağlayan bir sağlayıcı oluşturur. 
-   **[Kullanıcı profili](~/android/user-interface/user-profile.md)**  – cihaz sahibinin profil verilerine erişim sağlar. 
-   **[Takvim API](~/android/user-interface/controls/calendar.md)**  – Takvim sağlayıcısı'ndan Takvim verilere erişim sağlar. 

## <a name="x86-emulators"></a>x86 Öykünücüler

ICS henüz desteklemediği bir x86 ile geliştirme öykünücüsü. x86 Öykünücüler yalnızca Android 2.3.3, API düzey 10 ile desteklenir. Bkz: [x86 yapılandırma öykünücüsü](~/android/get-started/installation/android-emulator/index.md) daha fazla bilgi için.

## <a name="summary"></a>Özet

Bu makalede artık Android 4 ile kullanılabilen yeni teknolojileri çeşitli ele. Biz gibi yeni kullanıcı arabirimi özellikleri gözden *GridLayout*, *PopupMenu*, ve *anahtar* pencere öğesi. Ayrıca bazı çalışmak için yanı sıra sistem kullanıcı Arabirimi, denetleme yeni destek inceledik *TextureView*. Ardından yeni paylaşım teknolojileri çeşitli açıklanmıştır. Biz nasıl ele *Android ışını* şimdi kullanan aygıtlar arasında bilgi paylaşımı *NFC*, yeni ele alınan *Takvim API*ve ayrıca yerleşik içindekullanmaknasıloluşturulacağınıgösterir *ShareActionProvider*.
Son olarak, biz nasıl kullanılacağını incelenmesi *ContactsContract* sağlayıcı kullanıcı profili verilere erişme.



## <a name="related-links"></a>İlgili bağlantılar

- [Dondurma Sandwich örnekleri](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (örnek)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (örnek)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Sekme düzeni Öğreticisi](~/android/user-interface/layouts/tab-layout/index.md)
- [Dondurma Sandwich](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 platformu](http://developer.android.com/about/versions/android-4.0.html)
