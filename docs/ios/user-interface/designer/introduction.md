---
title: "iOS Tasarımcısı temelleri"
description: "Bu kılavuz, iOS için Xamarin Tasarımcısı tanıtır. İOS Tasarımcısı denetimleri görsel olarak düzenlemek için nasıl kullanılacağı, bu denetimleri kodda erişmek nasıl ve özelliklerini düzenlemek nasıl gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 3046d779239076098a8b2fb74fc87e2f211074e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-basics"></a>iOS Tasarımcısı temelleri

_Bu kılavuz, iOS için Xamarin Tasarımcısı tanıtır. İOS Tasarımcısı denetimleri görsel olarak düzenlemek için nasıl kullanılacağı, bu denetimleri kodda erişmek nasıl ve özelliklerini düzenlemek nasıl gösterir._

İOS için Xamarin Tasarımcısı Xcode'nın arabirimi Oluşturucu için benzer bir görsel bir arabirim Tasarımcısı ve Android Tasarımcısı olur. Bazı özelliklerinin Mac ve Visual Studio 2015 ve 2017 için Visual Studio, sürükle ve bırak düzenleme, olay işleyicileri kurmak için bir arabirim ve özel denetimler oluşturma olanağı ile entegrasyon içerir.

## <a name="requirements"></a>Gereksinimler

İOS Tasarımcısı Windows Mac için Visual Studio ve Visual Studio 2015 ve 2017 kullanılabilir. Xcode çalışmıyor ancak Visual Studio 2015 veya 2017 Tasarımcısı iOS düzgün yapılandırılmış bir Mac yapı ana bağlantısı gerektirir.

Bu kılavuzda ele içeriği bilindiğini varsayar [Başlarken kılavuzları](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>İOS Tasarımcısı nasıl çalışır?

Bu bölümde, bir kullanıcı arabirimi oluşturma ve kodu bağlanma Tasarımcısı iOS nasıl kolaylaştırır açıklanmaktadır.

Bir uygulamanın kullanıcı arabirimini görsel olarak tasarlamak geliştiricilerin iOS Tasarımcısı sağlar. Kısmında özetlendiği gibi [film şeritleri giriş](~/ios/user-interface/storyboards/index.md) Kılavuzu, film şeridi açıklayan bir uygulaması, yaptığınız ekranlar (Görünüm denetleyicileri) Bu görünüm denetleyicileri ve uygulamanın Genel gezinti akış yerleştirilen arabirim öğeleri (görünümler) . 

Bir görünüm denetleyicisini iki bölümden oluşur: iOS Tasarımcısı görsel bir sunumdur ve ilişkili bir C# sınıfı:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Bir görünüm denetleyicisi Tasarımcısı iOS](introduction-images/1-storyboardwithviewcontroller-vsmac.png "Tasarımcısı iOS bir görünüm denetleyicisi")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png)

[![Bir görünüm denetleyicisini kodunu](introduction-images/2-viewcontrollercode-vsmac.png "bir görünüm denetleyicisi için kod")](introduction-images/2-viewcontrollercode-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bir görünüm denetleyicisi Tasarımcısı iOS](introduction-images/1-storyboardwithviewcontroller-vs.png "Tasarımcısı iOS bir görünüm denetleyicisi")](introduction-images/1-storyboardwithviewcontroller-vs-large.png)

[![Bir görünüm denetleyicisini kodunu](introduction-images/2-viewcontrollercode-vs.png "bir görünüm denetleyicisi için kod")](introduction-images/2-viewcontrollercode-vs-large.png)

-----

Varsayılan durumundayken bir görünüm denetleyicisini tüm işlevleri sağlamaz; denetimler ile doldurulması gerekir. Bu denetimler görünüm denetleyicisinin görünümü, tüm ekran içeriğinin içeren dikdörtgen yerleştirilir. Çoğu görünüm denetleyicileri bir düğmeyi içeren bir görünüm denetleyicisini gösterir aşağıdaki ekran görüntüsünde gösterildiği gibi düğmeleri, etiketler ve metin alanları gibi ortak denetimler içerir: 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Bir düğmeyi içeren bir görünüm denetleyicisini](introduction-images/3-viewcontrollerwithbutton-vsmac.png "bir düğmeyi içeren bir görünüm denetleyicisi")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bir düğmeyi içeren bir görünüm denetleyicisini](introduction-images/3-viewcontrollerwithbutton-vs.png "bir düğmeyi içeren bir görünüm denetleyicisi")](introduction-images/3-viewcontrollerwithbutton-vs-large.png)

