---
title: 'Hello, Android: Hızlı Başlangıç'
description: Bu iki bölümden kılavuzda (Mac için Visual Studio veya Visual Studio kullanarak), ilk Xamarin.Android uygulaması oluşturma ve Xamarin ile Android uygulama geliştirme temelleri bir anlayış geliştirmek. Bu doğrultuda, Araçlar, kavramlar ve bir Xamarin.Android uygulaması derleme ve dağıtma için gerekli adımları sunulacaktır.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 44007FA1-3ABC-4935-BF52-4613AF0553A6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/20/2018
ms.openlocfilehash: beb90587e0d720de7770056c8b51264099edecdc
ms.sourcegitcommit: fb55eba393e43bcc9e9d1fef9ef1f1310e99f620
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/21/2018
ms.locfileid: "39189027"
---
# <a name="hello-android-quickstart"></a>Hello, Android: Hızlı Başlangıç

_Bu iki bölümden kılavuzda (Mac için Visual Studio veya Visual Studio kullanarak), ilk Xamarin.Android uygulaması oluşturma ve Xamarin ile Android uygulama geliştirme temelleri bir anlayış geliştirmek. Bu doğrultuda, Araçlar, kavramlar ve bir Xamarin.Android uygulaması derleme ve dağıtma için gerekli adımları sunulacaktır._

## <a name="hello-android-quickstart"></a>Hello, Android hızlı başlangıç

Bu kılavuzda, bir sayısal telefon numarası ile bir alfasayısal telefon numarası (kullanıcı tarafından girilen) çeviren bir uygulama oluşturup sayısal telefon numarasını kullanıcıya göstermek. Son uygulama şöyle görünür:

