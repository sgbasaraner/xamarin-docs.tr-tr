---
title: Veri bağlama ve anahtar-değer Xamarin.Mac'te kodlama
description: Bu makale, anahtar-değer kodlaması ve anahtar-değer Xcode'un arabirim Oluşturucu UI öğelerine veri bağlama için izin vermek için gözleme kullanarak kapsar.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0adb8cda71ca8803c535679da2aecf00f3fa46a5
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780633"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Veri bağlama ve anahtar-değer Xamarin.Mac'te kodlama

_Bu makale, anahtar-değer kodlaması ve anahtar-değer Xcode'un arabirim Oluşturucu UI öğelerine veri bağlama için izin vermek için gözleme kullanarak kapsar._

## <a name="overview"></a>Genel Bakış

C# ve .NET ile bir Xamarin.Mac uygulamasında çalışırken, aynı anahtar-değer kodlaması ve veri bağlama tekniklerinin erişiminiz, içinde çalışmakta olan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ veri bağlama ile kod yazmak yerine, kullanıcı Arabirimi öğeleri için.

Anahtar-değer kodlaması ve veri teknikleri Xamarin.Mac uygulamanızda bağlama kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız gereken kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma avantajı vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), bakımı kolay, daha esnek bir uygulama için önde gelen Tasarım.

[![Çalışan uygulamaya örneği](databinding-images/intro01.png "çalıştıran uygulama örneği")](databinding-images/intro01-large.png#lightbox)

Bu makalede, şu anahtar-değer kodlaması ve bir Xamarin.Mac uygulamasını veri bağlama ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve UI öğeleri, C# sınıfları kablo kullanılır.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Anahtar-değer kodlaması nedir

Anahtar-değer kodlaması (KVC) olan bir mekanizma örnek değişkenleri erişmesini yerine özellikleri tanımlamak için anahtarları (özel olarak biçimlendirilmiş dizeler) veya erişimci yöntemlerini kullanarak, dolaylı olarak bir nesnenin özelliklerine erişme (`get/set`). Xamarin.Mac uygulamanızda anahtar-değer kodlama uyumlu erişimcilerini uygulama tarafından (KVO gözlemci anahtar-değer), veri bağlama, temel veri, Cocoa bağlamaları ve scriptability gibi diğer macOS (eski adıyla OS X da bilinir) özelliklere erişiminiz olur.

Anahtar-değer kodlaması ve veri teknikleri Xamarin.Mac uygulamanızda bağlama kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız gereken kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma avantajı vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), bakımı kolay, daha esnek bir uygulama için önde gelen Tasarım. 

Örneğin, aşağıdaki sınıf tanımını KVC uyumlu bir nesnenin bakalım:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

İlk olarak, `[Register("PersonModel")]` öznitelik sınıfı kaydeder ve Objective-c gösterir Daha sonra sınıf devralınacak gerekir `NSObject` (veya devralınan bir alt `NSObject`), bu işlem birkaç temel sınıfa KVC uyumlu olmasını sağlayan bir yöntem ekler. Ardından, `[Export("Name")]` özniteliği kullanıma sunan `Name` özellik ve daha sonra KVC ve KVO teknikleri özelliğine erişmek için kullanılacak anahtar değeri tanımlar. 

Son olarak, anahtar-değer gözlemlenen özelliğin değerine değişir hayatımın için erişimci değeriyle değişiklikleri sarmalamanız gerekir `WillChangeValue` ve `DidChangeValue` yöntem çağrıları (aynı anahtarı olarak belirterek `Export` özniteliği).  Örneğin:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Bu adım _çok_ (Bu makalenin sonraki bölümlerinde göreceğiz gibi) arabirim Oluşturucu Xcode veri bağlama için önemlidir.

Daha fazla bilgi için lütfen Apple'nın bakın [anahtar-değer kodlaması Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Anahtarları ve anahtar yolları

