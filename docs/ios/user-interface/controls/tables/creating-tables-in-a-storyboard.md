---
title: İOS Tasarımcısı sekmelerle çalışma
description: Önceki bölümlerde tabloları kullanarak geliştirme incelediniz. Bu konuda beşinci ve son bölümünde, biz ne biz kadar öğrendiniz toplama ve film şeridi kullanarak temel bir işi listesi uygulaması oluşturun.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 869719b1d3514fee4b45f61dcdb34a0bcf54f7b2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30780729"
---
# <a name="working-with-tables-in-the-ios-designer"></a>İOS Tasarımcısı sekmelerle çalışma

Film şeritleri iOS uygulamaları oluşturmak için WYSIWYG bir yöntemdir ve Mac ve Windows üzerinde Visual Studio içinde desteklenir. Film şeritleri hakkında daha fazla bilgi için başvurmak [giriş için film şeritleri](~/ios/user-interface/storyboards/index.md) belge. Film şeritleri da izin hücre düzenleri düzenlemenizi *içinde* tabloları ve hücreleriyle geliştirme basitleştirir tablosu

Bir tablo görünümü özelliklerini iOS Tasarımcısı yapılandırırken, seçim yapabileceğiniz hücre içeriğinin iki tür vardır: **dinamik** veya **statik** prototip içeriği.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Dinamik prototip içeriği

