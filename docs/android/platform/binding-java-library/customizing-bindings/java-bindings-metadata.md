---
title: Java Bindings Metadata
description: "C# kod Xamarin.Android: Java kitaplıkları Java yerel arabirimi (JNI içinde) belirtilen alt düzey ayrıntıların soyutlar sistemdir bağlamaları aracılığıyla çağırır. Xamarin.Android bu bağlamaların oluşturan bir araç sağlar. Bu araç, ad alanlarını değiştirme ve üyeleri yeniden adlandırma gibi yordamları gerçekleştirmeye olanak tanıyan meta verileri kullanarak bir bağlama nasıl oluşturulduğunu Geliştirici denetimi sağlar. Bu belge meta verileri nasıl çalıştığı açıklanmaktadır, bu meta veri öznitelikleri özetler destekler ve bu meta verileri değiştirerek bağlama sorunları gidermek açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 70f17b6bc8dc991534cdf4dd065c813aa0e27e96
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="java-bindings-metadata"></a>Java Bindings Metadata

_C# kod Xamarin.Android: Java kitaplıkları Java yerel arabirimi (JNI içinde) belirtilen alt düzey ayrıntıların soyutlar sistemdir bağlamaları aracılığıyla çağırır. Xamarin.Android bu bağlamaların oluşturan bir araç sağlar. Bu araç, ad alanlarını değiştirme ve üyeleri yeniden adlandırma gibi yordamları gerçekleştirmeye olanak tanıyan meta verileri kullanarak bir bağlama nasıl oluşturulduğunu Geliştirici denetimi sağlar. Bu belge meta verileri nasıl çalıştığı açıklanmaktadır, bu meta veri öznitelikleri özetler destekler ve bu meta verileri değiştirerek bağlama sorunları gidermek açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Bir Xamarin.Android **Java bağlama kitaplığı** var olan bir Android kitaplık bazen olarak bilinen bir aracı yardımıyla bağlama için gerekli işin çoğunu otomatikleştirmek çalışır _bağlamaları Oluşturucu_. Java kitaplığı bağlanırken Xamarin.Android Java sınıflarını inceleyebilir ve tüm paketler, türleri ve üyeleri listesini oluşturabilir, bağlanacak. Bu API'lerin listesi bulunabilir bir XML dosyasında depolanır  **\{directory}\obj\Release\api.xml proje** için bir **sürüm** yapı ve  **\{proje Directory}\obj\Debug\api.xml** için bir **hata ayıklama** oluşturun.

![Obj/Debug klasörüne api.xml dosyasının konumu](java-bindings-metadata-images/java-bindings-metadata-01.png)

Bağlamaları Oluşturucu kullanacağı **api.xml** gerekli C# sarmalayıcı sınıfları oluşturmak için bir kılavuz olarak dosya. Bu XML dosyasının içeriğini Google çeşidi olan _Android açık kaynaklı proje_ biçimi.
Aşağıdaki kod parçacığında içeriğini örneğidir **api.xml**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

Bu örnekte, **api.xml** bir sınıfta bildirir `android` adlı paket `Manifest` genişleten `java.lang.Object`.

Çoğu durumda, İnsan Yardım daha fazla "gibi .NET" eşitleyerek Java API yapmak veya derlemesini bağlama derleme engelleme sorunları düzeltmek için gereklidir. Örneğin, .NET ad alanları için Java Paket adlarını değiştirmek, bir sınıfı yeniden adlandırın veya bir yöntemin dönüş türünü değiştirmek gerekli olabilir.

Bu değişiklikler değiştirerek elde edilir değil **api.xml** doğrudan.
Bunun yerine, Java bağlama kitaplığı şablon tarafından sağlanan özel XML dosyalarında değişiklikler kaydedilir. Xamarin.Android bağlama derleme derlerken bağlamaları Oluşturucu tarafından bu eşleme dosyaları bağlama derleme oluştururken etkileneceği

Bu XML eşleme dosyaları bulunabilir **dönüştüren** projenin klasörü:

-   **MetaData.xml** &ndash; değişiklik oluşturulan bağlama ad alanı değiştirme gibi son API'ye yapılmasına izin verir. 

-   **EnumFields.xml** &ndash; Java arasında eşleme içeren `int` sabitleri ve C# `enums` . 