-----

Statik metin içeren etiketler gibi bazı denetimler, görünüm denetleyiciye eklenir ve tek başına sol. Ancak, daha sık çok değil, denetimlerini programlı olarak özelleştirilmelidir. Örneğin, bir olay işleyicisi kodunda eklenmesi gerekir böylece tıklar düğmesi dokunduğunuz olduğunda, bir şey yapmanız gerekir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Erişim ve kod düğmesini işlemek için benzersiz bir tanımlayıcı içermelidir. Açma düğmesini seçerek benzersiz bir tanımlayıcı belirtin **özellikleri paneli**ve ayarı kendi **adı** "SubmitButton" gibi bir değer alanı:

[![Düğmenin adı özellikleri defterinde ayarlama](introduction-images/4-settingbuttonname-vsmac.png "özellikleri defterinde düğmenin adı ayarlama")](introduction-images/4-settingbuttonname-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Erişim ve kod düğmesini işlemek için benzersiz bir tanımlayıcı içermelidir. Açma düğmesini seçerek benzersiz bir tanımlayıcı belirtin **Özellikler penceresini**ve ayarı kendi **adı** "SubmitButton" gibi bir değer alanı:

[![Düğmenin adı Özellikler penceresinde ayarlama](introduction-images/4-settingbuttonname-vs.png "Özellikler penceresinde düğmenin adı ayarlama")](introduction-images/4-settingbuttonname-vs-large.png)

-----

Düğme bir ada sahip, kodda erişilebilir. Ancak bu nasıl çalışır?

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

İçinde **çözüm paneli**, gezinme için **ViewController.cs** ve ortaya açığa gösterge üzerinde tıklatarak Görünüm denetleyicinin `ViewController` sınıf tanımı yayılma iki dosyaları, her biri içeren bir [parçalı sınıf](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) tanımı:

[![İki ViewController sınıfı oluşturan dosyaları: ViewController.cs ve ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "iki ViewController sınıfı oluşturan dosyaları: ViewController.cs ve ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

İçinde **Çözüm Gezgini**, gezinme için **ViewController.cs** ve ortaya açığa gösterge üzerinde tıklatarak Görünüm denetleyicinin `ViewController` sınıf tanımını yayılan her iki dosya içeren bir [parçalı sınıf](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) tanımı:

[![İki ViewController sınıfı oluşturan dosyaları: ViewController.cs ve ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "iki ViewController sınıfı oluşturan dosyaları: ViewController.cs ve ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png)

-----

- **ViewController.cs** ilgili özel kod ile doldurulması gerekir `ViewController` sınıfı. Bu dosyadaki `ViewController` sınıfı görünüm denetleyicisini yaşam döngüsü yöntemleri için çeşitli iOS yanıt, kullanıcı arabirimini özelleştirmek ve düğmesi dokunur gibi giriş kullanıcıya yanıt.

- **ViewController.designer.cs** iOS görsel olarak oluşturulan arabirimi kodu eşlemek için tasarımcısı tarafından oluşturulan oluşturulmuş bir dosya değil. Bu dosyadaki değişiklikler üzerine beri değiştirilmemelidir. Bu dosya özelliği bildirimlerinde olanaklı hale getirir kodunu `ViewController` erişimi, göre sınıf **adı**, denetimleri kümesini yukarı iOS Tasarımcısı. Açma **ViewController.designer.cs** aşağıdaki kodu ortaya çıkarır:

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

`SubmitButton` Özellik bildirimi bağlanan tüm `ViewController` sınıfı değil - yalnızca **ViewController.designer.cs** dosyasına – film şeridi tanımlanmış düğme. Bu yana **ViewController.cs** parçası tanımlar `ViewController` sınıfı, erişimi olduğundan `SubmitButton`.

IntelliSense şimdi tanıdığı aşağıdaki ekran görüntüsü gösterilmektedir `SubmitButton` başvuru **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![IntelliSense SubmitButton başvuru algılamayı](introduction-images/6-submitbuttonintellisense-vsmac.png "SubmitButton başvuru algılamayı IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IntelliSense SubmitButton başvuru algılamayı](introduction-images/6-submitbuttonintellisense-vs.png "SubmitButton başvuru algılamayı IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png)

-----

Bu bölümde gösterilmiştir bir düğme Tasarımcısı iOS oluşturma ve bu düğme kodda erişim.

Bu belgenin geri kalanında Tasarımcısı iOS başka bir genel bakış sağlar.

## <a name="ios-designer-basics"></a>iOS Tasarımcısı temelleri

Bu bölüm iOS Tasarımcısı bölümlerini tanıtır ve gezinti özellikleri sağlar.

### <a name="launching-the-ios-designer"></a>İOS Tasarımcısı başlatma

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio ile oluşturulan Xamarin.iOS projeleri film şeridi içerir. Film şeridi içeriğini görüntülemek için .storyboard dosyasına çift tıklayarak **çözüm paneli**:

[![Film şeridi açmak iOS Tasarımcısı](introduction-images/7-storyboardopen-vsmac.png "film şeridi açmak iOS Tasarımcısı")](introduction-images/7-storyboardopen-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2015 veya 2017 ile oluşturulan çoğu Xamarin.iOS projeleri film şeridi içerir. Film şeridi içeriğini görüntülemek için .storyboard dosyasına çift tıklayarak **Çözüm Gezgini**:

[![Film şeridi açmak iOS Tasarımcısı](introduction-images/7-storyboardopen-vs.png "film şeridi açmak iOS Tasarımcısı")](introduction-images/7-storyboardopen-vs-large.png)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS Tasarımcısı özellikleri

İOS Tasarımcısı altı birincil bölümü vardır:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![İOS Tasarımcısı bölümlerini](introduction-images/8-sixpartsofiosdesigner-vsmac.png "Tasarımcısı iOS bölümleri")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png)

1. **Tasarım yüzeyini** – iOS tasarımcının birincil çalışma. Belge alanında gösterilen, kullanıcı arabirimleri, görsel yapımı sağlar.
2. **Kısıtlamaları araç** – sağlayan bir kullanıcı arabirimi öğeleri konumlandırmak için modu ve kısıtlama düzenleme modu, iki farklı şekilde düzenleyerek çerçevesini arasında geçiş yapmak için.
3. **Araç kutusu** – listeler denetleyicileri, nesneler, denetimler, veri görünümleri, hareketi tanıyıcıları, windows ve bu çubuklarını tasarım yüzeyine sürüklenen ve bir kullanıcı arabirimi eklenir.
4. **Özellikler paneli** – kimlik, görsel stiller, erişilebilirlik, Düzen ve davranış dahil olmak üzere seçili denetim özelliklerini gösterir.
5. **Belge Anahattı** – düzenlenmekte arabirimi düzenini oluşturma denetimleri ağaç gösterir. İOS Tasarımcısı seçer ve özelliklerini gösterir ağacındaki bir öğeyi tıklayarak **özellikleri paneli**. Bu, belirli bir denetim fazla iç içe kullanıcı arabiriminde seçmek için kullanışlıdır.
6. **Alt araç** – Tasarımcısı iOS aygıtı, Yönlendirme ve yakınlaştırma gibi .storyboard veya .xib dosya biçimini değiştirmek için seçenekleri içerir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![İOS Tasarımcısı bölümlerini](introduction-images/8-sixpartsofiosdesigner-vs.png "Tasarımcısı iOS bölümleri")](introduction-images/8-sixpartsofiosdesigner-vs-large.png)

1. **Tasarım yüzeyini** – iOS tasarımcının birincil çalışma. Belge alanında gösterilen, kullanıcı arabirimleri, görsel yapımı sağlar.
2. **Kısıtlamaları araç** – sağlayan bir kullanıcı arabirimi öğeleri konumlandırmak için modu ve kısıtlama düzenleme modu, iki farklı şekilde düzenleyerek çerçevesini arasında geçiş yapmak için.
3. **Araç kutusu** – listeler denetleyicileri, nesneler, denetimler, veri görünümleri, hareketi tanıyıcıları, windows ve bu çubuklarını tasarım yüzeyine sürüklenen ve bir kullanıcı arabirimi eklenir.
4. **Özellikler penceresi** – kimlik, görsel stiller, erişilebilirlik, Düzen ve davranış dahil olmak üzere seçili denetim özelliklerini gösterir.
5. **Belge Anahattı** – düzenlenmekte arabirimi düzenini oluşturma denetimleri ağaç gösterir. İOS Tasarımcısı seçer ve özelliklerini gösterir ağacındaki bir öğeyi tıklayarak **Özellikler penceresini**. Bu, belirli bir denetim fazla iç içe kullanıcı arabiriminde seçmek için kullanışlıdır.
6. **Alt araç** – Tasarımcısı iOS aygıtı, Yönlendirme ve yakınlaştırma gibi .storyboard veya .xib dosya biçimini değiştirmek için seçenekleri içerir.

-----

### <a name="design-workflow"></a>İş akışı tasarım

#### <a name="adding-a-control-to-the-interface"></a>Arabirimine denetim ekleme

Bir denetim için bir arabirim eklemek için buradan sürükleyin **araç** ve tasarım yüzeyine bırakın. Ekleme ya da bir denetim konumlandırma dikey ve yatay yönergeleri dikey Merkezi, yatay ortada ve kenar boşlukları gibi yaygın olarak kullanılan düzeni konumlar vurgula:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
![Tasarım yüzeyine yönergeleri yaygın olarak kullanılan düzeni konumlar vurgulayın](introduction-images/9-layoutguides-vsmac.png "tasarım yüzeyine yönergeleri yaygın olarak kullanılan düzeni konumlar vurgulayın.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Tasarım yüzeyine yönergeleri yaygın olarak kullanılan düzeni konumlar vurgulayın](introduction-images/9-layoutguides-vs.png "tasarım yüzeyine yönergeleri yaygın olarak kullanılan düzeni konumlar vurgulayın.")

-----

Yukarıdaki örnekte mavi noktalı çizgi ile düğmesi yerleşimi yardımcı olmak için bir yatay ortada visual hizalama kılavuz sağlar.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

#### <a name="context-menu-commands"></a>Bağlam menü komutları

Bir bağlam menüsü tasarım yüzeyine hem de kullanılabilir **belge anahattı**. Bu menü komutlarını sağlar Seçili denetim ve iç içe geçmiş bir hiyerarşideki görünümlerle çalışırken yararlı, kendi üst:

[![Tasarım yüzeyine bağlam menüsünde](introduction-images/10-contextmenudesignsurface-vsmac.png "tasarım yüzeyine bağlam menüsü")](introduction-images/10-contextmenudesignsurface-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Kısıtlamaları araç çubuğu

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
[![Contraints araç](introduction-images/11-constraintstoolbar-vsmac.png "kısıtlamaları araç çubuğu")](introduction-images/11-constraintstoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Contraints araç](introduction-images/11-constraintstoolbar-vs.png "kısıtlamaları araç çubuğu")](introduction-images/11-constraintstoolbar-vs-large.png)

-----

Kısıtlamaları araç güncelleştirildi ve artık iki denetimlerin oluşur: düzenleme modunda çerçeve / kısıtlaması düzenleme modu Değiştir ve güncelleştirme kısıtlamaları / güncelleştirmesi çerçeveler düğmesi.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Düzenleme modunda çerçeve / kısıtlaması düzenleme modu Değiştir

İOS Tasarımcısı önceki sürümlerde tasarım yüzeyine zaten seçilmiş bir görünümü tıklatarak düzenleme modu ve kısıtlama düzenleme modunu çerçeve arasında yükseğe. Şimdi, iki durumlu düğme denetim kısıtlamaları araç çubuğunda düzenleme modunu bunlar arasında geçiş yapar.

- Çerçeve düzenleme modu:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Düzenleme Modu düğmesini çerçeve](introduction-images/12a-frameeditingmode-vsmac.png "düzenleme modu düğmesini çerçeve")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Düzenleme Modu düğmesini çerçeve](introduction-images/12a-frameeditingmode-vs.png "düzenleme modu düğmesini çerçeve")

-----

- Kısıtlama düzenleme modu:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Düzenleme Modu düğmesini kısıtlaması](introduction-images/12b-constrainteditingmode-vsmac.png "kısıtlaması düzenleme modu düğmesi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Düzenleme Modu düğmesini kısıtlaması](introduction-images/12b-constrainteditingmode-vs.png "kısıtlaması düzenleme modu düğmesi")

-----

#### <a name="update-constraints--update-frames-button"></a>Kısıtlamaları güncelleştirme / güncelleştir çerçeveler düğmesi

Güncelleştirme kısıtlamaları / güncelleştirme çerçeve düzenleme modunda çerçeve sağındaki düğmesi yer alır / kısıtlaması düzenleme modu Değiştir.

- Düzenleme modunda çerçevesinde, bu düğmeye tıkladığınızda, kendi kısıtlamalarla eşleşen tüm seçili öğeleri çerçeveler ayarlar.
- Kısıtlama düzenleme modunda bu düğmeye tıkladığınızda, kendi çerçeveler eşleştirmek için herhangi bir seçili öğe kısıtlamaları ayarlar.

### <a name="bottom-toolbar"></a>Alt kısımdaki araç

Alt kısımdaki araç aygıt, Yönlendirme ve iOS Tasarımcısı film şeridi veya .xib bir dosyayı görüntülemek için kullanılan yakınlaştırma seçmek için bir yol sağlar:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Bir cihaz ve tasarım yüzeyine yönünü seçmek için kullanılan alt kısımdaki araç](introduction-images/13-bottomtoolbar-vsmac.png "bir cihaz ve tasarım yüzeyine yönünü seçmek için kullanılan alt araç çubuğu")](introduction-images/13-bottomtoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bir cihaz ve tasarım yüzeyine yönünü seçmek için kullanılan alt kısımdaki araç](introduction-images/13-bottomtoolbar-vs.png "bir cihaz ve tasarım yüzeyine yönünü seçmek için kullanılan alt araç çubuğu")](introduction-images/13-bottomtoolbar-vs-large.png)

