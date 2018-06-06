---
title: Xamarin.iOS içinde bir kullanıcı arabirimi oluşturmak için JSON kullanma
description: MonoTouch.Dialog (yüksekliğindeki D) JSON verilerini aracılığıyla dinamik kullanıcı Arabirimi oluşturma için destek içerir. Bu öğreticide, biz bir JSONElement ya da bir uygulamayla birlikte gelen veya uzak bir URL'den yüklenen JSON öğesinden bir kullanıcı arabirimi oluşturmak için nasıl kullanılacağını size rehberlik.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f9ba2cce1650260aa889e8282c091012ef8bbddc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790659"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Xamarin.iOS içinde bir kullanıcı arabirimi oluşturmak için JSON kullanma

_MonoTouch.Dialog (yüksekliğindeki D) JSON verilerini aracılığıyla dinamik kullanıcı Arabirimi oluşturma için destek içerir. Bu öğreticide, biz bir JSONElement ya da bir uygulamayla birlikte gelen veya uzak bir URL'den yüklenen JSON öğesinden bir kullanıcı arabirimi oluşturmak için nasıl kullanılacağını size rehberlik._

YÜKSEKLİĞİNDEKİ D JSON içinde bildirilen oluşturma kullanıcı arabirimini destekler. JSON, yüksekliğindeki kullanarak öğeleri bildirilen olduğunda D ilişkilendirilen öğeleri sizin için otomatik olarak oluşturur. JSON bir ayrıştırılmış bir yerel dosyadan ya da yüklenebilir `JsonObject` örneği veya uzak bir Url.

YÜKSEKLİĞİNDEKİ D JSON kullanırken öğeleri API'SİNDE kullanılabilen özellikleri tam aralığını destekler. Örneğin, aşağıdaki ekran görüntüsünde uygulamada tamamen JSON kullanarak bildirilmiş:

[![](json-element-walkthrough-images/01-load-from-file.png "Örneğin, JSON kullanarak tamamen bu ekran uygulamada bildirilmiş") ](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ] (json-element-walkthrough-images/01-load-from-file.png "Örneğin bu ekran uygulamada tamamen kullanarak bildirildi JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Şimdi örnekten yeniden ziyaret [öğeleri API izlenecek](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) kullanarak bir görev ayrıntı ekran eklemek nasıl gösteren öğretici.

## <a name="json-walkthrough"></a>JSON gözden geçirme

Bu kılavuzda örneğin oluşturulacak görevlerin olanak sağlar. Bir görev ilk ekranda seçildiğinde, ayrıntı ekranı gösterildiği gibi sunulur:

 [![](json-element-walkthrough-images/03-task-list.png "Bir görev ilk ekranda seçildiğinde, gösterildiği gibi bir ayrıntı ekranı sunulur")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON oluşturma

Bu örnekte, biz JSON adlı proje dosyasında yüklemek `task.json`. YÜKSEKLİĞİNDEKİ D JSON öğeleri API yansıtan bir sözdizimi için uygun olması için bekler. Koddan öğeleri API'yi kullanarak gibi JSON kullanırken, biz bölümleri bildirme ve bu bölümler içinde öğeleri ekleriz. Bölümler ve JSON öğelerinde bildirmek için dizeleri "Bölüm" ve "öğeleri" sırasıyla anahtarlar olarak kullanırız. Her öğe için ilişkili öğe türü kullanılarak ayarlanan `type` anahtarı. Her bir öğe özelliği ile özellik adı anahtar olarak ayarlanır.

Örneğin, aşağıdaki JSON öğeler için görev ayrıntılarını ve bölümlerde açıklanmaktadır:

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

Bildirim yukarıdaki JSON her öğe için bir kimlik içerir. Çalışma zamanında başvurmak için bir kimliği herhangi bir öğe içerebilir. Bu kod JSON yükleme zaman gösteriyoruz bir dakika içinde nasıl kullanıldığını göreceğiz.

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>Kodda JSON yükleniyor

JSON tanımlandıktan sonra yüksekliğindeki yüklemek ihtiyacımız D kullanarak `JsonElement` sınıfı. Yukarıda oluşturduğumuz JSON dosyasıyla varsayılarak olduğundan proje adı sample.json ile eklenir ve yükleme içeriğinin bir yapı eylemi verilen `JsonElement` aşağıdaki kod satırını çağırma olarak kadar basit hale getirir:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Bu görev oluşturulan isteğe bağlı olarak her zaman ekliyoruz olduğundan, biz önceki öğeleri API örnekten gibi tıklanan düğme değiştirebilirsiniz:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>Çalışma zamanında öğelere erişme

Biz JSON dosyasında bildirirken bir kimliği hem öğelerine eklediğimiz geri çağırma. Kodda özellikleri değiştirmek için çalışma zamanında her bir öğesine erişmek için şu kimliği özelliğini kullanabilirsiniz. Örneğin, aşağıdaki kod görev nesneden değerleri ayarlamak için giriş ve tarih öğeleri başvuruyor:

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

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>JSON bir URL'den yükleniyor

YÜKSEKLİĞİNDEKİ D de destekler URL'si basitçe oluşturucusuna geçirerek JSON bir dış Url dinamik olarak yükleme `JsonElement`. YÜKSEKLİĞİNDEKİ D ekranları arasında gezinmek gibi JSON'isteğe bağlı olarak bildirilen hiyerarşi genişletilir. Örneğin, aşağıdaki gibi bir JSON dosyası yerel web sunucusunun kök dizininde bulunan göz önünde bulundurun:

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

Biz kullanarak yükleyebilirsiniz `JsonElement` aşağıdaki kodu olduğu gibi:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

Çalışma zamanında dosyası alınır ve yüksekliğindeki tarafından ayrıştırılan Kullanıcı, aşağıdaki ekran görüntüsünde gösterildiği gibi ikinci görünümüne gittiğinde D:

 [![](json-element-walkthrough-images/04-json-web-example.png "Dosya alınır ve yüksekliğindeki tarafından ayrıştırılan Kullanıcı ikinci görünümüne gittiğinde D")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Özet

Bu makalede gösterilen kullanarak bir oluşturma yüksekliğindeki arabirimi JSON D. Bu uygulama ile bir uzak URL'den yanı sıra bir dosyasına dahil JSON yüklemek nasıl oluşturulacağını gösterir. Ayrıca çalışma zamanında JSON içinde açıklanan öğeleri erişmek nasıl oluşturulacağını gösterir.


## <a name="related-links"></a>İlgili bağlantılar

- [MTDJsonDemo (örnek)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Ekran kaydı - Miguel de Icaza iOS oturum açma ekranı MonoTouch.Dialog ile oluşturur](http://youtu.be/3butqB1EG0c)
- [Ekran kaydı - iOS kullanıcı arabirimleri MonoTouch.Dialog ile kolayca oluşturun](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Öğeleri API gözden geçirme](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Yansıma API gözden geçirme](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github'da MonoTouch iletişim](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation uygulama](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController sınıf başvurusu](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
