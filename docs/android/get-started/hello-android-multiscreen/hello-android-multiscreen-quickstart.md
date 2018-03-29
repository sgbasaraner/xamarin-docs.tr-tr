---
title: 'Merhaba, Android Multiscreen: Hızlı Başlangıç'
description: Bu iki parçalı kılavuz ikinci ekranı işlemek için Phoneword uygulama genişletir. Yol boyunca temel Android uygulama yapı taşları daha derin Dalış Android mimarisi içine ile sunulur.
ms.topic: article
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 2913508159787f6d369f5e55f879addfc1b2ba4f
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="hello-android-multiscreen-quickstart"></a>Merhaba, Android Multiscreen: Hızlı Başlangıç

_Bu iki parçalı kılavuz ikinci ekranı işlemek için Phoneword uygulama genişletir. Yol boyunca temel Android uygulama yapı taşları daha derin Dalış Android mimarisi içine ile sunulur._

## <a name="hello-android-multiscreen-quickstart"></a>Merhaba, Android Multiscreen hızlı başlangıç

Bu kılavuzun izlenecek bölümünde, ikinci bir ekrana ekleyeceksiniz [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) numaraları geçmişini izlemek için uygulama çevrilen uygulamasını kullanarak. [Son uygulama](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen/) "çevrilip" numaralarını görüntüler ikinci bir ekran sağdaki ekran tarafından gösterildiği şekilde olacaktır:

[![Örnek uygulama ekran görüntüleri](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

Eşlik eden [derinlemesine](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md) ne oluşturulmuş gözden geçirir ve mimari, gezinti ve yol boyunca karşılaştı diğer yeni Android kavramlar açıklanır.


## <a name="requirements"></a>Gereksinimler

Bu kılavuz nerede depolanacağını Çekmeleri çünkü [Hello, Android](~/android/get-started/hello-android/index.md) devre dışı bırakır, tamamlanmasından gerektirir [Hello, Android Hızlı Başlangıç](~/android/get-started/hello-android/hello-android-quickstart.md).
Aşağıdaki kılavuz doğrudan atlamak istiyorsanız, tamamlanmış sürümünü indirebilirsiniz [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) (Başlangıç Hello Android Hızlı Başlangıç) ve gözden geçirme başlatmak için kullanın.

## <a name="walkthrough"></a>İzlenecek yol

Bu kılavuzda, ekleyeceksiniz bir **çeviri geçmişi** için ekran **Phoneword** uygulama.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Başlangıç açarak **Phoneword** Visual Studio ve düzenleme uygulama **Main.axml** dosya **Çözüm Gezgini**.

### <a name="updating-the-layout"></a>Düzeni güncelleştiriliyor

Gelen **araç**, sürükleyin bir **düğmesini** tasarım üzerine yüzey ve aşağıdaki yerleştirin **TranslatedPhoneWord** kutusu TextView. İçinde **özellikleri** bölmesinde düğmeyi değiştirmek **kimliği** için `@+id/TranslationHistoryButton` 

[![Yeni bir düğme sürükleyin](hello-android-multiscreen-quickstart-images/vs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/vs/02-new-button.png#lightbox)

Ayarlama **metin** düğme özelliğinin `@string/translationHistory`. Android Tasarımcısı bu tam anlamıyla yorumlar ancak düğmenin metni doğru gösterir böylece birkaç değişiklik oluşturacağız:

[![Çeviri geçmişi düğme metni ayarlama](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string-sml.png)](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string.png#lightbox)

Genişletme **değerleri** düğümü altında **kaynakları** klasöründe **Çözüm Gezgini** ve dize kaynakları dosyasını çift tıklatın **Strings.xml**:

[![Strings.XML açın](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file.png#lightbox)

Ekleme `translationHistory` dize adı ve değeri **Strings.xml** dosya ve kaydedin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**Çeviri geçmişi** düğme metni yeni bir dize değeri yansıtacak şekilde güncelleştirin:

[![Yeni bir dize değeri düğmesi yansıtır](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png#lightbox)

İle **çeviri geçmişi** düğmesinin seçili tasarım yüzeyine, Bul `enabled` ayarı **özellikleri** bölmesi ve değerini ayarlama `false` düğmesini devre dışı bırakmak için. Bu tasarım yüzeyine koyu olacak düğmesi neden olur:

[![Çeviri Geçmiş düğmesini devre dışı bırak](hello-android-multiscreen-quickstart-images/vs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/vs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>İkinci etkinlik oluşturma

İkinci ekranı güç için ikinci bir etkinlik oluşturun. İçinde **Çözüm Gezgini**, sağ **Phoneword** proje ve seçin **Ekle > Yeni öğe...** :

[![Yeni bir dosya ekleyin](hello-android-multiscreen-quickstart-images/vs/07-add-new-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/07-add-new-file.png#lightbox)

İçinde **Yeni Öğe Ekle** iletişim kutusunda, seçin **Visual C# > etkinlik** ve etkinlik dosya adı **TranslationHistoryActivity.cs**.

Şablon kodla **TranslationHistoryActivity.cs** aşağıdaki:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

Bu sınıf, oluşturmakta olduğunuz bir `ListActivity` ve yeni bir düzen dosyası için bu etkinliği oluşturma gerek kalmaması programlı olarak doldurma. Bu daha ayrıntılı olarak ele alınmıştır [Hello, Android Multiscreen derinlemesine](~/android/get-started/hello-android/hello-android-deepdive.md).

### <a name="adding-translation-history-code"></a>Çeviri geçmişi kod ekleme

Bu uygulamayı (kullanıcı ilk ekranda çevrilen) telefon numaralarını toplar ve bunları ikinci ekranına geçirir. Telefon numaralarını dizelerinin listesini depolanır. Listeleri desteklemek için aşağıdaki ekleyin `using` üstüne yönerge `MainActivity` sınıfı:

```csharp
using System.Collections.Generic;
```

Ardından, telefon numaralarıyla doldurulabilir boş bir liste oluşturun.
`MainActivity` Sınıfı şu şekilde görünür:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

İçinde `MainActivity` sınıfı, kaydetmek için aşağıdaki kodu ekleyin **çeviri geçmişi** düğmesini (Bu satırından sonra yerleştirin `translateButton` bildirimi):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Sonuna aşağıdaki kodu ekleyin `OnCreate` yukarı kablo yöntemi **çeviri geçmişi** düğmesi:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Güncelleştirme **çevir** telefon numarasını listesine eklemek için düğmeyi `phoneNumbers`. `Click` İşleyicisi `TranslateHistoryButton` aşağıdaki kodu benzemelidir:

```csharp
// Add code to translate number
string translatedNumber = string.Empty;
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

Hiçbir hata bulunmadığından emin olmak için uygulama oluşturma ve kaydedin.

### <a name="running-the-app"></a>Uygulamayı çalıştırma

Bir öykünücü veya aygıt uygulamayı dağıtın. Aşağıdaki ekran görüntüleri çalışmasını göstermeye **Phoneword** uygulama:

[![Örnek ekran görüntüleri](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Başlangıç açarak **Phoneword** Mac ve düzenlemek için Visual Studio Proje **Main.axml** dosya **çözüm paneli**.

### <a name="updating-the-layout"></a>Düzeni güncelleştiriliyor

Gelen **araç**, sürükleyin bir **düğmesini** tasarım üzerine yüzey ve aşağıdaki yerleştirin **TranslatedPhoneWord** kutusu TextView. İçinde **özellikleri** paneli, düğmeyi değiştirmek **kimliği** için `@+id/TranslationHistoryButton` 

[![Yeni bir düğme sürükleyin](hello-android-multiscreen-quickstart-images/xs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/xs/02-new-button.png#lightbox)

Ayarlama **metin** düğme özelliğinin `@string/translationHistory`. Android Tasarımcısı bu tam anlamıyla yorumlar ancak düğmenin metni doğru gösterir böylece birkaç değişiklik oluşturacağız:

[![Çeviri geçmişi düğme metni ayarlama](hello-android-multiscreen-quickstart-images/xs/03-call-history-string-sml.png)](hello-android-multiscreen-quickstart-images/xs/03-call-history-string.png#lightbox)


Genişletme **değerleri** düğümü altında **kaynakları** klasöründe **çözüm paneli** ve dize kaynakları dosyasını çift tıklatın **Strings.xml**:

[![Açık dizeleri](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file.png#lightbox)


Ekleme `translationHistory` dize adı ve değeri **Strings.xml** dosya ve kaydedin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**Çeviri geçmişi** düğme metni yeni bir dize değeri yansıtacak şekilde güncelleştirin:

[![Yeni bir dize değeri düğmesi yansıtır](hello-android-multiscreen-quickstart-images/xs/05-new-string-value-sml.png)](hello-android-multiscreen-quickstart-images/xs/05-new-string-value.png#lightbox)


İle **çeviri geçmişi** tasarım yüzeyine, open seçili düğmesi **davranışı** sekmesinde **özellikleri paneli** çift tıklayın ve **etkin**  düğmesi devre dışı bırakmak için onay kutusu. Bu tasarım yüzeyine koyu olacak düğmesi neden olur:

[![Çeviri Geçmiş düğmesini devre dışı bırak](hello-android-multiscreen-quickstart-images/xs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/xs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>İkinci etkinlik oluşturma

İkinci ekranı güç için ikinci bir etkinlik oluşturun. İçinde **çözüm paneli**, gri dişli simgesine tıklayın **Phoneword** proje ve seçin **Ekle > yeni dosya...** :

Gelen **yeni dosya** iletişim kutusunda, seçin **Android > etkinlik**, etkinlik adı `TranslationHistoryActivity`, ardından **Ekle**.

Şablon kodla `TranslationHistoryActivity` aşağıdaki:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

Bu sınıftaki bir `ListActivity` oluşturulur ve yeni bir düzen dosyası için bu etkinliği oluşturmak zorunda kalmamak için programlı olarak doldurulur. Bu daha ayrıntılı olarak anlatılmıştır [Hello, Android Multiscreen derinlemesine](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).

### <a name="adding-translation-history-code"></a>Çeviri geçmişi kod ekleme

Bu uygulamayı (kullanıcı ilk ekranda çevrilen) telefon numaralarını toplar ve bunları ikinci ekranına geçirir. Telefon numaralarını dizelerinin listesini depolanır. Listeleri desteklemek için aşağıdaki ekleyin `using` üstüne yönerge `MainActivity` sınıfı:

```csharp
using System.Collections.Generic;
```

Ardından, telefon numaralarıyla doldurulabilir boş bir liste oluşturun. `MainActivity` Sınıfı şu şekilde görünür:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

İçinde `MainActivity` sınıfı, kaydetmek için aşağıdaki kodu ekleyin **TranslationHistory geçmişi** düğmesini (Bu satırından sonra yerleştirin `TranslationHistoryButton` bildirimi):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Sonuna aşağıdaki kodu ekleyin `OnCreate` yukarı kablo yöntemi **çeviri geçmişi** düğmesi:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Güncelleştirme **çevir** telefon numarasını listesine eklemek için düğmeyi `phoneNumbers`. `Click` İşleyicisi `TranslateHistoryButton` aşağıdaki kodu benzemelidir:

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

### <a name="running-the-app"></a>Uygulamayı çalıştırma

Bir öykünücü veya aygıt uygulamayı dağıtın. Aşağıdaki ekran görüntüleri çalışmasını göstermeye **Phoneword** uygulama:

[![Örnek ekran görüntüleri](hello-android-multiscreen-quickstart-images/screenshot.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

-----

İlk çok ekran Xamarin.Android uygulamanız Tamamlanıyor Tebrikler! Araçlar ve beceriler dissect artık yalnızca öğrenilen &ndash; sonraki çalışır durumda [Hello, Android Multiscreen derinlemesine](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin uygulama simgeleri & başlatma ekranlar (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (örnek)](https://developer.xamarin.com/samples/monodroid/Phoneword)
- [PhonewordMultiscreen (örnek)](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen)