-   **EnumMethods.xml** &ndash; sağlayan yöntem parametrelerini ve dönüş türleri Java'dan değiştirme `int` sabitleri C# `enums` . 

**MetaData.xml** dosyasıdır en bu dosyaların alma gibi bağlama için genel amaçlı değişikliğe izin verdiğinden olarak:

-   .NET kurallarına uygun şekilde ad alanları, sınıflar, yöntemleri veya alanları yeniden adlandırılıyor. 

-   Ad alanları, sınıflar, yöntemleri veya artık gerekmeyen alanları kaldırılıyor. 

-   Sınıfları farklı ad alanlarına taşıma. 

-   Bağlamanın bir tasarım oluşturmak üzere ekleme ek destek sınıfları .NET framework desenlerini izleyin. 

Tartışmak taşıma sağlar **Metadata.xml** daha ayrıntılı.


## <a name="metadataxml-transform-file"></a>Metadata.XML dönüşüm dosyası

Biz zaten öğrenilen, dosyanın **Metadata.xml** bağlamaları Oluşturucu tarafından bağlama derleme oluşturulmasını etkilemek için kullanılır.
Meta veri biçimi kullanan [XPath](https://www.w3.org/TR/xpath/) sözdizimi ve hemen hemen aynıdır *GAPI meta veri* açıklanan [GAPI meta veri](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) Kılavuzu. Bu uygulama neredeyse tam bir uygulaması XPath 1.0 olan ve bu nedenle standart 1.0 öğeleri destekler. Bu dosyayı değiştirmek, eklemek, Gizle veya herhangi bir öğe veya öznitelik API dosyasında taşımak için güçlü bir temel XPath mekanizmadır. Tüm meta veri spec kural öğeleri kuralı uygulanacak düğüm tanımlamak için yol özniteliğini içerir. Kuralları aşağıdaki sırayla uygulanır:

* **Düğüm Ekle** &ndash; yol özniteliği tarafından belirtilen düğümün bir alt düğüm ekler.
* **attr** &ndash; bir özniteliği yol özniteliği tarafından belirtilen öğenin değerini ayarlar.
* **Remove-node** &ndash; belirtilen XPath eşleşen düğümleri kaldırır.

Aşağıdaki örneğidir bir **Metadata.xml** dosyası:

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

Aşağıda bazı yaygın olarak kullanılan XPath öğelerinin Java API için listelenmektedir:

-   `interface` &ndash; Java arabirimi bulmak için kullanılır. Örneğin `/interface[@name='AuthListener']`.

-   `class` &ndash; Bir sınıf bulmak için kullanılır. Örneğin `/class[@name='MapView']`.

-   `method` &ndash; Java sınıfta veya arabirimde bir yöntem bulmak için kullanılır. Örneğin `/class[@name='MapView']/method[@name='setTitleSource']`.

-   `parameter` &ndash; Bir yöntem için parametre tanımlayın. Örneğin `/parameter[@name='p0']`



### <a name="adding-types"></a>Türleri ekleme

`add-node` Öğeye yeni bir sarmalayıcı sınıfı eklemek için Xamarin.Android bağlama proje söylemek **api.xml**. Örneğin, aşağıdaki kod parçacığını bir oluşturucu ve tek bir alanı ile bir sınıf oluşturmak için bağlama Oluşturucu yönlendirir:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>Türlerini kaldırma

Java türü yoksay ve bağlama değil Xamarin.Android bağlamaları Oluşturucu istemeniz mümkündür. Bu ekleyerek yapılır bir `remove-node` XML öğesi **metadata.xml** dosyası:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Üyeleri yeniden adlandırma

Üyeleri yeniden adlandırma yapılamıyor doğrudan düzenleyerek **api.xml** Xamarin.Android özgün Java yerel arabirimi (JNI) adlarını gerektirdiğinden dosya. Bu nedenle, `//class/@name` özniteliği olamaz değiştirilebilir; bu durumda, bağlama çalışmaz.

Biz bir türünü yeniden adlandırmak istediğiniz bir durum düşünün `android.Manifest`.
Bunu başarmak için biz doğrudan düzenlemek deneyebilirsiniz **api.xml** ve yeniden adlandırma sınıfı şu şekilde:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Bu aşağıdaki C# kod sarmalayıcı sınıfı için oluşturma bağlamaları Oluşturucu sonuçlanır:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Sarmalayıcı sınıfı adlandırıldı bildirimi `NewName`, özgün Java türü hala durumdayken `Manifest`. Artık herhangi bir yöntem'na erişmek Xamarin.Android bağlama sınıfı için mümkün değildir `android.Manifest`; sarmalayıcı sınıfı mevcut olmayan Java türüne bağlıdır.

Düzgün Sarmalanan bir türü (veya yöntem) yönetilen adını değiştirmek için bunu ayarlamak gerekli olan `managedName` özniteliği bu örnekte gösterildiği gibi:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Yeniden adlandırma `EventArg` sarmalayıcı sınıfları

Ne zaman Xamarin.Android bağlama Oluşturucu tanımlar bir `onXXX` ayarlayıcı yöntemi için bir _dinleyici türü_, C# olay ve `EventArgs` alt oluşturulacak bir .NET desteklemek için Java tabanlı bir dinleyici için API flavoured Desen. Örnek olarak, aşağıdaki Java sınıfı ve yöntemi göz önünde bulundurun:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android öneki bırakma `on` ayarlayıcı yönteminden ve bunun yerine kullanın `2DSignNextManuever` adı için temel olarak `EventArgs` bir alt kümesi. Bir alt kümesi için benzer bir şey adlı:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Bu geçerli bir C# sınıf adı değil. Bu sorunu düzeltmek için bağlama Yazar kullanmalısınız `argsType` özniteliği ve C# için geçerli bir ad sağlayın `EventArgs` bir alt kümesi:
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>Desteklenen öznitelikleri

Aşağıdaki bölümlerde bazı Java API dönüştürme için öznitelikleri açıklanmaktadır.

### <a name="argstype"></a>argsType

Bu öznitelik adı için ayarlayıcı yöntemleri yerleştirilir `EventArg` Java dinleyicileri desteklemek için oluşturulan bir alt. Bu bölümdeki daha ayrıntılı anlatılan [yeniden adlandırma EventArg sarmalayıcı sınıflar](#Renaming_EventArg_Wrapper_Classes) bu kılavuzda daha sonra.

### <a name="eventname"></a>EventName

Bir olay için bir ad belirtir. Boş ise, olay oluşturma engeller.
Bu bölüm başlığında daha ayrıntılı anlatılan [yeniden adlandırma EventArg sarmalayıcı sınıflar](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

Bu paket, sınıf, yöntemi veya parametre adını değiştirmek için kullanılır. Örneğin Java sınıfın adını değiştirmek `MyClass` için `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

Bir XPath ifadesi yöntemi yeniden adlandırmak için sonraki örnek gösterilmektedir `java.lang.object.toString` için `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` bir yöntemin dönüş türünü değiştirmek için kullanılır. Bazı durumlarda bağlamaları Oluşturucu hatalı bir derleme zamanı hatasına neden olacak bir Java yöntemin dönüş türünü Infer. Bu durumda olası bir çözüm yöntemin dönüş türünü değiştirmektir.

Bağlamaları Oluşturucu, örneğin, olduğunu düşündüğünü Java yöntemi `de.neom.neoreadersdk.resolution.compareTo()` döndürmelidir bir `int`, hata iletisinde sonuçları **hata CS0535: ' DE. Neom.Neoreadersdk.Resolution' arabirim üyesini 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' uygulamıyor**. Aşağıdaki kod parçacığında oluşturulan C# yöntemden dönüş türünü değiştirmek nasıl gösteren bir `int` için bir `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Bir yöntemin dönüş türünü değiştirir. Bu dönüş özniteliği (değiştikçe öznitelikleri uyumsuz değişiklikleri JNI imza sonuçlanabilir dönün) değişmez. Aşağıdaki örnekte, dönüş türü `append` yöntemi değiştirildiğinde `SpannableStringBuilder` için `IAppendable` (C# ' ın eşdeğişken dönüş türleri desteklemediğine geri çağırma):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>gizlenmiş

Java kitaplıkları belirsizleştirirseniz araçları bağlama Xamarin.Android oluşturucu ve C# sarmalayıcı sınıfları oluşturmak yeteneğini etkileyebilir. Karıştırılmış sınıfların özellikleri içerir: * sınıf adını içeren bir  **$** , yani **$.class** * sınıf adının tamamen küçük harf karakter yani tehlikeye  **a.class**

Bu kod parçacığında, bir "beklemediğiniz karıştırılmış" C# türü oluşturmak nasıl örneğidir:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Bu öznitelik, yönetilen bir özellik adını değiştirmek için kullanılabilir.

Kullanarak bir özel durum `propertyName` Java sınıfı bir alan için yalnızca bir alıcı yöntemi sahip olduğu durum içerir. Bu durumda bağlama Oluşturucu salt yazılır özellik, .NET önerilmez bir şey oluşturmak istersiniz. Aşağıdaki kod parçacığında ".NET özellikleri ayarlayarak" nasıl kaldırılacağını gösterir `propertyName` boş bir dize için:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Ayarlayıcı ve alıcı yöntemleri hala bağlamaları Oluşturucu tarafından oluşturulacak unutmayın.

### <a name="sender"></a>Gönderen

Bir yöntemin hangi parametresi olmalıdır belirtir `sender` yöntemi bir olaya eşlendiğinde parametresi. Değer şunlardan biri olabilir `true` veya `false`. Örneğin:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>görünürlük

Bu öznitelik, bir sınıf, yöntemi veya özelliği görünürlüğünü değiştirmek için kullanılır. Örneğin, bazı yükseltmek gerekli olabilir bir `protected` Java yöntemi, böylelikle C# sarmalayıcı karşılık gelen `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml ve EnumMethods.xml

Burada özellikleri veya kitaplıkları yöntemlerinin geçirilen durumlarını temsil etmesi için tamsayı sabitleri Android kitaplıkları kullanabilirsiniz durumlar vardır. Çoğu durumda bu tamsayı sabitleri, numaralandırmaları C# bağlamak kullanışlıdır. Bu eşlemeyi kolaylaştırmak için **EnumFields.xml** ve **EnumMethods.xml** bağlama projenizdeki dosyalar. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Enum EnumFields.xml kullanarak tanımlama

**EnumFields.xml** dosyasını içeren Java arasında eşleme `int` sabitleri ve C# `enums`. Aşağıdaki örnekte bir kümesi için oluşturulan bir C# Enum atalım `int` sabitleri: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Burada size Java sınıfı önlemlerin `SKRealReachSettings` adlı bir C# enum tanımlanan ve `SKMeasurementUnit` ad alanında `Skobbler.Ngx.Map.RealReach`. `field` Girişleri Java sabiti adını tanımlar (örnek `UNIT_SECOND`), enum giriş adını (örneğin `Second`) ve her iki varlıklar tarafından temsil edilen tamsayı değeri (örnek `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Alıcı/ayarlayıcı yöntemlerini EnumMethods.xml kullanarak tanımlama

**EnumMethods.xml** dosya sağlayan yöntem parametrelerini ve dönüş türleri Java'dan değiştirme `int` sabitleri C# `enums`. Diğer bir deyişle, okuma ve yazma C# Numaralandırmaların eşlemeleri (tanımlanan **EnumFields.xml** dosyası) Java için `int` sabit `get` ve `set` yöntemleri.

Verilen `SKRealReachSettings` enum aşağıdaki yukarıda tanımlanan **EnumMethods.xml** dosyası bu enum için alıcı/ayarlayıcı tanımlarsınız:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

İlk `method` satır eşlemeleri Java dönüş değerini `getMeasurementUnit` yönteme `SKMeasurementUnit` enum. İkinci `method` satırının ilk parametresi eşlemeleri `setMeasurementUnit` aynı numaralandırmaya.

Tüm yerinde bu değişiklikler, izleme kodu Xamarin.Android ayarlamak için kullanabileceğiniz `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>Özet

Bu makalede ele alınan meta veri Xamarin.Android bir API tanımından dönüştürmek için nasıl kullandığını *Google* *AOSP biçimi*. Olası değişiklikleri kapsayan sonra kullanarak *Metadata.xml*üyeleri adlandırırken karşılaştı sınırlamaları incelenmesi ve her özniteliği ne zaman kullanılmalı açıklayan desteklenen XML öznitelikleri listesi gösterilir.



## <a name="related-links"></a>İlgili bağlantılar

- [JNI ile çalışma](~/android/platform/java-integration/working-with-jni.md)
- [Java Kitaplığını Bağlama](~/android/platform/binding-java-library/index.md)
- [GAPI Metadata](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
