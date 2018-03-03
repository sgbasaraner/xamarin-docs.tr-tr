---
title: "Yerelleştirme"
description: "Xamarin.Forms uygulamaları .NET kaynak dosyaları kullanarak yerelleştirilmiş olmalıdır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: ad9129e06f43eea69518c4d876edc7cfd462f4e0
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="localization"></a>Yerelleştirme

_Xamarin.Forms uygulamaları .NET kaynak dosyaları kullanarak yerelleştirilmiş olmalıdır._

## <a name="overview"></a>Genel Bakış

.NET uygulamaları kullanan yerelleştirme için yerleşik mekanizması [RESX dosyaları](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) ve sınıflarda `System.Resources` ve `System.Globalization` ad alanları. Çevrilen dizelerin bulunduğu RESX dosyaları Xamarin.Forms derlemesindeki çevirileri kesin türü belirtilmiş erişim sağlayan derleyici tarafından üretilen bir sınıf birlikte katıştırılır. Çeviri metin ardından kodda alınabilir.

Bu belgede aşağıdaki bölümler yer alır:

**Xamarin.Forms kod Genelleştirme**

* Ekleme ve bir Xamarin.Forms PCL uygulamasında dize kaynaklarını kullanma.
* Yerel uygulamalar her dil algılamayı etkinleştirme.

**XAML yerelleştirme**

* XAML kullanarak yerelleştirme bir `IMarkupExtension`.
* Yerel uygulamalar biçimlendirme uzantı etkinleştiriliyor.

**Platforma özgü öğeleri yerelleştirme**

* Yerelleştirme görüntüleri ve yerel uygulamalar, uygulama adı.

### <a name="sample-code"></a>Örnek kod

Bu belgeyle ilişkilendirilen iki örnekleri şunlardır:

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) açıklanan kavramları çok basit bir örnektir. Aşağıda gösterilen kod parçacıkları tüm bu örnekten ' dir.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) bu yerelleştirme teknikleri kullanan temel bir çalışma uygulamadır.

#### <a name="shared-projects-are-not-recommended"></a>Paylaşılan projeleri tavsiye edilmez

TodoLocalized örnek içeren bir [paylaşılan proje demo](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) yapı sistem sınırlamaları nedeniyle kaynak dosyaları alınamadı ancak bir **. designer.cs** erişim olanağı kıran oluşturulan dosya dizeleri çevrilen [kesin türü belirtilmiş kodda](~/xamarin-forms/app-fundamentals/localization.md).

Bu belgenin geri kalanında Xamarin.Forms PCL şablonu kullanarak projeleri için ilişkilendirir.

## <a name="globalizing-xamarinforms-code"></a>Xamarin.Forms kod Genelleştirme

**Genelleştirme** uygulamanın "world hazır." yapma işlemidir. Bu, farklı diller görüntüleyebilen kod yazma anlamına gelir.

Bir uygulama Genelleştirme anahtar parçalarından derleme kullanıcı arabirimi yani hiçbir *sabit kodlanmış* metin. Bunun yerine, kullanıcıya gösterilen herhangi bir şey kendi seçilen dile tercüme dizeler kümesi nereden alınacağını.

Bu belgede biz RESX dosyaları Bu dizelerin depolamak ve bunlara kullanıcı tercihi bağlı olarak görüntülenmek almak için nasıl kullanılacağı anlatılmaktadır.

Örnekler, İngilizce, Fransızca, İspanyolca, Almanca, Çince, Japonca, Rusça ve Portekizce (Brezilya) dilleri hedefleyin. Uygulamaları gerektiği gibi birkaç veya sayıda dillere çevrilebilir.

### <a name="adding-resources"></a>Kaynakları ekleme

Xamarin.Forms PCL uygulama Genelleştirme ilk adımı, uygulamada kullanılan tüm metni depolamak için kullanılacak RESX kaynak dosyaları eklemektir. Varsayılan metin içeren bir RESX dosyası ekleyin ve ardından desteklemek için istediğimiz her dil için ek RESX dosyaları eklemek gerekir.

#### <a name="base-language-resource"></a>Temel dil kaynak

Temel kaynaklar (RESX) dosyası (örnekler İngilizce varsayılan dildir varsayılmıştır) varsayılan dil dizeleri içerir. Dosya Xamarin.Forms ortak kodu projesine projeye sağ tıklayıp seçerek eklemek **Ekle > yeni dosya...** .

Gibi anlamlı bir ad seçin **AppResources** ve basın **Tamam**.

[ ![Kaynak dosyası ekleme](localization-images/resx-new-file-sml.png "yeni dosya iletişim kutusu")](localization-images/resx-new-file.png "yeni dosya iletişim kutusu")

İki dosya projeye eklenecek:

* **AppResources.resx** çevrilebilir dizeleri XML biçiminde depolandığı dosya.
* **AppResources.designer.cs** RESX XML dosyasında oluşturulan tüm öğelere başvurular için bir parçalı sınıf bildirir dosya.

Çözüm ağaç dosyaları ilgili olarak gösterilir. RESX dosyası *gereken* düzenlenmesi yeni çevrilebilir dizeleri; eklemek için **. designer.cs** dosya gereken *değil* düzenlenmesi.

