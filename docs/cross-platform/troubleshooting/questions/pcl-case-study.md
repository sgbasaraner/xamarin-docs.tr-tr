---
title: System.Diagnostics.Tracing ve TPL veri akışı ile ilgili sorunları gidermek
description: 'PCL örnek olay incelemesi: nasıl ı çözümleyebildiğinden System.Diagnostics.Tracing için Microsoft TPL veri akışı NuGet paketi için ilgili sorunları?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: 1acc9ccc78ad14198a59e74d1fae845790d66b16
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33918532"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL örnek olay incelemesi: nasıl ı çözümleyebildiğinden System.Diagnostics.Tracing için Microsoft TPL veri akışı NuGet paketi için ilgili sorunları?

> [!IMPORTANT]
> Bu belirli örneği `System.Diagnostic.Tracing` artık varsayılan olarak en son sürümlerini Xamarin herhangi bir hata oluşturur. Önerilen çözüm çalışmaya devam eder ancak bazı "katmanları hataların" bölümünde belirtilen hatalar düzeltildi olduğunu unutmayın.
> Ayrıca, .NET standart artık platformlar arası .NET API'leri uygulama, tercih edilen yol olduğunu unutmamalısınız.

## <a name="summary"></a>Özet

Xamarin.iOS ve Xamarin.Android % 100'başvuru olarak izin her PCL profili kullanılmaz. Mac, Visual Studio ve NuGet Paket Yöneticisi için Visual Studio'da pratik kolaylık olması için Xamarin projeleri yalnızca birkaç profil kullanılmasına izin _tamamlanmamış_ uygulamaları. Örneğin, Xamarin.iOS ne Xamarin.Android şu anda "System.Diagnostics.Tracing" PCL türlerini tam bir uygulama içeren ad alanı. Bu sınırlama varsayılan kullanmaya çalışırken hataları üç katmanlara müşteri adayları `portable-net45+win8+wpa81` Microsoft TPL veri akışı NuGet paketi sürümü.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Geçici çözüm: başvurmak için uygulama projesi geçiş `portable-net45+win8+wp8+wpa81` TPL veri akışı kitaplığı sürümü

(Bu, hataları ve Xamarin tüm yeni sürümlerinde çalışan tüm üç katmanı önler.)

1. Uygulama projesini açın **.csproj** dosyasını bir metin düzenleyicisinde.

2. Şuna benzer satırı bulun:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Değişiklik `portable-net45+win8+wpa81` için `portable-net45+win8+wp8+wpa81` (`+wp8` eklenir):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Açıklama

`portable-net45+win8+wp8+wpa81` Kitaplığının sürümü başvurmuyor **System.Diagnostics.Tracing.dll** _hiç_, tüm üç katmanı sorunları tamamen ortadan kaldırır.

### <a name="limitations"></a>Sınırlamalar

- `portable-net45+win8+wp8+wpa81` Kitaplığın sürüm içermiyor olabilir işlevselliğini % 100 `portable-net45+win8+wpa81` sürümü.

- NuGet Paket Yöneticisi yükler `portable-net45+win8+wpa81` başvuru el ile ayarlamak için varsayılan olarak, PCL NuGet paketi sürümü.

## <a name="details-about-the-three-layers-of-errors"></a>Üç katmanı hatalarının ayrıntıları