-----

#### <a name="device-and-orientation"></a>Aygıt ve yönü

Genişletildiğinde, alt kısımdaki araç tüm cihazlar, yönler ve/veya uyarlamalar geçerli belgeye uygulanabilir görüntüler. Tıklatarak tasarım yüzeyine görüntülenen görünümü değiştirir. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Alt kısımdaki araç genişletilmiş aygıtları ve yönleri gösterecek şekilde](introduction-images/14-bottomtoolbarexpanded-vsmac.png "aygıtları ve yönleri gösterecek şekilde Genişletilmiş Alt araç çubuğu")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alt kısımdaki araç genişletilmiş aygıtları ve yönleri gösterecek şekilde](introduction-images/14-bottomtoolbarexpanded-vs.png "aygıtları ve yönleri gösterecek şekilde Genişletilmiş Alt araç çubuğu")](introduction-images/14-bottomtoolbarexpanded-vs-large.png)

-----

Bir cihaz seçip yönü yalnızca nasıl Tasarımcısı iOS tasarım önizlemeleri sağlanır değiştiğine dikkat edin. Geçerli seçim bağımsız olarak, kısıtlamalar uygulanır tüm cihazlar ve yönler sürece yeni eklenen **Düzenle nitelikler** düğmesi kullanıldığından aksi belirtmek için.

