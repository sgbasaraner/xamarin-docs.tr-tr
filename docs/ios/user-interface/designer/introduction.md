---
title: iOS Designer bilgileri
description: Bu kılavuz, iOS için Xamarin Tasarımcı sunar. Bu, iOS Tasarımcısı görsel olarak denetimleri düzenlemek için nasıl kullanılacağını ve kodda bu denetimlerin nasıl özelliklerini düzenlemek nasıl gösterir.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 6905eddbc4488b08f9c9e896efe5f980e0e03345
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242374"
---
# <a name="ios-designer-basics"></a>iOS Designer bilgileri

_Bu kılavuz, iOS için Xamarin Tasarımcı sunar. Bu, iOS Tasarımcısı görsel olarak denetimleri düzenlemek için nasıl kullanılacağını ve kodda bu denetimlerin nasıl özelliklerini düzenlemek nasıl gösterir._

İOS için Xamarin Tasarımcı Xcode'un arabirim Oluşturucu için benzer bir görsel arabirim tasarımcı ve Android Designer ' dir. Bazı özelliklerinin Mac ve Visual Studio 2015 ve 2017 için Visual Studio, sürükle ve bırak düzenleme, bir arabirim olay işleyicilerini ayarlama ve özel denetimler oluşturma olanağı ile entegrasyon içerir.

## <a name="requirements"></a>Gereksinimler

İOS Tasarımcısı Windows üzerinde Mac için Visual Studio ve Visual Studio 2015 ve 2017'de kullanılabilir olur. Xcode çalışmıyor ancak Visual Studio 2015 veya 2017, iOS Designer düzgün bir şekilde yapılandırılmış bir Mac derleme konağı bağlantısı gerektirir.

