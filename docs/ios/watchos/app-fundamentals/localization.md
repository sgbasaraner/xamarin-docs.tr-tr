---
title: "Yerelleştirme ile çalışma"
description: "Birden çok dil için watchOS uygulamalarınızı uyarlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9ad3499a232e5f2b2ef362f772ed0197e71e6bee
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-localization"></a>Yerelleştirme ile çalışma

_Birden çok dil için watchOS uygulamalarınızı uyarlama_

![](localization-images/both-languages-sml.png "Apple Watch yerelleştirilmiş içeriği görüntüleme")

watchOS uygulamalar standart iOS yöntemleri kullanarak yerelleştirilmiş:

- Kullanarak **yerelleştirme kimliği** film şeridi öğelerde,
- **.strings** film şeridi ile ilişkili dosyaları ve
- **Localizable.strings** dosyaları için kod içinde kullanılan metin.

Varsayılan film şeritleri ve kaynakları bulunur bir **temel** dizini ve dile özgü çevirileri ve diğer kaynakları depolanır **.lproj** dizinleri.
iOS ve izleme OS kullanıcının dil seçimi doğru dizeler ve kaynakları yüklemek için otomatik olarak kullanır.

Bir Apple Watch uygulama olduğundan iki - izleme uygulama ve izleme uzantısı - bölümleri yerelleştirilmiş dize kaynaklarını nasıl kullanıldıkları bağlı olarak iki yerde gereklidir.

Yerelleştirilmiş metin ve kaynakları *farklı* izleme uygulama ve izleme uzantısı.

## <a name="watch-app"></a>Uygulama izleme

İzleme uygulaması uygulamanın kullanıcı arabirimi açıklanmaktadır film şeridi içerir. Herhangi bir denetim (gibi `Label` ve `Image`) destek yerelleştirme sahip bir **yerelleştirme kimliği**.

Her dile özgü **.lproj** dizin içermelidir **.strings** çevirileri dosyalarıyla her öğe için (kullanarak **yerelleştirme kimliği**), görüntüleri yanı sıra Film şeridi tarafından başvurulan.

## <a name="watch-extension"></a>İzleme uzantısı

Uygulama kodunuzun çalıştığı izleme uzantısıdır. Koddan kullanıcıya görüntülenen herhangi bir metin uzantısı'nda ve izleme uygulamasında yerelleştirilmiş gerekiyor.

Uzantı dile özgü içermesi gereken **.lproj** dizinleri, ancak **.strings** dosyaları yalnızca iste çevirileri kodunuzda kullanılan metin.

## <a name="globalizing-the-watch-solution"></a>İzleme çözümü Genelleştirme

Genelleştirme uygulama yerelleştirilebilir hale getirme işlemidir.
Film şeridi farklı metin uzunluktaki aklınızda tasarlama yani izleme uygulamaları için her ekranı düzeni sağlama bağlı olarak hangi metin görüntülenir ayarlar uygun şekilde. Ayrıca izleme uzantısı kodda başvurulan dizeleri kullanarak dönüştürülür sağlamak zorunda `LocalizedString` yöntemi.

### <a name="watch-app"></a>Uygulama izleme

Varsayılan olarak, yerelleştirme için izleme uygulama yapılandırılmamış. Varsayılan film şeridi dosya taşıma ve bazı diğer dizinleri, çevirileri için oluşturmanız gerekir:

1. Oluşturma **Base.lproj** dizin ve taşıma **Interface.storyboard** içine.

2. Oluşturma  **<language>.lproj** desteklemek istediğiniz her dil için dizinler.

3. **.Lproj** dizinleri içermelidir bir **Interface.strings** metin dosyası (dosya adı storboard kişinin adı eşleşmelidir). İsteğe bağlı olarak bu dizinlerde yerelleştirme gerektiren tüm görüntüleri yerleştirebilirsiniz.

(Dosyalar eklenirken yalnızca İngilizce ve İspanyolca dil) bu değişiklikler yapıldıktan sonra izleme uygulama projesi şöyle görünür:

  ![](localization-images/watchapp-solution.png "İngilizce ve İspanyolca dil dosyalarını izleme uygulama projesi")

#### <a name="storyboard-text"></a>Film şeridi metin