Zaman [boyut sınıfları](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) olan [etkin](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), **Düzenle nitelikler** düğmesi, genişletilmiş alt araç çubuğunda görünür.  Tıklatarak **Düzenle nitelikler** düğmesi yönlendirmesini ve seçilen aygıt tarafından temsil edilen boyutu sınıfı dayalı bir arabirim değişim oluşturma seçeneklerini görüntüler. Aşağıdaki örnekler göz önünde bulundurun:

- Varsa **iPhone SE** / **dikey**, olan seçili popover compact genişlik, normal Yükseklik boyutu sınıfı için bir arabirimi çeşitlemesi oluşturmak için seçenekler sağlar. 
- Varsa **iPad Pro 9.7"** / **yatay** / **tam ekran** olan seçili popover için bir arabirimi çeşitlemesi oluşturmak için seçenekler sunan Normal genişlik, normal Yükseklik boyutu sınıfı.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Arabirim değiştirecek şekilde boyutu sınıfı tarafından kullanılan alt kısımdaki araç](introduction-images/15-edittraitsbutton-vsmac.png "bir arabirim değiştirecek şekilde boyutu sınıfı tarafından kullanılan alt araç çubuğu")](introduction-images/15-edittraitsbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Arabirim değiştirecek şekilde boyutu sınıfı tarafından kullanılan alt kısımdaki araç](introduction-images/15-edittraitsbutton-vs.png "bir arabirim değiştirecek şekilde boyutu sınıfı tarafından kullanılan alt araç çubuğu")](introduction-images/15-edittraitsbutton-vs-large.png)

