---
title: Yansıma API'sini kullanarak bir Xamarin.iOS uygulaması oluşturma
description: Bu belgede MonoTouch.Dialog öznitelik tabanlı yansıma sınıfları öznitelikleri ile donatılmış temel kullanıcı Arabirimi oluşturur API açıklanmaktadır.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: a1b77f46410ef20892485a866221bab2c872e54c
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038478"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Yansıma API'sini kullanarak bir Xamarin.iOS uygulaması oluşturma

MT'nin D yansıma API'si sınıfları olmasını sağlar, MT'nin öznitelikleri ile donatılmış Ekranlar otomatik olarak oluşturmak için D kullanır. Yansıma API'si, bu sınıflar arasındaki ekranda görüntülenen bir bağlama sağlar. Bu API, API öğeleri yapan hassas bir denetim sağlamaz olsa da, sınıf decoration üzerinde temel öğe hiyerarşisi kullanıma otomatik olarak oluşturarak karmaşıklığını azaltır.

## <a name="setting-up-mtd"></a>MT'nin ayarlama D

MT'NİN D Xamarin.iOS ile dağıtılır. Bunu kullanmak için sağ **başvuruları** bir Xamarin.iOS düğümünün Mac için Visual Studio 2017 veya Visual Studio'da proje ve bir başvuru ekleyin **MonoTouch.Dialog 1** derleme. Ardından, ekleme `using MonoTouch.Dialog` kaynağınızın tablolarda kod gerektiğinde.

## <a name="getting-started-with-the-reflection-api"></a>Yansıma API'si ile çalışmaya başlama

Yansıma API'sini kullanarak kadar kolaydır:

1.  MT'nin ile donatılmış bir sınıf oluşturma D öznitelikleri.
1.  Oluşturma bir `BindingContext` örneği, yukarıdaki sınıfının bir örneğini geçirerek. 
1.  Oluşturma bir `DialogViewController` , iletmeden `BindingContext’s` `RootElement` . 


Yansıma API'sini kullanmayı gösteren bir örneğe göz atalım. Bu örnekte, aşağıda gösterildiği gibi basit veri girişi ekranı oluşturacağız:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "Bu örnekte, bir basit veri girişi ekranı aşağıda gösterildiği gibi oluşturacağız")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>MT'nin ile bir sınıf oluşturma D öznitelikleri

Yansıma API'sini kullanmak için ihtiyacımız olan ilk şey, öznitelikleri ile donatılmış bir sınıftır. Bu öznitelikler, MT'nin tarafından kullanılacak Dahili olarak oluşturmak için D öğeleri API'den nesneleri. Örneğin, aşağıdaki sınıf tanımını göz önünde bulundurun:

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

`SectionAttribute` Bölümlerinde sonuçlanır `UITableView` bölüm başlığı doldurmak için kullanılan dize bağımsız değişkeni ile oluşturuluyor. Bir bölüm bildirildiğinde, başka bir bölüme bildirilen kadar bu bölümde, takip eden her bir alan eklenecektir.
Alan için oluşturulan kullanıcı arabirimi öğesi türü, alanın türünü ve MT'nin temel bağlıdır. Bunu dekorasyon D özniteliği.

Örneğin, `Name` alan bir `string` ve ile donatılmış bir `EntryAttribute`. Bir metin girişi alanı ve belirtilen başlık tabloya eklenmekte olan bir satır sonuçlanır. Benzer şekilde, `IsApproved` alan bir `bool` ile bir `CheckboxAttribute`, sonuçta elde edilen tablo hücresi sağındaki onay kutusu ile bir tablo satırında. MT'NİN D bir öznitelikte belirtilmediğinden bu durumda, açıklamalı alt yazı oluşturmak için otomatik olarak bir alan ekleyerek alan adı kullanır.

## <a name="adding-the-bindingcontext"></a>BindingContext ekleme