Bu kılavuzda ele içeriği bilindiğini varsayar [Başlarken kılavuzları](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>İOS Designer nasıl çalışır?

Bu bölümde, bir kullanıcı arabirimi oluşturmak ve kodu bağlanarak iOS Designer nasıl kolaylaştıran açıklanmaktadır.

İOS Designer, geliştiricilerin bir uygulamanın kullanıcı arabirimini görsel olarak tasarlamanıza olanak tanır. Açıklandığı şekilde [görsel taslaklara giriş](~/ios/user-interface/storyboards/index.md) Kılavuzu, bir görsel taslak açıklayan bir uygulamayı oluşturan ekranları (Görünüm denetleyicisi) Bu görünüm denetleyicileri ve uygulamanın Genel gezinti akış yer arabirim öğeleri (görünümleri) . 

Görünüm denetleyicisi iki bölümden oluşur: iOS Designer'daki görsel gösterimi ve ilişkili bir C# sınıfı:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![İOS Designer'daki görünüm denetleyicisi](introduction-images/1-storyboardwithviewcontroller-vsmac.png "iOS Designer'daki görünüm denetleyicisi")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Görünüm denetleyicisi kodunu](introduction-images/2-viewcontrollercode-vsmac.png "kodunu bir görünüm denetleyicisi")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![İOS Designer'daki görünüm denetleyicisi](introduction-images/1-storyboardwithviewcontroller-vs.png "iOS Designer'daki görünüm denetleyicisi")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Görünüm denetleyicisi kodunu](introduction-images/2-viewcontrollercode-vs.png "kodunu bir görünüm denetleyicisi")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

Varsayılan durumda, bir görünüm denetleyicisi herhangi bir işlevsellik sağlamaz; denetimler ile doldurulması gerekir. Bu denetimler, ekranın içeriğin tümünü içeren dikdörtgen alan görünümü denetleyicinin Görünümü'nde yerleştirilir. Bir düğme içeren bir görünüm denetleyicisi gösteren aşağıdaki ekran görüntüsünde gösterildiği gibi çoğu görünüm denetleyicileri düğmeler, etiketler ve metin alanları gibi ortak denetimler içerir: 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Bir düğme içeren bir görünüm denetleyicisi](introduction-images/3-viewcontrollerwithbutton-vsmac.png "bir düğme içeren bir görünüm denetleyicisi")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bir düğme içeren bir görünüm denetleyicisi](introduction-images/3-viewcontrollerwithbutton-vs.png "bir düğme içeren bir görünüm denetleyicisi")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Statik metin içeren etiket gibi bazı denetimler, görünüm denetleyiciye eklenir ve tek başına sol. Ancak, daha fazla değildir, denetimleri program aracılığıyla özelleştirilmelidir. Örneğin, bir olay işleyici kodu eklenmesi için yukarıda eklediğiniz düğmeye dokunduğunuzda bir şey yapmanız gerekir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Kod düğmeye erişim ve düzenleme için benzersiz bir tanımlayıcı olmalıdır. Açma düğmesini seçerek benzersiz bir tanımlayıcı sağlar **özellikler panelinde**ve ayarı kendi **adı** "SubmitButton" gibi bir değer alanı:

[![Ayar Özellikler panelinde bir düğmenin adı](introduction-images/4-settingbuttonname-vsmac.png "özellikler panelinde bir düğmenin adı ayarlama")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kod düğmeye erişim ve düzenleme için benzersiz bir tanımlayıcı olmalıdır. Açma düğmesini seçerek benzersiz bir tanımlayıcı sağlar **Özellikler penceresi**ve ayarı kendi **adı** "SubmitButton" gibi bir değer alanı:

[![Özellikler penceresinde bir düğmenin adı ayarlama](introduction-images/4-settingbuttonname-vs.png "Özellikler penceresinde bir düğmenin adı ayarlama")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Düğmeyi bir ada sahip, kod içinde erişilebilir. Ancak bu nasıl çalışır?

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

İçinde **çözüm bölmesi**, gezinme için **ViewController.cs** ve ortaya açığa gösterge üzerinde tıklayarak görünümü denetleyicinin `ViewController` sınıf tanımı yayılma iki dosyaları, her biri içeren bir [kısmi sınıf](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) tanımı:

[![İki ViewController sınıfı oluşturan dosyalar: ViewController.cs ve ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "iki ViewController sınıfı oluşturan dosyalar: ViewController.cs ve ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

İçinde **Çözüm Gezgini**, gezinme için **ViewController.cs** ve ortaya açığa gösterge üzerinde tıklayarak görünümü denetleyicinin `ViewController` sınıf tanımı yayılan her iki dosya içeren bir [kısmi sınıf](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) tanımı:

[![İki ViewController sınıfı oluşturan dosyalar: ViewController.cs ve ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "iki ViewController sınıfı oluşturan dosyalar: ViewController.cs ve ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** ilgili özel kod ile doldurulmalıdır `ViewController` sınıfı. Bu dosyadaki `ViewController` sınıf görünümü denetleyicisi yaşam döngüsü yöntemleri için çeşitli iOS yanıt, kullanıcı arabirimini özelleştirme ve düğmesine dokunduğunda gibi giriş kullanıcıya yanıt.

- **ViewController.designer.cs** iOS görsel olarak oluşturulan arabirimi için kod eşlemek için tasarımcı tarafından oluşturulan bir oluşturulan dosya. Bu dosyada yapılan değişikliklerin üzerine yazılır olduğundan, değiştirilmemelidir. Bu dosyadaki özellik bildirimleri olanaklı hale getirir, kod için `ViewController` göre erişimi, sınıf **adı**, denetimleri kümesini ayarlama iOS Designer'daki. Açma **ViewController.designer.cs** aşağıdaki kodu gösterir:

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

`SubmitButton` Özellik bildiriminde bağlanan tüm `ViewController` sınıfı değil - yalnızca **ViewController.designer.cs** dosyasına – film şeridi tanımlanmış düğme. Bu yana **ViewController.cs** parçası tanımlar `ViewController` sınıfı erişimi olan `SubmitButton`.

Aşağıdaki ekran görüntüsünde IntelliSense şimdi algıladığını gösterir `SubmitButton` başvuru **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![IntelliSense SubmitButton başvuru algılamayı](introduction-images/6-submitbuttonintellisense-vsmac.png "SubmitButton başvuru algılamayı IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IntelliSense SubmitButton başvuru algılamayı](introduction-images/6-submitbuttonintellisense-vs.png "SubmitButton başvuru algılamayı IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

Bu bölümde gösterilmiştir iOS Designer'daki bir düğme oluşturma ve bu düğme kod erişim.

Bu belgenin geri kalanında iOS Designer başka bir genel bakış sağlar.

## <a name="ios-designer-basics"></a>iOS Designer bilgileri

Bu bölümde iOS Designer parçalarını tanıtır ve tura özellikleri sağlar.

### <a name="launching-the-ios-designer"></a>İOS Designer başlatılıyor

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Xamarin.iOS projeleri Mac için Visual Studio ile oluşturulmuş bir görsel taslak içerir. Görsel taslak içeriğini görüntülemek için .storyboard dosyasına çift tıklayın **çözüm bölmesi**:

[![İOS Tasarımcısı bir görsel taslağı Aç](introduction-images/7-storyboardopen-vsmac.png "iOS Tasarımcısı bir görsel taslağı Aç")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Çoğu Xamarin.iOS projeleri Visual Studio 2015 veya 2017 ile oluşturulmuş bir görsel taslak içerir. Görsel taslak içeriğini görüntülemek için .storyboard dosyasına çift tıklayın **Çözüm Gezgini**:

[![İOS Tasarımcısı bir görsel taslağı Aç](introduction-images/7-storyboardopen-vs.png "iOS Tasarımcısı bir görsel taslağı Aç")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS Tasarımcısı özellikleri

İOS Designer altı birincil bölümü vardır:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![İOS Designer bölümlerini](introduction-images/8-sixpartsofiosdesigner-vsmac.png "iOS Designer bölümleri")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Tasarım yüzeyine** – iOS Tasarımcısı birincil çalışma alanı. Belge alanında gösterilen görsel yapımı kullanıcı arabirimleri sağlar.
2. **Kısıtlamalar araç çubuğu** – çerçeve içinde bir kullanıcı arabirimi öğeleri konumlandırmak için modu ve kısıtlama düzenleme modu, iki farklı şekilde düzenleme arasında geçiş yapmak için izin verir.
3. **Araç kutusu** – listeler denetleyicileri, nesneleri, denetimler, veri görünümleri, hareket tanıyıcılar, windows ve çubukları, sürüklenen tasarım yüzeyine sürükleyin ve bir kullanıcı arabirimine eklendi.
4. **Özellikleri paneli** – kimlik, görsel stilleri, erişilebilirlik, Düzen ve davranış da dahil olmak üzere, seçilen denetimin özelliklerini gösterir.
5. **Belge Anahattı** – düzenlenmekte olan arabirimi düzenini oluşturan denetimlerin ağacını gösterir. Bir öğe ağacında tıklayarak iOS Designer'daki seçer ve onun özelliklerini gösteren **özellikler panelinde**. Bu, bir iç içe geçmiş kullanıcı arabiriminde belirli bir denetimi seçmek için kullanışlıdır.
6. **Alt araç çubuğu** – iOS Designer, cihaz, Yönlendirme ve yakınlaştırma gibi .storyboard veya .xib dosya biçimini değiştirmek için seçenekler içerir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![İOS Designer bölümlerini](introduction-images/8-sixpartsofiosdesigner-vs.png "iOS Designer bölümleri")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Tasarım yüzeyine** – iOS Tasarımcısı birincil çalışma alanı. Belge alanında gösterilen görsel yapımı kullanıcı arabirimleri sağlar.
2. **Kısıtlamalar araç çubuğu** – çerçeve içinde bir kullanıcı arabirimi öğeleri konumlandırmak için modu ve kısıtlama düzenleme modu, iki farklı şekilde düzenleme arasında geçiş yapmak için izin verir.
3. **Araç kutusu** – listeler denetleyicileri, nesneleri, denetimler, veri görünümleri, hareket tanıyıcılar, windows ve çubukları, sürüklenen tasarım yüzeyine sürükleyin ve bir kullanıcı arabirimine eklendi.
4. **Özellikler penceresi** – kimlik, görsel stilleri, erişilebilirlik, Düzen ve davranış da dahil olmak üzere, seçilen denetimin özelliklerini gösterir.
5. **Belge Anahattı** – düzenlenmekte olan arabirimi düzenini oluşturan denetimlerin ağacını gösterir. Bir öğe ağacında tıklayarak iOS Designer'daki seçer ve onun özelliklerini gösteren **Özellikler penceresi**. Bu, bir iç içe geçmiş kullanıcı arabiriminde belirli bir denetimi seçmek için kullanışlıdır.
6. **Alt araç çubuğu** – iOS Designer, cihaz, Yönlendirme ve yakınlaştırma gibi .storyboard veya .xib dosya biçimini değiştirmek için seçenekler içerir.

-----

### <a name="design-workflow"></a>İş akışı tasarlayın

#### <a name="adding-a-control-to-the-interface"></a>Arabirimi denetim ekleme

Bir arabirim için bir denetim eklemek için ondan sürükleyin **araç kutusu** ve tasarım yüzeyine bırakın. Ekleme veya bir denetimin konumlandırma, dikey ve yatay yönergeleri merkezi dikey, yatay merkezine ve kenar boşlukları gibi Düzen sık kullanılan konumlar vurgular:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
![Tasarım yüzeyinde, Düzen sık kullanılan konumlar yönergeleri vurgulayın](introduction-images/9-layoutguides-vsmac.png "tasarım yüzeyinde yönergeleri Düzen sık kullanılan konumlar vurgulayın.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Tasarım yüzeyinde, Düzen sık kullanılan konumlar yönergeleri vurgulayın](introduction-images/9-layoutguides-vs.png "tasarım yüzeyinde yönergeleri Düzen sık kullanılan konumlar vurgulayın.")

-----

Yukarıdaki örnekte mavi noktalı çizgi düğmesi yerleştirmenize yardımcı olması için bir yatay merkezine visual hizalama kılavuz sağlar.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

#### <a name="context-menu-commands"></a>Bağlam menüsü komutları

Tasarım yüzeyinde hem de bağlam menüsünde kullanılabilir **belge anahattı**. Bu menü komutlarını sağlar. seçili denetime ve kendi üst iç içe hiyerarşisini görünümlerde ile çalışırken yararlıdır:

[![Tasarım yüzeyinde bağlam menüsü](introduction-images/10-contextmenudesignsurface-vsmac.png "tasarım yüzeyinde bağlam menüsü")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Kısıtlamalar araç çubuğu

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
[![Kısıtlamaları araç](introduction-images/11-constraintstoolbar-vsmac.png "kısıtlamalar araç çubuğu")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kısıtlamaları araç](introduction-images/11-constraintstoolbar-vs.png "kısıtlamalar araç çubuğu")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

Kısıtlamalar araç çubuğu güncelleştirildi ve artık iki denetimi içerir: düzenleme modunda çerçeve / kısıtlaması düzenleme modunu Aç/Kapat ve güncelleştirme kısıtlamaları / kare düğme güncelleştirin.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Çerçeve düzenleme modu / kısıtlaması düzenleme modunu Aç/Kapat

İOS Designer önceki sürümlerinde, bir tasarım yüzeyinde zaten seçili Görünüm'çerçeve düzenleme modu ve kısıtlama düzenleme modu arasında açılıp. Artık, bir iki durumlu denetimin kısıtlamalar araç çubuğu düzenleme modunu bunlar arasında geçiş yapar.

- Çerçeve düzenleme modu:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Çerçeve düzenleme modu düğmesini](introduction-images/12a-frameeditingmode-vsmac.png "çerçeve düzenleme modu düğmesi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Çerçeve düzenleme modu düğmesini](introduction-images/12a-frameeditingmode-vs.png "çerçeve düzenleme modu düğmesi")

-----

- Kısıtlama düzenleme modu:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Kısıtlama düzenleme modu düğmesini](introduction-images/12b-constrainteditingmode-vsmac.png "kısıtlama düzenleme modu düğmesi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kısıtlama düzenleme modu düğmesini](introduction-images/12b-constrainteditingmode-vs.png "kısıtlama düzenleme modu düğmesi")

-----

#### <a name="update-constraints--update-frames-button"></a>Kısıtlamaları güncelleştir / güncelleştir çerçeveler düğmesi

Güncelleştirme kısıtlamaları / güncelleştirme çerçeve düzenleme modu çerçeve sağındaki düğmesi yer alır / kısıtlaması düzenleme modunu aç/kapat.

- Düzenleme modunda çerçevesinde, bu düğmeye tıklandığında, tüm seçili öğelerin kendi kısıtlamalarla eşleşen çerçeveleri ayarlar.
- Kısıtlama düzenleme modunda, bu düğmeye tıklandığında çerçevelerine eşleştirilecek herhangi bir seçili öğe kısıtlamaları ayarlar.

### <a name="bottom-toolbar"></a>Alt araç çubuğu

Alt araç çubuğunun, cihaz, Yönlendirme ve yakınlaştırma iOS Tasarımcısı bir görsel taslak veya .xib dosyasını görüntülemek için kullanılan seçmek için bir yol sağlar:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Bir cihaz ve tasarım yüzeyi için yönlendirmeyi seçmek için kullanılacak alt araç çubuğunun](introduction-images/13-bottomtoolbar-vsmac.png "bir cihaz ve tasarım yüzeyi için yönlendirmeyi seçmek için kullanılacak alt araç çubuğu")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bir cihaz ve tasarım yüzeyi için yönlendirmeyi seçmek için kullanılacak alt araç çubuğunun](introduction-images/13-bottomtoolbar-vs.png "bir cihaz ve tasarım yüzeyi için yönlendirmeyi seçmek için kullanılacak alt araç çubuğu")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Cihaz ve yönü

Genişletildiğinde, tüm cihazlar, yönlendirmeler ve/veya uyarlamaları geçerli belge için uygun alt araç çubuğunun görüntüler. Bunları tıklayarak tasarım yüzeyinde görüntülenen görünümü değiştirir. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Cihazlar ve yönlendirmelerine göstermek için alt araç çubuğunun Genişletilmiş](introduction-images/14-bottomtoolbarexpanded-vsmac.png "cihazları ve yönlendirmelerine göstermek üzere genişletilip alt araç çubuğu")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihazlar ve yönlendirmelerine göstermek için alt araç çubuğunun Genişletilmiş](introduction-images/14-bottomtoolbarexpanded-vs.png "cihazları ve yönlendirmelerine göstermek üzere genişletilip alt araç çubuğu")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Bir cihaz seçip yönü yalnızca iOS Designer tasarım nasıl Önizleme değiştiğine dikkat edin. Geçerli seçimi bağımsız olarak, eklenen kısıtlamalar uygulanır tüm cihazları ve yönlendirmelerine sürece **nitelikleri Düzenle** düğmesi, aksi takdirde belirtmek için kullanılmıştı.

Zaman [boyut sınıfları](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) olan [etkin](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), **nitelikleri Düzenle** düğmesi, genişletilmiş alt araç çubuğunda görünür.  Tıklayarak **nitelikleri Düzenle** düğmesi seçili cihaz ve yönlendirmesini tarafından temsil edilen boyut sınıfına dayalı bir arabirim Varyasyon oluşturmak için seçenekleri görüntüler. Aşağıdaki örnekleri dikkate alın:

- Varsa **iPhone SE** / **dikey**, olan seçili popover compact genişlik, yükseklik normal boyut sınıfına için bir arabirimi Varyasyon oluşturmak için seçenekler sağlar. 
- Varsa **iPad Pro 9.7"** / **yatay** / **tam ekran** olduğu belirlenirse, popover için bir arabirimi Varyasyon oluşturmak için seçenekler sağlar Normal genişlik, yükseklik normal boyut sınıfı.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Boyut sınıfına göre farklı bir arabirim için kullanılan alt araç çubuğunun](introduction-images/15-edittraitsbutton-vsmac.png "boyut sınıfına göre farklı bir arabirim için kullanılan alt araç çubuğu")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Boyut sınıfına göre farklı bir arabirim için kullanılan alt araç çubuğunun](introduction-images/15-edittraitsbutton-vs.png "boyut sınıfına göre farklı bir arabirim için kullanılan alt araç çubuğu")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Yakınlaştırma denetimleri

Tasarım yüzeyinde, çeşitli denetimler yakınlaştırma destekler:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
![Alt araç çubuğunun yakınlaştırma denetimleri](introduction-images/16-zoomcontrols-vsmac.png "alt araç çubuğunun yakınlaştırma denetimleri")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alt araç çubuğunun yakınlaştırma denetimleri](introduction-images/16-zoomcontrols-vs.png "alt araç çubuğunun yakınlaştırma denetimleri")

-----

Denetimleri aşağıdakileri içerir:

1. Sığacak kadar Yakınlaştır
2. Uzaklaştır
3. Yakınlaştır
4. Gerçek Boyut (1:1 piksel boyutunu)

Bu denetimleri tasarım yüzeyinde yakınlaştırma ayarlayın. Uygulamanın çalışma zamanında kullanıcı arabirimi etkilemez.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="properties-pad"></a>Özellikleri paneli

Kullanım **özellikler panelinde** kimlik, görsel stilleri, erişilebilirlik ve denetiminin davranışını düzenlenecek. Aşağıdaki ekran görüntüsünde gösterilmektedir **özellikler panelinde** düğmesi seçenekleri:

[![Bir düğme için özellikler panelini](introduction-images/17-buttonpropertiespad-vsmac.png "özellikler panelinde düğmesi")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Özellikler panelini bölümler

**Özellikler panelinde** üç bölümleri içerir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Özellikler Penceresi

Kullanım **Özellikler penceresi** kimlik, görsel stilleri, erişilebilirlik ve denetiminin davranışını düzenlenecek. Aşağıdaki ekran görüntüsünde gösterilmektedir **Özellikler penceresi** düğmesi seçenekleri:

[![Bir düğme için Özellikler penceresini](introduction-images/17-buttonpropertieswindow-vs.png "bir düğme için Özellikler penceresi")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Özellikler penceresi bölümler

**Özellikler penceresi** üç bölümleri içerir:

-----

1.  **Pencere öğesi** – denetimin adını, sınıf, stil özellikleri, vb. gibi ana özellikleri. Denetimin içeriğini yönetmek için özellikler genellikle burada yerleştirilir.
2.  **Düzen** – kısıtlamaları ve çerçeveleri de dahil olmak üzere, denetimin boyutunu ve konumunu izlemenize özellikleri burada listelenir.
3.  **Olayları** – olayları ve olay işleyicileri belirtilen burada. Dokunma, vb. dokunun, sürükle olay işleme için kullanışlıdır. Olayları doğrudan kod içinde ele alınabilir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Özellikler panelindeki özelliklerini düzenleme

Görsel tasarım yüzeyinde düzenlemenin yanı sıra iOS Designer destekler özelliklerinde düzenleme **özellikler panelinde**. Kullanılabilir özellikler üzerinde seçili denetimi, aşağıdaki ekran görüntüleri ile gösterildiği gibi bağlı olarak değişir:

[![Düğme Özellikleri](introduction-images/18a-buttonpropertiespad-vsmac.png "düğmesi özellikleri")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Görüntüleme Denetleyicisi Özellikleri](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "Denetleyicisi Özellikleri Görüntüle")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Özellikler penceresinde özelliklerini düzenleme

Görsel tasarım yüzeyinde düzenlemenin yanı sıra iOS Designer destekler özelliklerinde düzenleme **Özellikler penceresi**. Kullanılabilir özellikler üzerinde seçili denetimi, aşağıdaki ekran görüntüleri ile gösterildiği gibi bağlı olarak değişir:

[![Düğme Özellikleri](introduction-images/18a-buttonpropertieswindow-vs.png "düğmesi özellikleri")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Görüntüleme Denetleyicisi Özellikleri](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "Denetleyicisi Özellikleri Görüntüle")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Kimlik bölümünde özellikler panelinde şimdi gösterir bir **Modülü** alan. Bu bölümde yalnızca Swift sınıflarla birlikte çalışırken doldurmak gereklidir. Namespaced olan Swift sınıflar için bir modül adı girmek için kullanın.

#### <a name="default-values"></a>Varsayılan değerler

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Birçok özelliklerinde **özellikler panelinde** herhangi bir değer veya varsayılan değeri gösterir. Ancak, uygulama kodunun yine de bu değerleri değiştirebilir. **Özellikler panelinde** kodda ayarlanmış değerleri göstermez.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Birçok özelliklerinde **Özellikler penceresi** herhangi bir değer veya varsayılan değeri gösterir. Ancak, uygulama kodunun yine de bu değerleri değiştirebilir. **Özellikler penceresi** kodda ayarlanmış değerleri göstermez.

-----

#### <a name="event-handlers"></a>Olay işleyicileri

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Çeşitli olayları için özel olay işleyicileri belirtmek için kullanın **olayları** sekmesinde **özellikler panelinde**. Örneğin, aşağıdaki ekran görüntüsünde, bir `HandleClick` yöntemi işler düğmenin **Touch içinde** olay:

[![Özellikleri paneli, bir düğme için ayarlanmış bir olay işleyicisi ile](introduction-images/19-buttonpropertiespadevents-vsmac.png "düğmesi için ayarlanan bir olay işleyicisi ile özellikleri paneli")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Çeşitli olayları için özel olay işleyicileri belirtmek için kullanın **olayları** sekmesinde **Özellikler penceresi**. Örneğin, aşağıdaki ekran görüntüsünde, bir `HandleClick` yöntemi işler düğmenin **Touch içinde** olay:

[![Özellikler penceresinde, bir düğme için ayarlanmış bir olay işleyicisi ile](introduction-images/19-buttonpropertieswindowevents-vs.png "düğmesi için ayarlanan bir olay işleyicisi ile Özellikler penceresi")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

Bir olay işleyicisi belirtilen sonra aynı ada sahip bir yöntem için karşılık gelen görünüm denetleyicisi sınıfını eklenmesi gerekir. Aksi takdirde, bir `unrecognized selector` düğmeye dokunulduğunda özel durum oluşur:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tanınmayan Seçici istisna](introduction-images/20-unrecognizedselector-vsmac.png "tanınmayan seçici özel durum")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

İçinde belirtilen bundan sonra bir olay işleyicisi Not **özellikler panelinde**, iOS Tasarımcısı hemen karşılık gelen kod dosyasını açın ve yöntem bildiriminde eklemesini sağlar. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tanınmayan Seçici istisna](introduction-images/20-unrecognizedselector-vs.png "tanınmayan seçici özel durum")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Özel olay işleyicileri kullanan bir örnek için bkz [Hello, iOS Başlarken Kılavuzu](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Anahat görünümü

İOS Designer, denetimlerin bir arabirimin hiyerarşi anahat olarak da görüntüleyebilirsiniz. Ana hat seçilerek kullanılabilir **belge anahattı** sekmesinde, aşağıda gösterildiği gibi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Belge Anahattı](introduction-images/21-buttonoutlineview-vsmac.png "Belge Anahattı")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Belge Anahattı](introduction-images/21-buttonoutlineview-vs.png "Belge Anahattı")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Ana görünümünde seçili denetimi tasarım yüzeyinde Seçili denetimi ile eşitlenmiş olarak tutulur.  Bu özellik, bir iç içe arabirimi hiyerarşiden bir öğeyi seçmek için yararlıdır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="revert-to-xcode"></a>Xcode için geri döndürme

İOS Tasarımcısı ve Xcode arabirim Oluşturucu dönüşümlü olarak kullanıp mümkündür. Xcode arabirim oluşturucu içinde bir film şeridi veya .xib dosya açmak için dosyasını sağ tıklatın ve **birlikte Aç > Xcode arabirim Oluşturucu**ekran aşağıda gösterildiği gibi:

[![Xcode içinde arabirim oluşturucu görsel taslak açma](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode arabirim oluşturucu görsel taslak açma")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode arabirim Oluşturucu'da düzenlemeler yaptıktan sonra dosyayı kaydedin ve Mac için Visual Studio'ya geri dönün Xamarin.iOS projesi için değişiklikleri eşitler.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>.xib desteği

İOS Designer, oluşturma, düzenleme ve yönetme .xib dosyaları destekler. Bu uygulama için hiyerarşisini görüntüle eklenebilir, respresent tek ve özel görünümler XML dosyalarıdır. Görsel taslak birçok ekranlar ve bunlar arasındaki geçişleri temsil eder, oysa .xib dosyası genellikle tek bir görünüm veya bir uygulamada ekranı arabirimini temsil eder.

Hakkında – .xib dosyaları, film şeritleri veya kod – çözümü oluşturmak ve bir kullanıcı arabirimi sürdürmek için en uygun birçok fikirlerini vardır. Gerçekte, mükemmel çözümü yoktur ve her zaman en uygun aracı işi eldeki dikkate değer olur. Bu, bir özel tablo görünümü hücresi gibi bir uygulama birden çok yerde gereken özel bir görünümü temsil etmek için kullanıldığında genellikle en güçlü .xib dosyaları belirtti. 

.Xib dosyaları kullanma hakkında daha fazla belge içinde aşağıdaki tarifleri bulunabilir:

- [Görünüm .xib şablonu kullanma](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Özel bir .xib kullanarak bir TableViewCell oluşturma](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Başlatma bir .xib kullanarak bir ekran oluşturma](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

Görsel Taslaklar kullanımına ilişkin daha fazla bilgi için [görsel taslaklara giriş](~/ios/user-interface/storyboards/index.md).

Bu ve diğer iOS Designer ile ilgili Kılavuzlar çoğu Xamarin.iOS yeni proje şablonları, varsayılan olarak bir görsel taslak sağladıklarından dolayı kullanıcı arabirimleri oluşturmak için görsel Taslaklar kullanımını standart bir yaklaşım olarak bakın.

## <a name="summary"></a>Özet

Bu kılavuz, iOS Designer, anahat güzel kullanıcı arabirimleri tasarlama için sunduğu araçları ve özellikleri açıklayan bir giriş sağlanır.

## <a name="related-links"></a>İlgili bağlantılar

- [Görsel Taslaklara Giriş](~/ios/user-interface/storyboards/index.md)
- [iOS Designable denetimleri gözden geçirme](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Çok Ekranlı Hello, iOS ](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android Designer genel bakış](~/android/user-interface/android-designer/index.md)
- [Parçalı Sınıflar ve Yöntemler](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [İOS - 2014 (video) gelişmek için Xamarin Tasarımcı içinde incelemenizi](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [İOS Designer kullanarak başlatma ekran (video) oluşturma](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