-----

#### <a name="zoom-controls"></a>Yakınlaştırma denetimleri

Tasarım yüzeyi birkaç denetimleri yakınlaştırma destekler:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
![Alt kısımdaki araç yakınlaştırma denetimlerinde](introduction-images/16-zoomcontrols-vsmac.png "alt kısımdaki araç yakınlaştırma denetimleri")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alt kısımdaki araç yakınlaştırma denetimlerinde](introduction-images/16-zoomcontrols-vs.png "alt kısımdaki araç yakınlaştırma denetimleri")

-----

Denetimleri aşağıdakileri içerir:

1. Uyacak şekilde Yakınlaştır
2. Uzaklaştır
3. Yakınlaştır
4. Gerçek Boyut (1:1 piksel boyutu)

Bu denetimler, tasarım yüzeyine yakınlaştırmanın. Çalışma zamanında uygulama kullanıcı arabiriminin etkilemez.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="properties-pad"></a>Özellikler paneli

Kullanım **özellikleri paneli** kimlik, görsel stiller, erişilebilirlik ve davranışını düzenlemek için. Aşağıdaki ekran görüntüsü gösterilmektedir **özellikleri paneli** düğmesi için seçenekleri:

[![Düğme Özellikleri paneli](introduction-images/17-buttonpropertiespad-vsmac.png "özellikleri paneli düğmesi")](introduction-images/17-buttonpropertiespad-vsmac-large.png)
#### <a name="properties-pad-sections"></a>Özellikler paneli bölümleri

