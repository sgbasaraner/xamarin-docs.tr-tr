---
title: Merhaba, iOS Multiscreen
description: "Bu iki parçalı kılavuzdaki biz Hello oluşturduğumuz Phoneword uygulama genişletin, ikinci bir ekran işlemek için iOS Kılavuzu. Model-View-Controller tasarım deseni neden şekilde, boyunca ilk bizim iOS Gezinti uygulamak ve iOS uygulama yapısı ve işlevleri daha derin bir anlayış geliştirin."
ms.topic: article
ms.prod: xamarin
ms.assetid: d72e6230-c9ee-4bee-90ec-877d256821aa
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 6df2e2ad97e42c854b6377268086b80eef145e37
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="helloios-multiscreen-quickstart"></a>Hello.iOS Multiscreen hızlı başlangıç

Kılavuzun bu bölümü uygulamayla adı veriliyordu telefon numaralarını geçmişini görüntüler Phoneword uygulama ikinci ekranı ekleyeceksiniz. Son uygulama çağrısı geçmişini görüntüler ikinci bir ekran tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi olacaktır:

 [ ![](hello-ios-multiscreen-quickstart-images/00.png "Son uygulama tarafından bu ekran görüntüsünde gösterildiği gibi çağrısı geçmişini görüntüler ikinci bir ekran gerekir")](hello-ios-multiscreen-quickstart-images/00.png)

[Derinlemesine eşlik eden](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md), yapı yerleşik olan uygulama inceleme ve mimarisi, gezinti ve size yol boyunca karşılaştığınız diğer yeni iOS kavramlarını ele alınmıştır.

 <a name="Requirements" />

## <a name="requirements"></a>Gereksinimler

Burada Hello, sol iOS belge kapalı ve tamamlanmasından gerektirir bu kılavuzu sürdürür [Hello, iOS Hızlı Başlangıç](~/ios/get-started/hello-ios/index.md). Phoneword uygulama tamamlanmış sürümünü şu adresten yüklenebilir: [Hello, iOS örnek](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="walkthrough"></a>İzlenecek yol

Bu kılavuz bir arama geçmişi ekrana ekleyeceksiniz bizim **Phoneword** uygulama.


1. Açık **Phoneword** Mac için Visual Studio'da uygulama Gerekirse, tamamlanmış Phoneword uygulamasından [Hello, iOS izlenecek](~/ios/get-started/hello-ios/index.md) Kılavuzu adresinden yüklenebilir [burada](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).


2. Açık **Main.storyboard** dosya **çözüm paneli**:

  ![](hello-ios-multiscreen-quickstart-images/02new.png "İOS Tasarımcısı'nda Main.storyboard")


3. Sürükleme bir **Gezinti denetleyicisi** gelen **araç** tasarım yüzeyine (gerekebilir tasarım yüzeyine bu tüm uyacak şekilde Uzaklaştır!):

  ![](hello-ios-multiscreen-quickstart-images/03new.png "Bir gezinme denetleyicisi araç tasarım yüzeyine sürükleyin")


4. Sürükleme **Sourceless ü** (tek görünüm denetleyici solundaki gri ok olan) için **Gezinti denetleyicisi** uygulamanın başlangıç noktası değiştirmek için:

  ![](hello-ios-multiscreen-quickstart-images/04new.png "Uygulama başlangıç noktasını değiştirmek için Gezinti denetleyiciye Sourceless ü sürükleyin")


5. Varolanı seç **kök View Controller** alt çubukta ve tuşuna tıklayarak **silmek** tasarım yüzeyden kaldırmak için.
Ardından, taşımak **Phoneword** yanındaki Sahne **Gezinti denetleyicisi**:

  ![](hello-ios-multiscreen-quickstart-images/05new.png "Gezinti denetleyicisi yanındaki Phoneword Sahne taşıma")


6. Ayarlama **ViewController** Gezinti denetleyicinin olarak **kök View Controller**. Aşağı basın **Ctrl** anahtarı ve içini tıklatın **Gezinti denetleyicisi**. Mavi bir çizgi görüntülenmesi gerekir. Ardından, hala basılı **Ctrl** anahtar, sürükleyin **Gezinti denetleyicisi** için **Phoneword** Sahne ve sürüm. Bu adlı _Ctrl sürükleyerek_:

 ![](hello-ios-multiscreen-quickstart-images/06.png "Gezinti denetleyicisinden Phoneword Sahne ve sürüm sürükleyin")


7. İlişki popover kümesine **kök**:

  ![](hello-ios-multiscreen-quickstart-images/07new.png "İlişki için kök ayarlama")

  **ViewController** artık **Gezinti denetleyicinin kök View Controller:**

  ![](hello-ios-multiscreen-quickstart-images/08.png "ViewController Gezinti denetleyicileri kök View Controller sunulmuştur")


8. Çift **Phoneword** ekran **başlık** çubuk ve değişiklik **başlık** için **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/09.png "Değişikliği 'Phoneword' başlığı")


