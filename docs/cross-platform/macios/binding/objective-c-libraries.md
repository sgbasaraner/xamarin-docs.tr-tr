---
title: Objective-C kitaplıklarını bağlama
description: Bu belge, Objective-C kodunun nasıl olayları, yöntemleri, özel denetimler ve diğer bağlanacağını açıklayan C# bağlamaları oluşturulacağını üst düzey bir genel bakış sağlar.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 4c414e0e863f44045473a248576a3612b1719559
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854837"
---
# <a name="binding-objective-c-libraries"></a>Objective-C kitaplıklarını bağlama

Xamarin.iOS ve Xamarin.Mac ile çalışırken, bir üçüncü taraf Objective-C kitaplığını kullanmak istediğiniz durumlarda karşılaşabilirsiniz. Bu durumlarda, Xamarin bağlama projeleri yerel Objective-C kitaplıklarını bir C# bağlama oluşturmak için kullanabilirsiniz. Proje iOS ve Mac API'leri C# ' tan getirmek için kullandığımız aynı araçları kullanır.

Bu belge, Objective C API'lerini bağlanacağını açıklar yalnızca C API'lerini bağlanıyorsanız, standart .NET mekanizmasını bunun için kullanmanız gereken [P/Invoke framework](http://www.mono-project.com/docs/advanced/pinvoke/).
Bir C Kitaplığı kitaplıklarla statik bağlantılar oluşturabilir hakkında ayrıntılar bulunur [bağlama yerel kitaplıkları](~/ios/platform/native-interop.md) sayfası.

Bizim Yardımcısı bkz [bağlama türleri Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md).
Ayrıca, başlık altında neler olduğu hakkında daha fazla bilgi edinmek istiyorsanız, kontrol bizim [Binding Overview](~/cross-platform/macios/binding/overview.md) sayfası.

Bağlamaları, iOS ve Mac kitaplıkları için oluşturulabilir.
Bu sayfa bağlıyorsanız, ancak Mac bağlamaları çok benzer bir iOS iş açıklar.

**İOS için örnek kod**

Kullanabileceğiniz [iOS bağlama örnek](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) bağlamalarla denemek için proje.

<a name="Getting_Started" />

## <a name="getting-started"></a>Başlarken

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bir bağlamayı oluşturmak için en kolay yolu, bir Xamarin.iOS bağlaması projesi oluşturmaktır.
Visual Studio üzerinden Mac için proje türünü seçerek bunu yapabilirsiniz **iOS > Kitaplık > bağlamaları Kitaplığı**:

[![](objective-c-libraries-images/00-sml.png "Proje türü, iOS kitaplığına bağlama kitaplığı seçerek Mac için bunu Visual Studio'dan")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bir bağlamayı oluşturmak için en kolay yolu, bir Xamarin.iOS bağlaması projesi oluşturmaktır.
Windows üzerinde Visual Studio'dan proje türünü seçerek bunu yapabilirsiniz **Visual C# > iOS > bağlamaları kitaplığı (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS bağlamaları kitaplığı iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Not: Bağlama projeleri için **Xamarin.Mac** Mac için Visual Studio'da yalnızca desteklenir

-----

Oluşturulan projeyi düzenleyebilirsiniz küçük bir şablon içeren, iki dosya içeriyor: `ApiDefinition.cs` ve `StructsAndEnums.cs`.

`ApiDefinition.cs` API sözleşmesi, burada tanımlarsınız bu nasıl temel Objective-C API C# diline gsyih açıklayan dosyasıdır. Söz dizimi ve bu dosyanın içeriği bu belgenin tartışma ana konu ve içeriğini C# arabirimleri ve C# temsilcisi bildirimleri ile sınırlıdır. `StructsAndEnums.cs` Girebileceğiniz gereken herhangi bir tanımı arabirimlerde ve temsilcilerde tarafından dosya dosyasıdır. Bu numaralandırma değerlerinden ve kodunuzu kullanıyor olabileceğiniz yapıları içerir.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Bir API bağlama

Kapsamlı bir bağlama yapmak için Objective-C API tanımı anlamak ve .NET Framework tasarım yönergeleri ile kendinizi alıştırın isteyeceksiniz.

Kitaplığınızı bağlamak için genellikle bir API tanım dosyası ile başlar. Bir API tanımını içeren sürücü Yardımı öznitelikleri bir dizi ek açıklama eklenen C# arabirimleri bağlama yalnızca bir C# kaynak dosyası dosyasıdır.  Bu dosya, C# ve Objective-C arasındaki nedir tanımlar ' dir.

Örneğin, bu önemsiz bir API dosya bir kitaplığı.

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

Yukarıdaki örnek adlı bir sınıf tanımlar `Cocos2D.Camera` türetilen `NSObject` temel türü (Bu tür geldiği `Foundation.NSObject`) ve statik bir özellik tanımlar (`ZEye`), bağımsız değişken olmadan ve yöntemi alır iki yöntem üç alır bağımsız değişkenler.

API dosyası ve kullanabileceğiniz özniteliklerini biçimi ilgili ayrıntılı bir tartışma olarak ele alınmıştır [API tanımlama dosyası](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) bölümüne bakın.

Eksiksiz bir bağlama oluşturmak için genellikle dört bileşenleri ile ilgilenecektir:

-  API tanımı dosyası (`ApiDefinition.cs` şablondaki).
-  İsteğe bağlı: tüm numaralandırmalar, türleri, yapılar API tanımı dosyasında gerekli (`StructsAndEnums.cs` şablondaki).
-  İsteğe bağlı: oluşturulan bağlama genişletin, veya bir daha fazla C# kolay API'si (projeye eklediğiniz tüm C# dosyaları) ek kaynakları.
-  Bağlama yerel kitaplığı.

Bu grafik dosyaları arasındaki ilişki gösterilmektedir:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Bu grafik dosyaları ilişkiyi gösterir.")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

API tanımı dosyası (bir arabirim içeren tüm üyeleri ile), ad alanları ve arabirimi tanımları yalnızca içerir ve sınıflar, numaralandırmalar, temsilciler ya da yapının içermemelidir. API tanımlama dosyası API oluşturmak için kullanılacak olan yalnızca bir sözleşmedir.

Numaralandırmalar istediğiniz veya destek sınıfları alanınızın barındırılması "CameraMode" Yukarıdaki örnekte, ayrı bir dosya çubuğunda ek bir kod CS dosyasında mevcut değil ve ayrı bir dosyada örneğin barındırılmalıdır bir numaralandırma değeridir `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` Dosyası ile birlikte `StructsAndEnum` sınıfı ve çekirdek bağlama kitaplığı oluşturmak için kullanılır. Sonuçta elde edilen kitaplık olarak kullanabileceğiniz-ancak genellikle, okuyan kişinin yararına olması, kullanıcıların bazı C# özelliklerini eklemek için elde edilen kitaplık ayarlamak istediğiniz sayıdır. Uygulama bazı örnekler bir `ToString()` yöntemi, C# dizin oluşturucular sağlamanızı, örtük dönüştürmelerin bazı yerel türlerine ve türlerinden ekleyin ya da bazı yöntemler kesin türü belirtilmiş sürümleri sağlar. Bu geliştirmeler, ek C# dosyaları içinde depolanır. Yalnızca C# dosyaları projenize ekleyin ve bu yapı işleminde eklenecektir.

Bu kodda nasıl uygulamak gösterir, `Extra.cs` dosya. Bunlar bileşiminden üretilen kısmi sınıfları genişletmek gibi parçalı sınıflar kullanacağınız fark `ApiDefinition.cs` ve `StructsAndEnums.cs` çekirdek bağlama:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Kitaplık oluşturma, yerel bağlama oluşturur.

Bu bağlama tamamlamak için yerel kitaplık projesine eklemeniz gerekir.  Bu projeye sağ tıklayıp seçerek projenize, Çözüm Gezgini'nde projeyi üzerine sürükleyip bırakarak yerel kitaplık Bulucu ya da yerel kitaplık ekleyerek yapabilirsiniz **Ekle**  >  **Add Files** yerel bir kitaplığı seçin.
Yerel kitaplıkları kuralı tarafından sözcüğü "LIB" ve ".a" uzantısıyla bitmelidir. Bunu yaptığınızda, Mac için Visual Studio iki dosya ekleyecektir: .a dosyası ve yerel kitaplık ne içerdiğini hakkında bilgi içeren bir otomatik olarak doldurulmuş C# dosyası:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Yerel kitaplıkları kuralı tarafından word lib'ile başlamalı ve uzantı .a ile")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

İçeriğini `libMagicChord.linkwith.cs` dosya bu kitaplığı nasıl kullanılabileceği hakkında bilgiler bulunur ve bu ikili sonuçta elde edilen DLL dosyasına paketlemek için IDE'nizi bildirir:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Tüm nasıl kullanılacağı hakkında ayrıntılar [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) özniteliği belgelenir [bağlama türleri Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md).

Proje oluşturduğunuzda, ile son bulur artık bir `MagicChords.dll` bağlama hem yerel kitaplığı içeren dosya. Bu proje dağıtabilirsiniz veya diğer geliştiriciler için kendi için ortaya çıkan DLL'yi kullanın.

Bazen birkaç numaralandırma değerlerinin, temsilci tanımları veya diğer türleri gerektiğini fark edebilirsiniz. Bu yalnızca bir sözleşme olduğu için bu API tanımlarını dosyasında yerleştirmeyin

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>API tanımı dosyası

API tanımlama dosyası arabirimleri birtakım oluşur. API tanımı arabirimlerde bir sınıf bildirimi içinde kapatılacak ve ile tasarlanmalıdır [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) sınıf için taban sınıf belirtmek için özniteliği.

Neden biz sınıflar arabirimleri yerine sözleşme tanımı kullanmayan merak ediyor olabilirsiniz. API tanımlama dosyası bir yöntem gövdesinde sağlamak zorunda veya bir özel durum veya anlamlı bir değer döndürmek için olan bir gövde sağlamak zorunda kalmadan bir yöntem için sözleşme yazmak bize izin için biz arabirimleri Çekildi.

Ancak, arabirim bir çatısını bir sınıf oluşturmak için kullandığımızdan biz bağlama sürücü için sözleşme öznitelikleri olan çeşitli bölümlerini dekorasyon için başvurmadan gerekiyordu.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Bağlama yöntemleri

Bir yöntemi bağlamak için yapabileceğiniz basit bağlaması var. Yalnızca C# adlandırma kuralları ile arabirimdeki bir yöntem bildirin ve yöntemiyle süslemek [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Özniteliği, ne, C# adını Xamarin.iOS çalışma zamanında Objective-C adıyla bağlar. Parametresi [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği Objective-C Seçici adıdır. Bazı örnekler:

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

Yukarıdaki örnekleri, örnek yöntemleri nasıl bağlayabilirsiniz gösterir. Statik yöntemler bağlamak için kullanmanız gerekir [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) böyle bir öznitelik:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Sözleşmenin bir arabirim bir parçasıdır ve öznitelikleri yine başvurmadan gerekli olmayacak biçimde arabirimler statik ve örnek bildirimleri kavramına sahip olmadığından bu işlem gereklidir. Belirli bir bağlama yönteminden gizlemek istiyorsanız, yöntemiyle süslemek [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği.

`btouch-native` Komut başvuru parametreleri null olmaması için denetimleri getirir. Belirli bir parametre için null değerlere izin vermek istiyorsanız, kullanmanız [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) şöyle parametre özniteliği:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Bir başvuru türü ile verirken [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) anahtar sözcük ayırma semantiği de belirtebilirsiniz. Bu, veri sızmış emin olmak gereklidir.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Bağlama özellikleri

Yöntemler gibi Objective-C özelliklerini kullanarak bağlı [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği ve C# özellikleri doğrudan eşleyin. Yalnızca yöntemler gibi özellikler ile tasarlanabilir [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) ve [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) öznitelikleri.

Kullanırken [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) kapsar btouch yerel altında bir özellik özniteliğini aslında iki yöntem bağlar: alıcı ve ayarlayıcı. Dışarı aktarma için sağladığınız ada **basename** ve ayarlayıcı "ilk harfini kapatma kümesi" sözcüğü eklenerek hesaplanır **basename** büyük harf ve ele Seçici yapmadan bir bağımsız değişken. Diğer bir deyişle `[Export ("label")]` uygulanan bir özellik gerçekten "etiket" bağlar ve "setLabel:" Objective-C yöntemleri.

Bazen Objective-C özelliklerini yukarıda açıklanan düzeni devam etmeyin ve el ile üzerine bir addır. Bu gibi durumlarda, bağlama kullanılarak oluşturulan şekilde denetleyebilirsiniz [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) alıcı veya ayarlayıcı, örneğin özniteliği:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Bu daha sonra "isMenuVisible" bağlar ve "setMenuVisible:". İsteğe bağlı olarak, bir özelliği, aşağıdaki sözdizimini kullanarak bağlanabilir:

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

Alıcı ve ayarlayıcı açıkça tanımlandığı gibi `name` ve `setName` yukarıdaki bağlar.

Kullanarak statik özellikleri için destek yanı sıra [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute), statik iş parçacığı özellikleri ile donatmak [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), örneğin:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Yalnızca bazı parametreler ile işaretlenmiş yöntemler izin vermek gibi [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute), uygulayabileceğiniz [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) bu null bir özellik için geçerli bir değer örnek olduğunu belirten bir özellik için:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Parametresi setter doğrudan da belirtilebilir:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Özel denetimler bağlama uyarılar

Aşağıdaki uyarılar için özel bir denetim bağlama ayarlama göz önünde bulundurulması:

1. **Bağlama özellikleri statik olmalıdır** - bağlama özelliklerinin tanımlarken [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) özniteliği kullanılmalıdır.
 2. **Özellik adlarının tam olarak eşleşmelidir** -özelliği bağlamak için kullanılan adı özel denetimin bir özelliğin adı tam olarak eşleşmelidir.
3. **Özellik türleri tam olarak eşleşmelidir** -değişken türü özelliği bağlamak için kullanılan özel denetim özelliği türünü tam olarak eşleşmelidir.
4. **Kesme noktaları ve alıcı/ayarlayıcı** - kesme noktaları yerleştirilen alıcı veya ayarlayıcı yöntemleri özelliğinin hiç isabet.
5. **Geri çağırmaları gözlemleyin** -özel denetimler özellik değerlerini içindeki değişikliklerin bildirilmesini gözlem geri çağırmaları kullanmanız gerekecektir.

Yukarıda listelenen uyarılar hiçbirini gözlemlemek için hata çalışma zamanında sessizce başarısız bağlama neden olabilir.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Objective-C değişebilir desen ve özellikleri

Objective-C çerçevelerini bazı sınıflar değişebilir bir alt sınıfı ile sabit olduğu bir deyim kullanın. Örneğin `NSString` değişmez sürümü sırasında `NSMutableString` Mutasyon izin veren bir alt sınıfı.

Bu sınıfların bir alıcı, ancak hiçbir ayarlayıcı ile özellikleri içeren sabit taban sınıf görmek için yaygındır. Ayarlayıcı tanıtmak değişebilir sürümü. Bu C# ile gerçekten mümkün olmadığından, bu deyim C# ile işe yarar bir deyim eşlenecek vardı.

Bu C# ' tan eşleştiğinden emin yol temel sınıfta hem alıcı hem de ayarlayıcı eklemeyi, ancak setter işaretleme, bir [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) özniteliği.

Daha sonra değişebilir alt sınıfı üzerinde kullanmanız [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) özellik, özelliğin gerçekten üst öğenin davranışı geçersiz kılma olduğundan emin olmak için özniteliği.

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

### <a name="binding-constructors"></a>Oluşturucular bağlama

`btouch-native` Aracı otomatik olarak oluşturacağını fours Oluşturucular, belirli bir sınıfın sınıfınızdaki `Foo`, onu oluşturur:

-  `Foo ()`: varsayılan oluşturucu (Objective-C'ın "Başlangıç" Oluşturucu eşlenir)
-  `Foo (NSCoder)`: NIB dosyaları seri kaldırma sırasında kullanılan oluşturucuya (Objective-C'ın haritaları "initWithCoder:" Oluşturucu).
-  `Foo (IntPtr handle)`: Oluşturucu tanıtıcı tabanlı oluşturulması için yönetilmeyen bir nesneden bir yönetilen bir nesneyi göstermek Çalışma Zamanı Modülü ihtiyacı olduğunda bu çalışma zamanı tarafından çağrılır.
-  `Foo (NSEmptyFlag)`: Bu çift başlatma önlemek için türetilmiş sınıfları tarafından kullanılır.

Arabirim tanımı içinde aşağıdaki imza kullanarak bildirilmesi için ihtiyaçları tanımladığınız oluşturucular: döndürmesi gereken bir `IntPtr` değeri ve yöntem adı oluşturucusu olmalıdır. Örneğin bağlamak `initWithFrame:` Oluşturucusu budur kullanmanız gerekir:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Bağlama protokolleri

API tasarım belge bölümünde açıklandığı gibi [modelleri ve protokolleri görüştükten](~/ios/internals/api-design/index.md#Models), Xamarin.iOS Objective-C protokolleri ile işaretlenmiş sınıf eşler [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) özniteliği. Bu genellikle Objective-C temsilci sınıfları uygulanırken kullanılır.

Normal bir ilişkili sınıf ve temsilci sınıfını arasında büyük fark, temsilci sınıfı bir veya daha fazla isteğe bağlı yöntemler olabilir ise.

Örneğin göz önünde bulundurun `UIKit` sınıfı `UIAccelerometerDelegate`, nasıl Xamarin.ios'ta bağlı budur:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Bu isteğe bağlı bir yöntem tanımında olduğundan `UIAccelerometerDelegate` yapmak için başka bir şey yoktur. Ancak protokole ilişkin gerekli bir yöntem ise eklemelisiniz [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) özniteliğini yöntemine. Bu, aslında bir gövde yöntemi sağlamak için kullanıcı uygulamasının zorlar.

Genel olarak, protokolleri, iletileri cevaplayin sınıflarda kullanılır. Bu genellikle Objective-C içinde Protokolü yöntemlere yanıt veren bir nesne örneği "temsilci" özelliğine atayarak yapılır.

Xamarin.iOS, herhangi iki Objective-C zamanı gevşek bağlanmış stili desteklemek için kuraldır örneğini bir `NSObject` bir kesin türü belirtilmiş sürümünü da kullanıma sunar ve bu temsilciye atanabilir. Bu nedenle, genellikle hem de sunuyoruz bir `Delegate` türü kesin olarak belirtilmiş bir özellik ve `WeakDelegate` gevşek türü. Biz genellikle geniş yazılmış sürümüyle bağlama [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), biz [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) kesin türü belirtilmiş sürümünü sağlamak için özniteliği.

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

**MonoTouch 7.0 yenilikleri**

İşlevi bağlama yeni ve geliştirilmiş bir protokolü MonoTouch 7.0 ile başlayarak eklenmiştir.  Bu yeni destek belirli bir sınıfın bir veya daha fazla protokolleri benimseme için Objective-C deyimleri kullanımı daha basit hale getirir.

Her Protokolü tanımı `MyProtocol` Objective-C içinde vardır, artık bir `IMyProtocol` Protokolü gerekli tüm yöntemleri listeler arabirimi yanı sıra, isteğe bağlı tüm yöntemler sağlayan bir uzantı sınıfı.  Yukarıdaki, birleştirilmiş Düzenleyicisi sayesinde geliştiriciler, önceki soyut model sınıfları ayrı sınıfları kullanmaya gerek kalmadan Protokolü yöntemleri uygulamak için Xamarin Studio yeni destek.

İçeren herhangi bir tanımını [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) özniteliği gerçekten protokolleri tüketme biçimini büyük ölçüde artıran üç destekleyici sınıfları oluşturun:

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

**Sınıf uygulamasını** bireysel yöntemleri geçersiz kılmak ve tam tür güvenliği alma tam bir Özet sınıf sağlar.  Birden çok devralma desteklemediğinden #c nedeniyle, ancak burada olabilir senaryoları farklı bir temel sınıf olması gerekiyor, ancak yine de nerede olduğu bir arayüzü uygulamak istiyor

Oluşturulan **arabirim tanımı** halinde sunulur.  Tüm gerekli Protokolü yöntemlerinden sahip bir arayüzdür.  Bu, yalnızca arabirim uygulamak için protokolünüzü uygulamak istediğiniz geliştiriciler sağlar.  Çalışma zamanı Protokolü'ı benimsemeyi olarak türü otomatik olarak kaydeder.

Arabirimi yalnızca gereken yöntemini listeler ve isteğe bağlı yöntemleri açığa dikkat edin.  Bu protokol benimseyin sınıfları tam tür için gerekli yöntemleri denetimini alır, ancak zayıf yazmaya başvurmadan gerekecektir anlamına gelir (el ile kullanarak [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) öznitelikleri ve imza eşleştirme) için isteğe bağlı protokol yöntemleri.

Protokolleri kullanan bir API kullanmak üzere güvenli hale getirmek için bağlama aracı ayrıca tüm isteğe bağlı yöntemleri gösteren bir uzantı yöntemi sınıf oluşturur.  Bu, bir API kullanan sürece, protokolleri tüm yöntemleri sahip olacak şekilde ele almanız mümkün olduğu anlamına gelir.

API'nizi protokol tanımlarını kullanmak istiyorsanız, çatı boş arabirimlerden API tanımınıza yazma gerekecektir.  İletişimKuralım bir API kullanmak istiyorsanız, bunu yapmanız gerekir:

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

Yukarıdaki çünkü gerekli bağlama süresi en `IMyProtocol` mevcut değil, diğer bir deyişle neden boş bir arabirim sağlamanız gerekir.

#### <a name="adopting-protocol-generated-interfaces"></a>Benimsemenin protokolü tarafından oluşturulan arabirimleri

Herhangi bir zamanda bu gibi protokoller için oluşturulan arabirimlerinden birini uygulayın:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Şuna eşdeğerdir, bu nedenle uygulama arabirim yöntemleri için uygun ad ile dışarı aktarılan:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Örtük veya açık arabirim uygulanmışsa önemli değildir.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Sınıf uzantısı bağlama

Objective-C içinde yeni yöntemlerle, C# ' ın genişletme yöntemleri Ruhu benzer sınıflarını genişletmek mümkündür. Aşağıdaki yöntemlerden birini olduğunda kullanabileceğiniz [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Objective-C ileti alıcısı olarak yöntemi işaretlemek için özniteliği.

Örneğin, Xamarin.ios'ta biz üzerinde tanımlanan genişletme yöntemleri bağlı `NSString` olduğunda `UIKit` yöntemleri olarak içeri aktarılır `NSStringDrawingExtensions`, şöyle:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Objective-C bağımsız değişken listeleri bağlama

Objective-C, değişen sayıda bağımsız değişken destekler. Örneğin:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

C# bu yöntemini çağırmak için şunun gibi bir imza oluşturmak isteyeceksiniz:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Bu, yukarıdaki API kullanıcılardan gizleyerek ancak kitaplığa gösterme yöntemi dahili olarak bildirir. Ardından, böyle bir yöntem yazabilirsiniz:

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

Bazen bir kitaplıkta bildirilmemişse genel alanlarına erişmek isteyeceksiniz.

Genellikle bu alanlar başvurulmalıdır dize veya tamsayı değerleri içerir. Sözlük anahtarlarını ve belirli bir bildirim temsil eden bir dize olarak yaygın olarak kullanılır.

Bir alana bağlamak için bir özellik arabirimi tanımı dosyanıza ekleyin ve özellik donatmak [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) özniteliği. Bu öznitelik, bir parametre alır: arama için Sembol C adı. Örneğin:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Çeşitli alanları türünden türemez bir statik sınıf içinde sarmalamak istiyorsanız `NSObject`, kullanabileceğiniz [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) şunun gibi bir sınıf özniteliği:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Yukarıdaki oluşturacak bir `LonelyClass` hangi türemiyor gelen `NSObject` ve bağlama içerecek `NSSomeEventNotification` 
 `NSString` olarak kullanıma sunulan bir `NSString`.

[ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Özniteliği aşağıdaki veri türlerini uygulanabilir:

-  `NSString` Başvurular (yalnızca salt okunur özellikler)
-  `NSArray` Başvurular (yalnızca salt okunur özellikler)
-  32-bit tamsayılara (`System.Int32`)
-  64-bit tamsayılara (`System.Int64`)
-  32-bit float (`System.Single`)
-  64-bit float (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

Yerel alan adı ek olarak, alan, kitaplık adını geçirerek bulunduğu kitaplık adını belirtebilirsiniz:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Statik olarak bağlanıyorsanız, yoktur, bağlama kitaplığına kullanmanız gereken `__Internal` adı:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Sabit listeleri bağlama

Ekleyebileceğiniz `enum` bağlamasında doğrudan dosyalara kolaylaştırır (Bu, bağlamalar ve son projenin derlenmesi gereken) farklı bir kaynak dosyasındaki kullanmadan API tanımlarını içinde - kullanmak.

Örnek:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Değiştirilecek kendi numaralandırmalar oluşturmak da mümkündür `NSString` sabitler. Bu durumda Oluşturucu olacak **otomatik olarak** numaralandırmalar değerleri ve NSString sabitleri, dönüştürme yöntemleri oluşturun.

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

Yukarıdaki örnekte donatmak karar `void Perform (NSString mode);` ile bir [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği. Bu **Gizle** bağlama tüketicilerinize sabiti tabanlı API.

Ancak bu tür sınıflara daha Hoş görünmesi API alternatif kullandıkça sınırlandıran bir [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) özniteliği. Bu oluşturulan yöntemler `virtual`, başka bir deyişle, hangi olabilir veya bunları - geçersiz kıl mümkün olmaz, iyi bir seçim olabilir.

Özgün işaretlemek için bir alternatifidir `NSString`-bağlı olarak, tanım olarak `[Protected]`. Bu, gerekli olduğunda çalışması sınıflara izin verir ve wrap'ed sürümü hala çalışır ve geçersiz kılınmış yöntemi çağırın.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Bağlama `NSValue`, `NSNumber`, ve `NSString` daha iyi bir türü

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Özniteliğe izin bağlama `NSNumber`, `NSValue` ve `NSString`(numaralandırmalar) daha doğru C# türlerini. Öznitelik, daha iyi, daha doğru oluşturmak için kullanılabilir .NET API'si üzerinden yerel API.

Yöntemler (dönüş değeri üzerinde) parametreleri ve özellikleri ile donatmak [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute). Tek kısıtlama, üye olan **gerekir** içinde olması bir [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) veya [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) arabirimi.

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

Dahili olarak yapacağız `bool?`  <->  `NSNumber` ve `CGRect`  <->  `NSValue` dönüşümler.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Ayrıca dizilerini destekler `NSNumber` `NSValue` ve `NSString`(numaralandırmalar).

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

`CAScroll` olan bir `NSString` enum, yedeklenen biz sağ getirir `NSString` değerine ve türüne dönüştürmeyi.

Lütfen [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) desteklenen dönüştürme türlerini görmek için belgeler.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Bağlama bildirimleri

Bildirim için gönderilen iletileri olan `NSNotificationCenter.DefaultCenter` ve mekanizması olarak ileti başka bir uygulamanın bir bölümünden yayınlamak için kullanılır. Genellikle kullanarak Bildirimlere abone olan geliştiricilere [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)'s [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) yöntemi. Bir uygulama için bildirim merkezi bir ileti gönderdiğinde, genellikle depolanan bir yükü içerdiği [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) sözlüğü. Bu sözlük zayıf yazılmış ve kullanıcıların genellikle hangi anahtarları sözlük sözlükte depolanan değerlerin türü mevcut belgelerinde okumak gereken bilgileri dışına alınıyor hataya olur. Bazen anahtarları varlığı, bir boolean olarak kullanılır.

Xamarin.iOS bağlaması Oluşturucu bildirimleri bağlamak, geliştiriciler için destek sağlar. Bunu yapmak için ayarladığınız [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) özniteliği de uyarlanmıştır bir özellik ile etiketlenmiş bir [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) (olabilir genel veya özel) özelliği.

Bu öznitelik yükü yok taşıyan bildirimleri için bağımsız değişkenler olmadan kullanılabilir veya belirtebileceğiniz bir `System.Type` başvuran başka bir arabirim API tanımında genellikle "EventArgs" ile biten bir ada sahip. Oluşturucu arabirimi alt sınıflara ayıran bir sınıf içinde açılır `EventArgs` ve orada listelenen tüm özellikler içerir. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Öznitelik değeri getirilecek bir Objective-C sözlük aramak için kullanılan anahtarın adını listelemek için EventArgs sınıfında kullanılmalıdır.

Örneğin:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Yukarıdaki kod, iç içe geçmiş bir sınıf oluşturur `MyClass.Notifications` aşağıdaki yöntemlerle:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Kullanıcıların, kodunuzun daha sonra kolayca abone olabileceği için gönderilen bildirimleri [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) aşağıdakine benzer bir kod kullanarak:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Döndürülen değerle `ObserveDidStart` kolayca böyle bir bildirim almayı durdurmak için kullanılabilir:

```csharp
token.Dispose ();
```

Veya çağırabilirsiniz [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) ve belirtecin geçip. Bildiriminiz parametreleri içeriyorsa, bir yardımcı belirtmelidir `EventArgs` böyle bir arabirim:

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

Yukarıdaki oluşturacak bir `MyScreenChangedEventArgs` sınıfıyla `ScreenX` ve `ScreenY` verileri getirir özellikleri [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) "ScreenXKey" ve "ScreenYKey" tuşlarıyla sözlüğü Sırasıyla ve uygun dönüştürmeleri uygulayın. `[ProbePresence]` Özniteliği için oluşturucuyu anahtarı ayarlanırsa araştırma için kullanılan `UserInfo`, yerine değeri ayıklanmaya çalışılıyor. Bu anahtarın varlığı değeri (genellikle Boole değerleri) olduğu durumlarda kullanılır.

Bu, aşağıdakine benzer bir kod yazmanıza olanak sağlar:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Bağlama kategorileri

Kategoriler, bir dizi yöntem ve bir sınıfta kullanılabilir özellikleri genişletmek için kullanılan bir Objective-C mekanizmasıdır.   Uygulamada, bunlar ya da bir temel sınıf işlevlerini genişletmek için kullanılır (örneğin `NSObject`) olduğunda belirli bir framework bağlı olarak (örneğin `UIKit`), kullanılabilir, ancak yeni framework bağlı olduğunda yalnızca kendi yöntemlerini yapma.   Bazı durumlarda, bunlar bir sınıfın özelliklerini düzenlemek için işlevsellik tarafından kullanılır.   C# genişletme yöntemleri Ruhu benzemez. Bu, bir kategori Objective-C: nasıl gibidir

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Yukarıdaki örnekte, bulunan bir kitaplık örneklerini kapsayacak şekilde genişletilebilir `UIView` yöntemiyle `makeBackgroundRed`.

Bu bağlamak için kullanabileceğiniz [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) arabirim tanımı özniteliği.  Kullanırken [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) özniteliği, anlamını [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) özniteliği değişiklikleri genişletmek için türü genişletmek için temel sınıf belirtmek için kullanılır.

Aşağıdaki gösterildiği nasıl `UIView` uzantılar bağlı ve C# uzantısı yöntemleri açık:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Yukarıdaki oluşturacak bir `MyUIViewExtension` içeren bir sınıf `MakeBackgroundRed` genişletme yöntemi.  Artık "MakeBackgroundRed" çağırabilirsiniz herhangi başka bir deyişle `UIView` alma Objective-c ile aynı işlevlere vererek öğesinin alt sınıfı Bazı durumlarda, kategoriler, sistem sınıfı genişletmeniz değil, ancak yalnızca süsleme amaçlı için işlevselliği düzenlemek için kullanılır.  Böyle:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Hizmetini kullanıyor olsanız da [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) özniteliği de bildirimleri için bu deseni stili de bunları tüm sınıf tanımına eklemeniz yeterlidir.  Bunların her ikisi de aynı elde edersiniz:

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

Kategorileri birleştirmek için bu durumda yalnızca daha kısa:

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

Objective-c C# anonim yöntemler işlevsel denk getirmek için Apple tarafından sunulan yeni bir yapı taşlarıdır Örneğin, `NSSet` sınıfı, bu yöntem artık gösterir:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Yukarıdaki açıklama adlı bir yöntem bildirir `enumerateObjectsUsingBlock:` adlı tek bir bağımsız değişken almayan `block`. Bu blok, geçerli ortam ("Bu" işaretçisi, yerel değişkenleri ve parametreleri erişim) yakalamak için desteğe sahip, bir C# anonim Metoda benzerdir. Yukarıdaki yöntemi `NSSet` çağırır blok iki parametre ile bir `NSObject` ( `id obj` bölümü) ve bir işaretçi bir Boole değeri ( `BOOL *stop`) bölümü.

Bu tür bir API ile btouch bağlamak için C# temsilcisi ve bunun gibi bir API giriş noktasından başvuru olarak blok türü imza ilk bildirmeniz gerekir:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

Ve kodunuzu işlevinizi C# ' tan çağırabilirsiniz:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Tercih ederseniz lambda ifadeleri de kullanabilirsiniz:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Zaman uyumsuz yöntemler

Bağlama Oluşturucu yöntemleri belirli bir sınıfı zaman uyumsuz kolay yöntemlere kapatabilirsiniz (bir görevi veya görev döndüren yöntemler&lt;T&gt;).

Kullanabileceğiniz [ `[Async]` ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) ilgili void dönüş ve, son bağımsız değişken olduğu bir geri çağırma yöntemleri özniteliği.  Bu yönteme uyguladığınızda, bağlama Oluşturucu sonekine sahip bu yöntem bir sürümünü oluşturacaktır `Async`.  Geri çağırma herhangi bir parametre alırsa, dönüş değeri olacak bir `Task`, geri çağırma bir parametre alırsa, sonuç olacaktır bir `Task<T>`.  Geri çağırma birden çok parametre alırsa, ayarlamalısınız `ResultType` veya `ResultTypeName` istenen tüm özellikleri tutulacağı oluşturulan tür adını belirtmek için.

Örnek:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Yukarıdaki kod, her iki önce LoadFile işlevini yöntemi oluşturur olarak:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Zayıf NSDictionary parametreleri için güçlü türler kavrayış

Objective-C API'sindeki pek çok yerde parametre olarak zayıf yazılmış geçirilen `NSDictionary` apı'lerdir ile özel anahtarları ve değerleri, ancak bu hataya (geçersiz anahtarları geçirmek ve uyarı alın; geçersiz değerlerini geçirirsiniz ve uyarı alma) ve can sıkıcı birden çok olası anahtar adlarını ve değerlerini aramak için belgeleri gelişler istedikleri kullanmak için.

Çözüm, API ve arka planda kesin türü belirtilmiş sürümünü çeşitli alt anahtarları ve değerleri eşleyen sağlayan kesin türü belirtilmiş bir sürümü sağlamaktır.

Örneğin, bunu Objective-C API kabul bir `NSDictionary` ve anahtar alma olarak belgelenmiştir `XyzVolumeKey` hangi alır bir `NSNumber` 1.0 için bir toplu değeri 0,0 ile ve `XyzCaptionKey` , bir dize alır, kullanıcılarınızın güzel API istersiniz. şöyle görünür:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Yöntemi Objective-C, burada bir değer nelze nastavit senaryoları olduklarından değerine sahip olacak şekilde bu sözlükler gerektirmez gibi özelliği boş değer atanabilir kayan noktalı olarak tanımlanır.

Bunu yapmak için birkaç şey yapmanız gerekir:

* Alt sınıflara ayıran bir türü kesin belirlenmiş sınıf oluşturma [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) ve çeşitli alıcılar ve ayarlayıcılar için her bir özellik sağlar.
* Alma yöntemi için aşırı yüklemeleri bildirmek `NSDictionary` kesin türü belirtilmiş yeni sürümü almak için.

Türü kesin belirlenmiş sınıf ya da el ile oluşturabilir veya sizin için kitaplıklarımızı Oluşturucu kullanın.  İlk neler olup bittiğini anlamak için el ile bunun nasıl yapılacağını ve ardından otomatik yaklaşım inceleyeceğiz.

Bunun için bir destek dosyası oluşturmanız gerekir, kendi API sözleşmesine geçmez.  Bu, XyzOptions sınıfı oluşturmak yazma gerekir.

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

Ardından, alt düzey API üzerinde üst düzey API'si ortaya çıkarır bir sarmalayıcı yöntem sağlamalıdır.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

API'nizi üzerine gerekmez, güvenli bir şekilde NSDictionary tabanlı API kullanarak gizleyebilirsiniz [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği.

Gördüğünüz gibi kullandığımız [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) yeni bir API giriş noktası yüzey özniteliğini ve biz kullanarak türü kesin belirlenmiş bizim yüzey `XyzOptions` sınıfı.  Sarmalayıcı yöntemin geçilmesi için null de sağlar.

Şimdi biz değil bahsetmek şeyi yerdir `XyzOptionsKeys` değerleri geldiği.  Bir API gibi bir statik sınıfta yüzeyler anahtarlar genellikle grup `XyzOptionsKeys`, şöyle:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Bize bu türü kesin belirlenmiş sözlük oluşturma otomatik desteği bakın.  Bu ortak bolca önler ve dış dosya kullanmak yerine doğrudan API sözleşmeniz, sözlük tanımlayabilirsiniz.

Bir türü kesin belirlenmiş sözlük oluşturmak için bir arabirimde API'nizi tanıtır ve onunla donatmak [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) özniteliği.  Bu oluşturucu, öğesinden türetilen Arabiriminizin aynı ada sahip bir sınıf oluşturmanız gerekir bildirir `DictionaryContainer` için güçlü yazılan erişimciler sağlar.

[ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Özniteliği, sözlüğündeki anahtarları içeren bir statik sınıf adı olan bir parametre alır.  Ardından her bir özellik arabiriminin kesin türü belirtilmiş bir erişimci olur.  Varsayılan olarak, kod özelliğin adını statik sınıf "Anahtarını" soneki ile erişimci oluşturmak için kullanır.

Bu, kesin türü belirtilmiş erişimci oluşturma artık bir dış dosya ya da alıcılar ve ayarlayıcılar her bir özellik için el ile oluşturmak zorunda veya anahtarları el ile arama yapmak zorunda gerektirir kendiniz.

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

Başvuru gerektiği durumlarda, `XyzOption` üyeleri farklı bir alan (yani değil özelliğin adı soneki ile `Key`), özellik donatmak bir [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) adında, öznitelik kullanmak istediğiniz.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Türü eşlemeleri

Bu bölüm, Objective-C tür C# türleri ile nasıl eşleştirildikleri kapsar.

<a name="Simple_Types" />

### <a name="simple-types"></a>Basit türler

Aşağıdaki tabloda, Objective-C ve CocoaTouch dünya türlerinden Xamarin.iOS dünyaya nasıl eşleştiğini gösterir:

|Objective-C tür adı|Xamarin.iOS birleştirilmiş API'sine türü|
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

Xamarin.iOS çalışma zamanı otomatik olarak C# dizilerinin dönüştürme üstlenir `NSArrays` ve dönüştürme geri, örneğin hayali Objective-C yöntemi, bunu döndürür bir `NSArray` , `UIViews`:

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

Bu kullanıcının tahmin veya dizisinde bulunan nesnelerin gerçek türünü bulmak için belgeleri aramak zorunda kalmadan uygun kod tamamlama gerçek türü ile sağlamak IDE izin verdiği bir türü kesin belirlenmiş C# dizi kullanılacak olur.

Kullanabileceğiniz değil izleyebileceğiniz dizisinde bulunan gerçek en çok türetilen tür aşağı durumlarda `NSObject []` dönüş değeri.

<a name="Selectors" />

### <a name="selectors"></a>Seçici

Objective-C API özel türü Seçici görünmez `SEL`. Bir seçici bağlanırken türüyle eşleştirmek `ObjCRuntime.Selector`.  Genellikle seçiciler, hedef nesneye çağırmak için bir API ile hem bir nesne, hedef nesne ve bir seçici içinde sunulur. Bunların her ikisi de temel sağlar C# temsilcisine karşılık gelir: hem çağrılacak yöntem, hem de yöntemi çağırmak için nesne kapsülleyen bir şey.

Bu bağlama benzer.

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

Ve bunu nasıl yöntemi genellikle uygulamada kullanılacaktır:

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

C# geliştiricilerine daha Hoş görünmesi bağlama yapmak için genellikle alan bir yöntem sağlayacak bir `NSAction` C# Temsilciler ve lambda ifadeleri yerine kullanılacak veren parametresi `Target+Selector`. Bu genellikle gizlemek yapmak için `SetTarget` ile işaretleme yöntemi bir [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) özniteliği ve daha sonra bu gibi yeni bir yardımcı yöntem kullanıma:

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

Şimdi, kullanıcı kodu şöyle yazılabilir:

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

Ne zaman dosyalar bağladığınız alan bir yöntem bir `NSString`, C# dize türü, ile hem de dönüş türleri ve parametreleri, değiştirebilirsiniz.

Yalnızca durum kullanmak isteyebileceğiniz bir `NSString` dize belirteç olarak doğrudan kullanılmasıdır. Dizeleri hakkında daha fazla bilgi ve `NSString`okuyun [NSString API tasarımına](~/ios/internals/api-design/nsstring.md) belge.

Bazı nadir durumlarda, bir API C gibi dize sunabileceğinize (`char *`) yerine bir Objective-C dizesi (`NSString *`). Bu gibi durumlarda parametresiyle açıklama ekleyebilirsiniz [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) özniteliği.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>out / ref parametreleri

Bazı API'leri dönüş değerleri kendi parametrelerini veya başvuruya göre parametreler.

Genellikle imza şöyle görünür:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

İlk örnekte hata kodları, işaretçi döndürmek için ortak bir Objective-C deyimi gösterilmektedir bir `NSError` işaretçi geçirilir ve dönüş değeri olarak ayarlanır.   İkinci yöntem, Objective-C yöntemi nasıl ve bir nesne olması içeriğini değiştirme gösterir.   Bu, bir seferde bir saf çıkış değeri yerine, başvuru tarafından olacaktır.

Bağlamanız şöyle görünür:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Bellek yönetimi öznitelikleri

Kullanırken [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) özniteliği ve çağrılan yöntem tarafından korunacak veri kaçı, ikinci parametre olarak, örneğin geçirerek için bu bağımsız değişken semantiği belirtebilirsiniz:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Yukarıdaki "Tut" semantiğine sahip olarak değer bayrak. Kullanılabilir semantiği vardır:

-  Ata
-  Kopyala
-  Tut

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Stil kılavuzları

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>[İç] kullanma

Kullanabileceğiniz [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Genel API yöntemden gizlemek için özniteliği. Burada açık bir API çok düşük düzey ve üst düzey bir uygulama bu yöntemine dayalı olarak ayrı bir dosyada sağlamak istediğiniz durumlarda bu yapmak isteyebilirsiniz.

Bu bağlama Oluşturucu sınırlamaları içine çalıştırdığınızda, örneğin bazı Gelişmiş senaryolar bağlı değil türleri açığa çıkmasına neden olabilecek ve kendi şekilde bağlamak istediğiniz ve kendi şekilde kendiniz türlerine sarmalamak istediğiniz de kullanabilirsiniz.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Olay işleyicileri ve geri aramalar

Objective-C sınıfları, genellikle bildirimleri yayınlayın veya bir temsilci sınıfı (Objective-C temsilci) bir ileti göndererek bilgi isteyin.

Bu model, tam olarak desteklenir ve Xamarin.iOS tarafından ortaya bazen olabilir hantal. Xamarin.iOS, C# olay deseni ve bu gibi durumlarda kullanılabilir sınıf yöntemi geri çağırma sisteminde kullanıma sunar. Bu kodu çalıştırmak için bu gibi sağlar:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Bağlama Oluşturucu C# deseni Objective-C deseni eşleştirmek için gereken yazmaya miktarını azaltarak yeteneğine sahiptir.

Xamarin.iOS 1.4 ile başlatmayı da belirli bir Objective-C temsilciler için bağlamaları üretmek ve temsilci olarak C# olayları ve ana bilgisayar türü özellikleri göstermek için oluşturucu bildirin mümkün olacaktır.

Bu işlemde kullanılan iki sınıf vardır, şu anda olaylar gönderir ve bunları da gönderen bir olacak konak sınıfı `Delegate` veya `WeakDelegate` ve gerçek temsilci sınıfı.

Aşağıdaki Kurulum de göz önünde bulundurur:

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

Sınıf sarmalamak için yapmanız gerekenler şunlardır:

-  Konak sınıfınıza ekleyin, [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   bildirimi, temsilci ve ortaya çıkardığınız C# adı hareket eden tür. Yukarıdaki örnekte, bizim olanlardır `typeof (MyClassDelegate)` ve `WeakDelegate` sırasıyla.
-  Temsilci sınıfınızda ikiden fazla parametresi olan her yönteme otomatik olarak oluşturulan EventArgs sınıf için kullanmak istediğiniz türünü belirtmeniz gerekir.

Bağlama Oluşturucu yalnızca tek bir olay hedef kaydırma sınırlı değildir; diziler, bu kurulum desteklemek için sağlamanız gereken şekilde birden fazla ileti yaymak için bazı Objective-C sınıfları, temsilci seçme, mümkündür. Çoğu ayarları bu gerekli değildir, ancak oluşturucunun bu gibi durumlarda desteklemeye hazırdır.

Sonuç kodu şöyle olacaktır:

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

`EventArgs` Adını belirtmek için kullanılan `EventArgs` sınıf oluşturulacak. İmza her kullanmanız gerekir (Bu örnekte, `EventArgs` içerecek bir `With` türü nint özelliği).

Yukarıdaki tanımlarla Oluşturucu oluşturulan MyClass aşağıdaki olayı üretir:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Bu sayede artık bu kodu kullanabilirsiniz:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Geri çağırmaları yalnızca olay çağrıları gibi fark, olası birden fazla aboneye sahip olmak yerine. (içine Örneğin, birden çok yöntem bağlama bir `Clicked` olay veya `DownloadFinished` olay) geri çağırmaları yalnızca tek bir abone olabilir.

İşlem aynıdır, tek fark, adını kullanıma sunmak yerine, `EventArgs` oluşturulur, EventArgs sınıfı gerçekten elde edilen C# temsilcisi adı için kullanılır.

Temsilci sınıfındaki yöntemi bir değer döndürüyorsa, bağlama Oluşturucu bu olaya yerine üst sınıfın içinde bir temsilci yöntemi içine eşleyecektir. Bu durumlarda kullanıcının takma değil temsilciye yöntem tarafından döndürülen varsayılan değeri sağlamanız gerekir. Bunu yapmak [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) veya [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) öznitelikleri.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) dönüş değeri, sabit kodlamayın olur ancak [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) hangi giriş bağımsız değişkeni döndürülecek belirtmek için kullanılır.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Numaralandırmalar ve temel türleri

Ayrıca, sabit listeleri veya doğrudan btouch arabirimi tanımı sistem tarafından desteklenmeyen temel türler de başvurabilirsiniz. Bunu yapmak için numaralandırmaları ve temel türleri ayrı bir dosyaya koymak ve buna btouch için sağladığınız ek dosyalardan birinde bir parçası olarak dahil.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Bağımlılıkları bağlama

Uygulamanızın bir parçası olmayan API'leri bağlanıyorsanız, yürütülebilir dosyanın karşı bu kitaplıklara bağlı olduğundan emin emin olmanız gerekir.

Xamarin.iOS Kitaplıklarınızı bağlama bildirmeniz gerekir, bu çağırmak için yapı yapılandırmanızı değiştirme ya da yapılabilir `mtouch` bazı ek komutuyla kullanarak yeni kitaplıkları ile bağlama belirtin bağımsız değişkenleri yapı "-gcc_flags" seçeneği Bu gibi programınız için gerekli olan tüm ek kitaplıkları içeren bir tırnak işaretli dize ardından:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Yukarıdaki örnekte bağlayacaksınız `libMyLibrary.a`, `libSystemLibrary.dylib` ve `CFNetwork` son yürütülebilir dosyanızın içine framework kitaplığı.

Veya derleme düzeyi yararlanabilir [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), bu sözleşme dosyalarınızı ekleyebilir (gibi `AssemblyInfo.cs`).
Kullanırken [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), yerel kitaplığınızı yaptığınız, bağlama, bu yerel kitaplık uygulamanızla katıştırır gibi zaman kullanılabilir olması gerekir. Örneğin:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Merak ediyor, neden gerektiğini `-force_load` komut ve neden olduğunu - ObjC bayrak kodu derler olsa da, Kategoriler (Bağlayıcı/derleyici ölü kod eleme kaldırır) desteklemek için gereken meta verilerini korumaz, Xamarin.iOS için çalışma zamanında gerekir.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Yardımlı başvuruları

Geliştiriciler için izlemek için eylem sayfaları ve uyarı kutuları gibi bazı geçici nesneler işlemlerdir ve bağlama Oluşturucu biraz burada yardımcı olabilir.

Örneğin, bir ileti gösterildi ve sonra oluşturulan bir sınıfı varsa bir `Done` olay, bu işleme geleneksel biçimde olacaktır:

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

Yukarıdaki senaryoda Geliştirici nesnesine başvuru kendisini ve her iki sızıntısı tutmak veya etkin olarak kendi kutusunda başvurusunu temizlemek gerekir.  Bağlama kodunu, oluşturucu desteklerken tutma sizin için başvurusunu izlemek ve temizleyin, özel bir yöntem çağrıldığında, yukarıdaki kod ardından olur:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Nasıl, artık sahip yerel bir değişken çalışır durumda olduğunu ve başvuru nesnesi sonlandığında temizlemek gerekli değildir, bir örnekte, değişken tutmak gerekli olduğuna dikkat edin.

Bu yararlanmak için sınıfınıza kümesinde bir olay özelliğine sahip olmalıdır [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) bildirimi hem de `KeepUntilRef` nesne gibi kendi iş tamamlandıktan sonra çağrılan yöntemin adı için değişkeni ayarlayın Bu:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Protokolleri devralıyor

Xamarin.iOS v3.2 itibarıyla ile işaretlenen protokolleri'dan devralan olan destekliyoruz [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) özelliği. Bu belirli API desenleri, gibi kullanışlıdır `MapKit` burada `MKOverlay` protokolü, devralınan `MKAnnotation` protokolünü ve öğesinden devralınan bir dizi sınıfları tarafından benimsenen `NSObject`.

Tarihsel olarak Protokolü kopyalama her uygulama için gerekli, ancak bu durumda biz artık olabilir `MKShape` sınıf türünden devralınır `MKOverlay` protokolü ve oluşturacağını gereken tüm yöntemleri otomatik olarak.

## <a name="related-links"></a>İlgili bağlantılar

- [Bağlama örneği](https://developer.xamarin.com/samples/BindingSample/)
- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
