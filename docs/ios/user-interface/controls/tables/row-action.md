---
title: Xamarin.iOS satır Eylemler ile çalışma
description: Bu kılavuz UISwipeActionsConfiguration veya UITableViewRowAction tablo satır için özel manyetik eylemlerini oluşturmak nasıl gösterir
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 4be8b6dc66c9c047e6662067e7e3ecf81ab22893
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789947"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Xamarin.iOS satır Eylemler ile çalışma

_Bu kılavuz UISwipeActionsConfiguration veya UITableViewRowAction tablo satır için özel manyetik eylemlerini oluşturmak nasıl gösterir_

![Satır geçirme eylemleri gösterimi](row-action-images/action02.png)

iOS bir tabloda eylemleri gerçekleştirmek için iki yol sağlar: `UISwipeActionsConfiguration` ve `UITableViewRowAction`.

`UISwipeActionsConfiguration` iOS 11 sunulmuştur ve bir dizi tanımlamak için kullanılan gerçekleştirmesi gereken eylemleri yerleştirin kullanıcı swipes _her iki yönde_ Tablo görünümünde bir satırda. Bu davranış yerel Mail.app benzer 

`UITableViewRowAction` Sınıfı, kullanıcı swipes yatay olarak bir Tablo görünümünde bir satırda bırakıldığına gerçekleşecek eylemi tanımlamak için kullanılır.
Örneğin, sol tarafta bir satır geçirme bir tablo düzenleme görüntülendiğinde bir **silmek** varsayılan düğme. Birden çok örneğini ekleyerek `UITableViewRowAction` sınıfının bir `UITableView`, birden çok özel eylemler tanımlanabilir, her biri kendi metin, biçimlendirme ve davranışı.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

Manyetik eylemlerle uygulamak için gereken üç adım vardır `UISwipeActionsConfiguration`:

1. Geçersiz kılma `GetLeadingSwipeActionsConfiguration` ve/veya `GetTrailingSwipeActionsConfiguration` yöntemleri. Bu yöntemler döndürür bir `UISwipeActionsConfiguration`. 
2. Örneği `UISwipeActionsConfiguration` döndürülür. Bu sınıf bir dizi alır `UIContextualAction`.
3. Oluşturma bir `UIContextualAction`.

Bunlar aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. SwipeActionsConfigurations yöntemleri uygulama

`UITableViewController` (ve ayrıca `UITableViewSource` ve `UITableViewDelegate`) iki yöntem içerir: `GetLeadingSwipeActionsConfiguration` ve `GetTrailingSwipeActionsConfiguration`, bir tablo görünümü satır geçirme Eylemler kümesi uygulamak için kullanılır. Önde gelen manyetik eylem bir sağdan soldan sağa dilde ekranın sol tarafındaki ve bir sağdan sola dilde ekranın sağ taraftaki ifade eder. 

Aşağıdaki örnekte (gelen [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) örnek) başında manyetik yapılandırma uygulama gösterir. İki eylem açıklanacak bağlamsal eylemlerden oluşturulan [aşağıda](#create-uicontextualaction). Bu eylemler sonra yeni başlatılmış iletilen [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), dönüş değeri olarak kullanılır.


```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });
    
    leadingSwipe.PerformsFirstActionWithFullSwipe = false;
    
    return leadingSwipe;
}  
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Örneği bir `UISwipeActionsConfiguration`

Örneği bir `UISwipeActionsConfiguration` kullanarak `FromActions` yeni bir dizi eklemek için yöntemini `UIContextualAction`aşağıdaki kod parçacığında gösterildiği gibi s:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Eylemleriniz görüntülendiği sırada, diziye nasıl geçtiğini üzerinde bağımlı olduğunu dikkate almak önemlidir. Örneğin, yukarıdaki kodu başında swipes için eylemleri olarak bu nedenle görüntüler:

![Manyetik Eylemler önünde bir tablo satır görüntüleme](row-action-images/action03.png)

Swipes sondaki için aşağıdaki görüntüde gösterildiği gibi eylemler görüntülenir:

![bir tablo satır geçirme Eylemler sondaki görüntülenen](row-action-images/action04.png)

Bu kod parçacığını ayrıca yeni kullanır `PerformsFirstActionWithFullSwipe` özelliği. Varsayılan olarak, bu özelliği ayarlamak true, bir kullanıcının tam olarak bir satırda swipes tıklattığınızda dizinin ilk eylem olacağını anlamına gelir. Bozucu değil bir eylem varsa (örneğin "Delete", bu ideal davranış olmayabilir ve, bu nedenle onu ayarlamalıdır `false`.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Oluşturma bir `UIContextualAction`

Kullanıcı bir tablo satırının swipes, görüntülenecek eylemi gerçekte oluşturduğunuz bağlamsal eylemdir.

Sağlamanız gerekir bir eylem başlatmak için bir `UIContextualActionStyle`, bir başlığı ve bir `UIContextualActionHandler`. `UIContextualActionHandler` Üç parametre alır: bir eylem, eylem içinde görüntülenen görünümü ve tamamlanma işleyici:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null)); 
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);
                            
                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

Arka plan rengini veya eylem görüntüsü gibi çeşitli görsel özellikleri düzenlenebilir. Yukarıdaki kod parçacığında eyleme görüntü ekleme ve mavi arka plan rengini ayarlama gösterir.

Bağlamsal Eylemler oluşturduktan sonra başlatmak için kullanabileceklerini `UISwipeActionsConfiguration` içinde `GetLeadingSwipeActionsConfiguration` yöntemi.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

İçin bir veya daha fazla özel satır eylemleri tanımlamak için bir `UITableView`, bir örneğini oluşturmanız gerekir `UITableViewDelegate` sınıfı ve geçersiz kılma `EditActionsForRow` yöntemi. Örneğin:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

Statik `UITableViewRowAction.Create` yöntemi yeni bir oluşturmak için kullanılan `UITableViewRowAction` , görüntüler bir **Hi** düğmesini tablosundaki satır üzerindeki kullanıcı swipes yatay bırakıldığına. Daha sonra yeni bir örneğini `TableDelegate` oluşturulur ve bağlı `UITableView`. Örneğin:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Yukarıdaki kod çalıştırdığınızda ve kullanıcı swipes sol tablo satırını temel **Hi** yerine düğmesi görüntülenir **silmek** varsayılan olarak görüntülenen düğmesi:

[![](row-action-images/action01.png "Yerine Sil düğmesini görüntülenmesini Hi düğmesi")](row-action-images/action01.png#lightbox)

Kullanıcı dokunur varsa **Hi** düğmesini `Hello World!` uygulamayı hata ayıklama modunda çalıştırdığınızda Visual Studio konsolunda Mac veya Visual Studio için yazılır.



## <a name="related-links"></a>İlgili bağlantılar

- [TableSwipeActions (örnek)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (örnek)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