Film şeridi düzenlediğinizde, her bir öğe ve bildirim seçmeniz **yerelleştirme kimliği** , görünür **özellikleri** paneli:

  [![](localization-images/storyboard-sml.png "Özellikler panelinde görünüyor yerelleştirme kimliği")](localization-images/storyboard.png#lightbox)

İçinde **Base.lproj** klasörü, anahtar-değer çiftleri burada anahtar tarafından biçimlendirildiğinden, aşağıda gösterildiği gibi oluşturun **yerelleştirme kimliği** ve denetimi, bir özellik adı noktayla alanına (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

Bu örnekte dikkat edin, bir **yerelleştirme kimliği** (ör. basit bir numara dize olabilir "0", "1", vb.) veya daha karmaşık bir dize (örneğin, "AgC-eL-Hgc"). `Label` denetimleriniz bir `Text` özelliği ve `Button`s sahip bir `Title` yerelleştirilmiş değerlerine ayarlanır - yolla yansıtılır özelliği yukarıdaki örnekte gösterildiği gibi küçük özellik adı kullandığınızdan emin.

Film şeridi saatin işlendiğinde kullanılacak doğru değerleri otomatik olarak ayıklanır ve kullanıcı tarafından seçilen dile göre görüntülenir.

#### <a name="storyboard-images"></a>Film şeridi görüntüleri

Örnek bir çözüm de içeren bir  **gradient@2x.png**  her dil klasöründe görüntü. Bu görüntü (ör. her dil için farklı olabilir çevirme gereken metin katıştırılmış sahip veya kullanım yansır yerelleştirilmiş).

Yalnızca görüntü ayarlama **görüntü** film şeridi ve doğru görüntüyü özelliğinde işlenmediğinde kullanıcı tarafından seçilen dile göre izleme.

![](localization-images/storyboard-image.png "Görüntüleri film şeridi resim özelliği ayarlı")

Not: tüm Apple saatlerde Retina görüntüler, yalnızca olduğundan  **@2x**  görüntü sürümü gereklidir. Belirtmeniz gerekmez  **@2x**  film şeridi içinde.

### <a name="watch-extension"></a>İzleme uzantısı

İzleme uzantısı yerelleştirme, ancak yoktur hiçbir film şeridi desteklemek için benzer bir dizin yapısı gerektirir. Yerelleştirilmiş uzantısı'nda yalnızca C# kodu tarafından başvurulan dizelerdir.

![](localization-images/watchextension-solution.png "Yerelleştirme desteklemek için izleme uzantısı dizin yapısı")

#### <a name="strings-in-code"></a>Kod dizeleri

**Localizable.strings** dosya görsel taslak haline getirme ile ilişkili olup olmadığını daha biraz farklı bir yapıya sahip. Bu durumda biz herhangi bir "anahtarı" dizesini seçebilirsiniz; Apple'nın varsayılan dilde görüntülenen gerçek metin yansıtan bir anahtar kullanmak için önerilir:

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString` Yöntemi dizeleri çevrilen dekiler çözmek için kullanılan aşağıdaki kodda gösterildiği gibi.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>Kod görüntülerde

Kod tarafından doldurulur görüntüleri iki yolla ayarlanabilir.

1. Değiştirebileceğiniz bir `Image` denetim ayarlayarak değeri görüntünün bir dize adı zaten var. izleme uygulamada ör

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. Gözcü kullanmaya uzantısından bir görüntü taşıyabilirsiniz `FromBundle` ve uygulama kullanıcının dil seçimi için doğru görüntüyü otomatik olarak seçer. Örnek bir çözüm yoktur görüntüyü  **language@2x.png**  her dil klasörünü ve onu görüntülenir `DetailController` aşağıdaki kodu kullanarak:

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  Belirtmeniz gerekmez Not  **@2x**  görüntünün filename söz konusu olduğunda.

İkinci yöntem, ayrıca saatin işlemek için uzak bir sunucudan bir görüntü yüklerseniz geçerlidir; Ancak bu durumda, indirdiğiniz resim kullanıcı tercihlerine göre doğru yerelleştirilmiş emin olun.

## <a name="localization"></a>Yerelleştirme

Çözümünüzü yapılandırdıktan sonra çevirmenler işlemek gerekir, **.strings** dosyaları ve desteklemek istediğiniz her bir dilin görüntüler.

Oluşturabileceğiniz kadar **.lproj** yazarken dizinleri (desteklenen her dil için bir tane) gerekir. Dil kodlarını kullanarak adlandırıldığı **tr**, **es**, **de**, **ja**, **pt-BR**, vb. (için İngilizce İspanyolca, Almanca, Japonca ve Portekizce (Brezilya) sırasıyla).

Ekli örnek (makine oluşturulur) çevirileri watchOS uygulama yerelleştirme nasıl yapılacağını göstermek üzere kullanır.

### <a name="watch-app"></a>Uygulama izleme

Bu değerler, izleme uygulamanın film şeridi tanımlanan kullanıcı arabirimi çevirmek için kullanılır. *Anahtar* değeri her film şeridi denetimin birleşimidir **yerelleştirme kimliği** ve çevrildiğini özelliği.

Çeviri olması gereken çevirmenler bilmesi dosyasının özgün metni içeren açıklamaları eklemek için önerilir.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

Film şeridi için (makine) İspanyolca dil dizeleri aşağıda verilmiştir. Bilmesi zor olduğundan, her satır için yorum eklemek yararlıdır **yerelleştirme kimliği** aksi referans veriyor:

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>İzleme uzantısı

Bu değerleri kodda önce kullanıcıya görüntülenen bilgileri çevirmek için kullanılır. *Anahtar* kodu yazarken geliştirici tarafından seçilir ve genellikle gerçek dize çevrilmesi içerir.

#### <a name="eslprojlocalizablestrings-file"></a>es.lproj/Localizable.strings file

(Makine) Spansish dil dizeleri:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>Sınama

Dil Tercihleri değiştirmek için yöntemi simulator ve fiziksel cihazları arasında farklılık gösterir.

### <a name="simulator"></a>Simulator

Simulator'da iOS kullanarak test etmek için dilini seçin. **ayarları** uygulama (gri kullanılan simge simulator giriş ekranında).

  ![](localization-images/sim-settings-sml.png "İOS ayarları uygulama yerelleştirme ayarları")

### <a name="watch-device"></a>Gözcü cihaz

İzleme ile sınarken İzleme'nin dilde değiştirme **Apple Watch** eşleştirilmiş iPhone uygulama.

  ![](localization-images/phone-settings-sml.png "Eşleştirilmiş iPhone Apple Watch uygulama izleme'nin dilini değiştirme")



## <a name="related-links"></a>İlgili bağlantılar

- [WatchLocalization (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
