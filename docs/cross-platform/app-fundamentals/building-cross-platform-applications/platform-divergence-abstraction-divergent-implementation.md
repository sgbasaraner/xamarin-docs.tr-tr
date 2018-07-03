---
title: Bölüm 4 - birden fazla platformla ilgilenme
description: Bu belge, platform veya özellik tabanlı uygulama Geçitler nasıl ele alınacağını açıklar. Ekran boyutu, gezinti metaphors, dokunma ve hareket, anında iletme bildirimleri ve arabirimi paradigmalarını listeler ve sekmeler gibi ele alınmaktadır.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4a60c99cbc9819f07b77bfe9abe046ea92a550a5
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403331"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Bölüm 4 - birden fazla platformla ilgilenme

## <a name="handling-platform-divergence-amp-features"></a>Platform Geçitler işleme &amp; özellikleri

Geçitler yalnızca bir 'platformlar arası' sorun değildir; 'aynı' platform cihazlarda farklı özellikleri (özellikle birçok çeşit kullanılabilir olan Android cihazlar) sahip. En belirgin ve temel ekran boyutu, ancak diğer cihaz özniteliklerine değişir ve belirli özellikleri kontrol edin ve farklı kendi varlığı (veya var olmayan) göre davranır uygulamaya gerektirir.

Başka bir deyişle, tüm uygulamaları işlevselliği normal performansında Dağıt, aksi takdirde paragrafta, ortak paydası en düşük özellik sunabilirler gerekir. Xamarin'in her platform için yerel SDK'ları ile kapsamlı tümleştirme, bu özellikleri kullanmak için uygulamaları tasarlamak için mantıklı şekilde platforma özgü işlevsellikten yararlanmak için uygulamaları sağlar.

Platformları işlevselliği farkı, genel bir bakış Platform özelliklerinden belgelerine bakın.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Platform Geçitler örnekleri

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Platformlar arasında mevcut temel öğeleri

Evrensel mobil uygulamalarının bazı özellikleri vardır.
Genellikle tüm cihazları doğru ve uygulamanızın tasarımına temelini bu nedenle üst düzey kavramları şunlardır:

-  Özellik Seçimi sekmeler veya menüleri aracılığıyla
-  Veri ve kaydırma listesi
-  Tek veri görünümleri
-  Tek veri görünümleri düzenleme
-  Geri dönme


Üst düzey ekran akışınızı tasarlarken Bu kavramlar hakkında genel bir kullanıcı deneyimi temel alabilir.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>Platforma özel öznitelikler

Tüm platformlarda mevcut temel öğelere ek olarak, anahtar platform farklılıklarına tasarımınızı gerekecektir. Bu farklılıkları göz önünde bulundurun (ve özel olarak işlemek için kod yazmak) gerekir:

-   **Ekran boyutları** – hedeflemek görece basit ekran boyutları standartlaşmış bazı platformlarda (örneğin, iOS ve Windows Phone sürümlerde). Android cihazları uygulamanızda desteklemek için daha fazla çaba gerektirir ekran boyutları, çok çeşitli var.
-   **Gezinti metaphors** – farklı platformlarda (örn.) Panorama UI denetimi 'geri' donanım düğme) ve platformları (Android 2 ve 4, iPhone ve iPad) içinde.
-   **Klavye** – bazı Android cihazlar, diğerleri yalnızca bir yazılım klavye varken fiziksel klavyeler sahip. Geçici klavye ekran parçası anlaşılması güç algılar kodu Bu farklılıklar duyarlı olması gerekir.
-   **Dokunma ve hareket** – işletim sistemi desteği tanıma hareketi değişir, özellikle her işletim sisteminin daha eski sürümlerinde. Android önceki sürümlerde çok sınırlı eski cihazları destekleyen ayrı kod gerektirebilir, yani, dokunma işlemleri için desteği
-   **Anında iletme bildirimleri** – (örn her platformda farklı özellikleri/uygulamaları vardır. Kutucuklar üzerinde Windows live).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Cihaza özgü özellikleri

