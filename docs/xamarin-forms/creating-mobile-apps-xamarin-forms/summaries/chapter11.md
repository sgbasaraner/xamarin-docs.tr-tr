---
title: Bölüm 11 özeti. Bağlanabilir altyapı
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 11 özeti. Bağlanabilir altyapı'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 9f3c077d7bae3557178236b81073afaf4892a272
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156567"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Bölüm 11 özeti. Bağlanabilir altyapı

Her C# Programcı C# kullanmaya alışkın olduğu *özellikleri*. Özellikleri içeren bir *ayarlamak* erişimci ve/veya bir *alma* erişimcisi. Bunlar genellikle adlandırılır *CLR Özellikleri* ortak dil çalışma zamanı.

Xamarin.Forms tanımlar adlı bir Gelişmiş özelliğinin tanımı bir *bağlanılabilir özellik* tarafından Kapsüllenen [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) sınıfı ve tarafından desteklenen [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)sınıfı. Bu sınıfların ilgili ancak oldukça farklı: `BindableProperty` özelliği sunmanın tanımlamak için kullanılır `BindableObject` benzer `object` bağlanabilir özellikler tanımlayan sınıflar için temel bir sınıf olması.

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms sınıf hiyerarşisi

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) örnek Xamarin.Forms sınıf hiyerarşisini görüntülemek ve oynadığı önemli rol göstermek için yansıtma kullanır `BindableObject` bu hiyerarşideki. `BindableObject` öğesinden türetilen `Object` ve üst sınıf [ `Element` ](xref:Xamarin.Forms.Element) içinden [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) türetilir. Üst sınıf budur [ `Page` ](xref:Xamarin.Forms.Page) ve [ `View` ](xref:Xamarin.Forms.View), üst sınıf için [ `Layout` ](xref:Xamarin.Forms.Layout):

[![Üç ekran paylaşımı sınıf hiyerarşisinin](images/ch11fg01-small.png "sınıf hiyerarşisi paylaşımı")](images/ch11fg01-large.png#lightbox "sınıf hiyerarşisi paylaşma")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Bir Özet BindableObject ve BindableProperty

Öğesinden türetilen sınıflarda `BindableObject` birçok CLR özelliği "tarafından bağlanabilir özellikler desteklenecek şekilde" söylenir. Örneğin, [ `Text` ](xref:Xamarin.Forms.Label.Text) özelliği `Label` sınıfı bir CLR özelliği, ancak `Label` sınıfı da tanımlar adlı bir ortak statik salt okunur alan [ `TextProperty` ](xref:Xamarin.Forms.Label.TextProperty) , tür `BindableProperty`.

Bir uygulama ayarlayın veya alın `Text` özelliği `Label` normalde veya uygulama ayarlayabilirsiniz `Text` çağırarak [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) yöntemi tarafından tanımlanan `BindableObject` ile bir `Label.TextProperty` bağımsız değişken. Benzer şekilde, bir uygulama değerini elde edebilirsiniz `Text` çağırarak [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) yöntemi ile bir `Label.TextProperty` bağımsız değişken. Bu tarafından gösterilmiştir [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) örnek.

Aslında, `Text` CLR özelliği, kullanılarak tamamen gerçekleştirilir `SetValue` ve `GetValue` tarafından tanımlanan yöntemleri `BindableObject` birlikte `Label.TextProperty` statik özellik.

`BindableObject` ve `BindableProperty` için destek sağlar:

- Özellikler varsayılan değerleri vererek
- Geçerli değerlerini depolama
- Özellik değerlerini doğrulamak için bir mekanizma sağlar
- Tek bir sınıftaki ilgili özellikler arasında tutarlılığı sürdürmeye
- Özellik değişikliklere yanıt verme
- Bir özellik değişmek üzere olduğunu veya değişti tetikleme bildirimleri
- Destekleyici veri bağlama
- Stilleri destekleme
- Dinamik kaynak destekleme

Her bir bağlanılabilir özellik değişiklikleri tarafından desteklenen bir özellik `BindableObject` ateşlenir bir [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) değişen özelliği tanımlayan olay. Özelliği aynı değere ayarlandığında bu olay harekete değil.

Bazı özellikler bağlanabilir özellikler ve bazı Xamarin.Forms sınıfları tarafından yedeklenmeyen &mdash; gibi `Span` &mdash; öğesinden türetilen değil `BindableObject`. Türetilen bir sınıf `BindableObject` bağlanabilir özellikler için destek `BindableObject` tanımlar `SetValue` ve `GetValue` yöntemleri.

Çünkü `Span` türünden türemez `BindableObject`, özelliklerini hiçbiri &mdash; gibi `Text` &mdash; bağlanabilir bir özelliği tarafından desteklenir. Bu nedenle, bir `DynamicResource` ayarını `Text` özelliği `Span` içinde özel durum harekete [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) önceki bölümdeki örnek. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) örnek kodu kullanarak dinamik kaynaklar ayarlama gösterir [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) yöntemi tarafından tanımlanan `Element`. İlk bağımsız değişken türünde bir nesnedir `BindableProperty`.

