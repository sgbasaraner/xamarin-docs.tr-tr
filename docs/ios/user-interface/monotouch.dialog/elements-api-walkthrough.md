---
title: "İzlenecek yol - öğeleri API kullanarak bir uygulama oluşturma"
description: "Bu makalede giriş MonoTouch iletişim makale için sunulan bilgiler temel oluşturur. MonoTouch.Dialog (yüksekliğindeki kullanmayı gösterir bir kılavuz gösterir D) hızlı bir şekilde yüksekliğindeki sahip bir uygulama oluşturmaya başlamak için öğeleri API D."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 19e20015d1872cbaea21dd8b8e5431981e463c33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>İzlenecek yol - öğeleri API kullanarak bir uygulama oluşturma

_Bu makalede giriş MonoTouch iletişim makale için sunulan bilgiler temel oluşturur. MonoTouch.Dialog (yüksekliğindeki kullanmayı gösterir bir kılavuz gösterir D) hızlı bir şekilde yüksekliğindeki sahip bir uygulama oluşturmaya başlamak için öğeleri API D._

Bu kılavuzda, yüksekliğindeki kullanacağız. Görev listesini görüntüleyen uygulamanın ana-ayrıntı stil oluşturmak için D öğeleri API. Kullanıcı seçtiğinde <span class="ui"> + </span> düğmesi gezinti çubuğunda görev tablosuna yeni bir satır eklenir. Satır seçme görev açıklaması ve son tarih, güncelleştirme kurmamızı aşağıda gösterildiği gibi sağlayan ayrıntı ekranına gidin:

 [ ![](elements-api-walkthrough-images/01-task-list-app.png "Satır seçerek görev açıklaması ve son tarih güncelleştirme kurmamızı sağlayan ayrıntı ekran gideceği")](elements-api-walkthrough-images/01-task-list-app.png)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>Öğeleri API gözden geçirme

İçinde [MonoTouch iletişim giriş](~/ios/user-interface/monotouch.dialog/index.md) makale, biz kazanılan yüksekliğindeki farklı parçalarının düz anlama D. Öğeleri API bunları tüm bir uygulamayı bir araya için kullanalım.

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>Çok ekran uygulamasını ayarlama

Ekran oluşturma işlemini başlatmak için MonoTouch.Dialog oluşturur bir `DialogViewController`ve ardından ekler bir `RootElement`.

MonoTouch.Dialog ile çok ekran uygulaması oluşturmak için kimliğinizi gerekiyor:

1.  Oluşturma bir  `UINavigationController.`
1.  Oluşturma bir  `DialogViewController.`
1.  Ekleme `DialogViewController` kökü olarak  `UINavigationController.` 
1.  Ekleme bir `RootElement` için  `DialogViewController.`
1.  Ekleme `Sections` ve `Elements` için  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>Bir UINavigationController kullanma

Bir gezinme stili uygulaması oluşturmak için oluşturmamız gerekir bir `UINavigationController`ve ardından olarak ekleyin `RootViewController` içinde `FinishedLaunching` yöntemi `AppDelegate`. Yapmak için `UINavigationController` iş MonoTouch.Dialog ile eklediğimiz bir `DialogViewController` için `UINavigationController` aşağıda gösterildiği gibi:

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

