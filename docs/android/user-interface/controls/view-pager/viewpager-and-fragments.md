---
title: "Parçalarla ViewPager"
description: "ViewPager jestsel Gezinti uygulamanıza olanak sağlayan bir düzen yöneticisidir. Sol ve sağ adıma veri sayfaları aracılığıyla jestsel Gezinti manyetik kullanıcıya sağlar. Bu kılavuz, parçaları veri sayfalarını kullanarak ViewPager ile swipeable bir kullanıcı Arabirimi uygulayan açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: cd71617cce209ef0127023f69c2b503fee031e43
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-fragments"></a>Parçalarla ViewPager

_ViewPager jestsel Gezinti uygulamanıza olanak sağlayan bir düzen yöneticisidir. Sol ve sağ adıma veri sayfaları aracılığıyla jestsel Gezinti manyetik kullanıcıya sağlar. Bu kılavuz, parçaları veri sayfalarını kullanarak ViewPager ile swipeable bir kullanıcı Arabirimi uygulayan açıklanmaktadır._

 
## <a name="overview"></a>Genel Bakış

`ViewPager` Böylece kullanım ömrü boyunca her sayfada, daha kolay genellikle parçaları ile birlikte kullanılan `ViewPager`. Bu kılavuzda `ViewPager` oluşturmak için kullanılan bir uygulama olarak adlandırılan **FlashCardPager** matematik sorunlarını bir dizi flash kartlarında sayısını gösterir. Her flash kartı bir parçası olarak uygulanır. Kullanıcı sağa ve sola flash kartları aracılığıyla swipes ve kendi yanıt ortaya çıkarmak için bir matematik sorun dokunur. Bu uygulamayı oluşturur bir `Fragment` her flash kartı ve uygulayan bağdaştırıcıyı türetilmiş bir örneğin `FragmentPagerAdapter`. İçinde [Viewpager ve görünümleri](~/android/user-interface/controls/view-pager/viewpager-and-views.md), işlerin çoğunu yapıldı `MainActivity` yaşam döngüsü yöntemleri. İçinde **FlashCardPager**, işlerin çoğunu yapılır bir `Fragment` yaşam döngüsü yöntemlerinden birini. 

Bu kılavuz parçaları temelleri kapsamaz &ndash; Xamarin.Android parçalanma alışık değilseniz, bkz: [parçaları](~/android/platform/fragments/index.md) parçaları ile çalışmaya başlamanıza yardımcı olmak için. 



## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Adlı yeni bir Android projesi oluşturma **FlashCardPager**. Ardından, NuGet Paket Yöneticisi'ni başlatın (NuGet paketlerini yükleme hakkında daha fazla bilgi için bkz: [izlenecek yol: de dahil olmak üzere bir NuGet projenizdeki](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Bulma ve yükleme **Xamarin.Android.Support.v4** paketini açıklandığı gibi [Viewpager ve görünümleri](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 



## <a name="add-an-example-data-source"></a>Bir örnek veri kaynağı ekleme

İçinde **FlashCardPager**, veri kaynağının bir deste flash tarafından temsil edilen olduğundan `FlashCardDeck` sınıfı; bu veri kaynakları kaynak `ViewPager` öğesi içeriğe sahip. `FlashCardDeck` Matematik sorunları ve yanıtları hazır bir koleksiyonunu içerir. `FlashCardDeck` Oluşturucu bağımsız değişken gerektirir: 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

Flash kartları koleksiyonunu `FlashCardDeck` her flash kartı bir dizin oluşturucu tarafından erişilebilecek şekilde düzenlenmiştir. Örneğin, aşağıdaki kod satırını deste dördüncü flash kartı sorunu getirir: 

```csharp
string problem = flashCardDeck[3].Problem;
```

Bu kod satırı, önceki sorun karşılık gelen yanıtı alır:

```csharp
string answer = flashCardDeck[3].Answer;
```

