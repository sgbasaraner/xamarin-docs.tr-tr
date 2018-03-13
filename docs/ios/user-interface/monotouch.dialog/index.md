---
title: "MonoTouch.Dialog giriş"
description: "(Yüksekliğindeki MonoTouch.Dialog D) araç seti, hızlı uygulama geliştirme Xamarin.iOS UI için vazgeçilmez bir çerçevedir. YÜKSEKLİĞİNDEKİ D, hızlı ve kolay karmaşık uygulama gezinti denetleyicileri, tablolar vb. birçoğunu yerine bildirim temelli bir yaklaşım kullanarak kullanıcı Arabirimi tanımlamanızı sağlar. Ayrıca, yüksekliğindeki D esnek bir tam denetim veya çekme yenileme, arka plan görüntüsü yükleme gibi ek özellikler yanı sıra yaklaşım ellerini ile geliştiriciler sağlayabilir, destek ve JSON verilerini aracılığıyla dinamik kullanıcı Arabirimi oluşturma arama API kümesi vardır. Bu kılavuz yüksekliğindeki ile çalışmak için kullanılabilecek çeşitli yöntemler sunar D ve ardından ayrıntılı Gelişmiş kullanım çekecek."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b9bf4c5ee803aa60a2730703e64fcf73d07efdb5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-monotouchdialog"></a>MonoTouch.Dialog giriş

_(Yüksekliğindeki MonoTouch.Dialog D) araç seti, hızlı uygulama geliştirme Xamarin.iOS UI için vazgeçilmez bir çerçevedir. YÜKSEKLİĞİNDEKİ D, hızlı ve kolay karmaşık uygulama gezinti denetleyicileri, tablolar vb. birçoğunu yerine bildirim temelli bir yaklaşım kullanarak kullanıcı Arabirimi tanımlamanızı sağlar. Ayrıca, yüksekliğindeki D esnek bir tam denetim veya çekme yenileme, arka plan görüntüsü yükleme gibi ek özellikler yanı sıra yaklaşım ellerini ile geliştiriciler sağlayabilir, destek ve JSON verilerini aracılığıyla dinamik kullanıcı Arabirimi oluşturma arama API kümesi vardır. Bu kılavuz yüksekliğindeki ile çalışmak için kullanılabilecek çeşitli yöntemler sunar D ve ardından ayrıntılı Gelişmiş kullanım çekecek._


