---
title: "Objective-C kitaplıkları bağlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 29b846453396d37adc689fe49e80299e8f35bbe2
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="binding-objective-c-libraries"></a>Objective-C kitaplıkları bağlama

Xamarin.iOS veya Xamarin.Mac ile çalışırken, bir üçüncü taraf Objective-C Kitaplığı kullanmak istediğiniz durumlarda karşılaşabilirsiniz. Bu durumlarda, yerel Objective-C kitaplıkları için C# bağlama oluşturmak için Xamarin bağlama projeleri kullanabilirsiniz. Proje iOS ve Mac API'ları C# getirmek için kullanırız aynı araçları kullanır.

Bu belge, Objective-C API ' larını bağlamak açıklar yalnızca C API'lerini bağlanıyorsanız, standart .NET mekanizmasını bunun için kullanmanız gereken [P/Invoke framework](http://mono-project.com/Dllimport).
Statik olarak bağlantı bir C Kitaplığı hakkında ayrıntılar bulunur [bağlama yerel kitaplıkları](~/ios/platform/native-interop.md) sayfası.

Bizim Yardımcısı bkz [türleri Başvuru Kılavuzu bağlama](~/cross-platform/macios/binding/binding-types-reference.md).
Ayrıca, başlık altında neler olduğu hakkında daha fazla bilgi edinmek istiyorsanız, işaretleyin bizim [bağlama genel bakış](~/cross-platform/macios/binding/overview.md) sayfası.

İOS ve Mac kitaplıklar için bağlamaları oluşturulabilir.
Bu sayfa, bağlama, ancak Mac bağlamaları çok benzer bir iOS iş açıklar.

**İOS için örnek kod**

Kullanabileceğiniz [iOS bağlama örnek](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) bağlamalarla denemek için proje.

<a name="Getting_Started" />

## <a name="getting-started"></a>Başlarken

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bir bağlama oluşturmak için en kolay yolu, bir Xamarin.iOS bağlama projesi oluşturmaktır.
Visual Studio'dan Mac için proje türünü seçerek bunu yapabilirsiniz **iOS > kitaplığı > bağlamaları Kitaplığı**:

[![](objective-c-libraries-images/00-sml.png "Proje türü, iOS kitaplığı bağlamaları kitaplığı seçerek Mac için bunu Visual Studio'dan")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bir bağlama oluşturmak için en kolay yolu, bir Xamarin.iOS bağlama projesi oluşturmaktır.
Visual Studio'da Windows proje türü seçerek bunu yapabilirsiniz **Visual C# > iOS > bağlamaları kitaplığı (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS bağlamaları kitaplığı iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Not: Bağlama projeleri için **Xamarin.Mac** Visual Studio'da Mac için yalnızca desteklenir

-----

Oluşturulan proje düzenleyebileceğiniz küçük bir şablon içerir, iki dosya içerir: `ApiDefinition.cs` ve `StructsAndEnums.cs`.

`ApiDefinition.cs` API sözleşme burada tanımlayacaksınız olduğundan bu nasıl temel Objective-C API C# diline yansıtılır açıklayan dosyasıdır. Sözdizimi ve bu dosyanın içeriğini tartışma bu belgenin ana konu ve içeriğini C# arabirimleri ve C# temsilci bildirimleri sınırlıdır. `StructsAndEnums.cs` Dosya gireceğiniz gereken tanımları Temsilciler ve arabirimleri dosyasıdır. Bu numaralandırma değerlerini ve kodunuzu kullanabilir yapılar içerir.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Bir API bağlama

Kapsamlı bir bağlama yapmak için Objective-C API tanımı anlamak ve .NET Framework tasarım yönergeleri ile tanımak isteyecektir.

Kitaplığınızı bağlamak için genellikle bir API tanımı dosyası ile başlar. API tanımlama dosyası bağlama sürücü Yardım öznitelikleri bir dizi açıklama C# arabirimleri içeren yalnızca bir C# kaynak dosyasıdır.  Bu ne C# ve Objective-C arasındaki sözleşmesi nedir tanımlayan bir dosyadır.

Örneğin, bu önemsiz bir API dosya bir kitaplık için şöyledir:

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

Yukarıdaki örnek adlı bir sınıf tanımlar `Cocos2D.Camera` , türetilen `NSObject` temel türü (Bu tür geldiği `Foundation.NSObject`) ve bir statik özellik tanımlar (`ZEye`), bağımsız değişkenler ve yöntemi alın iki yöntem üç alır bağımsız değişkenler.

API dosyası ve kullanabileceğiniz öznitelikler biçimi için ayrıntılı bir tartışma içinde ele [API tanımı dosyası](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) bölümüne bakın.

Tam bir bağlama üretmek için genellikle dört bileşenleri ile ilgilenecektir:

-  API tanımlama dosyası (`ApiDefinition.cs` şablondaki).
-  İsteğe bağlı: Tüm numaralandırmaları türleri, API tanımı dosyası tarafından gerekli yapılar (`StructsAndEnums.cs` şablondaki).
-  İsteğe bağlı: oluşturulan bağlama genişletmek veya bir daha fazla C# kolay API (projeye eklediğiniz tüm C# dosyalarını) sağlayan ek kaynakları.
-  Bağlama yerel kitaplığı.

Bu grafik dosyaları ilişkiyi gösterir:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Bu grafik dosyaları ilişkiyi gösterir")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

API tanımı dosyası yalnızca (bir arabirim içeren tüm üyeleri olan), ad alanları ve arabirim tanımlarını içerir ve sınıflar, numaralandırmalar, temsilciler veya yapılar içermemesi gerekir. API tanımlama dosyası yalnızca API oluşturmak için kullanılan Sözleşme ' dir.

Numaralandırmalar ister veya destek sınıfları alanınızın barındırılması "CameraMode" Yukarıdaki örnekte ayrı bir dosya üzerindeki fazladan kod CS dosyasında yok ve ayrı bir dosyada örneğin barındırılmalıdır bir numaralandırma değeridir `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` Dosyası ile birlikte `StructsAndEnum` sınıfı ve çekirdek bağlama kitaplığı oluşturmak için kullanılır. Sonuçta elde edilen kitaplık olarak kullanabileceğiniz-ancak genel olarak, bazı C# özelliklerini kullanıcılarınızın yararlanması eklemek için sonuçta elde edilen kitaplık ayarlamak istediğiniz sayıdır. Uygulama bazı örnekler bir `ToString()` yöntemi, C# dizin oluşturucular sağlamak, örtük dönüşümler ve bazı yerel türlerinden ekleyin veya bazı yöntemler kesin türü belirtilmiş sürümleri sağlar. Bu geliştirmeler, ek C# dosyalarında depolanır. Yalnızca C# dosyalarını projenize ekleyin ve bu derleme işlemde eklenecektir.

Bu nasıl kodda uygulamak gösterir, `Extra.cs` dosya. Bunlar bileşiminden üretilen kısmi sınıflar büyütmek gibi kısmi sınıflar kullanacağınız olduğunu fark `ApiDefinition.cs` ve `StructsAndEnums.cs` çekirdek bağlama:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Oluşturma kitaplığına yerel bağlama oluşturur.

Bu bağlama tamamlamak için yerel kitaplık projesine eklemeniz gerekir.  Bu projeye sağ tıklayıp seçerek projenize, Çözüm Gezgini'nde projeye üzerine Bulucu yerel kitaplığı bırakarak veya yerel kitaplığı ekleyerek yapabilirsiniz **Ekle**  >  **Dosyaları Ekle** yerel kitaplığı seçin.
Kural tarafından yerel kitaplıkları "LIB" sözcüğüyle başlayan ve "bir" uzantısı ile bitmelidir. Bunu yaparken, Visual Studio Mac için iki dosya ekleyecektir: bir dosya ve ne yerel kitaplığı içerir hakkında bilgi içeren bir otomatik olarak doldurulan C# dosyası:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Kural tarafından yerel kitaplıkları word lib ile başlayıp bitmelidir ile uzantısı bir")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

İçeriğini `libMagicChord.linkwith.cs` dosya bu kitaplığı nasıl kullanılabileceği hakkında bilgi sahibi olan ve sonuçta elde edilen DLL dosyasına bu ikili paketlemek için IDE'yi bildirir:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Tam nasıl kullanılacağı hakkında ayrıntılar [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) özniteliği konusunda belgelenir [türleri Başvuru Kılavuzu bağlama](~/cross-platform/macios/binding/binding-types-reference.md).

Projeyi derlerken, durumuyla karşılaşırsınız artık bir `MagicChords.dll` bağlama ve yerel kitaplığı içeren dosya. Bu proje dağıtabilirsiniz veya diğer geliştiriciler için kendi elde edilen DLL'e kullanın.

Bazen birkaç numaralandırma değerlerinin, temsilci tanımları veya diğer türleri gerektiğini fark edebilirsiniz. Bu yalnızca bir sözleşme olduğu gibi API tanımlarını dosyası de yerleştirmeyin

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>API tanımlama dosyası

API tanımlama dosyası arabirimleri sayısı oluşur. API tanımı arabirimlerde sınıf bildirimi içine kapatılacak ve ile tasarlanmalıdır [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) özniteliği sınıf için temel sınıf belirtin.

Neden biz sınıfları arabirimleri yerine sözleşme tanımı kullanmadı merak ediyor olabilirsiniz. Bize API tanımlama dosyası yöntemi gövdesinde sağlamak zorunda veya bir özel durum oluşturduğunda veya anlamlı bir değere gerekiyordu bir gövde sağlamak zorunda kalmadan bir yöntem için sözleşme yazmaya izin için biz arabirimleri Çekildi.

Ancak biz arabirimi bir çatısını bir sınıf oluşturmak için kullandığından biz bağlama sürücü sözleşme öznitelikleri olan çeşitli kısımlarını dekorasyon için çözümlemelere gerekiyordu.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Bağlama yöntemleri

Bir yöntem bağlamak için yapabileceğiniz basit bağlama var. Yalnızca C# adlandırma kuralları ile arabiriminde bir yöntem bildirin ve yöntemiyle dekorasyon [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Özniteliktir, C# adıyla Objective-C Xamarin.iOS çalışma zamanında ne bağlar. Parametresi, [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği Objective-C Seçici adıdır. Bazı örnekler:

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

Yukarıdaki örneklerde, örnek yöntemleri nasıl bağlayabilirsiniz gösterir. Statik yöntemler bağlamak için kullanmanız gerekir [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) özniteliği şuna benzer:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Bu sözleşme bir arabirim parçası olduğundan ve bir kez daha öznitelikleri çözümlemelere gerekli olmayacak biçimde statik vs örneği bildirimleri kavramı arabirimine sahip gereklidir. Belirli bir bağlama yönteminden gizlemek istiyorsanız, yöntemiyle tasarlamanız [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği.

`btouch-native` Komut başvurusu parametre null olmaması için denetimleri tanıtmak. Belirli bir parametre için null değerlere izin vermek istiyorsanız, kullanmak [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) şöyle parametre özniteliği:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Bir başvuru türü ile verilirken [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) anahtar sözcük ayırma semantiğini de belirtebilirsiniz. Bu, veri sızmış emin olmak gereklidir.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Bağlama özellikleri

Kullanarak Objective-C özelliklerine bağlı yöntemler gibi [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği ve C# özellikleri için doğrudan eşleme. Yalnızca yöntemleri gibi özellikleri tasarlanabilir [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) ve [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) öznitelikleri.

Kullandığınızda [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği kapsar btouch yerel altında bir özelliğe gerçekte iki yöntem bağlar: ' Set ' yordamı. Dışarı aktarmak için sağladığınız ad **basename** ve kurucu "Ayarla" ilk harfini kapatma, word eklenerek hesaplanır **basename** büyük harf ve ele Seçici yaparak içine bir bağımsız değişken. Bunun anlamı `[Export ("label")]` uygulanan bir özellik gerçekte "etiketi" bağlar ve "setLabel:" Objective-C yöntemleri.

Bazen Objective-C özellikleri yukarıda açıklanan düzeni izlemeyin ve adını el ile üzerine yazılır. Bu gibi durumlarda bağlama kullanılarak oluşturulur yolunu kontrol edebilirsiniz [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) alıcı veya ayarlayıcı, örneğin özniteliği:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Bu daha sonra "isMenuVisible" bağlar ve "setMenuVisible:". İsteğe bağlı olarak, aşağıdaki sözdizimini kullanarak bir özelliği bağlanabilir:

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

' Set'yordamı açıkça tanımlandığı gibi `name` ve `setName` bağlamaları yukarıdaki.

Kullanarak statik özellikleri için destek yanı sıra [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute), iş parçacığı statik özellikleri ile işaretleme [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), örneğin:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Yalnızca bazı parametreler ile işaretlenmesini yöntemlere izin gibi [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute), uygulayabileceğiniz [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) bu null bir özellik için geçerli bir değer örneğin belirtmek için bir özellik için:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Parametre doğrudan ayarlayıcı üzerinde de belirtilebilir:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Özel denetimler bağlama uyarılar

Aşağıdaki uyarılar özel bir denetim için bağlama ayarlama göz önünde bulundurulması:

1. **Bağlama özellikleri statik olmalıdır** - özellikleri, bağlama tanımlarken [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) özniteliği kullanılmalıdır.
 2. **Özellik adlarının tam olarak eşleşmelidir** -özelliği bağlamak için kullanılan ad özel denetiminde özelliğinin adı tam olarak eşleşmelidir.
3. **Özellik türleri tam olarak eşleşmelidir** -özelliği bağlamak için kullanılan değişken türü özel denetiminde özelliğinin türü ile tam olarak eşleşmelidir.
4. **Kesme noktaları ve alıcı/ayarlayıcı** - kesme noktaları yer işareti veya özellik ayarlayıcı yöntemlerini hiçbir zaman edeceği.
5. **Geri aramalar gözlemlemek** -özel denetimler özellik değerlerini değişikliklerin bildirilmesi gözlem geri aramalar kullanmanız gerekir.

Yukarıda listelenen uyarılar hiçbirini izlemek için hata sessizce çalışma zamanında başarısız olan bağlama neden olabilir.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Objective-C değişebilir düzeni ve özellikleri

Objective-C çerçeveleri bazı sınıflar ile değişebilir bir alt değişmez olduğu bir deyim kullanın. Örneğin `NSString` değişmez sürümü sırada `NSMutableString` mutation izin veren sınıfıdır.

Bu sınıfların bir alıcı, ancak hiçbir ayarlayıcı özelliklerini içeren değişmez temel sınıf görmek için yaygın bir sorundur. Kurucu tanıtmak değişebilir sürümü. Bu C# ile gerçekten mümkün olmadığından, bu deyim C# ile çalışır bir deyim içine eşleme içeriyor.

Bu C# eşlendiğinden hem alıcı hem de ayarlayıcı temel sınıfını eklemeyi, ancak setter işaretlemesini yoludur bir [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) özniteliği.

Ardından, değişebilir bir alt kümesi üzerinde kullandığınız [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) özelliği gerçekten üst öğenin davranışı geçersiz kılma olduğundan emin olmak için özellikte özniteliği.

Örnek:

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

<a name="Binding_Constructors" />

### <a name="binding-constructors"></a>Bağlama oluşturucular

`btouch-native` Aracı otomatik olarak oluşturacağını fours oluşturucuları için belirli bir sınıfın sınıfınızda `Foo`, onu oluşturur:

-  `Foo ()`: varsayılan Oluşturucusu (maps Objective-C'ın "Başlangıç" Oluşturucusu)
-  `Foo (NSCoder)`: NIB dosyaları seri kaldırma sırasında kullanılan Oluşturucusu (eşlemeleri Objective-C için 's "initWithCoder:" Oluşturucusu).
-  `Foo (IntPtr handle)`: Oluşturucusu tanıtıcı tabanlı oluşturma için yönetilmeyen bir nesneden yönetilen bir nesnenin kullanıma sunmak çalışma zamanı ihtiyacı olduğunda bu çalışma zamanı tarafından çağrılır.
-  `Foo (NSEmptyFlag)`: Bu türetilmiş sınıfları tarafından çift başlatma önlemek için kullanılır.

Tanımladığınız oluşturucuları için bunlar arabirim tanımı içinde aşağıdaki imzası kullanarak bildirilmesi gerekir: döndürmesi gerekir bir `IntPtr` değeri ve yöntemin adını oluşturucusu olması gerekir. Örneğin bağlamak `initWithFrame:` oluşturucusu, budur kullanmanız:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Bağlama protokolleri

API tasarım belge bölümünde açıklandığı gibi [modelleri ve protokolleri ele](~/ios/internals/api-design/index.md#Models), Xamarin.iOS Objective-C protokolleri ile işaretlenen sınıfları eşlemeleri [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) özniteliği. Bu genellikle Objective-C temsilci sınıfları uygulanırken kullanılır.

Büyük bir normal bağlı ve bir temsilci sınıf arasında temsilci sınıfı bir veya daha fazla isteğe bağlı yöntemler olabilir farktır.

Örneğin göz önünde bulundurun `UIKit` sınıfı `UIAccelerometerDelegate`, bu Xamarin.iOS içinde nasıl bağlı olur:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Bu isteğe bağlı bir yöntem tanımında olduğundan `UIAccelerometerDelegate` yapmak için başka bir şey yok. Ancak protokolünü gerekli bir yöntemi ise eklemeniz gerekir [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) özniteliği yöntemi. Bu gövde yöntemi için gerçekten sağlamak için kullanıcı uygulamasının zorlar.

Genel olarak, protokolleri iletileri yanıtlayın sınıfları kullanılır. Bu genellikle Objective-C Protokolü yöntemlere yanıtlaması bir nesnenin örneğine "temsilci" özelliğine atayarak yapılır.

Xamarin.iOS kural her iki Objective-C gevşek bağlanmış stili herhangi bir yerde desteklemektir örneğini bir `NSObject` temsilci ve ayrıca sunmaya bir kesin türü belirtilmiş sürümünü atanabilir. Bu nedenle, genellikle her ikisi de sunuyoruz bir `Delegate` kesin türü belirtilmiş özelliği ve `WeakDelegate` geniş türü. Geniş yazılmış sürümüyle biz genellikle bağlamak [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), ve kullanırız [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) kesin türü belirtilmiş sürümünü sağlamak için öznitelik.

Bu size nasıl bağlı gösterir `UIAccelerometer` sınıfı:

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

<a name="iOS7ProtocolSupport" />

**MonoTouch 7.0 yenilikler**

İşlev bağlama yeni ve geliştirilmiş bir iletişim kuralı MonoTouch 7.0 ile başlayan bir araya getirilmiştir.  Bu yeni destek belirli bir sınıfın bir veya daha fazla protokollerin benimsenmesi için Objective-C deyimleri kullanmak daha basit hale getirir.

Her protokol tanımı için `MyProtocol` Objective-C, var. Şimdi bir `IMyProtocol` Protokolü gerekli tüm yöntemleri listeler arabirimi yanı sıra tüm isteğe bağlı yöntemler sağlayan bir uzantı sınıfı.  Yukarıdaki, birleştirilmiş önceki soyut modeli sınıfları ayrı sınıfları kullanmak zorunda kalmadan Protokolü yöntemleri uygulamak geliştiricilerin Düzenleyicisi sağlar Xamarin Studio'da yeni desteğiyle.

İçeren herhangi bir tanımının [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) özniteliği gerçekte çok protokolleri kullanma şeklini geliştirmek üç destekleyen sınıfları oluşturun:

```csharp
    // Full method implementation, contains all methods
    class MyProtocol : IMyProtocol {
        public void Say (string msg);
        public void Listen (string msg);
    }

    // Interface that contains only the required methods
    interface IMyProtocol: INativeObject, IDisposable {
        [Export ("say:")]
        void Say (string msg);
    }

    // Extension methods
    static class IMyProtocol_Extensions {
        public static void Optional (this IMyProtocol this, string msg);
        }
    }
```

**Sınıf uygulamasını** tek tek yöntemlerini geçersiz kılın ve tam tür güvenliği alma tam bir Özet sınıf sağlar.  C birden çok devralma desteklemediğinden # nedeniyle, ancak burada olabilirsiniz senaryoları farklı bir temel sınıf olması gerekiyor, ancak yine de yerdir bir arabirim, istiyor

Oluşturulan **arabirim tanımı** devreye girer.  Protokol gereken tüm yöntemleri sahip bir arabirimdir.  Bu, yalnızca arabirimi uygulamak için protokol uygulamak isterseniz geliştiriciler sağlar.  Çalışma zamanı Protokolü'nu benimseme olarak türü otomatik olarak kaydeder.

Arabirimi yalnızca gerekli yöntemlerini listeler ve isteğe bağlı yöntemler kullanıma dikkat edin.  Bu protokol benimsemeye sınıfları için gerekli yöntemleri denetimi tam tür alır, ancak zayıf yazmaya çözümlemelere gerekecek anlamına gelir (el ile kullanarak [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) öznitelikleri ve imza eşleştirme) için isteğe bağlı protokol yöntemleri.

Protokollerini kullanan bir API kullanmak üzere kullanışlı hale getirmek Bağlama aracı ayrıca tüm isteğe bağlı yöntemleri gösteren bir uzantıları yöntemi sınıf oluşturur.  Bu bir API kullanıyor sürece, tüm yöntemleri sahip olarak protokolleri ele almanız mümkün olacağı anlamına gelir.

API'nizi protokol tanımlarını kullanmak istiyorsanız, API tanımı'nda iskelet boş arabirimlerden yazma gerekecektir.  İletişimKuralım bir API kullanmak istiyorsanız, bunu yapmanız gerekir:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Yukarıdaki çünkü gereklidir bağlama süresi, `IMyProtocol` mevcut değil, diğer bir deyişle neden boş bir arabirim sağlamanız gerekir.

#### <a name="adopting-protocol-generated-interfaces"></a>Protokol oluşturulan arabirimleri uygulamasını kullanma

Her şöyle protokoller için oluşturulan arabirimleri birini uygulayın:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Buna eşdeğer olacak şekilde arabirim yöntemleri için uygulama otomatik olarak en uygun ad ile dışarı:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Arabirim örtük veya açık olarak uygulanırsa önemli değildir.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Bağlama sınıf uzantıları

Objective-C yöntemleriyle yeni C# ' ın genişletme yöntemleri Ruhu benzer sınıflarını genişletmek mümkündür. Aşağıdaki yöntemlerden birini mevcut olduğunda kullanabileceğiniz [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Objective-C ileti alıcısı olarak yöntemi bayrak özniteliği.

Örneğin, içinde Xamarin.iOS biz üzerinde tanımlanan genişletme yöntemleri bağlı `NSString` zaman `UIKit` yöntemleri olarak içe `NSStringDrawingExtensions`, şöyle:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Objective-C bağımsız değişken listeleri bağlama

Objective-C variadic bağımsız değişkenini destekler. Örneğin:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

C# bu yöntemi çağırmak için şöyle bir imza oluşturmak isteyeceksiniz:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Bu, kullanıcılardan yukarıdaki API gizleme ancak kitaplığa gösterme yöntemi, dahili olarak bildirir. Ardından bir yöntem şöyle yazabilirsiniz:

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

<a name="Binding_Fields" />

### <a name="binding-fields"></a>Alanların bağlama

Bazen bir kitaplıkta bildirilen ortak alanlara erişmek isteyeceksiniz.

Genellikle bu alanlar başvurulmalıdır dizeleri veya tamsayı değerlerini içerir. Bunlar genellikle sözlüklerindeki anahtarlarını ve belirli bir bildirim temsil eden dize olarak kullanılır.

Bir alana bağlamak için bir özellik arabirim tanımı dosyanıza ekleyin ve özelliğiyle dekorasyon [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) özniteliği. Bu öznitelik bir parametre alan: arama simgesine C adı. Örneğin:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Türünden türemez statik bir sınıf çeşitli alanları sarmalamak istiyorsanız `NSObject`, kullanabileceğiniz [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) şöyle sınıfı özniteliği:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Yukarıdaki oluşturacak bir `LonelyClass` hangi öğesinden türetilmiyor `NSObject` ve bir bağlama içerir `NSSomeEventNotification` 
 `NSString` olarak sunulan bir `NSString`.

[ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Özniteliği aşağıdaki veri türlerini uygulanabilir:

-  `NSString` Başvurular (yalnızca salt okunur özellikler)
-  `NSArray` Başvurular (yalnızca salt okunur özellikler)
-  32-bit ints (`System.Int32`)
-  64-bit ints (`System.Int64`)
-  32-bit float (`System.Single`)
-  64-bit float (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

Yerel alan adına ek olarak, alan, kitaplık adı geçirerek bulunduğu kitaplık adını belirtebilirsiniz:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Statik olarak bağlanıyorsanız, olmadığından bağlamak için kitaplık kullanmanıza gerek `__Internal` adı:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Bağlama numaralandırmaları

Ekleyebileceğiniz `enum` bağlamasında doğrudan dosyalara kolaylaştırır (yani bağlamaları hem son projenin derlenmesi gerekiyor) farklı bir kaynak dosyasındaki kullanmadan API tanımlarını içinde - kullanmak.

Örnek:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Değiştirmek için kendi numaralandırmaları oluşturmak mümkündür `NSString` sabitleri. Oluşturucunun bu durumda olur **otomatik olarak** numaralandırmaları değerleri ve NSString sabitleri dönüştürmeye yöntemleri oluşturun.

Örnek:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

Yukarıdaki örnekte tasarlamanız karar `void Perform (NSString mode);` ile bir [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği. Bu işlem **Gizle** bağlama tüketicileriniz sabiti tabanlı API'SİNDEN.

Ancak bu tür daha Hoş görünmesi API alternatif kullandıkça sınıflara sınırlandırır bir [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) özniteliği. Bu oluşturulan yöntemler `virtual`, yani, hangi olabilir veya bunları - geçersiz kılmak için iyi bir seçimdir olması olmaz.

Özgün işaretlemek için alternatiftir `NSString`-bağlı olarak, tanımı olarak `[Protected]`. Bu, gerekli olduğunda çalışması sınıflara izin verir ve wrap'ed sürüm hala çalışma ve kılınmadı yöntemini çağırın.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Bağlama `NSValue`, `NSNumber`, ve `NSString` daha iyi türü

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Özniteliğe izin bağlama `NSNumber`, `NSValue` ve `NSString`(numaralandırmaları) daha doğru C# türleri içine. Öznitelik daha iyi ve daha doğru oluşturmak için kullanılan yerel API üzerinden .NET API.

Yöntemlerde (dönüş değeri), parametreleri ve özellikleri ile işaretleme [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute). Tek kısıtlama, üye olan **bulunmamalıdır** içinde olması bir [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) veya [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) arabirimi.

Örneğin:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Çıkış:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Dahili olarak gerçekleştiririz `bool?`  <->  `NSNumber` ve `CGRect`  <->  `NSValue` dönüşümler.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Ayrıca, dizilerin destekler `NSNumber` `NSValue` ve `NSString`(numaralandırmaları).

Örneğin:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

Çıkış:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` olan bir `NSString` enum, yedeklenen biz sağa getirir `NSString` değer ve tür dönüştürmeleri tanıtıcı.

Lütfen bakın [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) desteklenen dönüştürme türlerini görmek için belgeleri.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Bağlama bildirimleri

Bildirimler için gönderilen iletileri olan `NSNotificationCenter.DefaultCenter` ve başka bir uygulamanın bir bölümünden yayın iletilerinin için bir mekanizma olarak kullanılır. Geliştiriciler genellikle kullanarak Bildirimlere abone [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)'s [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) yöntemi. Bir uygulamayı bildirim Merkezi'ne bir ileti gönderdiğinde, genellikle depolanan bir yükü içerdiği [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) sözlük. Bu sözlük zayıf yazılmış ve kullanıcıların hangi anahtarları sözlük ve sözlük içinde depolanan değerlerin türleri kullanılabilir belgelerinde okumak genellikle gerekir. Bunun dışında bilgi alma hataya, aynıdır. Bazen anahtarları varlığını bir boolean olarak kullanılır.

Xamarin.iOS bağlama Oluşturucu bildirimleri bağlamak geliştiricilere yönelik destek sağlar. Bunu yapmak için ayarladığınız [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) de olan bir özellikte özniteliği ile etiketlenmiş bir [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) özelliği (olabilir genel veya özel).

Bu öznitelik yükü yok taşımak bildirimler için bağımsız değişkenler olmadan kullanılabilir veya belirleyebileceğiniz bir `System.Type` başvuran başka bir API tanımı arabiriminde genellikle "EventArgs" ile biten ada sahip. Oluşturucu arabirimi o alt sınıfların bir sınıfına dönüşecektir `EventArgs` ve orada listelenen tüm özellikler içerir. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Özniteliği Objective-C sözlüğün değeri getirmek aramak için kullanılan anahtarın adını listeye EventArgs sınıfında kullanılmalıdır.

Örneğin:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Yukarıdaki kod iç içe bir sınıf oluşturur `MyClass.Notifications` aşağıdaki yöntemlerle:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Kullanıcıların kodunuzu daha sonra kolayca abone olabileceği için gönderilen bildirimleri [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) aşağıdakine benzer bir kod kullanarak:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Döndürülen değerle `ObserveDidStart` kolayca şöyle bildirimleri almayı durdurmak için kullanılabilir:

```csharp
token.Dispose ();
```

Ya da çağırabilirsiniz [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) ve belirtecin geçip. Bildiriminizin parametreleri içeriyorsa, bir yardımcı belirtmelisiniz `EventArgs` arabirimi şöyle:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Yukarıdaki oluşturacak bir `MyScreenChangedEventArgs` ile sınıf `ScreenX` ve `ScreenY` verileri getirir özellikleri [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) "ScreenXKey" ve "ScreenYKey" tuşlarıyla sözlük Sırasıyla ve uygun dönüşümleri uygulayın. `[ProbePresence]` Özniteliği için oluşturucuyu anahtar ayarlanırsa araştırma için kullanılan `UserInfo`, değerini ayıklayın çalışılırken yerine. Bu anahtar varlığını değeri (genellikle Boole değerleri) olduğu durumlarda kullanılır.

Bu, aşağıdakine benzer bir kod yazmanıza olanak sağlar:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Bağlama kategorileri

Kategoriler yöntemleri ve özellikleri bir sınıfta kullanılabilir kümesini genişletmek için kullanılan bir Objective-C mekanizması bulunur.   Uygulamada, bunlar ya da bir temel sınıf işlevselliğini genişletmek için kullanılır (örneğin `NSObject`) ne zaman belirli bir framework bağlı olarak (örneğin `UIKit`), kullanılabilir, ancak yeni framework bağlıysa yalnızca kendi yöntemlerini yapma.   Bazı durumlarda, bunlar bir sınıf özelliklerini düzenlemek için işlevsellik tarafından kullanılır.   Bunlar için genişletme yöntemleri C# içinde Ruhu benzerdir. Bir kategori hedefi-C: nasıl gibidir budur

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Yukarıdaki örnekte, bulunan bir kitaplık örnekleri genişletir `UIView` yöntemiyle `makeBackgroundRed`.

Bu bağlamak için kullanabileceğiniz [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) arabirim tanımı özniteliği.  Kullanırken [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) özniteliği, anlamını [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) özniteliği değiştirir genişletmek için türü genişletmek için temel sınıf belirtmek için kullanılır.

Aşağıdaki gösterildiği nasıl `UIView` uzantıları bağlı ve C# genişletme yöntemleri açık:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Yukarıdaki oluşturacak bir `MyUIViewExtension` içeren bir sınıf `MakeBackgroundRed` genişletme yöntemi.  "MakeBackgroundRed" şimdi çağırabilirsiniz birinde yani `UIView` almak Objective-c üzerinde aynı işlevselliği vermiş bir alt Bazı durumlarda, kategoriler, bir sistem sınıfı genişletmek değil ancak tamamen decoration amacıyla işlevselliği düzenlemek için kullanılır.  Böyle:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Kullanabilirsiniz ancak [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) özniteliği de bildirimleri için bu deseni stili de yalnızca bunları tüm sınıf tanımına eklediğiniz.  Bunların her ikisi de aynı elde:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

Kategoriler birleştirmek için bu durumda yalnızca daha kısa olur:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks" />

### <a name="binding-blocks"></a>Blokları bağlama

Objective-c C# anonim yöntemler işlevsel denk getirmek için Apple tarafından sunulan yeni bir yapı taşlarıdır Örneğin, `NSSet` sınıfı şimdi bu yöntem sunar:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Yukarıdaki açıklama adlı bir yöntem bildirir `enumerateObjectsUsingBlock:` adlı tek bir bağımsız değişken alan `block`. Geçerli ortam ("Bu" işaretçisi, yerel değişkenleri ve parametreleri erişimi) yakalama desteğe sahiptir, bu bloğu bir C# anonim yöntemine benzer. Yukarıdaki yönteminde `NSSet` blok iki parametre ile çağırır bir `NSObject` ( `id obj` bölümü) ve bir işaretçi bir Boole değeri ( `BOOL *stop`) bölümü.

Bu tür bir API btouch ile bağlamak için C# temsilci ve böyle bir API giriş noktasından başvuru olarak blok türü imza ilk bildirmeniz gerekir:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

Ve şimdi kodunuzu işlevinizi C# ' dan çağırabilirsiniz:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Lambda'lar, isterseniz de kullanabilirsiniz:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Zaman uyumsuz yöntemleri

Bağlama Oluşturucu belirli sınıfının yöntemleri zaman uyumsuz kolay yöntemlerin içine kapatabilirsiniz (bir görevi veya görev döndüren yöntemler&lt;T&gt;).

Kullanabileceğiniz [ `[Async]` ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) , void dönün ve olan son bağımsız değişken bir geri çağırma olduğu yöntemleri özniteliği.  Bu yönteme uyguladığınızda, bağlama oluşturucunun soneki bu yöntem bir sürümünü oluşturur `Async`.  Geri çağırma parametre almayan, dönüş değeri olacaktır bir `Task`, geri çağırma parametresi alırsa, sonuç olacak bir `Task<T>`.  Geri çağırma birden çok parametre sürerse ayarlamalısınız `ResultType` veya `ResultTypeName` istenen tüm özellikleri tutacak oluşturulan tür adını belirtmek için.

Örnek:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Yukarıdaki kod iki önce LoadFile işlevini yöntemi oluşturur yanı:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Zayıf NSDictionary parametreleri için güçlü türleri görünmesini

Objective-C API'sindeki birçok yerde parametreleri olarak zayıf yazılmış geçirilir `NSDictionary` apı'leridir özel anahtarları ve değerleri, ancak bu hataya (geçersiz anahtarları geçirmek ve uyarı alın; geçersiz değerleri geçirmek ve hiçbir uyarıları alma) ve can sıkıcı birden çok dönüşleri olası anahtar adları ve değerleri aramak için belgeleri için gereksinim duydukları olarak kullanmak için.

Kesin türü belirtilmiş sürüm API ve arka planda çeşitli temel alınan anahtarlar ve değerler eşlemeleri sağlar kesin türü belirtilmiş bir sürümünü sağlamak için çözümüdür.

Örneğin, bunu Objective-C API onayladığınızda bir `NSDictionary` ve anahtar alan olarak belgelenen `XyzVolumeKey` hangi alır bir `NSNumber` 1.0 için bir birim değeri 0,0 ile ve `XyzCaptionKey` bir dize alır, kullanıcılarınızı iyi bir API istersiniz şuna benzer:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Objective-C kural vardır; bu nedenle burada değeri ayarlanmamış senaryoları değerine sahip olacak şekilde bu sözlükler gerektirmez gibi özelliği boş değer atanabilir float tanımlanır.

Bunu yapmak için birkaç şey yapmanız gerekir:

* Türü kesin belirlenmiş sınıf, o alt sınıfların oluşturma [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) ve çeşitli alıcılar ve ayarlayıcılar için her bir özellik sağlar.
* Alma yöntemleri için aşırı bildirme `NSDictionary` yeni kesin türü belirtilmiş sürümünü almak için.

Türü kesin belirlenmiş sınıf ya da el ile oluşturabilir veya sizin yerinize yapmaları için oluşturucunun kullanabilirsiniz.  Biz öncelikle neler olup bittiğini anlamak için bu el ile nasıl yapılacağını ve ardından otomatik yaklaşımı keşfedin.

Bu destek dosyası oluşturmanız gerekir, sizin sözleşmesine API geçmez.  Bu XyzOptions sınıfı oluşturmak yazma gerekirdi.

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

Ardından, alt düzey API üstünde üst düzey API'si ortaya çıkarır bir sarmalayıcı yöntemi sağlamanız gerekir.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

API'nizi üzerine gerekmez varsa, güvenli bir şekilde NSDictionary tabanlı API kullanarak gizleyebilirsiniz [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği.

Gördüğünüz gibi kullanırız [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) yeni bir API giriş noktası yüzey özniteliğini ve biz kesin türü belirtilmiş bizim kullanarak yüzey `XyzOptions` sınıfı.  Sarmalayıcı yöntemi de null geçirilmesini sağlar.

Şimdi, biz değil Bahsediyor tek şey yerdir `XyzOptionsKeys` değerleri geldiği.  Bir API gibi statik sınıfında ortaya çıkarır anahtarlar genellikle grup `XyzOptionsKeys`, şöyle:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Bize bu kesin türü belirtilmiş sözlükler oluşturmak için otomatik desteği bakın.  Bu ortak Eskinin önler ve dış dosyası kullanmak yerine doğrudan, API sözleşmesindeki, sözlük tanımlayabilirsiniz.

Kesin türü belirtilmiş bir sözlük oluşturmak için bir arabirim API'nizi, getirir ve onunla tasarlamanız [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) özniteliği.  Bu oluşturucu, öğesinden türetilen için arabirimle aynı ada sahip bir sınıf oluşturmanız gerekir söyler `DictionaryContainer` için güçlü yazılan erişimciler sağlar.

[ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Özniteliği, sözlük anahtarları içeren statik sınıf adı olan bir parametre alır.  Ardından arabiriminin her bir özellik kesin türü belirtilmiş bir erişimci olur.  Varsayılan olarak, kodu özelliğinin adı statik sınıf "Anahtarı" soneki ile erişimci oluşturmak için kullanır.

Bu, kesin türü belirtilmiş erişimcisi oluşturma artık dış bir dosya ya da alıcılar ve ayarlayıcılar her bir özellik için el ile oluşturmak zorunda kalmadan veya anahtarları el ile arama gerek kalmadan gerektirdiği anlamına gelir. kendiniz.

Bu, tüm bağlamayı aşağıdaki gibi görünür.

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Başvuru gerekebileceği, `XyzOption` üyeleri farklı bir alan (yani değil özelliğinin adı soneki `Key`), özelliğiyle tasarlamanız bir [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) adıyla aldığınız özniteliği kullanmak istediğiniz.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Türü eşlemeleri

Bu bölümde ele alınmaktadır nasıl Objective-C türleri C# türlerine eşlenir.

<a name="Simple_Types" />

### <a name="simple-types"></a>Basit türler

Aşağıdaki tabloda Objective-C ve CocoaTouch world türlerinden Xamarin.iOS dünyaya nasıl eşlemelisiniz gösterilmektedir:

|Objective-C türü adı|Xamarin.iOS birleşik API türü|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([NSString bağlama hakkında daha fazla](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (Ayrıca bkz: [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|CoreFoundation türleri (`CF*`)|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Foundation türleri (`NS*`)|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays" />

### <a name="arrays"></a>Diziler

Xamarin.iOS çalışma zamanı otomatik olarak C# dizilere dönüştürme mvc'deki `NSArrays` ve dönüştürme geri, dolayısıyla örneğin hayali Objective-C yöntemi, bunu döndüren bir `NSArray` , `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

Şuna bağlı:

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

Bu tahmin ya da gerçek dizisi içinde yer alan nesne türlerini bulmak için belgeleri aramak için kullanıcı zorlamadan uygun kod tamamlama gerçek türüyle sağlamak IDE sağlayacak bir kesin türü belirtilmiş C# dizi kullanacak şekilde olur.

Burada değil izleyebilirsiniz dizinin içindeki gerçek en çok türetilen tür aşağı durumlarda, kullandığınız `NSObject []` dönüş değeri olarak.

<a name="Selectors" />

### <a name="selectors"></a>Seçici

Özel tür olarak Objective-C API seçiciler görünmez `SEL`. Bir seçici bağlanırken türüne eşleyen `ObjCRuntime.Selector`.  Genellikle seçiciler hedef nesnedeki çağırmak için bir nesne, hedef nesne hem bir seçici bir API sunulur. Bunların her ikisi de temelde sağlama karşılık gelen C# temsilciye: hem çağrılacak yöntem, hem de yöntemi çağırmak için nesne yalıtan bir şey.

Bu bağlama gibi görünüyor.

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

Ve bu nasıl yöntemi genellikle bir uygulamada kullanılacak:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

C# geliştiricileri için bağlama daha Hoş görünmesi yapmak için genellikle götüren bir yöntem sağlayacaktır bir `NSAction` C# Temsilciler ve Lambda'lar yerine kullanılacak izin veren parametresi `Target+Selector`. Bu genellikle Gizle yapmak için `SetTarget` ile işaretleme tarafından yöntemi bir [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği ve daha sonra bu gibi yeni bir yardımcı yöntemi kullanıma:

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

Artık kullanıcı kodunuzun şu şekilde yazılabilir.

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

<a name="Strings" />

### <a name="strings"></a>Dizeler

Ne zaman bağladığınız götüren bir yöntem bir `NSString`, C# dize türü, ile hem türleri ve parametreleri döndürme olup olmadığını değiştirebilirsiniz.

Yalnızca durum kullanmak isteyebileceğiniz bir `NSString` dize belirteç olarak doğrudan kullanılmasıdır. Dizeleri hakkında daha fazla bilgi ve `NSString`okuyun [NSString API tasarım](~/ios/internals/api-design/nsstring.md) belge.

Bazı nadir durumlarda, bir API C benzeri dize doğurabilir (`char *`) yerine bir Objective-C dize (`NSString *`). Bu gibi durumlarda parametresiyle açıklayabilirsiniz [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) özniteliği.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>out / ref parametreleri

Bazı API'leri kendi parametrelerinde dönüş değerleri veya başvuruya göre parametreler Geçiren.

Genellikle imza şöyle görünür:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

İlk örnek hata kodları, bir işaretçi döndürmek için ortak bir Objective-C deyim gösterir bir `NSError` işaretçi geçirilir ve dönüş değeri ayarlayın.   İkinci yöntem, Objective-C yöntemi nasıl ve bir nesne ele içeriğini değiştirme gösterir.   Bu bir geçişi saf çıkış değeri yerine başvuru olacaktır.

Bağlama şöyle olabilir:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Bellek yönetimi öznitelikleri

Kullandığınızda [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği ve çağrılan yöntemi tarafından korunacak veri geçirme, ikinci bir parametresi örneğin geçirerek için bu bağımsız değişken semantiğini belirtebilirsiniz:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Yukarıdaki "Koru" semantiklerine sahip olarak değer bayrak. Kullanılabilir semantiğini şunlardır:

-  Ata
-  Kopyala
-  Tut

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Stil Kılavuzu

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>[İç] kullanma

Kullanabileceğiniz [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) genel API'si yönteminden gizlemek için öznitelik. Burada gösterilen API çok düşük düzey ve yüksek düzey bir uygulama bu yöntemine dayalı olarak ayrı bir dosyada sağlamak istediğiniz durumlarda bu yapmak isteyebilirsiniz.

Bu bağlama Oluşturucu sınırlamalarını içine çalıştırdığınızda, bazı Gelişmiş senaryolar bağlı olmayan türleri örneğin doğurabilir ve kendi şekilde bağlamak istediğiniz ve bu türlerde kendiniz kendi şekilde kaydırmak istediğiniz de kullanabilirsiniz.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Olay işleyicileri ve geri aramalar

Objective-C sınıflar genellikle bildirimleri yayın veya temsilci sınıfı (Objective-C temsilci) üzerinde bir ileti göndererek bilgi isteyin.

Bu model, tam olarak desteklenir ve Xamarin.iOS tarafından ortaya bazen olabilir sıkıcı. C# olay düzeni ve bu gibi durumlarda kullanılabilir sınıfı bir geri çağırma yöntemi sistemde Xamarin.iOS gösterir. Bu kodu çalıştırmak için şöyle sağlar:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Bağlama Oluşturucu C# düzeni Objective-C düzeni eşlemek için gerekli yazarak miktarını azaltır yeteneğine sahiptir.

Xamarin.iOS 1.4 ile başlatmanızı Ayrıca belirli bir Objective-C temsilciler bağlantılarında üretmek ve temsilci C# olayları ve ana bilgisayar türü özellikleri olarak kullanıma sunmak için oluşturucunun istemek üzere kullanılabilecektir.

İki sınıf bu işleminde, olacak konak sınıfı, şu anda olayları gösterir ve bu uygulamasına gönderir bilgisayardır `Delegate` veya `WeakDelegate` ve gerçek temsilci sınıfı.

Aşağıdaki Kurulum dikkate:

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

Sınıf sarmalamak için yapmanız gerekir:

-  Ana sınıfınız eklemek için [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   kendi temsilci ve size sunulan C# adı hareket türü bildirimi. Yukarıdaki örnekte, bizim olanlardır `typeof (MyClassDelegate)` ve `WeakDelegate` sırasıyla.
-  Temsilci sınıfınızda ikiden fazla parametrelere sahip her yöntemi otomatik olarak oluşturulan EventArgs sınıfı için kullanmak istediğiniz türünü belirtmeniz gerekir.

Bağlama Oluşturucu yalnızca tek bir olay hedef kaydırma sınırlı değildir, bu kurulum desteklemek için diziler vermeniz gerekir böylece birden fazla iletileri yaymak üzere bazı Objective-C sınıfları devretmenizi, mümkündür. Çoğu kurulumları gerekmez, ancak oluşturucunun bu gibi durumlarda desteklemeye hazırdır.

Ortaya çıkan kodu olacaktır:

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

`EventArgs` Adını belirtmek için kullanılan `EventArgs` oluşturulacak sınıfı. İmza her kullanmanız gerekir (Bu örnekte, `EventArgs` içerecek bir `With` türü nint özelliği).

Yukarıdaki tanımlarla oluşturucunun oluşturulan MyClass aşağıdaki olay üretir:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Bu nedenle, aşağıdakine benzer bir kod artık kullanabilirsiniz:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Geri aramalar yalnızca olay çağrılarını gibi, birden çok potansiyel aboneye sahip olmak yerine farktır (içine Örneğin, birden çok yöntem bağlama bir `Clicked` olay veya `DownloadFinished` olay) geri aramalar yalnızca tek bir abone olabilir.

İşlem aynıdır, tek fark, adını gösterme yerine `EventArgs` oluşturulur, EventArgs sınıfı gerçekten elde edilen C# temsilci adı için kullanılır.

Temsilci sınıfında yöntemi bir değer döndürürse, bağlama Oluşturucu bu olaya yerine ana sınıfı temsilci yönteminde içine eşler. Bu durumlarda kullanıcının takma değil temsilciye yöntem tarafından döndürülen varsayılan değer sağlamanız gerekir. Kullanarak bunu [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) veya [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) öznitelikleri.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) dönüş değeri stillerinizin olur ancak [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) hangi giriş bağımsız değişkenine döndürülecek belirtmek için kullanılır.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Numaralandırmalar ve taban türleri

Numaralandırmalar veya btouch arabirim tanımı sistem tarafından doğrudan desteklenmeyen temel türleri de başvurabilirsiniz. Bunu yapmak için numaralandırmalar ve çekirdek türleri ayrı bir dosyaya koymak ve bu btouch için sağladığınız ek dosyalardan birini bir parçası olarak içerir.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Bağımlılıkları bağlama

Uygulamanızın parçası olmayan API'leri bağlanıyorsanız, yürütülebilir dosyanın Bu kitaplıklar karşı bağlı olduğundan emin olmanız gerekir.

Xamarin.iOS nasıl Kitaplıklarınızı bağlantı bildirmeniz gerekir, bu da çağırmak için derleme yapılandırmasını değiştirmeyi tarafından yapılabilir `mtouch` bazı ek komutuyla yapı kullanarak yeni kitaplıkları ile bağlanma belirtin bağımsız değişkenler "-gcc_flags" seçeneği, Bu gibi program için gerekli olan tüm ek kitaplıkları içeren tırnak içine alınmış bir dize ve ardından:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Yukarıdaki örnekte bağlayacaksınız `libMyLibrary.a`, `libSystemLibrary.dylib` ve `CFNetwork` framework kitaplığa son çalıştırılabilir.

Ya da derleme düzeyi yararlanabilir [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), bu sözleşme dosyalarınızda katıştırma (gibi `AssemblyInfo.cs`).
Kullandığınızda [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), yerel kitaplığınızın yaptığınız, bağlama, bu yerel kitaplığı ile uygulamanızı katıştırır gibi zaman kullanılabilir olması gerekir. Örneğin:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Merak ediyor, neden ihtiyacınız `-force_load` komut ve neden olduğu - ObjC bayrak kodda derler olsa da, Kategoriler (Bağlayıcı/derleyici ölü kod eleme kaldırır) desteklemek için gereken meta verilerini korumaz, Çalışma zamanında Xamarin.iOS için gerekir.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Yardımlı başvuruları

Eylem sayfaları ve uyarı kutuları gibi bazı geçici nesneler geliştiriciler için izlemek için sıkıcı ve bağlama Oluşturucu biraz burada yardımcı olabilir.

Bir ileti gösterdi ve ardından oluşturulan bir sınıf sahipse, örneğin bir `Done` olay, bu işleme geleneksel şekilde olacaktır:

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

Yukarıdaki senaryoda Geliştirici nesnesine başvuru kendisi ve her iki sızıntısı tutun veya etkin olarak kendi kutusunu başvurusunu temizlemek gerekir.  Bağlama kod oluşturucunun desteklerken tutma başvurusunu sizin için izleyip temizleyin, özel bir yöntem çağrıldığında, yukarıdaki kod sonra olur:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Nasıl, artık sahip yerel bir değişken çalışır ve nesne sonlandıktan ayarlandığında başvuru silmek gerekli değildir, değişkeni bir örneğinde tutmak gerekli olduğuna dikkat edin.

Bu yararlanmak için sınıfınız kümesinde olayları özelliğine sahip olmalı [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) bildirimi ve ayrıca `KeepUntilRef` değişkenini nesne gibi kendi iş tamamlandıktan sonra çağrılan yöntemin adını ayarla Bu:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Protokolleri devralma

Xamarin.iOS v3.2 itibariyle ile işaretlenen protokolleri içinden devralma olan destekliyoruz [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) özelliği. Bu belirli API düzenleri gibi olarak içinde yararlıdır `MapKit` nerede `MKOverlay` protokolü, devraldığı `MKAnnotation` protokolünü ve devralınmalıdır sınıfların sayısı tarafından benimsenen `NSObject`.

Geçmişte Protokolü her uygulama için kopyalama gerekli, ancak bu durumda artık biz olabilir `MKShape` sınıf devralma `MKOverlay` protokolü ve oluşturacağını gereken tüm yöntemleri otomatik olarak.

## <a name="related-links"></a>İlgili bağlantılar

- [Bağlama örneği](https://developer.xamarin.com/samples/BindingSample/)
