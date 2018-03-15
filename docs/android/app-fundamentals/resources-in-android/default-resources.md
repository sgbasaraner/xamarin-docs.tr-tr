---
title: "Varsayılan kaynaklar"
ms.topic: article
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 5cf5bd38612f0f763e30456b0dd42198a3c0ff06
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="default-resources"></a>Varsayılan kaynaklar

Varsayılan kaynaklar olan herhangi bir belirli cihaz veya form faktörü özgü değildir ve bu nedenle Android işletim sistemi tarafından varsayılan seçim yoksa daha belirli öğeleri kaynakları bulunabilir. Bu nedenle, kaynak oluşturmak için en yaygın türü demektir. Alt dizinleri düzenlenmiştir **kaynakları** dizin kaynak türlerine göre:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Varsayılan kaynak dosyaları](default-resources-images/01-resource-files-vs.png)

Yukarıdaki resimde proje drawable kaynakları, düzenleri ve değerleri (basit değerleri içeren XML dosyaları) için varsayılan değerleri vardır.

Kaynak türlerinin tam bir listesi aşağıda verilmiştir:

-  **Animator'da** &ndash; özellik animasyonları açıklayan XML dosyaları.
   Özellik animasyonları API düzeyi 11 (Android 3.0) kullanıma sunulan ve bir nesne üzerindeki özellikleri animasyonun sağlar. Özellik animasyonları animasyonları nesne herhangi bir türde açıklamak için daha esnek ve güçlü bir yöntemdir.

-  **anim** &ndash; açıklayan XML dosyaları *Ara* animasyonları. Ara animasyonları animasyon yönergeleri bir görüntü veya metin boyutu büyüyen bir görünüm nesnesi veya örnek, döndürme içeriğine dönüşümleri gerçekleştirmek için bir dizi var. Yalnızca nesneleri görüntülemek için Ara animasyonları sınırlıdır.

-  **renk** &ndash; renk durumu listesi açıklayan XML dosyaları. Renk durumu listeleri anlamak için bir kullanıcı Arabirimi pencere öğesi bir düğmesi gibi göz önünde bulundurun.
   Basıldı veya devre dışı gibi farklı durumlara sahip olabilir ve düğme durumunda her değişiklik ile renk değiştirebilir. Listeden bir durum listesinde gösterilir.

-  **drawable** &ndash; Drawable kaynaklardır uygulamasına derlenmiş ve ardından API çağrıları tarafından erişilen ya da diğer XML kaynaklar tarafından başvurulan grafikler için genel bir kavram.
   Bit eşlem dosyaları (.png, .gif, .jpg) drawables bazı örnekler şunlardır, olarak bilinen özel yeniden boyutlandırılabilir bit eşlemler [dokuz düzeltme ekleri](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), durumu listeler, XML, vb. tanımlanan genel şekillerine.
 
-  **Düzen** &ndash; bir etkinlik veya bir listede bir satırı gibi bir kullanıcı arabirimi düzeni açıklayan XML dosyaları.

-  **Menü** &ndash; uygulama menüleri gibi açıklayan XML dosyaları *Seçenekler Menüler*, *bağlam menülerini*, ve *alt menüler*. Menüleri örneği için bkz: [açılan menü Demo](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) veya [standart denetimler](https://developer.xamarin.com/samples/mobile/StandardControls/) örnek.

-  **Ham** &ndash; kendi ham, ikili formda kaydedilen rastgele dosyalar. Bu dosyalar ikili biçimde bir Android uygulamasına derlenir.

-  **değerleri** &ndash; basit değerleri içeren XML dosyaları. Değerleri dizin XML dosyasında tek bir kaynak tanımlamaz, ancak bunun yerine birden çok kaynakları tanımlayabilirsiniz. Başka bir XML dosyası renk değerleri listesi tutabilir örneğin bir XML dosyası dize değerleri listesi tutarken.

-  **XML** &ndash; .NET yapılandırma dosyalarına işlevinde benzer XML dosyaları. Çalışma zamanında uygulama tarafından okunabilir rasgele XML bunlar


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Varsayılan kaynak dosyaları](default-resources-images/01-resource-files-xs.png)

Yukarıdaki resimde proje drawable kaynakları, düzenleri ve değerleri (basit değerleri içeren XML dosyaları) için varsayılan değerleri vardır.

Kaynak türlerinin tam bir listesi aşağıda verilmiştir:

-  **Animator'da** &ndash; özellik animasyonları açıklayan XML dosyaları.
   Özellik animasyonları API düzeyi 11 (Android 3.0) kullanıma sunulan ve bir nesne üzerindeki özellikleri animasyonun sağlar. Özellik animasyonları animasyonları nesne herhangi bir türde açıklamak için daha esnek ve güçlü bir yöntemdir.

-  **anim** &ndash; açıklayan XML dosyaları *Ara* animasyonları. Ara animasyonları animasyon yönergeleri bir görüntü veya metin boyutu büyüyen bir görünüm nesnesi veya örnek, döndürme içeriğine dönüşümleri gerçekleştirmek için bir dizi var. Yalnızca nesneleri görüntülemek için Ara animasyonları sınırlıdır.

-  **renk** &ndash; renk durumu listesi açıklayan XML dosyaları. Renk durumu listeleri anlamak için bir kullanıcı Arabirimi pencere öğesi bir düğmesi gibi göz önünde bulundurun.
   Basıldı veya devre dışı gibi farklı durumlara sahip hale getirebilir ve düğme durumunda her değişiklik ile renk değiştirebilir. Listeden bir durum listesinde gösterilir.

-  **yazı tipi** &ndash; API düzeyi 26 başlayarak, bir Android uygulamasını bir kaynak olarak yazı tiplerini katıştırmak mümkündür. Destek kitaplığı 26 API Düzey 14 backport yazı tipi olur. Yazı Tipleri Katıştırılıyor uygulamalarına izin verir

-  **Mipmap** &ndash; Drawable kaynaklardır uygulamasına derlenmiş ve ardından API çağrıları tarafından erişilen ya da diğer XML kaynaklar tarafından başvurulan grafikler için genel bir kavram.
   Bit eşlem dosyaları (.png, .gif, .jpg) drawables bazı örnekler şunlardır, olarak bilinen özel yeniden boyutlandırılabilir bit eşlemler [dokuz düzeltme ekleri](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), durumu listeler, XML, vb. tanımlanan genel şekillerine.

-  **Düzen** &ndash; bir etkinlik veya bir listede bir satırı gibi bir kullanıcı arabirimi düzeni açıklayan XML dosyaları.

-  **Menü** &ndash; uygulama menüleri gibi açıklayan XML dosyaları *Seçenekler Menüler*, *bağlam menülerini*, ve *alt menüler*. Menüleri örneği için bkz: [açılan menü Demo](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) veya [standart denetimler](https://developer.xamarin.com/samples/mobile/StandardControls/) örnek.

-  **Ham** &ndash; kendi ham, ikili formda kaydedilen rastgele dosyalar. Bu dosyalar ikili biçimde bir Android uygulamasına derlenir.

-  **değerleri** &ndash; basit değerleri içeren XML dosyaları. Değerleri dizin XML dosyasında tek bir kaynak tanımlamaz, ancak bunun yerine birden çok kaynakları tanımlayabilirsiniz. Başka bir XML dosyası renk değerleri listesi tutabilir örneğin bir XML dosyası dize değerleri listesi tutarken.

-  **XML** &ndash; .NET yapılandırma dosyalarına işlevinde benzer XML dosyaları. Çalışma zamanında uygulama tarafından okunabilir rasgele XML bunlar

-----
