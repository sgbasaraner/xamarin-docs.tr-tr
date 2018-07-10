---
title: ListView performans
description: ListView verileri görüntülemek için güçlü bir görünüm olsa da, bazı sınırlamalar vardır. Bu makalede, bir Xamarin.Forms ListView içinde bir uygulama ile muhteşem bir performans sağlamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 906fd60954b18064467e665295dba8bb75ed5a45
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935595"
---
# <a name="listview-performance"></a>ListView performans

Mobil uygulamaları yazarken performans önemlidir. Kullanıcılar, yumuşak kaydırma ve hızlı yükleme süreleri beklenir ulaştık. Kullanıcı beklentilerini karşılamak başarısız uygulama mağazasına derecelendirmelerini maliyet veya iş kolu satır uygulama söz konusu olduğunda, kuruluşunuzun zaman ve para maliyeti.

Ancak [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) güçlü bir görünüm verileri görüntülemek için bazı sınırlamaları vardır. Performans kaydırma, özellikle, derin şekilde iç içe görünüm Hiyerarşiler içeriyor veya çok fazla ölçü gerektiren belirli düzenlerini kullanmayı özel hücreleri kullanırken olumsuz etkilenebilir. Neyse ki, kötü performansa önlemek için kullanabileceğiniz teknikleri vardır.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Önbelleğe alma stratejisi

ListViews göre çok daha fazla veri görüntülemek için kullanılan genellikle uygun ekran. Örneğin bir music uygulaması düşünün. Şarkıya kitaplığı binlerce girişe sahip olabilir. Bir satır için her bir şarkıyı oluşturmak olacaktır, basit bir yaklaşım, kötü performansa sahip olması gerekir. Bu yaklaşım, belleği boşa harcar ve gezinme için kaydırma yavaşlatabilir. Başka bir yaklaşım oluşturma ve veri görünümüne kaydırılan gibi satır yok sağlamaktır. Bu sabit örnek oluşturma ve temizleme çok yavaş olabilir görünümü nesnelerin gerektirir.

Bellek ve yerel koruma [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) eşdeğerleri her platform için satırları yeniden kullanmaya yönelik yerleşik özellikler vardır. Yalnızca ekranda görünen hücreleri belleğe yüklenen ve **içeriği** mevcut hücrelere yüklenir. Bu uygulamanın süresi ve bellek kaydetme nesneleri binlerce örneklemek gerek engeller.

Xamarin.Forms verir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) hücre yeniden kullanımı üzerinden [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) aşağıdaki değerlere sahip sabit listesi:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Evrensel Windows Platformu (UWP) yoksayar [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) strateji, her zaman önbelleğe alma performansını geliştirmek için kullandığı için önbelleğe alma. Bu nedenle, varsayılan olarak davranır gibi [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) önbelleğe alma stratejisi uygulanır.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Belirtir önbelleğe alma stratejisi [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) her öğe için bir hücre listesinde oluşturur ve varsayılan `ListView` davranışı. Genel olarak aşağıdaki durumlarda kullanılmalıdır:

- Her bir hücre, çok sayıda bağlamaları olduğunda (20-30 +).
- Sık hücre şablonu değiştiğinde.
- Ne zaman test ortaya çıkarır, `RecycleElement` önbelleğe alma stratejisi azaltılmış yürütme hızını sonuçlanır.

Sonuçlarını bilmek önemlidir [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) stratejisi özel hücre ile çalışırken, önbelleğe alma. Herhangi bir hücreyi başlatma kodu her hücre oluşturulması için çalıştırmanız gerekir saniyede birden çok kez olabilir. Bu durumda, bir sayfa üzerinde ince Düzen teknikleri ister birden fazla iç içe geçmiş [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) örnekleri, Kurulum olmaları durumunda performans sorunlarını olur ve gerçek zamanlı ve kullanıcı sonuçlarda yok.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Belirtir önbelleğe alma stratejisi [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) liste hücreleri dönüştürerek bellek Ayak izi ve yürütme hızını en aza indirmek çalışacaktır. Bu mod her zaman bir performans geliştirmesi sunmaz ve tüm geliştirmeleri belirlemek için sınama gerçekleştirilmelidir. Ancak, bu genellikle tercih edilen seçenektir ve aşağıdaki durumlarda kullanılmalıdır:

- Her bir hücre küçük ve orta sayıda bağlama sahip olduğunda.
- Yaparken her hücrenin [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) tüm hücre verileri tanımlar.
- Ne zaman her hücre değişmeyen hücre şablonla, büyük ölçüde benzer.

Sanallaştırma sırasında hücre güncelleştirilmiş, bağlama bağlamı sahiptir ve uygulamanın bu modu kullanıyorsa, bu nedenle, bağlama bağlamı güncelleştirmeleri uygun şekilde işlendiğinden emin olmanız gerekir. Hücre ilgili tüm verileri bağlama bağlamdan gelmelidir veya tutarlılık hataları oluşabilir. Bu hücre verileri görüntülemek için veri bağlama kullanarak gerçekleştirilebilir. Alternatif olarak, hücre veri ayarlanması `OnBindingContextChanged` geçersiz kılmak, yerine aşağıdaki kod örneğinde gösterildiği gibi özel hücrenin Oluşturucusu:

```csharp
public class CustomCell : ViewCell
{
  Image image = null;

  public CustomCell ()
  {
    image = new Image();
    View = image;
  }

  protected override void OnBindingContextChanged ()
  {
    base.OnBindingContextChanged ();

    var item = BindingContext as ImageItem;
    if (item != null) {
      image.Source = item.ImageUrl;
    }
  }
}
```

Daha fazla bilgi için [bağlama bağlamı değişiklikleri](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

Özel oluşturucular hücreleri kullanırsanız, iOS ve Android üzerinde bunlar özellik değişikliği bildirimi doğru bir şekilde uygulandığından emin olmanız gerekir. Hücreleri yeniden yapılırken özellik değerlerine bağlama bağlamı, kullanılabilir bir hücre ile güncelleştirildiğinde değişir `PropertyChanged` gerçekleştirilen olaylarının. Daha fazla bilgi için [bir Viewcell'i özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>Bir DataTemplateSelector ile RecycleElement

Olduğunda bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kullanan bir [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) seçmek için bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) önbelleğe alma stratejisi önbelleğe almaz `DataTemplate`s. Bunun yerine, bir `DataTemplate` veri listedeki her öğe için seçilmiş.

> [!NOTE]
> [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Önbelleğe alma stratejisi, önkoşul olan Xamarin.Forms 2.4 içinde tanıtılan kullanırken bir [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) seçmesi istenir bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)her `DataTemplate` aynı döndürmelidir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) türü. Örneğin, belirtilen bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ile bir `DataTemplateSelector` , döndürebilir ya da `MyDataTemplateA` (burada `MyDataTemplateA` döndürür bir `ViewCell` türü `MyViewCellA`), veya `MyDataTemplateB` (burada `MyDataTemplateB`döndürür bir `ViewCell` türü `MyViewCellB`), `MyDataTemplateA` döndürmelidir döndürülen `MyViewCellA` veya bir özel durum oluşturulur.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Önbelleğe alma stratejisi yapılar [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) stratejisini kullanırken daha fazla sağlayarak önbelleğe alma bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) bir kullanır[ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) seçmek için bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`s listedeki bir öğe türü tarafından önbelleğe alınır. Bu nedenle, `DataTemplate`s seçili kez öğesi örneği başına bir kez yerine öğesi türü başına.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Önbelleğe alma stratejisi, önkoşul olan, `DataTemplate`tarafından döndürülen s [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) kullanmalısınız [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) alan oluşturucu bir `Type`.

### <a name="setting-the-caching-strategy"></a>Önbelleğe alma stratejisini belirleme

[ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) İle numaralandırma değeri belirtilen bir [ `ListView` ](xref:Xamarin.Forms.ListView) oluşturucu aşırı yüklemesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML içinde ayarlamak `CachingStrategy` aşağıdaki kodda gösterildiği gibi öznitelik:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Bu, önbelleğe alma stratejisi bağımsız değişken Oluşturucu C# ayarını aynı etkiye sahip; unutmayın hiçbir `CachingStrategy` özellikte `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Önbelleğe alma stratejisi sınıflandırılmış bir ListView ayarlama

