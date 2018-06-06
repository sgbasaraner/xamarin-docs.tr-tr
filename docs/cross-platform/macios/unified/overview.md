---
title: Birleşik API genel bakış
description: Xamarin'ın birleşik API kod Mac ve iOS ve Destek 32 ve 64-bit uygulamalarla aynı ikili arasında paylaşılmasını olanaklı kılar.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d54d31019f04dc28b9b85a13a0a93f85abc0be75
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782001"
---
# <a name="unified-api-overview"></a>Birleşik API genel bakış

Xamarin'ın birleşik API kod Mac ve iOS ve Destek 32 ve 64-bit uygulamalarla aynı ikili arasında paylaşılmasını olanaklı kılar. Birleşik API varsayılan olarak yeni Xamarin.iOS ve Xamarin.Mac projelerinde kullanılır.

> [!IMPORTANT]
> Xamarin Klasik hangi Unified API öncesinde, API, kullanım dışı bırakıldı. 
> - Klasik API (monotouch.dll) desteklemek için Xamarin.iOS son sürümü Xamarin.iOS 9.10 oluştu.
> - Xamarin.Mac hala Klasik API destekler, ancak artık güncelleştirilir. Kullanım dışı bırakılmıştır olduğundan, geliştiriciler uygulamalarını Unified API taşımanız gerekir.

## <a name="updating-classic-api-based-apps"></a>Klasik API tabanlı uygulamaları güncelleştirme

Platformunuz için ilgili yönergeleri izleyin:

