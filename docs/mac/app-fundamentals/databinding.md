---
title: Veri bağlama ve anahtar-değer kodlama
description: Bu makalede, anahtar-değer kodlama ve anahtar-değer veri bağlama Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğelerine izin vermek üzere Gözlemleme kullanmayı ele alır.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 48ee5d4e4a0a53de49fbba46d79424e03af6fe5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="data-binding-and-key-value-coding"></a>Veri bağlama ve anahtar-değer kodlama

_Bu makalede, anahtar-değer kodlama ve anahtar-değer veri bağlama Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğelerine izin vermek üzere Gözlemleme kullanmayı ele alır._

## <a name="overview"></a>Genel Bakış

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı anahtar-değer kodlama ve veri bağlama tekniklerinin erişiminiz, çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ veri bağlama ile kod yazmak yerine kullanıcı Arabirimi öğeleri için.

Anahtar-değer kodlama ve veri Xamarin.Mac uygulamanızda teknikleri bağlama kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız için sahip kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma faydası vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), başında bakımı kolay, daha esnek uygulama için Tasarım.

[![Çalışan uygulama örneği](databinding-images/intro01.png "çalışan uygulama örneği")](databinding-images/intro01-large.png#lightbox)

Bu makalede, biz anahtar-değer kodlama ve Xamarin.Mac uygulamasında veri bağlama ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve kullanıcı Arabirimi öğeleri, C# sınıfları wire için kullanılır.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Anahtar-değer kodlama nedir

Anahtar-değer (KVC) kodlama mekanizmasıdır dolaylı bir nesnenin özelliklerine erişme, örnek değişkenleri erişim yerine özelliklerini tanımlamak için (özel olarak biçimlendirilmiş dizeler) anahtarları veya erişimci yöntemlerini kullanarak bir (`get/set`). Anahtar-değer kodlama uyumlu erişimcilerini Xamarin.Mac uygulamanızda uygulayarak anahtar-değer gözlemci (KVO), veri bağlama, çekirdek verileri, Cocoa bağlamalar ve scriptability gibi diğer (önceki adıyla OS X olarak bilinir) macOS özelliklerine erişim sahibi olursunuz.

Anahtar-değer kodlama ve veri Xamarin.Mac uygulamanızda teknikleri bağlama kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız için sahip kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma faydası vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), başında bakımı kolay, daha esnek uygulama için Tasarım. 

Örneğin, aşağıdaki sınıf tanımını KVC uyumlu nesnesinin bakalım:

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

İlk olarak, `[Register("PersonModel")]` öznitelik sınıfı kaydeder ve hedefi C'ye gösterir Daha sonra devralmak sınıf gereken `NSObject` (veya devralan bir alt sınıfı `NSObject`), bu birkaç temel KVC uyumlu olması için sınıfa izin yöntemi ekler. İleri `[Export("Name")]` özniteliğini düzenlemenizi sağlayan `Name` özellik ve daha sonra KVC ve KVO teknikleri bir özelliğe erişmek için kullanılacak anahtar değeri tanımlar. 

Son olarak, anahtar-değer gözlenen özelliğin değerini değiştirir olması yapabilmek için erişimci değeriyle yapılan değişiklikler sarmalamanız gerekir `WillChangeValue` ve `DidChangeValue` yöntem çağrıları (aynı anahtara belirtme `Export` özniteliği).  Örneğin:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Bu adım _çok_ (Bu makalenin sonraki bölümlerinde göreceğiz gibi) arabirimi Oluşturucu xcode'da veri bağlama için önemlidir.

Daha fazla bilgi için lütfen Apple'nın bkz [anahtar-değer kodlama Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Anahtarları ve anahtar yolları

A _anahtar_ bir nesnenin belirli bir özellik tanımlayan bir dize değil. Genellikle, bir anahtar uyumlu nesne kodlama bir anahtar-değer erişimcisi yönteminde adına karşılık gelir. Anahtarları, ASCII kodlama, kullanmalıdır genellikle bir küçük harf ile başlamalı ve boşluk içermemelidir. Bu nedenle yukarıdaki örnekte verilen `Name` bir anahtar değeri olacaktır `Name` özelliği `PersonModel` sınıfı. Ancak, çoğu durumda olduklarını anahtar ve bunlar ortaya özelliğinin adı aynı olması gerekmez.

