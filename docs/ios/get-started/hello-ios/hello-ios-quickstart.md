---
title: Merhaba, iOS – hızlı başlangıç
description: Bu kılavuzda Hello, iOS adlı basit bir Xamarin.iOS uygulamasının nasıl oluşturulacağını gösterir. Bunu yaparken, temel Araçlar ve Xamarin.iOS uygulamaları geliştirmek için anlaşılmalıdır kavramları tanıtır.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: d9f5894f03bbfaa2145aec462dbd0ede7774354a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785992"
---
# <a name="hello-ios--quickstart"></a>Merhaba, iOS – hızlı başlangıç

Bu kılavuz bir sayısal telefon numarası kullanıcı tarafından girilen bir alfasayısal telefon numarasını çevirir bir uygulama oluşturmayı açıklar ve bu numarayı çağırır. Son uygulama şöyle görünür:

 [![](hello-ios-quickstart-images/image1.png "Hello.iOS hızlı başlangıç uygulaması")](hello-ios-quickstart-images/image1.png#lightbox)

<a name="Requirements" />

## <a name="requirements"></a>Gereksinimler

Xamarin iOS geliştirme gerektirir:

-  Mac çalışan macOS Sierra (10,12) veya üstü.
-  SDK Xcode ve iOS en son sürümü yüklü [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) .

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Xamarin.iOS aşağıdaki ayarlar ile çalışır:

-  Visual Studio yukarıdaki belirtimlerine uygun Mac için en son sürümü.

[Xamarin.iOS Mac Yükleme Kılavuzu](~/ios/get-started/installation/mac.md) adım adım yükleme yönergeleri için kullanılabilir

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS aşağıdaki ayarlar ile çalışır:

-  Visual Studio 2017 Community, Professional veya Enterprise Windows 7 veya üzeri, en son sürümünü yukarıdaki belirtimlerine uygun bir Mac yapı konak ile eşlendi.

[Xamarin.iOS Windows Yükleme Kılavuzu](~/ios/get-started/installation/windows/index.md) adım adım yükleme yönergeleri için kullanılabilir.

-----

Başlamadan önce indirme [Xamarin uygulaması simgeler ayarlama](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio Mac gözden geçirme

Bu kılavuz, bir alfasayısal telefon numarası bir sayısal telefon numarası çevirir Phoneword adlı bir uygulama oluşturmak açıklar.

1. Mac için Visual Studio'yu başlatın **uygulamaları** klasör veya **Spotlight** başlatma ekranı getirmek için:

  ![](hello-ios-quickstart-images/image2new.png "Başlatma ekranı")

Başlat ekranında tıklatın **yeni proje...**  yeni bir Xamarin.iOS çözüm oluşturmak için:

![](hello-ios-quickstart-images/image3new.png "iOS çözümü")

2. Gelen **yeni çözüm iletişim**, seçin **iOS > Uygulama > tek görünüm uygulaması** şablonu, C# seçili olduğundan emin olma. Tıklatın **sonraki**:

  ![](hello-ios-quickstart-images/image4new.png "Tek görünüm uygulaması seçin")

3. Uygulamayı yapılandırma. Bu verin **adı** `Phoneword_iOS`ve varsayılan olarak bir şey bırakın. Tıklatın **sonraki**:

  ![](hello-ios-quickstart-images/image5new.png "Uygulama adı girin")

4. Proje ve çözüm olarak adı bırakın. Burada projenin bir konum seçin veya varsayılan olarak tutun:

  ![](hello-ios-quickstart-images/image6new.png "Proje konumunu seçin")

5. Tıklatın **oluşturma** yapmak için **çözüm**.

6. Açık **Main.storyboard** içinde çift tarafından dosya **çözüm paneli**. Bu bir şekilde görsel olarak bir kullanıcı Arabirimi oluşturmak için aşağıdakileri sağlar:

  ![](hello-ios-quickstart-images/image7new.png "İOS Tasarımcısı")

  Unutmayın _boyut sınıfları_ varsayılan olarak etkinleştirilir. Başvurmak [birleşik film şeritleri](~/ios/user-interface/storyboards/unified-storyboards.md) bunları hakkında daha fazla bilgi için kılavuz.

8. İçinde **araç paneli**, "Arama çubuğuna etiket" yazın ve sürükleyin bir **etiket** tasarım yüzeyine (Merkezi alanında):

  ![](hello-ios-quickstart-images/image8new.png "Bir etiket merkezi alanında tasarım yüzeyine sürükleyin")

  > [!NOTE]
  > Getir **özellikleri paneli** veya **araç** giderek dilediğiniz zaman **Görünüm > klavye takımı**.

9. Tanıtıcıları yakalayın *sürükleyerek denetimleri* (denetimi etrafında daireler) ve geniş etiket yapın:

  ![](hello-ios-quickstart-images/image9.png "Daha geniş etiketi yap")


10. İle **etiket** tasarım yüzeyine seçili kullanmak **özellikleri paneli** değiştirmek için **metin** özelliği **etiket** için "Enter bir Phoneword: "

  ![](hello-ios-quickstart-images/image10.png "Bir Phoneword girmek için etiket ayarlayın")

11. Arama sürükleyin ve araç kutusu içinde "metin alanı" için bir **metin alanı** gelen **araç** tasarım üzerine yüzey ve altında yerleştirin **etiket**. Kadar genişliğini ayarla **metin alanı** olarak aynı genişliği **etiket**:

  ![](hello-ios-quickstart-images/image12new.png "Etiket aynı genişlikte metin alanı olun")


12. İle **metin alanı** tasarım yüzeyine seçili, değiştirme **metin alanı**'s **adı** özelliği kimlik bölümünde **özellikleri paneli** için `PhoneNumberText`, değiştirip **metin** "1-855-XAMARIN" özelliği:

  ![](hello-ios-quickstart-images/image13new.png "1-855-XAMARIN için başlık özelliğini değiştirme")


13. Sürükleme bir **düğmesini** gelen **araç** tasarım üzerine yüzey ve altında yerleştirin **metin alanı**. Genişliği ayarlama böylece **düğmesini** olarak geniş **metin alanı** ve **etiket**:

  ![](hello-ios-quickstart-images/image14new.png "Düğme etiketi ve metin alanı geniş olacak şekilde genişliğini ayarla")


14. İle **düğmesini** tasarım yüzeyine seçili, değiştirme **adı** özelliğinde **kimlik** bölümünü **özellikleri paneli** için `TranslateButton`. Değişiklik **başlık** özelliği "Çevir" için:

  ![](hello-ios-quickstart-images/image15new.png "Çeviri için başlık özelliğini değiştirme")


15. Yukarıdaki iki adımı yineleyin ve sürükleyin bir **düğmesini** gelen **araç** tasarım üzerine yüzey ve ilk altında yerleştirin **düğmesini**. Genişliğini ayarla böylece **düğmesini** ilk olarak geniş **düğmesini**:

  ![](hello-ios-quickstart-images/image16new.png "Düğme ilk düğme olarak kadar geniş olacak şekilde genişliğini ayarla")


16. İkinci ile **düğmesini** tasarım yüzeyine seçili, değiştirme **adı** özelliğinde **kimlik** bölümünü **özellikleri paneli**için `CallButton`. Değişiklik **başlık** "Araması" özelliğine:

  ![](hello-ios-quickstart-images/image17new.png "Çağrı başlık özelliğini değiştirme")

  Giderek değişiklikleri kaydetmek **Dosya > Kaydet** basarak veya **⌘ + s**.


17. Bazı mantığı telefon numaralarını alfasayısal sayısal çevirmek için uygulamasına eklenmesi gerekir. Projeye sağ tıklayarak yeni bir dosya eklemek **Phoneword_iOS** proje **çözüm paneli** ve seçme **Ekle > yeni dosya...**  veya tuşuna basarak **⌘ + n**:

  ![](hello-ios-quickstart-images/image18.png "Yeni bir dosya projeye ekleyin")


18. İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıfı** ve yeni dosya adı `PhoneTranslator`:

  ![](hello-ios-quickstart-images/image19.png "Boş sınıfı seçin ve PhoneTranslator yeni dosya adı")


19. Bu yeni, boş C# sınıf bize oluşturur. Tüm şablon kodu kaldırın ve aşağıdaki kod ile değiştirin:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    Kaydet **PhoneTranslator.cs** dosya ve kapatın.

20. Kullanıcı arabirimini oluşturan wire için kodu ekleyin. Bu çift yapmak için **ViewController.cs** içinde **çözüm paneli** açmak için:

  ![](hello-ios-quickstart-images/image20new.png "Kullanıcı arabirimini oluşturan wire için kodu ekleyin")


21. Begin kabloyla yukarı `TranslateButton`. İçinde **ViewController** sınıfı, Bul `ViewDidLoad` yöntemi ve altına aşağıdaki kodu ekleyin `base.ViewDidLoad()` çağırın:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    Dahil `using Phoneword_iOS;` dosyanın ad farklı olması durumunda.

22. Adlı ikinci düğmesine basarak kullanıcıya yanıt olarak kod ekleme `CallButton`. Aşağıdaki kodu için kod altına yerleştirin `TranslateButton` ve ekleme `using Foundation;` dosyanın en üstüne:

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```


23. Değişiklikleri kaydetmek ve seçerek sonra uygulamayı derlediğinizde **Yapı > Yapı tüm** veya tuşuna basarak **⌘ + B**.  Uygulama derler, bir başarı iletisi IDE üstünde görünür:

  ![](hello-ios-quickstart-images/image21.png "IDE en üstte bir başarı iletisi görüntülenir")

  Hatalar varsa, önceki adımlarda size gidin ve uygulama başarıyla derlemeler kadar hataları düzeltin.

27. Son olarak, uygulamayı test **iOS simülatörü**. Üst IDE solda, seçin **hata ayıklama** ilk açılan listesinde, gelen ve **iPhone 8 iOS x.x artı** ikinci açılır ve tuşuna **Başlat** (üçgen, düğmesi Yürüt düğmesine benzer):

  ![](hello-ios-quickstart-images/image27new.png "Başlat'a basın")

  > [!NOTE]
  > Günümüzde, Apple, bir gereksinim nedeniyle olması gerekebilir bir geliştirme sertifikası veya *kimlik imzalama* oluşturmak için aygıt veya simulator için kod. Adımları [cihaz sağlamayı Kılavuzu](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) bunu ayarlamak için.

28. Bu uygulama iOS simülatörü'nü içinden başlatılır:

  ![](hello-ios-quickstart-images/image28.png "İOS simülatörü'nü içinde çalışan uygulama")

  Telefon aramaları iOS Simulator'da desteklenmez; Bunun yerine, çağrı çalışırken uyarı iletişim kutusu görürsünüz:

  ![](hello-ios-quickstart-images/image29.png "Çağrı çalışırken uyarı iletişim kutusu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-walkthrough"></a>Visual Studio'da izlenecek yollar

Bu kılavuz, bir alfasayısal telefon numarası bir sayısal telefon numarası çevirir Phoneword adlı bir uygulama oluşturmak açıklar.

**Not**: Bu kılavuzda, bir Windows 10 sanal makinede Visual Studio Enterprise 2017 kullanılır. Ayarlama yukarıdaki gereksinimleri karşılayan sürece buradan farklılık gösterir ancak bazı ekran görüntüleri, ayarlamak için farklı görünebilir unutmayın.

> [!NOTE]
> Bu kılavuza devam etmeden önce zaten Mac'iniz için Visual Studio'dan bağlı gerekir. Xamarin.iOS oluşturmak ve iOS Tasarımcısı ve uygulamaları başlatmak için Apple'nın tooling kullandığından budur. Ayarlamak için adımları [Mac çiftine](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Kılavuzu.

1. Visual Studio'dan başlatma **Başlat** menüsü:

  ![](hello-ios-quickstart-images/image001-.png "Başlangıç ekranı")

  Seçerek yeni bir Xamarin.iOS çözüm oluşturmak **Dosya > Yeni > Proje... > Visual C# > iPhone & iPad > iOS uygulaması (Xamari**:

  ![İOS uygulaması (Xamarin) proje türü seçin](hello-ios-quickstart-images/image002.w157.png "iOS uygulaması (Xamarin) proje türü seçin")

  Görüntülenen sonraki iletişim kutusunda, seçin **Single View uygulaması** şablonu ve tuşuna **Tamam** projesi oluşturmak için:

  ![Tek bir görünüm proje şablonu seçin](hello-ios-quickstart-images/image002-2.w157.png "tek bir görünüm seçin proje şablonu")

1. Araç çubuğu Xamarin Mac arası simgesinde yeşil olduğunu doğrulayın.

    ![Araç çubuğu Xamarin Mac arası simgesinde yeşil olduğundan emin olun](hello-ios-quickstart-images/vs-image4.png)

    Değilse, bu Mac yapı konak bağlantı olduğu anlamına gelir, adımları [Yapılandırma Kılavuzu](~/ios/get-started/installation/windows/connecting-to-mac/index.md) bağlı.

1. Açık **Main.storyboard** içinde çift tarafından iOS Tasarımcısı dosyasında **Çözüm Gezgini**:

  ![](hello-ios-quickstart-images/vs-image7.png "İOS Tasarımcısı")

1. Açık **araç** sekmesinde "Arama çubuğuna etiket" yazın ve sürükleyin bir **etiket** tasarım yüzeyine (Merkezi alanında):

  ![](hello-ios-quickstart-images/vs-image8.png "Bir etiket merkezi alanında tasarım yüzeyine sürükleyin")

1. Ardından, işler, yakalayın *sürükleyerek denetimleri* ve daha geniş etiket yapın:

  ![](hello-ios-quickstart-images/vs-image9.png "Daha geniş etiketi yap")

1. İle **etiket** tasarım yüzeyine seçili kullanmak **özellikleri Windows** değiştirmek için **metin** özelliği **etiket** için "Enter bir Phoneword: "

  ![](hello-ios-quickstart-images/vs-image10.png "'Bir Phoneword girmek için ' etiketinin metin özelliğini değiştirin")

  > [!NOTE]
  > Getir **özellikleri** veya **araç** giderek dilediğiniz zaman **Görünüm** menüsü.

1. Arama sürükleyin ve araç kutusu içinde "metin alanı" için bir **metin alanı** gelen **araç** tasarım üzerine yüzey ve altında yerleştirin **etiket**. Kadar genişliğini ayarla **metin alanı** olarak aynı genişliği **etiket**:

  ![](hello-ios-quickstart-images/vs-image12.png "Etiket aynı genişlikte metin alanı olana kadar genişliğini ayarla")

10. İle **metin alanı** tasarım yüzeyine seçili, değiştirme **metin alanı**'s **adı** özelliği kimlik bölümünde **özellikleri**için `PhoneNumberText`, değiştirip **metin** "1-855-XAMARIN" özelliği:

  ![](hello-ios-quickstart-images/vs-image13.png "1-855-XAMARIN için metin özelliğini değiştirin")


11. Sürükleme bir **düğmesini** gelen **araç** tasarım üzerine yüzey ve altında yerleştirin **metin alanı**. Genişliği ayarlama böylece **düğmesini** olarak geniş **metin alanı** ve **etiket**:

  ![](hello-ios-quickstart-images/vs-image14.png "Düğme etiketi ve metin alanı geniş olacak şekilde genişliğini ayarla")


12. İle **düğmesini** tasarım yüzeyine seçili, değiştirme **adı** özelliğinde **kimlik** bölümünü **özellikleri** için`TranslateButton`. Değişiklik **başlık** özelliği "Çevir" için:

  ![](hello-ios-quickstart-images/vs-image15.png "Çeviri için başlık özelliğini değiştirme")


13. Önceki iki adımı yineleyin ve sürükleyin bir **düğmesini** gelen **araç** tasarım üzerine yüzey ve ilk altında yerleştirin **düğmesini**. Genişliğini ayarla böylece **düğmesini** ilk olarak geniş **düğmesini**:

  ![](hello-ios-quickstart-images/vs-image16.png "Düğme ilk düğme olarak kadar geniş olacak şekilde genişliğini ayarla")


14. İkinci ile **düğmesini** tasarım yüzeyine seçili, değiştirme **adı** özelliğinde **kimlik** bölümünü **özellikleri** için `CallButton`. Değişiklik **başlık** "Araması" özelliğine:

  ![](hello-ios-quickstart-images/vs-image17.png "Çağrı başlık özelliğini değiştirme")

  Değişiklikleri kaydetmek için giderek **Dosya > Tümünü Kaydet** basarak veya **Ctrl + s**.

15. Telefon numaralarını alfasayısal sayısal çevirmek için bazı kodlar ekleyin. Bunu yapmak için önce yeni bir dosya projeye üzerinde sağ tıklayarak ekleyin **Phoneword** proje **Çözüm Gezgini** ve seçme **Ekle > Yeni öğe...**  veya tuşuna basarak **Ctrl + Shift + A**:

  ![](hello-ios-quickstart-images/vs-image18.png "Telefon numaralarını alfasayısal sayısal çevirmek için bazı kod ekleme")


16. İçinde **Yeni Öğe Ekle** iletişim (projeye sağ tıklayın ve ardından Ekle'yi seçin > Yeni öğe...), select **Apple > sınıfı** ve yeni dosya adı `PhoneTranslator`:

  ![](hello-ios-quickstart-images/vs-image19.w157.png "PhoneTranslator adlı yeni bir sınıf ekleyin")

  > [!IMPORTANT]
  > C# simgesine sahip 'sınıfı' şablon seçtiğinizden emin olun. Aksi takdirde bu yeni sınıfa referans mümkün olmayabilir.


17. Bu, yeni bir C# sınıfı oluşturur. Tüm şablon kodu kaldırın ve aşağıdaki kod ile değiştirin:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

  Kaydet **PhoneTranslator.cs** dosya ve kapatın.

18. Çift **ViewController.cs** içinde **Çözüm Gezgini** böylece mantığı tanıtıcıları etkileşimleri düğmeleri ile eklenen, açmak için:

  ![](hello-ios-quickstart-images/vs-image20.png "Düğmeleri ile etkileşim işlemek için eklenen mantığı")


19. Begin kabloyla yukarı `TranslateButton`. İçinde **ViewController** sınıfı, Bul `ViewDidLoad` yöntemi. İçinde aşağıdaki düğmeyi kod ekleme `ViewDidLoad`, beneath `base.ViewDidLoad()` çağırın:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```
    Dahil `using Phoneword;` dosyanın ad farklı olması durumunda.

20. Adlı ikinci düğmesine basarak kullanıcıya yanıt olarak kod ekleme `CallButton`. Aşağıdaki kodu için kod altına yerleştirin `TranslateButton` ve ekleme `using Foundation;` dosyanın en üstüne:

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```


21. Değişiklikleri kaydetmek ve seçerek sonra uygulamayı derlediğinizde **Yapı > Yapı çözümü** veya tuşuna basarak **Ctrl + Shift + B**.  Uygulama derler, bir başarı iletisi IDE alt kısmında görünür:

  ![](hello-ios-quickstart-images/vs-image21.png "IDE altındaki bir başarı iletisi görüntülenir")

  Hatalar varsa, önceki adımlarda size gidin ve uygulama başarıyla derlemeler kadar hataları düzeltin.

22. Son olarak, uygulamayı test **düğümlerde iOS simülatörü**. IDE araç çubuğunda seçin **hata ayıklama** ve **iPhone 8 iOS x.x artı** menüleri ve tuşuna aşağı açılan **Başlat** (Oynat düğmesini benzer yeşil üçgenle):

  ![](hello-ios-quickstart-images/vs-image27.png "Başlat'a basın")

23. Bu uygulama iOS simülatörü'nü içinden başlatılır:

  ![](hello-ios-quickstart-images/vs-image28.png "İOS simülatörü'nü içinde çalışan uygulama")

  Telefon aramaları iOS Simulator'da desteklenmez; Bunun yerine, çağrı çalışırken uyarı iletişim kutusu görüntülenir:

  ![](hello-ios-quickstart-images/vs-image29.png "Çağrı çalışırken uyarı iletişim kutusu görüntüler")

-----

İlk Xamarin.iOS uygulamanızı Tamamlanıyor Tebrikler!

Araçlar dissect artık ve bu kılavuzda gösterilen becerileri [Hello, iOS derinlemesine](~/ios/get-started/hello-ios/hello-ios-deepdive.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin uygulama simgeleri ve başlatma görüntüleri (örnek)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Merhaba, iOS (örnek)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS İnsan Arabirimi yönergelerine](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS sağlama portalı](https://developer.apple.com/ios/manage/overview/index.action)
