---
title: Xamarin.iOS metin girişi
description: Bu belgede bir Xamarin.iOS uygulaması metin girişi açıklanmaktadır. Bu program aracılığıyla hem Tasarımcısı iOS UITextField ve UITextVIew kullanarak açıklanır.
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5d8648f5830a7adcd32d253b92fae45098f12a83
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790220"
---
# <a name="text-input-in-xamarinios"></a>Xamarin.iOS metin girişi

Kullanıcı metin girişi kabul ile gerçekleştirilir `UITextField` tek satırlı girişleri ve çok satırlı düzenlenebilir metin UITextView. Bu denetimlerin ya da bir ekrana sürükleyin ve ilk metin ayarlamak için çift tıklayın.

Aşağıdaki ekran görüntüleri Mac için Visual Studio araç takımı bulunan bu denetimleri simgelerini göster:

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

Çıkış adlı sonra film şeridi dosya kaydedilemedi, Mac için Visual Studio güncelleştirecektir `.designer.cs` parçalı sınıf ve sınıf dosyanıza denetimine başvuruda bulunan C# kodu ekleyebilirsiniz. Her denetim kendi benzersiz özellikleri ve C# kodunuzda erişilebilir olayları sahiptir.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

`UITextField` Denetimi tek satırlık bir kullanıcı adı veya parola gibi metin girişi kabul etmek için en sık kullanılır. Bazı denetimi özelleştirmek için kullanılabilir seçenekler burada gösterilir:

 [![](text-input-images/image15a.png "UITextField özellikleri")](text-input-images/image15a.png#lightbox)

Bu denetimler, aşağıda açıklanmıştır:

-  **Yer tutucu** – bu seçenek isteğe bağlıdır. Metin alanı genellikle kullanıcıya hangi giriş beklenen açıklamak için boş olduğunda ayarlanırsa, bu görüntüleniyorsa.
-  **Temizle düğmesi** – bu denetimleri zaman standart Temizle'yi (gri daire (X) ile) kullanıcının metin hızla temizlemek bir yol olarak metin alanında görüntülenir. Kalıcı olarak gizli, kalıcı olarak görünür veya alan desteklemediğini düzenleniyor bağlı olarak gösterilen olabilir.
-  **En küçük yazı tipi boyutu** ve **sığacak şekilde ayarla** – uzun metnin sığması ve kesme önlemek için otomatik olarak ayarlanmış, ancak bunlarla sınırlı yazı tipi boyutunu Hayır belirtilen boyuttan daha küçük sağlar.
-  **Büyük/küçük harf** – sözcükler, cümleleri veya tüm giriş sağladığınızdan otomatik olarak mı yoksa.
-  **Düzeltme** – yazım denetimi ve öneriler etkinleştirilip etkinleştirilmediğini.
-  **Klavye** – denetimleri klavye stili görüntülenen için giriş ve bu nedenle klavyede hangi anahtarları kullanılabilir. Bu, diğer seçenekleri yanı sıra numarası paneli, telefon defteri, e-posta, URL içerir.
-  **Görünüm** – klavye Görünüm stilini denetler ve her iki koyu veya kaldırılacak konulu açık.
-  **Return tuşunun** – daha iyi ne gerçekleştirilecek yansıtmak için Return tuşuna etikette değiştirin. Desteklenen değerler Git, birleştirme, İleri, Bitti, yol ve arama içerir.
-  **Güvenli** – giriş maskelenir tanımlar (Bu tür bir parola girişi için olduğu gibi).


Bir UITextField çağrıldıklarında `textfield1` eklendi bir ekran Tasarımcısı ile ayarlamak veya C# özelliklerini aşağıdaki gibi değiştirin:

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS sağlar numaralandırmalar gibi istediğiniz ayarları seçin kolaylaştırmak uygun olan yerlerde `UIKeyboardType` ve `UIReturnKeyType` Yukarıdaki kod parçacığında bulunan.

### <a name="display-text-programmatically"></a>Program aracılığıyla metin görüntüleme

Ekran Tasarımcısı ile tasarım istemediğiniz veya bazı metinleri çalışma zamanında dinamik olarak eklemek istiyorsanız, oluşturmak ve bir UITextField program aracılığıyla da görüntülemek `ViewDidLoad` şuna benzer bir görünüm denetleyicisinin yöntemi:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

`UITextView` Denetimi salt okunur metin görüntülemek veya birden çok satırlı metin girişi kabul etmek için kullanılabilir. Aynı seçenekleri çoğunu sahip `UITextField` (büyük/küçük harf gibi düzeltme, vb.).

 [![](text-input-images/image16a.png "UITextView özellikleri")](text-input-images/image16a.png#lightbox)

Belirli özellikler şunları içerir:

-  **Davranış** – metni düzenlenebilir veya salt okunur olup olmadığını.
-  **Algılama** – Detects ve tıklatılabilir öğeleri girilen verileri bir çağrı tetikleyebilir telefon numaralarını adresleri gibi hale haritalar, Safari ile açmak URL'ler için bağlantıları veya tarihler ve saatlere, dönüştürür takvimde olayları olur.


Bir UITextView ekran Tasarımcısı ile eklenmişse ayarlayabilir veya özelliklerini bu gibi değiştirebilirsiniz:

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