Ayarı `CachingStrategy` XAML özniteliği bir alt sınıflanan üzerinde [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) olduğundan istenen davranışı oluşturmaz hiçbir `CachingStrategy` özellikte `ListView`. Ayrıca, varsa [XAMLC](~/xamarin-forms/xaml/xamlc.md) olan etkinse, aşağıdaki hata iletisini üretilecek: **hiçbir özelliği, bağlanılabilir özellik veya olay 'CachingStrategy için' bulundu**

Bir oluşturucu üzerinde alt sınıflanan belirtmek için bu sorunun çözümü olan [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kabul eden bir [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) parametresi ve temel sınıfına geçirir:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Ardından [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) numaralandırma değeri belirtilebilir XAML kullanarak `x:Arguments` söz dizimi:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>ListView performansını iyileştirme

Performansı geliştirmek için birçok teknik vardır bir `ListView`:

-  Bağlama `ItemsSource` özelliğini bir `IList<T>` koleksiyonu yerine bir `IEnumerable<T>` koleksiyonu, çünkü `IEnumerable<T>` koleksiyonları, rastgele erişim desteklemez.
-  Yerleşik hücreleri kullanın (gibi `TextCell`  /  `SwitchCell` ) yerine `ViewCell` zaman şunları yapabilirsiniz.
-  Daha az öğe kullanın. Örneğin tek bir kullanmayı `FormattedString` birden çok etiketle yerine etiketi.
-  Değiştirin `ListView` ile bir `TableView` homojen olmayan verileri – diğer bir deyişle, farklı türlerde verileri görüntülerken.
-  Sınırlandırmak [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) yöntemi. Aşırı kullanılmasına, performans düşmesine neden.
-  Android'de ayarı kaçının bir `ListView`ait satır ayırıcı görünürlük veya örneği oluşturulduktan sonra bir büyük bir performans düşüşüyle sonuçları gibi rengi.
-  Temel hücre düzenini değiştirmekten kaçının [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Bu düzen ve başlatma büyük maliyetler doğurur.
-  İç içe Düzen hiyerarşileri kaçının. Kullanım `AbsoluteLayout` veya `Grid` iç içe geçme azaltmaya yardımcı olmak için.
-  Belirli önlemek `LayoutOptions` dışında `Fill` (dolgu değer ucuz hesaplamak için).
-  Yerleştirmekten kaçının bir `ListView` içinde bir `ScrollView` aşağıdaki nedenlerle:
    - `ListView` Kendi kaydırma uygular.
    - `ListView` Üst öğe tarafından işlenecek şekilde tüm hareketleri almaz `ScrollView`.
    - `ListView` Özelleştirilmiş üstbilgi ve kayan altbilgi potansiyel olarak işlevselliğini teklifini listesi öğeleriyle sunabilir `ScrollView` kullanıldı. Daha fazla bilgi için [üstbilgiler ve altbilgiler](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Hücrelerde sunulan belirli, karmaşık bir tasarıma ihtiyacınız varsa, özel Oluşturucu göz önünde bulundurun.

`AbsoluteLayout` bir tek ölçü çağrı olmadan düzenleri gerçekleştirmek için olasılığına sahiptir. Bu, çok güçlü bir performans sağlar. Varsa `AbsoluteLayout` olamaz kullanıldığında, göz önünde bulundurun [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Kullanıyorsanız `RelativeLayout`, kısıtlamaları doğrudan geçirme API ifade kullanmaktan çok daha hızlı olacaktır. API ifadesi JIT kullanır, ve İos'ta ağaç yorumlanması daha yavaş olduğu olduğundan olmasıdır. API ifadesi, yalnızca ilk düzeni ve döndürme, ancak bu gerekli sayfa düzenleri için uygun olan `ListView`, sürekli kaydırma sırasında çalıştırılan burada performans gelmez.

İçin özel Oluşturucu Oluşturma bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) veya hücrelerinden Düzen hesaplamalar kaydırma performans üzerindeki etkisini azaltmak için bir yaklaşım. Daha fazla bilgi için [bir ListView özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) ve [bir Viewcell'i özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Özel oluşturucu Görünüm (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Özel oluşturucu Viewcell'i (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