**Özellikleri paneli** üç bölümleri içerir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Özellikler Penceresi

Kullanım **Özellikler penceresini** kimlik, görsel stiller, erişilebilirlik ve davranışını düzenlemek için. Aşağıdaki ekran görüntüsü gösterilmektedir **Özellikler penceresini** düğmesi için seçenekleri:

[![Düğme için Özellikler penceresini](introduction-images/17-buttonpropertieswindow-vs.png "düğmesi için Özellikler penceresi")](introduction-images/17-buttonpropertieswindow-vs-large.png)

#### <a name="properties-window-sections"></a>Özellikler penceresi bölümleri

**Özellikler penceresini** üç bölümleri içerir:

-----

1.  **Pencere öğesi** – ana adı, sınıf, stil özellikleri, vb. gibi denetimin özelliklerini. Denetimin içeriğini yönetmeye yönelik özellikleri genellikle burada yerleştirilir.
2.  **Düzen** – konumu ve boyutu kısıtlamaları ve çerçeveleri gibi denetimin izlemek özellikleri burada listelenir.
3.  **Olayları** – olayların ve olay işleyicileri belirtilen burada. Dokunma, vb. dokunun, sürükle olayları işlemek için kullanışlıdır. Olayları doğrudan kodunda ele alınabilir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Özellikler panelinde özelliklerini düzenleme

Tasarım yüzeyine görsel düzenleme yanı sıra iOS Tasarımcısı destekler özelliklerinde düzenleme **özellikleri paneli**. Kullanılabilir özellikler aşağıdaki ekran görüntüleri gösterildiği gibi seçili denetime bağlı olarak değişir:

[![Düğme Özellikleri](introduction-images/18a-buttonpropertiespad-vsmac.png "düğme özellikleri")](introduction-images/18a-buttonpropertiespad-vsmac-large.png)