A _anahtar_ belirli bir özelliğine bir nesne tanımlayan bir dizedir. Genellikle, bir anahtar bir erişimci metot uyumlu nesne kodlama bir anahtar-değer adına karşılık gelir. Anahtarları kullanması gerekir ASCII kodlaması, genellikle bir küçük harf ile başlamalı ve boşluk içeremez. Bu nedenle yukarıdaki örnekte, verilen `Name` anahtar değerini olacaktır `Name` özelliği `PersonModel` sınıfı. Çoğu durumda, ancak anahtar ve ortaya özelliğin adı aynı olması gerekmez.

A _anahtar yolunu_ olduğu nokta dizisi ayrılmış anahtarları geçirmek için nesne özellikleri hiyerarşisi belirtmek için kullanılır. Dizideki ilk anahtar özelliğini göreli alıcıdır ve sonraki her anahtar, önceki özelliğinin değeri göre değerlendirilir. Aynı şekilde, bir nesne ve özelliklerini bir C# sınıf içinde gezeceği nokta gösterimi kullanırsınız.

Örneğin, genişletilmiş, `PersonModel` sınıfı ve eklenen `Child` özelliği:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Çocuğunuzun adı anahtar yoluna olacaktır `self.Child.Name` ya da yalnızca `Child.Name` (anahtar değer nasıl kullanılıyordu göre).

### <a name="getting-values-using-key-value-coding"></a>Anahtar-değer kodlaması kullanarak değerleri alma

`ValueForKey` Yöntemi, belirtilen anahtar için değeri döndürür (olarak bir `NSString`), istek alma KVC sınıfının örneğini göreli yolu. Örneğin, varsa `Person` örneğidir `PersonModel` yukarıda tanımlanan sınıfı:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

Bu değeri döndürmesi `Name` örneğini özelliği `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Anahtar-değer kodlaması kullanarak değerlerini ayarlama

Benzer şekilde, `SetValueForKey` belirtilen anahtar için değeri ayarlayın (olarak bir `NSString`), istek alma KVC sınıfının örneğini göreli yolu. Bu nedenle bir örneğini kullanarak `PersonModel` aşağıda gösterildiği gibi sınıfı:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Değerini değiştirmeniz gerekir `Name` özelliğini `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Gözlemci değeri değiştiğinde

