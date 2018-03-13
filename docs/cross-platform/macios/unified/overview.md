---
title: "Birleşik API genel bakış"
description: "Yeni stil API zamankinden kod Mac ve iOS yanı sıra aynı 32 ve 64 bit uygulamalarını desteklemek ikili sağlayarak arasında paylaşmak için kolaylaştırır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 21245d741ff025cb8c2a680642ec0226369540cb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="unified-api-overview"></a>Birleşik API genel bakış

_Yeni stil API zamankinden kod Mac ve iOS yanı sıra aynı 32 ve 64 bit uygulamalarını desteklemek ikili sağlayarak arasında paylaşmak için kolaylaştırır._

İOS ve Mac arasında paylaşımı kodu geliştirmek ve 32 ve 64 bit üzerinde çalışan tek bir kod temel sağlamak, geliştiricilerin için erken 2015 Xamarin.Mac ve Xamarin.iOS ürünlerinde Unified API adlı yeni bir API gösterdiğimizi.

> [!IMPORTANT]
> **Klasik profili kullanımdan:** yeni platformlar Xamarin.iOS içinde eklendikçe biz kademeli olarak Klasik profilinden (monotouch.dll) özellikleri alanı onaylanamadı başladılar. Örneğin, NRC olmayan (yeni ref-sayısı) seçeneği kaldırıldı. NRC tüm birleştirilmiş uygulamalar için her zaman etkinleştirildi (hiçbir zaman bir seçenek, yani olmayan NRC) ve bilinen bir sorun vardır. Gelecek sürümlerde Boehm atık toplayıcı kullanma seçeneğini kaldırır. Ayrıca, bu birleşik uygulamalara hiçbir zaman kullanılabilen bir seçenektir. Tam temizleme Klasik destek Xamarin.iOS 10.0 sürümü ile sonraki sonbaharda için zamanlandı.

## <a name="ios"></a>iOS

`Xamarin.iOS.dll` Xamarin.iOS 8.6 ile birlikte gelen derleme bizim **ilk kararlı ve desteklenen sürüm** iOS için birleşik API.
Birleşik API önceki Önizleme sürümleri Kapat ancak tamamen uyumlu değil.

## <a name="mac"></a>Mac

`Xamarin.Mac.dll` , Kararlı kanal Xamarin.Mac derleme bizim **ilk kararlı ve desteklenen sürüm** Mac için birleşik API
Birleşik API önceki Önizleme sürümleri Kapat ancak tamamen uyumlu değil.

## <a name="runtime-defaults"></a>Çalışma zamanı Varsayılanları

Varsayılan kullanımlar tarafından Unified API **SGen** atık toplayıcısını ve [yeni başvuru sayımı](~/ios/internals/newrefcount.md) sistemi nesnenin sahipliğini izleme için. Aynı özellik Xamarin.Mac için bağlantı noktalı.

