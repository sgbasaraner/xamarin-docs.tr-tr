---
title: Öğeler API'sini kullanarak bir Xamarin.iOS uygulaması oluşturma
description: Bu makalede, giriş MonoTouch iletişim makale bölümünde verilen bilgileri oluşturur. MonoTouch.Dialog (Colorado kullanmayı gösteren bir anlatım sunar D) hızlı bir şekilde MT'nin ile bir uygulama oluşturmaya başlamak için öğeleri API D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dcd6f1260be3414c515010c2fd615910c7b5c054
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038348"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Öğeler API'sini kullanarak bir Xamarin.iOS uygulaması oluşturma

_Bu makalede, giriş MonoTouch iletişim makale bölümünde verilen bilgileri oluşturur. MonoTouch.Dialog (Colorado kullanmayı gösteren bir anlatım sunar D) hızlı bir şekilde MT'nin ile bir uygulama oluşturmaya başlamak için öğeleri API D._

Bu kılavuzda, MT'nin kullanacağız Bir ana öğe-ayrıntı stilini görüntüleyen görev listesi uygulaması oluşturmak için D öğeleri API. Kullanıcı seçtiğinde <span class="ui"> + </span> düğmesi gezinti çubuğunda, görev için tabloya yeni bir satır eklenir. Satırı seçmek için aşağıda gösterildiği gibi görev açıklaması ve son tarih, güncelleştirilecek sağlıyor ayrıntı ekranı gider:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Görev açıklaması ve son tarihini güncelleştirmek sağlıyor ayrıntı ekranı için satır seçme gider")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 ## <a name="setting-up-mtd"></a>MT'nin ayarlama D

MT'NİN D Xamarin.iOS ile dağıtılır. Bunu kullanmak için sağ **başvuruları** bir Xamarin.iOS düğümünün Mac için Visual Studio 2017 veya Visual Studio'da proje ve bir başvuru ekleyin **MonoTouch.Dialog 1** derleme. Ardından, ekleme `using MonoTouch.Dialog` kaynağınızın tablolarda kod gerektiğinde.

## <a name="elements-api-walkthrough"></a>Öğeleri API gözden geçirme

İçinde [MonoTouch iletişim giriş](~/ios/user-interface/monotouch.dialog/index.md) makalesi, biz kazanılan MT'nin farklı bölümlerini düz bir anlayış D. Şimdi bunların tümünü bir araya bir uygulamaya koymak için öğeler API'sini kullanın.

## <a name="setting-up-the-multi-screen-application"></a>Çoklu ekran uygulamasını ayarlama

Ekran oluşturma işlemini başlatmak için MonoTouch.Dialog oluşturur bir `DialogViewController`ve ardından ekler bir `RootElement`.

MonoTouch.Dialog ile çoklu ekran bir uygulama oluşturmak için biz gerekir:

1.  Oluşturma bir `UINavigationController.`
1.  Oluşturma bir `DialogViewController.`
1.  Ekleme `DialogViewController` kökü olarak  `UINavigationController.` 
1.  Ekleme bir `RootElement` için  `DialogViewController.`
1.  Ekleme `Sections` ve `Elements` için  `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>Bir UINavigationController kullanma

Bir gezinti stil uygulama oluşturmak için oluşturmamız gerekir bir `UINavigationController`ve ardından olarak ekleyin `RootViewController` içinde `FinishedLaunching` yöntemi `AppDelegate`. Yapmak `UINavigationController` eklediğimiz MonoTouch.Dialog ile çalışmak bir `DialogViewController` için `UINavigationController` aşağıda gösterildiği gibi:

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
        _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        _rootElement = new RootElement ("To Do List"){new Section ()};

        // code to create screens with MT.D will go here …

        _rootVC = new DialogViewController (_rootElement);
        _nav = new UINavigationController (_rootVC);
        _window.RootViewController = _nav;
        _window.MakeKeyAndVisible ();
            
        return true;
}
```

Yukarıdaki kod örneği oluşturur. bir `RootElement` ve içine geçirir `DialogViewController`. `DialogViewController` Her zaman bir `RootElement` hiyerarşi üstünde. Bu örnekte, `RootElement` "Yapılacaklar Gezinti denetleyicinin Gezinti çubuğundaki başlığı olarak hizmet veren listesi," dizesi ile oluşturulur. Bu noktada, uygulamayı çalıştıran, aşağıda gösterilen ekran neden olabilir:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Uygulamayı çalıştıran burada gösterilen ekran sunar")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

MonoTouch.Dialog'ın hiyerarşik yapısını kullanmayı görelim `Sections` ve `Elements` daha fazla ekran ekleme.

### <a name="creating-the-dialog-screens"></a>İletişim ekranlar oluşturma

