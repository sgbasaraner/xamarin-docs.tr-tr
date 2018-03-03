---
title: "Merhaba, Android: Hızlı Başlangıç"
description: "Bu iki parçalı Kılavuzu'nda (Visual Studio veya Visual Studio için Mac kullanarak), ilk Xamarin.Android uygulaması oluşturma ve Xamarin ile Android uygulaması geliştirme ile ilgili temel bilgileri bir anlayış geliştirmek. Yol boyunca araçları, kavramlar ve oluşturmak ve bir Xamarin.Android uygulaması dağıtmak için gerekli adımları görülecektir."
ms.topic: article
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 2ec01091314df64070cafb570f01e54634759c77
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="hello-android-quickstart"></a>Merhaba, Android: Hızlı Başlangıç

_Bu iki parçalı Kılavuzu'nda (Visual Studio veya Visual Studio için Mac kullanarak), ilk Xamarin.Android uygulaması oluşturma ve Xamarin ile Android uygulaması geliştirme ile ilgili temel bilgileri bir anlayış geliştirmek. Yol boyunca araçları, kavramlar ve oluşturmak ve bir Xamarin.Android uygulaması dağıtmak için gerekli adımları görülecektir._

## <a name="hello-android-quickstart"></a>Merhaba, Android hızlı başlangıç

Bu kılavuzda, bir sayısal telefon numarası (kullanıcı tarafından girilen) bir alfasayısal telefon numarasını çeviren bir uygulama oluşturmak ve kullanıcıya sayısal telefon numarasını görüntüler. Son uygulama şöyle görünür:

