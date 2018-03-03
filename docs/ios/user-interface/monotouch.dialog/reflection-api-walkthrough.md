---
title: "İzlenecek yol: yansıma API kullanarak bir uygulama oluşturma"
description: "Öğeleri API yanı sıra, MonoTouch.Dialog (yüksekliğindeki D) bir öznitelik tabanlı yansıma API de içerir. Yansıma API yüksekliğindeki oluşturma ekranlar yapar D öznitelikleri sınıflarıyla dekorasyon olarak kadar kolay. Bu makale yansıma API'yi kullanarak bir uygulama oluşturmak nasıl gösteren aracılığıyla ilerlemesi sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 44df6fce4ec6d667c096da01cfc339ec2afdb077
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough-creating-an-application-using-the-reflection-api"></a>İzlenecek yol: yansıma API kullanarak bir uygulama oluşturma

_Öğeleri API yanı sıra, MonoTouch.Dialog (yüksekliğindeki D) bir öznitelik tabanlı yansıma API de içerir. Yansıma API yüksekliğindeki oluşturma ekranlar yapar D öznitelikleri sınıflarıyla dekorasyon olarak kadar kolay. Bu makale yansıma API'yi kullanarak bir uygulama oluşturmak nasıl gösteren aracılığıyla ilerlemesi sağlar._


Yüksekliğindeki D yansıma API sınıfları olmasını sağlar, yüksekliğindeki öznitelikleri ile donatılmış Ekranlar otomatik olarak oluşturmak için D kullanır. API yansıma bir bağlama bu sınıflar arasındaki ekranda görüntülenen sağlar. Bu API, API öğeleri mu hassas bir denetim sağlamaz rağmen sınıf decoration üzerinde temel öğesi hiyerarşi çıkışı otomatik olarak oluşturarak karmaşıklığını azaltır.

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>Yansıma API'si ile çalışmaya başlama

Yansıma API kullanarak kadar kolaydır:

1.  Yüksekliğindeki ile donatılmış bir sınıf oluşturma D öznitelikleri.
1.  Oluşturma bir `BindingContext` örneği, yukarıdaki sınıfının bir örneği geçirme. 
1.  Oluşturma bir `DialogViewController` , onu `BindingContext’s` `RootElement` . 


Yansıma API kullanmayı göstermek için bir örneğe bakalım. Bu örnekte, biz aşağıda gösterildiği gibi basit veri girişi ekran yapı:

 [ ![](reflection-api-walkthrough-images/01-expense-entry.png "Bu örnekte, biz basit veri girdi ekranını aşağıda gösterildiği gibi yapı")](reflection-api-walkthrough-images/01-expense-entry.png)

 <a name="Creating_a_Class_with_MT.D_Attributes" />


## <a name="creating-a-class-with-mtd-attributes"></a>Bir sınıf yüksekliğindeki ile oluşturma D öznitelikleri

Biz yansıma API kullanmak için gereken ilk şey, öznitelikleri ile donatılmış bir sınıftır. Bu öznitelikler yüksekliğindeki tarafından kullanılır Dahili olarak oluşturmak için D öğeleri API'SİNDEN nesneleri. Örneğin, aşağıdaki sınıf tanımına göz önünde bulundurun:

```csharp
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
}
```

`SectionAttribute` Bölümlerinde sonuçlanır `UITableView` bölümün başlığını doldurmak için kullanılan dize bağımsız değişkeni ile oluşturuldu. Bir bölüm bildirildiğinde, başka bir bölüme bildirilmiş kadar bu bölümde, kendisini izleyen her alan eklenecektir.
Alanın türü ve yüksekliğindeki temel alan için oluşturulan kullanıcı arabirimi öğesi türü bağlıdır Bunu dekorasyon D özniteliği.

Örneğin, `Name` alan bir `string` ve ile donatılmış bir `EntryAttribute`. Bu, bir metin giriş alanı ve belirtilen resim yazısı tabloyla eklenmekte olan bir satır sonuçlanır. Benzer şekilde, `IsApproved` alan bir `bool` ile bir `CheckboxAttribute`, tablo hücresi sağında bir onay kutusu içeren bir tablo satırı sonuç. YÜKSEKLİĞİNDEKİ D bir öznitelikte belirtilmediğinden resim yazısını bu durumda, oluşturmak için otomatik olarak bir boşluk ekleyerek alan adını kullanır.

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>Bindingparameters'te ekleme