Yukarıdaki kod örneğini oluşturur bir `RootElement` ve içine geçirir `DialogViewController`. `DialogViewController` Her zaman bir `RootElement` kendi hiyerarşisinin üstünde. Bu örnekte, `RootElement` "Yapılacaklar başlık Gezinti denetleyicinin gezinti çubuğu olarak hizmet veren listesi," dizesi ile oluşturulur. Bu noktada, uygulama çalışan aşağıda gösterildiği ekran sunmak:

 [ ![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Uygulamayı çalıştıran burada gösterilen ekran sunacaktır")](elements-api-walkthrough-images/02-to-do-list-screen-.png)

MonoTouch.Dialog'ın hiyerarşik yapısını kullanmak nasıl görelim `Sections` ve `Elements` daha fazla ekranlar eklemek için.

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>İletişim ekranlar oluşturma

A `DialogViewController` olan bir `UITableViewController` ekranlar eklemek için MonoTouch.Dialog kullanan bir alt kümesi. MonoTouch.Dialog oluşturur ekranlar ekleyerek bir `RootElement` için bir `DialogViewController`, yukarıda gördüğümüz gibi. `RootElement` Olabilir `Section` tablo bölümlerini temsil örnekleri.
Bölümler öğelerden oluşur, diğer bölümler ve hatta diğer `RootElements`. İç içe geçme tarafından `RootElements`, MonoTouch.Dialog sonraki anlatıldığı gibi bir gezinti stil uygulama otomatik olarak oluşturur.

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>DialogViewController kullanma

`DialogViewController`, Olan bir `UITableViewController` alt sahip bir `UITableView` kendi görünüm olarak. Bu örnekte, her zaman tabloya öğeleri Ekle istiyoruz <span class="ui"> + </span> düğmesi dokunduğunuz. Bu yana `DialogViewController` eklendi bir `UINavigationController`, biz kullanabilirsiniz `NavigationItem`'s `RightBarButton` eklemek üzere özellik <span class="ui"> + </span> düğmesi, aşağıda gösterildiği gibi:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Ne zaman oluşturduğumuz `RootElement` daha önce biz bunu tek bir geçirilen `Section` biz öğeler olarak ekleyebilirsiniz örneğine <span class="ui"> + </span> düğmesi kullanıcı tarafından dokunduğunuz. Biz bu olay işleyicisi düğmesi için gerçekleştirmek için aşağıdaki kodu kullanabilirsiniz:

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

Bu kod yeni bir oluşturur `Task` nesne düğmesi dokunduğunuz her zaman. Aşağıdaki basit uyarlamasını gösterir `Task` sınıfı:

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

 []()

Görevin `Name` özelliği oluşturmak için kullanılır `RootElement`'s adlı bir sayaç değişken birlikte resim yazısı `n` yeni her görev için artar. MonoTouch.Dialog öğeleri eklenir satırları kapatır `TableView` olduğunda her `taskElement` eklenir.

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>Sunan ve iletişim ekranlar yönetme

Kullandık bir `RootElement` MonoTouch.Dialog otomatik olarak her görevin Ayrıntılar için yeni bir ekran oluşturma ve bir satır seçildiğinde kendisine gidin.

Görev Ayrıntıları ekran kendisini iki bölümden oluşur; Bu bölümlerde, tek bir öğe içeriyor. İlk öğe oluşturulur bir `EntryElement` görevin için düzenlenebilir bir satır sağlamak için `Description` özelliği. Öğe seçildiğinde, metin düzenleme için klavye aşağıda gösterildiği gibi sunulur:

 [ ![](elements-api-walkthrough-images/03-create-task.png "Öğe seçildiğinde, metin düzenleme için klavye gösterildiği gibi sunulur")](elements-api-walkthrough-images/03-create-task.png)

İkinci bölüm içeren bir `DateElement` bize görevin yönetmek sağlayan `DueDate` özelliği. Tarih otomatik olarak seçme tarih seçici gösterildiği gibi yüklenir:

 [ ![](elements-api-walkthrough-images/04-date-picker.png "Bir tarih seçici olarak yükleyen tarihi otomatik olarak seçme")](elements-api-walkthrough-images/04-date-picker.png)

Hem de `EntryElement` ve `DateElement` durumları (veya MonoTouch.Dialog herhangi bir veri girişi öğesi için) değerlerini değişiklikleri otomatik olarak korunur. Biz bu tarihi düzenleyerek ve ardından İleri ve geri kök ekran ve burada ayrıntı ekranlar değerleri korunur, çeşitli görev ayrıntıları arasında gezinme personeline gösterebilir.

 <a name="Summary" />


## <a name="summary"></a>Özet

Bu makalede MonoTouch.Dialog öğeleri API kullanma gösterdi bir kılavuz sunulmuştur. İle yüksekliğindeki çok ekran uygulaması oluşturmak için temel adımlar ele D nasıl kullanılacağı dahil olmak üzere, bir `DialogViewController` ve öğeler ve bölümler ekranlar oluşturmak için ekleme. Ayrıca, yüksekliğindeki kullanmak nasıl oluşturulacağını gösterir D ile birlikte bir `UINavigationController`.


## <a name="related-links"></a>İlgili bağlantılar

- [MTDWalkthrough (örnek)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Ekran kaydı - Miguel de Icaza iOS oturum açma ekranı MonoTouch.Dialog ile oluşturur](http://youtu.be/3butqB1EG0c)
- [Ekran kaydı - iOS kullanıcı arabirimleri MonoTouch.Dialog ile kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Yansıma API gözden geçirme](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON öğesini gözden geçirme](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation Application](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