Hangi uygulama için gerekli en düşük özellikleri çalıştırılması gerektiğini belirler; veya karar verdiğinizde her platformda yararlanmak için hangi ek özellikler. Kod özellikleri Algıla ve işlevini devre dışı bırakın veya alternatifleri (örn. sunmak için gerekli "coğrafi konum için alternatif bir konum yazın ya da bir eşlemden seçin izin vermek için olabilir):

-   **Kamera** – işlevselliği cihazlar arasında farklılık gösterir: bazı cihazlar bir kamera yoksa, diğerleri hem ön ve arka yönelik kameralar sahip. Bazı kameralar video kaydını özelliğine sahiptir.
-   **Coğrafi konum ve haritalar** – GPS veya Wi-Fi konumu için desteği tüm cihazlarda mevcut değil. Her yöntem tarafından desteklenen doğruluğu değişen düzeylerde için gereksinimini karşılamak uygulamaları da gerekir.
-   **İvme ölçer, jiroskop ve compass** – uygulamaları neredeyse her zaman donanım sunulmaması halinde bir geri dönüş sağlamanız gerekir bu özellikler genellikle yalnızca bir seçim cihazların her platformda bulunur.
-   **Twitter ve Facebook** – yalnızca 'yerleşik' iOS5 ve iOS6 sırasıyla. Önceki sürümlerini ve diğer platformlar üzerinde kendi kimlik doğrulama işlevleri sağlayan ve doğrudan her arabirim ihtiyacınız olacak Hizmetler'in API.
-   **Yakın alan iletişimi (NFC)** – yalnızca (bazıları) üzerinde Android telefonları (makalenin yazıldığı sırada).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Platform Geçitler uğraşmanızı

Aynı kod tabanından-, her biri kendi avantaj ve dezavantajları kümesini birden çok platformu destekleme için iki farklı yaklaşım vardır.

-   **Platform soyutlama** – iş cephe deseni platformlar arasında birleştirilmiş bir erişim sağlar ve belirli bir platform uygulamalarını tek, birleştirilmiş bir API uygulamasına soyutlar.
-   **Uygulama kaliteleri** – belirli bir platform çağırma özellikleri arabirimleri ve devralma veya koşullu derleme gibi Mimari Araçları aracılığıyla kaliteleri uygulamaları aracılığıyla.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Platform Soyutlama

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Sınıfı Özet

Arabirimleri veya temel sınıfları kullanarak paylaşılan kod içinde tanımlanan uygulanan ve platforma özgü projelerinde genişletilmiş. Özellikle kullanabilecekleri framework sınırlı bir alt kümesine sahip ve platforma özgü kod dalları desteklemek için derleyici yönergeleri içeremez çünkü yazma ve paylaşılan kodu sınıf soyutlama ile genişletme taşınabilir sınıf kitaplıkları için uygundur.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Arabirimler

Arabirimleri kullanarak, ortak kodun yararlanmak için paylaşılan kitaplıklara yine de geçirilebilir platforma özgü sınıflar olanak tanır.

Arabirimi, paylaşılan kod içinde tanımlanan ve paylaşılan kitaplığa bir parametre veya özellik geçirildi.

Platforma özel uygulamalar, ardından arabirim uygular ve hala 'işlem sırasında ' paylaşılan kod yararlanın.

 **Avantajlar**

Uygulama, platforma özgü kod içeren ve hatta dış platforma özgü kitaplıklar başvuru.

 **Dezavantajlar**

Oluşturma ve Paylaşılan koda uygulamaları geçirmek zorunda. Arabirimi içinde derin paylaşılan kod kullanılıyorsa, ardından bunu olan yukarı birden çok yöntem parametresi geçirilen veya aksi halde çağrı zincirini gönderilen sona erer. Farklı arabirimler çok sayıda paylaşılan kod kullanıyorsa, ardından bunların tüm oluşturulmalı ve herhangi bir yerde paylaşılan kod içinde ayarlayabilirsiniz.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Devralma

Paylaşılan kodun bir veya daha fazla platforma özgü projelerinde Genişletilebilir soyut ya da sanal sınıflar uygulayabilirsiniz. Bu benzer arabirimleri kullanarak ancak bazı davranışı zaten uygulandı. Mı yoksa arabirimleri devralma mı daha iyi bir tasarım seçiminin farklı bakış açılarını şunlardır: özellikle C# yalnızca tek devralma izin verdiğinden, Apı'lerinizi tasarlanmış ileriye dönük şekilde okuyabilirsiniz. Devralma kullanırken dikkatli olun.

Avantajlar ve dezavantajlar arabirimlerin kalıtım bazı uygulama kodu (isteğe bağlı olarak genişletilebilen belki de tüm platformu belirsiz uygulaması) taban sınıfı içeren ek avantajı için eşit olarak uygulanır.

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Bkz: [Xamarin.Forms](~/xamarin-forms/get-started/index.md) belgeleri.


### <a name="plug-in-cross-platform-functionality"></a>Eklenti platformlar arası işlevsellik

Ayrıca, tutarlı bir şekilde eklentilerini kullanarak platformlar arası uygulamaları genişletebilirsiniz.

Gelen bağlı bizim [eklentileri github](https://github.com/xamarin/plugins), açık kaynaklı çoğu artırmasını yardımcı (Nuget aracılığıyla yükleme için kullanılabilir genellikle) projeler stav baterie işlevinden platforma özgü ayarları ile çeşitli uygulama bir Xamarin platformu ve Xamarin.Forms uygulamalarında kolayca ortak API.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Diğer platformlar arası kitaplıklar

Platformlar arası işlevsellik sağlayan kullanılabilir 3 şahıs kitaplıklardaki vardır:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Yerel dil** (yerelleştirme için) -  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (için XNA oyunlar) -  [http://www.monogame.net](http://www.monogame.net)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) ve kendi precursor [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Kaliteleri uygulama

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Koşullu Derleme

Burada paylaşılan kodunuza yine de farklı sınıflar veya farklı davranır özellikleri muhtemelen erişme, her bir platformda çalışması gerekir bazı durumlar vardır. Koşullu derleme, aynı kaynak dosyasında farklı sembollere sahip birden çok proje nerede başvurulan paylaşılan varlık projeleri ile en iyi şekilde çalışır.

Xamarin projeleri her zaman tanımlayın `__MOBILE__` olduğu true hem iOS hem de Android uygulaması projeleri (çift alt öncesi ve sonrası düzeltme bu sembolleri unutmayın).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Xamarin.iOS tanımlar `__IOS__` olduğu iOS cihazları algılamak için kullanabilirsiniz.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

İzleme ve TV özel simge vardır:

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

<a name="Android" />

##### <a name="android"></a>Android

Xamarin.Android uygulamalarına yalnızca derlenmiş kod aşağıdaki kullanabilirsiniz.

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Her bir API sürümü ayrıca yeni bir derleyici yönergesi tanımlar, aşağıdakine benzer bir kod izin verecek şekilde yeni API'ler hedeflenmişse özellikleri ekleyin. Her API düzeyi tüm 'alt' düzeyi sembolleri içerir. Bu özellik, birden çok platformu desteklemek için gerçekten kullanışlı değildir; genellikle `__ANDROID__` sembol yeterli olacaktır.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Yok şu anda Xamarin.Mac için yerleşik bir simge, ancak kendi Mac uygulaması projesi ekleyebilirsiniz **Seçenekleri > derleme > derleyici** içinde **sembolleri tanımlama** kutusu veya düzenleme **.csproj**  dosya ve orada ekleyin (örneğin `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Windows Phone uygulamaları tanımlayan iki simge – `WINDOWS_PHONE` ve `SILVERLIGHT` : platform kodu hedeflemek için kullanılabilir. Bunlar yapmak bunları Xamarin platformu simgeler gibi çevreleyen alt çizgi yoktur.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>Koşullu derleme kullanma

Koşullu derleme basit bir örnek olay incelemesi örneği SQLite veritabanı dosyası için dosya konumu ayarlıyor. Üç platformda dosya konumu belirtmek için biraz daha farklı gereksinimlere sahiptir:

-   **iOS** – Apple kullanıcıya özgü verileri, belirli bir konumda (kitaplık dizinini) yerleştirilecek tercih eder, ancak bu dizin için hiçbir sistem sabiti yoktur. Platforma özgü kod doğru yolu oluşturmak için gereklidir.
-   **Android** – tarafından döndürülen sistem yoluna `Environment.SpecialFolder.Personal` veritabanı dosyasını depolamak için kabul edilebilir bir konumdur.
-   **Windows Phone** – yalıtılmış depolama mekanizması belirtilmesi bir tam yol izin vermiyor yalnızca bir göreli yol ve dosya adı.
-   **Evrensel Windows platformu** – kullanan `Windows.Storage` API'leri.

Aşağıdaki kod koşullu derleme emin olmak için kullanır. `DatabaseFilePath` her platform için doğrudur:

```csharp
public static string DatabaseFilePath {
        get {
    var filename = "TodoDatabase.db3";
#if SILVERLIGHT
    // Windows Phone 8
    var path = filename;
#else

#if __ANDROID__
    string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine (libraryPath, filename);
#endif
        return path;
}
```

Sonuç oluşturduğu ve tüm platformlarda SQLite veritabanı dosyası her platformda farklı bir konum yerleştirerek kullanılan bir sınıftır.