[![İşlem tamamlandığında uygulamasının ekran görüntüsü](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)


## <a name="requirements"></a>Gereksinimler

Bu adım adım kılavuzla birlikte takip etmek için aşağıdakiler gerekir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 veya üzeri.

-   Visual Studio 2015 Professional veya üzeri.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   Mac için Visual Studio'nun en son sürümü

-   OS X Yosemite veya üzeri.

-----

Bu izlenecek yol, Xamarin.Android en son sürümü yüklü ve tercih ettiğiniz platformda çalışan olduğunu varsayar. Xamarin.Android Yükleme Kılavuzu için başvurmak [Xamarin.Android yükleme](~/android/get-started/installation/index.md) Kılavuzlar.
Başlamadan önce lütfen indirip sıkıştırmasını [Xamarin uygulama simgeleri ve başlatma ekranları](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) ayarlayın.

## <a name="configuring-emulators"></a>Öykünücüler yapılandırma

Android öykünücüsü'nü kullanıyorsanız öykünücünün donanım hızlandırmasını kullanmak için yapılandırmanızı öneririz. Donanım hızlandırma yapılandırmaya yönelik yönergeler kullanılabilir [Emulator performansını donanım hızlandırmasını](~/android/get-started/installation/android-emulator/hardware-acceleration.md).


## <a name="walkthrough"></a>İzlenecek yol

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'yu başlatın.  Tıklayın **Dosya > Yeni > Proje** yeni bir proje oluşturmaktır.

İçinde **yeni proje** iletişim kutusunda, tıklayın **Android uygulaması** şablonu.
Yeni proje adını `Phoneword`. Tıklayın **Tamam**:

[![Yeni Proje Phoneword olduğu](hello-android-quickstart-images/vs/01-new-project-name-w157-sml.png)](hello-android-quickstart-images/vs/01-new-project-name-w157.png#lightbox)

İçinde **yeni Android uygulaması** iletişim kutusunda, tıklayın **boş uygulama** tıklatıp **Tamam** yeni projeyi oluşturmak için:

[![Boş uygulama şablonunu seçin](hello-android-quickstart-images/vs/02-blank-app-w157-sml.png)](hello-android-quickstart-images/vs/02-blank-app-w157.png#lightbox)

### <a name="creating-the-layout"></a>Bir düzen oluşturma

Yeni projeyi oluşturduktan sonra genişletin **kaynakları** klasörünü ve ardından **Düzen** klasöründe **Çözüm Gezgini**.
Çift **activity_main.axml** Android Tasarımcısı'nda açın. Bu uygulamanın ekran için yerleşim dosyası.

[![Etkinlik main.axml açın](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png#lightbox)

Gelen **araç kutusu** (soldaki alan), girin `text` sürükleyin ve arama alanına içine bir **metin (büyük)** tasarım yüzeyine (ortasındaki alan) pencere öğesi:

[![Büyük metin pencere öğesi ekleme](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png#lightbox)

İle **metin (büyük)** denetimi tasarım yüzeyinde, kullanım seçili **özellikleri** değiştirileceğini bölmesine `text` özelliği **metin (büyük)** içinpencereöğesi`Enter a Phoneword:` burada gösterildiği gibi:

[![Büyük metin özelliklerini ayarlama](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png#lightbox)

Sürükleme bir **düz metin** pencere öğesinden **araç kutusu** tasarım yüzeyine ve altına yerleştirin **metin (büyük)** pencere öğesi:

[![Düz metin pencere öğesi ekleme](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png#lightbox)

İle **düz metin** kullanım tasarım yüzeyinde seçili bir pencere öğesi **özellikleri** değiştirileceğini bölmesine `id` özelliği **düz metin** pencere öğesine `@+id/PhoneNumberText`değiştirip `text` özelliğini `1-855-XAMARIN`:

[![Düz metin özelliklerini ayarlama](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png#lightbox)

Sürükleme bir **düğmesi** gelen **araç kutusu** tasarım yüzeyine ve altına yerleştirin **düz metin** pencere öğesi:

[![Sürükleme düğme tasarım Çevir](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png#lightbox)

İle **düğmesi** tasarım yüzeyinde seçili kullanın **özellikleri** değiştirileceğini bölmesine `id` özelliği **düğmesi** için `@+id/TranslateButton` ve değiştirme `text` özelliğini `Translate`:

[![Ayarla düğmesi özellikleri Çevir](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png#lightbox)

Sürükleme bir **TextView** gelen **araç kutusu** tasarım yüzeyine ve altındaki yerleştirmek **düğmesi** pencere öğesi. Ayarlama `id` özelliği **TextView** için `@+id/TranslatedPhoneWord` değiştirip `text` boş bir dize:

[![Metin görünümü özelliklerini ayarlayın.](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png#lightbox)    

Tuşlarına basarak çalışmanızı kaydedin **CTRL + S**.

### <a name="writing-translation-code"></a>Çeviri kod yazma

Sonraki adım, telefon numaralarını alfasayısal sayısal çevirmek için bazı kod eklemektir. Yeni bir dosya projeye sağ tıklayarak ekleyin **Phoneword** projesi **Çözüm Gezgini** bölmesi ve seçme **Ekle > Yeni öğe...**  aşağıda gösterildiği gibi:

[![Yeni Öğe Ekle](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png#lightbox)

İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod > kod dosyası** ve yeni bir kod dosyası adı **PhoneTranslator.cs**:

[![PhoneTranslator.cs ekleyin](hello-android-quickstart-images/vs/14-add-class-sml-w157.png)](hello-android-quickstart-images/vs/14-add-class-w157.png#lightbox)

Bu, yeni bir boş C# sınıf oluşturur. Bu dosyaya aşağıdaki kodu ekleyin:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
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
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Değişiklikleri kaydetmek **PhoneTranslator.cs** tıklayarak dosya **Dosya > Kaydet** (veya basarak **CTRL + S**), dosyayı kaydedip kapatın.

### <a name="wiring-up-the-interface"></a>Arabirimi oluşturan bağlama

Sonraki adım kullanıcı arabirimi oluşturan yedekleme kod içine ekleyerek kablo kod eklemektir `MainActivity` sınıfı. Kabloyla yukarı başlangıç **çevir** düğmesi. İçinde `MainActivity` sınıfı, Bul `OnCreate` yöntemi. Sonraki adım içinde düğmesine kod eklemektir `OnCreate`aşağıdaki `base.OnCreate(bundle)` ve `SetContentView
(Resource.Layout.Main)` çağırır. İlk olarak, şablonu kodu değiştirin böylece `OnCreate` yöntemi şuna benzer:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // New code will go here
        }
    }
}
```

Android Designer aracılığıyla Düzen dosyası içinde oluşturulmuş denetimler bir başvuru alın. Aşağıdaki kodu ekleyin `OnCreate` yöntem çağrısından sonra `SetContentView`:

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

İçin kullanıcı basarsa yanıt kodu ekleyin **çevir** düğmesi.
Aşağıdaki kodu ekleyin `OnCreate` yöntemi (sonra önceki adımda eklenen satırlar):

```csharp
// Add code to translate number
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    string translatedNumber = Core.PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Seçerek çalışmanızı kaydedin **Dosya > Tümünü Kaydet** (veya basarak **CTRL-SHIFT-S**) ve uygulama seçerek yapı **Yapı > çözümü yeniden derle** (veya tuşlarına basarak **CTRL-SHIFT-B**). 

Hatalar varsa, önceki adımlarında gidin ve uygulama başarıyla oluşturulursa kadar hataları düzeltin. Gibi bir yapı hatası alırsanız _kaynak geçerli bağlamda mevcut değil_, doğrulayın, ad alanı adı **MainActivity.cs** proje adıyla eşleşen (`Phoneword`) ve ardından tamamen çözümü yeniden oluşturun. Derleme hataları almaya devam ediyorsanız, en son Xamarin.Android güncelleştirmelerin yüklü olduğunu doğrulayın.

### <a name="setting-the-label-and-app-icon"></a>Uygulama simgesi ve etiket ayarlama

Şu anda çalışan bir uygulama olmalıdır &ndash; Son dokunuşları ekleme zamanı geldi! İçinde **MainActivity.cs**, Düzen `Label` için `MainActivity`. `Label` Olan Android kullanıcılarınıza uygulama olduğu için ekranın üst kısmındaki görüntüler.
Üst kısmındaki `MainActivity` sınıfı, değişiklik `Label` için `Phone Word` burada gösterildiği gibi:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Artık uygulamayı ayarlamak için zamanı geldi. Varsayılan olarak, Visual Studio projesi için varsayılan bir simge sağlar. Şimdi bu dosyaları çözümden silin ve bunları farklı bir simge ile değiştirin. Genişletin **kaynakları** klasöründe **çözüm bölmesi**. Ön eki beş klasörlere olduğunu fark **mipmap -**, ve her biri bu klasörleri içeren tek bir **Icon.png** dosyası:

[![Mipmap klasörlerinde ve Icon.png dosyaları](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png#lightbox)

Bu simge dosyaların her biri projeden silmek gereklidir. Sağ her birini tıklatın **Icon.png** dosyaları ' nı seçip **Sil** bağlam menüsünden:
   
[![Varsayılan Icon.png Sil](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png#lightbox)
   
Tıklayarak **Sil** iletişim düğmesi.

Ardından, indirip sıkıştırmasını [Xamarin uygulama simgeleri ayarlamak](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Uygulama simgeleri Bu zip dosyası tutar. Her simge görsel olarak aynıdır, ancak farklı çözünürlükte doğru şekilde farklı ekran densities ile farklı cihazlarda işlediğinden.  Xamarin.Android projeye dosya kümesini kopyalanmalıdır. Visual Studio içinde **Çözüm Gezgini**, sağ tıklayın **mipmap hdpı** klasörü ve select **Ekle > var olan öğeleri**:

[![Dosyaları Ekle](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png#lightbox)

Seçimi iletişim kutusundan sıkıştırması Xamarin AdApp simgeler dizine gidin ve açmak **mipmap hdpı** klasör. Seçin **Icon.png** tıklatıp **Ekle**.

Her biri için bu adımları yineleyin **mipmap -** klasörlerin içeriğini kadar **mipmap -** Xamarin uygulama simgeleri klasörleri, karşılık gelen kopyalanır **mipmap -** klasörleri **Phoneword** proje.

Tüm simgeleri için Xamarin.Android projesi kopyalandıktan sonra açın **proje seçenekleri** bölümünde projeye sağ tıklayarak iletişim **çözüm bölmesi**. Seçin **Yapı > Android uygulaması** seçip **@mipmap/icon** gelen **uygulama simgesi** birleşik giriş kutusu:

[![Proje simgesi ayarlama](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Uygulamayı çalıştırma

Son olarak, bir Android cihaz veya öykünücü üzerinde çalışan ve bir Phoneword çevirme uygulamayı test edin:

[![İşlem tamamlandığında uygulamasının ekran görüntüsü](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'yu başlatın **uygulamaları** klasör veya **Spotlight**. 

Tıklayın **yeni çözüm...**  yeni bir proje oluşturmaktır.

İçinde **yeni projeniz için bir şablon seçin** iletişim kutusunda, tıklayın **Android > Uygulama** seçip **Android uygulaması** şablonu. **İleri**'ye tıklayın.

[![Android uygulama şablonunu seçin](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png#lightbox)

İçinde **Android uygulamanızı yapılandırın** iletişim kutusunda, yeni uygulama adı `Phoneword` tıklatıp **sonraki**.

[![Android uygulaması yapılandırma](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png#lightbox)

İçinde **yeni Android uygulamanızı yapılandırın** iletişim kutusunda, çözüm ve proje adları kümesine bırakın `Phoneword` tıklatıp **Oluştur** projeyi oluşturmak için.

### <a name="creating-the-layout"></a>Bir düzen oluşturma

Yeni projeyi oluşturduktan sonra genişletin **kaynakları** klasörünü ve ardından **Düzen** klasöründe **çözüm** paneli.
Çift **Main.axml** Android Tasarımcısı'nda açın. Android Tasarımcısı'nda görüntülendiğinde bu ekran için yerleşim dosyası.

[![Main.AXML açın](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png#lightbox)

Seçin **tıklayın, Merhaba Dünya!** **Düğme** tuşuna basın ve tasarım yüzeyine **Sil** kaldırmak için anahtar. 

Gelen **araç kutusu** (sağ alanı) girin `text` sürükleyin ve arama alanına içine bir **metin (büyük)** tasarım yüzeyine (ortasındaki alan) pencere öğesi:

[![Büyük metin pencere öğesi ekleme](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png#lightbox)

İle **metin (büyük)** kullanabileceğiniz tasarım yüzeyinde seçili bir pencere öğesi, **özellikleri** değiştirmek için paneli `Text` özelliği **metin (büyük)** için pencere öğesi `Enter a Phoneword:` aşağıda gösterildiği gibi:

[![Büyük metin pencere öğesi özelliklerini ayarlama](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png#lightbox)

Sonraki adımda bir **düz metin** pencere öğesinden **araç kutusu** tasarım yüzeyine ve altına yerleştirin **metin (büyük)** pencere öğesi. Pencere öğeleri ada göre bulmak için arama alanını kullanabilirsiniz dikkat edin:

[![Düz metin pencere öğesi ekleme](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png#lightbox)

İle **düz metin** kullanabileceğiniz tasarım yüzeyinde seçili bir pencere öğesi, **özellikleri** değiştirmek için paneli `Id` özelliği **düz metin** içinpencereöğesi`@+id/PhoneNumberText` değiştirip `Text` özelliğini `1-855-XAMARIN`:

[![Düz metin pencere öğesi özelliklerini ayarlama](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png#lightbox)

Sürükleme bir **düğmesi** gelen **araç kutusu** tasarım yüzeyine ve altına yerleştirin **düz metin** pencere öğesi:

[![Bir düğme ekleyin](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png#lightbox)

İle **düğmesi** kullanabileceğiniz tasarım yüzeyinde seçili **özellikleri** değiştirmek için paneli `Id` özelliği **düğmesi** için `@+id/TranslateButton` ve değiştirme `Text` özelliğini `Translate`:

[![Çeviri düğme olarak yapılandırma](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png#lightbox)

Sürükleme bir **TextView** gelen **araç kutusu** tasarım yüzeyine ve altındaki yerleştirmek **düğmesi** pencere öğesi. İle **TextView** seçili Ayarla `id` özelliği **TextView** için `@+id/TranslatedPhoneWord` değiştirip `text` boş bir dize:

[![Metin görünümü özelliklerini ayarlayın.](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png#lightbox)    

Tuşlarına basarak çalışmanızı kaydedin  **&#8984; + S**.

### <a name="writing-translation-code"></a>Çeviri kod yazma

Şimdi, telefon numaralarını alfasayısal sayısal çevirmek için kod ekleyin. Yanında dişli simgesine tıklayarak yeni bir dosya projeye eklemek **Phoneword** projesi **çözüm** paneli ve seçerek **Ekle > yeni dosya...** :

[![Projeye yeni bir dosya ekleyin](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png#lightbox)

İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıf**, yeni dosya adı **PhoneTranslator**, tıklatıp **yeni**. Bu yeni bir boş C# sınıf bizim için oluşturur.

Tüm şablon kodunu Yeni sınıfta kaldırın ve aşağıdaki kodla değiştirin:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
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
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Değişiklikleri kaydetmek **PhoneTranslator.cs** seçerek dosya **Dosya > Kaydet** (veya basarak  **&#8984; + S**), dosyayı kaydedip kapatın. Herhangi bir derleme zamanı hata çözümün yeniden derlenmesi tarafından emin olun.

### <a name="wiring-up-the-interface"></a>Arabirimi oluşturan bağlama

Sonraki adım, kullanıcı arabirimi oluşturan yedekleme kodun içine ekleyerek bağlantıları tamamlayalım kod eklemektir `MainActivity` sınıfı.
Çift **MainActivity.cs** içinde **çözüm bölmesi** açın.

Başlamak için bir olay işleyicisi ekleyerek **çevir** düğmesi. İçinde `MainActivity` sınıfı, Bul `OnCreate` yöntemi. İçinde düğmesine kod ekleme `OnCreate`aşağıdaki `base.OnCreate(bundle)` ve `SetContentView (Resource.Layout.Main)` çağırır. İşleme kodunu varolan bir düğmeyi kaldırma (yani, başvuran kod `Resource.Id.myButton` ve bunun için bir tıklama işleyicisi oluşturur) böylece `OnCreate` yöntemi aşağıdakine benzer:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Our code will go here
        }
    }
}
```

Ardından, bir başvuru, Düzen dosyası Android Designer ile oluşturulmuş denetimler için gereklidir. Aşağıdaki kodu ekleyin `OnCreate` yöntemi (çağrısından sonra `SetContentView`):

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

İçin kullanıcı basarsa yanıt kodu ekleyin **çevir** düğme için aşağıdaki kodu ekleyerek `OnCreate` yöntemi (sonra son adımda eklenen satırlar):

```csharp
// Add code to translate number
string translatedNumber = string.Empty;

translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Çalışmanızı kaydedin ve uygulamayı seçerek yapı **Yapı > derleme tüm** (veya basarak  **&#8984; + B**). Uygulamayı derler, Mac için Visual Studio üst kısmındaki bir başarı iletisi alırsınız:

Hatalar varsa, önceki adımlarında gidin ve uygulama başarıyla oluşturulursa kadar hataları düzeltin. Gibi bir yapı hatası alırsanız _kaynak geçerli bağlamda mevcut değil_, doğrulayın, ad alanı adı **MainActivity.cs** proje adıyla eşleşen (`Phoneword`) ve ardından tamamen çözümü yeniden oluşturun. Derleme hataları almaya devam ediyorsanız, en son Xamarin.Android yüklü olduğunu ve Mac için Visual Studio güncelleştirmeleri doğrulayın.

### <a name="setting-the-label-and-app-icon"></a>Uygulama simgesi ve etiket ayarlama

Çalışan bir uygulamanız olduğuna göre Son dokunuşları ekleme zamanı geldi! Başlangıç düzenleyerek `Label` için `MainActivity`.
`Label` Olan Android kullanıcılarınıza uygulama olduğu için ekranın üst kısmındaki görüntüler. Üst kısmındaki `MainActivity` sınıfı, değişiklik `Label` için `Phone Word` burada gösterildiği gibi:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Artık uygulamayı ayarlamak için zamanı geldi. Varsayılan olarak, Mac için Visual Studio projesi için varsayılan bir simge sağlar. Şimdi bu dosyaları çözümden silin ve bunları farklı bir simge ile değiştirin. Genişletin **kaynakları** klasöründe **çözüm bölmesi**. Ön eki beş klasörlere olduğunu fark **mipmap -**, ve her biri bu klasörleri içeren tek bir **Icon.png** dosyası:

[![Mipmap klasörlerinde ve Icon.png dosyaları](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png#lightbox)

Bu simge dosyaların her biri projeden silmek gereklidir. Sağ her birini tıklatın **Icon.png** dosyaları ' nı seçip **Kaldır** bağlam menüsünden:

[![Varsayılan Icon.png Sil](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png#lightbox)

Tıklayarak **Sil** iletişim düğmesi.

Ardından, indirip sıkıştırmasını [Xamarin uygulama simgeleri ayarlamak](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Uygulama simgeleri Bu zip dosyası tutar. Her simge görsel olarak aynıdır, ancak farklı çözünürlükte doğru şekilde farklı ekran densities ile farklı cihazlarda işlediğinden.  Xamarin.Android projeye dosya kümesini kopyalanmalıdır. Mac için Visual Studio içinde **çözüm bölmesi**, sağ tıklayın **mipmap hdpı** klasörü ve select **Ekle > Add Files**:

[![Dosyaları Ekle](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png#lightbox)

Seçimi iletişim kutusundan sıkıştırması Xamarin AdApp simgeler dizine gidin ve açmak **mipmap hdpı** klasör. Seçin **Icon.png** tıklatıp **açık**.

İçinde **klasöre dosya Ekle** iletişim kutusunda **dosyasını dizinine kopyalayın** tıklatıp **Tamam**:

[![Dizin iletişim kutusuna dosya kopyalama](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png#lightbox)

Her biri için bu adımları yineleyin **mipmap -** klasörlerin içeriğini kadar **mipmap -** Xamarin uygulama simgeleri klasörleri, karşılık gelen kopyalanır **mipmap -** klasörleri **Phoneword** proje.

Tüm simgeleri için Xamarin.Android projesi kopyalandıktan sonra açın **proje seçenekleri** bölümünde projeye sağ tıklayarak iletişim **çözüm bölmesi**. Seçin **Yapı > Android uygulaması** seçip **@mipmap/icon** gelen **uygulama simgesi** birleşik giriş kutusu:

[![Proje simgesi ayarlama](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Uygulamayı çalıştırma

Son olarak, bir Android cihaz veya öykünücü üzerinde çalışan ve bir Phoneword çevirme uygulamayı test edin:

[![İşlem tamamlandığında uygulamasının ekran görüntüsü](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

-----

İlk Xamarin.Android uygulamanız tamamlamada Tebrikler!
Artık yalnızca öğrendiğiniz becerileri ve araçları ayrıntılı olarak inceleyelim zamanı geldi. Sonraki çalışır durumda [Hello, Android yakından](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin Android uygulama simgeleri (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (örnek)](https://developer.xamarin.com/samples/monodroid/Phoneword)
