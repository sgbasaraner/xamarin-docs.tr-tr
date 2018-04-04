---
title: C# 6 yeni genel bakış özellikleri
description: C# dili – sürüm 6 – en son sürümünü daha az Demirbaş, geliştirilmiş daha anlaşılır olması ve daha fazla tutarlılık sağlamak için dil gelişmeye devam eder. Temizleyici başlatma sözdizimi kullanma yeteneğini catch/finally blokları ve null-conditional await? işleç özellikle kullanışlıdır.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.technology: xamarin-cross-platform
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 2a189a19280576876e5d5a6a4fa34d2d00cab330
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="c-6-new-features-overview"></a>C# 6 yeni genel bakış özellikleri

_C# dili – sürüm 6 – en son sürümünü daha az Demirbaş, geliştirilmiş daha anlaşılır olması ve daha fazla tutarlılık sağlamak için dil gelişmeye devam eder. Temizleyici başlatma sözdizimi kullanma yeteneğini catch/finally blokları ve null-conditional await? işleç özellikle kullanışlıdır._

Bu belge, C# 6'ın yeni özellikler sunar. Mono derleyici tarafından tam olarak desteklenir ve geliştiricilerin tüm Xamarin Hedef platformlar genelinde yeni özellikleri kullanmaya başlayabilirsiniz.

Bu makalede temel kullanım göstermeye kısa C# 6 kod parçacıkları içerir.
Örnek uygulama, tüm Xamarin hedef platformlarında çalışır ve çeşitli özellikleri uygular komut satırı bir programdır.


> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Göre C# 6'da, yenilikler [Xamarin Üniversitesi](https://university.xamarin.com/)**


## <a name="development-environment"></a>Geliştirme ortamı

### <a name="mac"></a>Mac

