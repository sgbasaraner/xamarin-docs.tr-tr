---
title: Xamarin.iOS içinde bir kullanıcı arabirimi oluşturmak için JSON'ı kullanma
description: MonoTouch.Dialog (Colorado D) JSON verileri ile dinamik kullanıcı Arabirimi oluşturma için destek içerir. Bu öğreticide, uzak bir URL'den yüklü veya bir uygulamaya dahil edilen JSON bir kullanıcı arabirimi oluşturmak için bir JSONElement kullanmayı gösterilecektir.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 94cef78bb7eedc03192071f17af765ebb702e260
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038501"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Xamarin.iOS içinde bir kullanıcı arabirimi oluşturmak için JSON'ı kullanma

_MonoTouch.Dialog (Colorado D) JSON verileri ile dinamik kullanıcı Arabirimi oluşturma için destek içerir. Bu öğreticide, uzak bir URL'den yüklü veya bir uygulamaya dahil edilen JSON bir kullanıcı arabirimi oluşturmak için bir JSONElement kullanmayı gösterilecektir._

MT'NİN D JSON'da bildirilen oluşturma kullanıcı arabirimlerini destekler. JSON, MT'nin kullanarak bildirilen öğeler olduğunda D ilişkili öğeleri sizin için otomatik olarak oluşturur. JSON ya da bir ayrıştırılmış bir yerel dosyadan yüklenebilir `JsonObject` örneği veya uzak bir Url.

MT'NİN D JSON kullanırken öğeleri API'SİNDE kullanılabilir olan özelliklerin tam aralığını destekler. Örneğin, aşağıdaki ekran görüntüsünde uygulama tamamen JSON kullanarak bildirilmiştir:

[![](json-element-walkthrough-images/01-load-from-file.png "Örneğin, bu ekran görüntüsünde uygulama tamamen JSON kullanarak bildirilen") ](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ] (json-element-walkthrough-images/01-load-from-file.png "Örneğin bu ekran görüntüsünde uygulama tamamen kullanarak bildirildi JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Şimdi örnekten yeniden ziyaret [öğeleri API Kılavuzu](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) JSON'ı kullanarak bir görev ayrıntı ekranı ekleme gösteren öğretici.

## <a name="setting-up-mtd"></a>MT'nin ayarlama D

MT'NİN D Xamarin.iOS ile dağıtılır. Bunu kullanmak için sağ **başvuruları** bir Xamarin.iOS düğümünün Mac için Visual Studio 2017 veya Visual Studio'da proje ve bir başvuru ekleyin **MonoTouch.Dialog 1** derleme. Ardından, ekleme `using MonoTouch.Dialog` kaynağınızın tablolarda kod gerektiğinde.

## <a name="json-walkthrough"></a>JSON gözden geçirme

Bu izlenecek yol için örnek oluşturulacak görevleri sağlar. Bir görev ilk ekranda seçildiğinde, gösterildiği gibi bir ayrıntı ekranı sunulur:

 [![](json-element-walkthrough-images/03-task-list.png "Bir görev ilk ekranda seçildiğinde, gösterildiği gibi bir ayrıntı ekranı sunulur")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON'ı oluşturma

Bu örnekte, JSON adlı proje dosyasından yüklediğimiz `task.json`. MT'NİN D JSON öğeler API'sini yansıtan bir sözdizimine uygun olması için bekler. Koddan öğeler API'sini kullanarak, olduğu gibi JSON kullanırken, şu bölümler bildirme ve bu bölümler içinde öğeleri ekleriz. Bölümleri ve JSON öğelerini bildirmek için dizeleri "bölümler" ve "öğeleri" sırasıyla anahtarlar olarak kullanıyoruz. Her öğe için ilgili öğe türü kullanılarak ayarlanan `type` anahtarı. Her bir öğe özelliği özellik adı ile anahtar olarak ayarlanır.

Örneğin, aşağıdaki JSON, bölümler ve görev ayrıntılarını için öğeleri açıklanmaktadır:

```csharp
{
    "title": "Task",
    "sections": [
        {
          "elements" : [
            {
                "id" : "task-description",
                "type": "entry",
                "placeholder": "Enter task description"
            },
            {
                "id" : "task-duedate",
                "type": "date",
                "caption": "Due Date",
                "value": "00:00"
            }
         ]
        }
    ]
  }
```

Bildirim yukarıdaki JSON, her öğe için bir kimlik içerir. Herhangi bir öğeyi çalışma zamanında başvurmak için bir kimlik içerebilir. Bu biraz zaman kod JSON'da yükleme göstereceğiz nasıl kullanıldığına göreceğiz.

## <a name="loading-the-json-in-code"></a>JSON kod yükleniyor

JSON tanımlandıktan sonra MT'nin yüklemek ihtiyacımız D kullanarak `JsonElement` sınıfı. Yukarıda oluşturduğumuz JSON dosyasıyla varsayılarak olduğundan adı sample.json projeyle eklenir ve yükleme içeriği, bir derleme eylemi verilen `JsonElement` olarak aşağıdaki kod satırını çağırmak kadar kolaydır:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Bu her zaman isteğe bağlı bir görev oluşturulur ekliyoruz olduğundan, biz önceki öğeler API'sini örnekten gibi tıklanan düğme değiştirebilirsiniz:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>Çalışma zamanında öğelere erişme

Biz JSON dosyasında bildirildiğinde bir kimliği için her iki öğeleri ekledik geri çağırma. Kod özelliklerini değiştirmek için çalışma zamanında her öğeye erişmek için kimlik özelliği kullanabiliriz. Örneğin, aşağıdaki kod, görev nesnesinden değerleri ayarlamak için giriş ve tarih öğeleri başvuruyor:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        taskElement.Caption = task.Name;

        var description = taskElement ["task-description"] as EntryElement;

        if (description != null) {
                description.Caption = task.Name;
                description.Value = task.Description;       
        }

        var duedate = taskElement ["task-duedate"] as DateElement;

        if (duedate != null) {                
                duedate.DateValue = task.DueDate;
        }
        _rootElement [0].Add (taskElement);
};
```

## <a name="loading-json-from-a-url"></a>Bir URL'den JSON yükleniyor

MT'NİN D de destekler dinamik olarak bir dış Url yükleme URL'si basitçe oluşturucusuna geçirerek JSON `JsonElement`. MT'NİN D, ekranlar arasında gezinirken JSON'da isteğe bağlı olarak bildirilen hiyerarşi genişletilir. Örneğin, aşağıdaki gibi bir JSON dosyası yerel web sunucusunun kök dizininde bulunan göz önünde bulundurun:

```csharp
{
    "type": "root",
    "title": "home",
    "sections": [
       {
         "header": "Nested view!",
         "elements": [
           {
             "type": "boolean",
             "caption": "Just a boolean",
             "id": "first-boolean",
             "value": false
           },
           {
             "type": "string",
             "caption": "Welcome to the nested controller"
           }
         ]
       }
     ]
}
```

Biz kullanarak yükleyebilirsiniz `JsonElement` şu kod gibi:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

Çalışma zamanında dosyayı alınır ve MT'nin tarafından ayrıştırılan Kullanıcı, aşağıdaki ekran görüntüsünde gösterildiği gibi ikinci görünümüne gittiğinde D:

 [![](json-element-walkthrough-images/04-json-web-example.png "Dosya alınır ve MT'nin tarafından ayrıştırılan Kullanıcı ikinci görünümüne gittiğinde D")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>Özet

Bu makalede gösterilen bir kullanarak nasıl oluşturabileceğiniz MT'nin arabirimi JSON D. Bu, uygulama ile uzak bir URL'den yanı sıra bir dosyada bulunan JSON yük nasıl oluşturulacağını gösterir. Çalışma zamanında JSON'da açıklanan öğelere erişmek nasıl oluşturulacağını gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [MTDJsonDemo (örnek)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Yayını - bir iOS oturum açma ekranı Miguel de Icaza, MonoTouch.Dialog ile oluşturur.](http://youtu.be/3butqB1EG0c)
- [Yayını - iOS kullanıcı arabirimleri ile MonoTouch.Dialog kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Öğeleri API gözden geçirme](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Yansıma API'si gözden geçirme](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation uygulama](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
