---
title: İzlenecek yol - 2. Bölüm parça
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 58291388d375a4fd9273c8e0cd46db3799966766
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798898"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>İzlenecek yol parça &ndash; yatay

[Parçaları izlenecek &ndash; Kısım 1](./walkthrough.md) nasıl oluşturulacağı ve bir telefon küçük ekranlarda hedefleyen bir Android uygulaması parçaları kullanın gösterilmektedir. Tablet fazladan yatay boşluk yararlanmak için uygulamayı değiştirmek için bu kılavuzda bir sonraki adım olan &ndash; yürütür listesini her zaman olacak bir etkinlik olacaktır ( `TitlesFragment`) ve `PlayQuoteFragment` dinamik olarak eklenir kullanıcı tarafından yapılan bir seçim yanıta etkinliğinde için:

[![Tablet üzerinde çalışan uygulama](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Yatay modunda çalışan telefonları da bu geliştirme yararlı olacaktır:

[![Android telefonla yatay modunda çalışan uygulama](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Yatay yönlendirme işlemek için uygulama güncelleştiriliyor

Aşağıdaki değişiklikler yapılmıştır içinde iş bağlı oluşturacaksınız [parçaları izlenecek - telefon](./walkthrough.md)

1. Her ikisi de görüntülemek için alternatif bir düzen oluşturmak `TitlesFragment` ve `PlayQuoteFragment`.
1. Güncelleştirme `TitlesFragment` aygıt her iki parçada da aynı anda görüntülüyor olmadığını algılar ve davranış uygun şekilde değiştirin.
1. Güncelleştirme `PlayQuoteActivity` cihaz yatay modunda olduğunda kapatın.

## <a name="1-create-an-alternate-layout"></a>1. Alternatif bir düzen oluşturur

Android cihazında ana etkinlik oluşturulduğunda, Android cihaz yönünü yüklemek için hangi düzenin dayalı karar verir. Varsayılan olarak, Android sağlayacak **Resources/layout/activity_main.axml** Düzen dosyası. Yatay modunda cihazlar için Android sağlayacak **Resources/layout-land/activity_main.axml** Düzen dosyası. Kılavuzu [Android kaynakları](/xamarin/android/app-fundamentals/resources-in-android) Android için uygulama yüklemek için hangi kaynak dosyaları nasıl karar hakkında daha fazla ayrıntı içerir.

Alternatif bir düzen'i hedefleyen oluşturmak **yatay** açıklanan adımları izleyerek yönlendirmesini [alternatif düzenleri](/xamarin/android/user-interface/android-designer/alternative-layout-views) Kılavuzu. Bu yeni bir düzen kaynak dosyası projeye eklemelisiniz **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Çözüm Gezgini'nde alternatif düzeni](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Çözüm panelinde alternatif düzeni](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Alternatif düzenini oluşturduktan sonra dosyanın kaynağına Düzenle **Resources/layout-land/activity_main.axml** böylece bu XML eşleşir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

Etkinlik kök görünümünü kaynak kimliği verilir `two_fragments_layout` ve iki alt görünümlere sahip bir `fragment` ve `FrameLayout`. Sırada `fragment` statik olarak yüklenen `FrameLayout` "tarafından çalışma zamanında değiştirilecek bir yer tutucu" olarak görev yapar `PlayQuoteFragment`. Yeni bir Kullan seçildiğinde, her zaman `TitlesFragment`, `playquote_container` yeni bir örneğini ile güncelleştirileceği `PlayQuoteFragment`.

Her alt görünüm kendi üst tam yüksekliğini dolduracaktır. Her alt görünüm genişliğini tarafından denetlenen `android:layout_weight` ve `android:layout_width` öznitelikleri. Bu örnekte, her alt görünüm genişliği % 50'si kaplar üst tarafından sağlayın. Bkz: [LinearLayout Google belgeyi](https://developer.android.com/guide/topics/ui/layout/linear.html) hakkındaki ayrıntılar için _düzeni ağırlık_.

## <a name="2-changes-to-titlesfragment"></a>2. TitlesFragment yapılan değişiklikler

Alternatif düzeni oluşturulduktan sonra güncelleştirmek için gereken `TitlesFragment`. Ne zaman uygulama görüntülemektedir iki parçasının bir faaliyete sonra `TitlesFragment` yükleyeceğini `PlayQuoteFragment` üst etkinliği. Aksi takdirde, `TitlesFragment` belirmelidir `PlayQuoteActivity` hangi ana bilgisayarın `PlayQuoteFragment`. Bir Boole bayrağı yardımcı olacak `TitlesFragment` kullanması gereken hangi davranışı belirler. Bu bayrak, başlatılan `OnActivityCreated` yöntemi.

İlk olarak, en üstünde bir örnek değişkeni ekleyin `TitlesFragment` sınıfı:

```csharp
bool showingTwoFragments;
```

Ardından, aşağıdaki kod parçacığını ekleyin `OnActivityCreated` değişkeni başlatmak için: 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

Cihaz yatay modunda çalışıyorsa, sonra `FrameLayout` kaynak kimliği ile `playquote_container` ekranda görünür olacak şekilde `showingTwoFragments` üzere başlatılacaktır `true`. Cihaz dikey modda sonra çalışıp çalışmadığını `playquote_container` ekranında, bu nedenle olmaz `showingTwoFragments` olacaktır `false`.

`ShowPlayQuote` Yöntemi tırnak işareti biçimini değiştirmeniz gerekir &ndash; parçası veya yeni bir etkinlik başlatın.  Güncelleştirme `ShowPlayQuote` iki parça gösteren bir parça yükleme yöntemini bir etkinlik aksi belirmelidir:

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Kullanıcı şu anda görüntülenen bir farklı çalıştır seçtiyseniz `PlayQuoteFragment`, sonra yeni `PlayQuoteFragment` oluşturulur ve içeriğini yerini alacak `playquote_container` bağlamında bir `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Tam kod TitlesFragment için

Önceki tüm değişiklikleri tamamladıktan sonra `TitlesFragment`, tam sınıfı bu kodu eşleşmesi gerekir:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }


        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. PlayQuoteActivity yapılan değişiklikler

İlişkin halletmeniz için bir son ayrıntı yok: `PlayQuoteActivity` cihaz yatay modunda olduğunda gerekli değildir. Cihaz yatay modunda değilse `PlayQuoteActivity` görünür olmamalıdır. Güncelleştirme `OnCreate` yöntemi `PlayQuoteActivity` böylece kendisini kapanacak. Bu kodu son sürümüdür `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

Bu değişikliği cihaz yönlendirmesini denetle ekler. Yatay modda ise, `PlayQuoteActivity` kendisini kapanacak.

## <a name="4-run-the-application"></a>4. Uygulamayı çalıştırın

Bu değişiklikler uygulama çalıştırmak, tamamlandıktan sonra modu (gerekirse) yatay aygıta döndürün ve ardından bir oyun seçin. Teklif yürütür listesi aynı ekranda görüntülenmesi gerekir:

[![Android telefonla yatay modunda çalışan uygulama](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android tablet üzerinde çalışan uygulama](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
