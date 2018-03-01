---
title: "ListView performansı"
description: "ListView tabanlı uygulamanız ile harika performans emin olun."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 2acaef5fd42b867e88fb9b81d401ea752480124a
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="listview-performance"></a>ListView performansı

Mobil uygulamaları yazarken, performans önemlidir. Kullanıcılar, yumuşak kaydırma ve hızlı yükleme süreleri beklenir geldi. Kullanıcılarınızın beklentilerini karşılamak başarısız olan, uygulama Mağazası'nda derecelendirmeleri maliyet veya iş kolu satır uygulama söz konusu olduğunda, kuruluşunuzun zaman ve para maliyet.

Ancak [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) güçlü bir görünüm verileri görüntülemek için bazı sınırlamalar vardır. Performans kaydırma özellikle bunlar iç içe görünüm Hiyerarşiler içeriyor veya çok sayıda ölçüm gerektiren belirli düzenleri kullandığınızda özel hücreleri kullanırken olumsuz etkilenebilir. Neyse ki, düşük performans önlemek için kullanabileceğiniz teknikler vardır.

Aşağıdaki konular bu makalede ele alınmaktadır:

- **[Önbelleğe alma stratejisi](#cachingstrategy)**
- **[ListView performansı iyileştirme](#improving-performance)**

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Önbelleğe alma stratejisi

ListViews daha çok daha fazla veri görüntülemek için kullanılan çoğunlukla uyum ekranda. Müzik uygulama örneğin göz önünde bulundurun. Şarkıya kitaplığı binlerce girişe sahip olabilir. Her bir şarkıyı için bir satır oluşturmak olacaktır, basit bir yaklaşım performansının düşük olması gerekir. Bu yaklaşımı değerli bellek boşa harcar ve gezinme için kaydırma yavaşlatabilir. Başka bir oluşturma ve veri görünümüne kaydırılan gibi satır yok etmek için bir yaklaşımdır. Bu sabit örnek oluşturma ve temizleme çok yavaş olabilir görünüm nesnelerinin gerektirir.

Bellek, yerel tasarruf etmek için [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) eşdeğerleri her platform için satır yeniden kullanmak için yerleşik özellikleri. Yalnızca ekranda görünür hücreleri belleğe yüklenen ve **içerik** varolan hücrelere yüklenir. Bu uygulamanın süresi ve bellek kaydetme nesneleri, binlerce örneği gerek önler.

Xamarin.Forms verir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) hücre yeniden kullanımını aracılığıyla [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) şu değerleri numaralandırma:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> **Not**: Evrensel Windows Platformu (UWP) yoksayar [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) her zaman önbelleğe alma performansını artırmak için kullandığı için stratejisi, önbelleğe alma. Bu nedenle, varsayılan olarak davranır gibi [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) stratejisi önbelleğe alma uygulanır.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) Belirtir stratejisi önbelleğe alma [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) her öğe için bir hücre listede oluşturur ve varsayılan değer `ListView` davranışı. Genellikle aşağıdaki koşullarda kullanılmalıdır:

- Her bir hücre, çok sayıda bağlamaları olduğunda (20-30 +).
- Sık hücre şablonu değiştiğinde.
- Ne zaman sınama ortaya çıkarır, `RecycleElement` azaltılmış yürütme hızını stratejisi sonuçlarında önbelleğe alma.