[![Bu tamamlandığında uygulamasının ekran görüntüsü](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png)


## <a name="requirements"></a>Gereksinimler

Bu kılavuzda birlikte takip etmek için aşağıdakiler gerekir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 veya üzeri.

-   Visual Studio 2015 Professional ya da daha sonra.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   Mac için Visual Studio en son sürümü

-   OS X Yosemite veya daha sonra.

-----

Bu kılavuzda Xamarin.Android en son sürümü yüklü ve çalışır platformunuz tercih olduğunu varsayar. Xamarin.Android Yükleme Kılavuzu için başvurmak [Xamarin.Android yükleme](~/android/get-started/installation/index.md) kılavuzları.
Başlamadan önce lütfen indirip sıkıştırmasını [Xamarin uygulama simgeleri & başlatma ekranlar](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) ayarlayın.

## <a name="configuring-emulators"></a>Öykünücüler yapılandırma

Google Android SDK öykünücüsü kullanıyorsanız, donanım hızlandırmasını kullanılacak öykünücü yapılandırmanızı öneririz. Donanım hızlandırma yapılandırmaya ilişkin yönergeler bulunan [donanım hızlandırmasını](~/android/get-started/installation/android-emulator/hardware-acceleration.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio Android öykünücüsü kullanıyorsanız, Hyper-V bilgisayarınızda etkinleştirilmesi gerekir. Visual Studio Android öykünücüsü yapılandırma hakkında daha fazla bilgi için bkz: [Android için Visual Studio öykünücüsü sistem gereksinimleri](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-----

## <a name="walkthrough"></a>İzlenecek yol

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'yu başlatın.  Tıklatın **Dosya > Yeni > Proje** yeni bir proje oluşturmak için.

İçinde **yeni proje** iletişim kutusunda, tıklatın **boş uygulama (Android)** şablonu.
Yeni proje adı `Phoneword`. Tıklatın **Tamam** yeni proje oluşturmak için:

[![Yeni Proje Phoneword olduğu](hello-android-quickstart-images/vs/02-new-project-name-sml.png)](hello-android-quickstart-images/vs/02-new-project-name.png)

### <a name="creating-the-layout"></a>Düzen oluşturma

Yeni Proje oluşturulduktan sonra genişletin **kaynakları** klasörünü ve ardından **düzeni** klasöründe **Çözüm Gezgini**.
Çift **Main.axml** Android tasarımcısında açın. Bu uygulamanın ekranı düzeni dosyasıdır:

[![Main.AXML açın](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png)

Gelen **araç** (sol alanı) girin `text` sürükleyin ve arama alanı içine bir **metin (büyük)** pencere öğesi (alan Merkezi'ndeki) tasarım yüzeyine:

[![Büyük metin pencere öğesi ekleme](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png)

İle **metin (büyük)** denetimi tasarım yüzeyine, kullanım seçili **özellikleri** değiştirmek için bölmesinde `text` özelliği **metin (büyük)** içinpencereöğesi`Enter a Phoneword:` aşağıda gösterildiği gibi:

[![Büyük metin özelliklerini ayarlama](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png)

Sürükleme bir **düz metin** pencere öğesini **araç** tasarım yüzey ve altına yerleştirin **metin (büyük)** pencere öğesi:

[![Düz metin pencere öğesi ekleme](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png)

İle **düz metin** tasarım yüzeyine, kullanım seçili pencere öğesi **özellikleri** değiştirmek için bölmesinde `id` özelliği **düz metin** Pencere`@+id/PhoneNumberText`değiştirip `text` özelliğine `1-855-XAMARIN`:

[![Düz metin özelliklerini ayarlama](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png)

Sürükleme bir **düğmesini** gelen **araç** tasarım yüzey ve altına yerleştirin **düz metin** pencere öğesi:

[![Sürükleme Çevir tasarım düğmesi](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png)

İle **düğmesini** tasarım yüzeyine seçili, kullanın **özellikleri** değiştirmek için bölmesinde `id` özelliği **düğmesini** için `@+id/TranslateButton` ve değiştirin `text` özelliğine `Translate`:

[![Set çevir düğmesi özellikleri](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png)

Sürükleme bir **kutusu TextView** gelen **araç** tasarım yüzey ve altında yerleştirin **düğmesini** pencere öğesi. Ayarlama `id` özelliği **kutusu TextView** için `@+id/TranslatedPhoneWord` değiştirip `text` boş bir dize için:

[![Metin görünümü özellikleri ayarlayın.](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png)    

Tuşlarına basarak çalışmanızı kaydedin **CTRL + S**.

### <a name="writing-translation-code"></a>Çeviri kod yazma

Sonraki adım, telefon numaralarını alfasayısal sayısal çevirmek için bazı kod eklemektir. Projeye sağ tıklayarak yeni bir dosya eklemek **Phoneword** proje **Çözüm Gezgini** bölmesinde ve seçme **Ekle > Yeni öğe...**  aşağıda gösterildiği gibi:

[![Yeni Öğe Ekle](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png)

İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod** ve yeni kod dosyası adı **PhoneTranslator.cs**:

[![Add PhoneTranslator.cs](hello-android-quickstart-images/vs/14-add-class-sml.png)](hello-android-quickstart-images/vs/14-add-class.png)

Bu, yeni bir boş C# sınıfı oluşturur. Aşağıdaki kod bu dosyaya ekleyin:

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

Değişiklikleri kaydetmek **PhoneTranslator.cs** tıklatarak dosya **Dosya > Kaydet** (veya tuşlarına basarak **CTRL + S**), dosyayı kaydedip kapatın.

### <a name="wiring-up-the-interface"></a>Kablolama arabirimi ayarlama

Kullanıcı arabirimini oluşturan yedekleme koda ekleyerek wire üzere kod eklemek için sonraki adımdır `MainActivity` sınıfı. Begin kabloyla yukarı **çevir** düğmesi. İçinde `MainActivity` sınıfı, Bul `OnCreate` yöntemi. Sonraki adım düğme kodu içinde eklemektir `OnCreate`, aşağıdaki `base.OnCreate(bundle)` ve `SetContentView
(Resource.Layout.Main)` çağrıları. İlk olarak, şablonu kodu değiştirmek için `OnCreate` yöntemi aşağıdakine benzer:

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

Android Tasarımcısı aracılığıyla düzeni dosyasında oluşturulan denetimleri başvuru alın. Aşağıdaki kodu ekleyin `OnCreate` çağrısından sonra yöntemi `SetContentView`:

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

Seçerek çalışmanızı kaydedin **Dosya > Tümünü Kaydet** (veya tuşlarına basarak **CTRL-SHIFT-S**) ve uygulama seçerek yapı **Yapı > çözümü yeniden derle** (veya tuşlarına basarak **CTRL-SHIFT-B**). 

Hatalar varsa, önceki adımlarda size gidin ve uygulama başarıyla derlemeler kadar hataları düzeltin. Gibi bir derleme hatası alırsanız _kaynak geçerli bağlamda mevcut değil_, doğrulayın ad alanı adı **MainActivity.cs** proje adıyla eşleşen (`Phoneword`) ve ardından tamamen çözümü yeniden derleyin. Hala derleme hataları alırsanız, en son Xamarin.Android güncelleştirmelerin yüklü olduğunu doğrulayın.

### <a name="setting-the-label-and-app-icon"></a>Etiket ve uygulama simgesi ayarlama

Artık çalışan bir uygulama olmalıdır &ndash; tamamlama rötuşları ekleme zamanı geldi! İçinde **MainActivity.cs**, düzenleme `Label` için `MainActivity`. `Label` Olan Android uygulamada oldukları bilmesini sağlamak için ekranın üstünde görüntüler.
Üstündeki `MainActivity` sınıfı, değişiklik `Label` için `Phone Word` aşağıda gösterildiği gibi:

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

Şimdi uygulamayı ayarlamak için zaman yapılır. Varsayılan olarak, Visual Studio Proje için varsayılan bir simge sağlar. Şimdi bu dosyaları çözümden silmek ve bunları farklı bir simge ile değiştirin. Genişletme **kaynakları** klasöründe **çözüm paneli**. İle önek beş klasörleri olduğuna dikkat edin **mipmap -**, ve bu klasörlerinin her biri tek bir içeren **Icon.png** dosyası:

[![Mipmap klasörlerinde ve Icon.png dosyaları](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png)

Bu simge dosyaların projeden silmek gereklidir. Sağ her birini tıklatın **Icon.png** dosyaları ' nı seçip **silmek** ve bağlam menüsünden:
   
[![Varsayılan Icon.png Sil](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png)
   
Tıklayın **silmek** iletişim kutusunda düğme.

Ardından, indirip sıkıştırmasını [Xamarin uygulaması simgeler ayarlama](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Bu zip dosyasını uygulama için simgeleri içerir. Her bir simgenin görsel olarak aynıdır, ancak farklı çözünürlüklerde doğru farklı ekran densities ile farklı cihazlarda işleyen.  Dosyaları kümesini Xamarin.Android projeye kopyalanmalıdır. Visual Studio içinde **Çözüm Gezgini**, sağ **mipmap hdpi** klasörü ve seçin **Ekle > Varolan öğeleri**:

[![Dosyaları Ekle](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png)

Seçimi iletişim kutusundan sıkıştırması açılmış Xamarin AdApp simgeleri dizinine gidin ve açmak **mipmap hdpi** klasör. Seçin **Icon.png** tıklatıp **Ekle**.

Her biri için bu adımları yineleyin **mipmap -** içeriğini kadar klasörleri **mipmap -** Xamarin uygulama simgeleri klasörler, bunların karşılık gelen kopyalanır **mipmap -** klasörlerde **Phoneword** projesi.

Tüm simgeleri Xamarin.Android projesi kopyaladıktan sonra açmak **proje seçenekleri** nde projeye sağ tıklatarak iletişim **çözüm paneli**. Seçin **Yapı > Android uygulaması** seçip  **@mipmap/icon**  gelen **uygulama simgesi** birleşik giriş kutusu:

[![Proje simgesini ayarlama](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png)

### <a name="running-the-app"></a>Uygulamayı çalıştırma

Son olarak, uygulama, bir Android cihaz veya öykünücü çalışır ve bir Phoneword çevirme test edin:

[![Bu tamamlandığında uygulamasının ekran görüntüsü](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'yu başlatın **uygulamaları** klasör veya **Spotlight**. 

Tıklatın **yeni çözüm...**  yeni bir proje oluşturmak için.

İçinde **yeni projeniz için bir şablon seçin** iletişim kutusunda, tıklatın **Android > Uygulama** seçip **Android uygulaması** şablonu. **İleri**'ye tıklayın.

[![Android uygulaması şablonu seçin](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png)

İçinde **Android uygulamanızı yapılandırma** iletişim kutusunda, yeni uygulama adı `Phoneword` tıklatıp **sonraki**.

[![Android uygulamasını yapılandırma](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png)

İçinde **yeni Android uygulamanızı yapılandırma** iletişim, çözüm ve proje adları kümesine bırakın `Phoneword` tıklatıp **oluşturma** projesi oluşturmak için.

### <a name="creating-the-layout"></a>Düzen oluşturma

Yeni Proje oluşturulduktan sonra genişletin **kaynakları** klasörünü ve ardından **düzeni** klasöründe **çözüm** paneli.
Çift **Main.axml** Android tasarımcısında açın. Bu Android Tasarımcısı'nda görüntülendiğinde ekranı düzeni dosyasıdır:

[![Main.AXML açın](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png)

Seçin **Merhaba Dünya, kendinize tıklayın!** **Düğme** basın ve tasarım yüzeyi **silmek** kaldırmak için anahtar. 

Gelen **araç** (sağ alanı) girin `text` sürükleyin ve arama alanı içine bir **metin (büyük)** pencere öğesi (alan Merkezi'ndeki) tasarım yüzeyine:

[![Büyük metin pencere öğesi ekleme](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png)

İle **metin (büyük)** kullanabileceğiniz tasarım yüzeyine seçili pencere, **özellikleri** değiştirmek için paneli `Text` özelliği **metin (büyük)** için pencere öğesi `Enter a Phoneword:` aşağıda gösterildiği gibi:

[![Büyük metin pencere öğesi özelliklerini ayarlama](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png)

Ardından, sürükleyin bir **düz metin** pencere öğesini **araç** tasarım yüzey ve altına yerleştirin **metin (büyük)** pencere öğesi. Pencere öğeleri ada göre bulmak için arama alanı kullanabilirsiniz dikkat edin:

[![Düz metin pencere öğesi ekleme](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png)

İle **düz metin** kullanabileceğiniz tasarım yüzeyine seçili pencere, **özellikleri** değiştirmek için paneli `Id` özelliği **düz metin** içinpencereöğesi`@+id/PhoneNumberText` değiştirip `Text` özelliğine `1-855-XAMARIN`:

[![Düz metin pencere öğesi özelliklerini ayarlama](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png)

Sürükleme bir **düğmesini** gelen **araç** tasarım yüzey ve altına yerleştirin **düz metin** pencere öğesi:

[![Düğme ekleme](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png)

İle **düğmesini** kullanabileceğiniz tasarım yüzeyine seçili, **özellikleri** değiştirmek için paneli `Id` özelliği **düğmesini** için `@+id/TranslateButton` ve değiştirme `Text` özelliğine `Translate`:

[![Çeviri düğmesi olarak yapılandırın](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png)

Sürükleme bir **kutusu TextView** gelen **araç** tasarım yüzey ve altında yerleştirin **düğmesini** pencere öğesi. İle **kutusu TextView** seçili ayarlamak `id` özelliği **kutusu TextView** için `@+id/TranslatedPhoneWord` değiştirip `text` boş bir dize için:

[![Metin görünümü özellikleri ayarlayın.](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png)    

Tuşlarına basarak çalışmanızı kaydedin **&#8984; + S**.

### <a name="writing-translation-code"></a>Çeviri kod yazma

Şimdi, telefon numaralarını alfasayısal sayısal çevirmek için bazı kod ekleyin. Sonraki dişli simgesine tıklayarak yeni bir dosya projeye ekleyin **Phoneword** proje **çözüm** paneli ve seçme **Ekle > yeni dosya...** :

[![Yeni bir dosya projeye ekleyin](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png)

İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıfı**, yeni dosya adı **PhoneTranslator**, tıklatıp **yeni**. Bu yeni sınıf C# boş bize oluşturur.

Tüm şablon kodu Yeni sınıfta kaldırıp aşağıdaki kodla değiştirin:

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

Değişiklikleri kaydetmek **PhoneTranslator.cs** seçerek dosya **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**), dosyayı kaydedip kapatın. Derleme zamanı hataları çözümü yeniden oluşturma emin olun.

### <a name="wiring-up-the-interface"></a>Kablolama arabirimi ayarlama

Kullanıcı arabirimini oluşturan yedekleme koda ekleyerek wire üzere kod eklemek için sonraki adımdır `MainActivity` sınıfı.
Çift **MainActivity.cs** içinde **çözüm paneli** açın.

Başlamak için bir olay işleyicisi ekleyerek **çevir** düğmesi. İçinde `MainActivity` sınıfı, Bul `OnCreate` yöntemi. İçinde düğmesi kod ekleme `OnCreate`, aşağıdaki `base.OnCreate(bundle)` ve `SetContentView (Resource.Layout.Main)` çağrıları. Kod işleme şablonu düğmesini kaldırmak için `OnCreate` yöntemi aşağıdakine benzer:

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

Ardından, bir başvuru Android Tasarımcısı ile düzeni dosyasında oluşturulan denetimleri için gereklidir. Aşağıdaki kodu ekleyin `OnCreate` yöntemi (çağrısından sonra `SetContentView`):

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

İçin kullanıcı basarsa yanıt kodu ekleyin **çevir** aşağıdaki kodu ekleyerek düğmesi `OnCreate` yöntemi (sonra son adımda eklenen satırlar):

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

Çalışmanızı kaydedin ve uygulamayı seçerek yapı **Yapı > Yapı tüm** (veya tuşlarına basarak **&#8984; + B**). Uygulama derler, Mac için Visual Studio üstündeki bir başarı iletisi alırsınız:

Hatalar varsa, önceki adımlarda size gidin ve uygulama başarıyla derlemeler kadar hataları düzeltin. Gibi bir derleme hatası alırsanız _kaynak geçerli bağlamda mevcut değil_, doğrulayın ad alanı adı **MainActivity.cs** proje adıyla eşleşen (`Phoneword`) ve ardından tamamen çözümü yeniden derleyin. Hala derleme hataları alırsanız, en son Xamarin.Android yüklediyseniz ve Mac için Visual Studio güncelleştirmeleri doğrulayın.

### <a name="setting-the-label-and-app-icon"></a>Etiket ve uygulama simgesi ayarlama

Çalışan bir uygulama sahip olduğunuza göre son rötuşları ekleme zamanı geldi! Başlangıç düzenleyerek `Label` için `MainActivity`.
`Label` Olan Android uygulamada oldukları bilmesini sağlamak için ekranın üstünde görüntüler. Üstündeki `MainActivity` sınıfı, değişiklik `Label` için `Phone Word` aşağıda gösterildiği gibi:

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

Şimdi uygulamayı ayarlamak için zaman yapılır. Varsayılan olarak, Mac için Visual Studio Proje için varsayılan bir simge sağlar. Şimdi bu dosyaları çözümden silmek ve bunları farklı bir simge ile değiştirin. Genişletme **kaynakları** klasöründe **çözüm paneli**. İle önek beş klasörleri olduğuna dikkat edin **mipmap -**, ve bu klasörlerinin her biri tek bir içeren **Icon.png** dosyası:

[![Mipmap klasörlerinde ve Icon.png dosyaları](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png)

Bu simge dosyaların projeden silmek gereklidir. Sağ her birini tıklatın **Icon.png** dosyaları ' nı seçip **kaldırmak** ve bağlam menüsünden:

[![Varsayılan Icon.png Sil](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png)

Tıklayın **silmek** iletişim kutusunda düğme.

Ardından, indirip sıkıştırmasını [Xamarin uygulaması simgeler ayarlama](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Bu zip dosyasını uygulama için simgeleri içerir. Her bir simgenin görsel olarak aynıdır, ancak farklı çözünürlüklerde doğru farklı ekran densities ile farklı cihazlarda işleyen.  Dosyaları kümesini Xamarin.Android projeye kopyalanmalıdır. Mac için Visual Studio içinde **çözüm paneli**, sağ tıklatın **mipmap hdpi** klasörü ve seçin **Ekle > dosyaları Ekle**:

[![Dosyaları Ekle](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png)

Seçimi iletişim kutusundan sıkıştırması açılmış Xamarin AdApp simgeleri dizinine gidin ve açmak **mipmap hdpi** klasör. Seçin **Icon.png** tıklatıp **açık**.

İçinde **klasöre dosya eklemek** iletişim kutusunda **dosyasını dizinine kopyalayın** tıklatıp **Tamam**:

[![Dizin iletişim kutusuna dosya kopyalama](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png)

Her biri için bu adımları yineleyin **mipmap -** içeriğini kadar klasörleri **mipmap -** Xamarin uygulama simgeleri klasörler, bunların karşılık gelen kopyalanır **mipmap -** klasörlerde **Phoneword** projesi.

Tüm simgeleri Xamarin.Android projesi kopyaladıktan sonra açmak **proje seçenekleri** nde projeye sağ tıklatarak iletişim **çözüm paneli**. Seçin **Yapı > Android uygulaması** seçip  **@mipmap/icon**  gelen **uygulama simgesi** birleşik giriş kutusu:

[![Proje simgesini ayarlama](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png)

### <a name="running-the-app"></a>Uygulamayı çalıştırma

Son olarak, uygulama, bir Android cihaz veya öykünücü çalışır ve bir Phoneword çevirme test edin:

[![Bu tamamlandığında uygulamasının ekran görüntüsü](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png)

-----

İlk Xamarin.Android uygulamanız Tamamlanıyor Tebrikler!
Artık yalnızca öğrendiniz becerileri ve araçları dissect zamanı geldi. Sonraki çalışır durumda [Hello, Android derinlemesine](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin Andrid uygulama simgeleri (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (örnek)](https://developer.xamarin.com/samples/monodroid/Phoneword)
