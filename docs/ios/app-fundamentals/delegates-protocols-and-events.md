---
title: Olaylar, protokolleri ve temsilciler
description: "Bu makalede geri aramalar almak ve kullanıcı arabirimi denetimlerini veri ile doldurmak için kullanılan anahtar iOS teknolojiler sunar. Bu, olaylar, protokolleri ve temsilciler teknolojilerdir. Bu makalede ne açıklanmaktadır her birini ve nasıl her kullanılan C# ' dan. İOS denetimleri Xamarin.iOS Xamarin.iOS Objective-C Kavramları protokolleri ve temsilciler gibi için destek sağlar. nasıl tanıdık .NET olayları da kullanıma sunmak için nasıl kullanır gösterir (Objective-C temsilciler olmamalıdır C# ile temsilciler kafası). Bu makalede ayrıca nasıl protokolleri – hem Objective-C Temsilciler ve temsilci olmayan senaryolarda temel olarak kullanıldığını gösteren örnekler sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5df7c2bbc7be1089795c94b6f639bd4556b49366
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="events-protocols-and-delegates"></a>Olaylar, protokolleri ve temsilciler

_Bu makalede geri aramalar almak ve kullanıcı arabirimi denetimlerini veri ile doldurmak için kullanılan anahtar iOS teknolojiler sunar. Bu, olaylar, protokolleri ve temsilciler teknolojilerdir. Bu makalede ne açıklanmaktadır her birini ve nasıl her kullanılan C# ' dan. İOS denetimleri Xamarin.iOS Xamarin.iOS Objective-C Kavramları protokolleri ve temsilciler gibi için destek sağlar. nasıl tanıdık .NET olayları da kullanıma sunmak için nasıl kullanır gösterir (Objective-C temsilciler olmamalıdır C# ile temsilciler kafası). Bu makalede ayrıca nasıl protokolleri – hem Objective-C Temsilciler ve temsilci olmayan senaryolarda temel olarak kullanıldığını gösteren örnekler sağlar._

Xamarin.iOS denetimlerini çoğu kullanıcı etkileşimleri olaylar oluşturmak için kullanır.
Geleneksel .NET uygulamaları gibi Xamarin.iOS uygulamaları kadar aynı şekilde bu olayları kullanabilir. Örneğin, Xamarin.iOS UIButton sınıfı TouchUpInside adlı bir olay vardır ve bu olay yalnızca bu sınıf ve olay, bir .NET uygulamasında değilmiş gibi kullanır.

Bu .NET yaklaşım yanı sıra, daha karmaşık etkileşim ve veri bağlama için kullanılabilir başka bir model Xamarin.iOS kullanıma sunar. Bu yöntem, Apple Temsilciler ve protokolleri çağırır kullanılmaktadır. Temsilciler kavram olarak C# temsilcilere benzerdir, ancak tanımlama ve tek bir yöntemi çağırmak yerine, bir temsilci Objective-C protokolü ile uyumlu bir tüm sınıfıdır. Yöntemlerinin isteğe bağlı bir protokol C# ' ta, bir arabirim için benzerdir. Bu nedenle örneğin UITableView verilerle doldurmak için UITableView kendisini doldurmak için çağırırdı UITableViewDataSource protokolünde tanımlanan yöntemler uygulayan bir temsilci sınıf oluşturursunuz.

Bu konular hakkında öğreneceksiniz bu makalede, sağlam bir temel Xamarin.iOS, geri çağırma senaryolarda işlemek için vermiş dahil olmak üzere:

-  **Olayları** – Uıkit denetimleri ile kullanarak .NET olaylar.
-  **Protokolleri** – ne öğrenme protokolleri olan ve bunların nasıl kullanıldığı ve örneği oluşturma, harita ek bilgi için veri sağlar.
-  **Temsilciler** – hakkında ek açıklama içeren bir kullanıcı etkileşimi işlemek için harita örnek genişletme ardından güçlü ve zayıf Temsilciler ve bunların her birini kullanmak ne zaman arasındaki farkı öğrenme Objective-C temsilciler öğrenme.


Protokoller ve temsilciler göstermek için biz aşağıda gösterildiği gibi ek açıklamanın Haritası ekler bir basit harita uygulaması derlemeyi:

 [ ![](delegates-protocols-and-events-images/01-map.png "Ek açıklamanın Haritası ekler basit harita uygulaması örneği") ](delegates-protocols-and-events-images/01-map.png) [ ![ ] (delegates-protocols-and-events-images/04-annotation-with-callout.png "bir eşlemesine eklenen bir örnek ek açıklaması")](delegates-protocols-and-events-images/04-annotation-with-callout.png)