Sonuçlarını bilmek önemlidir [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) stratejisi özel hücrelerle çalışırken önbelleğe alma. Herhangi bir hücre başlatma kod her hücre oluşturma için çalıştırması gerekecek birden çok kez saniye başına olabilir. Bu durumda, bir sayfa üzerinde ince düzeni teknikleri ister birden fazla iç içe geçmiş [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Kurulum olduklarında performans sorunları haline gelir ve gerçek zamanlı kullanıcı kayarken yok örnekleri.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Belirtir stratejisi önbelleğe alma [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) liste hücreleri dönüştürerek kendi bellek kaplama alanı ve yürütme hızını en aza indirmek deneyecek. Bu mod her zaman bir performans iyileştirme sağlamaz ve geliştirmeler belirlemek için test gerçekleştirilmelidir. Ancak, bu genellikle tercih edilen seçimdir ve aşağıdaki durumlarda kullanılmalıdır:

- Her bir hücre küçük ve orta sayıda bağlamaları olduğunda.
- Zaman her hücrenin [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) tüm hücre verileri tanımlar.
- Ne zaman her bir hücre hücre şablonu değişmeyen büyük ölçüde benzerdir.

Sanallaştırma sırasında hücre güncelleştirildi, bağlama bağlamı sahip olur ve bir uygulama bu modu kullanıyorsa, bu nedenle bağlama bağlamı güncelleştirmeleri uygun şekilde işlenir emin olmak gerekir. Hücre ilgili tüm verileri bağlama bağlamı gelmelidir veya tutarlılık hataları oluşabilir. Bu, hücre verileri görüntülemek için veri bağlama kullanılarak gerçekleştirilebilir. Alternatif olarak, hücre veri ayarlanması `OnBindingContextChanged` geçersiz kılmak, yerine aşağıdaki kod örneğinde gösterildiği gibi özel hücrenin Oluşturucusu:

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

Daha fazla bilgi için bkz: [bağlama bağlam değişiklikleri](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

Özel oluşturucu hücreleri kullanırsanız, iOS ve Android cihazlarda, bunlar özellik değişikliği bildirimi doğru bir şekilde uygulandığından emin olmalısınız. Hücreleri yeniden yapıldığında bağlama bağlamı, kullanılabilir bir hücre ile güncelleştirildiğinde özellik değerlerine değişir `PropertyChanged` gerçekleştirilen olaylarının. Daha fazla bilgi için bkz: [bir ViewCell özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>Bir DataTemplateSelector ile RecycleElement

Zaman bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kullanan bir [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) seçmek için bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) önbelleğe alma stratejisi önbelleğe almaz `DataTemplate`s. Bunun yerine, bir `DataTemplate` veri listesindeki her bir öğe için seçilir.

> [!NOTE]
> **Not**: [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) stratejisi önbelleğe alma, bir önkoşul olan Xamarin.Forms 2.4 içinde sunulan olduğunda bir [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) seçmek için sorulan bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , her `DataTemplate` aynı döndürmelidir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) türü. Örneğin, bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ile bir `DataTemplateSelector` , dönebilirsiniz ya da `MyDataTemplateA` (burada `MyDataTemplateA` döndürür bir `ViewCell` türü `MyViewCellA`), veya `MyDataTemplateB` (burada `MyDataTemplateB`döndüren bir `ViewCell` türü `MyViewCellB`), `MyDataTemplateA` döndürmelidir döndürülen `MyViewCellA` veya bir özel durum.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) Stratejisi önbelleğe alma derlemeler [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) olduğunda daha fazla sağlayarak stratejisi önbelleğe alma bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) bir kullanır[ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) seçmek için bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`s önbelleğe listedeki bir öğe türüne göre. Bu nedenle, `DataTemplate`s seçilmiş bir kez başına öğesi örneği başına bir kez yerine öğesi türü.

> [!NOTE]
> **Not**: [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) sahip bir önkoşul stratejisi önbelleğe alma, `DataTemplate`tarafından döndürülen s [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) kullanmalısınız [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) alan oluşturucu bir `Type`.

### <a name="setting-the-caching-strategy"></a>Önbelleğe alma stratejisi ayarlama

[ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) Numaralandırma değeri ile belirtilen bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Oluşturucusu aşırı yüklemesi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML'de ayarlamak `CachingStrategy` özniteliği aşağıdaki kodda gösterildiği gibi:

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

Bu, C# oluşturucuda önbelleğe alma stratejisi bağımsız değişkeni ayarını aynı etkiye sahiptir; unutmayın hiçbir `CachingStrategy` özelliği `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Önbelleğe alma stratejisi Altsınıflanmış ListView içinde ayarlama

