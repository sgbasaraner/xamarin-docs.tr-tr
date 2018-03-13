---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1c9284f3db1c503b5c88f0d310f916f4c9e3363d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Hataları başvurusu

Bu belge, Xamarin çeşitli hata kodlarını bazı bilgiler sağlar.

<table>
    <thead>
        <tr>
            <td>Kategori</td>
            <td>Açıklama</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> Hataları</td></tr>
        <tr><td>XA1xxx</td><td>Dosya kopyalama / simgesel bağlantı (ilgili proje) hataları</td></tr>
        <tr><td>XA2xxx</td><td>Bağlayıcı hataları</td></tr>
        <tr><td>XA3xxx</td><td>Uygulama Nesne AĞACI hataları</td></tr>
        <tr><td>XA4xxx</td><td>Kod oluşturma hataları</td></tr>
        <tr><td>XA5xxx</td><td>GCC ve araç zinciri hataları</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> İç araçları hataları</td></tr>
        <tr><td>XA7xxx</td><td>Ayrılmış</td></tr>
        <tr><td>XA8xxx</td><td>Ayrılmış</td></tr>
        <tr><td>XA9xxx</td><td>Lisans hataları</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>Hata Kodları

### <a name="xa0xxx-errors"></a>XA0xxx Errors

<table>
    <thead>
        <tr><td>Hata Kodu</td><td>Açıklama</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>Beklenmeyen bir hata - Lütfen sırasında bir hata raporu doldurun <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>'-devname herhangi bir cihaza özel eylem sağlanmadığından</td></tr>
        <tr><td>XA0002</td><td>'{0}' ortam değişkeni ayrıştırılamadı.</td></tr>
        <tr><td>XA0003</td><td>Bir SDK veya ürün derlemesi (.dll) adıyla uygulama adı '{0} .exe' çakışıyor.</td></tr>
        <tr><td>XA0004</td><td>Yeni refcounting mantık sgen çok etkin olmasını gerektirir.</td></tr>
        <tr><td>XA0005</td><td>Çıktı dizini '{0}' mevcut değil</td></tr>
        <tr><td>XA0006</td><td>Hiçbir devel platformu olan '{0}' konumunda, kullanın--platform SDK belirtmek için PLAT =</td></tr>
        <tr><td>XA0007</td><td>Kök derleme '{0}' mevcut değil</td></tr>
        <tr><td>XA0008</td><td>Yalnızca bir kök derleme sağlamalıdır</td></tr>
        <tr><td>XA0009</td><td>Derlemeleri yüklenirken hata oluştu: {0}</td></tr>
        <tr><td>XA0010</td><td>Komut satırı bağımsız değişkenleri ayrıştırılamadı: {0}</td></tr>
        <tr><td>XA0011</td><td>MonoTouch desteklediğinden daha {0} daha yeni bir çalışma karşı ({1}) oluşturuldu</td></tr>
        <tr><td>XA0012</td><td>Eksik veri tamamlamak için sağlanan `{0}`.</td></tr>
        <tr><td>XA0013</td><td>Destek profil sgen çok etkin olmasını gerektirir</td></tr>
        <tr><td>XA0014</td><td>iOS {0} ARMv6 hedefleme uygulamaları oluşturma desteklemiyor</td></tr>
        <tr><td>XA0020</td><td>Mandroid yolu belirlenemedi.</td></tr>
        <tr><td>XA0100</td><td>Android uygulama projesi EmbeddedNativeLibrary '{0}' geçersiz. Lütfen AndroidNativeLibrary kullanın.</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>XA1xxx Errors