A `UITableView` prototip ile içerik genellikle veri listesini görüntülemek için tasarlanmıştır burada prototip hücrenin (veya hücre tanımlayabilirsiniz birden fazla) listesindeki her bir öğe için yeniden kullanılır. Hücreleri oluşturulmasını gerekmez, içinde elde `GetView` yöntemini çağırarak `DequeueReusableCell` yöntemi kendi `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>Statik İçerik

`UITableView`statik içeriğe sahip s sağ tasarım yüzeyine tasarlanmalıdır tabloları izin verin. Hücreleri tabloya sürüklenen ve özelleştirilmiş özelliklerini değiştirme ve denetimleri ekleme.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>Film şeridi temelli uygulama oluşturma

StoryboardTable örnek UITableView her iki türdeki bir film şeridi kullanan basit bir ana-ayrıntı uygulama içerir. Bu bölüm geri kalanı tamamlandığında şuna benzeyecektir küçük Yapılacaklar listesi örnek derlemeyi açıklar:

 [![Örnek ekranlar](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

Kullanıcı arabirimi ile film şeridi oluşturulacak ve her iki ekranlar bir UITableView kullanır. Ana Ekran kullanıp *prototip içerik* düzeni satır ve ayrıntı ekran kullanıp *statik içerik* özel hücre düzenleri kullanarak veri girişi formu oluşturmak için.

## <a name="walkthrough"></a>İzlenecek yol

Visual Studio kullanarak yeni bir çözüm oluşturun **(Oluştur) yeni proje... > tek görünüm App(C#)** ve bunu _StoryboardTables_.

 [![Yeni Proje iletişim kutusu oluşturma](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

Çözüm bazı C# dosyalarıyla açar ve bir `Main.storyboard` dosyası zaten oluşturulmuş. Çift `Main.storyboard` dosyayı iOS Tasarımcısı açın.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Film şeridi değiştirme

Film şeridi üç adımda düzenlenecek:

-  İlk olarak, düzen gerekli denetleyicileri görüntüleyebilir ve bunların özelliklerini ayarlayın.
-  İkinci olarak, sürükleyip bırakarak görünümünüzü oturum nesneleri, kullanıcı Arabirimi oluşturma
-  Son olarak, her görünümüne gerekli Uıkit sınıf ekleyin ve çeşitli denetimleri kodunda referans için bir ad verin.


Film şeridi tamamlandıktan sonra kodu her şeyi iş yapmak için eklenebilir.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Düzen görünümü denetleyicileri

Film şeridi ilk değişiklik varolan ayrıntı görünümü siliniyor ve UITableViewController ile değiştiriliyor. Aşağıdaki adımları uygulayın:

1.  Görünüm denetleyicisini altındaki Çubuğu'nu seçin ve silin.
2.  Sürükleme bir **Gezinti denetleyicisi** ve **Tablo görünümü denetleyicisi** film şeridi araç görünümünden üzerine. 
3.  Bir segue kök görünümü denetleyicisinden ikinci tablo görünümü yalnızca eklenen denetleyiciye oluşturun. Denetim segue Oluştur + sürükleme için *ayrıntı hücreden* yeni eklenen UITableViewController için. Seçeneği **Göster*** altında **ü seçimi**. 
4.  Yeni oluşturduğunuz ü ve bu kodda ü başvurusu tanımlayıcısını belirtin seçin. Üzerinde segue tıklatıp girin `TaskSegue` için **tanımlayıcısı** içinde **özellikleri paneli**, şöyle:    
  [![Özellik panelinde ü adlandırma](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. Ardından, bunları seçerek ve özellikleri panelini kullanarak iki tablo görünümü yapılandırın. Görünüm ve View Controller seçtiğinizden emin olun – seçimle yardımcı olmak için Belge Anahattı kullanabilirsiniz.

6.  Kök görünüm Denetleyicisi'ne değiştirme **içerik: dinamik prototipleri** (tasarım yüzeyine görünümde etiketli **prototip içerik** ):

    [![İçerik özelliği için dinamik prototipleri ayarlama](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  Yeni değişiklik **UITableViewController** olmasını **içerik: statik hücreleri**. 


8. Yeni UITableViewController kümesi tanımlayıcısı ve sınıf adı olması gerekir. View Controller ve türünü seçin _TaskDetailViewController_ için **sınıfı** içinde **özellikleri paneli** – bu yeni bir oluşturacak `TaskDetailViewController.cs` çözüm dosyasında Panel. Girin **StoryboardID** olarak _ayrıntı_aşağıdaki örnekte gösterildiği gibi. Bu daha sonra bu görünümde C# kodu yüklemek için kullanılır:  

    [![Film şeridi kimliği ayarlama](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. Film şeridi tasarım yüzeyi şimdi aşağıdaki gibi görünmelidir (kök görünümü denetleyicinin Gezinti öğesi başlığı "İşi panosuna" değiştirildi):

    [![Tasarım yüzeyi](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Kullanıcı Arabirimi oluşturma

Şimdi görünümleri ve segues olan yapılandırıldıysa, kullanıcı arabirimi öğeleri eklenmesi gerekir.

#### <a name="root-view-controller"></a>Kök görünümü denetleyicisi

İlk olarak, ana görünüm denetleyicisini prototip hücreyi seçin ve ayarlama **tanımlayıcısı** olarak _taskcell_aşağıda gösterildiği gibi. Bu kodu daha sonra bu UITableViewCell örneğini almak için kullanılacak:

 [![Hücre tanıtıcısını ayarlama](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

Ardından, aşağıda gösterildiği gibi yeni görevler ekleyecek bir düğme oluşturmanız gerekir:

[![Düğme öğesi Gezinti çubuğundaki çubuğu](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

Aşağıdakileri yapın: 

-  Sürükleme bir **çubuğu düğme öğesi** için araç _sağ taraftaki gezinti çubuğunun_.
-  İçinde **özellikleri paneli**altında **çubuğu düğme öğesi** seçin **tanımlayıcısı: eklemek** (Bunu yapmak için bir *+* artı düğmesini). 
-  Böylece, daha sonraki bir aşamada bu kodu tanımlanabilir bir ad verin. Not bir sınıf adı kök View Controller vermeniz gerekir (örneğin **ItemViewController**) çubuğu düğme öğesi'nin adı izin vermek için.


#### <a name="taskdetail-view-controller"></a>TaskDetail görünümü denetleyicisi

Ayrıntı görünümü çok daha fazla iş gerektirir. Tablo görünümü hücreleri görünümü sürüklediğiniz ve etiketleri, metin görünümleri ve düğmeleriyle doldurulmuş gerekir. Aşağıdaki ekran görüntüsünde iki bölümlü tamamlanmış UI gösterir. Bir bölümü sahip üç hücreler, üç etiket, ikinci bölümü iki düğmelerle bir hücre sahipken iki metin alanları ve tek, geçin:

 [![Ayrıntı görünüm düzeni](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

Tam yerleşimi oluşturmak için adımlar şunlardır:

Tablo görünümü seçin ve açın **özelliği paneli**. Aşağıdaki özellikler güncelleştirin:

-  **Bölümler**: _2_ 
-  **Stil**: _gruplandırılmış_
-  **Ayırıcı**: _yok_
-  **Seçim**: _seçimi yok_

Üst kısmında seçin ve altında **Özellikler > Tablo görünüm bölümü** değiştirme **satırları** için _3_aşağıda gösterildiği gibi:


 [![üç satır için üst kısmında ayarlama](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

Açık her hücre için **özellikleri paneli** ve ayarlayın:

-  **Stil**: _özel_
-  **Tanımlayıcı**: (ör. her bir hücre için benzersiz bir tanımlayıcı seçin "_başlık_","_notları_","_Bitti_").
-  Aşağıdaki ekran görüntüsünde gösterilen düzeni oluşturmak için gerekli denetimleri sürükleyin (yerleştirin **UILabel**, **UITextField** ve **UISwitch** doğru hücrelerde ve etiketleri ayarlama uygun şekilde, IE. Başlık, notlar ve yapılır).


İkinci bölümünde ayarlayın **satırları** için _1_ ve eninden yapmak için hücrenin alt yeniden boyutlandırma tutamacını yakalayın.

-  **Kümesi tanımlayıcısı**: benzersiz bir değere (ör.) "Kaydet"). 
-  **Arka plan Ayarla**: _temizleyin renk_ .
-  İki düğme hücresine sürükleyin ve bunların başlıklarını uygun şekilde ayarlayın (yani _kaydetmek_ ve _silmek_) aşağıda gösterildiği gibi:

   [![iki düğme alt kısmında ayarlama](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

Bu noktada aynı zamanda hücre ve Uyarlamalı düzeni emin olmak için denetim kısıtlamaları ayarlamak isteyebilirsiniz.

### <a name="adding-uikit-class-and-naming-controls"></a>Uıkit sınıfı ekleme ve denetimleri adlandırma

Bizim film şeridi oluşturmada son birkaç adım vardır. İlk bizim denetimlerinin her biri bir ad altında sunuyoruz gerekir **kimliği > adı** bunlar daha sonra kodda kullanılabilmesi için. Bunlar gibi ad:

-  **Başlık UITextField** : _TitleText_
-  **Notlar UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **Delete UIButton** : _DeleteButton_
-  **UIButton kaydetmek** : _saveButton yapın_


<a name="Adding_Code" />

## <a name="adding-code"></a>Kod ekleme

İş geri kalanı, Mac veya Windows C# ile Visual Studio'da yapılacaktır. Kod içinde kullanılan özellik adları yukarıdaki kılavuzda belirlenen yansıtacak unutmayın.

Biz oluşturmak istediğiniz ilk bir `Chores` almak ve kimliği, ad, notlar ve bitti Boolean, değerini ayarlamak için bir yol sağlar ve böylece uygulama boyunca bu değerler kullanıyoruz sınıfı.

İçinde `Chores` sınıfı aşağıdaki kodu ekleyin:

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Ardından, oluşturun bir `RootTableSource` devraldığı sınıfı `UITableViewSource`. 

Film şeridi olmayan tablo görünümü bu arasındaki fark `GetView` yöntemi hücreler – örneği gereken değil `theDequeueReusableCell` yöntemi her zaman prototip hücrenin (ile eşleşen tanımlayıcısı) örneğini döndürür.

Aşağıdaki kodu arasındadır `RootTableSource.cs` dosyası:

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

Kullanılacak `RootTableSource` sınıfı, yeni bir koleksiyon oluşturmak `ItemViewController`'s Oluşturucusu:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

İçinde `ViewWillAppear` kaynak koleksiyonu geçirmek ve Tablo görünümüne atayın:

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Uygulama çalıştırırsanız artık, ana ekran şimdi yüklemek ve iki görev listesini görüntüleyin. Bir görev işlemdeki olduğunda film şeridi tarafından tanımlanan segue ayrıntı ekranın görünmesini neden olur, ancak şu anda herhangi bir veri görüntülenmez.

'Parametresi bir segue göndermek için ', geçersiz kılma `PrepareForSegue` yöntemi ve özelliklerini ayarlama `DestinationViewController` ( `TaskDetailViewController` Bu örnekte). Hedef görünüm denetleyicisi sınıfı örneği ancak değil henüz kullanıcıya – görüntülenen bu sınıfa özelliklerini ayarlayın ancak kullanıcı Arabirimi denetimlerini değiştirmemek anlamına gelir:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

İçinde `TaskDetailViewController` `SetTask` yöntemi ViewWillAppear içinde başvurulan şekilde bu parametreleri özelliklerini atar. Denetim özellikleri değiştirilemez olarak `SetTask` çünkü var olmayabilir, `PrepareForSegue` adı verilir:

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

Segue şimdi ayrıntı ekranını açmak ve seçilen görev bilgilerini görüntüleyin. Ne yazık ki uygulaması yok **kaydetmek** ve **silmek** düğmeler. Düğmeleri uygulamadan önce bu yöntemlere ekleme **ItemViewController.cs** temel alınan veri güncelleştirmek ve ayrıntı ekranı kapatmak için:

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

Ardından, düğmenin eklemeniz gerekir `TouchUpInside` olay işleyicisine `ViewDidLoad` yöntemi **TaskDetailViewController.cs**. `Delegate` Özellik referansı `ItemViewController` biz özellikle çağırması oluşturulduğu `SaveTask` ve `DeleteTask`, kendi işleminin bir parçası bu görünüm kapatın:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

Son kalan oluşturmak için işlevselliği yeni görevler oluşturulmasını parçasıdır. İçinde **ItemViewController.cs** oluşturur Yeni bir yöntem, görevler ve ayrıntı görünümünü açar ekleyin. Film şeridi kullanımdan bir görünüm oluşturmak için `InstantiateViewController` yöntemiyle `Identifier` bu görünümün - bu örnekte 'ayrıntı' olacaktır:

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

Son olarak, wire gezinti çubuğunda düğmesini yukarı **ItemViewController.cs**'s `ViewDidLoad` onu çağrılacak yöntem:

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Film şeridi örnek – şöyle tamamlanan uygulama görünür tamamlanan:

[![Tamamlanan uygulama](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

Örnek gösterilmektedir:

-  Prototip hücreleri veri listesini görüntülemek yeniden kullanım için tanımlandığı içerik ile tablo oluşturma. 
-  Tablo statik bir giriş formu oluşturmak için içerik ile oluşturma. Bu tablo stilini değiştirme ve bölümler, hücre ve kullanıcı Arabirimi denetimlerini ekleme dahil. 
-  Bir segue oluşturma ve geçersiz kılma `PrepareForSegue` gerektirdiği herhangi bir parametre hedef görünümünü bildirmek için yöntem. 
-  Film şeridi görünümleri ile doğrudan yüklenirken `Storyboard.InstantiateViewController` yöntemi.



## <a name="related-links"></a>İlgili bağlantılar

- [StoryboardTable (örnek)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Görsel Taslaklara Giriş](~/ios/user-interface/storyboards/index.md)
