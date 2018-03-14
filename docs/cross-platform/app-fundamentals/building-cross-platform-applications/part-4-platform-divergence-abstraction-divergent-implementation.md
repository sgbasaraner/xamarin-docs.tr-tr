---
title: "Bölüm 4 - ile birden çok platform ele alma"
ms.topic: article
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 21cd08ad2eb9c78ba1bcd6b31400a38266c65e51
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Bölüm 4 - ile birden çok platform ele alma

## <a name="handling-platform-divergence-amp-features"></a>Platform Geçitler işleme &amp; özellikleri

Geçitler yalnızca bir 'platformlar arası' sorun değildir; 'aynı' platformu cihazlarda farklı özellikleri (özellikle çeşitli kullanılabilir olan Android cihazlar) sahip. En bariz ve temel ekran boyutu, ancak diğer cihaz öznitelikleri değişir ve belirli yeteneklerini denetleyin ve bunların varlığı (veya yokluğuna) farklı şekilde göre hareket etmesi için bir uygulama gerektirir.

Bu, tüm uygulamaların işlevselliği normal düşüşü ile ilgilidir, aksi takdirde bir paragrafta, yaygın payda düşük özellik kümesi sunmak gereken anlamına gelir. Xamarin'ın her platform için yerel SDK'ları ile derin tümleştirme uygulamaların bu özellikleri kullanmak için uygulamalar tasarlamak için bir anlam şekilde, platforma özgü işlevselliğinden yararlanmasına olanak sağlar.

Platformları işlevindeki nasıl farklılık gösterir, genel bir bakış Platform özelliklerini belgelerine bakın.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Platform Geçitler örnekleri

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Platformlar arası mevcut temel öğeler

Evrensel mobil uygulamaların bazı özellikleri vardır.
Tüm aygıtların genellikle true ve bu nedenle, uygulamanızın tasarım temelini üst düzey kavramları şunlardır:

-  Sekme veya menüleri aracılığıyla özellik seçimi
-  Veri ve kaydırma listesi
-  Tek veri görünümleri
-  Tek veri görünümlerini düzenleme
-  Geri dönme


Üst düzey ekran akışınız tasarlarken ortak bir kullanıcı deneyimi bu kavramları hakkında temel alabilir.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>platforma özel öznitelikler

Tüm platformlarda mevcut temel öğeleri yanı sıra, adres anahtar platform tasarımınızı farklılıkları gerekecektir. Bu farklılıklar göz önünde bulundurun (ve özel olarak işlemek için kod yazma) gerekebilir:

-   **Ekran boyutları** – bazı platformlar (örneğin, iOS ve Windows Phone sürümlerde) hedeflemek görece basit ekran boyutlarına standartlaşmış. Android aygıtların uygulamanızda desteklemek için daha fazla çaba gerektirir ekran boyutları çeşitliliğini vardır.
-   **Gezinti metaphors** – farklı platformlarda (ör.) Donanım 'geri' düğmesini, Panorama UI denetimi) ve platformlar (Android 2 ve 4, iPhone vs iPad) içinde.
-   **Klavyeler** – bazı Android cihazları, diğerleri yalnızca yazılım klavye varken fiziksel klavyeler sahip. Yumuşak klavye ekran parçası anlaşılması güç yükleyen algılar kod bu farklılıklar duyarlı olması gerekir.
-   **Dokunma ve hareket** – işletim sistemi desteği hareketi tanıma değişir, özellikle eski her işletim sistemi sürümlerinde. Android, önceki sürümlerde çok sınırlı eski cihaz destekleyen ayrı kod gerektirebilir anlamı dokunma işlemleri için destek
-   **Anında iletme bildirimleri** – (ör her platformda farklı özellikleri/uygulamaları vardır. Döşeme Windows live).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Aygıta özgü özellikleri

Hangi uygulama için gerekli minimum özellikleri çalıştırılması gerektiğini belirler; veya ne zaman her platformda yararlanmak için hangi ek özellikler karar verin. Kod özellikleri algılamak ve işlevini devre dışı bırakmak veya alternatifleri (ör.) sunmak için gerekli coğrafi konum için bir alternatif bir konum yazın ya da bir eşlemesinden seçin kullanıcı izin verecek şekilde olabilir):