<table>
    <thead>
        <tr><td>Hata Kodu</td><td>Açıklama</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>Bir uygulama belirtilen dizininde bulunamadı.</td></tr>
        <tr><td>XA1002</td><td>Simgesel bağlantı oluşturulamadı, dosyalar kopyalandı</td></tr>
        <tr><td>XA1003</td><td>'{0}' uygulama KILL değil. Uygulamayı el ile KILL gerekebilir.</td></tr>
        <tr><td>XA1004</td><td>Yüklü uygulamaların listesi alınamadı.</td></tr>
        <tr><td>XA1005</td><td>Cihaza '{1}' '{0}' uygulamasının KILL değil: {2}. Uygulamayı el ile KILL gerekebilir.</td></tr>
        <tr><td>XA1006</td><td>'{0}' uygulama '{1}' aygıtta yükleyemedi: {2}.</td></tr>
        <tr><td>XA1007</td><td>'{0}' '{1}' aygıtta uygulaması başlatılamadı: {2}. Hala uygulamayı el ile üzerindeki e dokunabilirsiniz başlatabilirsiniz.</td></tr>
        <tr><td>XA1008</td><td>Simulator başlatılamadı: {0}</td></tr>
        <tr><td>XA1009</td><td>'{0}' derleme '{1}' kopyalanamadı: {2}</td></tr>
        <tr><td>XA1010</td><td>'{0}' derlemesi yüklenemedi: {1}</td></tr>
        <tr><td>XA1011</td><td>Kaynak dosyası eksik eklenemedi: '{0}'</td></tr>
        <tr><td>XA1101</td><td>Uygulama başlatılamadı</td></tr>
        <tr><td>XA1102</td><td>(Bunu sonlandırmak için) uygulamaya eklenemedi: {0}</td></tr>
        <tr><td>XA1103</td><td>Ayrılamadı</td></tr>
        <tr><td>XA1104</td><td>Paket gönderme başarısız oldu: {0}</td></tr>
        <tr><td>XA1105</td><td>Beklenmeyen bir yanıt türü</td></tr>
        <tr><td>XA1106</td><td>Cihazda uygulamaların listesi alınamadı: İstek zaman aşımına uğradı.</td></tr>
        <tr><td>XA1107</td><td>Uygulama başlatılamadı.</td></tr>
        <tr><td>XA1201</td><td>Simulator yüklenemedi: {0}</td></tr>
        <tr><td>XA1301</td><td>Yerel Kitaplığı `{0}` ({1}), geçerli yapı architecture(s) ({2}) eşleşmediğinden yoksayıldı</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>XA2xxx Errors

<table>
    <thead>
        <tr><td>Hata Kodu</td><td>Açıklama</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>Derlemeleri bağlanamadı</td></tr>
        <tr><td>XA2002</td><td>Başvuru çözümleyemiyor: {0}</td></tr>
        <tr><td>XA2003</td><td>Bağlama devre dışı olduğundan '{0}' seçeneği göz ardı edilir</td></tr>
        <tr><td>XA2004</td><td>'Ek bağlayıcı tanımları {0}' dosyası bulunamadı.</td></tr>
        <tr><td>XA2005</td><td>'{0}' tanımları ayrıştırılamadı.</td></tr>
        <tr><td>XA2006</td><td>Meta veri öğesi ('{1}' içinde tanımlanmış) ' {0}' başvuru '{2}' den çözümlenemedi.</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>XA3xxx Errors

Bu, uygulama nesne AĞACI hatalardır.

<table>
    <thead>
        <tr><td>Hata Kodu</td><td>Açıklama</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>AOT '{0}' derlemesi yüklenemedi</td></tr>
        <tr><td>XA3002</td><td>Uygulama Nesne AĞACI kısıtlama: [MonoPInvokeCallback] ile donatılmış olduğundan '{0}' yöntemi statik olmalıdır.</td></tr>
        <tr><td>XA3003</td><td>Çakışan--hata ayıklama ve--llvm seçenekleri. Geçici hata ayıklama devre dışı bırakılır.</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>XA4xxx Errors

Bu kod oluşturma hatalardır.