Bu uygulama tackling önce .NET olayları Uıkit altında bakarak başlayalım.

 <a name=".NET_Events_with_UIKit" />


## <a name="net-events-with-uikit"></a>Uıkit .NET olayları

Xamarin.iOS Uıkit denetimlerinde .NET olayları gösterir. Örneğin, UIButton .NET içinde normalde yaptığınız gibi C# lambda ifadesi kullanır aşağıdaki kodda gösterildiği gibi ele TouchUpInside olay vardır:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

Ayrıca bu yöntemle bir C# 2.0 stili anonim bunun gibi uygulamanız:

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Önceki kod yukarı UIViewContoller ViewDidLoad yönteminde kablolu ise. İOS Tasarımcısı veya koduyla ekleyebilirsiniz bir düğme aButton değişkenine başvuruyor. Bu makalede eşlik örnekten gerçekleştirilecek iOS Tasarımcısı, eklenen aşağıdaki şekilde bu düğme gösterilmektedir:

 [ ![](delegates-protocols-and-events-images/02-interface-builder-outlet.png "İOS Tasarımcısı eklenen bir düğme")](delegates-protocols-and-events-images/02-interface-builder-outlet.png)

Xamarin.iOS, kodunuzu bir denetimle oluşan etkileşim bağlanma hedef eylem stili de destekler. Merhaba düğmesi için bir hedef eylem oluşturmak için çift iOS Tasarımcısı tıklatın. UIViewController'ın arka plan kod dosyasına görüntülenir ve geliştirici INSERT bağlantı yöntemi için bir konum seçin istenir:

 [ ![](delegates-protocols-and-events-images/03-interface-builder-action.png "UIViewControllers arka plan kod dosyası")](delegates-protocols-and-events-images/03-interface-builder-action.png)

Konumu seçtikten sonra yeni bir yöntem oluşturulur ve kablolu denetime yukarı. Düğme tıklatıldığında aşağıdaki örnekte, bir ileti konsola yazılır:

 [ ![](delegates-protocols-and-events-images/05-interface-builder-action.png "Düğme tıklatıldığında bir ileti konsola yazılır")](delegates-protocols-and-events-images/05-interface-builder-action.png)

Hedef eylem bölümü iOS hedef eylem desen hakkında daha fazla ayrıntı için bkz: " [iOS için uygulama yetkinlikleri çekirdek](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)" Apple iOS Geliştirici Kitaplığı'nda.

İOS Tasarımcısı Xamarin.iOS ile kullanma hakkında daha fazla bilgi için bkz: " [iOS Tasarımcısı genel bakış](~/ios/user-interface/designer/index.md)" belgeleri.

 <a name="Events" />


## <a name="events"></a>Olaylar

UI denetim olayları müdahale istiyorsanız, bir dizi seçenek vardır: C# Lambda'lar ve alt düzey Objective-C API'lerini kullanarak temsilci işlevlerini kullanarak.

Aşağıdaki bölümde, gereksinim duyduğunuz ne kadar denetime bağlı olarak bir düğme TouchDown olayına nasıl saptayacaktır gösterir.

 <a name="C#_Style" />


## <a name="c-style"></a>C# stili