-   **Kamera** – işlevselliği cihazlar arasında farklılık gösterir: bazı cihazların kamera yoksa, diğerleri hem ön ve arka yönelik kameralar sahip. Bazı kameralar video kaydını özelliğine sahiptir.
-   **Coğrafi konum & eşlemeleri** – GPS veya Wi-Fi konumu desteği tüm cihazlarda mevcut değil. Uygulamalar her yöntemi tarafından desteklenen doğruluğu değişen düzeylerde karşılamak amacıyla da gerekir.
-   **Accelerometer, jiroskop ve pusula** – uygulamaları neredeyse her zaman donanım desteklenmiyor olduğunda bir geri dönüş sağlamanız gerekir böylece bu özellikleri genellikle yalnızca her platform cihazlarda seçimi bulunur.
-   **Twitter ve Facebook** – yalnızca 'yerleşik' iOS5 ve iOS6 sırasıyla. Önceki sürümler ve diğer platformlar üzerinde kendi kimlik doğrulama işlevleri sağlayan ve doğrudan her arabirim gerekir hizmetlerin API.
-   **Yakın alan iletişimi (NFC)** – yalnızca (bazı) üzerinde Android telefonlar (yazıldığı sırada).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Platform Geçitler postalarla

Aynı kod tabanlı, her avantajları ve dezavantajları kendi kümesiyle gelen birden çok platformu destekleme için iki farklı yaklaşım vardır.

-   **Platform soyutlama** – iş cephesi düzeni, platformlar arası bir birleşik erişim sağlar ve tek, birleşik bir API uygulamasına belirli platform uygulamaları soyutlar.
-   **Divergent uygulama** – belirli platform çağırma özellikleri arabirimleri ve devralma veya koşullu derleme gibi Mimari Araçları aracılığıyla divergent uygulamaları aracılığıyla.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Platform Soyutlama

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Sınıfı Özet

Arabirimleri veya temel sınıflar kullanılarak paylaşılan kod içinde tanımlanan uygulanan ve platforma özgü projelerinde genişletilmiş. Çünkü bunlar kullanabilecekleri framework sınırlı bir alt kümesinde sahip ve platforma özgü kodu dalları desteklemek için derleyici yönergeleri içeremez yazma ve sınıf soyutlamalar paylaşılan koduyla genişletme özellikle de taşınabilir sınıf kitaplıkları için uygundur.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Arabirimler

Arabirimleri kullanarak, ortak kodun yararlanmak için paylaşılan kitaplıklara hala geçirilebilir platforma özgü sınıflar uygulamak sağlar.

Arabirim paylaşılan kod içinde tanımlanan ve paylaşılan Kitaplığı'na bir parametre veya özellik geçirildi.

Platforma özgü uygulamalar arabirimini uygulayan ve hala ', işlem için ' paylaşılan kod yararlanın.

 **Avantajlar**

Uygulama, platforma özgü kodu içeren ve hatta platforma özgü dış kitaplıklarına.

 **Dezavantajlar**

Oluşturma ve uygulamaları paylaşılan koda geçirmek zorunda. Arabirimi içinde ayrıntılı paylaşılan kodu kullandıysanız sonra onu olan yukarı birden çok yöntem parametreleri gözden geçirilen veya aksi halde çağrı zincirine gönderilen sona erer. Paylaşılan kod farklı arabirimleri çok kullanıyorsa sonra bunlar tüm oluşturulmalı ve herhangi bir yerde paylaşılan kod içinde ayarlayın.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Devralma

Paylaşılan kod Genişletilebilir soyut ya da sanal sınıf bir veya daha fazla platforma özgü projelerinde uygulamanız. Bu, benzer arabirimleri kullanarak ancak zaten uygulanmış bazı davranışı. Arabirimler veya devralma olup üzerinde daha iyi bir tasarım seçiminin farklı görüşlerini şunlardır: özellikle C# yalnızca tek devralma izin verdiğinden, Apı'lerinizi tasarlanmış ileride şekilde okuyabilirsiniz. Devralma dikkatli kullanın.

Olumlu ve olumsuz arabirimlerinin kalıtım bazı uygulama kodu (isteğe bağlı olarak genişletilebilen belki de tüm platform belirsiz uygulaması) temel sınıfı içeren ek avantajı için eşit olarak uygulanır.

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Bkz: [Xamarin.Forms](~/xamarin-forms/get-started/index.md) belgeleri.