[![Denetleyici özellikleri görüntüle](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "Denetleyicisi Özellikleri Görüntüle")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Özellikleri penceresinde özelliklerini düzenleme

Tasarım yüzeyine görsel düzenleme yanı sıra iOS Tasarımcısı destekler özelliklerinde düzenleme **Özellikler penceresini**. Kullanılabilir özellikler aşağıdaki ekran görüntüleri gösterildiği gibi seçili denetime bağlı olarak değişir:

[![Düğme Özellikleri](introduction-images/18a-buttonpropertieswindow-vs.png "düğme özellikleri")](introduction-images/18a-buttonpropertieswindow-vs-large.png)

[![Denetleyici özellikleri görüntüle](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "Denetleyicisi Özellikleri Görüntüle")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png)

-----

> [!IMPORTANT]
> Özellikler paneli şimdi gösterir kimlik bölümünde bir **Modülü** alan. Bu bölümde yalnızca SWIFT sınıfları ile birlikte çalışırken doldurmak gereklidir. Modül adı namespaced olan SWIFT sınıfları için girmek için kullanın.

#### <a name="default-values"></a>Varsayılan değerler

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Birçok özelliklerinde **özellikleri paneli** herhangi bir değer veya varsayılan bir değer gösterir. Ancak, uygulamanın kodunu hala bu değerleri değiştirebilir. **Özellikleri paneli** değerleri kodda göstermez.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Birçok özelliklerinde **Özellikler penceresini** herhangi bir değer veya varsayılan bir değer gösterir. Ancak, uygulamanın kodunu hala bu değerleri değiştirebilir. **Özellikler penceresini** değerleri kodda göstermez.

-----

#### <a name="event-handlers"></a>Olay işleyicileri

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Çeşitli olayları için özel olay işleyicileri belirtmek için kullanın **olayları** sekmesinde **özellikleri paneli**. Örneğin, aşağıdaki ekran görüntüsünde bir `HandleClick` yöntemi işler düğmenin **Touch içinde** olay:

[![Özellikler paneliyle düğmesi için ayarlanan bir olay işleyicisi](introduction-images/19-buttonpropertiespadevents-vsmac.png "düğmesi için ayarlanan bir olay işleyicisi ile özellikleri paneli")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Çeşitli olayları için özel olay işleyicileri belirtmek için kullanın **olayları** sekmesinde **Özellikler penceresini**. Örneğin, aşağıdaki ekran görüntüsünde bir `HandleClick` yöntemi işler düğmenin **Touch içinde** olay:

[![Özellikler penceresi düğmesi için ayarlanan bir olay işleyicisi ile](introduction-images/19-buttonpropertieswindowevents-vs.png "düğmesi için ayarlanan bir olay işleyicisi ile Özellikler penceresi")](introduction-images/19-buttonpropertieswindowevents-vs-large.png)

-----

Olay işleyici belirtilen sonra aynı ada sahip bir yöntem karşılık gelen görünüm denetleyici sınıfına eklenmesi gerekir. Aksi halde, bir `unrecognized selector` özel durum düğmesi dokunduğunuz olduğunda oluşur:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tanınmayan Seçici istisna](introduction-images/20-unrecognizedselector-vsmac.png "tanınmayan seçici özel durumu")](introduction-images/20-unrecognizedselector-vsmac-large.png)

Olay işleyici sonra içinde belirtilen unutmayın **özellikleri paneli**, iOS Tasarımcısı hemen karşılık gelen kod dosyasını açın ve yöntem bildirimi eklemesini sağlar. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tanınmayan Seçici istisna](introduction-images/20-unrecognizedselector-vs.png "tanınmayan seçici özel durumu")](introduction-images/20-unrecognizedselector-vs-large.png)

-----

Özel olay işleyicileri kullanan bir örnek için bkz [Hello, iOS Başlarken Kılavuzu](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Anahat görünümü

İOS Tasarımcısı, anahat olarak denetimleri hiyerarşisini bir arabirimin de görüntüleyebilirsiniz. Anahat seçilerek kullanılabilir **belge anahattı** sekmesinde, aşağıda gösterildiği gibi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Belge Anahattı](introduction-images/21-buttonoutlineview-vsmac.png "Belge Anahattı")](introduction-images/21-buttonoutlineview-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Belge Anahattı](introduction-images/21-buttonoutlineview-vs.png "Belge Anahattı")](introduction-images/21-buttonoutlineview-vs-large.png)