<table>
    <thead>
        <tr><td>Hata Kodu</td><td>Açıklama</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>Ana şablon için expansed bulunamadı `{0}`.</td></tr>
        <tr><td>XA4101</td><td>Kayıt türü için bir imza oluşturamaz `{0}`.</td></tr>
        <tr><td>XA4102</td><td>Kayıt türü geçersiz bulundu `{0}` yöntemi için imzada `{2}`. Bunun yerine `{1}` kullanın.</td></tr>
        <tr><td>XA4103</td><td>Kayıt türü geçersiz bulundu `{0}` yöntemi için imzada `{2}`: türü INativeObject uygular, ancak iki alan bir oluşturucu yok (IntPtr, bool) bağımsız değişkenleri</td></tr>
        <tr><td>XA4104</td><td>Kayıt türü için dönüş değerini sıralanamıyor `{0}` yöntemi için imzada `{1}`.</td></tr>
        <tr><td>XA4105</td><td>Kayıt türü parametresinin sıralanamıyor `{0}` yöntemi için imzada `{1}`.</td></tr>
        <tr><td>XA4106</td><td>Kayıt şirketi yapısı için dönüş değeri sıralanamıyor `{0}` yöntemi için imzada `{1}`.</td></tr>
        <tr><td>XA4107</td><td>Kayıt türü parametresinin sıralanamıyor `{0}` yöntemi için imzada `{1}`.</td></tr>
        <tr><td>XA4108</td><td>Kayıt şirketi yönetilen türü için ObjectiveC türü alınamıyor `{0}`.</td></tr>
        <tr><td>XA4109</td><td>Oluşturulan kayıt kodu derlemek başarısız oldu. Lütfen sırasında bir hata raporu dosya <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>Kayıt türü out parametresi sıralanamıyor `{0}` yöntemi için imzada `{1}`.</td></tr>
        <tr><td>XA4111</td><td>Kayıt türü için bir imza oluşturamaz `{0}' in method `{1}'.</td></tr>
        <tr><td>XA4200</td><td>Yalnızca ACW'ın 'claas' türleri için oluşturabilirsiniz.</td></tr>
        <tr><td>XA4201</td><td>{0} türü için JNI adı belirlenemiyor.</td></tr>
        <tr><td>XA4203</td><td>Belirtilen tür adı tam olarak nitelenmiş olmalıdır.</td></tr>
        <tr><td>XA4204</td><td>Arabirim türü '{0}' çözümlenemiyor. Eksik bir derleme başvurunuz mu var?</td></tr>
        <tr><td>XA4205</td><td>[ExportField] yalnızca 0 parametrelerle yöntemleri kullanılabilir.</td></tr>
        <tr><td>XA4206</td><td>[Verme] bir genel türünde kullanılamaz.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] bir genel türünde kullanılamaz.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] void döndüren bir yöntem kullanılamaz.</td></tr>
        <tr><td>XA4209</td><td>Sınıfı için JavaTypeInfo oluşturulamadı: {1} nedeniyle {0}</td></tr>
        <tr><td>XA4210</td><td>ExportAttribute veya ExportFieldAttribute kullandığınızda Mono.Android.Export.dll bir başvuru eklemeniz gerekir.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}' olan $(TargetFrameworkVersion) '{1}' den küçük. API - kullanma \ {1\} ACW derleme için.</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>XA5xxx Errors

<table>
    <thead>
        <tr><td>Hata Kodu</td><td>Açıklama</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>Eksik '{0}' derleyici. Android NDK yükleyin.</td></tr>
        <tr><td>XA5102</td><td>Yerel kod derlemesinden dönüştürme işlemi başarısız oldu. Lütfen sırasında bir hata raporu dosya <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>'{0}' dosyası derlenemedi. Lütfen sırasında bir hata raporu dosya <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>Yerel bağlama başarısız oldu. Lütfen gcc için sağlanan kullanıcı bayraklarını gözden geçirin: {0}</td></tr>
        <tr><td>XA5202</td><td>Yerel bağlama başarısız oldu. Lütfen yapı günlüğünü gözden geçirin.</td></tr>
        <tr><td>XA5303</td><td>Uyarı bağlama yerel: {0}</td></tr>
        <tr><td>XA5300</td><td>Android SDK bulunamadı veya tam olarak yüklü.</td></tr>
        <tr><td>XA5301</td><td>Aracı 'Şerit' eksik. Lütfen 'Komut satırı araçları' bileşeni yükleyin</td></tr>
        <tr><td>XA5302</td><td>Eksik 'dsymutil' aracı. Lütfen 'Komut satırı araçları' bileşeni yükleyin</td></tr>
        <tr><td>XA5203</td><td>Hata ayıklama simgeleri (dSYM dizin) oluşturulamadı. Lütfen yapı günlüğünü gözden geçirin.</td></tr>
        <tr><td>XA5204</td><td>Son ikili dizeden başarısız oldu. Lütfen yapı günlüğünü gözden geçirin.</td></tr>
        <tr><td>XA5205</td><td>Eksik 'aapt' aracı. Lütfen Android SDK derleme araçları paketini yükleyin.</td></tr>
        <tr><td>XA5206</td><td>{0}. Android kaynak dizini {1} yok.</td></tr>
        <tr><td>XA5207</td><td>{0}. Java kitaplığı dosya {1} yok.</td></tr>
        <tr><td>XA5208</td><td>Yükleme başarısız oldu. Lütfen {0} indirin ve {1} dizinine yerleştirin.</td></tr>
        <tr><td>XA5209</td><td>Şunu başarısız oldu. Lütfen {0} karşıdan yükleyip {1} dizine ayıklayın.</td></tr>
        <tr><td>XA5210</td><td>{0}. Özgün kitaplık dosyasının {1} yok.</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>XA6xxx Errors

