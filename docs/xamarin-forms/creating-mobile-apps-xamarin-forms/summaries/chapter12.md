---
title: Bölüm 12 özeti. Stilleri
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: bb26c5d93bc9945ad43ed62d7feba2bc851e510e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-12-styles"></a>Bölüm 12 özeti. Stilleri

Xamarin.Forms içinde stilleri özelliği ayarlar koleksiyonu paylaşmak birden çok görünüm sağlar. Bu biçimlendirme azaltır ve tutarlı görsel Temalar koruma sağlar.

Stilleri neredeyse her zaman tanımlanır ve biçimlendirme tüketilen. Türünde bir nesne [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) bir kaynak sözlüğü örneği ve ardından kümesine [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) kullanarak bir görsel öğe özelliği bir `StaticResource` veya `DyanamicResource` biçimlendirme uzantı.

## <a name="the-basic-style"></a>Temel stili

A `Style` gerektiren kendi [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) uygulandığı görsel nesne türü için ayarlanabilir. Zaman bir `Style` örneği de (genel olarak) kaynak sözlükte gerektirdiği bir `x:Key` özniteliği.

`Style` İçerik türü özelliğine [ `Setters` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/), koleksiyonu olduğu [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) nesneleri. Her `Setter` ilişkilendiren bir [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) ile bir [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/).

XAML'de `Property` ayardır CLR özellik adını (gibi `Text` özelliği `Button`) ancak stilde özelliği bağlanabilirse özelliği tarafından yedeklenmelidir. Ayrıca, özelliği tarafından belirtilen sınıf tanımlanmalıdır `TargetType` ayar ya da bu sınıf tarafından devralınan.

Belirleyebileceğiniz `Value` özellik öğesi kullanarak ayarlama `<Setter.Value>`. Bu ayarlamanıza olanak tanır `Value` bir metin dizesi veya çok açıklanamayan bir nesneye bir `OnPlatform` nesnesi veya bir nesne örneği kullanarak `x:Arguments` veya `x:FactoryMethod`. `Value` Özelliği de ayarlanabilir ile bir `StaticResource` başka bir öğe sözlükte ifade.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) program temel sözdizimi gösterir ve başvuru gösterilmektedir `Style` ile bir `StaticResource` biçimlendirme uzantısı:

[![Üçlü ekran görüntüsü temel stil](images/ch12fg01-small.png "temel stilleri")](images/ch12fg01-large.png#lightbox "temel stilleri")

`Style` Nesne ve oluşturulan herhangi bir nesne `Style` nesnesinin bir `Value` ayarı, başvuran tüm görünümleri arasında paylaşılan `Style`. `Style` , Gibi paylaşılamaz herhangi bir şey içeremez bir `View` türevi.

Olay işleyicileri ayarlanamaz bir `Style`. `GestureRecognizers` Özelliği ayarlanamaz bir `Style` bağlanabilir özelliği tarafından yedeklenen değil çünkü.

## <a name="styles-in-code"></a>Kodda stilleri

Ortak olmasa da, örneği başlayamadı ve `Style` kod nesneleri. Bu tarafından gösterilen [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) örnek.

## <a name="style-inheritance"></a>Stil devralma

`Style` sahip bir [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) ayarlamak için özellik bir `StaticResource` biçimlendirme uzantısı başka bir stil başvuruyor. Bu, önceki stilleri, devralır ve eklemek veya özellik ayarlarını değiştirmek stilleri sağlar. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) örnek bu gösterir.

Varsa `Style2` dayanır `Style1`, `TargetType` , `Style2` aynı olmalıdır `Style1` veya türetilmiş `Style1`. Kaynak sözlükte `Style1` depolanan aynı kaynak sözlüğü olarak olmalıdır `Style2` veya görsel ağaçta daha yüksek bir kaynak sözlüğü.

## <a name="implicit-styles"></a>Örtük stilleri

Varsa bir `Style` bir kaynak sözlüğü sahip olmayan bir `x:Key` öznitelik ayarı, bir sözlük anahtarı otomatik olarak atanır ve `Style` nesne haline gelir bir *örtük stili*. Bir görünümü olmadan bir `Style` ayarı ve türü ile eşleşen `TargetType` tam olarak bu stili bulur [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) örnek gösterilmektedir.

Örtülü bir stil öğesinden türetilen bir `Style` ile bir `x:Key` ayar ancak şekilde geçici. Örtülü bir stil açıkça başvuramaz.

Üç tür stilleri hiyerarşisiyle uygulayabilirsiniz ve `BasedOn`:

- Üzerinde tanımlanan stiller gelen `Application` ve `Page` düzenleri görsel ağaçta alt tanımlanan stiller aşağı kaydırın.
- Temel sınıflar için gibi tanımlanan stiller gelen `VisualElement` ve `View` belirli sınıfları için tanımlanan stiller için.
- Stilleri örtülü stiller açık sözlük anahtarlara sahip.

Bu hiyerarşi içinde gösterilmiştir [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) örnek.

## <a name="dynamic-styles"></a>Dinamik stilleri

Bir kaynak sözlüğü stilde tarafından başvurulan `DynamicResource` yerine `StaticResource`. Bu stilini kılar bir *dinamik stili*. Bu stili kaynak sözlükte tarafından başka bir stil aynı anahtarla o stille başvuran görünümlerin yerine `DynamicResource` otomatik olarak değiştirin. Ayrıca, belirtilen anahtara sahip sözlük girdisi yokluğu neden olacak `StaticResource` bir özel durum dışında `DynamicResource`.

Stil veya tema olarak dinamik olarak değiştirmek için bu tekniği kullanabilirsiniz [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) örnek gösterilmektedir.

Ancak, ayarlanamıyor `BasedOn` özelliğine bir `DynamicResource` oluşma şekli uzantısı olduğundan `BasedOn` bağlanabilirse özelliği tarafından yedeklenmiyor. Stil dinamik olarak türetmek için ayarlamayın `BasedOn`. Bunun yerine, ayarlamak [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) sözlük anahtarı türetin istediğiniz stili özelliğine. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) örneği, bu teknik gösterir.

## <a name="device-styles"></a>Cihaz stilleri

[ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) İç içe geçmiş sınıf ile altı stilleri için on iki statik salt okunur alanları tanımlayan bir `TargetType` , `Label` metin kullanımları genel türleri için kullanabileceğiniz.

Bu alanların altı türlerinin `Style` , doğrudan ayarlayabilirsiniz bir `Style` kod özelliğinde:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

Diğer altı tür alanlardır `string` ve dinamik stilleri için sözlük anahtarları olarak kullanılabilir:

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) "BodyStyle" eşit
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) "TitleStyle" eşit
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) "SubtitleStyle" eşit
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) "CaptionStyle" eşit
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) "ListItemTextStyle" eşit
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) "ListItemDetailTextStyle" eşit

Bu stiller tarafından gösterilen [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) örnek.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 12 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Bölüm 12 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