### <a name="plug-in-cross-platform-functionality"></a>Eklenti platformlar arası işlevi

Bu gibi durumlarda, platformlar arası uygulamalar da eklentileri kullanarak tutarlı bir şekilde genişletebilirsiniz.

Bağlanılan bizim [eklentileri github](https://github.com/xamarin/plugins), açık kaynaklı çoğu eklentileri yardımcı (genellikle Nuget aracılığıyla yükleme için kullanılabilir) projeleri uygulamak pil durumu platforma özgü işlevinden ayarlarla için çeşitli bir Xamarin Platform ve Xamarin.Forms uygulamaları tüketmek kolay ortak API.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Diğer platformlar arası kitaplıkları

Platformlar arası işlevselliği sağlayan kullanılabilir 3 taraf kitaplıklar vardır:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Yerel dil** (yerelleştirme için) - [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (için XNA oyunlar) - [http://monogame.codeplex.com/](http://monogame.codeplex.com/)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) ve kendi precursor [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Divergent uygulama

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Koşullu Derleme

Burada paylaşılan kodunuzu hala farklı büyük olasılıkla sınıfları veya farklı şekilde davranan özellikleri erişen her platformda çalışmanız gerekir bazı durumlar vardır. Koşullu derleme paylaşılan varlık burada tanımlanan farklı simgeleri sahip birden çok proje aynı kaynak dosyaya başvuruluyor projeleri ile en iyi şekilde çalışır.

Xamarin projeleri her zaman tanımlayın `__MOBILE__` iOS ve Android uygulaması projeleri (Not çift-alt çizgi öncesi ve sonrası düzeltme bu simgeleri) için doğru olduğu.

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Xamarin.iOS tanımlar `__IOS__` , iOS cihazları algılamak için kullanabilirsiniz.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

İzleme ve TV özel simgeleri vardır:

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

Xamarin.Android uygulamaları yalnızca derlenmiş kod aşağıdaki kullanabilirsiniz

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Her API sürümü yeni bir derleme yönergesi de tanımlar, aşağıdakine benzer bir kod sağlayacak şekilde yeni API'ler hedeflenen varsa özellikleri ekleyin. Her API düzey tüm 'alt' düzeyi simgeleri içerir. Bu özellik birden çok platform desteklemek için gerçekten kullanışlı değildir; genellikle `__ANDROID__` sembol yeterli olacaktır.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Yok. şu anda Xamarin.Mac için yerleşik bir simge, ancak kendi Mac uygulaması projesi eklemek **Seçenekleri > Yapı > derleyici** içinde **tanımlamak simgeleri** kutusu veya düzenleme **.csproj**  dosya ve orada ekleyin (örneğin `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Windows Phone uygulamaları tanımlar iki simge – `WINDOWS_PHONE` ve `SILVERLIGHT` – platform kodu hedeflemek için kullanılabilir. Bunlar yapmak gibi Xamarin platform simgeleri çevreleyen alt çizgi yok.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>Koşullu derleme kullanma

Koşullu derleme basit bir örnek olay incelemesi örneği SQLite veritabanı dosyası için dosya konumu ayarlıyor. Üç platformları dosya konumunu belirtmek için biraz farklı gereksinimleri vardır:

-   **iOS** – Apple belirli bir konuma (kitaplık dizini) yerleştirilecek kullanıcı olmayan veri tercih eder, ancak bu dizin için hiçbir sistem sabiti yok. Platforma özgü kodu, doğru yolu oluşturmak için gereklidir.
-   **Android** – tarafından döndürülen sistem yolu `Environment.SpecialFolder.Personal` veritabanı dosyasını depolamak için kabul edilebilir bir konumdur.
-   **Windows Phone** – yalıtılmış depolama mekanizmasını belirtilmesi bir tam yol izin verme yalnızca bir göreli yol ve dosya adı.
-   **Evrensel Windows platformu** – kullanan `Windows.Storage` API'leri.

Aşağıdaki kod emin olmak için koşullu derleme kullanır `DatabaseFilePath` her platform için doğrudur:

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

Sonuç yerleşik ve SQLite veritabanı dosyası her platformda farklı bir konuma yerleştirerek tüm platformlarda kullanılan bir sınıftır.

