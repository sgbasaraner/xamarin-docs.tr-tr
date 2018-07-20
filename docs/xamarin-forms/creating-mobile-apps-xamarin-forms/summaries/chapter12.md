---
title: 12. Bölüm özeti. Stilleri
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 12 özeti. Stilleri'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 8ee169d15c4b5060f2a7696bfebd314ed7029570
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156947"
---
# <a name="summary-of-chapter-12-styles"></a>12. Bölüm özeti. Stilleri

Xamarin.Forms içinde stilleri özelliği bir ayarlar koleksiyonu paylaşmak birden çok görünüm sağlar. Bu işaretleme azaltır ve tutarlı görsel Temalar koruma sağlar.

Stilleri neredeyse her zaman tanımlı ve işaretlemede kullanılan. Bir nesne türü [ `Style` ](xref:Xamarin.Forms.Style) bir kaynak sözlüğünde örneği ve ardından kümesine [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) kullanarak bir görsel öğe özelliği bir `StaticResource` veya `DyanamicResource` biçimlendirme uzantı.

## <a name="the-basic-style"></a>Temel stili

A `Style` gerektiren kendi [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) uygulandığı visual nesne türü için ayarlanabilir. Olduğunda bir `Style` örneği de (ortak olduğu gibi) bir kaynak sözlüğünde gerektiriyorsa bir `x:Key` özniteliği.

`Style` İçerik özellik türü olan [ `Setters` ](xref:Xamarin.Forms.Style.Setters), koleksiyonu olduğu [ `Setter` ](xref:Xamarin.Forms.Setter) nesneleri. Her `Setter` ilişkilendirir bir [ `Property` ](xref:Xamarin.Forms.Setter.Property) ile bir [ `Value` ](xref:Xamarin.Forms.Setter.Value).

XAML içinde `Property` CLR özellik adını bir ayardır (gibi `Text` özelliği `Button`) ancak stil uygulanmış özelliği bağlanabilir bir özelliğe göre yedeklenmelidir. Ayrıca, özelliği tarafından belirtilen bir sınıf tanımlanmalıdır `TargetType` ayarı veya bu sınıf tarafından devralınan.

Belirtebileceğiniz `Value` özellik öğesi kullanarak `<Setter.Value>`. Bu, ayarlamanıza olanak tanır `Value` bir metin dizesi veya çok ifade edilemeyecek bir nesneye bir `OnPlatform` nesnesi veya bir nesneye örneği kullanarak `x:Arguments` veya `x:FactoryMethod`. `Value` Özelliği de ayarlanabilir ile bir `StaticResource` başka bir öğe sözlükte ifade.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) program temel sözdizimi gösterir ve dosyasına nasıl başvurulacağı gösterilmektedir `Style` ile bir `StaticResource` işaretleme uzantısı:

[![Üç ekran görüntüsü temel stili](images/ch12fg01-small.png "temel stilleri")](images/ch12fg01-large.png#lightbox "temel stilleri")

`Style` Nesne ve oluşturulan herhangi bir nesne `Style` nesnesinin bir `Value` ayarı, başvuran tüm görünümleri arasında paylaşılan `Style`. `Style` , Gibi paylaşılamaz herhangi bir şey içeremez bir `View` Türev.

Olay işleyicileri ayarlanamaz bir `Style`. `GestureRecognizers` Özelliği ayarlanamaz bir `Style` çünkü bir bağlanılabilir özellik tarafından yedeklenmez.

## <a name="styles-in-code"></a>Kod stilleri

Ortak olmamasına karşın, Başlat ve örneği `Style` kod nesneleri. Bu tarafından gösterilmiştir [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) örnek.

## <a name="style-inheritance"></a>Stil devralımı

`Style` sahip bir [ `BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) ayarlamak için özellik bir `StaticResource` işaretleme uzantısı başka bir stil başvuruyor. Bu, önceki stillerden devralır ve eklemek veya özellik ayarlarını değiştirmek stiller sağlar. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) örnek bunu gösterir.

Varsa `Style2` dayanır `Style1`, `TargetType` , `Style2` aynı olmalıdır `Style1` veya türetilmiş `Style1`. Kaynak sözlüğünde `Style1` depolanan olarak aynı kaynak sözlüğü olmalıdır `Style2` veya görsel ağacında daha yüksek bir kaynak sözlüğü.

## <a name="implicit-styles"></a>Örtük stiller

Varsa bir `Style` kaynak sözlüğü bulunmayan bir `x:Key` özniteliği ayarını sözlük anahtarı otomatik olarak atanır ve `Style` nesne haline gelir bir *örtük stil*. Bir görünüm olmadan bir `Style` ayarı ve türü ile eşleşen `TargetType` tam olarak o stilin bulabilirsiniz [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) örnek gösterir.

Örtülü bir stil türeyebilir bir `Style` ile bir `x:Key` ayarı ancak değil güvenmelidir. Örtülü bir stil açıkça başvurulamıyor.

Üç tür hiyerarşi stillerle uygulayabilir ve `BasedOn`:

- Üzerinde tanımlanan stiller gelen `Application` ve `Page` düzenleri görsel ağacında daha düşük stilleri gösteriyor.
- Gibi temel sınıflar için tanımlanan stiller gelen `VisualElement` ve `View` belirgin sınıflar için tanımlanan stiller için.
- Örtük stiller açık sözlük anahtarları ile stilleri.

Bu hiyerarşi içinde gösterilen [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) örnek.

## <a name="dynamic-styles"></a>Dinamik stiller

Bir kaynak sözlüğünde bir stil tarafından başvurulan `DynamicResource` yerine `StaticResource`. Bu stil sağlar bir *dinamik stili*. O stilin kaynak sözlüğünde tarafından başka bir stil aynı anahtarla o stilin ile başvuran görünümlerin değiştirilirse `DynamicResource` otomatik olarak değiştirin. Ayrıca, belirtilen anahtara sahip sözlük girişi olmaması neden olacak `StaticResource` bir özel durum dışında `DynamicResource`.

Dinamik olarak stil ve tema olarak değiştirmek için bu tekniği kullanabilirsiniz [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) örnek gösterir.

Ancak, ayarlanamıyor `BasedOn` özelliğini bir `DynamicResource` düzenini uzantısı olduğundan `BasedOn` bağlanabilir bir özelliği tarafından yedeklenmiyor. Bir stil dinamik olarak türetmek için ayarlamayın `BasedOn`. Bunun yerine, [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) özelliğini sözlük anahtarı türetin istediğiniz stili. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) örnek, bu teknik gösterir.

## <a name="device-styles"></a>Cihaz stilleri

[ `Device.Styles` ](xref:Xamarin.Forms.Device.Styles) İç içe geçmiş sınıf tanımlar on iki statik salt okunur alanlar için altı stilleriyle bir `TargetType` , `Label` metin kullanımları genel türleri için kullanabileceğiniz.

Bu alanların altı türlerinin `Style` , doğrudan ayarlayabilirsiniz bir `Style` kodda özelliği:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

Bir altı alanı türlerinin `string` ve dinamik stiller sözlük anahtarları olarak kullanılabilir:

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) "BodyStyle" eşit
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) "TitleStyle" eşit
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) "SubtitleStyle" eşit
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) "CaptionStyle" eşit
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) "ListItemTextStyle" eşit
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) "ListItemDetailTextStyle" eşit

Bu stiller tarafından gösterilen [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) örnek.

## <a name="related-links"></a>İlgili bağlantılar

- [12. Bölüm tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [12. Bölüm örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
