---
title: Xamarin.iOS için MonoTouch.Dialog giriş
description: Bu belgede, MonoTouch.Dialog (Colorado açıklanmaktadır. D), Xamarin.iOS ile hızlı, bildirim temelli kullanıcı Arabirimi geliştirme için bir çerçeve. Bu, bir arabirim kod veya JSON oluşturma ve çekme ve yenileme, arama, arka plan görüntü yükleyen ve daha fazla gibi özellikleri kullanmak için MonoTouch.Dialog API'leri kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bee4b460552c7273021b16955b52ba3d95d3e07c
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038410"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Xamarin.iOS için MonoTouch.Dialog giriş

MonoTouch.Dialog, MT'nin başvurulan Kısaca, D, geliştiricilerin uygulama ekranları ve görünüm denetleyicileri, tablolar vb. oluşturma sorunların yerine bilgileri kullanarak gezinme çıkış oluşturmasına olanak tanıyan bir Hızlı Kullanıcı Arabirimi geliştirme araç setidir. Bu nedenle, bir kullanıcı Arabirimi geliştirme ve kod azaltma önemli basitleştirme sağlar. Örneğin, aşağıdaki ekran görüntüsüne göz önünde bulundurun:

 [![](images/image1.png "Örneğin, bu ekran göz önünde bulundurun.")](images/image1.png#lightbox)

Aşağıdaki kod, bu ekranın tamamını tanımlamak için kullanılan:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

İOS tablolarla çalışırken da genellikle bir sürü tekrarlayan kod yoktur.
Örneğin, bir tablo gereklidir her zaman bu tabloyu doldurmak için bir veri kaynağı gereklidir. Bir gezinti denetleyicisi bağlanan iki tablo tabanlı ekranlı bir uygulamada, aynı kod her ekranını paylaşır.

MT'NİN D, tablo oluşturma için genel bir API'de tüm kodun kapsülleyerek basitleştirir. Ardından, daha da kolay hale getirir söz dizimi bağlama için bildirim temelli bir nesne sağlayan bu API üzerinde bir Özet sağlar. Bu nedenle, kullanılabilir iki API MT'nin D:

-   **Alt düzey öğeler API'sini** – *öğeler API'sini* ekranlar ve bileşenleri temsil eden öğe Cerberus ağacındaki oluşturma ile ilgili temel alır. Öğeler API'sini geliştiriciler, en fazla esnekliği ve kullanıcı arabirimleri oluşturmada denetim sağlar. Ayrıca, öğeler API'sini hem inanılmaz hızlı bildirimi, hem de dinamik kullanıcı Arabirimi oluşturma bir sunucudan veren, JSON aracılığıyla destek bildirim temelli tanımı için Gelişmiş sahiptir. 
-   **Üst düzey yansıma API'si** – olarak da bilinen *bağlama**API* , hangi sınıfların UI ipuçları ve sonra da MT'nin açıklamalı olan, D otomatik olarak nesnelere bağlı olan ekranlar oluşturur ve hangi arasında bir bağ görüntülenen (ve isteğe bağlı olarak düzenlenebilir) ekranda ve arka plandaki nesne yedekleme sağlar.   Yukarıdaki örnekte, yansıma API'si kullanımı gösterilmektedir. Bu API, API öğeleri yapan hassas bir denetim sağlamaz, ancak sınıfın özniteliklerine dayalı öğe hiyerarşisi kullanıma otomatik olarak oluşturarak daha karmaşıklığı azaltır. 


MT'NİN Ekran oluşturma için kullanıcı Arabirimi öğeleri yerleşik D büyük bir dizi birlikte paketlenmiş gelir ancak gereken özelleştirilmiş öğeleri ve Gelişmiş ekran düzenleri da tanır. Bu nedenle, genişletilebilirlik, birinci sınıf bir API'de oluşturulan kapsamlı özellikler de içerir. Geliştiriciler var olan öğe genişletin veya yenilerini oluşturabilir ve sorunsuz şekilde tümleştirin.

Ayrıca, MT'nin D bir "çekme yenileme", zaman uyumsuz resim yüklemeyi, destek ve Destek arama gibi yerleşik ortak iOS UX özelliklere sahiptir.

Bu makalede, MT'nin ile çalışma kapsamlı göz atalım D, dahil olmak üzere:

-   **MT'NİN D bileşenleri** – bu MT'nin yapmak sınıfları anlamaya odaklanır Hızlı bir şekilde kısa sürede alma etkinleştirmek için D. 
-   **Öğeleri başvurusu** – MT'nin yerleşik öğelerini kapsamlı bir listesi D. 
-   **Gelişmiş kullanım** – bu çekme ve yenileme, arama, arka plan görüntüsü yüklenirken, öğesi Hiyerarşiler için LINQ kullanma ve hücre, özel öğeler oluşturma gibi gelişmiş özellikleri kapsar ve denetleyicileri için MT'nin ile kullanın D. 

## <a name="setting-up-mtd"></a>MT'nin ayarlama D

MT'NİN D Xamarin.iOS ile dağıtılır. Bunu kullanmak için sağ **başvuruları** bir Xamarin.iOS düğümünün Mac için Visual Studio 2017 veya Visual Studio'da proje ve bir başvuru ekleyin **MonoTouch.Dialog 1** derleme. Ardından, ekleme `using MonoTouch.Dialog` kaynağınızın tablolarda kod gerektiğinde.

## <a name="understanding-the-pieces-of-mtd"></a>MT'nin parçalarını anlama D

Yansıma API'si, MT'nin kullanırken bile Yalnızca bu öğeleri API aracılığıyla doğrudan olarak oluşturulduysa D, başlık altında bir öğe hiyerarşisi oluşturur. Ayrıca, önceki bölümde belirtilen JSON desteği öğeleri de oluşturur. Bu nedenle, MT'nin oluşturan parçaları temel bir anlayışa sahip olmanın önemli olduğunu D.

MT'NİN D aşağıdaki dört bölümden kullanarak ekranlar oluşturur:

-  **DialogViewController**
-  **RootElement**
-  **Bölüm**
-  **Öğesi**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, veya *zamanlı kullanılan Cihazlar* kısa için devralınan `UITableViewController` ve bu nedenle bir tablo içeren bir ekran temsil eder. Normal UITableViewController gibi bir gezinti denetleyicisi üzerine DVCs gönderilemez.

### <a name="rootelement"></a>RootElement

A *RootElement* bir zamanlı kullanılan Cihazlar Git öğeleri için üst düzey bir kapsayıcıdır. Bu öğeler de içerebilen ardından bölümler içerir. RootElements işlenmez; Bunun yerine ne gerçekten işlenen için yalnızca kapsayıcılar demektir. Bir RootElement için bir zamanlı kullanılan Cihazlar atanır ve ardından alt zamanlı kullanılan Cihazlar işler.

### <a name="section"></a>Bölüm

Bir bölümü, bir tablodaki hücre grubudur. Normal tablo bir bölümle sahip isteğe bağlı olarak olabilir üstbilgi ve altbilgi oluşturabilir ya da metin ya da aşağıdaki ekran görüntüsünde gösterildiği gibi hatta özel görünümler olması:

 [![](images/image2.png "Normal tablo bir bölümle sahip isteğe bağlı olarak olabilir üstbilgi ve altbilgi oluşturabilir ya da metin ya da bu ekran görüntüsünde gösterildiği gibi hatta özel görünümler olması")](images/image2.png#lightbox)

### <a name="element"></a>Öğe

Bir öğeyi tabloda gerçek bir hücreyi temsil eder. MT'NİN D çok çeşitli veri türleri farklı veya farklı girdiler temsil eden öğeler paketlenmiş gelir. Örneğin, aşağıdaki ekran görüntüleri, birkaç kullanılabilir öğeleri göstermektedir:

 [![](images/image3.png "Örneğin, bu ekran görüntüleri gösteren birkaç kullanılabilir öğeleri")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>Üzerinde daha fazla bölüm ve RootElements

Haydi şimdi RootElements ve bölümlerde daha ayrıntılı olarak ele alınmıştır.

### <a name="rootelements"></a>RootElements

En az bir RootElement MonoTouch.Dialog işlemini başlatmak için gereklidir.

Bir RootElement bölüm/öğe değeri ile başlatılmış olursa bu değer, ekranın sağ tarafında işlenen yapılandırma özetini sağlayan öğesi bir alt bulmak için kullanılır. Örneğin, aşağıdaki ekran görüntüsünde sağdaki "Tatlı" Seçili desert değerini birlikte ayrıntı ekran başlığını içeren bir hücrenin ile sol tarafta bir tablo gösterir.

 [![](images/image4.png "Bu ekran görüntüsünde sağdaki tatlı, seçili desert değerini birlikte ayrıntı ekran başlığını içeren bir hücrenin soldaki bölmede bir tablo gösterir") ](images/image4.png#lightbox) [ ![ ] (images/image5.png "bu Aşağıdaki ekran görüntüsünde sağdaki tatlı, seçili desert değerini birlikte ayrıntı ekran başlığını içeren bir hücrenin soldaki bölmede bir tablo gösterir")](images/image5.png#lightbox)

Kök öğeleri de bölümleri içinde yeni bir iç içe geçmiş yapılandırma sayfası yükleniyor tetiklemek için yukarıda da gösterildiği gibi kullanılabilir. Bu modda kullanıldığında sağlanan açıklamalı alt yazı bölümünün içinde oluşturulması sırasında kullanılır ve alt sayfa için başlığı olarak da kullanılır. Örneğin:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

Yukarıdaki örnekte, kullanıcı "Tatlı üzerinde" dokunduğunda MonoTouch.Dialog yeni bir sayfa oluşturun ve ona "Tatlı" olan ve bir seçenek grubu üç değerlerle sahip kök ile gidin.

Biz geçirilen değer "2" için radiogroup denetimi içindeki için belirli Bu örnekte, "Tatlı" bölümünde "çikolatalı kek" radyo grubu seçersiniz. Bu 3 öğe listede (dizin sıfır) çekme anlamına gelir.

Add metodu çağrılırken veya C# 4 Başlatıcısı sözdizimi kullanılarak bölümlere ekler.
INSERT yöntemi bölümlerle animasyon eklemek için sağlanır.

Bir grup örnek (yerine bir radiogroup denetimi içindeki) ile RootElement oluşturursanız, bir bölümde görüntülendiğinde RootElement Özet değerini BooleanElements ve Group.Key değeri olarak aynı anahtara sahip CheckboxElements birikmiş olacaktır.

### <a name="sections"></a>Bölümler

Bölümler, ekranda Grup öğeleri için kullanılır ve bunlar RootElement yalnızca geçerli doğrudan alt öğeleri. Bölüm yeni RootElements dahil olmak üzere standart öğelerini içerebilir.

Bir bölümde katıştırılmış RootElements daha yeni bir düzeye gidin için kullanılır.

Dize olarak veya UIViews olarak bölümlerde üstbilgiler ve altbilgiler olabilir.
Dizeler genellikle yalnızca kullanır, ancak özel kullanıcı arabirimleri oluşturmak için herhangi bir Uıview üstbilgisindeki veya altbilgisindeki kullanabilirsiniz. Şu şekilde oluşturmak için bir dize ya da kullanabilirsiniz:

```csharp
var section = new Section ("Header", "Footer")
```

Görünümleri kullanmak için görünümler oluşturucusuna geçirmeniz yeterlidir:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Bildirim

#### <a name="handling-nsaction"></a>NSAction işleme

MT'NİN D yüzeyleri bir `NSAction` geri çağırmaları işlemek için temsilci olarak.
Örneğin, MT'nin tarafından oluşturulan bir tablo hücresi için bir touch olayı işlemek istediğiniz varsayalım. D. Bir öğe ile MT'nin oluştururken Aşağıda gösterildiği gibi bir geri çağırma işlevini D, yalnızca sağlar:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Öğe değerini alma

Birlikte `Element.Value` özelliği, geri çağırma diğer öğesinde ayarlanan değer alabilir. Örneğin, aşağıdaki kodu düşünün:

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

Bu kod, aşağıda gösterildiği gibi bir kullanıcı Arabirimi oluşturur. Bu örneğin tam bir kılavuz için bkz. [öğeleri API Kılavuzu](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) öğretici.

 [![](images/image6.png "Element.Value özelliğiyle birlikte, geri çağırma diğer öğesinde ayarlanan değer alabilir")](images/image6.png#lightbox)

Kullanıcı alt tablo hücresi bastığında anonim işlev kodu, değeri yazma yürütür `element` için örnek **uygulama çıktısı** Mac için Visual Studio takımı

## <a name="built-in-elements"></a>Yerleşik öğeler

MT'NİN D, bir dizi öğeleri olarak bilinen yerleşik tablo hücresi öğeleri ile birlikte gelir.
Bu öğeleri tablo hücrelerini birkaç adlandırmak için dizeleri, float, tarihleri ve hatta resimler gibi çeşitli farklı türlerini görüntülemek için kullanılır. Her öğe veri türü uygun şekilde görüntüleme üstlenir. Örneğin, bir boolean öğesi değeri geçiş yapmak için bir anahtar görüntülenir. Benzer şekilde, bir kayan nokta öğesi float değeri değiştirmek için bir kaydırıcı görüntülenir.

Görüntüleri ve html gibi daha zengin veri türleri desteklemek için daha da karmaşık öğeleri vardır. Örneğin, bir UIWebView seçildiğinde bir web sayfası yüklemek için açar, bir html öğesi bir açıklamalı alt yazı tablo hücresi içinde görüntüler.

### <a name="working-with-element-values"></a>Öğe değerleri ile çalışma

Kullanıcı girişi yakalamak için kullanılan öğeleri genel kullanıma `Value` herhangi bir zamanda geçerli öğenin değerini tutan bir özellik. Kullanıcının uygulamanın kullandığı gibi otomatik olarak güncelleştirilir.

Bu davranıştır MonoTouch.Dialog parçası olan tüm öğeleri için ancak kullanıcı tarafından oluşturulan öğeleri için gerekli değildir.

### <a name="string-element"></a>Dize öğesi

A `StringElement` tablo hücresi hücrenin sağ taraftaki dize değeri ve sol tarafta bir resim yazısı gösterir.

 [![](images/image7.png "Bir StringElement tablo hücresi ve dize değeri hücrenin sağ taraftaki sol tarafındaki resim yazısı gösterir.")](images/image7.png#lightbox)

Kullanılacak bir `StringElement` bir düğme olarak bir temsilci sağlar.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "Bir StringElement bir düğme olarak kullanılacak bir temsilci sağlar.")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Stil uygulanmış bir dize öğesi

A `StyledStringElement` dizeler ya da yerleşik tablosu hücre stilleri kullanarak gösterilmesini sağlar veya özel biçimlendirme.

 [![](images/image9.png "Dizeleri ya da yerleşik tablosu hücre stilleri kullanarak sunulacak bir StyledStringElement sağlar veya özel biçimlendirme")](images/image9.png#lightbox)

`StyledStringElement` Sınıf türetilir `StringElement`, ancak sağlar geliştiriciler özellikler yazı tipi, metin rengini, arka plan hücre rengi, satır kesme modu, görüntülenecek satırların sayısı gibi birkaç özelleştirebilir ve olup Donatılardan görüntülenmesi gerekir.

### <a name="multiline-element"></a>Çok satırlı öğesi

 [![](images/image10.png "Çok satırlı öğesi")](images/image10.png#lightbox)

### <a name="entry-element"></a>Giriş öğesi

`EntryElement`, Adından da anlaşılacağı, kullanıcı girişi almak için kullanılır. Bu, normal dize veya karakter burada gizlidir parolalar, destekler.

 [![](images/image11.png "EntryElement kullanıcı girişi almak için kullanılır")](images/image11.png#lightbox)

Bu üç değerle başlatılır:

-  Kullanıcıya gösterilen giriş başlığı.
-  (Bu, kullanıcıya bir ipucu sağlar gri metin) yer tutucu metni. 
-  Metin değeri.


Yer tutucu ve değer null olabilir. Ancak, başlık gereklidir.

Herhangi bir noktada kendi değer özellik erişimi değerini alabilir `EntryElement`.

Ayrıca `KeyboardType` özelliği, klavye türü için veri girişinin istenen stil oluşturma zamanında ayarlanabilir. Bu değerleri kullanarak klavye yapılandırmak için kullanılabilir `UIKeyboardType` aşağıda listelenen:

-  Sayısal
-  Telefon
-  URL
-  E-posta


### <a name="boolean-element"></a>Boolean öğesi

 [![](images/image12.png "Boolean öğesi")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Onay kutusu öğesi

 [![](images/image13.png "Onay kutusu öğesi")](images/image13.png#lightbox)

### <a name="radio-element"></a>Radyo öğesi

A `RadioElement` gerektiren bir `RadioGroup` belirtilmesi için `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "İçinde RootElement belirtilmesi radiogroup denetimi içindeki bir RadioElement gerektirir")](images/image14.png#lightbox)

 `RootElements` radyo öğeleri koordine etmek için de kullanılır. `RadioElement` Üyeleri, birden fazla bölüm (örneğin sistem zil seslerine zil sesi Seçici ve ayrı özel halkası tonları benzer bir şey uygulamak) kapsayabilir. Seçili radyo öğesi Özet görünümü gösterilir. Bunu kullanmak için oluşturma `RootElement` şöyle grubu Oluşturucusu ile:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Grup içinde adını `RadioGroup` seçili değer içeren bir sayfa (varsa) ve bu durumda sıfırsa, değeri olan ilk seçilen öğenin dizinini göstermek için kullanılır.

### <a name="badge-element"></a>Gösterge öğesi

 [![](images/image15.png "Gösterge öğesi")](images/image15.png#lightbox)

### <a name="float-element"></a>Float öğesi

 [![](images/image16.png "Float öğesi")](images/image16.png#lightbox)

### <a name="activity-element"></a>Element aktivity

 [![](images/image17.png "Element aktivity")](images/image17.png#lightbox)

### <a name="date-element"></a>Tarih öğesi

 ![](images/image18.png "Tarih öğesi")

DateElement için karşılık gelen hücre seçildiğinde, aşağıda gösterildiği gibi bir tarih seçici sunulur:

 [![](images/image19.png "DateElement için karşılık gelen hücre seçildiğinde, gösterildiği gibi bir tarih seçici sunulur")](images/image19.png#lightbox)

### <a name="time-element"></a>Time öğesi

 [![](images/image20.png "Time öğesi")](images/image20.png#lightbox)

TimeElement için karşılık gelen hücre seçildiğinde, aşağıda gösterildiği gibi bir saat Seçici sunulur:

 [![](images/image21.png "TimeElement için karşılık gelen hücre seçildiğinde, gösterildiği gibi bir saat Seçici sunulur")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime öğesi

 [![](images/image22.png "DateTime öğesi")](images/image22.png#lightbox)

DateTimeElement için karşılık gelen hücre seçildiğinde, aşağıda gösterildiği gibi bir tarih saat Seçici sunulur:

 [![](images/image23.png "DateTimeElement için karşılık gelen hücre seçildiğinde, gösterildiği gibi bir tarih saat Seçici sunulur")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML öğesi

 [![](images/image24.png "HTML öğesi")](images/image24.png#lightbox)

`HTMLElement` Değerini görüntüler, `Caption` tablo hücresi özelliği. Seçili Microsoft `Url` öğesine yüklenir bir `UIWebView` denetim aşağıda gösterildiği gibi:

 [![](images/image25.png "Microsoft seçtiyseniz, öğeye atanan Url UIWebView denetiminde aşağıda gösterildiği gibi yüklenir")](images/image25.png#lightbox)

### <a name="message-element"></a>İleti öğesi

 [![](images/image26.png "İleti öğesi")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Daha fazla öğe yükleme

Kullanıcıların listesine daha fazla öğe yüklemek bu öğeyi kullanırsınız. Normal ve yükleme açıklamalı alt yazılar yanı sıra, yazı tipini ve metin rengini özelleştirebilirsiniz.
`UIActivity` Göstergesi başlar animasyon ekleme ve bir kullanıcı hücre dokunduğunda yükleme başlık görüntülenir ve ardından `NSAction` yöntemlere geçirilen Oluşturucu yürütülür. Bir kez kodunuzda `NSAction` bittiğinde `UIActivity` göstergesi durdurur animasyon ekleme ve normal resim yazısı yeniden görüntülenir.

### <a name="uiview-element"></a>Uıview öğesi

Ayrıca, herhangi bir özel `UIView` kullanarak görüntülenebilen `UIViewElement`.

### <a name="owner-drawn-element"></a>Sahip tarafından çizilmiş öğesi

Bir soyut sınıfı olduğundan bu öğenin alt sınıflanmış gerekir. Geçersiz kılmalıdır `Height(RectangleF bounds)` yöntemi içinde döndürmelidir, öğenin yüksekliğini yanı `Draw(RectangleF bounds, CGContext context, UIView view)` içinde belirtilen sınırları içindeki tüm özelleştirilmiş çizim bağlamını ve görünüm parametrelerini kullanarak yapmanız gerekir. Bu öğe sınıflara kaynaklanan ağır yüklerden mu bir `UIView`ve, yalnızca iki basit geçersiz kılmalarını gerek bırakarak döndürülecek, hücrede yerleştirme. Örnek uygulamada daha iyi bir örnek uygulamasında gördüğünüz `DemoOwnerDrawnElement.cs` dosya.

Sınıfı, çok basit bir örnek aşağıda verilmiştir:

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
 }
```

### <a name="json-element"></a>JSON öğesi

`JsonElement` Sınıfıdır `RootElement` genişleten bir `RootElement` iç içe geçmiş alt içeriğini yerel veya uzak bir URL'den yüklenecek kullanabilmek için.

`JsonElement` Olduğu bir `RootElement` iki biçimde oluşturulabilir. Bir sürüm oluşturur bir `RootElement` isteğe bağlı içeriği yükler. Bunlar kullanılarak oluşturulan `JsonElement` içeriği yüklemek için url sonunda fazladan bağımsız değişken alan oluşturucular:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Yerel bir dosyaya veya var olan bir veri diğer formu oluşturur `System.Json.JsonObject` Ayrıştırılan:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

JSON MT'nin ile kullanma hakkında daha fazla bilgi için D, bkz: [JSON öğesi izlenecek](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) öğretici.

## <a name="other-features"></a>Diğer özellikler

### <a name="pull-to-refresh-support"></a>Çekme yenileme desteği

 *Çekme-için-* *Yenile* görsel efekt başlangıçta bulunur *Tweetie2* uygulamasını pek çok uygulama arasında popüler bir etkin hale geldi.

İletişim kutuları için otomatik yenileme çekme desteği eklemek için yalnızca iki işlem yapmanız gereken: kullanıcı veri çeker bildirilmesini sağlamak için bir olay işleyicisi bağlama ve bildirim `DialogViewController` ne zaman veri yüklendi varsayılan durumuna geri dönmek için.

Bir bildirim takma basittir; hemen bağlanmak `RefreshRequested` olayda `DialogViewController`, şöyle:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Sonra da yönteminizi `OnUserRequestedRefresh`, bazı veri yükleme kuyruk, bazı veriler net istek veya işlem verileri için bir iş parçacığı çalıştırın. Veriler yüklendikten sonra bilgilendirmelisiniz `DialogViewController` yeni veriler ve görünüm varsayılan durumuna geri yüklemek için çağrı yaparak bunu `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Arama desteği

Arama desteklemek için ayarlamanız `EnableSearch` özelliği, `DialogViewController`. Ayrıca `SearchPlaceholder` arama çubuğunda filigran metni olarak kullanılacak özellik.

Arama görünüm içeriğinin kullanıcı türleri olarak değişecektir. Bu görünen alanları arar ve bu kullanıcıya gösterilir. `DialogViewController` Programlı olarak başlatmak, sonlandırma veya sonuçları üzerinde yeni bir filtre işlemi tetiklemek için üç yöntem sunar. Bu yöntemleri aşağıda listelenmiştir:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


Sistem Genişletilebilir olduğundan istiyorsanız bu davranışı değiştirebilirsiniz.

### <a name="background-image-loading"></a>Arka plan görüntü yükleme

MonoTouch.Dialog içerir [TweetStation](https://github.com/migueldeicaza/TweetStation) uygulamanın resim yükleyici. Bu resim yükleyici destekler önbelleğe alma arka plan görüntüleri yüklemek için kullanılabilir ve görüntünün yüklendiğinde kodunuzu bildirimde bulunabilir.

Bu ayrıca giden ağ bağlantılarını sayısını sınırlar.

Resim yükleyici uygulanan `ImageLoader` sınıfı, tüm yapmanız gereken, arama `DefaultRequestImage` metodu, URI'yı yüklemek istediğiniz görüntünün, yanı sıra bir örneğini sağlayın gerekecek `IImageUpdated` olacağı arabirimi ne zaman çağrılır görüntü ha yüklenen s.

Örneğin aşağıdaki kod bir görüntü bir URL'ye yükleyen bir `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader sınıfı, tüm bellekte önbelleğe alınmış olan görüntüleri serbest bırakmak istediğiniz zaman çağırabilirsiniz bir temizleme yöntemi gösterir. Geçerli kod, 50 görüntüleri için bir önbelleğe sahiptir. Varsa (örneğin, 50 görüntüleri çok fazla olduğu büyük görüntüleri beklediğiniz varsa) farklı bir önbellek boyutu kullanmak istiyorsanız, yalnızca ImageLoader örneklerini oluşturmak ve önbellekte tutmak istediğinizi görüntülerinin sayısını geçirin.

## <a name="using-linq-to-create-element-hierarchy"></a>Öğe hiyerarşisi oluşturmak için LINQ kullanma

LINQ ve C# ' nin başlatma söz dizimi akıllı kullanımı, LINQ, bir öğe hiyerarşisi oluşturmak için kullanılabilir. Örneğin, aşağıdaki kod, bazı dize dizilerden ekran oluşturur ve tanıtıcıları hücre her geçirilen bir anonim işlev aracılığıyla seçimi `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

Bu kolayca bir XML veri deposu veya verilerden neredeyse tamamen karmaşık uygulamalar oluşturmak için bir veritabanından veri ile birleştirilir.

## <a name="extending-mtd"></a>MT'nin genişletme D

### <a name="creating-custom-elements"></a>Özel öğeleri oluşturma

Öğesinden devralan var olan bir öğe veya öğe kök sınıftan türetme tarafından kendi öğesi oluşturabilirsiniz.

Kendi öğesi oluşturmak için aşağıdaki yöntemleri geçersiz kılmak isteyeceksiniz:

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

Öğeniz bir değişken boyutu varsa uygulamanız gereken `IElementSizing` arabirimi, bir yöntem içerir:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Uygulamaya planlıyorsanız, `GetCell` yöntemi çağırarak `base.GetCell(tv)` ve döndürülen hücre özelleştirme, ayrıca geçersiz kılmanız gerekir `CellKey` öğeniz için benzersiz bir anahtar dönmesini şöyle:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Bu öğelerin çoğu, ancak için çalışır `StringElement` ve `StyledStringElement` gibi çeşitli işleme senaryoları için bu anahtar kendi kümesini kullanın. Bu sınıfların kodda çoğaltmak gerekir.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Hem yansıma hem de öğeler API'sini aynı kullanmak `DialogViewController`. Bazen görünümü görünümünü özelleştirmek isteyeceksiniz veya bazı özelliklerini kullanmak isteyebileceğiniz `UITableViewController` kullanıcı arabirimleri temel oluşturulmasını gidin.

`DialogViewController` Yalnızca sınıfıdır `UITableViewController` ve özelleştiren aynı şekilde özelleştirme bir `UITableViewController`.

Örneğin, ya da liste stilini değiştirmek istediğinizde `Grouped` veya `Plain`, bu değer, bu gibi denetleyicisi oluşturduğunuzda özelliğini değiştirerek ayarlayabilirsiniz:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Daha fazla bilgi için özelleştirmeleri Gelişmiş `DialogViewController`, kendi arka plan ayarlama gibi alt ve geçersiz kılma uygun olduğu yöntemleri, aşağıdaki örnekte gösterildiği gibi:

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

Aşağıdaki sanal yöntemleri başka bir özelleştirme noktasıdır `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Bu yöntem öğesinin döndürmelidir `DialogViewController.Source` çalışmaları nereye, hücre eşit boyutta veya öğesinin `DialogViewController.SizingSource` , hücre eşit olduğunda.

Bu geçersiz kılma herhangi birini yakalamak için kullanabileceğiniz `UITableViewSource` yöntemleri. Örneğin, [TweetStation](https://github.com/migueldeicaza/TweetStation) bu kullanıcı için üst kaydırılan ne zaman açtıklarını izlemek ve buna göre okunmamış tweet sayısı güncelleştirmek için kullanır.

## <a name="validation"></a>Doğrulama

Öğeleri doğrulama kendilerini web sayfaları için uygun olan model sağlamaz ve Masaüstü uygulamaları doğrudan iPhone etkileşim model eşlemeyin.

Veri doğrulama yapmak istiyorsanız, kullanıcı bir eylem girilen veriler ile tetiklendiğinde bunu yapmanız gerekir. Örneğin bir <span class="ui">Bitti</span> veya <span class="ui">sonraki</span> üstteki araç çubuğunda veya bazı düğmesine `StringElement` sonraki aşamaya git bir düğme olarak kullanılan.

Burada temel giriş doğrulaması gerçekleştirmeniz ve belki de daha fazla doğrulama sunucusuyla bir kullanıcı/parola bileşimini geçerliliğini denetleme gibi karmaşık budur.

Kullanıcı bir hata size nasıl belirli bir uygulamadır. Açılır bir `UIAlertView` veya İpucu Göster.

## <a name="summary"></a>Özet

Bu makalede, birçok MonoTouch.Dialog hakkında bilgi kapsamında. Nasıl temelleri ele MT'nin D çalışır ve kapsamdaki MT'nin oluşturan çeşitli bileşenler D. Aynı zamanda çeşit öğeleri ve tablo özelleştirmeleri MT'nin tarafından desteklenen gösterildi D ve nasıl ele MT'nin D özel öğeleri ile genişletilebilir. Ayrıca, MT'nin JSON desteği açıklanmaktadır. Bu D öğeleri JSON'dan dinamik olarak oluşturulmasını sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Yayını - bir iOS oturum açma ekranı Miguel de Icaza, MonoTouch.Dialog ile oluşturur.](http://youtu.be/3butqB1EG0c)
- [Yayını - iOS kullanıcı arabirimleri ile MonoTouch.Dialog kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [İzlenecek Yol: Öğeler API’sini kullanarak uygulama oluşturma](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [İzlenecek Yol: Yansıma API’sini kullanarak uygulama oluşturma](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [İzlenecek Yol: Kullanıcı Arabirimi oluşturmak için bir JSON Öğesini Kullanma](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON biçimlendirmesi](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