9. Sürükleme bir **düğmesini** gelen **araç** ve altında yerleştirin **arama düğmesini**. Yeni yapmak için işleyiciler sürükleyin **düğmesini** aynı genişliğe **çağrısı düğmesi**:

  ![](hello-ios-multiscreen-quickstart-images/10new.png "Arama düğmesini aynı genişliğe yap yeni düğmesi")


10. İçinde **özellikleri paneli**, değiştirme **adı** için düğmesinin **CallHistoryButton** değiştirip **başlık** için **çağırın Geçmiş**:

  ![](hello-ios-multiscreen-quickstart-images/11new.png "Düğmenin adı için CallHistoryButton değiştirin ve Arama Geçmişi Başlığı değiştirin")


11. Oluşturma **Arama Geçmişi** ekran. Gelen **araç**, sürükleyin bir **tablo View Controller** tasarım yüzeyine:

 ![](hello-ios-multiscreen-quickstart-images/12new.png "Bir tablo View Controller tasarım yüzeyine sürükleyin")


12. Ardından, **tablo View Controller** Sahne altındaki siyah çubuğunda tıklatarak. İçinde **özellikleri paneli**, değiştirme **Tablo görünümü denetleyicinin** sınıfının `CallHistoryController` ve tuşuna basın **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/13new.png "Değişiklik CallHistoryController Tablo görünümü denetleyicileri sınıfı")

  Adlı bir özel yedekleme Sınıf Tasarımcısı iOS oluşturacak `CallHistoryController` bu ekran yönetmek için kullanıcının içerik hiyerarşisini görüntüleme.
  **CallHistoryController.cs** dosya görünür **çözüm paneli**:

  ![](hello-ios-multiscreen-quickstart-images/14new.png "Çözüm paneli CallHistoryController.cs dosyasında")


13. Çift **CallHistoryController.cs** dosyasını açın ve içeriğini aşağıdaki kodla değiştirin:

  ```csharp
  using System;
  using Foundation;
  using UIKit;
  using System.Collections.Generic;

  namespace Phoneword_iOS
  {
      public partial class CallHistoryController : UITableViewController
      {
          public List<string> PhoneNumbers { get; set; }

          static NSString callHistoryCellId = new NSString ("CallHistoryCell");

          public CallHistoryController (IntPtr handle) : base (handle)
          {
              TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
              TableView.Source = new CallHistoryDataSource (this);
              PhoneNumbers = new List<string> ();
          }

          class CallHistoryDataSource : UITableViewSource
          {
              CallHistoryController controller;

              public CallHistoryDataSource (CallHistoryController controller)
              {
                  this.controller = controller;
              }

              public override nint RowsInSection (UITableView tableView, nint section)
              {
                  return controller.PhoneNumbers.Count;
              }

              public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
              {
                  var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                  int row = indexPath.Row;
                  cell.TextLabel.Text = controller.PhoneNumbers [row];
                  return cell;
              }
          }
      }
  }
  ```

  Uygulamayı Kaydet (**⌘ + s**) ve bu derleme (**⌘ + b**) hiç hata olmadığından emin olun.


