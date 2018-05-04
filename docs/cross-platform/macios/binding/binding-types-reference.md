---
title: Bağlama türü Başvuru Kılavuzu
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: e064eda3db9aa0156869cf1c7392823553af9bd2
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="binding-types-reference-guide"></a>Bağlama türü Başvuru Kılavuzu

Bu belgede, oluşturulan kodda ve API sözleşme dosyalarınızı bağlama sürücü için ek açıklama eklemek için kullanabileceğiniz özniteliklerin listesi açıklanmaktadır.

Xamarin.iOS ve Xamarin.Mac API sözleşmeleri C# ' ta biçimini tanımlayan çoğunlukla Arabirim tanımları Objective-C kodunu C# ortaya emin yazılır. İşlem bir karışımını arabirimi bildirimleri artı API sözleşme gerektirebilir bazı temel tür tanımlarını içerir. Bizim yardımcı Kılavuzu bağlama türlere giriş için bkz [bağlama Objective-C kitaplıkları](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Tür tanımları

Sözdizimi:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Her arabirimde sahip sözleşme tanımınızı [ `[BaseType]` ](#BaseTypeAttribute) oluşturulan nesnesi için temel tür bildirir özniteliği. Yukarıdaki bildirimde bir `MyType` Objective-C türü için bağlamalar adlı sınıfı C# türü oluşturulmayacak `MyType`.

Typename sonra herhangi bir türü belirtirseniz (yukarıdaki örnekte `Protocol1` ve `Protocol2`) için sözleşmenin parçası olsaydı gibi arabirimi devralma sözdizimini kullanarak içeriği bu arabirimleri içermesinden olacaktır `MyType`.
Bir tür bir protokol uyarlar Xamarin.iOS yüzeyleri tarafından şekilde satır içi kullanım tüm protokolde türü bildirilen özellikleri ve yöntemleri.

Aşağıdaki gösterildiği nasıl Objective-C bildirimi `UITextField` Xamarin.iOS sözleşmede tanımlanmış olması:

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

Bu gibi bir C# API'si sözleşme yazılır:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Diğer öznitelikleri arabirimine uygulama yanı sıra yapılandırma kod oluşturma birçok diğer yönlerini kontrol edebilir [ `[BaseType]` ](#BaseTypeAttribute) özniteliği.


### <a name="generating-events"></a>Olaylar oluşturma

Bir Xamarin.iOS ve Xamarin.Mac API tasarım biz Objective-C temsilci sınıfları C# olayları ve geri aramalar eşlemenizi özelliğidir. Kullanıcılar, bir örnek bazında gibi özellikleri atayarak Objective-C programlama düzeni benimsemeye isteyip istemediklerini seçebilir `Delegate` Objective-C çalışma zamanı çağırırdı çeşitli yöntemleri uygulayan bir sınıf ya da tarafından örneği C# seçme-stil olayları ve özellikleri.

Bize Objective-C modelin nasıl kullanıldığı bir örneğe bakın:

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

Yukarıdaki örnekte, gördüğünüz iki yöntem üzerine yazmayı seçtiniz, kayan bir olay yerinde ve ikincisi, gerçekleştirdiği bir bildirim biridir söyleyen bir Boole değeri döndürmelidir bir geri çağırma `scrollView` için kaydırma gerekmediğini top veya değil.

C# modeli değer döndürmek için beklenen geri aramalar kanca için C# olay sözdizimi veya özellik sözdizimini kullanarak bildirimleri dinlemek kullanıcının kitaplığınızın sağlar.

Bu, nasıl Lambda'lar kullanarak gibi aynı özellik için C# kod aşağıdaki gibidir:

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

Olaylar (geçersiz bir dönüş türüne sahip oldukları) değerleri döndürmeyen beri birden çok kopya bağlanabilir. `ShouldScrollToTop` Bir olay değil yerine türüne sahip bir özellik olduğundan `UIScrollViewCondition` Bu imza vardır:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Döndürdüğü bir `bool` değeri, bu durumda lambda sözdizimi verir yalnızca değer almak `MakeDecision` işlevi.

Bağlama generator destekler oluşturma olayları ve gibi bir sınıf bağlantı özelliklerini `UIScrollView` ile kendi `UIScrollViewDelegate` (iyi çağrısı bu modeli sınıfı), bu açıklama ekleyerek yapılır, [ `[BaseType]` ](#BaseTypeAttribute) tanımıyla `Events` ve `Delegates` parametreleri (aşağıda açıklanmıştır). Ek açıklama olarak [ `[BaseType]` ](#BaseTypeAttribute) bu parametrelere sahip birkaç daha fazla bileşen Oluşturucu bildirmek gerekli değildir.

Birden fazla parametre alan olayları için (Objective-C kuralı bir temsilci sınıfta ilk parametre gönderen nesne örneğini olmasıdır) için oluşturulan istediğiniz adı sağlamalısınız `EventArgs` olmasını sınıfı. Bu gerçekleştirilir [ `[EventArgs]` ](#EventArgsAttribute) modeli sınıfınız yöntemi bildiriminde özniteliği. Örneğin:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Yukarıdaki bildirimde oluşturacak bir `UIImagePickerImagePickedEventArgs` öğesinden türetilen sınıf `EventArgs` ve her iki parametre paketleri `UIImage` ve `NSDictionary`. Oluşturucunun bu üretir:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Ardından aşağıdakileri gösteren `UIImagePickerController` sınıfı:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

Bir değer döndürmesi modeli yöntemleri farklı bağlıdır. Bu, hem bir ad durumda kullanıcı uygulaması kendisi sağlamaz döndürmek oluşturulan C# temsilci (imza yöntemi için) ve ayrıca varsayılan bir değer gerektirir. Örneğin, `ShouldScrollToTop` tanımı şudur:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

Yukarıdaki oluşturacak bir `UIScrollViewCondition` temsilci imzayla, yukarıda gösterilen ve kullanıcı bir uygulama sağlamıyorsa, dönüş değeri true olur.

Ek olarak [ `[DefaultValue]` ](#DefaultValueAttribute) özniteliğini de kullanabilirsiniz [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute) çağrısı veya Belirtilenparametredeğerinidöndürmekiçinoluşturucununyönlendirirözniteliği[ `[NoDefaultValue]` ](#NoDefaultValueAttribute) varsayılan değer yoktur oluşturucunun bildirir parametresi.

<a name="BaseTypeAttribute" />

### <a name="basetypeattribute"></a>BaseTypeAttribute

Sözdizimi:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

Kullandığınız `Name` Objective-C dünyasında bu tür bağlanacağı adı denetleme özelliği. Bu genellikle C# türü, .NET Framework tasarım yönergeleri ile uyumlu olan, ancak, bu kuralı izlemez Objective-C adlarında eşleşen bir ad vermek için kullanılır.

Örneğin, aşağıdaki durumda biz Objective-C eşleme `NSURLConnection` için yazın `NSUrlConnection`, "URL" yerine "Url".NET Framework tasarım yönergeleri kullanın:

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Belirtilen ad değeri olarak oluşturulan için kullanılan `[Register]` bağlama özniteliği. Varsa `Name` belirtilmezse, tür kısa ad değeri olarak kullanılan `[Register]` oluşturulan çıktı özniteliği.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events ve BaseType.Delegates

Bu özellikleri C# nesil sürücü için kullanılan-oluşturulan sınıflar olayları stili. Bunlar, kendi Objective-C temsilci sınıfı ile belirli bir sınıfın bağlamak için kullanılır. Çoğu durumda olduğu bir sınıf bildirimleri ve olayları göndermek için bir temsilci sınıfı kullanır karşılaşır. Örneğin bir `BarcodeScanner` bir yardımcı olması gereken `BardodeScannerDelegate` sınıfı. `BarcodeScanner` Sınıfı genellikle sahip olabilir bir `Delegate` örneği atamanız gerekir özelliği `BarcodeScannerDelegate` bu works while için kullanıcılarınız için C# kullanıma sunmak isteyebilirsiniz-stil olay arabirimini gibi ve bu durumlarda kullanırsınız`Events`ve `Delegates` özelliklerini [ `[BaseType]` ](#BaseTypeAttribute) özniteliği.

Bu özellikleri her zaman birlikte ayarlanır ve aynı sayıda öğe olması gerekir ve eşitleme tutulmalıdır. `Delegates` Dizi sarmalamak istediğiniz her zayıf yazılmış temsilci için bir dize içeriyor ve `Events` dizi ile ilişkilendirmek istediğiniz her bir türü için bir türü içeriyor.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

Bu sınıfın yeni örnekleri oluşturulduğunda bu öznitelik uygularsanız, söz konusu nesne örneğini geçici bir çözüm tarafından başvurulan kadar tutulacak `KeepRefUntil` çağrılmış. Bu, kodunuzu kullanılacak bir nesneye başvuru tutmak için kullanıcı istemediğinizde, Apı'lerinizi kullanılabilirliğini artırmak kullanışlıdır. Bu özelliğin değeri bir yöntem adıdır `Delegate` sınıfı ile birlikte bu kullanmalısınız `Events` ve `Delegates` özellikleri de.

Aşağıdaki örnek Göster bu tarafından nasıl kullanıldığını `UIActionSheet` Xamarin.iOS içinde:

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

Bu öznitelik için arabirim tanımı uygulandığında varsayılan oluşturucu oluşturan Oluşturucu engeller.

Nesne sınıfında diğer oluşturucular biriyle başlatılması gerektiğinde bu özniteliğini kullanın.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

Bu öznitelik için arabirim tanımı uygulandığında varsayılan oluşturucu özel olarak işaretleyecektir. Bu, hala nesne bu sınıfın dahili uzantısı dosyanızdan örneği, ancak yalnızca paylaşmıyor sınıfınız kullanıcılara erişilemez anlamına gelir.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Bu öznitelik bir tür tanımında Objective-C kategorileri bağlamak için ve Objective-C işlevselliği kullanıma sunan şekilde yansıtmak için C# genişletme yöntemleri olarak kullanıma sunmak için kullanın.

Kategoriler yöntemleri ve özellikleri bir sınıfta kullanılabilir kümesini genişletmek için kullanılan bir Objective-C mekanizması bulunur.   Uygulamada, bunlar ya da bir temel sınıf işlevselliğini genişletmek için kullanılır (örneğin `NSObject`) ne zaman belirli bir framework bağlı olarak (örneğin `UIKit`), kullanılabilir, ancak yeni framework bağlıysa yalnızca kendi yöntemlerini yapma.   Bazı durumlarda, bunlar bir sınıf özelliklerini düzenlemek için işlevsellik tarafından kullanılır.   Bunlar için genişletme yöntemleri C# içinde Ruhu benzerdir.

Bir kategori hedefi-C: nasıl gibidir budur

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Yukarıdaki örnekte örneklerini genişletir bir kitaplıkta bulunan `UIView` yöntemiyle `makeBackgroundRed`.

Bu bağlamak için kullanabileceğiniz [ `[Category]` ](#CategoryAttribute) arabirim tanımı özniteliği.   Kullanırken [ `[Category]` ](#CategoryAttribute) özniteliği, anlamını [ `[BaseType]` ](#BaseTypeAttribute) özniteliği değiştirir genişletmek için türü olmaya genişletmek için temel sınıf belirtmek için kullanılır.

Aşağıdaki gösterildiği nasıl `UIView` uzantıları bağlı ve C# genişletme yöntemleri açık:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Yukarıdaki oluşturacak bir `MyUIViewExtension` içeren bir sınıf `MakeBackgroundRed` genişletme yöntemi.   Şimdi arayabileceğiniz yani `MakeBackgroundRed` herhangi `UIView` almak Objective-c üzerinde aynı işlevselliği vermiş bir alt

Bazı durumlarda, bulacaksınız **statik** üyeleri kategorileri içinde aşağıdaki örnekte ister:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

Bu neden bir **yanlış** kategori C# arabirim tanımı:

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Bu yanlıştır çünkü kullanmak için `BoolMethod` uzantı örneği ihtiyacınız `FooObject` ancak bir ObjC bağlama **statik** bu uzantısıdır C# genişletme yöntemleri nasıl uygulandığını bulgu nedeniyle bir yan etkisi.

Yukarıdaki tanımları kullanmanın tek yolu aşağıdaki çirkin kodu tarafından.:

```csharp
(null as FooObject).BoolMethod (range);
```

Bu durumu önlemek için satır içi önerilir `BoolMethod` tanımının içinde `FooObject` arabirim tanımı kendisi, bu etmenizi sağlar amaçlıdır gibi bu uzantı çağrı `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Biz Bul olduğunda bir uyarı (BI1117) yayımlayacak mı bir [ `[Static]` ](#StaticAttribute) üye iç bir [ `[Category]` ](#CategoryAttribute) tanımı. Gerçekten sahip olmak istiyorsanız [ `[Static]` ](#StaticAttribute) üyeleri içinde [ `[Category]` ](#CategoryAttribute) , sessiz uyarı kullanarak tanımları `[Category (allowStaticMembers: true)]` veya üye veya dekorasyon[ `[Category]` ](#CategoryAttribute) arabirim tanımıyla [ `[Internal]` ](#InternalAttribute).

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

Bu öznitelik bir sınıfa uygulandığında, yalnızca bir statik sınıf, bir türünden türemez oluşturur `NSObject`, böylece [ `[BaseType]` ](#BaseTypeAttribute) özniteliği göz ardı edilir. Statik sınıflar kullanıma sunmak istediğiniz C genel değişkenleri barındırmak için kullanılır.

Örneğin:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Bir C# sınıfına aşağıdaki API ile oluşturur:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>Tanımları Protokolü/Model

Modelleri genellikle Protokolü uygulaması tarafından kullanılır.
Çalışma zamanı yalnızca gerçekten üzerine yöntemleri Objective-C ile kaydeder, bunların farklı.
Aksi takdirde yöntemi kaydedilmez.

Bu genel olduğunda anlamına gelir, bir alt kümesi ile işaretlenmiş bir sınıf `ModelAttribute`, temel yöntemi çağırmalıdır değil.   Bu metodu çağıran bir özel durum oluşturur, herhangi bir yöntem, geçersiz kılmak için bir alt üzerindeki tüm davranışı uygulamak beklenir.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

Varsayılan olarak, bir protokol parçası olan üyeleri zorunlu değildir. Bu öğesinin bir alt kümesi oluşturmak kullanıcılara `Model` yalnızca C# sınıfı türetme ve yalnızca bunlar çok önem verdiğiniz yöntemlerini geçersiz kılma nesne. Kullanıcı bu yöntemin bir uygulaması sağlar Objective-C sözleşme bazen gerektirir (olanlar ile işaretlenmiş `@required` Objective-C yönergesini). Bu durumda, bu yöntemleri bayrak `[Abstract]` özniteliği.

`[Abstract]` Özniteliği yöntemler veya özellikler için uygulanabilir ve oluşturulan üye soyut ve bir soyut sınıfı için sınıf işaretlemek için oluşturucunun neden olur.

Aşağıdaki Xamarin.iOS alınır:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

Kullanıcı bu belirli yöntemi, Model nesnesi için bir yöntem sağlamıyorsa modeli yöntemi tarafından döndürülecek varsayılan bir değer belirtir

Sözdizimi:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Örneğin, sınıfında aşağıdaki sanal temsilci bir `Camera` sınıfı, size sağladığımız bir `ShouldUploadToServer` hangi ortaya bir özellik olarak üzerinde `Camera` sınıfı. Varsa kullanıcısı `Camera` sınıfı açıkça ayarlamaz bir değerin true veya false yanıtlayabilir bir lambda için varsayılan değeri dönüş bu durumda false olacaktır biz belirtilen değeri `DefaultValue` özniteliği:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Kullanıcı bir işleyici sanal sınıfında ayarlar, bu değer dikkate:

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

Ayrıca bkz: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

Sözdizimi:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Bu öznitelik bir model sınıfı bir değer döndüren bir yöntem üzerinde sağlandığında kullanıcı kendi yöntemi veya lambda sağlamadı, belirtilen parametre değerini döndürmek için oluşturucunun ister.

Örnek:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

Yukarıdaki örnek IF kullanıcısı `NSAnimation` Sınıf C# olaylar/özelliklerinden herhangi birini kullanmak seçtiğiniz ve ayarlanmadı `NSAnimation.ComputeAnimationCurve` yöntemi veya lambda için dönüş değerini ilerleme parametrede aktarılan değer olacaktır.

Ayrıca bkz: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

Bazen bir olay kullanıma veya bu özniteliği eklemek ile donatılmış herhangi bir yöntemini oluşturulmasını önlemek için oluşturucunun girmemesini şekilde özelliği bir Model sınıfı ana bilgisayar sınıfına temsilci mantıklıdır.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

Bu öznitelik modeli yöntemlerinde dönüş değerleri kullanmak için temsilci imza adını ayarlamak için kullanılır.

Örnek:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

Yukarıdaki tanımıyla oluşturucunun aşağıdaki ortak bildirim üretir:

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

Bu öznitelik ana bilgisayar sınıfında oluşturulan özelliğinin adını değiştirmek için oluşturucunun izin vermek için kullanılır. Bazen FooDelegate sınıfı yönteminin adı, temsilci sınıfı için anlamlı ancak tek bir özellik olarak ana bilgisayar sınıfındaki görünür durumunda faydalı olur.

Ayrıca bu gerçekten yararlı (gerekli) ise zaman iki sahip veya mantıklıdır birden çok aşırı yükleme yöntemleri kalmalarını FooDelegate sınıfında olduğu gibi adlı ancak bunları daha iyi belirli bir ada sahip ana bilgisayar sınıfındaki kullanıma sunmak istediğiniz.

Örnek:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

Yukarıdaki tanımıyla Oluşturucu ana sınıf aşağıdaki ortak bildiriminde üretir:

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

Birden fazla parametre alan olayları için (Objective-C kuralı bir temsilci sınıfta ilk parametre gönderen nesne örneğini olmasıdır) olması için oluşturulan EventArgs sınıf istediğiniz adı sağlamanız gerekir. Bu gerçekleştirilir `[EventArgs]` yöntemi bildiriminde özniteliği, `Model` sınıfı.

Örneğin:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Yukarıdaki bildirimde oluşturacak bir `UIImagePickerImagePickedEventArgs` EventArgs'dan türetilen ve her iki parametre paketleri sınıfı `UIImage` ve `NSDictionary`. Oluşturucunun bu üretir:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Ardından aşağıdakileri gösteren `UIImagePickerController` sınıfı:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

Bu öznitelik, bir olay veya sınıfında oluşturulan özellik adını değiştirmek için oluşturucunun izin vermek için kullanılır. Bazen Model sınıfı yönteminin adı, model sınıfı için anlamlı ancak bir olay veya özellik kaynak sınıfında tek görünür durumunda faydalı olur.

Örneğin, `UIWebView` aşağıdaki bit kullanan `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

Yukarıdaki çıkarır `LoadingFinished` yöntemi olarak `UIWebViewDelegate`, ancak `LoadFinished` kadar için olay olarak bir `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

Uyguladığınızda `[Model]` , sözleşme API, çalışma zamanı tür tanımında özniteliğine kullanıcı sınıfında bir yöntem geçersiz kıldı, çağrılarını sınıftaki yöntemlerin yalnızca belirir özel kod üretir. Bu öznitelik, genellikle bir Objective-C temsilci sınıfı sarmalama tüm API'leri için uygulanır.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

Model üzerinde yöntemi varsayılan bir dönüş değeri sağlamaz belirtir.

Bu yanıt vererek Objective-C çalışma zamanı ile çalışır `false` Objective-C çalışma zamanı isteği belirtilen Seçici bu sınıfta uygulanmışsa belirlemek için.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

Ayrıca bkz: [ `[DefaultValue]` ](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>protokolleri

Objective-C Protokolü kavram gerçekten C# ' ta yok. C# arabirimlerine protokolleri benzer ancak bir protokol bildirilen özellikler ve yöntemler tüm bu uyarlar sınıfı tarafından uygulanmalı, bunların farklı. Bunun yerine bazı yöntemleri ve özellikleri isteğe bağlıdır.

Bazı protokoller, genellikle Model sınıfları olarak kullanılır, bunlar kullanarak bağlanmalıdır [ `[Model]` ](#ModelAttribute) özniteliği.

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
```

İşlev bağlama yeni ve geliştirilmiş bir iletişim kuralı Xamarin.iOS 7.0 ile başlayan bir araya getirilmiştir.  İçeren herhangi bir tanımının `[Protocol]` özniteliği gerçekte çok protokolleri kullanma şeklini geliştirmek üç destekleyen sınıfları oluşturun:

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

**Sınıf uygulamasını** tek tek yöntemlerini geçersiz kılın ve tam tür güvenliği alma tam bir Özet sınıf sağlar. Ancak C birden çok devralma desteklemediğinden # nedeniyle, burada, farklı bir temel sınıf gerektirir, ancak yine de bir arabirim istiyor senaryo vardır.

Bu yerdir oluşturulan **arabirim tanımı** devreye girer.  Protokol gereken tüm yöntemleri sahip bir arabirimdir.  Bu, yalnızca arabirimi uygulamak için protokol uygulamak isterseniz geliştiriciler sağlar.  Çalışma zamanı Protokolü'nu benimseme olarak türü otomatik olarak kaydeder.

Arabirimi yalnızca gerekli yöntemlerini listeler ve isteğe bağlı yöntemler kullanıma dikkat edin.   Bu protokol benimsemeye sınıfları için gerekli yöntemleri denetimi tam tür alır, ancak (el ile dışarı aktarım öznitelikleri kullanarak ve imza eşleştirme) zayıf yazmaya çözümlemelere gerekecek için isteğe bağlı Protokolü yöntemleri anlamına gelir.

Protokollerini kullanan bir API kullanmak üzere kullanışlı hale getirmek Bağlama aracı ayrıca tüm isteğe bağlı yöntemleri gösteren bir uzantıları yöntemi sınıf oluşturur.   Bu bir API kullanıyor sürece, tüm yöntemleri sahip olarak protokolleri ele almanız mümkün olacağı anlamına gelir.

API'nizi protokol tanımlarını kullanmak istiyorsanız, API tanımı'nda iskelet boş arabirimlerden yazma gerekecektir.   İletişimKuralım bir API kullanmak istiyorsanız, bunu yapmanız gerekir:

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

### <a name="adopting-protocol-generated-interfaces"></a>Protokol oluşturulan arabirimleri uygulamasını kullanma

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

### <a name="protocol-inlining"></a>Satır içi kullanım Protokolü

Bir protokol benimsenmesi olarak bildirilen mevcut Objective-C türleri bağlamak olsa da, satır içi Protokolü doğrudan isteyeceksiniz. Bunu yapmak için yalnızca, protokol olarak olmadan herhangi bir arabirim bildirin [ `[BaseType]` ](#BaseTypeAttribute) özniteliği ve arabiriminiz için temel arabirimleri listesine protokolünde listesi.

Örnek:

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```


## <a name="member-definitions"></a>Üye tanımları

Bu bölümdeki öznitelikleri bir türde ayrı ayrı üyelerine uygulanır: özellik ve yöntem bildirimleri.


### <a name="alignattribute"></a>AlignAttribute

Özellik dönüş türleri hizalama değeri belirtmek için kullanılır. Belirli sınırlarında hizalanmalıdır adresleri işaretçiler belirli özellikler ele (Bu gerçekleşir örneğin bazı Xamarin.iOS içinde `GLKBaseEffect` 16 bayt olmalıdır özellikleri hizalı). Alıcı tasarlamanız için bu özelliği kullanın ve hizalama değeri kullanın. Bu genellikle ile kullanılan `OpenTK.Vector4` ve `OpenTK.Matrix4` Objective-C API'leri ile tümleştirildiğinde türleri.

Örnek:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`[Appearance]` Özniteliği iOS 5, sınırlı Görünüm Yöneticisi'ni nereye sunulmuştur.

`[Appearance]` Özniteliği, herhangi bir yöntemi veya katılma özelliği uygulanabilir `UIAppearance` framework. Bu öznitelik bir yöntemi veya özelliği bir sınıfta uygulandığında, bu sınıfın tüm örnekleri ya da belirli ölçütlere uyan örnekleri stilini belirlemek için kullanılan bir görünüm türü kesin belirlenmiş sınıf oluşturmak için bağlama Oluşturucu yönlendirir.

Örnek:

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

Yukarıdaki aşağıdaki kod UIToolbar oluşturur:

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

Kullanım `[AutoReleaseAttribute]` yöntemleri ve özellikleri yöntemine yöntem çağrısını sarmalamak için bir `NSAutoReleasePool`.

Objective-C dönüş varsayılan eklenen değerleri bazı yöntemler vardır `NSAutoReleasePool`. Varsayılan olarak, bu, iş parçacığı geçecek `NSAutoReleasePool`, ancak Yönetilen Nesne yaşadığı sürece Xamarin.iOS de nesnelerinizi başvuru tutar olduğundan, ek bir başvuru tut istemeyebilirsiniz `NSAutoReleasePool` hangi yalnızca boşaltmış kadar iş parçacığı sonraki iş parçacığına döndürür denetlemek veya ana döngü geri dönün.

Bu öznitelik örneğin ağır özellikleri uygulanır (örneğin `UIImage.FromFile`) varsayılan eklenen nesneleri döndürür `NSAutoReleasePool`. İş parçacığı denetimi için ana döngü döndürmedi sürece bu öznitelik olmadan, görüntüleri korunması. Her zaman etkin arka plan yükleyici çeşit UF, iş parçacığı oldu ve iş için bekleyen, görüntüleri hiçbir zaman yayımlanması.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]` Döndürülen yönetilmeyen nesne bağlama tanımı'nda açıklanan türüyle eşleşmiyor olsa bile bir yönetilen türü oluşturulmasını zorlamak için kullanılıyor.

Yerel yöntemi döndürülen türü bir başlığında açıklanan türüyle eşleşmiyor gerektiğinde bu faydalıdır, örneğin aşağıdaki Objective-C tanımından ele `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Döndürülecek olduğunu açıkça bildiren bir `NSURLSessionDownloadTask` örneği, ancak henüz bunu **döndürür** bir `NSURLSessionTask`, bir üst sınıf olduğu ve bu nedenle değil dönüştürülebilir `NSURLSessionDownloadTask`. Biz bir tür kullanımı uyumlu bağlamında olduğundan bir `InvalidCastException` gerçekleşir.

Üstbilgi açıklaması ile uyumlu ve önlemek için `InvalidCastException`, `[ForcedTypeAttribute]` kullanılır.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]` De adlı bir boolean değeri kabul eder `Owns` diğer bir deyişle `false` varsayılan `[ForcedType (owns: true)]`. Parametresi, izlemek için kullanılır sahibi [sahipliği ilkesini](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) için **çekirdek Foundation** nesneleri.

`[ForcedTypeAttribute]` Yalnızca parametreleri, özellikleri ve dönüş değeri geçerli değil.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]` Bağlama verir `NSNumber`, `NSValue` ve `NSString`(numaralandırmaları) daha doğru C# türleri içine. Öznitelik daha iyi ve daha doğru oluşturmak için kullanılan yerel API üzerinden .NET API.

Yöntemlerde (dönüş değeri), parametreleri ve özellikleri ile işaretleme `BindAs`. Tek kısıtlama, üye olan **bulunmamalıdır** içinde olması bir `[Protocol]` veya [ `[Model]` ](#ModelAttribute) arabirimi.

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

Geçerli desteklenen kapsülleme türleri şunlardır:

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

/ Dosyalarına saklanmasını desteklenen aşağıdaki C# veri türleri `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* CGRect / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

/ Dosyalarına saklanmasını desteklenen aşağıdaki C# veri türleri `NSNumber`:

* bool
* byte
* çift
* float
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* Numaralandırmalar

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute) conjuntion ile çalışır [numaralandırmaları NSString sabiti tarafından yedeklenen](#enum-attributes) daha iyi .NET API, örneğin oluşturabilmesi için:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

Çıkış:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Biz işleyecek `enum`  <->  `NSString` yalnızca sağlanan enum için yazarsanız dönüştürme [ `[BindAs]` ](#BindAsAttribute) olan [NSString sabiti tarafından yedeklenen](#enum-attributes).

#### <a name="arrays"></a>Diziler

[`[BindAs]`](#BindAsAttribute) Ayrıca diziler destekler desteklenen türlerden her birini bir örnek olarak aşağıdaki API tanımı sahip olabilir:

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

Çıkış:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects` Parametresi kapsüllenmiş içine bir `NSArray` içeren bir `NSValue` her `CGRect` ve bir dizi iade alırsınız `CAScroll?` içinde oluşturulan döndürülen değerleri kullanılarak `NSArray` içeren `NSStrings`.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]` Özniteliğine sahip iki kullanan bir yöntem veya özellik bildirimi ve tek tek bir alıcı veya özellik ayarlayıcı uygulandığında başka bir uygulandığında bir.

Kullanıldığında bir yöntemi veya özelliği, etkisini `[Bind]` özniteliktir belirtilen Seçici çağıran bir yöntem oluşturmak için. Ancak elde edilen oluşturulan yöntemi ile donatılmış değil [ `[Export]` ](#ExportAttribute) bu yöntemi geçersiz kılma katılabilir değil demektir özniteliği. Bu genellikle birlikte kullanılan `[Target]` Objective-C genişletme yöntemleri uygulamak için öznitelik.

Örneğin:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Bir alıcı veya ayarlayıcı, kullanıldığında `[Bind]` özniteliği bir özellik için'Set ' yordamı Objective-C Seçici adları oluşturulurken Kod Oluşturucu tarafından olayla varsayılanları değiştirmek için kullanılır. Adında bir özellik bayrak zaman varsayılan olarak `fooBar`, oluşturucu oluşturur bir `fooBar` dışarı aktarmak için bir alıcı ve `setFooBar:` ayarlayıcı için. Bazı durumlarda bu kuralı Objective-C izlemez, genellikle bunlar olmasını alıcı adını değiştirmek `isFooBar`.
Bu oluşturucu bildirmek için bu öznitelik kullanırsınız.

Örneğin:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

Yalnızca Xamarin.iOS 6.3 kullanılabilir ve daha yeni.

Bu öznitelik tamamlama işleyicisi kendi son bağımsız değişken olarak ele yöntemleri uygulanabilir.

Kullanabileceğiniz `[Async]` özniteliği, en son bağımsız değişken bir geri çağırma olduğu yöntemleri.  Bu yönteme uyguladığınızda, bağlama oluşturucunun soneki bu yöntem bir sürümünü oluşturur `Async`.  Geri çağırma parametre almayan, dönüş değeri olacaktır bir `Task`, geri çağırma parametresi alırsa, sonuç olacak bir `Task<T>`.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Aşağıdakiler bu zaman uyumsuz yöntem üretir:

```csharp
Task LoadFileAsync (string file);
```

Geri çağırma birden çok parametre sürerse ayarlamalısınız `ResultType` veya `ResultTypeName` istenen tüm özellikleri tutacak oluşturulan tür adını belirtmek için.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Bu zaman uyumsuz yöntem aşağıdaki oluşturacak nerede `FileLoading` hem erişim özellikleri içeren `files` ve `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

Geri arama son parametre ise bir `NSError`, ardından oluşturulan `Async` yöntemi değeri null değil ve bu durumda, oluşturulan async yöntemi görev özel durum ayarlayacaktır denetler.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

Yukarıdaki aşağıdaki zaman uyumsuz yöntem oluşturur:

```csharp
Task<string> UploadAsync (string file);
```

Ve hatası, sonuçta elde edilen görev ayarlamak özel durum olacak bir `NSErrorException` elde edilen sarmalar `NSError`.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

Döndürmek için değeri belirtmek için bu özelliği kullanmak `Task` nesnesi.   Bu parametre mevcut bir türü alır, bu nedenle, çekirdek API tanımlarını birinde tanımlanacak gerekir.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

Döndürmek için değeri belirtmek için bu özelliği kullanmak `Task` nesnesi.   Bu parametre, istenen tür adını alır, bir dizi özellik, bir geri çağırma alan her bir parametreye üreteci oluşturur.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

Oluşturulan zaman uyumsuz yöntemleri adını özelleştirmek için bu özelliği kullanın.   Varsayılan yöntemin adını kullanın ve "Zaman uyumsuz" metin eklemek için bu varsayılanı değiştirmek için bunu kullanabilirsiniz.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

Bu öznitelik dizesi parametreleri veya dize özellikleri için uygulanır ve bu parametre için hazırlama sıfır kopyalama dize kullanmamayı Kod Oluşturucu bildirir ve bunun yerine yeni bir NSString örnek C# dizeden oluşturun.
Sıfır kopyalama dize kullanarak sıralama kullanmak için oluşturucunun istemeniz durumunda bu öznitelik yalnızca dizeleri gerekli `--zero-copy` komut satırı seçeneği veya derleme düzeyi özniteliğini `ZeroCopyStringsAttribute`.

Bu özellik hedefi-olmasını C'de bildirilen olduğu durumlarda gereklidir bir `retain` veya `assign` özelliği yerine bir `copy` özelliği. Bunlar genellikle yanlış "geliştiriciler tarafından iyileştirilmiş" üçüncü taraf kitaplıklarında gerçekleşir. Genel olarak, `retain` veya `assign` `NSString` özellikler bu yana yanlış `NSMutableString` veya kullanıcı türetilmiş sınıfları `NSString` dilden çok az çiğnemekten kitaplık kodu bilgisi olmadan dizeleri içeriğini değiştirebilir uygulama. Genellikle bu erken iyileştirmesi nedeniyle oluşur.

Aşağıdaki gibi iki özellikleri hedefi-C: gösterir

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

Uyguladığınızda `[DisposeAttribute]` eklenecek bir kod parçacığı sağlayan bir sınıfa `Dispose()` sınıfının yöntem uygulaması.

Bu yana `Dispose` yöntemi tarafından oluşturulan otomatik olarak `bmac-native` ve `btouch-native` araçlarını kullanmanızı gerektiren `[Dispose]` bazı kodda oluşturulan eklemesine özniteliği `Dispose` yöntem uygulaması.

Örneğin:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]` Özniteliği bir yöntemi veya özelliği Objective-C çalışma zamanı için açığa çıkarılması bayrak için kullanılır. Bu öznitelik, bağlama aracı ve gerçek Xamarin.iOS ve Xamarin.Mac çalışma zamanları arasında paylaşılır. Özellikler, alıcı hem de ayarlayıcı dışarı base bildirime dayanarak oluşturulan için yöntemleri için parametre üretilen kod için verbatim geçirilir (bölümüne bakarak [ `[BindAttribute]` ](#BindAttribute) alter hakkında bilgi için Aracı'nın davranışı bağlama).

Sözdizimi:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

[Seçici](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) temel Objective-C yöntem veya bağlı özellik adını temsil eder.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

Bu öznitelik, bir C genel değişkeni isteğe bağlı olarak yüklenir ve C# kodu kullanıma bir alan olarak kullanıma sunmak için kullanılır. Genellikle bu C ya da Objective-C tanımlanır ve bazı API'leri kullanılan ya da belirteçleri olabilir veya değerleri donuk olarak kullanılmalıdır sabitleri değerlerini almak için gerekli-kullanıcı kodu tarafından.

Sözdizimi:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` İle bağlamak için C simgesi. Varsayılan olarak bu tür tanımlandığı adı ad alanından çıkarımı yapılan bir Kitaplığı'ndan yüklenir. Bu nerede simgenin Aranan kitaplığı değilse, geçirmelisiniz `libraryName` parametresi. Bir statik kitaplık bağlanıyorsanız kullanmak `__Internal` olarak `libraryName` parametresi.

Oluşturulan her zaman statik özelliklerdir.

Alan özniteliği ile işaretlenmiş özellikler aşağıdaki türde olabilir:

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* Numaralandırmalar

Ayarlayıcılar desteklenmemektedir [numaralandırmaları NSString sabitler tarafından yedeklenen](#enum-attributes), ancak bunlar el ile gerekirse bağlı olabilir.

Örnek:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

`[Internal]` Yöntemleri ya da özellikleri özniteliği uygulanabilir ve oluşturulan kodu ile işaretleme etkisi vardır `internal` kodu oluşturulan derlemede koduna yalnızca erişilebilir hale getirme C# anahtar sözcüğü. Bu, genellikle çok düşük düzey API'leri gizlemek veya üzerine veya Oluşturucu tarafından desteklenmeyen API'ları için artırmak istediğiniz uygun olmayan bir ortak API sağlar ve bazı elle kodlama gerektiren için kullanılır.

Bağlama tasarlama, genellikle yöntemi veya özelliği bu özniteliği kullanılarak Gizle ve yöntemi veya özelliği için farklı bir ad sağlayın ve ardından, C# tamamlayıcı destek dosya üzerinde kesin türü belirtilmiş kullanıma sunan bir sarmalayıcı eklediğiniz zaman temel işlevselliği.

Örneğin:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Sonra destek dosyanızda, biraz kod şöyle olabilir:

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

Bu öznitelik ile .NET açıklama eklemek bir özellik için yedekleme alanı bayrakları `[ThreadStatic]` özniteliği. Bu alan bir iş parçacığı statik değişkeni ise yararlı olur.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

Bu öznitelik, bir yöntem destek yerel (Objective-C) özel durumları hale getirir.
Çağırmak yerine `objc_msgSend` çağırma ObjectiveC özel durumları yakalar ve bunları yönetilen özel durumlar sıralar özel bir trampoline aracılığıyla doğrudan geçer.

Şu anda yalnızca birkaç `objc_msgSend` imzalar desteklenir (, eksik monotouch_ ile yerel bağlama bağlama kullanan bir uygulama başarısız olduğunda bir imza desteklenmiyor, bulacaksınız *_objc_msgSend* simgesi), ancak daha fazla olabilir isteğiyle eklendi.


### <a name="newattribute"></a>Newattrıbute

Bu öznitelik yöntemlerine uygulanır ve oluşturucunun sağlamak için özellikler oluşturmak `new` bildirimi önünde anahtar sözcüğü.

Taban sınıf içinde zaten var olan bir alt kümesi içinde aynı yöntemi veya özelliği adı eklendiğinde Derleyici uyarılarını önlemek amacıyla kullanılır.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

Bu öznitelik alanları kesin türü belirtilmiş yardımcıyı bildirimleri sınıf oluşturucu üretmek için uygulayabilirsiniz.

Bu öznitelik yükü yok taşımak bildirimler için bağımsız değişkenler olmadan kullanılabilir veya belirleyebileceğiniz bir `System.Type` başvuran başka bir API tanımı arabiriminde genellikle "EventArgs" ile biten ada sahip. Oluşturucu arabirimi o alt sınıfların bir sınıfına dönüşecektir `EventArgs` ve orada listelenen tüm özellikler içerir. [ `[Export]` ](#ExportAttribute) Özniteliği kullanılmalıdır `EventArgs` Objective-C sözlüğün değeri getirmek aramak için kullanılan anahtarın adını listelemek için sınıf.

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
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Kullanıcıların kodunuzu daha sonra kolayca abone olabileceği için gönderilen bildirimleri [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) aşağıdakine benzer bir kod kullanarak:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Veya izlemek için belirli bir nesneyi ayarlayın. Geçirirseniz `null` için `objectToObserve` bu yöntem yalnızca kendi diğer eş gibi davranır.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

Döndürülen değerle `ObserveDidStart` kolayca şöyle bildirimleri almayı durdurmak için kullanılabilir:

```csharp
token.Dispose ();
```

Ya da çağırabilirsiniz [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//) ve belirtecin geçip. Bildiriminizin parametreleri içeriyorsa, bir yardımcı belirtmelisiniz `EventArgs` arabirimi şöyle:

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

Yukarıdaki oluşturacak bir `MyScreenChangedEventArgs` ile sınıf `ScreenX` ve `ScreenY` verileri getirir özellikleri [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) anahtarlar kullanılarak sözlük `ScreenXKey` ve `ScreenYKey` Sırasıyla ve uygun dönüşümleri uygulayın. `[ProbePresence]` Özniteliği için oluşturucuyu anahtar ayarlanırsa araştırma için kullanılan `UserInfo`, değerini ayıklayın çalışılırken yerine. Bu anahtar varlığını değeri (genellikle Boole değerleri) olduğu durumlarda kullanılır.

Bu, aşağıdakine benzer bir kod yazmanıza olanak sağlar:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

Bazı durumlarda, bir sözlük geçirilen değeri ile ilişkili hiçbir sabiti yoktur.  Apple bazen ortak simgesi sabitleri ve bazen dize sabitleri kullanır.  Varsayılan olarak [ `[Export]` ](#ExportAttribute) , sağlanan özniteliğinde `EventArgs` sınıfı kullanacak belirtilen ad ortak bir simge çalışma zamanında Bakılacak.  Bu durumda değilse ve bunun yerine, bir dize sabit değer olarak Bakılacak sonra geçirmek gerektiği `ArgumentSemantic.Assign` dışarı aktarma özniteliği değeri.

**Xamarin.iOS 8.4 yenilikler**

Bazı durumlarda, bildirimler tüm bağımsız değişkenler olmadan yaşam böylece başlayacak kullanımını [ `[Notification]` ](#NotificationAttribute) bağımsız değişkenler olmadan kabul edilebilir.  Ancak bazı durumlarda, bildirim parametreleri görülecektir.  Bu senaryoyu desteklemek için öznitelik birden çok kez uygulanabilir.

Bir bağlama geliştirdiğiniz ve var olan kullanıcı kodu çiğnemekten önlemek istiyorsanız, mevcut bir bildirim alanından kapatma:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

Bildirim özniteliği iki kez şöyle listeleyen bir sürüme:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

Bu özelliğe uygulandığında bir değere izin verme olarak özellik bayrakları `null` için atanacak. Bu yalnızca başvuru türleri için geçerli olur.

Ne zaman bu uygulanan gösterir belirtilen parametre null olabilir ve Denetimsiz geçirme için gerçekleştirilmelidir yöntemi imza parametresinde `null` değerleri.

Başvuru türü bu öznitelik yoksa, bağlama aracı Objective-C geçirmeden önce atanmasını değeri için bir onay oluşturur ve özel durum oluşturacak bir onay oluşturacak bir `ArgumentNullException` atanan değer ise `null`.

Örneğin:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

Bu belirli yöntemi için bağlama ile işaretlenen bağlama Oluşturucu istemek için bu öznitelik kullanın bir `override` anahtar sözcüğü.

### <a name="presnippetattribute"></a>PreSnippetAttribute

Giriş parametreleri doğrulandıktan sonra ancak Objective-c kodunu çağrılarını önce eklenecek biraz kod eklemesine bu özniteliği kullanabilirsiniz

Örnek:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

Bu öznitelik parametrelerinden herhangi birini oluşturulan yönteminde doğrulandığı önce eklenecek biraz kod eklemesine kullanabilirsiniz.

Örnek:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

Belirtilen özellik değeri buradan getirmek için bu sınıftan çağrılacak bağlama Oluşturucu bildirir.

Bu özellik genellikle başvurulan nesne grafiğinin tutmak başvuru nesneleri işaret önbelleğini yenilemek için kullanılır. Genellikle Ekle/Kaldır gibi işlemleri sahip Code görüntülersiniz. Bu yöntem, böylece öğeleri eklendiğinde veya iç önbellek emin olmak için güncelleştirilmesi kaldırılarak sonra gerçekte kullanımda olan nesneler için yönetilen başvuru tutuyor musunuz kullanılır. Bağlama aracı verilen bağlamasında tüm başvuru nesneleri için bir yedekleme alan oluşturduğundan bu mümkün olur.

Örnek:

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

Bu durumda, `Dependencies` özelliği ekleyerek veya kaldırarak bağımlılıklardan sonra çağrılacak `NSOperation` gerçek gösteren bir grafik sahibiz sağlama nesne yüklenen nesneleri, hem bellek sızıntıları, hem de Bellek Bozulması engelliyor.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

Kod altta yatan Objective-C yöntemi çağrılmış sonra eklenecek bazı C# kaynak kodu eklemesine bu özniteliği kullanabilirsiniz

Örnek:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

Bu öznitelik, proxy nesneleri olduğu bayrak değer döndürmek için uygulanır. Kullanıcı bağlantılardan ayırt değil bazı Objective-C API'lerini dönüş proxy nesneleri. Nesne olarak işaretlemek için bu öznitelik etkisi olan bir `DirectBinding` nesnesi. Xamarin.Mac bir senaryo için gördüğünüz [bu hatayı tartışma](https://bugzilla.novell.com/show_bug.cgi?id=670844).

### <a name="retainlistattribute"></a>RetainListAttribute

Parametresi için yönetilen başvuru tutun veya parametresine bir iç başvurusunu kaldırmak için oluşturucunun bildirir. Bu, başvurulan nesneler tutmak için kullanılır.

Sözdizimi:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Varsa değerini `doAdd` parametresi için eklendikten sonra doğrudur `__mt_{0}_var List<NSObject>;`. Burada `{0}` ile değiştirilir verilen `listName`. Sınıfınızda tamamlayıcı kısmi API Bu yedekleme alanını bildirmeniz gerekir.

Bir örnek için bkz [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) ve [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

Bu dönüş türleri oluşturucunun çağırmalıdır belirtmek için uygulanabilir `Release` onu dönmeden önce nesne üzerinde. Bir yöntem (aksine, en yaygın senaryo bir autoreleased nesnesi) tutuldu nesne verdiğinde yalnızca bu gereklidir

Örnek:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

Ayrıca bu tür bir işleve Objective-C döndürme bağlı nesne korumanız gerekir Xamarin.iOS çalışma zamanı bilir böylece bu öznitelik için oluşturulan kodu, yayılır.


### <a name="sealedattribute"></a>SealedAttribute

Oluşturulan yöntemi korumalı olarak işaretlemek için oluşturucunun bildirir. Bu özniteliği belirtilmezse, varsayılan sanal bir yöntem (sanal bir yöntem, soyut bir yöntem veya diğer öznitelikleri nasıl kullanıldığını bağlı olarak bir geçersiz kılma) oluşturmaktır.

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

Zaman `[Static]` özniteliği bir yöntemi veya özelliği uygulandığında, bu bir statik yöntem veya özellik oluşturur. Bu özniteliği belirtilmezse, bir örnek yöntemi veya özelliği üreteci oluşturur.


### <a name="transientattribute"></a>TransientAttribute

Bu öznitelik değerleri başka bir deyişle, geçici, bayrak özelliklerine geçici olarak iOS tarafından oluşturulur ancak uzun süreli olmayan nesneleri kullanın. Bu öznitelik bir özelliğe uygulandığında oluşturucunun yönetilen sınıf nesnesine başvuru korumaz anlamına gelir. Bu özellik için bir yedekleme alanını oluşturmaz.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Xamarin.iOS/Xamarin.Mac bağlamaları tasarımında `[Wrap]` özniteliği, türü kesin belirlenmiş bir nesne zayıf yazılmış bir nesneyle kaydırmak için kullanılır. Bu genellikle türünden olduğu bildirilen çoğunlukla Objective-C temsilci nesneleri ile oyuna gelir `id` veya `NSObject`. Xamarin.iOS ve Xamarin.Mac tarafından kullanılan bu temsilciler veya veri kaynağı türü olarak kullanıma sunmak için kuraldır `NSObject` ve kuralı "Weak" + yararlanılmasını ad ile adlandırılır. Bir `id delegate` Objective-C özelliğinden ortaya olarak bir `NSObject WeakDelegate { get; set; }` API sözleşme dosyası bir özellik.

Ancak biz güçlü tür yüzey ve uygulamak için genellikle bu temsilciye atanmış değer güçlü bir tür `[Wrap]` özniteliği, yani kullanıcılar bazı ince denetim ihtiyacınız varsa veya alt düzey tric çözümlemelere gerekirse zayıf türleri kullanmayı da seçebilirsiniz görevleri veya kesin türü belirtilmiş özelliği çalışmalarının çoğu için kullanabilirsiniz.

Örnek:

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

Bu kullanıcı temsilci zayıf yazılmış sürümü nasıl kullanır.

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

Ve bu olduğu kullanıcı, kesin türü belirtilmiş sürümünü kullanmak duyuru kullanıcı C# ' ın tür sistemi avantajlarından yararlanır ve kendi hedefi bildirmek için override anahtar sözcüğünü kullanarak ve kendisi için el ile sahip olup olmadığını nasıl dekorasyon yöntemiyle `[Export]`, yaptığımız beri kullanıcı için bağlama çalışır:

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

Başka bir kullanımını `[Wrap]` özniteliktir yöntemleri kesin türü belirtilmiş sürümünü desteklemek için.  Örneğin:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Tarafından oluşturulan üyeleri `[Wrap]` değil `virtual` gerekirse varsayılan olarak, bir `virtual` için ayarlayabileceğiniz üye `true` isteğe bağlı `isVirtual` parametresi.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>Parametre öznitelikleri

Bu bölümde bir yöntemin tanımı parametrelerinde uygulayabileceğiniz öznitelikleri açıklanmaktadır yanı sıra `[NullAttribute]` bir bütün olarak bir özellik için geçerlidir.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

Bu öznitelik parametre türleri bağlayıcı bildirim söz konusu parametre çağırma Objective-C bloğuyla uyan ve bu şekilde sıralama C# temsilci bildirimlerden uygulanır.

Bu genellikle şu şekilde hedefi-C: tanımlanan geri aramalar için kullanılır

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Ayrıca bkz: [CCallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

Bu öznitelik parametre türleri bağlayıcı bildirim söz konusu parametre C ABI işlevi işaretçisi çağırma kuralı için uyumlu ve bu şekilde sıralama C# temsilci bildirimlerden uygulanır.

Bu genellikle şu şekilde hedefi-C: tanımlanan geri aramalar için kullanılır

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Ayrıca bkz: [BlockCallback](#BlockCallback).

### <a name="params"></a>Parametreleri

Kullanabileceğiniz `[Params]` tanımı'ndaki "params" ekleme Oluşturucu için bir yöntem tanımını son dizi parametresinin özniteliği.   Bu bağlama için isteğe bağlı parametreler kolayca olanak sağlar.

Örneğin, aşağıdaki tanımı:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

Yazılması için aşağıdaki kodu sağlar:

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

Bu öğeleri geçirme için yalnızca bir dizi oluşturmak kullanıcıların gerektirmez eklenen avantajına sahiptir.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

Kullanabileceğiniz `[PlainString]` parametre olarak geçirme yerine bir C dize olarak bir dizeyi geçirmek için bağlama oluşturucunun istemek üzere dizesi parametreleri önünde öznitelik bir `NSString`.

Çoğu Objective-C API'lerini geliştirmenize `NSString` parametreleri ancak kullanıma API'leri sayıda bir `char *` yerine dizeleri, geçirme için API `NSString` değişim.
Kullanım `[PlainString]` bu durumlarda.

Örneğin, aşağıdaki Objective-C bildirimi:

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

Bu gibi bağlanması:

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

Belirtilen parametre bir başvuru tutmak için oluşturucunun bildirir. Oluşturucunun Bu alan için yedekleme deposu sağlayacak veya bir ad belirtebilirsiniz ( `WrapName`) değerinde depolamak için. Bu, Objective-C için parametre olarak geçirilir ve Objective-C, bu nesnenin kopyasını yalnızca devam edilecek bildiğinizde yönetilen bir nesne için bir başvuru tutmak kullanışlıdır. Örneğin, bir API gibi `SetDisplay (SomeObject)` aynı anda bir nesne, SetDisplay yalnızca görüntüleyebilir olası olduğundan bu öznitelik kullanırsınız. Birden fazla nesne (örneğin, için yığın benzeri API) izlemek gerekiyorsa kullanacağınız `[RetainList]` özniteliği.

Sözdizimi:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

Parametresi için yönetilen başvuru tutun veya parametresine bir iç başvurusunu kaldırmak için oluşturucunun bildirir. Bu, başvurulan nesneler tutmak için kullanılır.

Sözdizimi:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Varsa değerini `doAdd` parametresi için eklendikten sonra doğrudur `__mt_{0}_var List<NSObject>`. Burada `{0}` ile değiştirilir verilen `listName`. Sınıfınızda tamamlayıcı kısmi API Bu yedekleme alanını bildirmeniz gerekir.

Bir örnek için bkz [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) ve [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

Bu öznitelik parametreleri uygulanır ve yalnızca Objective-C C# geçiş kullanılır.  Bu geçişleri çeşitli Objective-C sırasında `NSObject` parametreleri, yönetilen bir nesne gösterimini sarılır.

Çalışma zamanı yerel nesneye bir başvurusu alın ve başvuru nesnesi son yönetilen referansı kaldırılmıştır olduğunu ve GC çalıştırmak için bir fırsat kadar tutun.

Bazı durumlarda, yerel nesneye bir başvurusu tutma C# çalışma zamanı için önemlidir.  Bu durum, bazen temel alınan yerel kod parametresi yaşam döngüsü için özel bir davranış eklenmiş ortaya çıkar.  Örneğin: yıkıcı parametresi için bazı temizleme eylemi gerçekleştirin veya bazı değerli kaynak dispose.

Bu öznitelik geri Objective-C için üzerine yönteminden döndürülürken mümkünse çıkarılması nesnesine işlemleriniz çalışma zamanı bildirir.

Kural basittir: çalışma zamanı yerel nesnesinden yeni bir yönetilen temsilini oluşturmak zorunda sonra işlevi sonunda yerel nesne için tut sayısı bırakılacak ve yönetilen nesne tanıtıcı özelliğinin temizlenir.   Bu yönetilen nesne başvuru bıraktıysanız, bu başvuruyu gereksiz olacaktır anlamına gelir (Bu yöntemleri çağırma throw bir özel durum).

Zorlanmış dispose geçirilen nesnesi oluşturulamadı ya da bir bekleyen Yönetilen Nesne gösterimini zaten olduysa, gerçekleşmez. 


## <a name="property-attributes"></a>Özellik öznitelikleri

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

Bu öznitelik, burada bir alıcı özelliğiyle bir taban sınıf içinde sunulmuştur ve değişebilir bir alt ayarlayıcı tanıtır Objective-C deyim desteklemek için kullanılır.

C# bu modeli desteklemez, temel sınıf kurucu ve alıcı sahip olması ve bir alt kümesi kullanabilirsiniz beri [OverrideAttribute](#OverrideAttribute).

Bu öznitelik yalnızca özellik ayarlayıcıları kullanılır ve değişebilir deyim Objective-c desteklemek için kullanılır

Örnek:

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Enum öznitelikleri

Eşleme `NSString` sabitleri enum değerleri için daha iyi .NET API oluşturmak için kolay bir yolu olmadığını. Bu:

* göstererek daha kullanışlı olması kod tamamlama verir **yalnızca** ; API için doğru değerleri
* tür güvenliği ekler başka kullanamazsınız `NSString` sabit bir yanlış bağlamda ve
* kod tamamlama işlevselliği kaybetmeden kısa API listesini göster yapma bazı sabitleri gizlemek için sağlar.

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
```

Yukarıdaki bağlama tanımından oluşturucunun oluşturacak `enum` kendisi ve ayrıca oluşturacak bir `*Extensions` enum değerleri arasında iki yolu dönüştürme yöntemleri içeren statik türü ve `NSString` sabitleri. Bunun anlamı sabitleri kalır geliştiricilerinin kullanabileceği bunlar API parçası olmasa bile.

Örnekler:

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

İşaretleme **bir** bu öznitelikle enum değeri. Bu liste değeri bilinmiyor, döndürülen sabiti olur.

Yukarıdaki örnekten:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Enum değeri donatılırsa sonra bir `NotSupportedException` oluşturulur.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

Hata kodları enum değerleri olarak bağlanır. Genellikle bir hata etki alanı için bunları yoktur ve her zaman hangisinin geçerli Bul (veya bir bile var olup olmadığını) kolay değildir.

Hata etki alanı enum ile ilişkilendirmek için bu özniteliği kullanabilirsiniz.

Örnek:

```csharp
    [Native]
    [ErrorDomain ("AVKitErrorDomain")]
    public enum AVKitError : nint {
        None = 0,
        Unknown = -1000,
        PictureInPictureStartFailed = -1001
    }
```

Ardından genişletme yöntemi çağırabilirsiniz `GetDomain` etki alanı herhangi bir hata sabit alınamıyor.

### <a name="fieldattribute"></a>FieldAttribute

Bu aynı olduğundan `[Field]` türü içinde sabitleri için kullanılan öznitelik. Bu ayrıca numaralandırmaları içinde belirli bir sabit bir değerle eşleştirmek için kullanılabilir.

A `null` değeri belirtmek için kullanılabilir değilse hangi enum değeri'nin döndürülmesi bir `null` `NSString` sabiti belirtilir.

Yukarıdaki örnekten:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Öyle değilse `null` değerdir mevcut bir `ArgumentNullException` oluşturulur.

## <a name="global-attributes"></a>Genel Öznitelikler

Genel Öznitelikler, kullanma ya da uygulanır `[assembly:]` özniteliği değiştiricisi gibi [ `[LinkWithAttribute]` ](#LinkWithAttribute) veya olabilir her yerden, gibi kullanılan [ `[Lion]` ](#SinceAndLionAttributes) ve [ `[Since]` ](#SinceAndLionAttributes) öznitelikleri.

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

Bu bağlama bayrakları gcc_flags el ile yapılandırmak için tüketici kitaplığının zorlamadan ilişkili bir kitaplığı yeniden kullanmak için gereken ve bir kitaplıkta geçirilen fazladan mtouch bağımsız değişkenleri belirtmek geliştiricilere sağlayan bir derleme düzeyi özniteliğidir.

Sözdizimi:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

Bu öznitelik derleme düzeyinde uygulanır, örneğin, bu nedir [CorePlot bağlamaları](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) kullanın:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

Kullandığınızda `[LinkWith]` özniteliği, belirtilen `libraryName` hem yönetilmeyen bağımlılıkları, hem de düzgün bir şekilde kullanmak gerekli komut satırı bayrakları içeren tek bir DLL sevk etmek kullanıcıların elde edilen derlemeye katıştırılmış Xamarin.iOS kitaplığından.

Değil sağlamak mümkündür bir `libraryName`, bu durumda `LinkWith` özniteliği, yalnızca ek bağlayıcı bayrakları belirtmek için kullanılabilir:

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute oluşturucular

Bu Oluşturucu ile bağlantı ve kitaplık destekleyen desteklenen hedefleri ve kitaplığı ile bağlantı için gerekli olan herhangi bir isteğe bağlı kitaplık bayrağı, sonuçta elde edilen derlemesini katıştırmak için kitaplık belirtmenizi sağlar.

Unutmayın `LinkTarget` bağımsız değişkeni tarafından Xamarin.iOS algılanır ve ayarlanmış olması gerekmez.

Örnekler:

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

`ForceLoad` Karar vermek için kullanılan özellik desteklemediğini `-force_load` bağlantı bayrağı yerel kitaplığı bağlama için kullanılır. Şimdilik, bu her zaman true olmalıdır.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

Bağlanan kitaplığı hiçbir çerçevesinde sabit bir gereksinim olup olmadığını (dışında `Foundation` ve `UIKit`), ayarlamanız gerekir `Frameworks` gerekli platformu çerçeveleri boşlukla ayrılmış bir listesini içeren bir dize özelliği. Örneğin, bir kitaplık bağlama varsa gerektiren `CoreGraphics` ve `CoreText`, ayarlamalısınız `Frameworks` özelliğine `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

Bu özellik, sonuçta elde edilen yürütülebilir varsayılan yerine bir C Derleyici bir C++ derleyicisi ile derlenmesi gerekiyorsa true olarak ayarlayın. Bağlama kitaplığı C++ ile yazılmış varsa bunu kullanın.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

Paket için yönetilmeyen kitaplık adı. Bu "bir" uzantılı bir dosya ve (örneğin, ARM ve benzetici x86 için) birden çok platform için nesne kodu içerebilir.

Xamarin.iOS önceki sürümlerinde kullanıma `LinkTarget` kitaplığınızın desteklenen platform belirlemek için özelliği, ancak bu artık otomatik olarak algılanır ve `LinkTarget` özelliği yoksayılır.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags` Dize bağlama belirtmelerini herhangi bir ek bağlayıcı bayrağı gereken yerel kitaplığı uygulamasına bağlanırken için bir yol sağlar.

Örneğin, yerel kitaplığı libxml2 ve zlib gerektiriyorsa, ayarlamalısınız `LinkerFlags` dizesinden `"-lxml2 -lz"`.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Xamarin.iOS önceki sürümlerinde kullanıma `LinkTarget` kitaplığınızın desteklenen platform belirlemek için özelliği, ancak bu artık otomatik olarak algılanır ve `LinkTarget` özelliği yoksayılır.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

Bu özellik, bağlama kitaplığı GCC özel durum işleme kitaplığı (gcc_eh) gerektiriyorsa true olarak ayarlayın

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

`SmartLink` Özelliği, belirlemek Xamarin.iOS izin vermek için true olarak ayarlanmalıdır olup olmadığını `ForceLoad` veya gereklidir.

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks` Özelliği aynı şekilde çalışır `Frameworks` hariç bağlantı zamanında, özellik `-weak_framework` belirticisi gcc için her listelenen çerçeveleri geçirilir.

`WeakFrameworks` kullanılabilir, ancak ek özellikler eklemek için kitaplığınızın istediyseniz kullanışlıdır sabit bir bağımlılık bunlardaki yeni almayan varsa bunlar isteğe bağlı olarak bunları kullanabilmesi için kitaplıkları ve platformu çerçeveleri karşı zayıf bağlantı uygulamaları mümkün hale getirir iOS sürümleri. Zayıf bağlama ile ilgili daha fazla bilgi için Apple'nın belgelerine bakın [zayıf bağlama](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Zayıf bağlama için iyi bir aday olacaktır `Frameworks` ister hesapları `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` ve `Twitter` yalnızca iOS 5 kullanılabilir olduğu.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) ve LionAttribute (macOS)

Kullandığınız `[Since]` özniteliği bayrağı API'ler belirli bir noktada zamanında eklenen sahip olarak. Öznitelik, yalnızca türleri ve temel sınıfı, yöntemi veya özelliği kullanılabilir değilse, bir çalışma zamanı sorunu neden olabilecek yöntemleri bayrak için kullanılmalıdır.

Sözdizimi:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

İşletim sisteminin daha eski bir sürümü olan bir aygıtta yürütülen varsa bu çalışma zamanı hatası neden olmaz gibi genel numaralandırmalar, kısıtlamalar veya yeni yapılar uygulanmamalıdır.

Bir türe uygulandığında örnek:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

Yeni bir üye uygulandığında örnek:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`[Lion]` Özniteliği aynı şekilde ancak Lion ile sunulan türleri için uygulanır. Kullanmak için neden `[Lion]` bu iOS düzenlendi sıklıkla, OS X sürüm nadiren gerçekleşir ve kendi kod bu adı sürüm numarasına göre tarafından işletim sistemi unutmayın daha kolaydır karşı iOS kullanılan daha özel sürüm numarası olan

### <a name="adviceattribute"></a>AdviceAttribute

Bu öznitelik, geliştiricilerin bunları kullanmak daha kullanışlı olabilir diğer API'leri hakkında bir ipucu vermek için kullanın.   Bir API kesin türü belirtilmiş bir sürümünü sağlayın, örneğin, bu öznitelik zayıf yazılmış öznitelikte daha iyi API geliştiriciye yönlendirmek için kullanabilirsiniz.

Bu öznitelik bilgileri belgelerde gösterilir ve Araçlar geliştirme hakkında kullanıcı önerileri vermek için geliştirilmiş olmalıdır

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Yalnızca Xamarin.iOS 5.4 kullanılabilir ve daha yeni.

Bu öznitelik oluşturucunun bildirir, belirli Bu kitaplık için bağlama (ile uyguladıysanız `[assembly:]`) veya türü, hızlı sıfır kopyalama dize sıralama kullanmalıdır. Bu komut satırı seçeneği geçirme için eşdeğer bir özniteliktir `--zero-copy` oluşturucusuna.

Sıfır kopyalama için dizeleri kullanırken oluşturucunun etkili bir şekilde aynı C# dize yeni oluşturulmasını yansıtılmasını olmadan Objective-C tüketir dize olarak kullanır `NSString` nesne ve verileri C# dizelerden Objective-C dizeye kopyalayarak önleme. Sıfır kopyalama dizeleri kullanmanın tek dezavantajı emin olarak işaretlenmesini gerçekleşen, sarmalayan bir dize özelliği olmanız gerekir, olan `retain` veya `copy` sahip `[DisableZeroCopy]` öznitelik kümesi. Bu gerektirir sıfır kopyalama dizeleri için tanıtıcı yığında ayrılan ve dönüş işlevi geçersiz olduğu için.

Örnek:

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

Derleme düzeyinde özniteliği de uygulayabilirsiniz ve derlemenin tüm türleri için geçerli olacaktır:

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>Kesin türü belirtilmiş sözlük

Xamarin.iOS 8.0 ile kolayca bu kaydırma kesin türü belirtilmiş sınıfları oluşturmak için destek gösterdiğimizi `NSDictionaries`.

Bu her zaman kullanmak mümkün olmuştur sırada [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) el ile bir API ile birlikte veri türü, artık bunu yapmak çok daha kolaydır.  Daha fazla bilgi için bkz: [görünmesini güçlü türleri](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

Bu öznitelik için bir arabirim uygulandığında, türetilen arabirimle aynı ada sahip bir sınıf oluşturucu üretecektir [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) ve bir kesin türü belirtilmiş arabiriminde tanımlanan her bir özellik kapatır ' Set'yordamı için Sözlük.

Bu mevcut bir otomatik olarak oluşturulabilir bir sınıf oluşturur `NSDictionary` veya, oluşturulan yeni.

Bu öznitelik bir parametre, sözlük öğelerde erişmek için kullanılan anahtarları içeren sınıf adını alır.   Varsayılan olarak özniteliğiyle arabiriminde her bir özellik için belirtilen tür için bir ad soneki "Anahtarı" üye arama.

Örneğin:

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

Yukarıdaki örnekte `MyOption` sınıfı, bir dize özelliği için oluşturacak `Name` , kullanacağınız `MyOptionKeys.NameKey` sözlüğüne bir dize almak için anahtar olarak.   Ve kullanacağı `MyOptionKeys.AgeKey` sözlüğüne almak için anahtar olarak bir `NSNumber` int içerir

Farklı bir anahtarı kullanmak istiyorsanız, dışarı aktarma özniteliği özelliği, örneğin kullanabilirsiniz:

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>Güçlü sözlük türleri

Aşağıdaki veri türlerini desteklenen `StrongDictionary` tanımı:

|C# arabirim türü|`NSDictionary` Depolama türü|
|---|---|
|`bool`|`Boolean` depolanmış bir `NSNumber`|
|Numaralandırma değerleri|tamsayı saklanan bir `NSNumber`|
|`int`|32 bit tamsayı depolanan bir `NSNumber`|
|`uint`|32 bit işaretsiz tamsayıyı depolanan bir `NSNumber`|
|`nint`|`NSInteger` depolanmış bir `NSNumber`|
|`nuint`|`NSUInteger` depolanmış bir `NSNumber`|
|`long`|64 bit tamsayı depolanan bir `NSNumber`|
|`float`|32 bit tamsayı olarak saklanan bir `NSNumber`|
|`double`|64 bit tam sayı olarak saklanan bir `NSNumber`|
|`NSObject` ve sınıfları|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C# `Array` , `NSObject`|`NSArray`|
|C# `Array` numaralandırmalar,|`NSArray` içeren `NSNumber` değerleri|
