---
title: Bölüm 10 özeti. XAML biçimlendirme uzantıları
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 10 özeti. XAML biçimlendirme uzantıları'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 74f7e2846a9e8d8390a8322c57db0845718bbba7
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39157009"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Bölüm 10 özeti. XAML biçimlendirme uzantıları

Normalde, XAML ayrıştırıcı temel .NET veri türleri için standart dönüştürmeler göre özelliğinin türü için bir öznitelik değeri ayarlayın. herhangi bir dize dönüştürür veya [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) Türev iliştirilmiş özellik veya türü ile bir [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

Ancak, bir öznitelik bir öğe gibi bir sözlüğün veya bir statik özellik veya alan değerini başka bir kaynaktan veya bir hesaplama çeşit ayarlamak bazen kullanışlıdır.

Bu görevi, bir *XAML işaretleme uzantısı*. Adına rağmen XAML biçimlendirme uzantıları olan *değil* XML uzantısı. XAML her zaman yasal XML'dir.

## <a name="the-code-infrastructure"></a>Kod Altyapısı

XAML işaretleme uzantısı uygulayan sınıftır [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) arabirimi. Bu sınıf genellikle sözcüğü bulunan `Extension` adının sonunda, ancak genellikle XAML içinde soneki görüntülenir.

Aşağıdaki XAML biçimlendirme uzantıları tüm XAML uygulamaları tarafından desteklenir:

- `x:Static` tarafından desteklenen [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)
- `x:Reference` tarafından desteklenen [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)
- `x:Type` tarafından desteklenen [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)
- `x:Null` tarafından desteklenen [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)
- `x:Array` tarafından desteklenen [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)

Bu dört XAML biçimlendirme uzantıları, XAML, Xamarin.Forms dahil olmak üzere birçok uygulamaları tarafından desteklenir:

- `StaticResource` tarafından desteklenen [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)
- `DynamicResource` tarafından desteklenen [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)
- `Binding` tarafından desteklenen [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) &mdash;ele [Bölüm 16. Veri bağlama](#chapter16)
- `TemplateBinding` tarafından desteklenen [ `TemplateBindingExtension` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) &mdash;kitap kapsamında olmayan

Xamarin.Forms ile ek bir XAML işaretleme uzantısı dahil [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;Kitap kapsamında olmayan

## <a name="accessing-static-members"></a>Statik üyelere erişme

Kullanım [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) bir öznitelik değeri bir ortak statik özelliği, alan veya sabit listesi üye olarak ayarlamak için öğesi. Ayarlama [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) statik üye özelliği. Belirtmek daha kolay `x:Static` ve küme ayraçlarının içindeki üye adı. Adını `Member` özelliği dahil olması gerekmez üye bizzat yeterlidir. Bu sık kullanılan söz dizimi gösterilen [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) örnek. Statik alanlar tanımlanan [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) sınıfı. Bu teknik, bir program aracılığıyla kullanılan sabit oluşturmanıza olanak tanır.

Ek XML ad alanı bildirimi ile genel statik özellikler, alanlar veya .NET framework, tanımlanan numaralandırma üyelerini gösterildiği şekilde başvurabileceğiniz [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) örnek .

## <a name="resource-dictionaries"></a>Kaynak sözlükleri

`VisualElement` Sınıfı tanımlar adlı bir özellik [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) türünde bir nesne için ayarlayabileceğiniz [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). XAML içinde öğeleri bu sözlükte depolamak ve bunları tanımlamak `x:Key` özniteliği. Kaynak sözlüğünde depolanan öğelerin öğesinin tüm başvurularını arasında paylaşılır.

### <a name="staticresource-for-most-purposes"></a>StaticResource birçok amaç için

Çoğu durumda, kullanacağınız [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) tarafından gösterildiği gibi kaynak sözlüğünden bir öğe başvurusu için işaretleme uzantısı [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) örnek . Kullanabileceğiniz bir `StaticResourceExtension` öğesi veya `StaticResource` kaşlı ayraçlar içinde:

[![Kaynak paylaşımını üç ekran görüntüsü](images/ch10fg03-small.png "kaynak paylaşımını")](images/ch10fg03-large.png#lightbox "kaynak paylaşma")

Karıştırmayın `x:Static` işaretleme uzantısı ve `StaticResource` işaretleme uzantısı.

### <a name="a-tree-of-dictionaries"></a>Sözlükler ağacı

XAML ile karşılaştığında bir `StaticResource`, eşleşen bir anahtar için görsel ağacı aramaya başlar ve ardından görünen `ResourceDictionary` içinde uygulamanın `App` sınıfı. Bu öğeleri görsel ağacında daha yüksek bir kaynak sözlüğü geçersiz kılmak için görsel ağaç daha derin bir kaynak sözlüğü sağlar. Bu gösterilmiştir [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) örnek.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource özel amaçlı

`StaticResource` İşaretleme uzantısı neden olur, öğeyi görsel ağacı sırasında yapılandırıldığında sözlükten alınacak `InitializeComponent` çağırın. Alternatif `StaticResource` olduğu [ `DynamicResource` ](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), sözlük anahtarı bağlantısını korur ve hedef öğenin anahtarı değişiklikleri tarafından başvurulduğunda güncelleştirir.

Arasındaki fark `StaticResource` ve `DynamicResource` gösterilmiştir [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) örnek.

Bir özellik kümesi `DynamicResource` tarafından bir bağlanılabilir özellik anlatıldığı gibi yedeklenmelidir [Bölüm 11, bağlanabilir altyapı](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Daha az kullanılan biçimlendirme uzantıları

Kullanım [ `x:Null` ](xref:Xamarin.Forms.Xaml.NullExtension) bir özellik ayarlanacak işaretleme uzantısı `null`.

Kullanım [ `x:Type` ](xref:Xamarin.Forms.Xaml.TypeExtension) özellik için bir .NET ayarlamak için işaretleme uzantısı `Type` nesne.

Kullanım [ `x:Array` ](xref:Xamarin.Forms.Xaml.ArrayExtension) dizi tanımlamak için. Dizi üyeleri türünü belirtmek [`Type`] özelliğini bir `x:Type` işaretleme uzantısı.

## <a name="a-custom-markup-extension"></a>Özel biçimlendirme uzantısı

Uygulayan bir sınıf yazarak kendi XAML biçimlendirme uzantıları oluşturabilirsiniz [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) ile arabirim bir [ `ProvideValue` ](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) yöntemi.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Sınıfı bu gereksinimini karşılar. Türünde bir değer oluşturur `Color` adlı özelliklerinin değerlerine göre `H`, `S`, `L`, ve `A`. Bu sınıf adlı bir Xamarin.Forms Kitaplığı'nda ilk öğedir [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) yerleşik ve bu kitap boyunca kullanılır.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) örnek nasıl özel biçimlendirme uzantısı bu kitaplık başvurusu ve nasıl kullanılacağını gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [Tam metin Bölüm 10 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Bölüm 10 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/markup-extensions/index.md)