Temsilci sözdizimini kullanarak:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {    
    Console.WriteLine ("Touched");
};
```

Lambda'lar bunun yerine isterseniz:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Sahip olmak istiyorsanız aynı kod paylaşmak için aynı işleyici birden çok düğmelerini kullanın:

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

<a name="Monitoring_more_than_one_kind_of_Event" />


## <a name="monitoring-more-than-one-kind-of-event"></a>Birden fazla tür olay izleme

C# olaylar UIControlEvent bayrakları için tek tek bayraklarına bire bir eşleme sahiptir. İki veya daha fazla olayları işlemek kodu aynı parçası olmasını istediğinizde, kullanmak `UIControl.AddTarget` yöntemi:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Lambda sözdizimini kullanarak:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Belirli nesne örneğine takma ve belirli bir seçici çağırma gibi Objective-C, alt düzey özelliklerini kullanmanız gerekiyorsa:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Lütfen unutmayın, devralınan bir taban sınıf içinde örnek yöntemi uygularsanız, genel yöntem olması gerekir.

 <a name="Protocols" />


## <a name="protocols"></a>protokolleri

Bir protokol yöntem bildirimleri listesini sağlayan bir Objective-C dil özelliğidir. Bir iletişim kuralı isteğe bağlı yöntemler olabilir olmasına, C#, temel fark bir arabirim için benzer bir amaca hizmet eder. Bir protokol uyarlar sınıfı bunları uygulamadığında isteğe bağlı yöntemler adı değil. Ayrıca, bir C# sınıfı birden çok arabirimleri yalnızca uygulayabilir gibi Objective-C tek sınıfta birden çok protokolü uygulayabilirsiniz.

Apple iOS boyunca protokollerini sınıflar, böylece yalnızca bir C# arabirimi gibi işletim çağıran uygulama sınıfından hemen özetleyen sırada benimsemek için sözleşmelerini tanımlamak için kullanır. Protokolleri kullanılır temsilci olmayan senaryolarda her ikisi de (gibi ile `MKAnnotation` sonraki gösterilen örnek) ve temsilciler (temsilciler bölümünde bu belgenin sonraki bölümlerinde sunulan gibi).

 <a name="Protocols_with_Monotouch" />


### <a name="protocols-with-xamarinios"></a>Xamarin.ios protokollerle

Xamarin.iOS Objective-C protokolünden kullanarak örnek bir göz atalım. Bu örnekte, kullanacağız `MKAnnotation` parçasıdır protokolü, `MapKit` framework. `MKAnnotation` Haritaya eklenebilir açıklamanın hakkında bilgi sağlamak üzere uyarlar herhangi bir nesne sağlayan bir protokoldür. Örneğin, bir nesne uygulama `MKAnnotation` ek açıklama ve ilişkili başlık konumunu sağlar.

Bu şekilde `MKAnnotation` Protokolü açıklamanın eşlik ilgili veri sağlamak için kullanılır. Ek açıklama kendisi için gerçek görünümü uyarlar nesnesindeki veri oluşturulur `MKAnnotation` protokolü. Örneğin, kullanıcı (aşağıdaki ekran görüntüsünde gösterildiği gibi) ek açıklamayı dokunur açtığınızda görüntülenen belirtme çizgisi için metin alanından gelir `Title` protokolü uygulayan sınıf özelliği:

 [ ![](delegates-protocols-and-events-images/04-annotation-with-callout.png "Ek açıklamayı kullanıcı dokunur zaman belirtme çizgisi için metin örneği")](delegates-protocols-and-events-images/04-annotation-with-callout.png)

Derin Dalış protokolleri, sonraki bölümde açıklandığı gibi Xamarin.iOS protokolleri soyut sınıflar bağlar. İçin `MKAnnotation` ilişkili C# sınıfı protokolü, adlandırılan `MKAnnotation` adını protokolü ve onu taklit etmek üzere sınıfıdır `NSObject`, CocoaTouch için kök taban sınıfı. Protokol alıcı hem de ayarlayıcı koordinatı uygulanacak gerektirir; Ancak, başlık ve alt başlık isteğe bağlıdır. Bu nedenle, içinde `MKAnnotation` sınıfı, `Coordinate` özelliği *soyut*, uygulanması için gerektiren ve `Title` ve `Subtitle` özellikleri işaretlenir *sanal* , bunları isteğe bağlı, aşağıda gösterildiği gibi yapma:

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

Herhangi bir sınıf ek açıklama verileri yalnızca türetme tarafından sağlayabilir `MKAnnotation`, en az olarak uzun `Coordinate` özelliği uygulanır. Örneğin, koordinat oluşturucuda alıp başlığı için bir dize döndüren bir örnek sınıfı şöyledir:

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string _title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        _title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return _title;
        }
    }
}
```

Bağlı için protokolü aracılığıyla herhangi bir alt sınıfı `MKAnnotation` açıklamanın görünüm oluşturduğunda, eşleme tarafından kullanılacak ilgili verileri sağlayabilir. Eşleme için ek açıklama eklemek için yalnızca çağrısı `AddAnnotation` yöntemi bir `MKMapView` aşağıdaki kodda gösterildiği gibi örneği:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
new CLLocationCoordinate2D (42.3467512, -71.0969456);

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