14. Oluşturma bir _Segue_ (arasında geçiş) **Phoneword** Sahne ve **Arama Geçmişi** Sahne.
  İçinde **Phoneword Sahne**seçin **çağrısı Geçmiş düğmesini** ve Ctrl tuşunu gelen **düğmesini** için **Arama Geçmişi** Sahne:

  ![](hello-ios-multiscreen-quickstart-images/15.png "Arama Geçmişi Sahne CTRL-sürükleyin düğmesi")

  Gelen **eylem ü** popover, select **Göster**

  İOS Tasarımcısı Segue iki planda arasında ekleyecek:

  ![](hello-ios-multiscreen-quickstart-images/17new.png "İki planda arasında Segue")


15. Ekleme bir **başlık** için **tablo View Controller** siyah bir çubuk Sahne sonundaki seçerek ve değiştirme **görünüm denetleyicisini başlığı** için **çağırın Geçmiş** içinde **özellikleri paneli**:

  ![](hello-ios-multiscreen-quickstart-images/18new.png "Değişiklik geçmişi özelliklerini defterinde çağırmak için Görünüm denetleyicisini başlığı")

16. Uygulamayı çalıştırdığınızda **çağrısı Geçmiş düğmesini** açılır **Arama Geçmişi** ekran, ancak Tablo görünümünde boş olacaktır izlemek ve telefon numaralarını görüntülemek için hiçbir kod olduğundan.

  Bu uygulamayı telefon numaralarını dizelerinin listesini depolar.

  Ekleme bir `using` için yönerge `System.Collections.Generic` en üstündeki **ViewController**:

  ```csharp
  using System.Collections.Generic;
  ```