1. **System.Diagnostics.Tracing.dll** cephesi derleme şu anda tüm Mac sürümlerinden Xamarin.Android (genel hata 34888) yok ve yok sürümlerinden tüm Xamarin.iOS 9.0 değerinden daha düşük (veya XamarinVS 3.11.1443 değerinden daha düşük Windows) (sabit [hata 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Bu sorun dağıtım hedefini ve bağlayıcı bağlı olarak aşağıdaki hatalardan biri ayarları neden olur:

    - Xamarin.Android.Common.targets: Hata: derlemeleri yüklenirken özel durum: System.IO.FileNotFoundException: Derleme yüklenemedi ' System.Diagnostics.Tracing, sürüm 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a ='. Belki de Android profili Mono içinde yok?

    - Dosya veya derleme 'System.Diagnostics.Tracing' ya da bağımlılıklarından biri yüklenemedi. Sistem belirtilen dosyayı bulamıyor. (System.IO.FileNotFoundException)

    - MTOUCH: hata MT3001: AOT derlemesi yüklenemedi ' / Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll '

    - MTOUCH: hata MT2002: derleme çözülemedi: ' System.Diagnostics.Tracing, sürüm 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a '

2. Geçerli ["System.Diagnostics.Tracing" türlerinde Mono uyarlamasını](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) bazı yöntemi aşırı eksik ([hata 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Bu sorun aşağıdaki bağlayıcı hatalardan biri Xamarin uygulaması oluştururken neden olur:

    - / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: hata: LinkAssemblies Görev yürütülürken hata oluştu: hata XA2006: meta veri öğesi Başvurusu ' System.Void System.Diagnostics.Tracing.EventSource:: WriteEvent(System.Int32,System.Object[])' (tanımlanan ' System.Threading.Tasks.Dataflow, sürüm 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a ') gelen ' System.Threading.Tasks.Dataflow, sürüm 4.5.24.0, Culture = neutral, = PublicKeyToken b03f5f7f11d50a3a ' çözümlenemedi.

    - MTOUCH: MT2002 hata: "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" başvurusundan çözümlenemedi "System.Diagnostics.Tracing, sürüm 4.0.0.0, Culture = neutral, PublicKeyToken = = b03f5f7f11d50a3a"

3. Geçerli ["System.Diagnostics.Tracing" türlerinde Mono uyarlamasını](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) da geçerli olan bir _boş_ "kukla" uygulama ([hata 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Bu yöntemler çalışma zamanında kullanılacak her türlü girişim, bu nedenle beklenmeyen sonuçlara yol açabilir. İçin _belirli_ durumda çağrıları göründüğü Microsoft TPL veri akışı Kitaplığı'nın `WriteEvent(System.Int32,System.Object[])` kitaplığın davranışı, çoğu için gerekli olmayan böylece "Katman 2" için düzeltme ([hata 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), boş ekleme uygulamaları), büyük olasılıkla çoğu Microsoft TPL veri akışı kullanım durumları için yeterli olacaktır.

## <a name="questions--answers"></a>Sorular ve yanıtlar

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>I etkinleştirilmiş olan bağlama bırakın kurtararak <code>portable-net45+win8+wpa81</code> Xamarin.iOS eski sürümleri ya da Xamarin.Android kitaplığı sürümü. Nasıl o çalıştı mı?

#### <a name="answer"></a>Yanıt

Bu _olası_ başvuru eklerseniz, derleme için tam "başarılı" (etkin bağlama ile) Xamarin.iOS eski sürümleri veya Mac üzerinde Xamarin.Android almak için `System.Diagnostics.Tracing.dll` _derlemebaşvurusu_ \[1\] yerine _cephesi derleme_ \[2], ancak ne yazık ki bu "doğru" geçici bir çözüm değildir. Başvuru derlemeleri yalnızca oluştururken kullanılacak amacı _taşınabilir kitaplıklara_, platforma özgü kod uygulamaları gibi. Çalışırken _çalıştırmak_ başvuru derlemeleri (yalnızca derleme karşısında yerine) içinde yer alan kodu beklenmeyen sonuçlara neden olabilir. Doğru düzeltme eksik eklemek için Mono ekibi olacaktır `WriteEvent(System.Int32,System.Object[])` için aşırı [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) türü ([hata 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Şimdi en iyi seçenek geçmek için `portable-net45+win8+wp8+wpa81` yukarıdaki geçici çözüm bölümünde anlatıldığı gibi Microsoft TPL veri akışı kitaplığı sürümü.

(Bu makalede ilgili daha eski, briefer yanıt StackOverflow gelen gördükten sonra okuma herkes için (<http://stackoverflow.com/a/23591322/2561894>), başvuru derlemeleri ve cephesi derleme arasında ayrım edildi Not _değil_ var. belirtilen.)

**\[1\] "derleme başvurusu" konumları**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "Cephesi derleme" konumları**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>El ile "System.Diagnostics.Tracing" cephesi derlemesine başvuru eklemek, yardımcı olur?

_Özellikle 2 adımları kullanarak sorunu çözebilir?_

1. _Kopya `System.Diagnostics.Tracing.dll` uygulama proje klasöründen aşağıdaki konumlardan birinde cephesi derlemeye:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Xamarin.iOS veya Xamarin.Android uygulaması projesinde cephesi derlemesine başvuru ekleyin._

#### <a name="answer"></a>Yanıt

Hayır, bu yardımcı olmayacaktır.

- Xamarin.iOS 9.0 veya herhangi yeni bir Xamarin.Android Windows sürümü için bu geçici çözüm kesinlikle gereksiz ve "aynı kimliğe sahip bir derleme 'System.Diagnostics.Tracing' önceden içeri aktarıldı için." benzer derleme hatalara neden olabilir.

- Xamarin.iOS 8.10 veya daha düşük veya Mac üzerinde Xamarin.Android için bu geçici çözüm yardımcı olur, ancak _yalnızca_ "Katman 1" eksik derleme sorun için. İçinde _değil_ eksiksiz bir çözüm değildir "Katman 2" bağlayıcı hatalarını çözdü.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Kullanabilirim [System.Diagnostics.Tracing NuGet paketi](https://www.nuget.org/packages/System.Diagnostics.Tracing/) sorunu çözmek için?

#### <a name="answer"></a>Yanıt

Hayır, NuGet 3.0 "System.Diagnostics.Tracing" paketi, yalnızca "DNXCore50" ve "netcore50" için platforma özel uygulamaları içerir. Bunu açıkça _atlar_ uygulamaları Xamarin.Android ("MonoAndroid") ve Xamarin.iOS ("MonoTouch" ve "xamarinios"). Bu paketi yüklerken olacağı anlamına gelir _hiçbir etkisi_ Xamarin.Android ve Xamarin.iOS projeleri için. Bu platformlar her ikisi de sağladığını NuGet paketi varsayar kendi _kendi_ uygulama türleri. Bu "Mono sahip anlamda doğru" varsayılır _bir_ uygulama ad alanı, ancak altında ele noktaları olarak \#2 ve \#"Ayrıntılarını hataların üç katmanı" yukarıda 3 Uygulama şu anda eksik. Mono takım çözümlemek uygun düzeltme olacak [hata 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) ve [hata 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890).

## <a name="next-steps"></a>Sonraki Adımlar

Bizimle iletişime geçin veya bile Yukarıdaki bilgilerin kullanılarak sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [için Xamarin hangi destek seçenekleri kullanılabilir?](~/cross-platform/troubleshooting/support-options.md) önerileri, iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Yeni bir hatanın gerekirse dosya.