-----

Anahat görünümünde seçili Denetim tasarım yüzeyine Seçili denetimi ile eşitlenmiş olarak tutulur.  Bu özellik, bir iç içe arabirimi hiyerarşisinden öğeyi seçmek için kullanışlıdır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="revert-to-xcode"></a>Xcode için geri

İOS Tasarımcısı ve Xcode arabirimi Oluşturucu birbirinin yerine kullanmak da mümkündür. Film şeridi veya .xib dosyası Xcode arabirimi Oluşturucusu'nda açmak için dosyayı sağ tıklatın ve **birlikte Aç > Xcode arabirimi Oluşturucu**aşağıdaki ekran görüntüsüne gösterildiği gibi:

[![Film şeridi Xcode arabirimi Oluşturucusu'nda açmayı](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode arabirimi Oluşturucusu'nda bir film şeridi açma")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png)

Xcode arabirimi Oluşturucusu'nda Düzenlemeleri yaptıktan sonra dosyayı kaydedin ve Mac için Visual Studio'ya geri dönün Değişiklikleri Xamarin.iOS projesi eşitler.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>.xib desteği

İOS Tasarımcısı oluşturma, düzenleme ve .xib dosyalarını yönetme destekler. Bu uygulamanın görünümü hiyerarşisine eklenebilir, respresent tek, özel görünümler XML dosyalarıdır. Film şeridi birçok ekranları ve bunları arasındaki geçişler temsil eder ancak .xib dosyası genellikle tek bir görünüm veya bir uygulamadaki ekran arabirimini temsil eder.

Çözüm – .xib dosyaları, film şeridi veya kod – oluşturmak ve bir kullanıcı arabirimi sürdürmek için en iyi çalıştığı birçok görüşlerini vardır. Gerçekte, mükemmel bir çözüm ve iş elinizdeki için en iyi aracı dikkate değer her zaman olur. Bu, .xib dosyaları özel tablo görünümü hücresi gibi bir uygulama birden çok yerde gereken özel bir görünüm temsil etmek üzere kullanıldığında genellikle en güçlü belirtti. 

İçinde aşağıdaki tarif .xib dosyalarını kullanma hakkında daha fazla belge bulunabilir:

- [Şablonu görüntüleme .xib kullanma](https://developer.xamarin.com/recipes/ios/general/templates/using_the_ios_view_xib_template/)
- [Özel bir .xib kullanarak bir TableViewCell oluşturma](https://developer.xamarin.com/recipes/ios/content_controls/tables/custom-tableviewcell/)
- [Başlatma bir .xib kullanarak ekran oluşturma](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)

Film şeritleri kullanılmasıyla ilgili daha fazla bilgi için bkz [film şeritleri giriş](~/ios/user-interface/storyboards/index.md).

Bu ve diğer iOS Tasarımcısı ile ilgili kılavuzları çoğu Xamarin.iOS yeni proje şablonları film şeridi varsayılan olarak sağlar. bu yana kullanıcı arabirimleri oluşturmak için film şeritleri kullanımını standart yaklaşımı olarak bakın.

## <a name="summary"></a>Özet

Bu kılavuz özelliklerini açıklayan ve anahat güzel kullanıcı arabirimleri tasarlama için sunduğu araçları Tasarımcısı iOS bir giriş sağlanır.

## <a name="related-links"></a>İlgili bağlantılar

- [Film şeritleri giriş](~/ios/user-interface/storyboards/index.md)
- [iOS Designable denetimleri gözden geçirme](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Merhaba, iOS](~/ios/get-started/hello-ios/index.md)
- [Merhaba, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android Tasarımcı genel bakış](~/android/user-interface/android-designer/index.md)
- [Parçalı Sınıflar ve Yöntemler](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [İOS - 2014 (video) gelişmesi için Xamarin tasarımcısı içine Dalma](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Başlatma ekranı (video) oluşturmak için iOS Tasarımcısı kullanma](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