17. Değiştirme `ViewController` aşağıdaki kodla sınıfı:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```


Burada gerçekleştiği birkaç şey vardır
  * Değişkeni `translatedNumber` gelen taşınmış `ViewDidLoad` yönteme bir _sınıf düzeyi değişkeni_.
  * **CallButton** kodu ne zaman değiştirildiğini çağırarak Aranan numaralar telefon numaraları listesine eklemek için `PhoneNumbers.Add(translatedNumber)`
  * `PrepareForSegue` Yöntemi eklendi

  Hiçbir hata bulunmadığından emin olmak için uygulama oluşturma ve kaydedin.

20. Tuşuna **Başlat** içinde uygulamayı başlatmak için düğmesini **iOS simülatörü**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "İOS simülatörü'nü içinde uygulamayı başlatmak için Başlat düğmesine basın")


İlk çok ekran Xamarin.iOS uygulamanızı Tamamlanıyor Tebrikler!

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="walkthrough"></a>İzlenecek yol

Bu kılavuz bir arama geçmişi ekrana ekleyeceksiniz bizim **Phoneword** uygulama.


1. Açık **Phoneword** Visual Studio'da uygulama. Gerekirse, indirme [Phoneword uygulama tamamlandı](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) gelen [Hello, iOS izlenecek](~/ios/get-started/hello-ios/index.md) Kılavuzu indirme. Bağlanmak için gerekli olan bir [Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) iOS Tasarımcısı ve iOS simülatörü kullanılacak.


2. Kullanıcı arabirimi düzenleyerek başlayın. Açık **Main.storyboard** dosya **Çözüm Gezgini**, dikkat ederek **görünüm olarak** ayarlanır _iPhone 6_:

  ![](hello-ios-multiscreen-quickstart-images/image1.png "İOS Tasarımcısı'nda Main.storyboard")


3. Sürükleme bir **Gezinti denetleyicisi** gelen **araç** tasarım yüzeyine:

  ![](hello-ios-multiscreen-quickstart-images/image2.png "Bir gezinme denetleyicisi araç tasarım yüzeyine sürükleyin")


4. Sürükleme **Sourceless ü** (sol tarafındaki gri ok olan **Phoneword** Sahne) gelen **Phoneword** için Sahne **Gezinti denetleyicisi** uygulamanın başlangıç noktası değiştirmek için:

  ![](hello-ios-multiscreen-quickstart-images/image3.png "Uygulama başlangıç noktasını değiştirmek için Gezinti denetleyiciye Sourceless ü sürükleyin")


5. Seçin **kök View Controller** siyah bir çubuk ve tuşuna tıklayarak **silmek** tasarım yüzeyden kaldırmak için.
  Ardından, taşımak **Phoneword** yanındaki Sahne **Gezinti denetleyicisi**:

  ![](hello-ios-multiscreen-quickstart-images/image4.png "Gezinti denetleyicisi yanındaki Phoneword Sahne taşıma")


6. Ayarlama **ViewController** Gezinti denetleyicinin olarak `Root View Controller`. Aşağı basın **Ctrl** anahtarı ve içini tıklatın **Gezinti denetleyicisi**. Mavi bir çizgi görüntülenmesi gerekir. Ardından, hala basılı **Ctrl** anahtar, sürükleyin **Gezinti denetleyicisi** için **Phoneword** Sahne ve sürüm. Bu adlı _Ctrl sürükleyerek_:

  ![](hello-ios-multiscreen-quickstart-images/image5.png "Gezinti denetleyicisinden Phoneword Sahne ve sürüm sürükleyin")


7. İlişki popover kümesine **kök**:

  ![](hello-ios-multiscreen-quickstart-images/image6.png "İlişki için kök olarak")

  **ViewController** artık bizim **Gezinti denetleyicinin kök görünümü denetleyicisi.**


8. Çift **Phoneword** ekran **başlık** çubuk ve değişiklik **başlık** için **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/image7.png "Başlık Phoneword değiştirme")


9. Sürükleme bir **düğmesini** gelen **araç** ve altında yerleştirin **arama düğmesini**. Yeni yapmak için işleyiciler sürükleyin **düğmesini** aynı genişliğe **çağrısı düğmesi**:

  ![](hello-ios-multiscreen-quickstart-images/image8.png "Arama düğmesini aynı genişliğe yap yeni düğmesi")


10. İçinde **özellikleri Explorer**, değiştirme **adı** , **düğmesini** için `CallHistoryButton` değiştirip **başlık** için  **Arama Geçmişi**:

  ![](hello-ios-multiscreen-quickstart-images/image9.png "Düğmenin adı 'CallHistoryButton' ve 'Arama geçmişi için ' başlığı için değiştirme")


11. Oluşturma **Arama Geçmişi** ekran. Gelen **araç**, sürükleyin bir **tablo View Controller** tasarım yüzeyine:

  ![](hello-ios-multiscreen-quickstart-images/image10.png "Bir tablo View Controller tasarım yüzeyine sürükleyin")


12. Seçin **tablo View Controller** Sahne altındaki siyah çubuğunda tıklatarak. İçinde **özellikleri Explorer**, değiştirme **Tablo görünümü denetleyicinin** sınıfının `CallHistoryController` ve tuşuna basın **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/image11.png "Değişiklik CallHistoryController Tablo görünümü denetleyicileri sınıfı")

  Adlı bir özel yedekleme Sınıf Tasarımcısı iOS oluşturacak `CallHistoryController` bu ekran yönetmek için kullanıcının içerik hiyerarşisini görüntüleme.
  **CallHistoryController.cs** dosya görünür **Çözüm Gezgini**:

  ![](hello-ios-multiscreen-quickstart-images/image12.png "Çözüm Gezgini'nde CallHistoryController.cs dosyası")


13. Çift **CallHistoryController.cs** dosyasını açın ve içeriğini aşağıdaki kodla değiştirin:

        using System;
        using Foundation;
        using UIKit;
        using System.Collections.Generic;

        namespace Phoneword
        {
            public partial class CallHistoryController : UITableViewController
            {
                public List<String> PhoneNumbers { get; set; }

                static NSString callHistoryCellId = new NSString ("CallHistoryCell");

                public CallHistoryController (IntPtr handle) : base (handle)
                {
                    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
                    TableView.Source = new CallHistoryDataSource (this);
                    PhoneNumbers = new List<string> ();
                }

                class CallHistoryDataSource : UITableViewSource
                {
                    CallHistoryController controller;

                    public CallHistoryDataSource (CallHistoryController controller)
                    {
                        this.controller = controller;
                    }

                    // Returns the number of rows in each section of the table
                    public override nint RowsInSection (UITableView tableView, nint section)
                    {
                        return controller.PhoneNumbers.Count;
                    }

                    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
                    {
                        var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                        int row = indexPath.Row;
                        cell.TextLabel.Text = controller.PhoneNumbers [row];
                        return cell;
                    }
                }
            }
        }

  Uygulamayı kaydedin ve hata olmadığından emin olmak için oluşturun. Şu an için yapı uyarılar yoksayılabilir.


14. Oluşturma bir _Segue_ (arasında geçiş) **Phoneword** Sahne ve **Arama Geçmişi** Sahne.
  İçinde **Phoneword Sahne**seçin **çağrısı Geçmiş düğmesini** ve **Ctrl tuşunu** gelen **düğmesini** için **Arama Geçmişi**  Sahne:

  ![](hello-ios-multiscreen-quickstart-images/image13.png "Arama Geçmişi Sahne CTRL-sürükleyin düğmesi")

  Gelen **eylem ü** popover, select **Göster**:

  ![](hello-ios-multiscreen-quickstart-images/image14.png "Göster segue türü seçin")

  İOS Tasarımcısı Segue iki planda arasında ekleyecek:

  ![](hello-ios-multiscreen-quickstart-images/image15.png "İki planda arasında Segue")


15. Ekleme bir **başlık** için **tablo View Controller** siyah bir çubuk Sahne sonundaki seçerek ve değiştirme **View Controller > başlık** için **çağırın Geçmiş** içinde **özellikleri Explorer**:

  ![](hello-ios-multiscreen-quickstart-images/image16.png "Arama Geçmişi için Görünüm denetleyicisini başlığını değiştirme")


16. Uygulamayı çalıştırdığınızda **çağrısı Geçmiş düğmesini** açılır **Arama Geçmişi** ekran, ancak Tablo görünümünde boş olacaktır izlemek ve telefon numaralarını görüntülemek için hiçbir kod olduğundan.

  Bu uygulamayı telefon numaralarını dizelerinin listesini depolar.

  Ekleme bir `using` için yönerge `System.Collections.Generic` en üstündeki **ViewController**:

        using System.Collections.Generic;

17. Değiştirme `ViewController` aşağıdaki kodla sınıfı:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

Burada gerçekleştiği birkaç şey vardır
  * Değişkeni `translatedNumber` gelen taşınmış `ViewDidLoad` yönteme bir _sınıf düzeyi değişkeni_.
  * **CallButton** kodu ne zaman değiştirildiğini çağırarak Aranan numaralar telefon numaraları listesine eklemek için `PhoneNumbers.Add(translatedNumber)`
  * `PrepareForSegue` Yöntemi eklendi

  Hiçbir hata bulunmadığından emin olmak için uygulama oluşturma ve kaydedin.

  Hiçbir hata bulunmadığından emin olmak için uygulama oluşturma ve kaydedin.


20. Tuşuna **Başlat** uygulamamız içinde Başlat düğmesi **iOS simülatörü**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "Örnek uygulamayı ilk ekran")


İlk çok ekran Xamarin.iOS uygulamanızı Tamamlanıyor Tebrikler!


-----

Uygulamayı şimdi iki film şeridi Segues kullanarak gezinti işleyebilir ve kod. Araçlar ve beceriler dissect artık yalnızca konusunda öğrendiğiniz [Hello, iOS Multiscreen derinlemesine](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Merhaba, iOS (örnek)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS İnsan Arabirimi yönergelerine](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS sağlama portalı](https://developer.apple.com/ios/manage/overview/index.action)