Kullanılacak `Expense` sınıfı, ihtiyacımız oluşturmak bir `BindingContext`. A `BindingContext` veri öğeleri hiyerarşisini oluşturmak için öznitelikli sınıfından bağlayacaksınız bir sınıftır. Oluşturmak için biz yalnızca bu örneği ve öznitelikli sınıfının bir örneğini oluşturucuya geçirin.

Örneğin, kullanılarak bildirilen UI eklemek için özniteliğini `Expense` sınıfında, aşağıdaki kodda dahil `FinishedLaunching` yöntemi `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Tüm kullanıcı arabirimini oluşturmak için sahip olduğumuz ardından ekleyin `BindingContext` için `DialogViewController` olarak ayarlayın `RootViewController` aşağıda gösterildiği gibi penceresinin:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
   
        window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        var expense = new Expense ();
        var bctx = new BindingContext (null, expense, "Create a task");
        var dvc = new DialogViewController (bctx.Root);
            
        window.RootViewController = dvc;
        window.MakeKeyAndVisible ();
            
        return true;
}
```

Uygulamayı şimdi çalıştıran görüntülenen yukarıda gösterildiği ekran sonuçlanır.

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>Bir UINavigationController ekleme

Ancak fark başlığı "bir görev oluşturma," biz geçirilecek `BindingContext` görüntülenmez. Bunun nedeni, `DialogViewController` olan bir parçası olmayan bir `UINavigatonController`. Eklemek için kodu değiştirelim bir `UINavigationController` pencere olarak `RootViewController,` ve ekleme `DialogViewController` kökü olarak `UINavigationController` aşağıda gösterildiği gibi:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Biz uygulamayı çalıştırdığınızda, başlık görünür artık `UINavigationController’s` gezinti çubuğu olarak ekran görüntüsü aşağıda gösterilmiştir:

 [ ![](reflection-api-walkthrough-images/02-create-task.png "Şimdi biz uygulamayı çalıştırdığınızda, başlık UINavigationControllers gezinti çubuğunda görünür.")](reflection-api-walkthrough-images/02-create-task.png)

Ekleyerek bir `UINavigationController`, biz şimdi yüksekliğindeki diğer özelliklerini yararlanabilir D Gezinti gereklidir. Örneğin, bir numaralandırma ekleyebiliriz `Expense` gider ve yüksekliğindeki kategori tanımlamak için sınıfı D seçimi ekran otomatik olarak oluşturur. Göstermek için değiştirme `Expense` eklemek için sınıfı bir `ExpenseCategory` gibi alan:

```csharp
public enum Category
{
        Travel,
        Lodging,
        Books
}
        
public class Expense
{
        …

        [Caption("Category")]
        public Category ExpenseCategory;
}
```

Uygulamayı şimdi çalıştıran kategorisinin tablosunda yeni bir satırı gösterildiği gibi sonuçlanır:

 [ ![](reflection-api-walkthrough-images/03-set-details.png "Uygulamayı şimdi çalıştıran kategori tablosunda yeni bir satıra gösterildiği gibi sonuçları")](reflection-api-walkthrough-images/03-set-details.png)

Satır seçilmesi, uygulama için yeni bir ekran numaralandırma, karşılık gelen satırlarla aşağıda gösterildiği gibi gezinme içinde sonuçları:

 [ ![](reflection-api-walkthrough-images/04-set-category.png "Uygulama için yeni bir ekran satır numaralandırma karşılık gelen gezinme içinde sonuçlarını satır seçme")](reflection-api-walkthrough-images/04-set-category.png)

 <a name="Summary" />


## <a name="summary"></a>Özet

Bu makalede bir kılavuz yansıma API sunulmuştur. Biz görüntülenenleri denetlemek için bir sınıf için öznitelikler eklemek nasıl oluşturulacağını gösterir. Biz de kullanmak nasıl ele alınan bir `BindingContext` veri kullanımı yüksekliğindeki nasıl yanı sıra oluşturulan öğesi hiyerarşi bir sınıftan bağlamak için D ile birlikte bir `UINavigationController`.


## <a name="related-links"></a>İlgili bağlantılar

- [MTDReflectionWalkthrough (örnek)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Ekran kaydı - Miguel de Icaza iOS oturum açma ekranı MonoTouch.Dialog ile oluşturur](http://youtu.be/3butqB1EG0c)
- [Ekran kaydı - iOS kullanıcı arabirimleri MonoTouch.Dialog ile kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch iletişim giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Öğeleri API gözden geçirme](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON öğesini gözden geçirme](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation Application](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