Gözlemci KVC uyumlu sınıfın belirli bir anahtarın ekleyebilirsiniz (KVO) anahtar-değer gözleme kullanarak ve bu anahtarın değeri (KVC teknikleri veya C# kodunda özelliğe doğrudan erişmek) değiştiren dilediğiniz zaman bildirim. Örneğin:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Şimdi, dilediğiniz zaman `Name` özelliği `Person` örneğini `PersonModel` sınıfı değiştirilmişse, yeni değer konsoluna yazılır. 

Daha fazla bilgi için lütfen Apple'nın bakın [anahtar-değer gözleme programlama kılavuzu için giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Veri bağlama

Aşağıdaki bölümlerde veri okuma ve yazma C# kodunu kullanarak değerleri yerine Xcode'un arabirimi Oluşturucusu'nda kullanıcı Arabirimi öğeleri bağlamak için bir anahtar-değer kodlaması ve anahtar-değer uyumlu sınıfı gözleme nasıl kullanabileceğinizi gösterir. Bu şekilde, ayrı, _veri modeli_ bunları görüntülemek için kullanılan görünümlerinden Xamarin.Mac uygulama daha esnek ve bakımını yapma. Büyük ölçüde yazılması gereken kod miktarını azaltır.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Veri modelinizi tanımlama

Bir arabirim Oluşturucu kullanıcı Arabirimi öğesinde veri bağlamak için önce KVC/KVO uyumlu sınıflar olarak davranmak üzere Xamarin.Mac uygulamanızda tanımlanmış olmalıdır _veri modeli_ bağlama için. Veri modeli, tüm kullanıcı arabiriminde görüntülenir ve herhangi bir değişiklik uygulama çalışırken kullanıcı Arabiriminde kullanıcının yaptığı veri alan verileri sağlar.

Örneğin, çalışan bir grup olarak yönetilen bir uygulama yazıyorsanız, veri modeli tanımlamak için aşağıdaki sınıf kullanabilirsiniz:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

Bu sınıfın özelliklerinin birçoğunu ele alınmıştır [anahtar-değer kodlaması nedir](#What_is_Key-Value_Coding) yukarıdaki bölümde. Ancak, birkaç belirli öğeleri ve bu sınıf için bir veri modeli olarak davranmasına izin vermek için yapılan bazı eklemeler göz atalım **dizisi denetleyicinin** ve **ağaç denetleyicileri** (Bu, daha sonra verileri kullanacağız Bağlama **ağaç görünümlerini**, **anahat görünümleri** ve **koleksiyon görünümlerini**).

İlk olarak, bir çalışan bir yönetici olabileceği için kullandığımız bir `NSArray` (özellikle bir `NSMutableArray` değerler değiştirilebilir şekilde) kendilerine iliştirilmiş yönetilen çalışanların izin vermek için:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Burada dikkat edilecek iki noktalar:

1. Kullandık bir `NSMutableArray` standart bir dizi C# ya da bu veri bağlama için bir gereksinim için olan tüm AppKit denetimleri gibi olduğundan koleksiyonu yerine **tablo görünümleri**, **anahat görünümleri** ve **koleksiyonları** .
2. Biz çalışanlar dizisi atama tarafından kullanıma sunulan bir `NSArray` adı değiştirildi, C# ve bağlama amacıyla veri biçimlendirilmiş `People`, veri bağlama bekliyor, bir `personModelArray` biçiminde **{$class_name} dizi** (Not ilk karakterin küçük harf yapılmadığını).

Ardından, desteklemek için bazı özel ad genel yöntemler eklemek ihtiyacımız **dizisi denetleyicinin** ve **ağaç denetleyicileri**:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Bunlar, istek ve görüntüledikleri verileri değiştirmek denetleyicileri izin verir. İfşa edilen gibi `NSArray` yukarıda bu (Bu, tipik C# adlandırma kurallarından farklı) belirli bir adlandırma kuralı vardır:

- `addObject:` -Bir nesne dizisine ekler.
- `insertObject:in{class_name}ArrayAtIndex:` -Burada `{class_name}` sınıfınıza adıdır. Bu yöntem dizi verilen dizindeki bir nesne ekler.
- `removeObjectFrom{class_name}ArrayAtIndex:` -Burada `{class_name}` sınıfınıza adıdır. Bu yöntem, belirtilen dizindeki dizideki nesnesini kaldırır.
- `set{class_name}Array:` -Burada `{class_name}` sınıfınıza adıdır. Bu yöntem ile yeni bir tane mevcut carry çoğaltmanıza imkan tanır.

Bu yöntemlerin içinde biz dizide değişiklikleri sarmalanmış `WillChangeValue` ve `DidChangeValue` KVO uyumluluk iletileri.

Son olarak, bu yana `Icon` özellik değerini kullanır `isManager` özellik değişiklikleri `isManager` özelliği değil yansıtılır `Icon` veri bağlı kullanıcı Arabirimi öğeleri (sırasında KVO) için:

```csharp
[Export("Icon")]
public NSImage Icon {
    get {
        if (isManager) {
            return NSImage.ImageNamed ("group.png");
        } else {
            return NSImage.ImageNamed ("user.png");
        }
    }
}
``` 

Bu düzeltmek için aşağıdaki kodu kullanırız:

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

Ek olarak kendi anahtarı unutmayın `isManager` erişimci de gönderme `WillChangeValue` ve `DidChangeValue` için iletileri `Icon` değişikliği de görürsünüz için anahtar.

Kullanacağız `PersonModel` bu makalenin geri kalanında veri modeli.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Basit veri bağlama

Veri tanımlanan bizim modeliyle Xcode'un arabirim oluşturucu içinde veri bağlamayı basit bir örneği göz atalım. Örneğin, bir form Xamarin.Mac uygulamamız düzenlemek için kullanılan ekleyelim `PersonModel` , yukarıda tanımladık. Birkaç metin alanları ve onay kutusu görüntülemek ve modelimizi özelliklerini düzenlemek için ekleyeceğiz.

İlk olarak, yeni bir ekleyelim **görünüm denetleyicisi** için sunduğumuz **Main.storyboard** dosya arabirimi Oluşturucu'da ve kendi sınıf adını `SimpleViewController`: 

[![Yeni bir görünüm denetleyicisi ekleme](databinding-images/simple01.png "yeni bir görünüm denetleyicisi ekleme")](databinding-images/simple01-large.png#lightbox)

Ardından, Mac için Visual Studio'ya dönün, düzenleme **SimpleViewController.cs** (otomatik olarak bizim projeye eklendi) dosyası ve bir örneği üzerinden kullanıma sunacaksınız `PersonModel` veri formumuzun bağlama olacaktır. Aşağıdaki kodu ekleyin:

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

Sonraki görünüm yüklendiğinde, şimdi bir örneğini oluşturmak bizim `PersonModel` ve bu kod ile doldurun:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

Formumuzu oluşturmak ihtiyacımız şimdi çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Aşağıdaki gibi aranacak form düzeni:

[![Xcode içinde bir film şeridini düzenleme](databinding-images/simple02.png "Xcode içinde bir film şeridini düzenleme")](databinding-images/simple02-large.png#lightbox)

Forma veri bağlamasına `PersonModel` biz aracılığıyla sunulan `Person` anahtarı, aşağıdakileri yapın:

1. Seçin **çalışan adı** metni alanına ve anahtara **bağlamaları denetçisi**.
2. Denetleme **bağlamak** kutusunda ve seçin **Basit Görünüm denetleyicisi** açılır listeden. Ardından girin `self.Person.Name` için **anahtar yolunu**: 

    [![Anahtar yoluna girdiğinden](databinding-images/simple03.png "anahtar yoluna girme")](databinding-images/simple03-large.png#lightbox)
3. Seçin **meslek** metni alanına ve onay **bağlamak** kutusunda ve seçin **Basit Görünüm denetleyicisi** açılır listeden. Ardından girin `self.Person.Occupation` için **anahtar yolunu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple04.png "anahtar yoluna girme")](databinding-images/simple04-large.png#lightbox)
4. Seçin **çalışan bir yönetici mi** onay kutusunu işaretleyin **bağlamak** kutusunda ve seçin **Basit Görünüm denetleyicisi** açılır listeden. Ardından girin `self.Person.isManager` için **anahtar yolunu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple05.png "anahtar yoluna girme")](databinding-images/simple05-large.png#lightbox)
5. Seçin **çalışanlar, sayı yönetilen** metni alanına ve onay **bağlamak** kutusunda ve seçin **Basit Görünüm denetleyicisi** açılır listeden. Ardından girin `self.Person.NumberOfEmployees` için **anahtar yolunu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple06.png "anahtar yoluna girme")](databinding-images/simple06-large.png#lightbox)
6. Çalışan bir yönetici değil ise, sayı, çalışanların yönetilen etiket ve metin alanı gizlemek istiyoruz.
7. Seçin **çalışanlar, sayı yönetilen** etiketini genişletin **gizli** turndown ve onay **bağlamak** kutusunda ve seçin **Basit Görünüm denetleyicisi** açılır listeden. Ardından girin `self.Person.isManager` için **anahtar yolunu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple07.png "anahtar yoluna girme")](databinding-images/simple07-large.png#lightbox)
8. Seçin `NSNegateBoolean` gelen **değer dönüştürücü** açılır:  

    ![NSNegateBoolean anahtar dönüşümü seçerek](databinding-images/simple08.png "NSNegateBoolean anahtar dönüşümü seçme")
9. Bu etiket, gizlenecek veri bağlama bildirir değerini `isManager` özelliği `false`.
10. Adım 7 ve 8 için yineleyin **çalışanlar, sayı yönetilen** metin alanı.
11. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Uygulama, değerleri çalıştırırsanız `Person` özelliği otomatik olarak formumuzu doldurur:

[![Otomatik olarak doldurulan bir form gösteren](databinding-images/simple09.png "otomatik olarak doldurulan bir form gösteriliyor")](databinding-images/simple09-large.png#lightbox)

Kullanıcıların forma yaptığı tüm değişiklikler geri yazılır `Person` görünüm denetleyicisi özelliği. Örneğin, seçimini **çalışan bir yönetici olan** güncelleştirmeleri `Person` örneğini bizim `PersonModel` ve **çalışanlar, sayı yönetilen** etiket ve metin alanı otomatik olarak (aracılığıyla gizlidir veri bağlama):

[![Yönetici olmayan çalışanların sayısını gizleme](databinding-images/simple10.png "yönetici olmayan çalışanların sayısını gizleme")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Tablo görünümü veri bağlama

Biz dışına veri bağlama temellerini sahip olduğunuza göre daha karmaşık veri bağlama görev kullanarak bakalım bir _dizisi denetleyici_ ve Tablo görünümüne veri bağlama. Tablo görünümleri ile çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [tablo görünümleri](~/mac/user-interface/table-view.md) belgeleri.

İlk olarak, yeni bir ekleyelim **görünüm denetleyicisi** için sunduğumuz **Main.storyboard** dosya arabirimi Oluşturucu'da ve kendi sınıf adını `TableViewController`:

[![Yeni bir görünüm denetleyicisi ekleme](databinding-images/table01.png "yeni bir görünüm denetleyicisi ekleme")](databinding-images/table01-large.png#lightbox)

Ardından, düzenleyelim **TableViewController.cs** (otomatik olarak bizim projeye eklendi) dosyası ve bir dizi kullanıma sunar (`NSArray`), `PersonModel` sınıfları veri formumuzun bağlama olacaktır. Aşağıdaki kodu ekleyin:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Üzerinde yaptığımız gibi `PersonModel` yukarıda sınıfını [veri modelinizi tanımlama](#Defining_your_Data_Model) bölümünde, size özel olarak adlandırılmış dört genel yöntemleri açığa şekilde bizim koleksiyonundan dizisi denetleyici ve okuma ve yazma veri `PersonModels`.

Sonraki görünümü yüklendiğinde, biz bu kodla bizim dizi doldurmanız gerekir:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Bizim Tablo görünümü oluşturmak ihtiyacımız şimdi çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Aşağıdaki gibi görünmesini Tablo düzeni:

[![Yeni bir tablo görünüm düzenleme](databinding-images/table02.png "yeni bir tablo görünüm düzenleme")](databinding-images/table02-large.png#lightbox)

Eklememiz gerekiyor bir **dizisi denetleyici** tablomuza ilişkili verilerini sağlamak için aşağıdakileri yapın:

1. Sürükleme bir **dizi denetleyici** gelen **kitaplığı denetçisi** üzerine **Arayüzü Düzenleyicisi**:  

    ![Bir dizi denetleyicisi Kitaplığı'ndan seçerek](databinding-images/table03.png "kitaplıktan bir dizi denetleyicisi seçme")
2. Seçin **dizisi denetleyici** içinde **arabirimi hiyerarşi** geçin **özniteliği denetçisi**:  

    [![Öznitelikleri Inspector'ı seçerek](databinding-images/table04.png "öznitelikleri Inspector'ı seçme")](databinding-images/table04-large.png#lightbox)
3. Girin `PersonModel` için **sınıf adı**, tıklayın **artı** düğmesini tıklatın ve üç anahtarları ekleyin. Bunları `Name`, `Occupation` ve `isManager`:  

    ![Gerekli anahtar yolu ekleme](databinding-images/table05.png "gerekli anahtar yolu ekleme")
4. Bu dizi denetleyicisi ne bir dizi yönetiyor bildirir ve hangi özellikleri, üzerinden kullanıma sunacaksınız (anahtarlar).
5. Geçiş **bağlamaları denetçisi** altında **içerik dizisi** seçin **bağlamak** ve **Tablo görünümü denetleyicisi**. Girin bir **Model anahtar yolu** , `self.personModelArray`:  

    ![Anahtar yoluna girdiğinden](databinding-images/table06.png "anahtar yolu girme")
6. Bu dizi dizi denetleyiciye bölümlere `PersonModels` size sunduğumuz görünüm denetleyicisi üzerinde gösterilen.

Bizim Tablo görünümü dizi Denetleyicisi'ne bağlamak ihtiyacımız şimdi aşağıdakileri yapın:

1. Tablo görünümü seçin ve **denetçisi bağlama**:  

    [![Bağlama Inspector'ı seçerek](databinding-images/table07.png "bağlama Inspector'ı seçme")](databinding-images/table07-large.png#lightbox)
2. Altında **İçindekiler** turndown, select **bağlamak** ve **dizisi denetleyici**. Girin `arrangedObjects` için **denetleyicisi anahtarı** alan:  

    ![Denetleyici anahtar tanımlama](databinding-images/table08.png "denetleyicisi anahtarını tanımlama")
3. Seçin **Tablo görünümü hücresi** altında **çalışan** sütun. İçinde **bağlamaları denetçisi** altında **değer** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Name` için **Model anahtar yolu**:  

    [![Model anahtar yolu ayarlamayı](databinding-images/table09.png "modeli anahtar yolu ayarlama")](databinding-images/table09-large.png#lightbox)
4. `objectValue` Geçerli `PersonModel` dizi denetleyici tarafından yönetilmekte olan dizi.
5. Seçin **Tablo görünümü hücresi** altında **meslek** sütun. İçinde **bağlamaları denetçisi** altında **değer** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Occupation` için **Model anahtar yolu**:  

    [![Model anahtar yolu ayarlamayı](databinding-images/table10.png "modeli anahtar yolu ayarlama")](databinding-images/table10-large.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Uygulama çalıştırıyoruz, tablonun bizim dizisi ile doldurulur `PersonModels`:

[![Uygulamayı çalıştıran](databinding-images/table11.png "uygulamayı çalıştırma")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Anahat görünüm veri bağlama

veri bağlama anahat görünüme yönelik bağlama Tablo görünümüne karşı çok benzer. Anahtar fark kullanacağız, bir **ağaç denetleyicisi** yerine bir **dizisi denetleyici** anahat görünüme bağlı veri sağlamak için. Anahat görünümleri ile çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [anahat görünümleri](~/mac/user-interface/outline-view.md) belgeleri.

İlk olarak, yeni bir ekleyelim **görünüm denetleyicisi** için sunduğumuz **Main.storyboard** dosya arabirimi Oluşturucu'da ve kendi sınıf adını `OutlineViewController`: 

[![Yeni bir görünüm denetleyicisi ekleme](databinding-images/outline01.png "yeni bir görünüm denetleyicisi ekleme")](databinding-images/outline01-large.png#lightbox)

Ardından, düzenleyelim **OutlineViewController.cs** (otomatik olarak bizim projeye eklendi) dosyası ve bir dizi kullanıma sunar (`NSArray`), `PersonModel` sınıfları veri formumuzun bağlama olacaktır. Aşağıdaki kodu ekleyin:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Üzerinde yaptığımız gibi `PersonModel` yukarıda sınıfını [veri modelinizi tanımlama](#Defining_your_Data_Model) bölümünde, size özel olarak adlandırılmış dört genel yöntemleri açığa şekilde bizim koleksiyonundan ağaç denetleyici ve okuma ve yazma veri `PersonModels`.

Sonraki görünümü yüklendiğinde, biz bu kodla bizim dizi doldurmanız gerekir:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

Bizim anahat görünümü oluşturmak ihtiyacımız şimdi çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Aşağıdaki gibi görünmesini Tablo düzeni:

[![Anahat görünümü oluşturma](databinding-images/outline02.png "anahat görünümü oluşturma")](databinding-images/outline02-large.png#lightbox)

Eklememiz gerekiyor bir **ağaç denetleyicisi** ilişkili anahat bizim verilerini sağlamak için aşağıdakileri yapın:

1. Sürükleme bir **ağaç denetleyicisi** gelen **kitaplığı denetçisi** üzerine **Arayüzü Düzenleyicisi**:  

    ![Bir ağaç denetleyicisi Kitaplığı'ndan seçerek](databinding-images/outline03.png "kitaplıktan bir ağaç denetleyicisi seçme")
2. Seçin **ağaç denetleyicisi** içinde **arabirimi hiyerarşi** geçin **özniteliği denetçisi**:  

    [![Öznitelik Inspector'ı seçerek](databinding-images/outline04.png "özniteliği Inspector'ı seçme")](databinding-images/outline04-large.png#lightbox)
3. Girin `PersonModel` için **sınıf adı**, tıklayın **artı** düğmesini tıklatın ve üç anahtarları ekleyin. Bunları `Name`, `Occupation` ve `isManager`:  

    ![Gerekli anahtar yolu ekleme](databinding-images/outline05.png "gerekli anahtar yolu ekleme")
4. Bu ağaç denetleyicisi ne bir dizi yönetiyor bildirir ve hangi özellikleri, üzerinden kullanıma sunacaksınız (anahtarlar).
5. Altında **ağaç denetleyicisi** bölümünde, girin `personModelArray` için **alt**, girin `NumberOfEmployees` altında **sayısı** girin `isEmployee` altında **Yaprak**:  

    ![Ağaç denetleyicisi anahtar yollarını ayarlama](databinding-images/outline05.png "ağaç denetleyicisi anahtar yollarını ayarlama")
6. Bu, tüm alt düğümleri, kaç tane alt düğümler ve geçerli düğümünün alt düğümleri varsa nerede bulacağını ağaç denetleyicisi bildirir.
7. Geçiş **bağlamaları denetçisi** altında **içerik dizisi** seçin **bağlamak** ve **dosya sahibi**. Girin bir **Model anahtar yolu** , `self.personModelArray`:  

    ![Anahtar yoluna düzenleme](databinding-images/outline06.png "anahtar yolunu düzenleme")
8. Bu dizi ağaç denetleyiciye bölümlere `PersonModels` size sunduğumuz görünüm denetleyicisi üzerinde gösterilen.

Bizim anahat görünümü ağaç Denetleyicisi'ne bağlamak ihtiyacımız şimdi aşağıdakileri yapın:

1. Anahat görünümünü seçin ve **bağlama denetçisi** seçin:  

    [![Bağlama Inspector'ı seçerek](databinding-images/outline07.png "bağlama Inspector'ı seçme")](databinding-images/outline07-large.png#lightbox)
2. Altında **anahat içeriği görüntüle** turndown, select **bağlamak** ve **ağaç denetleyicisi**. Girin `arrangedObjects` için **denetleyicisi anahtarı** alan:  

    ![Denetleyici anahtarı ayarı](databinding-images/outline08.png "denetleyicisi tuş ayarlama")
3. Seçin **Tablo görünümü hücresi** altında **çalışan** sütun. İçinde **bağlamaları denetçisi** altında **değer** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Name` için **Model anahtar yolu**:  

    [![Model anahtar yoluna girdiğinden](databinding-images/outline09.png "modeli anahtar yoluna girme")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` Geçerli `PersonModel` ağaç denetleyicisi tarafından yönetilen dizi.
5. Seçin **Tablo görünümü hücresi** altında **meslek** sütun. İçinde **bağlamaları denetçisi** altında **değer** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Occupation` için **Model anahtar yolu**:  

    [![Model anahtar yoluna girdiğinden](databinding-images/outline10.png "modeli anahtar yoluna girme")](databinding-images/outline10-large.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Ana hat uygulama çalıştırıyoruz, bizim dizisi ile doldurulur `PersonModels`:

[![Uygulamayı çalıştıran](databinding-images/outline11.png "uygulamayı çalıştırma")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Koleksiyon görünümü veri bağlama

Bir dizi denetleyicisi için koleksiyon verilerini sağlamak için kullanılan bir koleksiyon görünümü ile veri bağlaması olan bir tablo görünümü, bağlama gibi çok benzer. Koleksiyon görünümü önceden oluşturulmuş görüntü biçimi sahip olmadığından, daha fazla iş kullanıcı etkileşimi geri bildirim sağlamak ve kullanıcı seçimine izlemek için gereklidir.

> [!IMPORTANT]
> Xcode 7 ve macOS 10.11 (ve büyük) bir sorun nedeniyle, bir görsel Taslak (.storyboard) dosyalarının içinde kullanılacak koleksiyon görünümlerini belirleyemiyoruz. Sonuç olarak, Xamarin.Mac uygulamalarınız için koleksiyon görünümlerini tanımlamak için .xib dosyaları kullanmaya devam etmek gerekir. Lütfen bkz. bizim [koleksiyon görünümlerini](~/mac/user-interface/collection-view.md) daha fazla bilgi için belgelere bakın.

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`: 

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>Yerel kilitlenme hata ayıklama

Veri bağlamalarınızı hata yapma sonuçlanabilir bir _yerel kilitlenme_ yönetilmeyen kod ve Xamarin.Mac uygulamanızın ile tamamen başarısız olmasına neden bir `SIGABRT` hata:

[![Yerel kilitlenme iletişim kutusu örneği](databinding-images/debug01.png "yerel kilitlenme iletişim kutusu örneği")](databinding-images/debug01-large.png#lightbox)

Genellikle dört ana nedeni vardır yerel kilitlenme veri bağlama sırasında:

1. Veri modelinizi öğesinden devralmayan `NSObject` veya öğesinin `NSObject`.
2. Objective-C kullanarak, bir özelliği kullanıma sunmadı `[Export("key-name")]` özniteliği.
3. Değişiklikleri erişimcisi'nın değeri kaydırılacak `WillChangeValue` ve `DidChangeValue` yöntem çağrıları (aynı anahtarı olarak belirterek `Export` özniteliği).
4. Hatalı veya yanlış yazılan bir anahtarı olan **bağlama denetçisi** arabirimi Oluşturucu.

### <a name="decoding-a-crash"></a>Bir kilitlenme kodunu çözme

Şimdi bulmak ve düzeltmek nasıl göstereceğiz için yerel bir kilitlenme bizim veri bağlama neden. Arabirimi Oluşturucu'da koleksiyon görünümü örnekte ilk etiketin bizim bağlama değiştirelim `Name` için `Title`:

[![Bağlama anahtarı düzenleme](databinding-images/debug02.png "bağlama anahtarı düzenleme")](databinding-images/debug02-large.png#lightbox)

Şimdi değişikliği kaydetmek, Xcode ile eşitleyin ve uygulamamızı çalıştırmak Mac için Visual Studio için dönün. Koleksiyon görünümü görüntülendiğinde, uygulamayı ile kısa bir süre içinde kilitlenir bir `SIGABRT` hata (gösterildiği gibi **uygulama çıktısı** Mac için Visual Studio'da) bu yana `PersonModel` anahtarla bir özelliği kullanıma sunmuyor `Title`:

[![Bağlama hatası örneği](databinding-images/debug03.png "bağlama hatası örneği")](databinding-images/debug03-large.png#lightbox)

Hata çok üstüne kaydırırsanız **uygulama çıktısı** sorunu çözme anahtarını görebiliriz:

[![Hata günlüğünde sorunu bulma](databinding-images/debug04.png "hata günlüğünde sorunu bulma")](databinding-images/debug04-large.png#lightbox)

Bu satırı bize, söylüyor anahtar `Title` bağlıyoruz nesne mevcut değil. Bağlama yeniden değiştirirsek `Name` arabirim Oluşturucu, kaydetme, eşitleme, yeniden derleyin ve çalıştırın, uygulama sorun beklendiği gibi çalışır.

## <a name="summary"></a>Özet

Bu makalede ayrıntılı veri bağlama ve bir Xamarin.Mac uygulamasında anahtar-değer kodlaması ile çalışmak üzere göz duruma getirdi. İlk olarak, C# sınıfı Objective-c (KVC) anahtar-değer kodlaması kullanarak ve anahtar-değer (KVO) gözleme gösterme görünüyordu. Ardından, KVO uyumlu sınıfının nasıl kullanılacağını gösterdi ve veri Xcode'un arabirim Oluşturucu UI öğelerine bağlar. Son olarak, karmaşık veri bağlama kullanarak gösterdi **dizisi denetleyicinin** ve **ağaç denetleyicileri**.


## <a name="related-links"></a>İlgili bağlantılar

- [MacDatabinding film şeridi (örnek)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (örnek)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Standart denetimler](~/mac/user-interface/standard-controls.md)
- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Anahat görünümleri](~/mac/user-interface/outline-view.md)
- [Koleksiyon görünümleri](~/mac/user-interface/collection-view.md)
- [Anahtar-değer kodlaması Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Anahtar-değer gözleme programlama kılavuzu için giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Cocoa bağlamaları programlama konuları giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa bağlamaları başvuru giriş](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