* **Mac için Visual Studio** C# 6 desteği olan: derleme ve C# 6 özellikleri kullanarak Xamarin uygulamaları derleyin.
  Daha fazla bilgi edinin [Mac için Visual Studio](https://docs.microsoft.com/visualstudio/mac/).

### <a name="windows"></a>Windows

* **Visual Studio 2015 ve 2017** ve yukarıdaki C# 6 için tam destek vardır. Visual Studio'nun önceki sürümleri C# 6 desteklemez.

* **Windows için Xamarin Studio** C# 6 özellikleri Düzenleyicisi'nde desteklemiyor.



## <a name="compiler"></a>Derleyici

Mono C# 6 derleyici olan Mono olarak 4.0 ve üzeri, dahil [serbestçe İndirilebilecek](http://www.mono-project.com/download/).
Mac için Visual Studio sisteminizdeki Mono yükleme otomatik olarak güncelleştirir.

Windows kullanıcıları olmalıdır [Visual Studio 2015 veya 2017 ^](https://www.visualstudio.com/) (Windows için Xamarin Studio IDE'yi seçtiğiniz olsa bile) C# 6 Kodu derlemek için yüklü.

^ veya *[Microsoft derleme araçları 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48159)* komutu için derleme veya derleme sunucuları, örneğin satır.

## <a name="using-c-6"></a>C# 6 kullanarak

C# 6 derleyici tüm en son Visual Studio sürümlerinde Mac için kullanılır
Bu komut satırı derleyicileri kullanarak onaylamalısınız `mcs --version` 4.0 veya sonraki döndürür.
Mac kullanıcıları için Visual Studio Mono 4 (veya daha yeni) varsa yüklü başvurarak kontrol edebilirsiniz **Mac için Visual Studio hakkında > Mac için Visual Studio > ayrıntıları göster**.

## <a name="less-boilerplate"></a>Daha az Demirbaş
### <a name="using-static"></a>statik kullanma
Numaralandırmalar ve gibi belirli sınıfları `System.Math`, öncelikle statik değerler ve işlevleri sahiplerini şunlardır. C# 6'da, tüm statik üyeleri bir türde tek bir aktarabilirsiniz `using static` deyimi. C# 5 ve C# 6 tipik bir trigonometrik işlevinde karşılaştırın:

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static` Ortak yapmaz `const` gibi alanları `Math.PI` ve `Math.E`, doğrudan erişilebilir:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>Genişletme yöntemleri ile statik kullanma

`using static` Tesis çalışır biraz farklı ile genişletme yöntemleri. Genişletme yöntemleri kullanılarak yazılsa `static`, üzerinde çalışılacak bir örneği olmadan anlamlı yok. Böyle olduğunda `using static` genişletme yöntemleri, bunların hedef türünü kullanılabilir hale genişletme yöntemleri tanımlayan bir türü ile birlikte kullanılan (yöntemin `this` türü). Örneğin, `using static System.Linq.Enumerable` API'si genişletmek için kullanılan `IEnumerable<T>` LINQ türlerinin tümü getiren olmadan nesneler:

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

Önceki örnekte davranış farkı gösterir: genişletme yöntemi `Enumerable.Where` statik yöntemi sırasında dizisi ile ilişkili `String.Join` başvuru olmadan adlı `String` türü.

### <a name="nameof-expressions"></a>nameof ifadeleri
Bazı durumlarda, başvurmak istediğinizde adına bir değişken veya alan verdiniz. C# 6 `nameof(someVariableOrFieldOrType)` dize döndürülecek `"someVariableOrFieldOrType"`. Örneğin, atarken bir `ArgumentException` hangi bağımsız değişkeni geçersiz adlandırmak istediğiniz büyük olasılıkla:

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

Baş avantajı `nameof` ifadeleri olduğundan türü işaretli ve aracı destekli yeniden düzenleme ile uyumludur. Tür denetleme, `nameof` ifadeleri durumlarda özellikle Hoş Geldiniz burada bir `string` dinamik olarak türleri ilişkilendirmek için kullanılır. Örneğin, iOS içinde bir `string` prototip için kullanılan türünü belirtmek için kullanılan `UITableViewCell` nesnelerini bir `UITableView`. `nameof` Bu ilişkilendirme bir yazım hatası veya Özensiz yeniden düzenleme nedeniyle başarısız olmaz güvence altına almak:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

Bir tam ad geçirebilirsiniz ancak `nameof`, yalnızca son öğesi (son sonra `.`) döndürülür. Örneğin, Xamarin.Forms içinde veri bağlama ekleyebilirsiniz:

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

İki çağrılar `SetBinding` aynı değerlere geçirme: `nameof(ReactiveType.StringField)` olan `"StringField"`değil `"ReactiveType.StringField"` başlangıçta beklediğiniz.

## <a name="null-conditional-operator"></a>Null-conditional işleci
C# önceki güncelleştirmeleri tanıtılan boş değer atanabilir türler ve null birleşim işlecinin kavramlarını `??` boş değer atanabilir değer işlerken Demirbaş kod miktarını azaltmak için. C# 6 Bu tema "null-conditional işleci" ile devam `?.`. Nesne değilse ifade sağ taraftaki bir nesne üzerinde kullanıldığında, null-conditional işleci üye değerini döndürür `null` ve `null` Aksi durumda:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(Her ikisi de `length0` ve `length1` türünde olması olayla `int?`)

Önceki örnekte gösterildiği son satırında `?` null-conditional işleci ile birlikte `??` null birleşim işleci. Yeni C# 6 null-conditional işleci döndürür `null` hangi noktada null birleşim işlecinin devreye girer ve 0 sağlayan dizisindeki 2 öğede `lengths` (uygun veya, doğal olarak, sorunu özgü değildir olup olmadığını) dizisi.

Null-conditional işleci büyük ölçüde Demirbaş null denetimi gereken birçok, birçok uygulamalarda miktarını azaltmanız gerekir.

Null-conditional işleci belirsizlikleri nedeniyle bazı sınırlamalar vardır. Hemen izlenemiyor bir `?` parantez içine alınmış bir bağımsız değişken listesi ile gibi umuyoruz bir temsilci ile yapmak:

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Ancak, `Invoke` ayırmak için kullanılan `?` bağımsız değişkeni listesinden ve hala işaretli geliştirme üzerinden bir `null`-Demirbaş bloğunu denetleniyor:

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Dize ilişkilendirme
`String.Format` İşlevi geleneksel olarak kullanılan dizinlerini biçim dizesindeki yer tutucu olarak ör `String.Format("Expected: {0} Received: {1}.", expected, received`). Elbette, yeni değer ekleme her zaman rahatsız edici küçük bir görev bağımsız değişkenleri sayım, yer tutucular zahmetli ve yeni değişken bağımsız değişken listesinde doğru sırayla ekleme dahil.

C# 6'ın yeni dize ilişkilendirme özellik büyük ölçüde artırır üzerine `String.Format`. Şimdi, doğrudan önekine sahip bir dize değişkenleri adlandırabilirsiniz bir `$`. Örneğin:

```csharp
$"Expected: {expected} Received: {received}."
```

Değişkenleri doğal olarak, denetlenir ve yanlış yazılmış veya kullanılabilir olmayan bir değişken derleyici hatası neden olur.

Yer tutucuları basit değişkenlerin olması gerekmez, herhangi bir ifade olabilir. Bu yer tutucularını, tırnak işaretleri kullanabilirsiniz *olmadan* bu teklifleri kaçış. Örneğin, Not `"s"` aşağıdaki:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Dize ilişkilendirme hizalama ve biçimlendirme sözdizimini destekler `String.Format`. Bıraktığınız gibi daha önce yazmış `{index, alignment:format}`, C# 6 yazdığınız `{placeholder, alignment:format}`:

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```
Sonuçları:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

Dize ilişkilendirme olduğu için söz dizim sugar `String.Format`: ile kullanılamaz `@""` dize değişmez değerleri ve ile uyumlu olmayan `const`yer tutucular kullanılan olsa bile:

```csharp
const string s = $"Foo"; //Error : const requires value
```

Ortak kullanın-işlev bağımsız değişkenleri dize ilişkilendirme ile oluşturmanın durumda hala kaçış, kodlama ve kültür sorunları hakkında dikkatli olmanız gerekir. SQL ve URL sorguları, doğal olarak, temizlenmeye için kritik öneme sahiptir. İle `String.Format`, dize ilişkilendirme kullanır `CultureInfo.CurrentCulture`. Kullanarak `CultureInfo.InvariantCulture` biraz daha uzundur:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Başlatma

C# 6 çeşitli özellikleri, alanları ve üyeleri belirtmek için kısa yollarla sağlar.

### <a name="auto-property-initialization"></a>Otomatik özelliği başlatma

Otomatik özellikleri artık alanları aynı kısa şekilde başlatılabilir. Değişmez otomatik özellikleri yalnızca bir alıcı ile yazılabilir:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

Oluşturucuda bir salt değer alıcı otomatik özelliğinin değeri ayarlayabilirsiniz:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

Bu başlatma otomatik özelliklerinin nesnelerine girişi vurgulamak isteyen geliştiriciler için katkıda bulunuyor ve genel yer kaplayan özellik ' dir.

### <a name="index-initializers"></a>Dizin başlatıcıları

C# 6, anahtar ve değer bir dizin oluşturucu sahip türlerinde ayarlamanıza olanak tanır dizin başlatıcıları tanıtır. Genellikle, bu içindir `Dictionary`-stil veri yapıları:

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>İşlev ifadesi bodied üyeleri

Lambda işlevleri biri yalnızca alanı kaydediliyor birkaç avantaj vardır. Benzer şekilde, ifade bodied sınıf üyeleri C# 6 önceki sürümlerde mümkün olandan biraz daha temellerini ifade küçük işlevler izin verir.

İşlev ifadesi bodied üyeleri geleneksel blok sözdizimi yerine lambda ok sözdizimini kullanın:

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Lambda ok sözdizimi açık bir kullanmaz bildirimi `return`. Bu dönüş işlevleri için `void`, ifade da bir deyim olmalıdır:

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

İfade bodied hala tabi kural üyeleridir, `async` yöntemleri ancak özellikleri için desteklenir:

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Özel Durumlar

İlgili hiçbir iki yolu vardır: özel durum işleme sağ almak sabit. C# 6'daki yeni özellikleri, daha esnek ve tutarlı özel durum işleme olun.

### <a name="exception-filters"></a>Özel durum filtreleri

Tanımı gereği, olağan dışı durumlarda özel durumlar ortaya ve nedeni ve kod hakkında oldukça zor olabilir *tüm* yolları belirli türünde bir özel durum oluşabilir. C# 6 çalışma zamanı hesaplanan bir filtreyle bir yürütme işleyicisi koruma olanağı sunar. Bu ekleyerek yapılır bir `when (bool)` normal sonra desen `catch(ExceptionType)` bildirimi. Aşağıda, bir filtre ile ilgili bir ayrıştırma hatası ayırt `date` diğer ayrıştırma hataları aksine parametresi.

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>await catch... finally...

`async` C# 5'te kullanıma sunulan yetenekler, oyun değiştirici dil olmuştur. C# 5, `await` içinde izin verilmiyor `catch` ve `finally` engeller, değerinin verilen bir sıkıntı getirir `async/await` yeteneği. C# 6 tutarlı bir şekilde program aracılığıyla aşağıdaki kod parçacığında gösterildiği gibi beklemenin zaman uyumsuz sonuç izin vererek bu sınırlamayı kaldırır:

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>Özet

C# dili de iyi yöntemler yükseltme ve araç destekleme sırasında geliştiriciler daha üretken yapmak için gelişmeye devam eder. Bu belge yeni dil özelliklerine C# 6'verdiği ve bunların nasıl kullanıldığı kısaca gösterilmiştir.

## <a name="related-links"></a>İlgili bağlantılar

- [C# 6 yeni dil özellikleri](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
- [C# 6 kullanarak çalışma kitabı](https://developer.xamarin.com/workbooks/csharp/csharp6/csharp6.workbook)