<table>
    <thead>
        <tr><td>Hata Kodu</td><td>Açıklama</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>Çalışan CECIL sürümü çıkarma derleme desteklemiyor</td></tr>
        <tr><td>XA6002</td><td>Derleme Şerit değil `{0}`.</td></tr>
        <tr><td>XA6003</td><td>UnauthorizedAccessException ileti]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>XA9xxx Errors

Bu hata kodları, lisans ve etkinleştirme hatalardır.



#### <a name="xa9000"></a>XA9000

 **Neden:** lisansının süresi

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>UYARI</td>
            <td>UYARI</td>
            <td>UYARI</td>
            <td>UYARI</td>
            <td>UYARI</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9001"></a>XA9001

 **Neden:** deneme süresi

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>UYARI</td>
            <td>UYARI</td>
            <td>UYARI</td>
            <td>UYARI</td>
            <td>UYARI</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9002"></a>XA9002

 **Neden:** AndroidJavaSource

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>

 **Neden:** AndroidJavaLibrary

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>

 **Ardından bir bağlama derleme katıştırılmış .jar varsa, bu paket zaman, derleme zamanında yakalandı.**

 **Neden:** AndroidNativeLibrary

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9003"></a>XA9003

 **Neden:** System.Runtime.Serialization

 **Sırasında kullanıma:** paketi

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>

 **Cause:** System.ServiceModel.Web

 **Sırasında kullanıma:** paketi

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>

 **Neden:** Mono.Data.Tds

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>

 **Bu izin System.Data.dll tarafından başvuruluyor**

 **Neden:** Mono.Android.Export

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9004"></a>XA9004

 **Neden:** --profil oluşturma

 **Sırasında kullanıma:** paketi

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9005"></a>XA9005

 **Neden:** boyut sınırı (32 kb)

 **Sırasında kullanıma:** paketi

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>Tamam</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9006"></a>XA9006

 **Neden:** System.Data.SqlClient ad alanı

 **Sırasında kullanıma:** paketi

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9008"></a>XA9008

 **Neden:** komut satırından oluşturma

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>Tamam</td>
            <td>Tamam</td>
            <td>Tamam</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9009"></a>XA9009

 **Neden:** eksik seri numarası

 **Sırasında kullanıma:** Untestable

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9010"></a>XA9010

 **Neden:** geçersiz ProductID

 **Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
        </tr>
    </tbody>
</table>

XA9018 denktir



#### <a name="xa9011"></a>XA9011

 **Neden:** lisans dosyasına (yeni dosya biçimi) güncelleştirilemedi

 **Sırasında kullanıma:** etkinleştirme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9012"></a>XA9012

 **Neden:** Internet yok

 **Sırasında kullanıma:** etkinleştirme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9013"></a>XA9013

 **Neden:** bilinmeyen hata

 **Sırasında kullanıma:** etkinleştirme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9014"></a>XA9014

 **Neden:** geçersiz etkinleştirme kodu

 **Sırasında kullanıma:** etkinleştirme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9017"></a>XA9017

 **Neden:** Etkinleştirme sunucusu, geçerli bir lisans iade değil

 **Sırasında kullanıma:** etkinleştirme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
            <td>HATA</td>
        </tr>
    </tbody>
</table>


#### <a name="xa9018"></a>XA9018

**Neden:** lisansı geçersiz

**Sırasında kullanıma:** derleme

<table>
    <thead>
        <tr>
            <th>Başlatıcı</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>İş</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>HATA</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