Örneği burada harita değişkeni olan bir `MKMapView`, harita temsil eden sınıf olduğu. `MKMapView` Kullanacağı `Coordinate` veri türetilen `SampleMapAnnotation` harita üzerinde ek açıklama görünümü konumlandırmak için örneği.

`MKAnnotation` Protokolü sağlar bilinen uyguladıktan herhangi bir nesne üzerinde özelliklerini (Bu durumda eşleme) tüketici olmadan uygulama ayrıntılarını bilmenize gerek. Bu, olası ek açıklamalar, çeşitli haritaya ekleme kolaylaştırır.

 <a name="Protocols_Deep_Dive" />


### <a name="protocols-deep-dive"></a>Derin Dalış protokolleri

C# arabirimleri isteğe bağlı yöntemler desteği olmadığından Xamarin.iOS protokolleri soyut sınıflar eşler. Bu nedenle, Objective-C protokolünde benimsenmesi Xamarin.iOS içinde protokole bağlı soyut sınıf türetme ve gerekli yöntemleri uygulayarak gerçekleştirilir. Bu yöntemler, soyut yöntemler olarak sunulur. Protokol isteğe bağlı yöntemler için C# sınıfının sanal yöntemlerini bağlı olur.

Örneğin, bir kısmı şöyledir `UITableViewDataSource` protokolü, ilişkili Xamarin.iOS olarak:

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

