---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: d113e7747f656eda2bedb88574f4b2027616add8
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Hataları başvurusu

Bu belge, Xamarin çeşitli hata kodlarını bazı bilgiler sağlar.

|Kategori|Açıklama|
|--- |--- |
|XA0xxx|mandroid hataları|
|XA1xxx|Dosya kopyalama / simgesel bağlantı (ilgili proje) hataları|
|XA2xxx|Bağlayıcı hataları|
|XA3xxx|Uygulama Nesne AĞACI hataları|
|XA4xxx|Kod oluşturma hataları|
|XA5xxx|GCC ve araç zinciri hataları|
|XA6xxx|mandroid iç araçları hataları|
|XA7xxx|Ayrılmış|
|XA8xxx|Ayrılmış|
|XA9xxx|Lisans hataları|


## <a name="error-codes"></a>Hata Kodları

### <a name="xa0xxx-errors"></a>XA0xxx Errors

|Hata Kodu|Açıklama|
|--- |--- |
|XA0000|Beklenmeyen bir hata - Lütfen doldurun bir [hata raporu](http://bugzilla.xamarin.com).|
|XA0001|'-devname sağlanan herhangi bir cihaza özel eylem.|
|XA0002|'{0}' ortam değişkeni ayrıştırılamadı.|
|XA0003|Bir SDK veya ürün derlemesi (.dll) adıyla uygulama adı '{0} .exe' çakışıyor.|
|XA0004|Yeni refcounting mantık sgen çok etkin olmasını gerektirir.|
|XA0005|Çıktı dizini '{0}' mevcut değil.|
|XA0006|Hiçbir devel platformu olan '{0}' konumunda, kullanın--platform SDK belirtmek için PLAT =|
|XA0007|Kök derleme '{0}' mevcut değil.|
|XA0008|Yalnızca bir kök derleme sağlamalıdır.|
|XA0009|Derlemeleri yüklenirken hata oluştu: {0}.|
|XA0010|Komut satırı bağımsız değişkenleri ayrıştırılamadı: {0}.|
|XA0011|MonoTouch desteklediğinden daha {0} daha yeni bir çalışma ({1}) karşı oluşturuldu.|
|XA0012|Eksik veri '{0}' tamamlamak için sağlanır.|
|XA0013|Destek profil sgen çok etkin olmasını gerektirir.|
|XA0014|iOS {0} ARMv6 hedefleme uygulamaları oluşturma desteklemez.|
|XA0020|Mandroid yolu belirlenemedi.|
|XA0100|Android uygulama projesi EmbeddedNativeLibrary '{0}' geçersiz. Lütfen AndroidNativeLibrary kullanın.|

### <a name="xa1xxx-errors"></a>XA1xxx Errors

|Hata Kodu|Açıklama|
|--- |--- |
|XA1001|Bir uygulama belirtilen dizininde bulunamadı.|
|XA1002|Simgesel bağlantı oluşturulamadı, dosyalar kopyalandı.|
|XA1003|'{0}' uygulama KILL değil. Uygulamayı el ile KILL gerekebilir.|
|XA1004|Yüklü uygulamaların listesi alınamadı.|
|XA1005|Cihaza '{1}' '{0}' uygulamasının KILL değil: {2}. Uygulamayı el ile KILL gerekebilir.|
|XA1006|'{0}' uygulama '{1}' aygıtta yükleyemedi: {2}.|
|XA1007|'{0}' '{1}' aygıtta uygulaması başlatılamadı: {2}. Hala uygulamayı el ile üzerindeki e dokunabilirsiniz başlatabilirsiniz.|
|XA1008|Simulator başlatılamadı: {0}.|
|XA1009|'{0}' derleme '{1}' kopyalanamadı: {2}.|
|XA1010|'{0}' derlemesi yüklenemedi: {1}.|
|XA1011|Kaynak dosyası eksik eklenemedi: '{0}'.|
|XA1101|Uygulama başlatılamadı.|
|XA1102|(Bunu sonlandırmak için) uygulamaya eklenemedi: {0}.|
|XA1103|Ayrılamadı.|
|XA1104|Paket gönderme başarısız oldu: {0}.|
|XA1105|Beklenmeyen bir yanıt türü.|
|XA1106|Cihazda uygulamaların listesi alınamadı: İstek zaman aşımına uğradı.|
|XA1107|Uygulama başlatma başarısız oldu.|
|XA1201|Simulator yüklenemedi: {0}.|
|XA1301|Geçerli yapı architecture(s) ({2}) eşleşmediğinden yerel ' {0}' kitaplığı ({1}) yoksayıldı.|

### <a name="xa2xxx-errors"></a>XA2xxx Errors

|Hata Kodu|Açıklama|
|--- |--- |
|XA2001|Derlemeleri bağlanamadı.|
|XA2002|Başvuru çözümleyemiyor: {0}.|
|XA2003|Bağlama devre dışı olduğundan '{0}' seçeneği göz ardı edilir.|
|XA2004|'Ek bağlayıcı tanımları {0}' dosyası bulunamadı.|
|XA2005|'{0}' tanımları ayrıştırılamadı.|
|XA2006|Meta veri öğesi ('{1}' içinde tanımlanmış) ' {0}' başvuru '{2}' den çözümlenemedi.|

### <a name="xa3xxx-errors"></a>XA3xxx Errors

Bu, uygulama nesne AĞACI hatalardır.

|Hata Kodu|Açıklama|
|--- |--- |
|XA3001|AOT '{0}' derlemesi yüklenemedi.|
|XA3002|Uygulama Nesne AĞACI kısıtlama: [MonoPInvokeCallback] ile donatılmış olduğundan '{0}' yöntemi statik olmalıdır.|
|XA3003|Çakışan--hata ayıklama ve--llvm seçenekleri. Geçici hata ayıklama devre dışı bırakılır.|


### <a name="xa4xxx-errors"></a>XA4xxx Errors

Bu kod oluşturma hatalardır.

|Hata Kodu|Açıklama|
|--- |--- |
|XA4001|Ana Şablon '{0}' expansed bulunamadı.|
|XA4101|Kayıt türü '{0}' için imza oluşturamaz.|
|XA4102|Kayıt türü geçersiz '{0}' yöntemi '{2}' imzada bulundu. Bunun yerine '{1}' kullanın.|
|XA4103|Kayıt türü geçersiz '{0}' için imzası '{2}' bulundu: türü INativeObject uygular, ancak iki alan bir oluşturucu yok (IntPtr, bool) bağımsız değişkenler.|
|XA4104|Kayıt türü '{0}' için '{1}' imzası dönüş değeri sıralanamıyor.|
|XA4105|Kayıt türü '{0}' için '{1}' imzası parametresinin sıralanamıyor.|
|XA4106|Kayıt şirketi yapısında '{0}' için '{1}' imzası dönüş değeri sıralanamıyor.|
|XA4107|Kayıt türü '{0}' için '{1}' imzası parametresinin sıralanamıyor.|
|XA4108|Kayıt şirketi yönetilen türü '{0}' için ObjectiveC türü alınamıyor.|
|XA4109|Oluşturulan kayıt kodu derlemek başarısız oldu. Lütfen dosya bir [hata raporu](http://bugzilla.xamarin.com).|
|XA4110|Kayıt şirketi, için '{1}' imzası '{0}' türünde out parametresi sıralanamıyor.|
|XA4111|Kayıt türünün '{0}' yöntemi '{1}' imza oluşturamaz.|
|XA4200|Yalnızca ACW'ın 'claas' türleri için oluşturabilirsiniz.|
|XA4201|{0} türü için JNI adı belirlenemiyor.|
|XA4203|Belirtilen tür adı tam olarak nitelenmiş olmalıdır.|
|XA4204|Arabirim türü '{0}' çözümlenemiyor. Eksik bir derleme başvurunuz mu var?|
|XA4205|[ExportField] yalnızca 0 parametrelerle yöntemleri kullanılabilir.|
|XA4206|[Verme] bir genel türünde kullanılamaz.|
|XA4207|[ExportField] bir genel türünde kullanılamaz.|
|XA4208|[Java.Interop.ExportFieldAttribute] void döndüren bir yöntem kullanılamaz.|
|XA4209|Sınıfı için JavaTypeInfo oluşturulamadı: {1} nedeniyle {0}.|
|XA4210|ExportAttribute veya ExportFieldAttribute kullandığınızda Mono.Android.Export.dll bir başvuru eklemeniz gerekir.|
|XA4211|AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}' olan $(TargetFrameworkVersion) '{1}' den küçük. API - kullanma \ {1\} ACW derleme için.|


### <a name="xa5xxx-errors"></a>XA5xxx Errors

|Hata Kodu|Açıklama|
|--- |--- |
|XA5101|Eksik '{0}' derleyici. Android NDK yükleyin.|
|XA5102|Yerel kod derlemesinden dönüştürme işlemi başarısız oldu. Lütfen dosya bir [hata raporu](http://bugzilla.xamarin.com).|
|XA5103|'{0}' dosyası derlenemedi. Lütfen dosya bir [hata raporu](http://bugzilla.xamarin.com).|
|XA5201|Yerel bağlama başarısız oldu. Lütfen gcc için sağlanan kullanıcı bayraklarını gözden geçirin: {0}|
|XA5202|Yerel bağlama başarısız oldu. Lütfen yapı günlüğünü gözden geçirin.|
|XA5303|Uyarı bağlama yerel: {0}.|
|XA5300|Android SDK bulunamadı veya tam olarak yüklü.|
|XA5301|Aracı 'Şerit' eksik. Lütfen 'Komut satırı araçları' bileşenini yükleyin.|
|XA5302|Eksik 'dsymutil' aracı. Lütfen 'Komut satırı araçları' bileşenini yükleyin.|
|XA5203|Hata ayıklama simgeleri (dSYM dizin) oluşturulamadı. Lütfen yapı günlüğünü gözden geçirin.|
|XA5204|Son ikili dizeden başarısız oldu. Lütfen yapı günlüğünü gözden geçirin.|
|XA5205|Eksik 'aapt' aracı. Lütfen Android SDK derleme araçları paketini yükleyin.|
|XA5206|{0}. Android kaynak dizini {1} yok.|
|XA5207|{0}. Java kitaplığı dosya {1} yok.|
|XA5208|Yükleme başarısız oldu. Lütfen {0} indirin ve {1} dizinine yerleştirin.|
|XA5209|Şunu başarısız oldu. Lütfen {0} karşıdan yükleyip {1} dizine ayıklayın.|
|XA5210|{0}. Özgün kitaplık dosyasının {1} yok.|

### <a name="xa6xxx-errors"></a>XA6xxx Errors

|Hata Kodu|Açıklama|
|--- |--- |
|XA6001|CECIL çalışan sürümünü çıkarma derleme desteklemiyor.|
|XA6002|'{0}' derlemesi Şerit değil.|
|XA6003|UnauthorizedAccessException iletisi.|

### <a name="xa9xxx-errors"></a>XA9xxx Errors

Bu hata kodları, lisans ve etkinleştirme hatalardır.


#### <a name="xa9000"></a>XA9000

 **Neden:** lisansının süresi

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|UYARI|UYARI|UYARI|UYARI|UYARI|


#### <a name="xa9001"></a>XA9001

 **Neden:** deneme süresi

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|UYARI|UYARI|UYARI|UYARI|UYARI|



#### <a name="xa9002"></a>XA9002

 **Neden:** AndroidJavaSource

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|Tamam|Tamam|Tamam|Tamam|

 **Neden:** AndroidJavaLibrary

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|Tamam|Tamam|Tamam|Tamam|

 **Ardından bir bağlama derleme katıştırılmış .jar varsa, bu paket zaman, derleme zamanında yakalandı.**

 **Neden:** AndroidNativeLibrary

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|Tamam|Tamam|Tamam|Tamam|


#### <a name="xa9003"></a>XA9003

 **Neden:** System.Runtime.Serialization

 **Sırasında kullanıma:** paketi

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|Tamam|Tamam|Tamam|

 **Cause:** System.ServiceModel.Web

 **Sırasında kullanıma:** paketi

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|Tamam|Tamam|Tamam|

 **Neden:** Mono.Data.Tds

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|Tamam|Tamam|Tamam|

 **Bu izin System.Data.dll tarafından başvuruluyor**

 **Neden:** Mono.Android.Export

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|Tamam|Tamam|Tamam|Tamam|



#### <a name="xa9004"></a>XA9004

 **Neden:** --profil oluşturma

 **Sırasında kullanıma:** paketi

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|Tamam|Tamam|Tamam|



#### <a name="xa9005"></a>XA9005

 **Neden:** boyut sınırı (32 kb).

 **Sırasında kullanıma:** paketi

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|Tamam|-|-|-|



#### <a name="xa9006"></a>XA9006

 **Neden:** System.Data.SqlClient ad.

 **Sırasında kullanıma:** paketi

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|Tamam|Tamam|Tamam|


#### <a name="xa9008"></a>XA9008

 **Neden:** komut satırından oluşturma.

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|Tamam|Tamam|Tamam|


#### <a name="xa9009"></a>XA9009

 **Neden:** eksik seri numarası.

 **Sırasında kullanıma:** Untestable

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|HATA|HATA|HATA|


#### <a name="xa9010"></a>XA9010

 **Neden:** geçersiz ProductID.

 **Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|HATA|HATA|HATA|

XA9018 eşdeğerdir.



#### <a name="xa9011"></a>XA9011

 **Neden:** lisans dosyasına (yeni dosya biçimi) güncelleştirmesi başarısız oldu.

 **Sırasında kullanıma:** etkinleştirme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|HATA|HATA|HATA|

#### <a name="xa9012"></a>XA9012

 **Neden:** Internet yok

 **Sırasında kullanıma:** etkinleştirme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|HATA|HATA|HATA|


#### <a name="xa9013"></a>XA9013

 **Neden:** bilinmeyen hata

 **Sırasında kullanıma:** etkinleştirme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|HATA|HATA|HATA|


#### <a name="xa9014"></a>XA9014

 **Neden:** geçersiz etkinleştirme kodu

 **Sırasında kullanıma:** etkinleştirme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|HATA|HATA|HATA|


#### <a name="xa9017"></a>XA9017

 **Neden:** Etkinleştirme sunucusu, geçerli bir lisans iade değil.

 **Sırasında kullanıma:** etkinleştirme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|HATA|HATA|HATA|HATA|HATA|


#### <a name="xa9018"></a>XA9018

**Neden:** lisansı geçersiz

**Sırasında kullanıma:** derleme

|Başlatıcı|Indie|Business(trial)|İş|Enterprise|
|--- |--- |--- |--- |--- |
|-|-|HATA|-|-|

