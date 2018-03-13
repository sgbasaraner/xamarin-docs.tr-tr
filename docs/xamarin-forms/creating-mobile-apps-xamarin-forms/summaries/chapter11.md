---
title: "Bölüm 11 özeti. Bağlanabilir altyapısı"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6e0f1abf04695dfb5348b631a9fbdbd2c81bc431
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Bölüm 11 özeti. Bağlanabilir altyapısı

Her C# Programcı C# ile alışkın olduğu *özellikleri*. Özellikleri içeren bir *ayarlamak* erişimcisi ve/veya bir *almak* erişimcisi. Bunlar genellikle adlandırılır *CLR Özellikleri* ortak dil çalışma zamanı için.

Xamarin.Forms adlı bir gelişmiş özellik tanımını tanımlayan bir *bağlanabilirse özelliği* yalıtılan [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) sınıfı ve tarafından desteklenen [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)sınıfı. Bu sınıfların ilgili ancak oldukça farklı: `BindableProperty` özelliğinin kendisini; tanımlamak için kullanılır `BindableObject` benzer `object` bağlanabilir özelliklerini tanımlayan sınıflar için temel sınıf olması.

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms sınıf hiyerarşisi

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) örnek Xamarin.Forms sınıf hiyerarşisi görüntülemek ve oynadığı önemli rol tanıtmak için yansıma kullanan `BindableObject` bu hiyerarşide. `BindableObject` türetilen `Object` ve üst sınıftır [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) içinden [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) türetilir. Üst sınıf budur [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) ve [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), üst sınıf olduğu [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![Üçlü ekran paylaşımı sınıf hiyerarşisinin](images/ch11fg01-small.png "sınıf hiyerarşisi paylaşımı")](images/ch11fg01-large.png#lightbox "sınıf hiyerarşisi paylaşımı")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Peek BindableObject ve BindableProperty

Öğesinden türetilen sınıflarda `BindableObject` birçok CLR özelliği "bağlanabilir özellikleri desteklenecek şekilde" denir. Örneğin, [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) özelliği `Label` sınıfı bir CLR özelliği, ancak `Label` sınıfı ayrıca adlı bir public static salt okunur alanı tanımlayan [ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/) , türü `BindableProperty`.

Bir uygulama ayarlayın veya alın `Text` özelliği `Label` normalde veya uygulama ayarlayabilirsiniz `Text` çağırarak [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi tarafından tanımlanan `BindableObject` ile bir `Label.TextProperty` bağımsız değişken. Benzer şekilde, bir uygulama değeri elde edebilirsiniz `Text` çağırarak özelliği [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) yöntemi, yeniden ile bir `Label.TextProperty` bağımsız değişkeni. Bu tarafından gösterilen [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) örnek.

Aslında, `Text` CLR özelliği, kullanılarak tamamen gerçekleştirilir `SetValue` ve `GetValue` tarafından tanımlanan yöntemler `BindableObject` birlikte `Label.TextProperty` statik özelliği.

`BindableObject` ve `BindableProperty` için destek sağlar:

- Özellikler varsayılan değerleri verir
- Geçerli değerlerini depolama
- Özellik değerlerini doğrulama mekanizmaları sağlama
- Tek bir sınıftaki ilgili özellikler arasında tutarlılığı koruma
- Özellik değişikliklere yanıt verme
- Bir özellik değiştirilmek veya değişti tetikleme bildirimleri
- Veri bağlama destekleme
- Stilleri destekleme
- Dinamik kaynaklar destekleme

Bağlanabilir özelliği değişiklikleri tarafından yedeklenen bir özelliği her `BindableObject` ateşlenir bir [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) değişti özellik tanımlama olay. Özelliği aynı değere ayarlandığında bu olay tetiklenir değil.

Bazı özellikler bağlanabilir özellikleri ve bazı Xamarin.Forms sınıfları & #x 2014 tarafından yedeklenen değil; gibi `Span` & #x 2014; öğesinden türetilen değil `BindableObject`. Türeyen bir sınıf `BindableObject` bağlanabilir özellikleri nedeniyle destekleyebilir `BindableObject` tanımlar `SetValue` ve `GetValue` yöntemleri.

Çünkü `Span` türünden türemez `BindableObject`, hiçbiri özellikleri & #x 2014; gibi `Text` & #x 2014; bağlanabilirse özelliği tarafından desteklenir. Nedeni budur bir `DynamicResource` ayarı `Text` özelliği `Span` bir özel durumla başlatır [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) önceki bölümde örnek. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) örnek kodu kullanarak dinamik kaynaklar ayarlama gösteren [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/) tarafından tanımlanan yöntemi `Element`. İlk bağımsız değişken türünde bir nesnedir `BindableProperty`.

Benzer şekilde, [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) yöntemi tarafından tanımlanan `BindableObject` ilk bağımsız değişken türü sahip `BindableProperty`.

## <a name="defining-bindable-properties"></a>Bağlanabilir özellikleri tanımlama

Statik kullanarak kendi bağlanabilir özelliklerini tanımlayabilirsiniz [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi veya biraz daha kısa [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) türü statik salt okunur alanı oluşturmak için aşırı `BindableProperty`.

Bu, gösterilmiştir [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. Sınıfın türetildiği `Label` ve bir yazı tipi boyutu noktaları belirtmenize olanak tanır. Örneklerde gösterildiği [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) örnek.

Dört bağımsız `BindableProperty.Create` yöntemi gereklidir:

- `propertyName`: metin özelliğinin adı (aynı CLR özellik adı)
- `returnType`: CLR özellik türü
- `declaringType`: özelliği bildirme sınıfın türü
- `defaultValue`: özelliğin varsayılan değeri

Çünkü `defaultValue` türü `object`, derleyici varsayılan değerinin türü belirlemek mümkün olması gerekir. Örneğin, varsa `returnType` olan `double`, `defaultValue` 0.0 yerine yalnızca 0 gibi bir şekilde ayarlamanız gerekir ya da bir özel durum çalışma zamanında tür uyuşmazlığı tetikler.

Eklenecek bağlanabilirse özelliği için sık görülür:

- `propertyChanged`: bir statik yöntem, özellik değeri değiştiğinde çağrıldı. İlk bağımsız değişkeni, özelliği değiştirildi sınıfı örneğidir.

İçin başka bir bağımsız değişken `BindableProperty.Create` gibi yaygın değildir:

- `defaultBindingMode`: veri bağlama bağlantılı olarak kullanılan (anlatıldığı gibi [ **Bölüm 16. Veri bağlama**](chapter16.md))
- `validateValue`: için geçerli bir değer denetlemek için bir geri çağırma
- `propertyChanging`: özellik değiştirilmek olduğunda belirtmek için bir geri çağırma
- `coerceValue`: başka bir değer kümesi değerine coerce için bir geri çağırma
- `defaultValueCreate`: (örneğin, bir koleksiyon) sınıfının örnekleri arasında paylaşılan bir varsayılan değeri oluşturmak için bir geri çağırma

### <a name="the-read-only-bindable-property"></a>Salt okunur bağlanabilirse özelliği

Bağlanabilir özelliği salt okunur olabilir. Salt okunur bir bağlanabilirse özelliği oluşturma gerektiren statik yöntemini çağırma [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) veya daha kısa bir [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) özel statik salt okunur türünde bir alan tanımlamak için [ `BindablePropertyKey` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

Ardından, CLR özellik tanımlamak `set` olarak accesor `private` çağırmak için bir [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/) ile aşırı `BindablePropertyKey` nesnesi. Bu özelliği sınıfın dışından ayarlamak engeller.

Bu, gösterilmiştir [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) kullanılan sınıf [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) örnek.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 11 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Bölüm 11 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