Bu, geliştiricilerin eski sistemiyle karşılaştığı ve ayrıca kolaylaştırır sorunlardan bazıları çözdü [bellek yönetimi](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Yeni Refcount etkinleştirmek için bile Klasik API mümkündür, ancak varsayılan koruyucu ve kullanıcıların herhangi bir değişiklik gerektirmez unutmayın. Birleşik API ile biz varsayılan değiştirme Fırsatı sürdü ve geliştiricilerin tüm geliştirmeleri aynı anda verin yeniden düzenlemeniz ve kendi kodlarını yeniden sınayın.

<a name="namespace-changes" />

## <a name="library-split"></a>Kitaplık Böl

Bu noktadan itibaren bizim API'leri iki yolla kullanıma sunulur:

-  **Klasik API:** 32-bit (yalnızca) sınırlı ve içinde sunulan `monotouch.dll` ve `XamMac.dll` derlemeler.
-  **Birleşik API:** desteği bulunan tek bir API ile 32 ve 64 bit geliştirme `Xamarin.iOS.dll` ve `Xamarin.Mac.dll` derlemeler.

Başka bir deyişle, kuruluş için yeni API'ları biz bunları sonsuza kadar veya koruma tutmak şekilde mevcut Klasik API'lerini kullanmaya devam edebilirsiniz geliştiriciler (değil atamak uygulama mağazası), yükseltme yapabilirsiniz.

### <a name="namespace-changes"></a>Namespace değişiklikleri

Kod Mac ve iOS Ürünlerimiz arasında paylaşmak için uyuşmazlık azaltmak için biz ad alanları API'leri ürünler için değiştiriyorsunuz.

Biz öneki "MonoTouch" bizim iOS ürün ve "MonoMac" bizim Mac ürünün veri türleri üzerinde atar.

Bu kod koşullu derleme yeniden ayırma olmadan Mac ve iOS platformlarında arasında paylaşmak daha basit hale getirir ve kaynak kodu dosyaları üstündeki gürültü azaltır.

-  **Klasik API:** ad alanları kullanmak `MonoTouch.` veya `MonoMac.` öneki.
-  **Birleşik API:** herhangi bir ad alanı öneki

### <a name="api-changes"></a>API değişiklikleri

Kullanım dışı yöntemler Unified API kaldırır ve birkaç örneği vardır vardı burada yazım hatalarını API adlarında özgün MonoTouch ve MonoMac ad alanlarına Klasik API'lerde bağlanmış olduğunda. Bu örnekler, yeni birleşik API'lerinde düzeltildi ve ve bileşen, iOS ve Mac uygulamalarınızda güncelleştirilmesi gerekir. İçine çalışabilir en yaygın olanları listesi aşağıdadır:

<table width="100%" border="1">
<tr>
    <th>Klasik API yöntem adı</th>
    <th>Birleşik API yöntem adı</th>
</tr>
<tr>
    <td>UINavigationController.PushViewControllerAnimated()</td>
    <td>UINavigationController.PushViewController()</td>
</tr>
<tr>
    <td>UINavigationController.PopViewControllerAnimated()</td>
    <td>UINavigationController.PopViewController()</td>
</tr>
<tr>
    <td>CGContext.SetRGBFillColor()</td>
    <td>CGContext.SetFillColor()</td>
</tr>
<tr>
    <td>NetworkReachability.SetCallback()</td>
    <td>NetworkReachability.SetNotification()</td>
</tr>
<tr>
    <td>CGContext.SetShadowWithColor</td>
    <td>CGContext.SetShadow</td>
</tr>
<tr>
    <td>UIView.StringSize</td>
    <td>UIKit.UIStringDrawing.StringSize</td>
</tr>
</table>

Birleşik API'sine Klasikten geçiş yaparken değişiklikleri tam bir listesi için lütfen bkz bizim [Klasik (monotouch.dll) vs Unified (Xamarin.iOS.dll) API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) belgeleri.

### <a name="updating-to-unified"></a>Çok birleşik güncelleştiriliyor

Bazı eski/bozuk/kullanım API **Klasik** kullanılamayan **Unified** API. Düzeltmek daha kolay olabilir `CS0616` , (el ile veya otomatik) başlatmadan önce uyarıları yükseltme sahip olacaksınız beri `[Obsolete]` sağ API için kılavuzluk etmesi için ileti (uyarı parçası) özniteliği.

Biz yayımlama Not bir [ *fark* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) önce veya sonra proje güncelleştirmeleri kullanılabilir API değişiklikleri Klasik vs birleşik. Hala düzelttikten olması zaman tasarrufu (daha az belgelerine aramaları) Klasik çağrılarında genellikle obsoletes.

Bu yönergeleri izleyin [var olan iOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-ios-apps.md), veya [Mac uygulamaları](~/cross-platform/macios/unified/updating-mac-apps.md) Unified API.
Bu sayfanın kalanını gözden geçirin ve [bu ipuçlarını](~/cross-platform/macios/unified/updating-tips.md) kodunuzu geçirme hakkında ek bilgi için.

## <a name="components-and-nuget"></a>Bileşenleri ve NuGet

Çoğu varolan bileşenleri ve NuGet paketleri Unified API destekleyecek şekilde güncelleştirilmesi gerekir.
Klasik API karşı yerleşik bileşenleri bir birleşik API projesinde başvurulamaz.

Tüm başvuruları sahip olmaması var olan bileşenlerin `monotouch.dll` (veya `XamMac.dll`) güncelleştirmeleri gerekmez.

### <a name="components"></a>Bileşenler

iOS bileşenlerinde [Xamarin bileşen Deposu'nda](https://components.xamarin.com/) Unified API projelerle çalışmak için güncelleştirilmesi gerekir. Xamarin yayımlamak ve aynı işlemi gerçekleştirmek için diğer yazarlar teşvik bileşenleri güncelleştirmeye çalışıyor.

### <a name="nuget"></a>NuGet

Daha önce Klasik API aracılığıyla Xamarin.iOS desteklenen NuGet paketlerini kullanarak kendi derlemeler yayımlanan **Monotouch10** platform ad.

Birleşik API uyumlu paketler için-yeni bir platform tanımlayıcısı tanıtır **Xamarin.iOS10**. Mevcut NuGet paketleri, bu platform desteği eklemek için birleşik API'sine karşı oluşturmaya güncelleştirilmesi gerekir.

> [!IMPORTANT]
> **Not:** biçiminde bir hata varsa _"hatası 3 'monotouch.dll' ve 'Xamarin.iOS.dll' aynı Xamarin.iOS projede içeremez - 'Xamarin.iOS.dll' 'monotouch.dll' tarafından başvurulan sırada açıkça başvurulmaktadır ' xxx Sürüm 0.0.000, Culture = neutral, PublicKeyToken = null ='"_ Unified API uygulamanıza dönüştürdükten sonra genellikle bir bileşen veya NuGet paketi birleşik API'sine güncelleştirilmemiş projesinde sahip nedeniyle istenir. Varolan bileşeni/NuGet kaldırmak, birleşik API'lerini destekleyen bir sürüme güncelleştirmek ve temiz bir yapı yapmanız gerekir.

<a name="deprecated-apis" />

## <a name="arrays-and-systemcollectionsgeneric"></a>Diziler ve System.Collections.Generic

C# dizin oluşturucular türü beklediğiniz çünkü `int`, açıkça cast gerekecek `nint` değerler `int` bir koleksiyon ya da dizi öğeleri erişmek için. Örneğin:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Bu beklenen bir davranış çünkü dönüştürme `int` için `nint` olan kayıplı 64-bit üzerinde örtük bir dönüştürme yok yapılır.

## <a name="converting-datetime-to-nsdate"></a>DateTime için NSDate dönüştürme

Örtük dönüştürme birleşik API'leri kullanırken `DateTime` için `NSDate` değerleri artık gerçekleştirilir. Bu değerleri bir türden diğerine açıkça dönüştürülmesi gerekir. Bu işlemi otomatikleştirmek için aşağıdaki genişletme yöntemleri kullanılabilir:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

## <a name="deprecated-apis-and-typos"></a>Kullanım dışı API'ları ve yazım hatalarını

İç Xamarin.iOS Klasik API (monotouch.dll) `[Obsolete]` özniteliği, iki farklı şekilde kullanıldı:

-  **İOS API Kullanım:** size yeni bir tarafından bırakan bir API kullanarak durdurmak için Apple ipuçları durumunda olur. Hala sorun ve genellikle gerekli Klasik API (IOS eski sürümü destekliyorsa).
 Bu tür API (ve `[Obsolete]` özniteliği) yeni Xamarin.iOS derlemelerine dahil edilir.
-  **Yanlış API** bazı API adlarını yazım hatalarını vardı.

Özgün derlemelerde (monotouch.dll ve XamMac.dll) biz uyumluluk için kullanılabilir eski kod tutulur, ancak birleşik API derlemelerden (Xamarin.iOS.dll ve Xamarin.Mac) kaldırıldı

<a name="NSObject_ctor" />

## <a name="nsobject-subclasses-ctorintptr"></a>NSObject alt sınıfların .ctor(IntPtr)

Her `NSObject` alt kabul eden bir oluşturucuya sahip bir `IntPtr`. Biz yerel ObjC işleyici yeni bir yönetilen örneğinden nasıl örneği budur.

Klasik, bu bir `public` Oluşturucusu. Örneğin birkaç oluşturma örnekleri tek bir ObjC örneği için bu özelliği kullanıcı kodu kötüye kolay ancak yönetilen *veya* yönetilen bir örneği oluşturma (için alt sınıfların) beklenen yönetilen durumu olmaması.

Bu tür sorunları önlemek için `IntPtr` oluşturucular olan şimdi `protected` içinde **birleştirilmiş** yalnızca sınıflara için kullanılacak API. Bu düzeltme/güvenli API yönetilen örneği tanıtıcısı, yani oluşturmak için kullanılan garanti eder

    var label = Runtime.GetNSObject<UILabel> (handle);

Bu API (varsa) var olan bir yönetilen örneği döndürür veya (gerektiğinde) yeni bir tane oluşturun. Hem Klasik hem de birleşik API zaten mevcuttur.

Unutmayın `.ctor(NSObjectFlag)` de sunulmuştur `protected` ancak bunun dışında sınıflara nadiren kullanıldı.

<a name="NSAction" />

## <a name="nsaction-replaced-with-action"></a>NSAction eylemiyle değiştirildi

Birleşik API'leri ile `NSAction` lehinde standart .NET kaldırıldı `Action`. Bunun nedeni büyük bir geliştirme, `Action` ise ortak bir .NET türü olan `NSAction` Xamarin.iOS için belirli oluştu. Her ikisi de tam olarak aynı işlevi görür, ancak farklı ve uyumlu türleri olan ve aynı sonucu elde etmek üzere yazılmış gerek kalmadan daha koda sonuçlandı.

Örneğin, mevcut Xamarin uygulamanızda aşağıdaki kodu içeriyorsa:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Şimdi ile basit bir lambda değiştirilebilir:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Daha önce olacaktır derleyici hatası nedeniyle bir `Action` atanamaz `NSAction`, ancak `UITapGestureRecognizer` şimdi geçen bir `Action` yerine bir `NSAction` birleşik API'lerinde geçerli değil.

## <a name="custom-delegates-replaced-with-actiont"></a>Eylemiyle değiştirilen özel temsilciler<T>

İçinde **birleştirilmiş** bazı basit (örn. bir parametre) .net temsilciler yerine `Action<T>`. Örneğin

    public delegate void NSNotificationHandler (NSNotification notification);

artık olarak kullanılabilir bir `Action<NSNotification>`. Bu yükseltme kodu yeniden ve Xamarin.iOS ve kendi uygulamalarınız içinde kod yinelemesinden azaltır.

## <a name="taskbool-replaced-with-taskbooleannserror"></a>Görev<bool> < Boole, NSError >> Görev ile değiştirilir

İçinde **Klasik** bazı zaman uyumsuz API'leri vardı döndürme `Task<bool>`. Ancak bazı bunların nerede kullanılacak olan bir `NSError` yani imza parçası olan `bool` zaten `true` ve almak için bir özel durum catch gerekiyordu `NSError`.

Bazı yaygın hatalardır ve dönüş değeri yararlı değildi olduğundan bu deseni olarak değiştirildi **birleştirilmiş** döndürmek için bir `Task<Tuple<Boolean,NSError>>`. Bu, hem başarılı hem de zaman uyumsuz çağrı sırasında olmuş olabilir herhangi bir hata denetlemenizi sağlar.

## <a name="nsstring-vs-string"></a>NSString vs dize

Bazı durumlarda bazı sabitleri değiştirildiği gerekiyordu `string` için `NSString`, örn. `UITableViewCell`

**Klasik**

    public virtual string ReuseIdentifier { get; }

**Birleşik**

    public virtual NSString ReuseIdentifier { get; }

Genel biz .NET tercih `System.String` türü. Ancak, Apple yönergelerini rağmen bazı yerel API karşılaştırma sabit işaretçileri (dizesinin kendisini) ve biz sabitleri olarak kullanıma bu yalnızca çalışabilirsiniz `NSString`.

 <a name="protocols" />

## <a name="objective-c-protocols"></a>Objective-C protokolleri

Özgün MonoTouch tam destek ObjC protokolleri ve bazı, en iyi olmayan sahip değil, API eklendi en yaygın senaryoyu desteklemek için. Bu sınırlama artık yok ancak geriye dönük uyumluluk için geçici API'leri tutulur içinde `monotouch.dll` ve `XamMac.dll`.

Bu sınırlamalara kaldırıldı ve birleşik API'leri temizlendi. Çoğu değişiklikleri şuna benzeyecektir:

**Klasik**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Birleşik**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

`I` Önek anlamına gelir **birleştirilmiş** ObjC protokolü için belirli bir tür yerine bir arabirimi kullanıma sunar. Bu, burada, bir alt kümesi için Xamarin.iOS sağlanan belirli türü istemediğiniz durumlarda kolaylaştıracak.

Ayrıca, bazı API daha kesin ve kullanımı kolay, örneğin olmasını izin:

**Klasik**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Birleşik**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Bu API artık bize için daha kolay belgelerine başvuruyor olmadan, ve IDE kod tamamlama protokolü/arabirimi esas alan daha kullanışlı önerileri sağlayacaktır.

### <a name="nscoding-protocol"></a>NSCoding Protokolü

Bunu desteklemediği olsa bile bir .ctor(NSCoder) her türünün - bizim özgün bağlama dahil `NSCoding` protokolü.  Tek bir `Encode(NSCoder)` yöntemi mevcut `NSObject` nesne kodlamak için.
Ancak bu yöntem yalnızca örnek NSCoding protokolü için uygun hale varsa çalışır.

Birleşik API Biz bu sabit.  Yeni derlemeleri yalnızca sahip `.ctor(NSCoder)` türü için uyuyorsa `NSCoding`. Bu tür türleri şimdi de bir `Encode(NSCoder)` uyan yöntemi `INSCoding` arabirimi.

Düşük etkisi: eski, kaldırılan, Oluşturucular kullanılmaması gibi çoğu durumda bu değişiklik uygulamaları etkilemez.

## <a name="further-tips"></a>Daha fazla ipuçları

Dikkat edilmesi gereken ek değişiklikler içinde listelenen [Unified API uygulamaları güncelleştirmek için ipuçları](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Örnek kod

31 Temmuz itibariyle şu bağlantı noktalarını bu yeni API iOS örnekleri üzerinde yayımladığınız `magic-types` adresindeki dal [monotouch örnekleri](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Mac için şu örnekleri hem de denetleniyor [mac-samples](https://github.com/xamarin/mac-samples) (yeni API'leri Mavericks/Yosemite gösteren) deposu ve bunun yanı sıra 32/64 bit örnekleri Sihirli türleri dal [mac-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## <a name="related-links"></a>İlgili bağlantılar

- [İOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Güncelleştirme bağlamaları](~/cross-platform/macios/unified/update-binding.md)
- [Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](~/cross-platform/macios/native-types-cross-platform.md)
- [İpuçları güncelleştiriliyor](~/cross-platform/macios/unified/updating-tips.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
