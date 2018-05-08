---
title: Bağlama bir. AAR
description: Bu kılavuz, bir Android bir Xamarin.Android Java bağlamaları kitaplığı oluşturmak için adım adım yönergeler sağlar. AAR dosyası.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/11/2018
ms.openlocfilehash: 54708a7cfd071f77968991c9fe4e52938697c9bb
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="binding-an-aar"></a>Bağlama bir. AAR

_Bu kılavuz, bir Android bir Xamarin.Android Java bağlamaları kitaplığı oluşturmak için adım adım yönergeler sağlar. AAR dosyası._


## <a name="overview"></a>Genel Bakış

*Android arşiv (. AAR)* dosyasıdır Android kitaplıkları için dosya biçimi.
Bir. AAR dosyası bir. ZIP arşivini aşağıdakileri içerir:

-   Derlenmiş Java kod
-   Kaynak kimlikleri
-   Kaynaklar
-   Meta veriler (örneğin, etkinlik bildirimleri, izinleri)

Bu kılavuzda, biz tek bir için bağlamaları kitaplığı oluşturma temelleri adım. AAR dosyası. Java kitaplığı (temel kod örneği ile) genel bağlama genel bakış için bkz: [bir Java kitaplığı bağlama](~/android/platform/binding-java-library/index.md).


> [!IMPORTANT]
> Bir bağlama proje yalnızca birini içerebilir. AAR dosyası. Varsa. Diğer AAR bağımlılıkları. AAR sonra bu bağımlılıkların kendi bağlama projesinde bulunan ve ardından başvurulan. Bkz: [hata 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).


## <a name="walkthrough"></a>İzlenecek yol

Android Arşiv dosya örnek Android Studio'da oluşturulan için bağlamaları kitaplığı oluşturacağız [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). Bu. AAR içeren bir `TextCounter` sesli harf ve bir dizede ünsüzler sayısı statik yöntemleriyle sınıfı. Ayrıca, **textanalyzer.aar** sayım sonuçları görüntülemek amacıyla bir görüntü kaynağı içeriyor.

Aşağıdaki adımlar bir bağlamaları Kitaplığı'ndan oluşturmak için kullanacağız. AAR dosyası:

1. Yeni bir Java bağlamaları kitaplık projesi oluşturun.

2. Tek bir ekleyin. Proje dosyası AAR. Bir bağlama proje yalnızca tek bir içerebilir. AAR.

3. Uygun yapı eylemi için ayarlayın. AAR dosyası.

4. Bir hedef Framework'ü seçin. AAR destekler.

5. Bağlamaları kitaplığı oluşturun.

Biz bağlamaları kitaplığı oluşturduktan sonra size bir metin dizesi çağrıları kullanıcıdan ister küçük bir Android uygulaması geliştireceksiniz. Metin çözümlemek için AAR yöntemleri görüntüsünü alır. AAR ve sonuçları görüntüsü ile birlikte görüntüler.

Örnek uygulaması erişecek `TextCounter` sınıfının **textanalyzer.aar**:

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

Ayrıca, bu örnek uygulaması alıp içinde paketlenmiş bir görüntü kaynağı görüntüleyin **textanalyzer.aar**:

[![Xamarin monkey görüntüsü](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Bu görüntü kaynağı konumunda bulunan **res/drawable/monkey.png** içinde **textanalyzer.aar**.



### <a name="creating-the-bindings-library"></a>Bağlamaları kitaplığı oluşturma

Aşağıdaki adımlarla başlatmadan önce lütfen örnek indirin [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android arşiv dosyası:

1.  Android bağlamaları kitaplığı şablonu ile başlayarak yeni bir bağlamaları kitaplığı projesi oluşturun. Mac için Visual Studio veya Visual Studio (Visual Studio aşağıdaki ekran görüntüleri gösterir, ancak Mac için Visual Studio çok benzer) kullanabilirsiniz. Çözüm adı **AarBinding**:

    [![AarBindings projesi oluşturma](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2.  Şablon içeren bir **Jar'lar** eklediğiniz klasörü. AAR(s) bağlamaları kitaplığı projesine. Sağ **Jar'lar** klasörü ve select **Ekle > varolan öğeyi**:

    [![Varolan öğeyi Ekle](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  Gidin **textanalyzer.aar** dosyasını indirdiğiniz daha önce sunucuyu seçin ve tıklatın **Ekle**:

    [![Textanalayzer.AAR Ekle](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  Doğrulayın **textanalyzer.aar** dosyası projeye başarıyla eklendi:

    [![Textanalyzer.aar dosya eklendi](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  Yapı eylem için **textanalyzer.aar** için `LibraryProjectZip`. Mac için Visual Studio'da sağ **textanalyzer.aar** yapı eylemi ayarlamak için. Visual Studio'da, yapı eylemi ayarlanabilir **özellikleri** bölmesi):

    [![Ayar için LibraryProjectZip textanalyzer.aar yapı eylemi](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  Projeyi yapılandırmak için özelliklerini açmak *hedef Framework*. Varsa. AAR tüm Android API'lerini kullanır, API düzeyi hedef Framework'ü ayarlayın. AAR bekliyor. (Hedef Framework ayarını ve genel Android API düzeyleri hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).)

    API düzeyi hedef bağlamaları kitaplığınızı ayarlayın. Bu örnekte, biz çünkü en son platform API düzeyini (API düzeyi 23) kullanmak boş bizim **textanalyzer** bir bağımlılık Android API yok:

    [![API 23 hedef düzeyini ayarlama](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  Bağlamaları kitaplığı oluşturun. Bağlamaları kitaplığı projesi başarılı bir şekilde oluşturmak ve bir çıktı oluşturmak gerekir. DLL şu konumda: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>Bağlamaları kitaplığı kullanma

Bu kullanmak için. DLL'si Xamarin.Android uygulamanıza ilk bağlamaları kitaplığına bir başvuru eklemeniz gerekir. Aşağıdaki adımları kullanın:

1.  Bu kılavuzda basitleştirmek için bağlamaları kitaplığı olarak aynı çözümde bu uygulamayı oluşturuyoruz. (Bağlamaları kitaplığı tüketen uygulamayı da farklı bir çözümde bulunan.) Yeni bir Xamarin.Android uygulaması oluşturma: çözüme sağ tıklayın ve seçin **Yeni Proje Ekle**. Yeni proje adı **BindingTest**:

    [![Yeni BindingTest projesi oluşturma](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2.  Sağ **başvuruları** düğümünün **BindingTest** proje ve seçin **Başvuru Ekle...** :

    [![Başvuru Ekle'yi tıklatın](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  Seçin **AarBinding** daha önce oluşturduğunuz proje ve tıklatın **Tamam**:

    [![AAR bağlama proje denetleyin](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  Açık **başvuruları** düğümünün **BindingTest** doğrulamak için proje **AarBinding** başvuru varsa:

    [![AarBinding başvuruları altında listelenir](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


Bağlama kitaplığı projesi içeriğini görüntülemek istiyorsanız, başvuru içinde açmak için çift tıklayarak **Nesne Tarayıcısı**. Eşlenen içeriğini görebilir `Com.Xamarin.Textcounter` ad alanı (Java eşlenen `com.xamarin.textanalyzezr` paket) ve üyeleri görüntüleyebilir `TextCounter` sınıfı:

[![Nesne Tarayıcısı görüntüleme](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

Yukarıdaki ekran iki vurgular `TextAnalyzer` örnek uygulama çağıracak yöntemleri: `NumConsonants` (temel Java sarmalar `numConsonants` yöntemi), ve `NumVowels` (temel Java sarmalar `numVowels` yöntemi).



### <a name="accessing-aar-types"></a>Erişme. AAR türleri

Bağlama Kitaplığı'na işaret uygulamanız için bir başvuru ekledikten sonra Java türlerinde erişebilirsiniz. Yazarken AAR C# türleri (C# sarmalayıcıları) sayesinde erişir. C# uygulama kodu çağırabilir `TextAnalyzer` Bu örnekte gösterildiği gibi yöntemleri:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

Yukarıdaki örnekte, statik yöntemler arıyoruz `TextCounter` sınıfı. Ancak, ayrıca sınıfları örneği ve örnek yöntemlerini çağırın. Örneğin, varsa. AAR sarmalar adlı bir sınıf `Employee` örnek yöntemi olan `buildFullName`, örneğini oluşturabilirsiniz `MyClass` ve görülen burada kullanın:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Metin, kullanımlar için kullanıcıdan aşağıdaki adımları kod uygulamaya ekleyebilirsiniz, böylece `TextCounter` metin ve görüntüler sonuçlarını analiz etmek için.

Değiştir **BindingTest** Düzen (**Main.axml**) aşağıdaki XML ile. Bu düzenine sahip bir `EditText` metin girişi ve sesli ve sessiz sayısını başlatma için iki düğmeleri için:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

Değiştir **MainActivity.cs** aşağıdaki kod ile. Bu örnekte görüldüğü gibi düğmesi olay işleyicileri araması Sarmalanan `TextCounter` bulunan yöntemleri. Sonuçları görüntülemek için bildirimleri AAR ve kullanın. Bildirim `using` ilişkili kitaplık ad alanı için deyimi (Bu durumda, `Com.Xamarin.Textcounter`):

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

Derleme ve çalıştırma **BindingTest** projesi. Uygulamayı başlatın ve sol tarafta ekran sunmak ( `EditText` bazı metinle başlatılır, ancak bunu değiştirmek için dokunun). Dokunduğunuzda **sayısı sesli harf**, sağ tarafta gösterildiği gibi bir bildirim sesli harf sayısını görüntüler:

[![BindingTest çalışmasını ekran görüntüleri](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Dokunmayı deneyin **sayısı ÜNSÜZLER** düğmesi. Ayrıca, metin satırının değiştirin ve tekrar için farklı sesli test etmek için bu düğmeleri dokunun ve sessiz sayar.



### <a name="accessing-aar-resources"></a>Erişme. AAR kaynakları

Birleştirmeler tooling Xamarin **R** verileri. Uygulamanızın içine AAR **kaynak** sınıfı. Sonuç olarak, erişebilirsiniz. AAR kaynakları düzeninizi (ve arka plan kodu) aynı şekilde içinde olan kaynaklara erişim gibi **kaynakları** projenizin yolu.

Bir görüntü kaynağa erişmek için kullandığınız **Resource.Drawable** görüntü içinde paketlenmiş için ad. AAR. Örneğin, başvuru **image.png** içinde. Kullanarak AAR dosya `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Bulunan kaynak düzenlerini de erişebilirsiniz. AAR. Bunu yapmak için kullandığınız **Resource.Layout** içinde paketlenmiş Düzen adı. AAR. Örneğin:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**Textanalyzer.aar** örnek içeren konumunda bulunan bir görüntü dosyası **res/drawable/monkey.png**. Şimdi bu görüntü kaynağa erişim ve bizim örnek uygulama kullanma:

Düzen **BindingTest** Düzen (**Main.axml**) ve ekleme bir `ImageView` sonuna `LinearLayout` kapsayıcı. Bu `ImageView` bulunan görüntü görüntüler **@drawable/monkey**; bu görüntü kaynak bölümünden yüklenecek **textanalyzer.aar**:

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

Derleme ve çalıştırma **BindingTest** projesi. Uygulamayı başlatın ve sol tarafta ekran sunmak &ndash; dokunduğunuzda **sayısı ÜNSÜZLER**, sonuçlar sağ tarafta gösterildiği gibi görüntülenir:

[![BindingTest ünsüz sayısı görüntüleme](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


Tebrikler! Java kitaplığı başarıyla bağladınız. AAR!



## <a name="summary"></a>Özet

Bu kılavuzda, oluşturduğumuz bağlamaları kitaplığı için bir. AAR dosya en az test uygulaması için bağlamaları kitaplık eklenebilir ve bizim C# kodu bulunan Java kod çağırabilir doğrulamak için uygulama çalıştırılmıştır. AAR dosyası.
Ayrıca, biz erişmek ve bulunan bir görüntü kaynağı görüntülemek için uygulama genişletilmiş. AAR dosyası.


## <a name="related-links"></a>İlgili bağlantılar

- [Java bağlamaları kitaplığı (video) oluşturma](https://university.xamarin.com/classes#10090)
- [.JAR’ı Bağlama](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java Kitaplığını Bağlama](~/android/platform/binding-java-library/index.md)
- [AarBinding (örnek)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [Hata 44573 bir proje birden çok .aar dosyaları bağlanamıyor](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