Benzer şekilde, [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) yöntemi tarafından tanımlanan `BindableObject` türünde bir ilk bağımsız değişken olan `BindableProperty`.

## <a name="defining-bindable-properties"></a>Bağlanabilir özellikler tanımlama

' Using static kendi bağlanabilir özellikler tanımlayabilirsiniz [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) türünde statik bir salt okunur alan yöntemini `BindableProperty`.

Bu gösterilmiştir [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. Bu sınıfın türetildiği `Label` ve yazı tipi boyutu noktaları belirtmenize olanak tanır. Örnekte gösterildiği [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) örnek.

Dört bağımsız değişkenleri `BindableProperty.Create` yöntemi gereklidir:

- `propertyName`: özelliğin (aynı CLR özellik adı olarak) metin adı
- `returnType`: CLR özelliğinin türü
- `declaringType`: bir özelliği bildirme sınıf türü
- `defaultValue`: özelliğin varsayılan değeri

Çünkü `defaultValue` türünde `object`, derleyici varsayılan değerin türü belirlemek mümkün olması gerekir. Örneğin, varsa `returnType` olduğu `double`, `defaultValue` 0.0 yerine yalnızca 0 gibi bir şekilde ayarlanması gerekir ya da çalışma zamanında bir özel durum türü uyuşmazlığı tetikler.

Ayrıca, bir bağlanılabilir özellik eklemek çok sık olarak:

- `propertyChanged`: bir statik yöntem, özellik değeri değiştiğinde çağırılır. İlk bağımsız değişken, özelliği değiştirildi sınıf örneğidir.

Diğer bağımsız değişkenleri `BindableProperty.Create` gibi yaygın değildir:

- `defaultBindingMode`: veri bağlama bağlantılı olarak kullanılan (açıklandığı gibi [ **Bölüm 16. Veri bağlama**](chapter16.md))
- `validateValue`: denetlemek için geçerli bir değer için bir geri çağırma
- `propertyChanging`: özellik ne zaman değişmek üzere belirtmek için bir geri çağırma
- `coerceValue`: başka bir değer kümesi değerine zorlamak için bir geri çağırma
- `defaultValueCreate`: (örneğin, bir koleksiyon) sınıfın örnekleri arasında paylaşılan bir varsayılan değer oluşturmak için bir geri çağırma

### <a name="the-read-only-bindable-property"></a>Salt okunur bağlanılabilir özellik

Bağlanılabilir özellik salt okunur olabilir. Salt okunur bağlanılabilir özellik oluşturabilmek statik yöntemi [ `BindableProperty.CreateReadOnly` ](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) özel statik salt okunur türünde bir alan tanımlamak için [ `BindablePropertyKey` ](xref:Xamarin.Forms.BindablePropertyKey).

Sonra CLR özelliği tanımlayın `set` olarak accesor `private` çağırmak için bir [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) aşırı yüklemesine `BindablePropertyKey` nesne. Bu sınıf dışında ayarlama özelliği engeller.

Bu gösterilmiştir [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) kullanılan sınıf [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) örnek.

## <a name="related-links"></a>İlgili bağlantılar

- [Tam metin Bölüm 11 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Bölüm 11 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [Bağlanabilir Özellikler](~/xamarin-forms/xaml/bindable-properties.md)