Kullanılacak `Expense` sınıfı ihtiyacımız oluşturmak bir `BindingContext`. A `BindingContext` öğeleri hiyerarşisini oluşturmak için öznitelik sınıfından verileri bağlayacaksınız bir sınıftır. Oluşturmak için biz yalnızca bu örneği ve oluşturucuya öznitelikli sınıfı bir örneğini geçirin.

Örneğin, kullanılarak bildirilen kullanıcı Arabirimi eklemek için özniteliğini `Expense` sınıfında, aşağıdaki kodda dahil `FinishedLaunching` yöntemi `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Biz kullanıcı arabirimini oluşturmak için yapmanız gereken tek şey, sonra ekleyin `BindingContext` için `DialogViewController` ve olarak `RootViewController` penceresi, aşağıda gösterildiği gibi:

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

Şimdi uygulamayı çalıştıran görüntülenen yukarıda gösterilen ekran sonuçlanır.

### <a name="adding-a-uinavigationcontroller"></a>Bir UINavigationController ekleme

Ancak fark başlığı "bir görev oluşturmanızı" biz geçirilecek `BindingContext` görüntülenmez. Bunun nedeni, `DialogViewController` olduğu bir parçası olmayan bir `UINavigatonController`. Eklenecek kodu değiştirelim bir `UINavigationController` pencerenin olarak `RootViewController,` ve ekleme `DialogViewController` kökü olarak `UINavigationController` aşağıda gösterildiği gibi:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Şimdi biz uygulamayı çalıştırdığınızda, bir başlık görünür `UINavigationController’s` gezinti çubuğu olarak ekran görüntüsü aşağıda gösterilmiştir:

 [![](reflection-api-walkthrough-images/02-create-task.png "Şimdi biz uygulamayı çalıştırdığınızda, başlık UINavigationControllers Gezinti bölmesinde görünür")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Ekleyerek bir `UINavigationController`, MT'nin diğer özelliklerinden artık yapabileceğimiz D Gezinti gereklidir. Örneğin, sabit listesine ekleyebiliriz `Expense` gider ve MT'nin kategori tanımlamak için sınıfı D seçimi ekran otomatik olarak oluşturur. Göstermek için değiştirin `Expense` sınıfı eklemek için bir `ExpenseCategory` gibi alan:

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

Şimdi uygulamayı çalıştıran kategorisi için tablosunda yeni bir satıra gösterildiği sonuçlanır:

 [![](reflection-api-walkthrough-images/03-set-details.png "Şimdi uygulamayı çalıştıran kategorisi için tablosunda yeni bir satıra gösterildiği sonuçlanır")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Satır seçerek uygulama için yeni bir ekran satırları numaralandırma, karşılık gelen aşağıda gösterildiği gibi gezinme içinde sonuçlanır:

 [![](reflection-api-walkthrough-images/04-set-category.png "Satır seçerek uygulama için yeni bir ekran satırları numaralandırma karşılık gelen gezinme sonuçları")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Özet

Bu makalede, yansıma API'sini bir kılavuz sunulur. Biz özniteliklere görüntülenenler denetlemek için bir sınıf eklemek nasıl gösterilmiştir. Ayrıca nasıl kullanılacağı ele aldığımız bir `BindingContext` veri kullanımı MT'nin nasıl yanı sıra oluşturulan öğe hiyerarşisi bir sınıftan bağlamak için D bir `UINavigationController`.


## <a name="related-links"></a>İlgili bağlantılar

- [MTDReflectionWalkthrough (örnek)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Yayını - bir iOS oturum açma ekranı Miguel de Icaza, MonoTouch.Dialog ile oluşturur.](http://youtu.be/3butqB1EG0c)
- [Yayını - iOS kullanıcı arabirimleri ile MonoTouch.Dialog kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch iletişim giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Öğeleri API gözden geçirme](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON öğesi gözden geçirme](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation uygulama](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
