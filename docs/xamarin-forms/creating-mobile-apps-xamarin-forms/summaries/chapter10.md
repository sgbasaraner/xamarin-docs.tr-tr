---
title: "Bölüm 10 özeti. XAML işaretleme uzantıları"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3e9f630fbfc9f7a1d6346b6dd8308504a6806e1a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Bölüm 10 özeti. XAML işaretleme uzantıları

Normalde, XAML ayrıştırıcı temel .NET veri türleri için standart dönüşümler göre özelliğinin türü için bir öznitelik değeri olarak ayarlamak herhangi bir dize dönüştürür veya [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) türevi iliştirilmiş özellik veya türü ile bir [`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

Ancak bazen, örneğin, bir öğeyi bir sözlük veya bir statik özellik veya alan değerinin farklı kaynak ya da bir hesaplama tür bir öznitelik ayarlamak uygun değil.

Bu iş, bir *XAML biçimlendirme uzantısı*. XAML biçimlendirme uzantıları adı rağmen olan *değil* XML uzantısı. XAML her zaman yasal XML'dir.

## <a name="the-code-infrastructure"></a>Kod Altyapısı

XAML biçimlendirme uzantısı uygulayan bir sınıftır [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) arabirimi. Böyle bir sınıfın genellikle sözcüğü bulunan `Extension` adının sonunda, ancak genellikle XAML'de soneki görünür.

Aşağıdaki XAML biçimlendirme uzantıları XAML tüm uygulamaları tarafından desteklenir:

- `x:Static` tarafından desteklenen [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` tarafından desteklenen [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` tarafından desteklenen [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` tarafından desteklenen [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` tarafından desteklenen [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

Bu dört XAML biçimlendirme uzantıları XAML Xamarin.Forms dahil olmak üzere, birçok uygulamaları tarafından desteklenir:

- `StaticResource` tarafından desteklenen [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` tarafından desteklenen [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` tarafından desteklenen [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) &mdash;ele [Bölüm 16. Veri bağlama](#chapter16)
- `TemplateBinding` tarafından desteklenen [ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) &mdash;Defteri'nde kapsanmayan

Xamarin.Forms birlikte ek bir XAML biçimlendirme uzantısı dahil [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;Defteri'nde kapsanmayan

## <a name="accessing-static-members"></a>Statik üyeler erişme

Kullanım [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) öğesi bir öznitelik ortak bir statik özellik, alan veya sabit listesi üye değerine ayarlayın. Ayarlama [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) statik üye özelliği. Belirtmek daha kolay `x:Static` ve süslü ayraçlar içindeki üye adı. Adını `Member` özelliği dahil edildiği gerekmez öğenin kendisini yalnızca. Bu ortak sözdizimi gösterilen [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) örnek. Statik alanları tanımlanan [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) sınıfı. Bu teknik bir program aracılığıyla kullanılan sabitler kurmanızı sağlar.

Bir ek XML ad alanı bildirimi ile ortak statik özellikler, alanlar veya .NET framework, tanımlanan numaralandırma üyeleri şekilde başvurabileceğiniz [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) örnek .

## <a name="resource-dictionaries"></a>Kaynak sözlükleri

`VisualElement` Sınıfı tanımlayan adlı bir özellik [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) türünde bir nesne için ayarlanan [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). XAML içinde bu sözlükteki maddeleri depolamak ve bunlarla tanımlamak `x:Key` özniteliği. Kaynak sözlükte depolanan öğeler öğesinin tüm başvurularını arasında paylaşılır.

### <a name="staticresource-for-most-purposes"></a>Çoğu amaç için StaticResource

Çoğu durumda, kullanacağınız [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) bir öğe tarafından gösterildiği gibi kaynak sözlüğünden başvurmak için işaretleme uzantısı [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) örnek . Kullanabileceğiniz bir `StaticResourceExtension` öğesi veya `StaticResource` süslü ayraçlar içinde:

[![Kaynak Paylaşımı Üçlü ekran](images/ch10fg03-small.png "kaynak paylaşımı")](images/ch10fg03-large.png#lightbox "kaynak paylaşma")

Aynı şey `x:Static` biçimlendirme uzantısı ve `StaticResource` biçimlendirme uzantısı.

### <a name="a-tree-of-dictionaries"></a>Sözlük ağacı

XAML ile karşılaştığında bir `StaticResource`, eşleşen bir anahtarı için görsel ağaç aramaya başlar ve ardından görünür `ResourceDictionary` uygulamanın içinde `App` sınıfı. Bu öğeleri görsel ağaçta daha yüksek bir kaynak sözlüğü geçersiz kılmak için görsel ağaç daha derin bir kaynak sözlüğü sağlar. Bu, gösterilmiştir [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) örnek.

### <a name="dynamicresource-for-special-purposes"></a>Özel amaçlı DynamicResource

`StaticResource` Biçimlendirme uzantısı neden olan bir görsel ağaç sırasında yapılandırıldığında sözlükten alınması için bir öğe `InitializeComponent` çağırın. Alternatif `StaticResource` olan [ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/), sözlük anahtarı bağlantı korur ve öğe anahtarı değişiklikleri tarafından başvurulan hedef güncelleştirir.

Arasındaki farkı `StaticResource` ve `DynamicResource` örneklerde gösterildiği [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) örnek.

Bir özellik kümesi `DynamicResource` bağlanabilirse özelliği tarafından anlatıldığı gibi yedeklenmelidir [Bölüm 11, bağlanabilirse altyapı](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Daha az kullanılan biçimlendirme uzantıları

Kullanım [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) bir özelliği ayarlamak için işaretleme uzantısı `null`.

Kullanım [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) bir özellik için bir .NET ayarlamak için işaretleme uzantısı `Type` nesnesi.

Kullanım [ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) bir dizi tanımlamak için. Dizi üyeleri ayarlayarak türünü [`Type`] özelliği için bir `x:Type` biçimlendirme uzantısı.

## <a name="a-custom-markup-extension"></a>Özel biçimlendirme uzantısı

Kendi XAML işaretleme uzantılarına uygulayan bir sınıf yazarak oluşturabileceğiniz [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) ile arabirim bir [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/) yöntemi.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Sınıfı bu gereksinimi karşılar. Türünde bir değer oluşturur `Color` adlı özelliklerinin değerlerine göre `H`, `S`, `L`, ve `A`. Bu sınıf adlı bir Xamarin.Forms Kitaplığı'nda ilk öğedir [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) yerleşik ve bu kitap boyunca kullanılan.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) örneği, bu kitaplık başvurusu ve özel biçimlendirme uzantısı nasıl kullanılacağını gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 10 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Bölüm 10 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