![](localization-images/appresources-tree.png "AppResources.resx dosyası")

##### <a name="string-visibility"></a>Dize görünürlüğü

Varsayılan olarak kesin türü belirtilmiş başvuruları dizelere oluşturulduğunda, bunların olacaktır `internal` derleme. RESX dosyaları için varsayılan Yapılandırma Aracı'nı oluşturur olmasıdır **. designer.cs** ile dosya `internal` özellikleri.

Seçin **AppResources.resx** dosya ve Göster **özellikleri** bu yapı aracı olduğu görmek için paneli yapılandırın. Aşağıda gösterildiği ekran **özel araç: ResXFileCodeGenerator**.

[[ide name="xs]]

[ ![](localization-images/xs-resx-internal-sml.png "AppResources.Resx özellikleri paneli")](localization-images/xs-resx-internal.png)

[[/ide]]

[[ide name="vs]]

[ ![](localization-images/vs-resx-internal-sml.png "Özellikler penceresi AppResources.Resx için")](localization-images/vs-resx-internal.png)

[[/ide]]

Kesin türü belirtilmiş dize özellikleri yapmak için `public`, yapılandırma için el ile değiştirmeniz gerekir **özel araç: PublicResXFileCodeGenerator**, aşağıdaki ekran görüntüsünde gösterildiği gibi:


[[ide name="xs]]

[ ![](localization-images/xs-resx-public-sml.png "AppResources.Resx özellikleri paneli")](localization-images/xs-resx-public.png)

[[/ide]]

[[ide name="vs]]

[ ![](localization-images/vs-resx-public-sml.png "Özellikler penceresi AppResources.Resx için")](localization-images/vs-resx-public.png)

[[/ide]]

Bu değişiklik isteğe bağlıdır ve yalnızca yerelleştirilmiş dizeleri (örneğin, RESX dosyaları farklı bir derlemede kodunuzu yerleştirdiğiniz varsa) farklı derlemeler başvuru isteyip istemediğinizi gerekli. Bu konu için örnek dizeleri bırakır `internal` bunlar burada kullanılır Xamarin.Forms PCL derlemede tanımlandığından.

Yalnızca yukarıda gösterildiği gibi temel RESX dosyasında özel araç ayarlamanız yeterlidir; Ayarlanacak gerekmez *herhangi* oluşturma aracını aşağıdaki bölümlerde ele dile özgü RESX dosyaları.

##### <a name="editing-the-resx-file"></a>RESX dosya düzenleme

Ne yazık ki yoktur yerleşik RESX Düzenleyici Mac için Visual Studio'da Yeni çevrilebilir dizeleri eklemek için yeni bir XML eklenmesi gerekir `data` her dize öğesi. Her `data` öğesi aşağıdakileri içerebilir:

* `name` (gerekli) özniteliği bu çevrilebilir dize anahtardır. Hiçbir boşluk veya özel karakterler izin geçerli bir C# özelliği adı - olması gerekir.
* `value` uygulamada görüntülenen gerçek dizesidir (gerekli) öğesi.
* `comment` öğe (isteğe bağlı) Bu dize nasıl kullanıldığını açıklar Çevirmen için yönergeler içerebilir.
* `xml:space` öznitelik (aralığı dizesindeki nasıl korunur denetlemek isteğe bağlı).

Bazı örnek `data` öğeleri burada gösterilir:

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

Uygulama yazıldığı şekilde, her kullanıcı için görüntülenen metin parçası temel RESX kaynak dosyasına yeni bir olarak eklenmelidir `data` öğesi. Olmaması önerilir `comment`yüksek kaliteli çeviri emin olmak mümkün olduğunca s.

> [!NOTE]
> Visual Studio (ücretsiz Community sürümü dahil) bir temel RESX Düzenleyicisi'ni içerir. Bir Windows bilgisayara erişiminiz varsa, Ekle ve RESX dosyaları dizelerini düzenlemek için kolay bir yol olabilir.

#### <a name="language-specific-resources"></a>Dile özgü kaynaklar

Genellikle, varsayılan metin dizelerini gerçek çevrilmesi uygulama büyük boyutta (Bu durumda varsayılan RESX dosyası dizeleri çok sayıda içerir) yazılan kadar gerçekleşmez. İsteğe bağlı olarak yerelleştirme kodu test yardımcı olması için makine çevrilmiş metinle doldurma erken geliştirme döngüsünde dile özgü kaynakları eklemek için hala iyi bir fikirdir.

Bir ek RESX dosyası desteklemek için istediğimiz her dil için eklenir.
Dile özgü kaynak dosyaları, belirli bir adlandırma kuralı uymalıdır: temel kaynaklarına (ör.) dosyası olarak aynı adı kullanın **AppResources**) bir nokta (.) ve ardından dil kodu tarafından. Basit örnekler:

* **AppResources.fr.resx** -Fransızca Dil çevirileri.
* **AppResources.es.resx** -İspanyolca dil çevirileri.
* **AppResources.de.resx** -Almanca dil çevirileri.
* **AppResources.ja.resx** -Japonca dil çevirileri.
* **AppResources.zh Hans.resx** - Çince (Basitleştirilmiş) dil çevirileri.
* **AppResources.zh Hant.resx** - Çince (Geleneksel) dil çevirileri.
* **AppResources.pt.resx** -Portekizce dil çevirileri.
* **AppResources.pt BR.resx** -Brezilya Portekizcesi dil çevirileri.

Genel düzeni iki harfli dil kodlarını kullanmak için ancak mevcuttur farklı bir biçim kullanıldığı bazı örnekler (örneğin, Çince) ve diğer örnekleri (örneğin, Portekizce (Brezilya)) dört karakter yerel ayar tanımlayıcısı gerekli olduğu.

Bu dile özgü kaynaklar dosyaları *sağlamadığı* gerektiren bir **. designer.cs** kısmi sınıf ile XML dosyaları normal olarak bunlar eklenebilir şekilde **yapı eylemi: EmbeddedResource**ayarlayın. Bu ekran dile özgü kaynak dosyaları içeren bir çözüm gösterilmektedir:

![](localization-images/appresources-langs.png "Dile özgü kaynak dosyaları")

Bir uygulama geliştirilen ve temel RESX dosyasına eklenen metin var. gibi her çevirir çevirmenler için göndermeden `data` öğesi ve return (gösterilen adlandırma kuralını kullanarak) dile özgü kaynak dosyası uygulamada dahil edilecek. 'Makine çevrilmiş' bazı örnekleri aşağıda verilmiştir:

**AppResources.es.resx (İspanyolca)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (Japanese)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt BR.resx (Portekizce (Brezilya))**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

Yalnızca `value` öğesi tarafından Çeviricisi - güncelleştirilmesi gerekiyor `comment` çevrilmesi üzere tasarlanmamıştır. Unutmayın: kaçınmak için XML dosyalarını düzenlerken gibi ayrılmış karakterler `<`, `>`, `&` ile `&lt;`, `&gt;`, ve `&amp;` içinde görünüyorsa `value` veya `comment`.

<a name="incode" />

### <a name="using-resources-in-code"></a>Kaynak kod içinde kullanma

RESX kaynak dosyalarında dizeleri, kullanıcı arabirimini kullanarak kod kullanmak kullanılabilir olacak `AppResources` sınıfı. `name` Her dize RESX, atanan dosya Xamarin.Forms kodda başvurulabilir, aşağıda gösterildiği gibi bu sınıf, bir özellik olur:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

İOS, Android ve Windows platformları işler aynı kullanıcı arabiriminde beklediğiniz, metnin bir kaynaktan yüklendiğinden uygulamanın birden çok dillere çevirmek olası şimdi dışında yerine sabit kodlanmış. Çeviri önce her platformda kullanıcı arabirimini gösteren ekran görüntüsü aşağıda verilmiştir:

![](localization-images/simple-example-english.png "Platformlar arası Uı'lar çeviri önce")

### <a name="troubleshooting"></a>Sorun giderme

#### <a name="testing-a-specific-language"></a>Belirli bir dil test etme

Farklı dillerde, farklı kültürler hızlı bir şekilde test etmek istediğiniz zaman geliştirme sırasında particulary benzeticisi veya bir aygıt geçiş yapmak zor olabilir.

Ayarlayarak yüklenecek belirli bir dil zorlayabilirsiniz `Culture` Bu kod parçacığında gösterildiği gibi:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Bu yaklaşım doğrudan kültürü ayarlama – `AppResources` sınıf – uygulamanızı (yerine cihazın yerel kullanarak) içinde bir dil Seçici uygulamak için de kullanılabilir.

#### <a name="loading-embedded-resources"></a>Yükleme kaynakları katıştırılmış

Aşağıdaki kod parçacığını (RESX dosyaları gibi) katıştırılmış kaynakları ile ilgili sorunları hata ayıklamak çalışırken yararlı olur. Bu kodu uygulamanıza (başlarında uygulama yaşam döngüsü) ekleyin ve tam kaynak tanımlayıcısı gösteren derlemede katıştırılmış tüm kaynaklar listelenir:

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

İçinde **AppResources.Designer.cs** dosyası (geçici *satır 33*), tam *Kaynak Yöneticisi adı* (ör. belirtilen `"UsingResxLocalization.Resx.AppResources"`) aşağıdaki kodu benzer:

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Denetleme **uygulama çıktısı** yukarıda gösterilen hata ayıklama kodu sonuçlar için onaylamak için doğru kaynakları (örn. listelenir `"UsingResxLocalization.Resx.AppResources"`).

Aksi takdirde, `AppResources` sınıfı kaynaklarını yükleyemiyor olacaktır.
Kaynakların nerede bulunamıyor sorunları gidermek için şunları denetleyin:

* Proje için varsayılan ad alanını kök ad alanını eşleşen **AppResources.Designer.cs** dosya.
* Varsa **AppResources.resx** dosya alt dizininde bulunuyorsa, alt ad ad alanı parçası olması gereken *ve* kaynak tanımlayıcısı parçası.
* **AppResources.resx** dosyasının **yapı eylemi: EmbeddedResource**.
* **Proje Seçenekleri > kaynak kodu > .NET adlandırma ilkeleri > kullanım Visual Studio stili kaynakları adları** ticked. Tercih ederseniz bu untick, ancak RESX kaynaklarınızı başvururken kullanılan ad alanları gerekecek uygulama boyunca güncelleştirildi.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Hata ayıklama modu (yalnızca Android) çalışmıyor

Çevrilen dizelerin yayın Android derlemeleriniz ancak değil hata ayıklama sırasında çalışıyorsanız, sağ tıklayın **Android projesi** seçip **Seçenekleri > Yapı > Android derleme** ve emin olun **Hızlı derleme dağıtım** değil ticked. Bu seçenek kaynakları yükleme ile ilgili sorunlara neden olur ve yerelleştirilmiş uygulamaları sınıyorsanız kullanılmamalıdır.

### <a name="displaying-the-correct-language"></a>Doğru dil görüntüleme

Şu ana kadar biz çevirileri sağlanabilir böylece kodunun nasıl yazılacağını incelenmesi ancak aslında onları görünür hale getirmek nasıl değil. Xamarin.Forms kod yararlanabilir. NET'in kaynakları doğru dil çevirileri yüklenmesi için ancak kullanıcı seçildi dili belirlemek için her platformun işletim sisteminde sorgu gerekir.

Kullanıcının dil tercihi elde etmek için bazı platforma özgü kod gereklidir çünkü bir [bağımlılık hizmeti](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Xamarin.Forms uygulaması, bu bilgileri göstermek ve her platform için uygulamak için.

İlk olarak, kullanıcının tercih ettiği kültürel dil, aşağıdaki kodu benzer kullanıma sunmak için bir arabirim tanımlayın:

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

İkinci olarak, kullanın [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Xamarin.Forms içinde `App` arabirimi çağırın ve bizim RESX kaynakları kültür doğru değerine ayarlamak için sınıf. Biz el ile bu değer Windows Phone ve evrensel Windows platformu için bu yana kaynakları framework otomatik olarak ayarlamak üzere gerekmez dikkat edin, bu platformlarda seçilen dil tanır.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

Kaynak `Culture` doğru dil dizeleri kullanılan böylelikle uygulamanın ilk kez yüklediğinde ayarlanması gerekir. İsteğe bağlı olarak bu değer uygulama çalışırken kullanıcının dil tercihlerini güncelleştirmeleri olursa, iOS veya Android geçirilen platforma özgü olaylar göre güncelleştirebilir.

Uygulamalar için `ILocalize` arabirimi gösterilmiştir [platforma özgü kodu](#Platform-specific_Code) bölümüne bakın. Bu uygulamaları bu yararlanmak `PlatformCulture` yardımcı sınıfı:

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>Platforma özgü kodu

Bu bilgileri, iOS, Android ve Windows platformları, biraz farklı şekillerde kullanıma sunmak için görüntülenecek dili algılamak için kodu platforma özgü olmalıdır. Kodunu `ILocalize` bağımlılık hizmetidir sağlanan Aşağıda her platform için emin olmak için ek platforma özgü gereksinimler birlikte yerelleştirilmiş metin doğru şekilde işlenir.

Platforma özgü kodu da burada işletim sistemi tarafından desteklenmeyen bir yerel ayar tanımlayıcısı yapılandırmak kullanıcı veren durumlarda işlemesi gerekir. NET'in `CultureInfo` sınıfı. Bu durumlarda, desteklenmeyen yerel ayarlar algılamak ve en iyi yerine için özel kod yazılmış olmalıdır. NET uyumlu yerel ayar.

#### <a name="ios-application-project"></a>iOS uygulama projesi

iOS kullanıcıları kendi tercih edilen dili tarih ve saat kültür biçimlendirme ayrı ayrı seçin. Yalnızca ihtiyacımız sorgulamak için bir Xamarin.Forms uygulaması yerelleştirme için doğru kaynakları yüklemek için `NSLocale.PreferredLanguages` dizinin ilk öğesi için.

Aşağıdaki uygulanması `ILocalize` bağımlılık hizmeti iOS uygulama projesinde yerleştirilmelidir. İOS (.NET standart gösterimi olan) tire yerine alt çizgi kullandığından kodu alt çizgi başlatmasını önce değiştirir `CultureInfo` sınıfı:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string iOSToDotnetLanguage(string iOSLanguage)
        {
            var netLanguage = iOSLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch` Blokları `GetCurrentCultureInfo` yöntemi tam eşleşme, arama dili (yerel karakterlerin ilk blok) yalnızca temel yakın bir eşleşme bulunmazsa yerel ayar tanımlayıcıları ile – genellikle kullanılan geri dönüş davranışını taklit eden.
>
> Xamarin.Forms, söz konusu olduğunda bazı yerel iOS geçerli olan, ancak geçerli bir benzemez `CultureInfo` .NET ve bu durumu çözmek için deneme yukarıdaki kodu.
>
> Örneğin, iOS **ayarlar > genel dil &amp; bölge** ekran telefonunuz ayarlamanıza olanak verir **dil** için **İngilizce** ancak  **Bölge** için **İspanya**, yerel ayar dizesi içinde sonuçları `"en-ES"`. Zaman `CultureInfo` oluşturma başarısız oldu, kod döner geri görüntüleme dilini seçmek için yalnızca ilk iki harf kullanma.
>
> Geliştiriciler değiştirme `iOSToDotnetLanguage` ve `ToDotnetFallbackLanguage` kendi desteklenen diller için gerekli özel durumları işlemek için yöntemleri.


İOS tarafından gibi otomatik olarak çevrilir bazı sistem tarafından tanımlanan kullanıcı arabirimi öğeleri vardır **Bitti** düğmesini `Picker` denetim. İhtiyacımız destekliyoruz içinde hangi dilleri belirtmek için bu öğeler çevirmek için iOS zorlamak için **Info.plist** dosya. Bu değerleri aracılığıyla ekleyebilirsiniz **Info.plist > kaynak** aşağıda gösterildiği gibi:

![Info.plist yerelleştirme anahtarlarında](localization-images/info-plist.png "Info.plist anahtarlarında yerelleştirme")

Bunun yerine, **Info.plist** bir XML Düzenleyicisi'nde dosya ve değerlerini doğrudan düzenleyin:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

Bağımlılık hizmeti uygulanması ve güncelleştirilmiş sonra **Info.plist**, iOS uygulaması yerelleştirilmiş metin görüntüleyemeyecek olacaktır.

> [!NOTE]
> Apple davranır Portekizce yaşadığınız biraz daha farklı beklediğini unutmayın.
> Gelen [kendi belgeleri](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"olarak Portekiz içinde kullanılan Brezilya ve pt-PT dil kimliği için Portekizce kullanıldığı gibi pt dil kimliği için Portekizce kullan"_.
> Bu zaman anlamına gelir Portekizce dil, geri dönüş dil olacaktır Portekizce (Brezilya), iOS bu davranışı değiştirmek için kod yazılmış sürece standart olmayan bir yerel ayarda seçilidir (gibi `ToDotnetFallbackLanguage` yukarıda).

#### <a name="android-application-project"></a>Android uygulama projesi

Android sunan aracılığıyla seçili yerel `Java.Util.Locale.Default`ve ayrıca (kendisi tarafından aşağıdaki kodu yerine) bir tire yerine bir alt çizgi ayırıcı kullanır. Bu bağımlılık hizmet uygulaması Android uygulaması projesine ekleyin:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch` Blokları `GetCurrentCultureInfo` yöntemi tam eşleşme, arama dili (yerel karakterlerin ilk blok) yalnızca temel yakın bir eşleşme bulunmazsa yerel ayar tanımlayıcıları ile – genellikle kullanılan geri dönüş davranışını taklit eden.
>
> Xamarin.Forms, söz konusu olduğunda bazı yerel Android geçerli olan, ancak geçerli bir benzemez `CultureInfo` .NET ve bu durumu çözmek için deneme yukarıdaki kodu.
>
> Geliştiriciler değiştirme `iOSToDotnetLanguage` ve `ToDotnetFallbackLanguage` kendi desteklenen diller için gerekli özel durumları işlemek için yöntemleri.


Bu kod Android uygulaması projesine eklendikten sonra otomatik olarak çevrilen dizeleri görüntüleme kuramaz.

> [!NOTE]
>️ **Uyarı:** çevrilen dizelerin yayın Android derlemeleriniz ancak değil hata ayıklama sırasında çalışıyorsanız, sağ tıklayın **Android projesi** seçip **Seçenekleri > Yapı > Android Yapı** ve emin **hızlı derleme dağıtım** değil ticked. Bu seçenek kaynakları yükleme ile ilgili sorunlara neden olur ve yerelleştirilmiş uygulamaları sınıyorsanız kullanılmamalıdır.

#### <a name="windows-application-projects"></a>Windows Uygulama projeleri

Windows 8.1 ve evrensel Windows Platformu (UWP) projeleri bağımlılık hizmeti gerektirmez – bu platformlar kaynağın kültür doğru şekilde otomatik olarak ayarlanır.

Bu belgenin sonraki bölümlerinde açıklanan XAML biçimlendirme uzantısı uygulama gerektirebilir `ILocalize` Windows Phone için aşağıda gösterilen uygulama.

##### <a name="windows-phone-80"></a>Windows Phone 8.0

Kullanılan değil ancak `App` sınıfı, Windows Phone uygulamasını işte `ILocalize` bağımlılık hizmeti. Bu sınıf Windows Phone uygulaması projesine ekleyin; Daha sonra açıklanan XAML biçimlendirme uzantısı uyguluyorsanız, gerekli olacaktır:

```csharp
[assembly: Dependency(typeof(UsingResxLocalization.WinPhone.Localize))]

namespace UsingResxLocalization.WinPhone
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci) { }
        public System.Globalization.CultureInfo GetCurrentCultureInfo ()
        {
            return System.Threading.Thread.CurrentThread.CurrentUICulture;
        }
    }
}

```

Windows Phone 8.0 projeleri düzgün bir şekilde görüntülenecek yerelleştirilmiş metni için yapılandırılmış olması gerekir.
Proje seçenekleri desteklenen diller seçili *ve* **WMAppManifest.xml** dosyaları.
Bu ayarları işlenmediyse yerelleştirilmiş RESX kaynaklar yüklenmeyecek.

##### <a name="project-options"></a>Proje seçenekleri

Windows Phone projeye sağ tıklayıp **özellikleri**. İçinde **uygulama** sekmesinde onay **desteklenen kültürler** uygulamasının desteklediği:

[ ![](localization-images/winphone-projectproperties-sml.png "Proje Özellikleri - desteklenen kültürler")](localization-images/winphone-projectproperties.png "özellikleri - desteklenen kültürler proje")

##### <a name="wmappmanifestxml"></a>WMAppManifest.xml

Windows Phone proje özellikleri düğümünü genişletin ve çift **WMAppManifest.xml** dosya. Tıklayın **paketleme** sekmesinde ve uygulama tarafından desteklenen tüm dillerde değer.

[ ![](localization-images/winphone-wmappmanifest-sml.png "WMAppManifest.xml - desteklenen diller")](localization-images/winphone-wmappmanifest.png "WMAppManifest.xml - desteklenen diller")

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Taşınabilir sınıf kitaplığı (PCL) proje özellikleri düğümünü genişletin ve çift **AssemblyInfo.cs** dosya. Dilden bağımsız kaynak assembly dili İngilizce'ye ayarlamak için dosyasında aşağıdaki satırı ekleyin:

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Bu nedenle dil belirsiz RESX dosyasında tanımlanan dizeler sağlanarak Kaynak Yöneticisi'nin uygulamanın varsayılan kültürü bildirir (**AppResources.resx**) uygulama içinde bir İngilizce yerel ayar çalışırken görüntülenir.

### <a name="example"></a>Örnek

Yukarıdaki platforma özgü projeleri gösterildiği gibi güncelleştirme ve uygulama ile çevrilmiş RESX dosyaları yeniden derlenmesi sonra her uygulamada güncelleştirilmiş çevirileri kullanılabilir. Basitleştirilmiş Çince çevrilen örnek kod bir ekran görüntüsü aşağıda verilmiştir:

![](localization-images/simple-example-hans.png "Basitleştirilmiş Çince-çevrilen platformlar arası Uı'lar")

## <a name="localizing-xaml"></a>XAML yerelleştirme

Ne zaman Xamarin.Forms kullanıcı arabiriminde XAML işaretleme derleme dizelerle, benzer görünür doğrudan XML'de katıştırılmış:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

İdeal olarak, biz XAML'de hangi oluşturarak yapabiliriz doğrudan, kullanıcı arabirimi denetimlerini Çevir bir *biçimlendirme uzantısı*. XAML RESX kaynakları gösteren biçimlendirme uzantısı için kod aşağıda verilmiştir. Bu sınıf, Xamarin.Forms ortak kodun (birlikte XAML sayfaları) eklenmesi gerekir:

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in Xaml markup
    [ContentProperty ("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        private static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(()=> new ResourceManager(ResourceId
                                                                                                                  , typeof(TranslateExtension).GetTypeInfo().Assembly));

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public string Text { get; set; }

        public object ProvideValue (IServiceProvider serviceProvider)
        {
            if (Text == null)
                return "";

            var translation = ResMgr.Value.GetString(Text, ci);

            if (translation == null)
            {
                #if DEBUG
                throw new ArgumentException(
                    String.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
                #else
                translation = Text; // returns the key, which GETS DISPLAYED TO THE USER
                #endif
            }
            return translation;
        }
    }
}
```

Aşağıdaki madde işaretleri Yukarıdaki kod içindeki önemli öğelerini açıklamaktadır:

* Sınıf adlı `TranslateExtension`, ancak biz başvurabilir kural tarafından olduğu olarak **çevir** bizim biçimlendirme.
* Sınıf uygulayan `IMarkupExtension`, çalışmaya tarafından onun için Xamarin.Forms gerekli.
* `"UsingResxLocalization.Resx.AppResources"` RESX KAYNAKLARIMIZI kaynak tanımlayıcısıdır. Bizim varsayılan ad alanı, kaynak dosyalarının bulunduğu klasörü ve varsayılan RESX filename oluşur.
* `ResourceManager` Sınıfı kullanılarak oluşturulur `typeof(TranslateExtension)` kaynaklardan yüklemek için geçerli derleme belirlemek için.
* `ci` Seçilen kullanıcının dilini yerel işletim sisteminden almak için bağımlılık hizmeti kullanır.
* `GetString` kaynak dosyalarından gerçek çevrilmiş dizesini alır yöntemidir. Windows Phone 8.1 ve evrensel Windows platformu `ci` null olur çünkü `ILocalize` arabirimi bu platformlarda uygulanan değil. Bu arama için eşdeğerdir `GetString` yöntemi yalnızca ilk parametresine sahip. Bunun yerine, kaynakların framework yerel otomatik olarak algılar ve çevrilmiş dize uygun RESX dosyasından alır.
* Hata işleme bir özel durum atma tarafından eksik kaynakları hata ayıklama yardımcı olmak için birlikte (içinde `DEBUG` yalnızca modu).

Aşağıdaki XAML parçacığını biçimlendirme uzantısı kullanmayı gösterir. Çalışması için iki adım vardır:

1. Özel bildirme `xmlns:i18n` ad alanında kök düğümü. `namespace` Ve `assembly` tam olarak - bu örnekte bunlar aynıdır ancak projenizde farklı olabilir proje ayarları eşleşmelidir.
2. Kullanım `{Binding}` aramak için metin normalde içerecektir özniteliklerinde sözdizimi `Translate` biçimlendirme uzantısı. Kaynak anahtarı yalnızca gerekli parametredir.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

Aşağıdaki daha ayrıntılı sözdizimi biçimlendirme uzantısı için geçerli değildir:

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>Platforma özgü öğeleri yerelleştirme

Biz Xamarin.Forms kod kullanıcı arabiriminde çevrilmesi işleyebilir olmasa da, her platforma özgü projesinde yapılması gereken bazı öğeler vardır. Bu bölümde, yerelleştirme nasıl ele alınacaktır:

* Uygulama Adı
* Görüntüler

Örnek Proje adında yerelleştirilmiş bir görüntü içerdiğinden **flag.png**, hangi başvurulan C# gibi:

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

Bayrak görüntü ayrıca şöyle XAML'de başvurulur:

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

Aşağıda açıklandığı proje yapılarını uygulanan sürece tüm platformlar otomatik olarak bu görüntüleri yerelleştirilmiş sürümleri için görüntü başvuruları çözümleyin.

### <a name="ios-application-project"></a>iOS uygulama projesi

iOS yerelleştirme projeleri adlı bir adlandırma standardı kullanır veya **.lproj** görüntü ve dize kaynaklarını içeren dizinleri. Bu dizinleri uygulamada kullanılan görüntüleri yerelleştirilmiş sürümleri içerebilir ve ayrıca **InfoPlist.strings** uygulama adı yerelleştirme için kullanılan dosya.

#### <a name="images"></a>Görüntüler

Bu ekran dile özgü iOS örnek uygulamasıyla gösterir **.lproj** dizinleri. İspanyolca dizini olarak da adlandırılır **es.lproj**, varsayılan görüntü yerelleştirilmiş sürümlerini içeren yanı **flag.png**:

![](localization-images/ios-resources.png "iOS yerelleştirme proje dizinleri")

Her dil dizini bir kopyasını içeren **flag.png**, o dil için yerelleştirilmiş. Hiçbir resim sağlanırsa, varsayılan işletim sistemi görüntüsüne varsayılan dil dizine. Tam Retina desteği, sağladığınız  **@2x**  ve  **@3x**  her görüntü kopyalarını.

#### <a name="app-name"></a>Uygulama adı

İçeriğini **InfoPlist.strings** yalnızca tek bir anahtar-uygulama adını yapılandırmak için değerdir:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Uygulamayı çalıştırdığınızda, hem uygulama adı ve görüntü yerelleştirilmiş:

![](localization-images/ios-imageicon.png "iOS örnek uygulama metin ve görüntü yerelleştirme")

### <a name="android-application-project"></a>Android uygulama projesi

Android izleyen farklı kullanarak yerelleştirilmiş görüntüleri depolamak için farklı bir düzeni **drawable** ve **dizeleri** dizinleri dil kodu soneki. Dört harfli yerel ayar kodu (zh-TW veya pt-BR gibi) gerekli olduğunda, Android ek gerektirir **r** çizgi/önceki aşağıdaki yerel ayar kod (ör.) zh-rTW veya pt rBR).

#### <a name="images"></a>Görüntüler

Bu ekran Android gösteren örnek bir bazı yerelleştirilmiş drawables ve dizeleri:

![](localization-images/android-resources.png "Android Drawables ve dize dizinleri yerelleştirilmiş")

Not Android zh-atanır kullanmaz ve zh-Hant kodları Basitleştirilmiş ve Geleneksel Çince; Bunun yerine, yalnızca Ülkeye özel kodlarını zh-CN ve zh-TW destekler.

Farklı bir çözünürlük görüntüleri için yüksek yoğunluklu ekranlar desteklemek için ek dil klasörlerle oluşturmak `-*dpi` gibi sonekleri **drawables es mdpi**, **drawables es xdpi**, **drawables es xxdpi**, vb. Bkz: [alternatif Android kaynakları sağlama](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) daha fazla bilgi için.

#### <a name="app-name"></a>Uygulama adı

İçeriğini **strings.xml** yalnızca tek bir anahtar-uygulama adını yapılandırmak için değerdir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Güncelleştirme **MainActivity.cs** Android uygulama projesi, böylece `Label` XML dizeleri başvuruyor.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

Uygulamayı şimdi uygulama adı ve görüntü yerelletirilmesi. Bir ekran görüntüsünü sonucu (İspanyolca) aşağıdadır:

![](localization-images/android-imageicon.png "Android örnek uygulama metin ve görüntü yerelleştirme")

### <a name="windows-phone-80-application-project"></a>Windows Phone 8.0 uygulama projesi

Windows Phone basit bir yerleşik yöntem belirli bir yerelleştirilmiş görüntüsü seçme veya uygulama adı yerelleştirme sahip değil.

#### <a name="images"></a>Görüntüler

Yükleme görüntüsünü kullanarak nasıl uygulayabilir yerelleştirilmiş bu sınırlamaya geçici almak için örnek bir öneri sunar bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) için `Image` denetim.

Özel oluşturucu kodu - aşağıda gösterilen kaynağı ise bir `FileImageSource` filename ayıklar ve yerelleştirilmiş görüntü kullanarak bir yolu derlemeler `CurrentUICulture`. Böylece geri dönüşler beklendiği gibi çalışmayabilir bazı dillerde özel işlem gerektiren; örnekte varsayılan yalnızca iki harfli dil kodunu dışındaki bazı özel durumlarda kullanmaktır:

```csharp
using System.IO;
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinPhone;

[assembly: ExportRenderer(typeof(Image), typeof(UsingResxLocalization.WinPhone.LocalizedImageRenderer))]
namespace UsingResxLocalization.WinPhone
{
    public class LocalizedImageRenderer : ImageRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Image> e)
        {
            base.OnElementChanged(e);

            if (e.NewElement != null)
            {
                var s = e.NewElement.Source as FileImageSource;
                if (s != null)
                {
                    var fileName = s.File;
                    string ci = System.Threading.Thread.CurrentThread.CurrentUICulture.ToString();
                    // you might need some custom logic here to support particular cultures and fallbacks
                    if (ci == "pt-BR") {
                        // use the complete string 'as is'
                    } else if (ci == "zh-CN") {
                         // we could have named the image directories differently,
                         // but this keeps them consisent with RESX file naming
                        ci = "zh-Hans";
                    } else if (ci == "zh-TW" || ci == "zh-HK") {
                        ci = "zh-Hant";
                    } else {
                        // for all others, just use the two-character language code
                        ci = System.Threading.Thread.CurrentThread.CurrentUICulture.TwoLetterISOLanguageName;
                    }
                    e.NewElement.Source = Path.Combine("Assets/" + ci + "/" + fileName);
                }
            }
        }
    }
}
```

Bu kod, aşağıda gösterilen dizin yapısına yerelleştirilmiş görüntülerle çalışır. (Daha özel yerel ayarlar işleme ve geri görüntüleri olmadığında dönmeden) gibi belirli yerelleştirme gereksinimlerinizi karşılayacak şekilde kodu değiştirmeniz önerilir:

![](localization-images/winphone-resources.png "WinPhone yerelleştirilmiş görüntü dizin yapısı")

Windows Phone şimdi görüntü yerelletirilmesi. Bir ekran görüntüsünü sonucu (İspanyolca ve Basitleştirilmiş Çince) aşağıdadır:

![](localization-images/winphone-image-sml.png "WinPhone örnek uygulama metin ve görüntü yerelleştirme")

#### <a name="app-name"></a>Uygulama adı

Microsoft'un belgelerine bakın [Windows Phone 8.0 Uygulama Başlığı yerelleştirme](http://msdn.microsoft.com/library/windows/apps/ff967550(v=vs.105).aspx).

### <a name="windows-phone-81-and-universal-windows-platform-application-projects"></a>Windows Phone 8.1 ve evrensel Windows platformu uygulama projeleri

Windows Phone 8.1 ve evrensel Windows platformu ikisi de yerelleştirilmesi resimler ve uygulama adı basitleştiren bir kaynak altyapısı sahip.

#### <a name="images"></a>Görüntüler

Görüntüleri bir kaynağa özel klasöre yerleştirerek aşağıdaki ekran görüntüsünde gösterildiği gibi yerelleştirilebilir:

![](localization-images/uwp-image-folder-structure.png "WinPhone 8.1 ve UWP görüntü yerelleştirme klasör yapısı")

Çalışma zamanında Windows Kaynak Altyapısı kullanıcının bölgesel ayarına göre uygun görüntüyü seçer.

#### <a name="app-name"></a>Uygulama adı

Microsoft'un belgelerine bakın [Windows 8.1 mağazası uygulamalarını: uygulamanızı kullanıcılara açıklayan bilgileri yerelleştirme](https://msdn.microsoft.com/library/windows/apps/hh454044.aspx) ve [uygulama bildirimden dizeleri Yükleniyor](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323.aspx#loading_strings_from_the_app_manifest.).

## <a name="summary"></a>Özet

Xamarin.Forms uygulamaları RESX dosyaları ve .NET Genelleştirme sınıflarını kullanarak yerelleştirilmiş olmalıdır. Kullanıcı tercih hangi dilde algılamaya yönelik platforma özgü kodu az miktarda dışında çoğu yerelleştirme çaba merkezi ortak kodu.

Görüntüleri genellikle hem iOS hem de Android sağlanan çok çözünürlük desteği yararlanmak için platforma özgü şekilde ele alınır. Windows Phone görüntüleri bir çapraz platform kolay şekilde yerelleştirme için özel kod gerektirir; Örnek kod, bu özelliği eklemek için sağlandı.


## <a name="related-links"></a>İlgili bağlantılar

- [RESX yerelleştirme örnek](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized örnek uygulaması](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Platformlar arası yerelleştirme](~/cross-platform/app-fundamentals/localization.md)
- [iOS yerelleştirme](~/ios/app-fundamentals/localization/index.md)
- [Android yerelleştirme](~/android/app-fundamentals/localization.md)
- [CultureInfo sınıfı (MSDN) kullanma](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Bulma ve kaynakların belirli bir kültür için (MSDN) kullanma](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