Sınıfı Özet olduğuna dikkat edin. Xamarin.iOS protokollerin isteğe bağlı/gerekli yöntemlerini desteklemek için Özet sınıf sağlar.
Ancak, Objective-C protokolleri (veya C# arabirimleri), C# sınıfları birden çok devralma desteklemez. Bu protokoller kullanan ve iç içe geçmiş sınıflar genellikle müşteri adayları C# kodu tasarımını etkiler. Daha fazla Bu sorun hakkında temsilciler bölümünde bu belgenin sonraki bölümlerinde ele alınmaktadır.

 `GetCell(…)` Objective-C bağlı bir Özet yöntem *Seçici*, `tableView:cellForRowAtIndexPath:`, gerekli bir yöntemi, `UITableViewDataSource` protokolü. Seçici yöntem adı için Objective-C terimdir. Yöntemin gerektiği gibi zorlamak için Xamarin.iOS, soyut olarak bildirir. Başka bir yöntem `NumberOfSections(…)`, bağlı `numberOfSectionsInTableview:`. Bu yöntem protokol, isteğe bağlı kitabıdır Xamarin.iOS C# içinde geçersiz kılmak isteğe bağlı kolaylaştırarak sanal olarak bildirir.

Xamarin.iOS için tüm iOS bağlama ilgilenir. Bir protokol Objective-C el ile bağlama ihtiyacınız varsa, ancak bir sınıfla tasarlayarak bunu yapabilirsiniz `ExportAttribute`. Bu Xamarin.iOS tarafından kullanılan yöntem ile aynıdır.

Objective-C türleri içinde Xamarin.iOS bağlama hakkında daha fazla bilgi için bkz: [Objective-C türleri bağlama](~/ios/platform/binding-objective-c/index.md).

Aracılığıyla protokollerle henüz, ancak değiliz. Bunlar ayrıca iOS temel olarak bir sonraki bölüm konuya Objective-C temsilciler için kullanılır.

 <a name="Delegates" />


## <a name="delegates"></a>Temsilciler

iOS Objective-C temsilciler içinde bir nesne iş diğerine geçirir temsilci desen uygulamak için kullanır. Çalışarak ilk nesne temsilci nesnesidir. Bir nesne belirli konular olduktan sonraki iletiler göndererek çalışmak için kendi temsilci söyler. Objective-C şuna benzer bir ileti gönderme, bir C# ' metodunu için işlevsel olarak eşdeğerdir. Bir temsilci bu çağrıları yanıtta yöntemlerini uygular ve böylece uygulama için işlevsellik sağlar.

Temsilciler alt sınıfların oluşturmak zorunda kalmadan sınıfları davranışını genişletmenizi sağlar. Önemli bir eylem oluştuktan sonra bir sınıf geri diğerine çağırdığında iOS uygulamalarında genellikle temsilciler kullanın. Örneğin, `MKMapView` temsilci sınıfı yazarı uygulama içinde yanıt fırsatı verir açıklamanın bir harita kullanıcı dokunur zaman kendi temsilci geri çağrı sınıfı. Bu tür bir örnek kullanarak bu makalenin sonraki bölümlerinde temsilci kullanım örneği aracılığıyla bir temsilci Xamarin.iOS ile çalışabilirsiniz.

Bu noktada, bir sınıf kendi temsilci üzerinde çağırmak için hangi yöntemlerin nasıl belirlediğini merak ediyor olabilirsiniz. Bu protokollerini kullandığınız başka bir yerdir. Genellikle, bir temsilci için kullanılabilen yöntemler bunlar benimsemeye kurallarından gelir.

 <a name="How_Protocols_are_used_with_Delegates" />


### <a name="how-protocols-are-used-with-delegates"></a>Protokolleri temsilciler ile nasıl kullanılır

Bir harita ekleme ek açıklamalar desteklemek için protokolleri önceki nasıl kullanıldığı gördük.
Protokolleri yöntemleri belirli olaylar meydana, gibi kullanıcı dokunma gibi ek açıklama harita üzerinde sonra veya bir tablodaki bir hücreyi seçer sonra çağrılacak sınıfları için bilinen bir dizi sağlamak için de kullanılır. Bu yöntemleri uygulayan sınıflar onları çağıran sınıfları temsilci olarak bilinir.

Temsilci destekleyen sınıfları temsilci uygulayan sınıfa atanmış bir temsilci özelliği göstererek bunu. Temsilcisi uygulamak yöntemler belirli temsilci uyarlar Protokolü değişir. İçin `UITableView` yöntemi, uygulamanız `UITableViewDelegate` protokolü için `UIAccelerometer` yöntemi, uygulaması `UIAccelerometerDelegate`, vb. bir temsilci istediğiniz için iOS boyunca başka bir sınıf kullanıma sunmak için.

`MKMapView` Önceki örneğimizde gördüğümüz sınıfı de bunu çağıracak temsilci adlı bir özelliği vardır çeşitli olayları oluştuktan sonra. Temsilcisi `MKMapView` türü `MKMapViewDelegate`.
Bu kısa süre içinde seçildikten sonra ek açıklama için yanıt için bir örnek, ancak ilk şimdi kullanacağınız güçlü ve zayıf temsilciler arasındaki farkı tartışın.

 <a name="Strong_Delegates_vs._Weak_Delegates" />


### <a name="strong-delegates-vs-weak-delegates"></a>Güçlü temsilciler vs. Zayıf temsilciler

Şu ana kadar inceledik temsilciler kesin türü belirtilmiş anlamı güçlü temsilciler ' dir. İOS her temsilci protokolünde için kesin türü belirtilmiş bir sınıf ile Xamarin.iOS bağlamaları açıklanır. Ancak, iOS zayıf bir temsilci kavramı vardır. Bir sınıf için belirli bir temsilci Objective-C protokole bağlı sınıflara yerine iOS ayrıca Protokolü yöntemleri kendiniz yöntemlerinizi ExportAttribute ile dekorasyon NSObject türeyen istediğiniz herhangi bir sınıf olarak bağlamak seçmenize olanak tanır ve ardından uygun seçiciler sağlama.
Bu yaklaşımı seçtiğinizde WeakDelegate özelliğine temsilci özelliğine, sınıfının bir örneği atayın. Zayıf bir temsilci temsilci sınıfınız farklı bir devralma hiyerarşisi aşağı olabilmesi için esneklik sunar. Güçlü ve zayıf temsilciler kullanan bir Xamarin.iOS örneğe bakalım.

 <a name="Example_using_a_Delegate_with_Xamarin.iOS" />


### <a name="example-using-a-delegate-with-xamarinios"></a>Örnek bir temsilci Xamarin.iOS ile kullanma

Bir alt geçebiliriz yanıt örneğimizde ek açıklama dokunarak kullanıcı olarak kod yürütmek için `MKMapViewDelegate` ve bir örneğine atadığınız `MKMapView`'s `Delegate` özelliği. `MKMapViewDelegate` Protokolü yalnızca isteğe bağlı yöntemler içerir.
Bu nedenle, tüm yöntemleri sanal Xamarin.iOS bu protokolünde bağlı `MKMapViewDelegate` sınıfı. Kullanıcı bir ek açıklamanın seçtiğinde `MKMapView` örneği gönderecek `mapView:didSelectAnnotationView:` kendi temsilci ileti. Bu Xamarin.iOS işlemek için geçersiz kılmak ihtiyacımız `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` yöntemi MKMapViewDelegate alt şöyle:

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

Yukarıda gösterilen SampleMapDelegate sınıfı içeren denetleyicisi içinde iç içe geçmiş sınıf olarak gerçekleştirilen `MKMapView` örneği. Objective-C, doğrudan sınıf içinde birden çok protokolü benimsemeye denetleyicisi genellikle görürsünüz. Ancak, protokolleri Xamarin.iOS sınıflarda bağlı olduğundan, kesin türü belirtilmiş temsilciler uygulayan sınıflar iç içe geçmiş sınıflar genellikle dahil edilir.

Yerinde temsilci sınıfı uygulama ile yalnızca bir temsilci denetleyicideki örneği ve atayın gerekir `MKMapView`'s `Delegate` aşağıda gösterildiği gibi özelliği:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

Aynı şey gerçekleştirmek için zayıf bir temsilci kullanmak için yöntem kendiniz türetilen herhangi bir sınıf bağlanacağını gerekir `NSObject` ve atayın `WeakDelegate` özelliği `MKMapView`. Bu yana `UIViewController` sınıfı sonuçta türetilen `NSObject` (her Objective-C sınıfında gibi CocoaTouch), biz yalnızca bağlı bir yöntem uygulayabilirsiniz `mapView:didSelectAnnotationView:` doğrudan controller'daki ve denetleyicisine Ata `MKMapView`'s `WeakDelegate`, çok iç içe geçmiş sınıf gereksinimini önleme. Aşağıdaki kodu, bu yaklaşım gösterir:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

Bu kod çalıştırırken, tam olarak kesin türü belirtilmiş temsilci sürüm çalıştırırken yaptığınız gibi uygulamayı şekilde davranır. Bu koddan avantajı, zayıf temsilci biz kesin türü belirtilmiş temsilci kullanıldığında, oluşturulan ek sınıfı oluşturulmasını gerektirmez ' dir. Ancak, bu tür güvenliği ödün verme pahasına olur. Geçirilmedi Seçici içinde bir hata yaparsanız olsaydı `ExportAttribute`, çalışma zamanına kadar öğrenmek olmayacaktır.

 <a name="Events_and_Delegates" />


### <a name="events-and-delegates"></a>Olaylar ve temsilciler

Temsilciler .NET olayları benzer şekilde nasıl kullanacağını iOS içinde geri aramalar için kullanılır. İOS yapmak için API'ler ve Objective-C temsilcileri kullanma biçiminizle gözükmüyor daha .NET gibi Xamarin.iOS temsilciler iOS kullanıldığı birçok yerde .NET olayları gösterir.

Örneğin, önceki uygulama nerede `MKMapViewDelegate` yanıt Seçili ek açıklama da Xamarin.iOS içinde .NET olay kullanılarak uygulanabilir. Bu durumda, olay tanımlanması `MKMapView` ve adlı `DidSelectAnnotationView`. Olması gereken bir `EventArgs` türünde bir alt `MKMapViewAnnotationEventsArgs`. `View` Özelliği `MKMapViewAnnotationEventsArgs` ek açıklama görünüme yönelik bir başvuru verirsiniz, hangi aynı uygulamasıyla devam öğesinden önceki Resimli olarak burada sahip olduğunuz:

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

 <a name="Summary" />


## <a name="summary"></a>Özet

Bu makalede olaylar, protokolleri ve temsilciler Xamarin.iOS içinde kullanmak nasıl ele. Xamarin.iOS denetimleri için normal .NET stili olayları nasıl gösterir gördük.
Sonraki nasıl C# arabirimlerinden farklı ve Xamarin.iOS bunları nasıl kullandığını dahil olmak üzere Objective-C protokolleri hakkında öğrendiniz. Son olarak, bir Xamarin.iOS açısından Objective-C temsilciler incelendi. Nasıl Xamarin.iOS kesinlikle destekler ve zayıf yazılmış Temsilciler ve yöntemleri temsilci .NET olayları bağlanacak nasıl gördük.


## <a name="related-links"></a>İlgili bağlantılar

- [Protokolleri, temsilciler ve olaylar (örnek)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Merhaba, iOS](~/ios/get-started/hello-ios/index.md)
- [Bağlama Objective-C türleri](~/ios/platform/binding-objective-c/index.md)
- [Objective-C programlama dili](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [4 xcode'da kullanıcı arabirimleri tasarlama](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [İOS için uygulama yetkinlikleri çekirdek](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
