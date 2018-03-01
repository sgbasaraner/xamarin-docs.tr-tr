---
title: "Kullanıcı arabirimi nesneleri oluşturma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 035956f5c39a77c625a6f4cb92cbfa67a42f2402
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-user-interface-objects"></a>Kullanıcı arabirimi nesneleri oluşturma

Apple grupları "hangi Xamarin.iOS ad alanlarına eşitlemek çerçeveleri" işlevsellik parçalarını ilgili. `UIKit` iOS için tüm kullanıcı arabirimi denetimlerini içeren ad alanıdır.

Şunlar kodunuzu bir etiket veya düğmesi gibi bir kullanıcı arabirimi denetim başvuru gerektiğinde unutmayın deyimi kullanarak:

```csharp
using UIKit;
```


Bu bölümde açıklanan tüm denetimleri Uıkit ad alanında bulunur ve her bir kullanıcı denetimi sınıf adı sahip `UI` öneki.

UI denetimleri ve düzenleri üç şekilde düzenleyebilirsiniz:

-  **[Xamarin iOS Tasarımcısı](~/ios/user-interface/designer/index.md)**  – ekranlar tasarlamak için kullanım Xamarin'ın yerleşik düzeni tasarımcısı. Film şeridi veya XIB dosyaları yerleşik Tasarımcısı ile düzenlemek için çift tıklayın.
-  **Xcode arabirimi Oluşturucu** – denetimleri, ekranı düzeni arabirimi Oluşturucu ile üzerine sürükleyin. Film şeridi veya XIB dosyasını Xcode'da dosyasında sağ tıklayarak açın **çözüm paneli** ve seçme **birlikte Aç > Xcode arabirimi Oluşturucu**.
-  **C# kullanarak** – denetimleri ayrıca program aracılığıyla kodu ile oluşturulan ve hiyerarşisini görüntüleme eklendi.

Bir iOS projeye sağ tıklayıp seçerek yeni film şeridi ve XIB dosyaları eklenebilir **Ekle > yeni dosya...** .

Kullandığınız, hangi yöntemi denetim özelliklerini ve olayları yine C# ile uygulama mantığınızın yönetilebilir.

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS Tasarımcısı kullanma