Çünkü uygulama ayrıntılarını `FlashCardDeck` anlamak için ilgili olmayan `ViewPager`, `FlashCardDeck` kodu buraya listelenmiyor.
Kaynak koduna `FlashCardDeck` şu adresten edinilebilir [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Bu kaynak dosyasını karşıdan yükleyin (veya kodu kopyalayıp yeni dosyaya yapıştırın **FlashCardDeck.cs** dosyası) ve projenize ekleyin.



## <a name="create-a-viewpager-layout"></a>ViewPager düzenini oluşturma

Açık **Resources/layout/Main.axml** ve içeriğini aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

Bu XML tanımlayan bir `ViewPager` tüm ekranı kaplar. Tam adı kullanmalıdır Not **android.support.v4.view.ViewPager** çünkü `ViewPager` destek Kitaplığı'nda paketlenmiştir. `ViewPager` Yalnızca kullanılabilir [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); Android SDK'ın kullanılabilir değil.


## <a name="set-up-viewpager"></a>ViewPager ayarlayın

Düzen **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimleri:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Değişiklik `MainActivity` böylece öğesinden türetilmiş sınıf bildiriminin `AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` türetilmiş`FragmentActivity` (yerine `Activity`) çünkü `FragmentActivity` parçaları destek yönetme bilir. Değiştir `OnCreate` aşağıdaki kod ile yöntemi: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

Bu kod şunları yapar:

1.  Görünümden ayarlar **Main.axml** düzeni kaynak.

2.  Bir başvuru alır `ViewPager` düzeninden.

3.  Yeni bir örneğini oluşturur `FlashCardDeck` veri kaynağı olarak.

Derleme ve bu kodu çalıştırmak, aşağıdaki ekran görüntüsüne benzer bir ekran görmeniz gerekir: 

[![Boş ViewPager ekran görüntüsü, FlashCardPager uygulamayla](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

Bu noktada, `ViewPager` olan boş kullanılan parçaları bulunmaması nedeniyle doldurmak `ViewPager`, ve verilerde bulunan bu parçasının oluşturmak için bir bağdaştırıcı bulunmaması **FlashCardDeck**. 

Aşağıdaki bölümlerde bir `FlashCardFragment` her flash kartı işlevselliğini uygulamak için oluşturmaktır ve `FragmentPagerAdapter` bağlanmak için oluşturulan `ViewPager` kısmında verilerinden oluşturulan parçaları için `FlashCardDeck`. 



## <a name="create-the-fragment"></a>Parça oluşturun

Her flash kartı adlı bir kullanıcı Arabirimi parça tarafından yönetilecek `FlashCardFragment`. `FlashCardFragment`kişinin görünümü ile tek bir flash kartı yer alan bilgileri görüntüler. Her örneği `FlashCardFragment` tarafından barındırılan `ViewPager`. 
`FlashCardFragment`kişinin görünümü oluşur bir `TextView` flash kartı sorun metin görüntüler. Bu görünüm kullanan olay işleyicisi uygulayacak bir `Toast` kullanıcı flash kartı soru dokunur yanıt görüntülemek için. 



### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment düzenini oluşturma

Önce `FlashCardFragment` olabilir uygulanan düzenini tanımlanmalıdır. Bu düzen tek parça parça kapsayıcı düzeni ' dir. Yeni bir Android düzene eklediğinizde **kaynakları/düzeni** adlı **flashcard_layout.axml**. Açık **Resources/layout/flashcard_layout.axml** ve içeriğini aşağıdaki kodla değiştirin: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

Bu düzen tek flash kartı parçasını tanımlar; her parça oluşan bir `TextView` büyük (100sp) yazı tipi kullanarak bir matematik sorun görüntüler. Bu metin flash kart üzerinde dikey ve yatay olarak ortalanır. 



### <a name="create-the-initial-flashcardfragment-class"></a>İlk FlashCardFragment sınıfı oluşturma

Adlı yeni bir dosya ekleme **FlashCardFragment.cs** ve içeriğini aşağıdaki kodla değiştirin:

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

Bu kod gerekli yerleştirir `Fragment` flash kartı görüntülemek için kullanılan tanımı. Unutmayın `FlashCardFragment` destek kitaplığı sürümünden türetilmiş `Fragment` tanımlanan `Android.Support.V4.App.Fragment`. Oluşturucusu boştur böylece `newInstance` Üreteç yöntemi yeni bir oluşturmak için kullanılan `FlashCardFragment` yerine bir oluşturucu. 

`OnCreateView` Yaşam döngüsü yöntemi oluşturur ve yapılandırır `TextView`. Parça 's düzenini Şişir `TextView` ve inflated döndürür `TextView` çağırana. `LayoutInflater` ve `ViewGroup` geçirilen `OnCreateView` böylece düzeni Şişir. `savedInstanceState` Paket verilerini içerir, `OnCreateView` yeniden oluşturmak için kullandığı `TextView` kaydedilmiş durumu. 

Parça ait görünüm açıkça çağrısıyla şişirileceğini `inflater.Inflate`. `container` Değişkendir görünümün üst ve `false` bayrak söyler görünümün üst öğeye inflated görünümü ekleme engellemeye inflater (Bu zaman eklenir `ViewPager` çağrısı kullanıcının bağdaştırıcının `GetItem` yönteminde daha sonra bu izlenecek yol). 



### <a name="add-state-code-to-flashcardfragment"></a>Durum kodu için FlashCardFragment ekleyin

Bir etkinlik gibi bir parçası olan bir `Bundle` kaydedin ve durumunu almak için kullanır. İçinde **FlashCardPager**, bu `Bundle` soru kaydetmek ve ilişkili flash kart için metin yanıtlamak için kullanılır. İçinde **FlashCardFragment.cs**, aşağıdakileri ekleyin `Bundle` anahtarları üstüne `FlashCardFragment` sınıf tanımı: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Değiştirme `newInstance` şekilde oluşturur Üreteç yöntemi bir `Bundle` nesne ve geçirilen soru depolamak ve bu örneği sonra parça metinde yanıtlamak için yukarıdaki anahtarları kullanır: 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

Parça yaşam döngüsü yöntemi Değiştir `OnCreateView` geçilen paket bu bilgileri almak ve soru metne yüklemek için `TextBox`: 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

`answer` Değişkeni burada kullanılmaz, ancak daha sonra olay işleyici kodu bu dosyaya eklendiğinde kullanılacak. 


## <a name="create-the-adapter"></a>Bağdaştırıcısı oluştur

`ViewPager` arasında bulunur bağdaştırıcısı denetleyicisi nesnesini kullanan `ViewPager` ve veri kaynağı (ViewPager çizimde bkz [bağdaştırıcısı](~/android/user-interface/controls/view-pager/index.md#adapter) makale). Bu verilere erişmek için `ViewPager` türetilen özel bir bağdaştırıcı sağlamanızı ister `PagerAdapter`. Bu örnek parçaları kullandığından, onu kullanan bir `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` türetildiği `PagerAdapter`. 
`FragmentPagerAdapter` Her bir sayfa olarak temsil eden bir `Fragment` , kalıcı olarak tutulur için parça Yöneticisi'nde kullanıcı sayfasına geri dönmek sürece. Sayfaları aracılığıyla kullanıcı swipes olarak `ViewPager`, `FragmentPagerAdapter` veri kaynağından bilgileri ayıklar ve oluşturmak için kullandığı `Fragment`s için `ViewPager` görüntülemek için. 

Ne zaman uygulamanız bir `FragmentPagerAdapter`, aşağıdaki geçersiz kılmanız gerekir:

-   **Count** &ndash; Görünüm (sayfaları) kullanılabilir sayısını döndürür özelliği salt okunur.

-   **GetItem** &ndash; belirtilen sayfa için görüntülenecek parça döndürür.

Adlı yeni bir dosya ekleme **FlashCardDeckAdapter.cs** ve içeriğini aşağıdaki kodla değiştirin:

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

Bu kod gerekli yerleştirir `FragmentPagerAdapter` uygulaması. Aşağıdaki bölümlerde bu yöntemlerin her biri çalışma kodu ile değiştirilir. Oluşturucusu amacı parça Yöneticisi geçirmektir `FlashCardDeckAdapter`ait temel sınıf oluşturucu. 



### <a name="implement-the-adapter-constructor"></a>Uygulama bağdaştırıcısı Oluşturucusu

Uygulamanın ne zaman başlatır `FlashCardDeckAdapter`, parça Yöneticisi'ni ve başlatılan bir başvuru kaynakları `FlashCardDeck`. En üst kısmına aşağıdaki üye değişkeni ekleme `FlashCardDeckAdapter` sınıfını **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Aşağıdaki kod satırını ekleyin `FlashCardDeckAdapter` Oluşturucusu: 

```csharp
this.flashCardDeck = flashCards;
```

Bu satır kod depolarının `FlashCardDeck` örneğine `FlashCardDeckAdapter` kullanır. 



### <a name="implement-count"></a>Uygulama sayısı

`Count` Uygulaması oldukça basittir: flash kartı deste flash kartlarının sayısını döndürür. Değiştir `Count` aşağıdaki kod ile: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards` Özelliği `FlashCardDeck` veri kümesinde flash kartları (parça sayısı) sayısını döndürür. 



### <a name="implement-getitem"></a>Implement GetItem

`GetItem` Yöntemi belirtilen konumu ile ilişkilendirilmiş parçanın döndürür. Zaman `GetItem` döndürdüğü adlı flash kartı deste bir konum için bir `FlashCardFragment` o konumdan flash kartı sorun görüntüleyecek şekilde yapılandırılmış. Değiştir `GetItem` aşağıdaki kod ile yöntemi: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Bu kod şunları yapar:

1.  Matematik sorunu dizesini arayan `FlashCardDeck` deste belirtilen konum için. 

2.  Yanıt dizesini arayan `FlashCardDeck` deste belirtilen konum için. 

3.  Çağrıları `FlashCardFragment` Üreteç yöntemi `newInstance`, flash kartı sorun ve yanıt dizelerinde geçen. 

4.  Oluşturur ve yeni bir flash kartı döndürür `Fragment` o konumdan soru ve yanıt metni içerir. 

Zaman `ViewPager` işler `Fragment` adresindeki `position`, görüntülediği `TextBox` konumunda bulunan matematik sorunu dizeyi içeren `position` flash kartı deste içinde. 



## <a name="add-the-adapter-to-the-viewpager"></a>Bağdaştırıcı için ViewPager Ekle

Şimdi `FlashCardDeckAdapter` olan uygulanan eklemek için zamanı `ViewPager`. İçinde **MainActivity.cs**, sonuna aşağıdaki kod satırını ekleyin `OnCreate` yöntemi:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Bu kod başlatır `FlashCardDeckAdapter`, içinde geçen `SupportFragmentManager` ilk bağımsız değişkeninde. ( `SupportFragmentManager` FragmentActivity özelliği bir başvuru almak için kullanılan `FragmentManager` &ndash; hakkında daha fazla bilgi için `FragmentManager`, bkz: [yönetme parçaları](~/android/platform/fragments/managing-fragments.md).) 

Çekirdek uygulamasını tamamlanmıştır &ndash; oluşturmak ve uygulamayı çalıştırın.
Ekranda soldaki sonraki ekran görüntüsünde gösterildiği gibi görünen ilk görüntüsü flash kartı deste görmeniz gerekir. Ardından geçirme sağ flash kartı deste geri taşımak için daha fazla flash kart görmek için sola geçirme:

[![Çağrı cihazı göstergeleri olmadan FlashCardPager uygulama örnek ekran görüntüleri](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Çağrı cihazı göstergesi Ekle

Bu en az `ViewPager` uygulama deste her flash kartı görüntüler, ancak kullanıcının deste içinde olduğu konusunda herhangi bir gösterge sağlar. Sonraki adım eklemektir bir `PagerTabStrip`. `PagerTabStrip` Kullanıcı sayısı görüntülenir ve önceki ve sonraki flash kartların ipucu görüntüleyerek Gezinme bağlamı sağlar hangi sorunu konusunda sizi bilgilendirir. 

Açık **Resources/layout/Main.axml** ve ekleme bir `PagerTabStrip` Düzen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

  <android.support.v4.view.PagerTabStrip
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:layout_gravity="top"
      android:paddingBottom="10dp"
      android:paddingTop="10dp"
      android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

Derleme ve uygulamayı çalıştırma, boş görmelisiniz `PagerTabStrip` her flash kartı üst kısmında görüntülenir: 

[![Metin olmadan PagerTabStrip Closeup](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>Bir başlığı görüntüleme

Her bir sayfa sekmesini bir başlık eklemek için uygulama `GetPageTitleFormatted` bağdaştırıcısında yöntemi. `ViewPager` çağrıları `GetPageTitleFormatted` (uygulanırsa) belirli bir konumda sayfasını açıklar başlık dize elde edilir. Aşağıdaki yöntemi ekleyin `FlashCardDeckAdapter` sınıfını **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Bu kod flash kartı deste konumda bir sorun sayıya dönüştürür. Sonuç dizesini bir Java dönüştürülür `String` için döndürülen `ViewPager`. Bu yeni yöntemiyle uygulamayı çalıştırdığınızda, her bir sayfa sorun sayısında görüntüler `PagerTabStrip`: 

[![Her bir sayfada görüntülenen sorun numarasıyla FlashCardPager ekran görüntüleri](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

İleri ve geri her flash kartı üstünde görüntülenen flash kartı deste sorun numarasında görmek için doğru çekin. 



## <a name="handle-user-input"></a>Kullanıcı girişini işleme

**FlashCardPager** flash kartları parça tabanlı bir dizi sunan bir `ViewPager`, ancak her sorun için yanıt ortaya çıkarmak için bir yol henüz yok. Bu bölümde, olay işleyici eklenen `FlashCardFragment` kullanıcı flash kartı sorun metni dokunur yanıt görüntülemek için. 

Açık **FlashCardFragment.cs** ve sonuna aşağıdaki kodu ekleyin `OnCreateView` görünümü çağırana yalnızca döndürülmeden önce yöntemi: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Bu `Click` olay işleyicisi yanıt kullanıcı dokunur olduğunda görüntülenen bir bildirim görüntüler `TextBox`. `answer` Değişkeni başlatılmış önceki durum bilgilerini geçirilmedi paket okuma zaman `OnCreateView`. Derleme ve uygulamayı çalıştırın, ardından yanıt görmek için her flash kart üzerindeki sorun metin dokunun: 

[![Ekran görüntüleri, FlashCardPager uygulama matematik sorunu dokunduğunuz olduğunda bildirimleri](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

**FlashCardPager** bu kılavuzda sunulan kullanan bir `MainActivity` türetilen `FragmentActivity`, ancak aynı zamanda türetilemeyeceğini `MainActivity` gelen `AppCompatActivity` (de destek sağlayan parçaları yönetmek için). Görüntülemek için bir `AppCompatActivity` örnek, bkz: [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) örnek galerisinde. 



## <a name="summary"></a>Özet

Bu kılavuz temel yapı konusunda adım adım örnek sağlanan `ViewPager`-tabanlı uygulama kullanarak `Fragment`s. Flash kartı sorular ve yanıtlar, içeren bir örnek veri kaynağı sunulan bir `ViewPager` flash kartları görüntülemek için Düzen ve `FragmentPagerAdapter` bağlayan bir alt `ViewPager` veri kaynağı. Flash kartları gidin kullanıcının yardımcı olmak için yönergeleri dahil nasıl ekleneceği açıklanmaktadır bir `PagerTabStrip` her sayfanın en üstünde sorun sayısını görüntülemek için. Son olarak, kullanıcı bir flash kartı sorunu dokunur yanıt görüntülemek için olay kod işleme eklendi. 



## <a name="related-links"></a>İlgili bağlantılar

- [FlashCardPager (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