- [Mevcut Uygulamaları Güncelleştirme](updating-apps.md)
- [Mevcut iOS Uygulamalarını Güncelleştirme](updating-ios-apps.md)
- [Mevcut Mac Uygulamalarını Güncelleştirme](updating-mac-apps.md)
- [Mevcut Xamarin.Forms Uygulamalarını Güncelleştirme](updating-xamarin-forms-apps.md)
- [Bir Bağlamayı Unified API’ye Geçirme](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[Kodu Unified API’ye Güncelleştirmeye İlişkin İpuçları](updating-tips.md)

Hangi uygulamaların bağımsız olarak geçirdiğiniz, kullanıma [bu ipuçlarını](updating-tips.md) başarıyla birleşik API'sine güncelleştirmenize yardımcı olması için.

## <a name="library-split"></a>Kitaplık Böl

Bu noktadan itibaren bizim API'leri iki yolla kullanıma sunulur:

-  **Klasik API:** 32-bit (yalnızca) sınırlı ve içinde sunulan `monotouch.dll` ve `XamMac.dll` derlemeler.
-  **Birleşik API:** desteği bulunan tek bir API ile 32 ve 64 bit geliştirme `Xamarin.iOS.dll` ve `Xamarin.Mac.dll` derlemeler.

Başka bir deyişle, kuruluş için yeni API'ları biz bunları sonsuza kadar veya koruma tutmak şekilde mevcut Klasik API'lerini kullanmaya devam edebilirsiniz geliştiriciler (değil atamak uygulama mağazası), yükseltme yapabilirsiniz.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Namespace değişiklikleri

Kod Mac ve iOS Ürünlerimiz arasında paylaşmak için uyuşmazlık azaltmak için biz ad alanları API'leri ürünler için değiştiriyorsunuz.

Biz öneki "MonoTouch" bizim iOS ürün ve "MonoMac" bizim Mac ürünün veri türleri üzerinde atar.

Bu kod koşullu derleme yeniden ayırma olmadan Mac ve iOS platformlarında arasında paylaşmak daha basit hale getirir ve kaynak kodu dosyaları üstündeki gürültü azaltır.

-  **Klasik API:** ad alanları kullanmak `MonoTouch.` veya `MonoMac.` öneki.
-  **Birleşik API:** herhangi bir ad alanı öneki

## <a name="runtime-defaults"></a>Çalışma zamanı Varsayılanları

Varsayılan kullanımlar tarafından Unified API **SGen** atık toplayıcısını ve [yeni başvuru sayımı](~/ios/internals/newrefcount.md) sistemi nesnenin sahipliğini izleme için. Aynı özellik Xamarin.Mac için bağlantı noktalı.

Bu, geliştiricilerin eski sistemiyle karşılaştığı ve ayrıca kolaylaştırır sorunlardan bazıları çözdü [bellek yönetimi](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Yeni Refcount etkinleştirmek için bile Klasik API mümkündür, ancak varsayılan koruyucu ve kullanıcıların herhangi bir değişiklik gerektirmez unutmayın. Birleşik API ile biz varsayılan değiştirme Fırsatı sürdü ve geliştiricilerin tüm geliştirmeleri aynı anda verin yeniden düzenlemeniz ve kendi kodlarını yeniden sınayın.

## <a name="api-changes"></a>API değişiklikleri

Kullanım dışı yöntemler Unified API kaldırır ve birkaç örneği vardır vardı burada yazım hatalarını API adlarında özgün MonoTouch ve MonoMac ad alanlarına Klasik API'lerde bağlanmış olduğunda. Bu örnekler, yeni birleşik API'lerinde düzeltildi ve ve bileşen, iOS ve Mac uygulamalarınızda güncelleştirilmesi gerekir. İçine çalışabilir en yaygın olanları listesi aşağıdadır:

|Klasik API yöntem adı|Birleşik API yöntem adı|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Birleşik API'sine Klasikten geçiş yaparken değişiklikleri tam bir listesi için lütfen bkz bizim [Klasik (monotouch.dll) vs Unified (Xamarin.iOS.dll) API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) belgeleri.

## <a name="updating-to-unified"></a>Çok birleşik güncelleştiriliyor

Bazı eski/bozuk/kullanım API **Klasik** kullanılamayan **Unified** API. Düzeltmek daha kolay olabilir `CS0616` , (el ile veya otomatik) başlatmadan önce uyarıları yükseltme sahip olacaksınız beri `[Obsolete]` sağ API için kılavuzluk etmesi için ileti (uyarı parçası) özniteliği.

Biz yayımlama Not bir [ *fark* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) önce veya sonra proje güncelleştirmeleri kullanılabilir API değişiklikleri Klasik vs birleşik. Hala düzelttikten olması zaman tasarrufu (daha az belgelerine aramaları) Klasik çağrılarında genellikle obsoletes.

Bu yönergeleri izleyin [var olan iOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-ios-apps.md), veya [Mac uygulamaları](~/cross-platform/macios/unified/updating-mac-apps.md) Unified API.
Bu sayfanın kalanını gözden geçirin ve [bu ipuçlarını](~/cross-platform/macios/unified/updating-tips.md) kodunuzu geçirme hakkında ek bilgi için.

### <a name="nuget"></a>NuGet

Daha önce Klasik API aracılığıyla Xamarin.iOS desteklenen NuGet paketlerini kullanarak kendi derlemeler yayımlanan **Monotouch10** platform ad.

Birleşik API uyumlu paketler için-yeni bir platform tanımlayıcısı tanıtır **Xamarin.iOS10**. Mevcut NuGet paketleri, bu platform desteği eklemek için birleşik API'sine karşı oluşturmaya güncelleştirilmesi gerekir.

> [!IMPORTANT]
> Formda bir hata varsa _"hatası 3 'monotouch.dll' ve 'Xamarin.iOS.dll' aynı Xamarin.iOS projede içeremez - 'Xamarin.iOS.dll' 'monotouch.dll' tarafından başvurulan sırada açıkça başvurulmaktadır ' xxx, sürüm 0.0.000, = Culture = neutral, PublicKeyToken = null'"_ Unified API uygulamanıza dönüştürdükten sonra genellikle bir bileşen veya NuGet paketi birleşik API'sine güncelleştirilmemiş projesinde sahip nedeniyle istenir. Varolan bileşeni/NuGet kaldırmak, birleşik API'lerini destekleyen bir sürüme güncelleştirmek ve temiz bir yapı yapmanız gerekir.

### <a name="the-road-to-64-bits"></a>64 bit yol

32 ve 64 bit uygulamalar ve çerçeveler hakkında bilgi destekleme hakkında arka plan için bkz: [32 ve 64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md).

 <a name="new-data-types" />

#### <a name="new-data-types"></a>Yeni veri türleri

Fark özünde Mac ve iOS API'leri her zaman 32 bit platformlarda ve 64 bit 64 bit platformlarda 32 bit bir mimariye özgü veri türlerini kullanın.

Örneğin, Objective-C eşlemeleri `NSInteger` veri türü için `int32_t` 32 bit sistemler ve çok `int64_t` 64 bit sistemler üzerinde.

Bu davranış, bizim Unified API eşleşecek şekilde biz önceki kullanımlarını değiştirdiğiniz `int` (.NET içinde tanımlanan her zaman olduğu `System.Int32`) için yeni bir veri türü: `System.nint`.  Yerel tamsayı platformunun türü için "n" / anlamı "yerel" düşünebilirsiniz.

Sunuyoruz `nint`, `nuint` ve `nfloat` de veri türleri sağlayan yerleşik bunların üstüne gerektiğinde.

Bu veri türü değişiklikleri hakkında daha fazla bilgi için bkz: [yerel türler](~/cross-platform/macios/nativetypes.md) belge.

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>İOS uygulamalarını mimarisini algılamaya nasıl

Uygulamanızın nerede 32 bit veya 64 bit iOS sistemi üzerinde çalışan olmadığını bilmek ister durumlar olabilir. Aşağıdaki kod, mimarisi denetlemek için kullanılabilir:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>Diziler ve System.Collections.Generic

C# dizin oluşturucular türü beklediğiniz çünkü `int`, açıkça cast gerekecek `nint` değerler `int` bir koleksiyon ya da dizi öğeleri erişmek için. Örneğin:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Bu beklenen bir davranış çünkü dönüştürme `int` için `nint` olan kayıplı 64-bit üzerinde örtük bir dönüştürme yok yapılır.

### <a name="converting-datetime-to-nsdate"></a>DateTime için NSDate dönüştürme

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

<a name="deprecated-typos" />

### <a name="deprecated-apis-and-typos"></a>Kullanım dışı API'ları ve yazım hatalarını

İç Xamarin.iOS Klasik API (monotouch.dll) `[Obsolete]` özniteliği, iki farklı şekilde kullanıldı:

-  **İOS API Kullanım:** size yeni bir tarafından bırakan bir API kullanarak durdurmak için Apple ipuçları durumunda olur. Hala sorun ve genellikle gerekli Klasik API (IOS eski sürümü destekliyorsa).
 Bu tür API (ve `[Obsolete]` özniteliği) yeni Xamarin.iOS derlemelerine dahil edilir.
-  **Yanlış API** bazı API adlarını yazım hatalarını vardı.

Özgün derlemelerde (monotouch.dll ve XamMac.dll) biz uyumluluk için kullanılabilir eski kod tutulur, ancak birleşik API derlemelerden (Xamarin.iOS.dll ve Xamarin.Mac) kaldırıldı

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject alt sınıfların .ctor(IntPtr)

Her `NSObject` alt kabul eden bir oluşturucuya sahip bir `IntPtr`. Biz yerel ObjC işleyici yeni bir yönetilen örneğinden nasıl örneği budur.

Klasik, bu bir `public` Oluşturucusu. Örneğin birkaç oluşturma örnekleri tek bir ObjC örneği için bu özelliği kullanıcı kodu kötüye kolay ancak yönetilen *veya* yönetilen bir örneği oluşturma (için alt sınıfların) beklenen yönetilen durumu olmaması.

Bu tür sorunları önlemek için `IntPtr` oluşturucular olan şimdi `protected` içinde **birleştirilmiş** yalnızca sınıflara için kullanılacak API. Bu düzeltme/güvenli API yönetilen örneği tanıtıcısı, yani oluşturmak için kullanılan garanti eder

    var label = Runtime.GetNSObject<UILabel> (handle);

Bu API (varsa) var olan bir yönetilen örneği döndürür veya (gerektiğinde) yeni bir tane oluşturun. Hem Klasik hem de birleşik API zaten mevcuttur.

Unutmayın `.ctor(NSObjectFlag)` de sunulmuştur `protected` ancak bunun dışında sınıflara nadiren kullanıldı.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>NSAction eylemiyle değiştirildi

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

### <a name="custom-delegates-replaced-with-actiont"></a>Eylemiyle değiştirilen özel temsilciler<T>

İçinde **birleştirilmiş** bazı basit (örn. bir parametre) .net temsilciler yerine `Action<T>`. Örneğin

    public delegate void NSNotificationHandler (NSNotification notification);

artık olarak kullanılabilir bir `Action<NSNotification>`. Bu yükseltme kodu yeniden ve Xamarin.iOS ve kendi uygulamalarınız içinde kod yinelemesinden azaltır.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>Görev<bool> < Boole, NSError >> Görev ile değiştirilir

İçinde **Klasik** bazı zaman uyumsuz API'leri vardı döndürme `Task<bool>`. Ancak bazı bunların nerede kullanılacak olan bir `NSError` yani imza parçası olan `bool` zaten `true` ve almak için bir özel durum catch gerekiyordu `NSError`.

Bazı yaygın hatalardır ve dönüş değeri yararlı değildi olduğundan bu deseni olarak değiştirildi **birleştirilmiş** döndürmek için bir `Task<Tuple<Boolean,NSError>>`. Bu, hem başarılı hem de zaman uyumsuz çağrı sırasında olmuş olabilir herhangi bir hata denetlemenizi sağlar.

### <a name="nsstring-vs-string"></a>NSString vs dize

Bazı durumlarda bazı sabitleri değiştirildiği gerekiyordu `string` için `NSString`, örn. `UITableViewCell`

**Klasik**

    public virtual string ReuseIdentifier { get; }

**Birleşik**

    public virtual NSString ReuseIdentifier { get; }

Genel biz .NET tercih `System.String` türü. Ancak, Apple yönergelerini rağmen bazı yerel API karşılaştırma sabit işaretçileri (dizesinin kendisini) ve biz sabitleri olarak kullanıma bu yalnızca çalışabilirsiniz `NSString`.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>Objective-C protokolleri

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

#### <a name="nscoding-protocol"></a>NSCoding Protokolü

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

- [İOS uygulamaları güncelleştirme](updating-ios-apps.md)
- [Mac uygulamaları güncelleştirme](updating-mac-apps.md)
- [Xamarin.Forms uygulamaları güncelleştirme](updating-xamarin-forms-apps.md)
- [Güncelleştirme bağlamaları](update-binding.md)
- [İpuçları güncelleştiriliyor](updating-tips.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](~/cross-platform/macios/native-types-cross-platform.md)