İOS Tasarımcısı Kullanıcı Arabiriminizin oluşturmaya başlamak için bir film şeridi dosyasını çift tıklatın. Denetimleri, tasarım yüzeyine sürüklenebilir **araç** aşağıda gösterildiği gibi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 [ ![](creating-ui-objects-images/image2b.png "Toolbox Pad")](creating-ui-objects-images/image2b.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [ ![](creating-ui-objects-images/image2b-vs.png "Araç kutusu paneli - Visual Stuio")](creating-ui-objects-images/image2b.png)
 
-----

Tasarım yüzeyine bir Denetim seçildiğinde **özellikleri paneli** denetleyen öznitelikleri gösterir. **Pencere öğesi > kimlik > adı** aşağıdaki ekran görüntüsünde doldurulur, alan olarak kullanılan *çıkışı* adı. Bu, C# denetimi nasıl başvurabilir.

 [ ![](creating-ui-objects-images/image3b.png "Özellikler pencere öğesi paneli")](creating-ui-objects-images/image3b.png)

İOS Tasarımcısı'nı kullanarak bir daha ayrıntılı bilgi edinmek için bkz [Tasarımcısı iOS giriş](~/ios/user-interface/designer/introduction.md) Kılavuzu.

## <a name="using-xcode-interface-builder"></a>Xcode arabirimi Oluşturucusu'nu kullanarak

Apple için arabirimi oluşturucu kullanılarak ile tanımıyorsanız başvurun [arabirimi Oluşturucu](https://developer.apple.com/xcode/interface-builder/) belgeleri.

Film şeridi dosya bağlam menüsünü erişmeye ve açmak bir film şeridi Xcode'da açmak için sağ **Xcode arabirimi Oluşturucu**:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 [ ![](creating-ui-objects-images/imagexcode.png "Film şeridi bağlam menüsü - Xcode")](creating-ui-objects-images/imagexcode.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](creating-ui-objects-images/imagexcode-vs.png "Film şeridi bağlam menüsü - Xcode")](creating-ui-objects-images/imagexcode-vs.png)

-----

Denetimleri, tasarım yüzeyine sürüklenebilir **Nesne Kitaplığı** aşağıda gösterilmiştir:

 [ ![](creating-ui-objects-images/image5a.png "Xcode Nesne Kitaplığı")](creating-ui-objects-images/image5a.png)

UI arabirimini oluşturmalısınız Oluşturucu ile tasarlarken bir **çıkışı** , C# başvurusu istediğiniz her denetim için. Bu açarak yapılır **Yardımcısı Düzenleyicisi** Merkezi'ni kullanarak **Düzenleyicisi** Xcode araç çubuğu düğmesi düğmesinde:

 [ ![](creating-ui-objects-images/image6a.png "Yardımcısı Düzenleyici düğmesi")](creating-ui-objects-images/image6a.png)

Bir kullanıcı arabirimi nesnesinde'ı tıklatın; ardından **denetimi Sürükle** .h dosyasına. İçin ** denetimi Sürükle **, CTRL tuşunu basılı tutun sonra tıklayın ve kullanıcı arabirimi nesnesi üzerinde tutmak için çıkış (veya eylem) oluşturmakta olduğunuz. CTRL tuşunu basılı üstbilgi dosyası sürükleyin basılı tutun. Aşağıda sürükleyerek bitiş `@interface` tanımı. Mavi bir çizgi, bir resim yazısı Ekle çıkışı veya çıkışı koleksiyonu, aşağıdaki ekran görüntüsünde gösterildiği gibi görünmelidir.

Öğesini serbest bıraktığınızda başvurulabilir bir C# özellik kodu oluşturmak için kullanılan çıkış için bir ad istenir:

 [ ![](creating-ui-objects-images/image8a.png "Prizine oluşturma")](creating-ui-objects-images/image8a.png)

Nasıl Xcode'nın arabirimi Oluşturucu Mac için Visual Studio ile tümleşir. daha fazla bilgi için başvurmak [Xib kod oluşturma](~/ios/internals/xib-code-generation.md#generated) belge.

##  <a name="using-c"></a>C# kullanarak

C# (', bir görünüm veya örneğin View Controller) kullanarak bir kullanıcı arabirimi nesnesi programlı olarak oluşturmanıza karar verirseniz, aşağıdaki adımları izleyin:

-  Kullanıcı arabirimi nesnesi için bir sınıf düzeyinde alan bildirin. Denetim oluşturma defa, `ViewDidLoad` örneğin. Nesne sonra View Controller (ör. boyunca yaşam döngüsü yöntemleri başvurulabilir
`ViewWillAppear`).
-  Oluşturma bir `CGRect` çerçevesi (X ve Y koordinatları ekran yanı sıra genişlik ve yükseklik üzerinde) denetiminin tanımlar. Sahip olduğunuzdan emin olmak gerekir bir `using CoreGraphics` bu yönerge.
-  Oluşturmak ve denetim atamak için oluşturucusunu çağırın.
-  Herhangi bir özellik veya olay işleyicileri ayarlayın.
-  Çağrı `Add()` hiyerarşisini görüntüleme denetim eklemek için.

İşte oluşturma basit bir örnek bir `UILabel` bir görünüm denetleyicisini C# kullanarak:

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>C# ve film şeritleri kullanma

Görünüm denetleyicileri tasarım yüzeyine eklendiğinde, iki karşılık gelen C# dosyalar projede oluşturulur. Bu örnekte, `ControlsViewController.cs` ve `ControlsViewController.designer.cs` otomatik olarak oluşturulan:

 [ ![](creating-ui-objects-images/image9b.png "ViewController parçalı sınıf")](creating-ui-objects-images/image9b.png)

`MainViewController.cs` Dosya içindir *kodunuzu*. Bu yerdir `View` gibi yaşam döngüsü yöntemler `ViewDidLoad` ve `ViewWillAppear` uygulanır ve ekleyebileceğiniz kendi özellikleri, alanları ve yöntemleri.

`ControlsViewController.designer.cs` Olan oluşturulan kodu içeren bir parçalı sınıf. Tasarım Denetim adı Mac için Visual Studio'da yüzey veya Xcode, karşılık gelen özelliği veya kısmi yöntemi bir çıkış veya eylem oluşturmak, Tasarımcısı (designer.cs) dosyasına eklenir. Aşağıdaki kod iki düğmeler ve bir metin görünümü için oluşturulan kod örneği düğmelerden birini de sahip olduğu gösterir bir `TouchUpInside` olay.

Bu öğeleri kısmi sınıfının denetimlerine başvuruda ve tasarım yüzeyine bildirilen eylemleri yanıtlamak için kodunuzu etkinleştirin:

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

`designer.cs` Dosyası değil el ile düzenlenmesi gerekir – görsel taslak haline getirme ile eşitlenen onu tutmak için (Mac veya Visual Studio için Visual Studio) IDE sorumludur.

Ne zaman kullanıcı arabirimi nesneleri eklenir program aracılığıyla çok bir `View` veya `ViewController`, örneği ve nesne başvuruları kendiniz yönetmek ve bu nedenle herhangi bir tasarımcı dosyası gereklidir.



## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