A `DialogViewController` olduğu bir `UITableViewController` ekranlarını eklemek için MonoTouch.Dialog kullanan alt sınıfı. MonoTouch.Dialog ekleyerek ekranlar oluşturur bir `RootElement` için bir `DialogViewController`yukarıda gördüğümüz gibi. `RootElement` Olabilir `Section` tablo bölümlerini gösteren örnekler.
Bölümler öğelerden oluşur, diğer bölümler ve hatta diğer `RootElements`. İç içe geçirerek `RootElements`, MonoTouch.Dialog sonraki anlatıldığı gibi bir gezinti stili uygulama otomatik olarak oluşturur.

### <a name="using-dialogviewcontroller"></a>DialogViewController kullanma

`DialogViewController`, Olan bir `UITableViewController` alt sahip bir `UITableView` kendi görünüm olarak. Tablonun her zaman öğeleri eklemek istediğimiz Bu örnekte, <span class="ui"> + </span> düğmeye dokunulduğunda. Bu yana `DialogViewController` eklenmiş bir `UINavigationController`, kullanabiliriz `NavigationItem`ait `RightBarButton` özelliği eklemek için <span class="ui"> + </span> düğmesi, aşağıda gösterildiği gibi:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Ne zaman oluşturduğumuz `RootElement` daha önce biz bunu tek bir geçirilen `Section` öğeleri olarak eklediğimiz böylece örnek <span class="ui"> + </span> düğmesi kullanıcı tarafından dokunulduğunda. Bu olay işleyicisi, düğmenin gerçekleştirmek için aşağıdaki kodu kullanabiliriz:

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

Bu kod yeni bir oluşturur `Task` nesne düğmeye dokunulduğunda her zaman. Aşağıdaki basit uygulamasını gösterir `Task` sınıfı:

```csharp
public class Task
{   
        public Task ()
        {
        }
        
        public string Name { get; set; }
        
        public string Description { get; set; }

        public DateTime DueDate { get; set; }
}
```

Görevin `Name` özelliği oluşturmak için kullanılan `RootElement`ait başlık adlı bir sayaç değişkeni birlikte `n` için yeni bir görev artırılır. MonoTouch.Dialog öğeleri eklenen satırları kapatır `TableView` olduğunda her `taskElement` eklenir.

## <a name="presenting-and-managing-dialog-screens"></a>Sunu ve iletişim ekranları yönetme

Kullandık bir `RootElement` MonoTouch.Dialog otomatik olarak her görevin ayrıntıları için yeni bir ekran oluşturun ve bir satır seçildiğinde bu sayfaya gidin.

Görev ayrıntı ekranı kendisini iki bölümden oluşur; Bu bölümlerin her birinde, tek bir öğe içeriyor. İlk öğeyi içinden oluşturulan bir `EntryElement` görevin için düzenlenebilir bir satır sağlamak `Description` özelliği. Öğe seçildiğinde, metin düzenleme klavye aşağıda gösterildiği gibi görüntülenir:

 [![](elements-api-walkthrough-images/03-create-task.png "Öğe seçildiğinde, metin düzenleme klavye gösterildiği sunulur")](elements-api-walkthrough-images/03-create-task.png#lightbox)

İkinci bölüm içeren bir `DateElement` bize görevin yönetme sağlayan `DueDate` özelliği. Tarih otomatik olarak seçilmesi, gösterildiği gibi bir tarih seçici yükler:

 [![](elements-api-walkthrough-images/04-date-picker.png "Otomatik olarak bir tarih seçerek bir tarih seçici olarak yükler")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

Hem de `EntryElement` ve `DateElement` durumları (veya herhangi bir veri girişi öğenin MonoTouch.Dialog), tüm değişiklikler için değerleri otomatik olarak korunur. Biz tarih düzenleyerek ve ardından kök ekran ve burada ayrıntı ekranları değerleri korunur, çeşitli görev ayrıntıları arasında ileri ve geri gezinme gösterebilirsiniz.

## <a name="summary"></a>Özet

Bu makalede MonoTouch.Dialog öğeleri API'SİNİN nasıl kullanılacağı gösterilmiştir bir kılavuz sunulur. Bir çoklu ekran uygulaması ile MT'nin oluşturmak için temel adımlar ele D, nasıl kullanılacağını dahil olmak üzere bir `DialogViewController` ve öğeler ve bölümler ekranlar oluşturmak için ekleme işlemleri açıklanmaktadır. Ayrıca, MT'nin kullanmak nasıl oluşturulacağını gösterir D ile birlikte bir `UINavigationController`.

## <a name="related-links"></a>İlgili bağlantılar

- [MTDWalkthrough (örnek)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Yayını - bir iOS oturum açma ekranı Miguel de Icaza, MonoTouch.Dialog ile oluşturur.](http://youtu.be/3butqB1EG0c)
- [Yayını - iOS kullanıcı arabirimleri ile MonoTouch.Dialog kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Yansıma API'si gözden geçirme](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON öğesi gözden geçirme](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation uygulama](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