Ayarı `CachingStrategy` XAML özniteliğinden bir altsınıflanmış üzerinde [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) olduğundan istenen davranışı oluşturmaz hiçbir `CachingStrategy` özelliği `ListView`. Ayrıca, varsa [XAMLC](~/xamarin-forms/xaml/xamlc.md) olan etkinse, aşağıdaki hata iletisini üretilecek: **özelliği, bağlanabilirse özelliği veya 'CachingStrategy için' bulunan olay yok**

Bu sorunun çözümü bir oluşturucu altsınıflanmış üzerinde belirtmektir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kabul eden bir [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) parametre ve temel sınıfına geçirir:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Ardından [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) numaralandırma değeri belirtilebilir XAML kullanarak `x:Arguments` sözdizimi:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>ListView performansı iyileştirme

Performansını artırmak için birçok tekniği vardır bir `ListView`:

-  Bağlama `ItemsSource` özelliğine bir `IList<T>` koleksiyon yerine bir `IEnumerable<T>` koleksiyonu, çünkü `IEnumerable<T>` koleksiyonları, rastgele erişim desteklemez.
-  Yerleşik hücreleri kullanın (gibi `TextCell`  /  `SwitchCell` ) yerine `ViewCell` zaman şunları yapabilirsiniz.
-  Daha az öğe kullanın. Örneğin tek bir kullanmayı `FormattedString` birden çok etiketleri yerine etiketi.
-  Değiştir `ListView` ile bir `TableView` homojen olmayan verileri – başka bir deyişle, farklı türlerde görüntülenirken.
-  Kullanımını sınırla [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) yöntemi. Aşırı kullanılmasına varsa performansının düşmesine neden.
-  Android, ayarı kaçının bir `ListView`ait satır ayırıcı görünürlük veya örneği oluşturulduktan sonra bir büyük performans cezası sonuçları gibi rengi.
-  Temel hücre düzeni değiştirmekten kaçının [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Bu, büyük düzeni ve başlatma maliyetler doğurur.
-  İç içe düzeni hiyerarşileri kullanmamaya özen gösterin. Kullanım `AbsoluteLayout` veya `Grid` iç içe geçme azaltmaya yardımcı olmak için.
-  Belirli kaçının `LayoutOptions` dışında `Fill` (Fill olduğundan cheapest hesaplamak için).
-  Yerleştirmez bir `ListView` içinde bir `ScrollView` aşağıdaki nedenlerle:
  - `ListView` Kendi kaydırma uygular.
  - `ListView` Üst tarafından işlenecek gibi tüm hareketleri almaz `ScrollView`.
  - `ListView` Özelleştirilmiş üstbilgi ve kayar altbilgi olası işlevselliği teklifini listenin öğelerle sunabilir `ScrollView` için kullanıldı. Daha fazla bilgi için bkz: [üstbilgiler ve altbilgiler](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Hücrelerde sunulan belirli ve karmaşık bir tasarım ihtiyacınız varsa özel Oluşturucu göz önünde bulundurun.

`AbsoluteLayout` tek bir ölçü çağrı olmadan düzenleri gerçekleştirmek için olasılığı vardır. Bu çok güçlü bir performans sağlar. Varsa `AbsoluteLayout` olamaz kullanıldığında, göz önünde bulundurun [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Kullanıyorsanız `RelativeLayout`, kısıtlamalar doğrudan geçirme API ifade kullanmaktan daha önemli ölçüde daha hızlı olacaktır. JIT API ifade kullanır, ve İos'ta ağaç yorumlanması daha yavaş olduğu olduğundan olmasıdır. Burada, yalnızca ilk düzen ve döndürme gerekli, ancak içinde API ifade sayfa düzenleri için uygun olan `ListView`, sürekli kaydırma sırasında çalıştırılan burada performans hurts.

İçin özel Oluşturucu Oluşturma bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) veya hücrelerinden performans kaydırma çubuğunda düzeni hesaplamalar etkisini azaltmak için bir yaklaşım. Daha fazla bilgi için bkz: [ListView özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) ve [bir ViewCell özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Özel oluşturucu görünümü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Özel oluşturucu ViewCell (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