Yüksekliğindeki başvurulan MonoTouch.Dialog D kısaca, geliştiricilerin uygulama ekranlar ve oluşturma görünümü denetleyicileri, tablolar vb. birçoğunu yerine bilgileri kullanarak gezinti oluşturmasına olanak veren bir hızlı UI geliştirme araç seti olur. Bu nedenle, kullanıcı Arabirimi geliştirme ve kod azaltma önemli bir alma sağlar. Örneğin, aşağıdaki ekran görüntüsünde göz önünde bulundurun:

 [![](images/image1.png "Örneğin, bu ekran göz önünde bulundurun")](images/image1.png#lightbox)

Aşağıdaki kod, tüm bu ekranı belirlemek için kullanılmıştır:

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

İOS tablolarda ile çalışırken, da genellikle bir ton tekrarlayan kod yoktur.
Örneğin, bir tablo gerektiği her zaman, bu tabloyu doldurmak için bir veri kaynağı gereklidir. Bir gezinti denetleyicisi bağlanan iki tablo tabanlı ekranlar sahip bir uygulamada, aynı kod çok her ekran paylaşır.

YÜKSEKLİĞİNDEKİ D, tüm bu kodu tablo oluşturma için genel bir API uygulamasına kapsülleyerek basitleştirir. Ardından, bile kolaylaştırır sözdizimi bağlama bildirim temelli bir nesne için sağlar, API üstünde bir Özet sağlar. Bu nedenle, iki API yüksekliğindeki içinde kullanılabilir yok D:

-   **Alt düzey öğeleri API** – *öğeleri API* bir hiyerarşik ağaç ekranlar ve bileşenleri temsil eden öğe oluşturma ile ilgili temel alır. Öğeleri API geliştiricilere en esneklik ve Uı'lar oluşturmada denetim sağlar. Buna ek olarak, öğeleri API bildirim temelli tanımı için hem son derece hızlı bildirimi yanı sıra bir sunucudan dinamik kullanıcı Arabirimi oluşturma sağlar JSON aracılığıyla Gelişmiş desteği. 
-   **Üst düzey yansıma API** – olarak da bilinen *bağlama**API* , hangi sınıfların UI ipuçları ve sonra da yüksekliğindeki açıklama içinde D otomatik olarak nesnelere bağlı ekranlar oluşturur ve ne arasında bir bağ görüntülenen (ve isteğe bağlı olarak düzenlenmiş) ekranda ve arka plandaki nesne yedekleme sağlar. Yukarıdaki örnekte yansıma API kullanımı gösterilmiştir. Bu API, API öğeleri mu hassas bir denetim sağlamaz, ancak sınıf özniteliklerini temel alarak öğesi hiyerarşi çıkışı otomatik olarak oluşturarak daha karmaşıklığını azaltır. 


YÜKSEKLİĞİNDEKİ Ekran oluşturma için kullanıcı Arabirimi öğeleri yerleşik D büyük bir dizi birlikte paketlenmiş gelir ancak özelleştirilmiş öğeleri ve Gelişmiş ekranı düzeni gereksinimini de algılar. Bu nedenle, genişletilebilirlik birinci sınıf bir API uygulamasına fırın kapsamlı özellikler içerir. Geliştiriciler mevcut öğelerini genişletmek veya yenilerini oluşturun ve sorunsuz şekilde tümleşir.

Ayrıca, yüksekliğindeki D "çekme yenileme" desteği, yükleme, zaman uyumsuz görüntü ve Destek arama gibi yerleşik genel iOS UX özellikleri sayısına sahip.

Bu makalede yüksekliğindeki ile çalışma kapsamlı bir göz atalım D dahil olmak üzere:

-   **YÜKSEKLİĞİNDEKİ D bileşenleri** – bu yüksekliğindeki yapmak sınıfları anlamaya odaklanır Hızlı bir şekilde hızlıca alma etkinleştirmek için D. 
-   **Öğeleri başvurusu** – yüksekliğindeki yerleşik öğelerinin kapsamlı bir listesi D. 
-   **Kullanım Gelişmiş** – bu çekme yenileme, arama, arka plan görüntü yükleme, öğesi Hiyerarşiler için LINQ kullanarak ve özel öğeleri, hücre oluşturma gibi gelişmiş özellikleri kapsar ve denetleyicileri için yüksekliğindeki ile kullanma D. 

## <a name="understanding-the-pieces-of-mtd"></a>Yüksekliğindeki parçalarını anlama D

API, yüksekliğindeki yansıma kullanırken bile Yalnızca bu öğeler API üzerinden doğrudan olarak oluşturulmuşsa D başlık altında bir öğe hiyerarşisi oluşturur. Ayrıca, önceki bölümde belirtildiği JSON desteği öğeleri de oluşturur. Bu nedenle, yüksekliğindeki bağlı parçasını ilgili temel bilgilere sahipsiniz önemlidir D.

YÜKSEKLİĞİNDEKİ D aşağıdaki dört bölümden kullanarak ekranlar oluşturur:

-  **DialogViewController**
-  **RootElement**
-  **Bölüm**
-  **Öğesi**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, veya *DVC* kısa için devraldığı `UITableViewController` ve bu nedenle bir tablo olan bir ekran temsil eder. Gezinti denetleyicisine normal UITableViewController gibi DVCs gönderilemez.

### <a name="rootelement"></a>RootElement

A *RootElement* DVC gidin öğeleri için üst düzey bir kapsayıcısıdır. Ardından öğeler içerebilir bölümleri içerir. RootElements işlenmez; Bunun yerine ne gerçekte işlendiğini için basitçe kapsayıcıları demektir. Bir RootElement bir DVC atanır ve daha sonra alt öğelerini DVC işler.

### <a name="section"></a>Bölüm

Bir bölümü, bir tablodaki hücre grubudur. Normal tablo bölümüyle, isteğe bağlı olarak sağlayabilirsiniz üstbilgi ve altbilgi seçebilir ya da metin ya da aşağıdaki ekran görüntüsünde olduğu gibi özel görünümleri olması:

 [![](images/image2.png "İsteğe bağlı olarak sağlayabilirsiniz normal tablo bölümle üstbilgi ve altbilgi seçebilir ya da metin ya da bu ekran olduğu gibi özel görünümleri olması")](images/image2.png#lightbox)

### <a name="element"></a>Öğe

Bir öğeyi tabloda gerçek bir hücreyi temsil eder. YÜKSEKLİĞİNDEKİ D çok çeşitli farklı veri türleri veya farklı girişleri temsil eden öğeleri paketlenmiş gelir. Örneğin, aşağıdaki ekran görüntüleri kullanılabilir öğelerin bazılarını gösterir:

 [![](images/image3.png "Örneğin, bu ekran görüntüleri kullanılabilir öğelerin bazılarını gösterir")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>Üzerinde daha fazla bölüm ve RootElements

Şimdi şimdi RootElements ve bölümlerde daha ayrıntılı olarak açıklanmaktadır.

### <a name="rootelements"></a>RootElements

En az bir RootElement MonoTouch.Dialog işlemini başlatmak için gereklidir.

Bir RootElement bir bölüm/öğesi değerle başlatılırsa, bu değer bir alt ekranın sağ tarafta işlenen yapılandırma özetini sağlayacak öğesi bulmak için kullanılır. Örneğin, aşağıdaki ekran görüntüsünde ayrıntı ekranın sağ taraftaki "Tatlı" Seçili çöl değerini birlikte başlığı içeren bir hücrenin ile sol tarafta bir tablo gösterir.

 [![](images/image4.png "Bu ekran ayrıntı ekranın sağ taraftaki tatlı, seçili çöl değerini birlikte başlığı içeren bir hücrenin sol tarafta bir tablo gösteren") ](images/image4.png#lightbox) [ ![ ] (images/image5.png "bu Aşağıdaki ekran görüntüsünde bir tablo ayrıntı ekranın sağ taraftaki tatlı, seçili çöl değerini birlikte başlığı içeren bir hücrenin ile sol tarafta gösterir")](images/image5.png#lightbox)

Kök öğe ayrıca bölümler içinde yeni bir iç içe geçmiş yapılandırma sayfa yüklenirken tetiklemek için yukarıda gösterildiği gibi kullanılabilir. Bu modda kullanıldığında sağlanan resim yazısı bir bölüm içinde oluşturulması sırasında kullanılır ve başlık olarak için alt sayfa de kullanılır. Örneğin:

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

Yukarıdaki örnekte, kullanıcı "Tatlı üzerinde" dokunur zaman MonoTouch.Dialog yeni bir sayfa oluşturur ve ona "Tatlı" olmasına ve üç değerden radyo grubuyla sahip kök ile gidin.

Biz "2" değeri RadioGroup geçirilen çünkü belirli Bu örnekte, "Tatlı" bölümündeki "çikolatalı kek" radyo grubu seçin. Başka bir deyişle, 3. öğe listede (sıfır-dizin) seçin.

Add yöntemi çağırma veya C# 4 Başlatıcısı sözdizimi kullanılarak bölümleri ekler.
INSERT yöntemi, bir animasyon bölümlerle eklemek için sağlanır.

(Yerine bir RadioGroup) bir grup örnekle RootElement oluşturursanız bir bölümde görüntülendiğinde RootElement Özet değeri tüm BooleanElements ve Group.Key değeri aynı anahtara sahip CheckboxElements toplu sayısı olacaktır.

### <a name="sections"></a>Bölümler

Bölümler için Grup öğeleri ekranında kullanılır ve bunlar RootElement yalnızca geçerli doğrudan alt. Bölüm yeni RootElements dahil olmak üzere standart öğelerini içerebilir.

Bir bölümü katıştırılmış RootElements için yeni biliyor gitmek için kullanılır.

Bölümü, dize olarak veya UIViews olarak üstbilgiler ve altbilgiler olabilir.
Dizeler genellikle yalnızca kullanır, ancak özel Uı'lar oluşturmak için tüm UIView üstbilgisinde veya altbilgisinde kullanabilirsiniz. Şu şekilde oluşturmak için bir dize ya da kullanabilirsiniz:

```csharp
var section = new Section ("Header", "Footer")
```

Görünümleri kullanmak için görünümleri oluşturucuya geçirmeniz yeterlidir:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Bildirim

#### <a name="handling-nsaction"></a>NSAction işleme

YÜKSEKLİĞİNDEKİ D yüzeyleri bir `NSAction` geri çağırmaları işlemek için temsilci olarak.
Örneğin, yüksekliğindeki tarafından oluşturulan bir tablo hücresi için touch olayını işlemek istediğiniz söyleyin D. Bir öğe ile yüksekliğindeki oluştururken Aşağıda gösterildiği gibi bir geri çağırma işlevini D, yalnızca sağlar:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Öğesi değeri alınıyor

Birlikte `Element.Value` özelliği, geri çağırma diğer öğeler ayarlanan değer alabilir. Örneğin, aşağıdaki kodu göz önünde bulundurun:

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

Bu kod, aşağıda gösterildiği gibi bir kullanıcı Arabirimi oluşturur. Bu örnek eksiksiz bir anlatım için bkz: [öğeleri API izlenecek](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) Öğreticisi.

 [![](images/image6.png "Element.Value özelliğiyle birlikte, geri çağırma diğer öğeler ayarlanan değer alabilirsiniz")](images/image6.png#lightbox)

Kullanıcı alt tablo hücresi bastığında kodu anonim işlevinde, değerinden yazma yürütür `element` için örnek **uygulama çıktısı** Mac için Visual Studio'da paneli

## <a name="built-in-elements"></a>Yerleşik öğeleri

YÜKSEKLİĞİNDEKİ D bir öğe olarak bilinen yerleşik tablo hücre öğe sayısı ile birlikte gelir.
Bu öğeler, tablo hücreleri dizeler, float, tarihleri ve hatta görüntüler, yalnızca birkaç adlandırmak için gibi çeşitli farklı türlerini görüntülemek için kullanılır. Her öğe veri türü uygun şekilde görüntüleme mvc'deki. Örneğin, bir boolean öğesi değerini geçiş yapmak için bir anahtar görüntüler. Benzer şekilde, bir float öğesi float değeri değiştirmek için kaydırıcıyı görüntüler.

Görüntüleri ve html gibi daha zengin veri türlerini desteklemek için daha karmaşık öğe daha vardır. Örneğin, bir web sayfası seçildiğinde yüklemek için bir UIWebView açar, bir html öğesi bir resim yazısı tablo hücresinde görüntüler.

### <a name="working-with-element-values"></a>Öğe değerlerle çalışma

Kullanıcı girişi yakalamak için kullanılan öğelerin genel kullanıma `Value` herhangi bir zamanda geçerli öğenin değerini tutan özelliği. Uygulama kullanıcının kullandığı gibi otomatik olarak güncelleştirilir.

MonoTouch.Dialog parçası olan tüm öğeleri için davranış budur ancak kullanıcı tarafından oluşturulan öğeleri için gerekli değildir.

### <a name="string-element"></a>Dize öğesi

A `StringElement` tablo hücresi ve hücrenin sağ tarafında dize değeri sol tarafındaki resim yazısı gösterir.

 [![](images/image7.png "Tablo hücresi ve hücrenin sağ tarafında dize değeri sol tarafındaki resim yazısı bir StringElement gösterir")](images/image7.png#lightbox)

Kullanılacak bir `StringElement` düğmesi olarak bir temsilci sağlar.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "Bir StringElement düğme olarak kullanmak üzere bir temsilci sağlar.")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Stilde dize öğesi

A `StyledStringElement` ya da yerleşik tablo hücre stilleri kullanarak sunulacak dizeleri izin verir veya özel biçimlendirmeye sahip.

 [![](images/image9.png "Bir StyledStringElement ya da yerleşik tablo hücre stilleri kullanarak sunulacak dizeleri izin verir veya özel biçimlendirme")](images/image9.png#lightbox)

`StyledStringElement` Sınıfı türer `StringElement`, ancak sağlar geliştiriciler yazı tipi, metin rengi, arka plan hücre rengini, satır sonu modu, görüntülenecek satır sayısı gibi özellikleri sayıda özelleştirebilir ve olup bir donatıyı görüntülenmesi.

### <a name="multiline-element"></a>Çok satırlı öğesi

 [![](images/image10.png "Çok satırlı öğesi")](images/image10.png#lightbox)

### <a name="entry-element"></a>Girişi öğesi

`EntryElement`, Adından da anlaşılacağı, kullanıcı girişi almak için kullanılır. Normal dizeleri veya parolaları, burada karakter gizli destekler.

 [![](images/image11.png "EntryElement kullanıcı girişi almak için kullanılır")](images/image11.png#lightbox)

Üç değerlerle başlatılır:

-  Kullanıcıya gösterilecek giriş için resim yazısı.
-  Yer tutucu metin (kullanıcıya bir ipucu sağlar gri çıkış metni budur). 
-  Metin değeri.


Yer tutucu ve değeri null olabilir. Ancak, resim yazısını gereklidir.

Herhangi bir noktada, değer özellik erişimi değerini alabilir `EntryElement`.

Ayrıca `KeyboardType` özelliği, klavye türü veri girişi için istenen stil oluşturma zamanında ayarlanabilir. Bu değerleri kullanılarak klavye yapılandırmak için kullanılabilir `UIKeyboardType` aşağıda listelenen:

-  sayısal
-  Telefon
-  URL
-  E-posta


### <a name="boolean-element"></a>Boole öğesi

 [![](images/image12.png "Boole öğesi")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Onay kutusu öğesi

 [![](images/image13.png "Checkbox Element")](images/image13.png#lightbox)

### <a name="radio-element"></a>Radyo öğesi

A `RadioElement` gerektiren bir `RadioGroup` içinde belirtilen `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "Bir RadioElement bir RadioGroup RootElement belirtilmesini gerektirir")](images/image14.png#lightbox)

 `RootElements` radyo öğeleri koordine etmek için de kullanılır. `RadioElement` Üyeleri (örneğin sistem zil halkası ton Seçici ve ayrı özel halkası tonlarını benzer bir şey uygulamak) birden çok bölüm yayılabilir. Özet görünümü şu anda seçili radyo öğesi gösterir. Bunu kullanmak için Oluştur `RootElement` şöyle Grup Oluşturucusu ile:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Grup adı `RadioGroup` seçili değer içeren bir sayfa (varsa) ve bu durumda sıfırsa, değeri olan ilk seçilen öğenin dizini göstermek için kullanılır.

### <a name="badge-element"></a>Gösterge öğesi

 [![](images/image15.png "Gösterge öğesi")](images/image15.png#lightbox)

### <a name="float-element"></a>Öğesi float

 [![](images/image16.png "Öğesi float")](images/image16.png#lightbox)

### <a name="activity-element"></a>Etkinlik öğesi

 [![](images/image17.png "Etkinlik öğesi")](images/image17.png#lightbox)

### <a name="date-element"></a>Tarih öğesi

 ![](images/image18.png "Tarih öğesi")

DateElement karşılık gelen hücre seçildiğinde, tarih seçici aşağıda gösterildiği gibi sunulur:

 [![](images/image19.png "DateElement karşılık gelen hücre seçildiğinde, tarih seçici gösterildiği gibi sunulur")](images/image19.png#lightbox)

### <a name="time-element"></a>Time öğesi

 [![](images/image20.png "Time öğesi")](images/image20.png#lightbox)

TimeElement karşılık gelen hücre seçildiğinde, aşağıda gösterildiği gibi bir saat Seçici sunulur:

 [![](images/image21.png "TimeElement karşılık gelen hücre seçildiğinde, gösterildiği gibi bir saat Seçici sunulur")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime öğesi

 [![](images/image22.png "DateTime öğesi")](images/image22.png#lightbox)

DateTimeElement karşılık gelen hücre seçildiğinde, aşağıda gösterildiği gibi bir datetime Seçici sunulur:

 [![](images/image23.png "DateTimeElement karşılık gelen hücre seçildiğinde, gösterildiği gibi bir datetime Seçici sunulur")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML öğesi

 [![](images/image24.png "HTML öğesi")](images/image24.png#lightbox)

`HTMLElement` Değerini görüntüler kendi `Caption` tablo hücresinde özelliği. Seçili Microsoft `Url` öğesine atanan içinde yüklü olduğu bir `UIWebView` denetim aşağıda gösterildiği gibi:

 [![](images/image25.png "Microsoft seçili, öğeye atanan Url aşağıda gösterildiği gibi bir UIWebView denetiminde yüklendi")](images/image25.png#lightbox)

### <a name="message-element"></a>İleti öğesi

 [![](images/image26.png "İleti öğesi")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Daha fazla öğe yükleme

Daha fazla öğe listesinde yük yapmalarına izin vermek için bu öğeyi kullanın. Normal ve yükleme resim yazıları yanı sıra, yazı tipi ve metin rengini özelleştirebilirsiniz.
`UIActivity` Göstergesi başlatır animasyon ekleme ve bir kullanıcı hücrenin dokunur yükleme resim yazısı görüntülenir ve ardından `NSAction` içine geçirilen Oluşturucusu yürütülür. Bir kez kodunuzda `NSAction` tamamlandı, `UIActivity` göstergesi durdurur animasyon ve normal resim yazısı yeniden görüntülenir.

### <a name="uiview-element"></a>UIView öğesi

Ayrıca, herhangi bir özel `UIView` kullanılarak görüntülenen `UIViewElement`.

### <a name="owner-drawn-element"></a>Sahip tarafından çizilmiş öğesi

Bir Özet sınıf olduğundan bu öğe sınıflandırma gerekir. Geçersiz kılmalısınız `Height(RectangleF bounds)` içinde döndürmelidir, öğenin yüksekliğini yöntemi yanı `Draw(RectangleF bounds, CGContext context, UIView view)` içinde hangi verilen sınırları içinde tüm özelleştirilmiş çizim bağlamını ve görünüm parametrelerini kullanarak yapmanız gerekir. Bu öğe sınıflara, ağır lifting mu bir `UIView`ve, yalnızca iki basit geçersiz kılmalarını gerek bırakarak döndürülen hücresine yerleştirme. Örnek uygulamasında daha iyi bir örnek uygulamasında gördüğünüz `DemoOwnerDrawnElement.cs` dosya.

Çok basit bir sınıf uygulama örneği şöyledir:

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

### <a name="json-element"></a>JSON Element

`JsonElement` Sınıfıdır `RootElement` genişleten bir `RootElement` yerel veya uzak bir URL'den iç içe alt içeriğini yüklemek için.

`JsonElement` Olan bir `RootElement` iki biçimde oluşturulabilir. Bir sürümünü oluşturur bir `RootElement` isteğe bağlı içerik yükler. Bunlar kullanılarak oluşturulan `JsonElement` içeriği yüklemek için url sonunda fazladan bir değişken Al oluşturucular:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Yerel bir dosya veya var olan verileri diğer formu oluşturur `System.Json.JsonObject` Ayrıştırılan:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

JSON yüksekliğindeki ile kullanma hakkında daha fazla bilgi için D, bkz: [JSON öğesi izlenecek](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) Öğreticisi.

## <a name="other-features"></a>Diğer özellikler

### <a name="pull-to-refresh-support"></a>Çekme yenileme desteği

 *Çekme-için-* *yenileme* görsel bir efekt başlangıçta bulunur *Tweetie2* pek çok uygulama arasında popüler bir etkisi hale geldi uygulama.

İletişim kutuları için otomatik çekme yenileme desteği eklemek için yalnızca iki şey yapmanız gerekir: kullanıcı veri çeker geldiğinde bildirim almak için bir olay işleyicisi kanca oluşturur ve bildirim `DialogViewController` ne zaman veri yüklendiğinde varsayılan durumuna geri gidin.

Bir bildirim ayarlayalım takma basit bir işlemdir; bağlamanız yeterlidir `RefreshRequested` olayda `DialogViewController`, şöyle:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Sonra da yönteminizi `OnUserRequestedRefresh`, bazı veri yükleme sırası, bazı veriler net istek veya veri işlem için bir iş parçacığı döndür. Veriler yüklendikten sonra üzerindedir `DialogViewController` yeni veriler ve görünüm varsayılan durumuna geri yüklemek için çağırarak bunu `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Arama desteği

Arama destekleyecek şekilde ayarlanamıyor `EnableSearch` özelliği, `DialogViewController`. Ayrıca ayarlayabilirsiniz `SearchPlaceholder` özelliği arama çubuğunda filigran metni olarak kullanın.

Arama Görünümü içeriğini kullanıcı türleri olarak değiştirir. Görünür alanları arar ve bu kullanıcıya gösterilir. `DialogViewController` Programlı olarak başlatmak, sonlandırma veya sonuçları üzerinde yeni bir filtre işlemi tetiklemek için üç yöntem sunar. Bu yöntemleri aşağıda listelenmiştir:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


İstiyorsanız, bu davranışı değiştirebilirsiniz şekilde genişletilebilir bir sistemdir.

### <a name="background-image-loading"></a>Arka plan görüntü yükleme

MonoTouch.Dialog içerir [TweetStation](https://github.com/migueldeicaza/TweetStation) uygulamanın resim yükleyici. Bu resim yükleyici destekler önbelleğe alma arka plan görüntüleri yüklemek için kullanılan ve görüntünün yüklendiğinde kodunuzu bildirebilir.

Ayrıca, giden ağ bağlantıları sayısı da sınırlar.

Resim yükleyici uygulanan `ImageLoader` sınıfı, tüm yapmanız gereken, çağrı `DefaultRequestImage` yöntemi, URI'yı yüklemek istediğiniz görüntünün, yanı sıra bir örneğini sağlayın gerekir `IImageUpdated` olacağı arabirimi olduğunda çağrılan görüntü ha yüklenen s.

Örneğin aşağıdaki kod bir URL'den görüntüyü yükler bir `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader sınıfı, tüm bellekte önbelleğe alınmış olan resimleri serbest bırakmak istediğiniz zaman çağırabilirsiniz bir temizleme yöntemi gösterir. Geçerli kod 50 görüntüleri için bir önbelleğe sahiptir. Varsa (örneğin, 50 görüntüleri çok fazla olacağını çok büyük olacak şekilde görüntüleri beklediğiniz varsa) farklı önbellek boyutu kullanmak istediğiniz, yalnızca ImageLoader örnekleri oluşturun ve önbellekte tutulacağı istediğiniz resimlerinin sayısı geçirin.

## <a name="using-linq-to-create-element-hierarchy"></a>LINQ kullanarak öğe hiyerarşisi oluşturmak için

LINQ ve C# ' ın başlatma sözdizimi akıllı kullanımı LINQ öğesi hiyerarşisi oluşturmak için kullanılabilir. Örneğin, aşağıdaki kod bir ekran bazı dize diziler de oluşturur ve tanıtıcıları hücre seçimi ayırır geçirilen adsız bir işlev aracılığıyla `StringElement`:

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

Bu kolayca bir XML veri deposu veya verileri neredeyse tamamen verilerden karmaşık uygulamalar oluşturmak için bir veritabanı ile birleştirilir.

## <a name="extending-mtd"></a>Yüksekliğindeki genişletme D

### <a name="creating-custom-elements"></a>Özel öğeleri oluşturma

Kendi öğe herhangi birinden var olan bir öğeyi devralma veya öğesi kök sınıfından türetilen oluşturabilirsiniz.

Kendi öğesi oluşturmak için aşağıdaki yöntemleri geçersiz kılmak istediğiniz:

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

Öğenizde değişken bir boyuta varsa uygulamanız gereken `IElementSizing` arabiriminin bir yöntem içerir:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Uygulama üzerinde planlıyorsanız, `GetCell` yöntemini çağırarak `base.GetCell(tv)` ve ayrıca geçersiz kılmanız gerekiyorsa döndürülen hücre özelleştirme, `CellKey` , öğesi için benzersiz bir anahtar döndürmek için özellik şöyle:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Bu öğelerin çoğu için ancak için çalışır `StringElement` ve `StyledStringElement` olanlar çeşitli işleme senaryolar için kendi anahtarları kümesini kullanın. Bu sınıfların kodda çoğaltılması gerekir.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Yansıma ve öğeleri API aynı kullanmak `DialogViewController`. Bazen görünümü görünümünü özelleştirmek istediğiniz veya bazı özellikleri kullanmak isteyebilirsiniz `UITableViewController` Uı'lar temel oluşturma gidin.

`DialogViewController` Yalnızca sınıfıdır `UITableViewController` ve özelleştirme aynı şekilde özelleştirin bir `UITableViewController`.

Örneğin, aşağıdakilerden biri olması için liste stilini değiştirmek istiyorsanız `Grouped` veya `Plain`, böyle denetleyicisini oluşturduğunuzda özelliğini değiştirerek bu değer ayarlayabilirsiniz:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Daha fazla bilgi için özelleştirmeleri Gelişmiş `DialogViewController`, kendi arka plan ayarlama gibi bir alt kümesi ve geçersiz kılma uygun yaptığınız yöntemleri, aşağıdaki örnekte gösterildiği gibi:

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

Aşağıdaki sanal yöntemlere başka bir özelleştirme noktasıdır `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Bu yöntem öğesinin bir alt kümesi döndürmelidir `DialogViewController.Source` Burada, hücre eşit boyutta durumlarda veya bir alt sınıfı için `DialogViewController.SizingSource` , hücre düzensiz varsa.

Bu geçersiz kılma herhangi birini yakalamak için kullanabileceğiniz `UITableViewSource` yöntemleri. Örneğin, [TweetStation](https://github.com/migueldeicaza/TweetStation) bu kullanıcı için üst kaydırılan olduğunda izlemek ve buna göre güncelleştirme okunmamış tweet'leri sayısı için kullanır.

## <a name="validation"></a>Doğrulama

Öğeleri doğrulama kendilerini web sayfaları için uygundur modelleri olarak sağlamaz ve Masaüstü uygulamaları doğrudan iPhone etkileşim modeliyle eşlemek değil.

Veri doğrulama yapmak istiyorsanız, kullanıcı bir eylem girilen verilerle harekete geçirdiğinde bunu yapmanız gerekir. Örneğin bir <span class="ui">Bitti</span> veya <span class="ui">sonraki</span> üst araç veya bazı düğmesine `StringElement` sonraki aşamaya gitmek için bir düğme olarak kullanılır.

Burada temel giriş doğrulaması gerçekleştirecek ve belki de daha fazla kullanıcı/parola bileşimi sunucusuyla geçerliliğini denetleme gibi doğrulama karmaşık budur.

Bir hata kullanıcıya bildirmek nasıl belirli bir uygulamadır. Açılır bir `UIAlertView` veya İpucu Göster.

## <a name="summary"></a>Özet

Bu makalede ele alınan çok sayıda MonoTouch.Dialog hakkında bilgi. Nasıl temelleri ele alınan yüksekliğindeki D çalışır ve yüksekliğindeki oluşturan çeşitli bileşenler ele D. Ayrıca çeşitli öğeleri ve tablo özelleştirmeleri yüksekliğindeki tarafından desteklenen gösterdi D ve nasıl ele yüksekliğindeki D ile özel öğeleri genişletilebilir. Ayrıca yüksekliğindeki JSON desteği açıklanmıştır Öğeleri dinamik olarak JSON öğesinden oluşturulması sağlar D.


## <a name="related-links"></a>İlgili bağlantılar

- [Ekran kaydı - Miguel de Icaza iOS oturum açma ekranı MonoTouch.Dialog ile oluşturur](http://youtu.be/3butqB1EG0c)
- [Ekran kaydı - iOS kullanıcı arabirimleri MonoTouch.Dialog ile kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [İzlenecek Yol: Öğeler API’sini kullanarak uygulama oluşturma](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [İzlenecek Yol: Yansıma API’sini kullanarak uygulama oluşturma](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [İzlenecek Yol: Kullanıcı Arabirimi oluşturmak için bir JSON Öğesini Kullanma](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON biçimlendirme](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