A _anahtar yolu_ olan nokta dizesi ayrılmış anahtarları gezinmesine nesne özellikleri hiyerarşisini belirtmek için kullanılır. Sıradaki ilk tuşu özelliği göre alıcıdır ve sonraki her anahtar önceki özellik değeri göre değerlendirilir. Aynı şekilde bir nesne ve C# sınıfındaki özellikleri gezinmesine noktalı gösterim kullanın.

Örneğin, genişlettiyseniz `PersonModel` sınıfı ve eklenen `Child` özelliği:

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

Çocuğunuzun adına anahtar yol `self.Child.Name` ya da yalnızca `Child.Name` (anahtar değer nasıl kullanılıyordu göre).

### <a name="getting-values-using-key-value-coding"></a>Anahtar-değer kodlama kullanılarak değerleri alma

`ValueForKey` Yöntemi, belirtilen anahtar için değeri döndürür (olarak bir `NSString`), göreli isteği alma KVC sınıfı örneği. Örneğin, varsa `Person` örneği `PersonModel` yukarıda tanımlanan sınıfı:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

Bu değeri döndürmesi `Name` özelliği'nın bu örneği için `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Anahtar-değer kodlama kullanarak değerleri ayarlama

Benzer şekilde, `SetValueForKey` belirtilen anahtar için değer ayarlayın (olarak bir `NSString`), göreli isteği alma KVC sınıfı örneği. Bu nedenle bir örneği kullanılarak `PersonModel` aşağıda gösterildiği gibi sınıfı:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Değerini değişeceğinden `Name` özelliğine `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Gözlemci değer değişiklikleri

Bir gözlemci KVC uyumlu sınıfın belirli bir anahtarın ekleyebilirsiniz (KVO) anahtar-değer Gözlemleme kullanarak ve bu anahtar değeri (KVC teknikleri veya özelliğe C# kodunda doğrudan erişimini) değiştiren dilediğiniz zaman bildirim. Örneğin:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Şimdi, dilediğiniz zaman `Name` özelliği `Person` örneğini `PersonModel` sınıfı değişiklik, yeni değer konsoluna yazılır. 

Daha fazla bilgi için lütfen Apple'nın bkz [anahtar-değer Gözlemleme programlama kılavuzu giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Veri bağlama

Aşağıdaki bölümlerde, kullanıcı Arabirimi öğeleri okuma ve yazma C# kodu kullanarak değerleri yerine Xcode'nın arabirimi Oluşturucu veri bağlamak için bir anahtar-değer kodlama ve anahtar-değer uyumlu sınıfı Gözlemleme nasıl kullanabileceğinizi gösterir. Bu şekilde, ayrı, _veri modeli_ bunları görüntülemek için kullanılan görünümlerinden Xamarin.Mac uygulama daha esnek ve bakımını yapma. Ayrıca önemli ölçüde yazılması gereken kod miktarını azaltın.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Veri modelinizi tanımlama

Bir kullanıcı Arabirimi öğesi arabirimi Oluşturucu veri bağlamak için önce olarak davranacak şekilde Xamarin.Mac uygulamanızda tanımlanmış bir KVC/KVO uyumlu bir sınıf olmalıdır _veri modeli_ bağlama için. Veri modeli, tüm kullanıcı arabiriminde görüntülenir ve kullanıcı Arabiriminde uygulaması çalıştırılırken yapar verilere herhangi bir değişiklik alan verileri sağlar.

Örneğin, bir çalışan grubu yönetilen bir uygulama yazıyorsanız, veri modeli tanımlamak için aşağıdaki sınıf kullanabilirsiniz:

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

Bu sınıfın özelliklerinin çoğu kapsanan [anahtar-değer kodlama nedir](#What_is_Key-Value_Coding) yukarıdaki bölümde. Ancak, bazı belirli öğeleri ve bu sınıf için bir veri modeli olarak davranmasına izin vermek için yapılan bazı ilaveler bakalım **dizi denetleyicileri** ve **ağaç denetleyicileri** (hangi biz daha sonra verileri kullanmaya başlayacağınız Bağlama **ağaç görünümleri**, **anahat görünümleri** ve **koleksiyon görünümlerini**).

İlk olarak, bir çalışan bir yöneticisi olabileceğinden, kullandığımız bir `NSArray` (özellikle bir `NSMutableArray` değerler değiştirilebilir şekilde) kendisine bağlı yönetilen çalışanların izin vermek için:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Burada dikkat edilecek noktalar iki:

1. Kullandık bir `NSMutableArray` standart bir C# dizisi ya da bu AppKit denetimlere veri bağlama için bir gereksinim gibi olduğundan koleksiyonu yerine **tablosu görünümleri**, **anahat görünümleri** ve **koleksiyonları** .
2. Biz dizi çalışanların atama tarafından kullanıma sunulan bir `NSArray` adı, bağlama amacıyla ve kendi C# değiştirilen verileri biçimlendirilmiş için `People`, veri bağlama bekler, bir `personModelArray` biçiminde **{class_name} dizi** (Not ilk karakteri küçük yapılmadığını).

Ardından, desteklemek için bazı özel ad ortak yöntemleri eklemek ihtiyacımız **dizi denetleyicileri** ve **ağaç denetleyicileri**:

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

Bunlar, istek ve görüntüledikleri verileri değiştirmek denetleyicileri izin verir. Gösterilen gibi `NSArray` yukarıdaki bu (Bu, tipik C# adlandırma kurallarından farklı) belirli bir adlandırma kuralı vardır:

- `addObject:` -Bir nesne dizisine ekler.
- `insertObject:in{class_name}ArrayAtIndex:` -Burada `{class_name}` sınıfınız adıdır. Bu yöntem bir nesne belirli bir dizine dizisi ekler.
- `removeObjectFrom{class_name}ArrayAtIndex:` -Burada `{class_name}` sınıfınız adıdır. Bu yöntem, belirli bir dizine dizide nesneyi kaldırır.
- `set{class_name}Array:` -Burada `{class_name}` sınıfınız adıdır. Bu yöntem ile yeni bir tane var olan taşıyan değiştirmenizi sağlar.

Bu yöntemler içinde biz dizide değişiklikler Sarmalanan `WillChangeValue` ve `DidChangeValue` KVO uyumluluk iletileri.

Son olarak, bu yana `Icon` özellik değerine göre dayanır `isManager` özelliği, değişiklikleri `isManager` özellik değil yansıtılır `Icon` veri ilişkili kullanıcı Arabirimi öğeleri (sırasında KVO) için:

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

Düzeltmek için aşağıdaki kodu kullanırız:

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

Kendi anahtarını yanı sıra unutmayın `isManager` erişen ayrıca gönderen `WillChangeValue` ve `DidChangeValue` için iletileri `Icon` değişiklik de görürsünüz şekilde anahtar.

Biz kullanmaya başlayacağınız `PersonModel` bu makalenin kalanında veri modeli.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Basit veri bağlama

Veri tanımlanan Modelimizi ile basit bir örnek veri bağlamanın Xcode'nın arabirimi Oluşturucu bakalım. Örneğin, bir form Xamarin.Mac uygulamamız düzenlemek için kullanılan ekleyelim `PersonModel` biz yukarıda tanımlanan. Birkaç metin alanları ve onay kutusu görüntülemek ve modelimizi özelliklerini düzenlemek için ekleyeceğiz.

İlk olarak, yeni bir ekleyelim **View Controller** için bizim **Main.storyboard** arabirimi Oluşturucusu'nda dosya ve alt sınıf adını `SimpleViewController`: 

[![Yeni bir görünüm denetleyicisi ekleme](databinding-images/simple01.png "yeni bir görünüm denetleyicisi ekleme")](databinding-images/simple01-large.png#lightbox)

Ardından, Mac için Visual Studio'ya geri dönün, düzenleme **SimpleViewController.cs** (otomatik olarak bizim projesine eklendi) dosyası ve bir örneğini kullanıma `PersonModel` veri formumuzun bağlama olacaktır. Aşağıdaki kodu ekleyin:

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

Görünüm yüklendiğinde, sonraki örneği oluşturalım bizim `PersonModel` ve bu kodu ile doldurun:

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

Bizim form oluşturmak ihtiyacımız artık çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Düzen formun aşağıdakine benzer aramak için:

[![Xcode'da film şeridi düzenleme](databinding-images/simple02.png "xcode'da film şeridi düzenleme")](databinding-images/simple02-large.png#lightbox)

Forma verilere bağlama `PersonModel` biz aracılığıyla sunulan `Person` anahtarı, aşağıdakileri yapın:

1. Seçin **çalışan adı** metin alanı ve anahtara **bağlamaları denetçisi**.
2. Denetleme **bağlamak** kutusunda ve seçin **basit View Controller** gelen açılır. Sonraki girin `self.Person.Name` için **anahtar yolu**: 

    [![Anahtar yoluna girdiğinden](databinding-images/simple03.png "anahtar yoluna girme")](databinding-images/simple03-large.png#lightbox)
3. Seçin **Mesleği** metin alanı ve onay **bağlamak** kutusunda ve seçin **basit View Controller** gelen açılır. Sonraki girin `self.Person.Occupation` için **anahtar yolu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple04.png "anahtar yoluna girme")](databinding-images/simple04-large.png#lightbox)
4. Seçin **çalışan bir yöneticisi olan** onay kutusunu ve denetleyin **bağlamak** kutusunda ve seçin **basit View Controller** gelen açılır. Sonraki girin `self.Person.isManager` için **anahtar yolu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple05.png "anahtar yoluna girme")](databinding-images/simple05-large.png#lightbox)
5. Seçin **numarası, çalışanların yönetilen** metin alanı ve onay **bağlamak** kutusunda ve seçin **basit View Controller** gelen açılır. Sonraki girin `self.Person.NumberOfEmployees` için **anahtar yolu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple06.png "anahtar yoluna girme")](databinding-images/simple06-large.png#lightbox)
6. Çalışan bir yönetici değilse, sayı, çalışanların yönetilen etiket ve metin alanı gizleyin istiyoruz.
7. Seçin **numarası, çalışanların yönetilen** etiketi genişletin **gizli** turndown ve onay **bağlamak** kutusunda ve seçin **basit View Controller** gelen açılır. Sonraki girin `self.Person.isManager` için **anahtar yolu**:  

    [![Anahtar yoluna girdiğinden](databinding-images/simple07.png "anahtar yoluna girme")](databinding-images/simple07-large.png#lightbox)
8. Seçin `NSNegateBoolean` gelen **değeri Transformer** açılır:  

    ![NSNegateBoolean anahtar dönüşümü seçme](databinding-images/simple08.png "NSNegateBoolean anahtar dönüşümü seçme")
9. Etiket, gizlenmiş olabilir, bu veri bağlama söyler değerini `isManager` özelliği `false`.
10. Adım 7 ve 8 için yineleyin **numarası, çalışanların yönetilen** metin alanı.
11. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Uygulama, değerleri çalıştırırsanız `Person` özelliği otomatik olarak bizim formu doldurun:

[![Bir otomatik olarak doldurulmuş formu gösteren](databinding-images/simple09.png "otomatik olarak doldurulan bir form gösterme")](databinding-images/simple09-large.png#lightbox)

Kullanıcıların forma yaptığı tüm değişiklikler geri yazılır `Person` görünüm denetleyicisini özelliği. Örneğin, unselecting **çalışan bir yöneticisi olan** güncelleştirmeleri `Person` örneğini bizim `PersonModel` ve **numarası, çalışanların yönetilen** etiket ve metin alanı aracılığıyla otomatik olarak (gizlidir veri bağlama):

[![Yöneticileri olmayan çalışanların sayısını gizleme](databinding-images/simple10.png "Yöneticileri olmayan çalışanların sayısını gizleme")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Tablo görünüm veri bağlama

Biz göz önünden veri bağlamanın temelleri sahip olduğunuza göre daha karmaşık veri bağlama görev kullanarak bakalım bir _dizi denetleyicisi_ ve bir tablo görünümü veri bağlama. Tablo görünümler ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [tablosu görünümleri](~/mac/user-interface/table-view.md) belgeleri.

İlk olarak, yeni bir ekleyelim **View Controller** için bizim **Main.storyboard** arabirimi Oluşturucusu'nda dosya ve alt sınıf adını `TableViewController`:

[![Yeni bir görünüm denetleyicisi ekleme](databinding-images/table01.png "yeni bir görünüm denetleyicisi ekleme")](databinding-images/table01-large.png#lightbox)

Ardından, düzenleyelim **TableViewController.cs** (otomatik olarak bizim projesine eklendi) dosyası ve bir dizi sunmaya (`NSArray`), `PersonModel` sınıfları veri formumuzun bağlama olacaktır. Aşağıdaki kodu ekleyin:

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

Yalnızca üzerinde yaptığımız gibi `PersonModel` yukarıda sınıfını [veri modelinizi tanımlama](#Defining_your_Data_Model) bölümünde, dört özel olarak adlandırılmış genel yöntemler sunulan böylece bizim koleksiyonundan verilerini dizisi denetleyici ve okuma ve yazma `PersonModels`.

Görünüm yüklendiğinde, sonra bu kodu bizim dizisiyle doldurmak ihtiyacımız var:

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

Bizim Tablo görünümü oluşturmak ihtiyacımız artık çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Düzen tablonun aşağıdakine benzer aramak için:

[![Yeni bir tablo görünümü yerleştirmede](databinding-images/table02.png "yeni bir tablo görünümü düzenleme")](databinding-images/table02-large.png#lightbox)

Eklemek ihtiyacımız bir **dizi denetleyicisi** bizim tabloya bağlı veri sağlamak için aşağıdakileri yapın:

1. Sürükleme bir **dizi denetleyicisi** gelen **kitaplığı denetçisi** üzerine **Arayüzü Düzenleyicisi**:  

    ![Kitaplıktan bir dizi denetleyicisi seçme](databinding-images/table03.png "kitaplıktan bir dizi denetleyicisi seçme")
2. Seçin **dizi denetleyicisi** içinde **arabirimi hiyerarşi** ve geçiş **özniteliği denetçisi**:  

    [![Öznitelikleri denetçisi seçme](databinding-images/table04.png "öznitelikleri denetçisi seçme")](databinding-images/table04-large.png#lightbox)
3. Girin `PersonModel` için **sınıf adı**, tıklatın **artı** düğmesini tıklatın ve üç anahtarları ekleyin. Bunları `Name`, `Occupation` ve `isManager`:  

    ![Gerekli anahtar yollar ekleme](databinding-images/table05.png "gerekli anahtar yollar ekleme")
4. Bu ne bir dizi yönetiyor dizi denetleyicisi bildirir ve hangi özelliklerini bu ortaya (anahtarlar).
5. Geçiş **bağlamaları denetçisi** ve altında **içerik dizi** seçin **bağlamak** ve **tablo View Controller**. Girin bir **Model anahtar yolu** , `self.personModelArray`:  

    ![Anahtar yoluna girdiğinden](databinding-images/table06.png "anahtar yolu girme")
6. Bu dizi dizi denetleyiciye bağlar `PersonModels` biz bizim görünüm denetleyicisinde sunulan.

Bizim Tablo görünümü dizi denetleyiciyi bağlamak için ihtiyacımız artık, aşağıdakileri yapın:

1. Tablo görünümü seçin ve **denetçisi bağlama**:  

    [![Bağlama denetçisi seçme](databinding-images/table07.png "bağlama denetçisi seçme")](databinding-images/table07-large.png#lightbox)
2. Altında **İçindekiler** turndown, select **bağlamak** ve **dizi denetleyicisi**. Girin `arrangedObjects` için **denetleyicisi anahtar** alan:  

    ![Denetleyici anahtar tanımlama](databinding-images/table08.png "denetleyicisi anahtarını tanımlama")
3. Seçin **Tablo görünümü hücresi** altında **çalışan** sütun. İçinde **bağlamaları denetçisi** altında **değeri** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Name` için **Model anahtar yolu**:  

    [![Model anahtar yolu ayarlanıyor](databinding-images/table09.png "modeli anahtar yolu ayarlanıyor")](databinding-images/table09-large.png#lightbox)
4. `objectValue` Geçerli `PersonModel` dizi denetleyicisi tarafından yönetilen dizisindeki.
5. Seçin **Tablo görünümü hücresi** altında **Mesleği** sütun. İçinde **bağlamaları denetçisi** altında **değeri** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Occupation` için **Model anahtar yolu**:  

    [![Model anahtar yolu ayarlanıyor](databinding-images/table10.png "modeli anahtar yolu ayarlanıyor")](databinding-images/table10-large.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Biz uygulama çalıştırırsanız, tablonun bizim dizisi ile doldurulur `PersonModels`:

[![Uygulamayı çalıştıran](databinding-images/table11.png "uygulamayı çalıştırma")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Anahat görünüm veri bağlama

veri bağlama anahat görünümü karşı bir tablo görünümü karşı bağlama çok benzer. Anahtar farktır biz kullanmaya başlayacağınız, bir **ağaç denetleyicisi** yerine bir **dizi denetleyicisi** anahat görünümüne bağlı veri sağlamak için. Anahat görünümler ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [anahat görünümleri](~/mac/user-interface/outline-view.md) belgeleri.

İlk olarak, yeni bir ekleyelim **View Controller** için bizim **Main.storyboard** arabirimi Oluşturucusu'nda dosya ve alt sınıf adını `OutlineViewController`: 

[![Yeni bir görünüm denetleyicisi ekleme](databinding-images/outline01.png "yeni bir görünüm denetleyicisi ekleme")](databinding-images/outline01-large.png#lightbox)

Ardından, düzenleyelim **OutlineViewController.cs** (otomatik olarak bizim projesine eklendi) dosyası ve bir dizi sunmaya (`NSArray`), `PersonModel` sınıfları veri formumuzun bağlama olacaktır. Aşağıdaki kodu ekleyin:

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

Yalnızca üzerinde yaptığımız gibi `PersonModel` yukarıda sınıfını [veri modelinizi tanımlama](#Defining_your_Data_Model) bölümünde, dört özel olarak adlandırılmış genel yöntemler sunulan böylece bizim koleksiyonundan verilerini ağaç denetleyicisi ve okuma ve yazma `PersonModels`.

Görünüm yüklendiğinde, sonra bu kodu bizim dizisiyle doldurmak ihtiyacımız var:

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

Bizim anahat görünümü oluşturmak ihtiyacımız artık çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Düzen tablonun aşağıdakine benzer aramak için:

[![Anahat görünümü oluşturma](databinding-images/outline02.png "özeti görünümü oluşturma")](databinding-images/outline02-large.png#lightbox)

Eklemek ihtiyacımız bir **ağaç denetleyicisi** bizim anahat ilişkili veri sağlamak için şunları yapın:

1. Sürükleme bir **ağaç denetleyicisi** gelen **kitaplığı denetçisi** üzerine **Arayüzü Düzenleyicisi**:  

    ![Kitaplıktan bir ağaç denetleyicisi seçme](databinding-images/outline03.png "kitaplıktan bir ağaç denetleyicisi seçme")
2. Seçin **ağaç denetleyicisi** içinde **arabirimi hiyerarşi** ve geçiş **özniteliği denetçisi**:  

    [![Öznitelik denetçisi seçme](databinding-images/outline04.png "özniteliği denetçisi seçme")](databinding-images/outline04-large.png#lightbox)
3. Girin `PersonModel` için **sınıf adı**, tıklatın **artı** düğmesini tıklatın ve üç anahtarları ekleyin. Bunları `Name`, `Occupation` ve `isManager`:  

    ![Gerekli anahtar yollar ekleme](databinding-images/outline05.png "gerekli anahtar yollar ekleme")
4. Bu ne bir dizi yönetiyor ağaç denetleyicisi bildirir ve hangi özelliklerini bu ortaya (anahtarlar).
5. Altında **ağaç denetleyicisi** bölümünde, girin `personModelArray` için **alt**, girin `NumberOfEmployees` altında **sayısı** ve girin `isEmployee` altında **Yaprak**:  

    ![Ağaç denetleyicisi anahtar yollarını ayarlama](databinding-images/outline05.png "ağaç denetleyicisi anahtar yollarını ayarlama")
6. Bu, tüm alt düğümleri, kaç tane alt düğümler ve geçerli düğüm alt düğümleri olup olmadığını bulmak nereye ağaç denetleyicisi bildirir.
7. Geçiş **bağlamaları denetçisi** ve altında **içerik dizi** seçin **bağlamak** ve **dosyanın sahibi**. Girin bir **Model anahtar yolu** , `self.personModelArray`:  

    ![Anahtar yoluna düzenleme](databinding-images/outline06.png "anahtar yolunu düzenleme")
8. Bu dizi ağaç denetleyiciye bağlar `PersonModels` biz bizim görünüm denetleyicisinde sunulan.

Bizim anahat görünümü ağaç denetleyiciyi bağlamak için ihtiyacımız artık, aşağıdakileri yapın:

1. Anahat görünümü seçin ve **bağlama denetçisi** seçin:  

    [![Bağlama denetçisi seçme](databinding-images/outline07.png "bağlama denetçisi seçme")](databinding-images/outline07-large.png#lightbox)
2. Altında **anahat içeriği görüntüle** turndown, select **bağlamak** ve **ağaç denetleyicisi**. Girin `arrangedObjects` için **denetleyicisi anahtar** alan:  

    ![Denetleyici anahtarı ayarı](databinding-images/outline08.png "denetleyicisi tuş ayarlama")
3. Seçin **Tablo görünümü hücresi** altında **çalışan** sütun. İçinde **bağlamaları denetçisi** altında **değeri** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Name` için **Model anahtar yolu**:  

    [![Model anahtar yoluna girdiğinden](databinding-images/outline09.png "modeli anahtar yolu girme")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` Geçerli `PersonModel` ağaç denetleyicisi tarafından yönetilen dizisindeki.
5. Seçin **Tablo görünümü hücresi** altında **Mesleği** sütun. İçinde **bağlamaları denetçisi** altında **değeri** turndown, select **bağlamak** ve **tablo hücre görünümü**. Girin `objectValue.Occupation` için **Model anahtar yolu**:  

    [![Model anahtar yoluna girdiğinden](databinding-images/outline10.png "modeli anahtar yolu girme")](databinding-images/outline10-large.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Biz uygulama çalıştırırsanız, anahattı bizim dizisi ile doldurulur `PersonModels`:

[![Uygulamayı çalıştıran](databinding-images/outline11.png "uygulamayı çalıştırma")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Koleksiyon görünümü veri bağlama

Bir dizi denetleyicisi için koleksiyon veri sağlamak için kullanılan bir koleksiyon görünümü ile veri bağlaması olan bir tablo görünümü, bağlama gibi çok benzer. Koleksiyon görünümü hazır görüntü biçimi sahip olmadığından, daha fazla iş kullanıcı etkileşimi geribildirim sağlamak ve kullanıcının seçimi izlemek için gereklidir.

> [!IMPORTANT]
> Xcode 7 ve macOS 10.11 (ve büyük) bir sorun nedeniyle, koleksiyon görünümlerini bir film şeridi (.storyboard) dosyalarını kullanılacak sorunu yaşıyor. Sonuç olarak, koleksiyon görünümlerini Xamarin.Mac uygulamalarınız için tanımlamak için .xib dosyalarını kullanmaya devam etmek gerekir. Lütfen bakın bizim [koleksiyon görünümlerini](~/mac/user-interface/collection-view.md) daha fazla bilgi için.

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

## <a name="debugging-native-crashes"></a>Yerel çökme (Crash) hata ayıklama

Hata, veri bağlamaları yapma içinde sonuçlanabilir bir _yerel kilitlenme_ yönetilmeyen kod ve Xamarin.Mac uygulamanız ile tamamen başarısız olmasına neden bir `SIGABRT` hata:

[![Yerel kilitlenme iletişim kutusu örneği](databinding-images/debug01.png "yerel kilitlenme iletişim kutusu örneği")](databinding-images/debug01-large.png#lightbox)

Genellikle dört ana nedeni vardır yerel çökme (Crash) veri bağlama sırasında:

1. Veri modelinizi öğesinden devralmıyor `NSObject` veya bir alt sınıfı `NSObject`.
2. Objective-C kullanarak özelliğinizi kullanıma değil `[Export("key-name")]` özniteliği.
3. Erişimci'nın değeri değişiklikler kaydırılacak `WillChangeValue` ve `DidChangeValue` yöntem çağrıları (aynı anahtara belirtme `Export` özniteliği).
4. Hatalı veya yanlış yazılan anahtar sahip **bağlama denetçisi** arabirimi Oluşturucu.

### <a name="decoding-a-crash"></a>Bir kilitlenme kod çözme

Şimdi bulmak ve düzeltmek nasıl gösteriyoruz şekilde yerel çökmesine bizim veri bağlama neden oluyor. Arabirim Oluşturucusu'nda, koleksiyon görünümü örnekten ilk etiketi bizim bağlamasının değiştirelim `Name` için `Title`:

[![Bağlama anahtarı düzenleme](databinding-images/debug02.png "bağlama anahtarı düzenleme")](databinding-images/debug02-large.png#lightbox)

Şimdi değişikliği kaydetmek, Xcode ile eşitleme ve uygulamamızı çalıştırmak Mac için Visual Studio için dönün. Koleksiyon görünümü görüntülendiğinde, uygulama ile kısa bir süre içinde kilitleniyor bir `SIGABRT` hata (gösterildiği gibi **uygulama çıktısı** Mac için Visual Studio) bu yana `PersonModel` bir özellik anahtarı ile kullanıma sunmuyor `Title`:

[![Bir bağlama hatası örneği](databinding-images/debug03.png "bir bağlama hatası örneği")](databinding-images/debug03-large.png#lightbox)

Hata çok üstüne kaydırırsanız **uygulama çıktısı** sorunu çözmek için anahtarı görebiliriz:

[![Hata günlüğünde sorunu bulma](databinding-images/debug04.png "hata günlüğünde sorunu bulma")](databinding-images/debug04-large.png#lightbox)

Bu satırı bize söyleyen olan anahtar `Title` bağlama biz nesnesi mevcut değil. Biz değiştirirseniz, bağlama yeniden `Name` arabirimi Oluşturucu, kaydetme, eşitleme, yeniden oluşturun ve çalıştırın, uygulama sorun beklendiği gibi çalışır.

## <a name="summary"></a>Özet

Bu makalede, veri bağlama ve anahtar-değer Xamarin.Mac uygulamada kodlama ile çalışan bir ayrıntılı bakış duruma getirdi. İlk olarak, bir C# sınıfına Objective-C (KVC) anahtar-değer kodlama kullanılarak ve anahtar-değer (KVO) Gözlemleme gösterme Aranan. Ardından, KVO uyumlu sınıfının nasıl kullanılacağı gösterilmiştir ve veri Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğelerine bağlayın. Son olarak, karmaşık veri bağlama işlemini kullanma gösterdi **dizi denetleyicileri** ve **ağaç denetleyicileri**.


## <a name="related-links"></a>İlgili bağlantılar

- [MacDatabinding film şeridi (örnek)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (örnek)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Standart denetimler](~/mac/user-interface/standard-controls.md)
- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Anahat görünümleri](~/mac/user-interface/outline-view.md)
- [Koleksiyon görünümleri](~/mac/user-interface/collection-view.md)
- [Anahtar-değer programlama kılavuzu kodlama](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Anahtar-değer Gözlemleme Programlama Kılavuzu'na giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Programlama konularına Cocoa bağlamaları giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Giriş Cocoa bağlamaları başvurusu](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
