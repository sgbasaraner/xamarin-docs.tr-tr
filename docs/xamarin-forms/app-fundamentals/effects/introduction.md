---
title: "Giriş etkileri"
description: "Efektler izin yerel denetimlere özelleştirilmek üzere her platformda ve genellikle küçük stil değişiklikler için kullanılır. Bu makalede etkileri tanıtılmaktadır, etkiler ve özel Oluşturucu arasında sınır özetler ve PlatformEffect sınıfı tanımlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b66f47ecb8f955f6558df6fff18af92a7a8b97cf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-effects"></a>Giriş etkileri

_Efektler izin yerel denetimlere özelleştirilmek üzere her platformda ve genellikle küçük stil değişiklikler için kullanılır. Bu makalede etkileri tanıtılmaktadır, etkiler ve özel Oluşturucu arasında sınır özetler ve PlatformEffect sınıfı tanımlar._

Xamarin.Forms [sayfalar, Düzen ve denetimlerin](~/xamarin-forms/user-interface/controls/index.md) platformlar arası mobil kullanıcı arabirimleri açıklamak için ortak bir API sunar. Her sayfaya, Düzen ve denetim farklı kullanarak her platformunda işlenen bir `Renderer` sırayla (Xamarin.Forms gösterimine karşılık gelen), yerel bir denetimi oluşturur sınıfı ekranda düzenler ve paylaşılan içinde belirtilen davranışı ekler kod.

Geliştiriciler kendi özel uygulayabilirsiniz `Renderer` görünümünü ve/veya Denetim davranışını özelleştirmek için sınıflar. Ancak, bir basit denetim özelleştirme gerçekleştirmek için özel Oluşturucu sınıfı uygulama ağır yanıt görülür. Efektler daha kolay özelleştirilmek üzere her platformda yerel denetimlere izin vererek, bu işlemi basitleştirir.

Etkileri platforma özgü projelerinde sınıflara tarafından oluşturulan `PlatformEffect` denetim ve etkileri tüketilen Xamarin.Forms taşınabilir sınıf kitaplığı (PCL) veya paylaşılan kitaplığı projesindeki uygun bir denetim için ekleyerek.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Neden bir efekt özel Oluşturucu kullanılsın mı?

Efektler denetim özelleştirme basitleştirmek, yeniden kullanılabilir ve daha fazla yeniden artırmak için parametreli olabilir.

Efekt ile elde edilebilir herhangi bir şey de özel Oluşturucu ile elde edilebilir. Ancak, daha fazla esneklik ve özelleştirme etkileri daha özel Oluşturucu sunar. Aşağıdaki yönergeler özel oluşturucu üzerinde bir etkisi seçebilirsiniz durumlarda listesi:

- Platforma özgü denetim özelliklerini değiştirme istenen sonucu elde edecek bir efekt önerilir.
- Platforma özgü denetim yöntemleri geçersiz kılmak için ihtiyacı olduğunda özel Oluşturucu gereklidir.
- Xamarin.Forms Denetim uygulayan platforma özgü denetimi Değiştir gerek olduğunda özel Oluşturucu gereklidir.

## <a name="subclassing-the-platformeffect-class"></a>PlatformEffect sınıfı alt sınıf yapma

Ad alanı için aşağıdaki tabloda `PlatformEffect` her platformu ve özelliklerini türlerini sınıfı:

<table>
 <thead>
   <tr>
     <td><strong>Platform</strong></td>
     <td><strong>Namespace</strong></td>
     <td><strong>kapsayıcı</strong></td>
     <td><strong>denetimi</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td>iOS</a></td>
     <td>Xamarin.Forms.Platform.iOS</td>
     <td>UIView</td>
     <td>UIView</td>
   </tr>
   <tr>
     <td>Android</a></td>
     <td>Xamarin.Forms.Platform.Android</td>
     <td>ViewGroup</td>
     <td>Görüntüle</td>
   </tr>
   <tr>
     <td>Windows Phone 8.1</a></td>
     <td>Xamarin.Forms.Platform.WinRT</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td>Evrensel Windows Platformu (UWP)</a></td>
     <td>Xamarin.Forms.Platform.UWP</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

Her platforma özgü `PlatformEffect` sınıfı aşağıdaki özellikleri sunar:

- `Container` – düzenini uygulamak için kullanılan platforma özgü denetim başvuruyor.
- `Control` – Xamarin.Forms denetimi için kullanılan platforma özgü denetim başvuruyor.
- `Element` – işlenen Xamarin.Forms Denetim başvuruyor.

Kapsayıcı, denetim veya herhangi bir öğeye bağlı olduğundan bağlandıkları öğesi türü bilgilerini etkileri değil. Bu nedenle, bir efekt desteği olmayan bir öğe eklendiğinde, düzgün biçimde düşebilir veya bir özel durum. Ancak, `Container`, `Control`, ve `Element` özellikleri kendi uygulama türüne dönüştürülür. Bkz: bunlar hakkında daha fazla bilgi türleri için [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Her platforma özgü `PlatformEffect` sınıfı geçersiz kılınmalıdır efekt uygulamak için aşağıdaki yöntemleri sunar:

- [`OnAttached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnAttached()/) – bir etkileyen bir Xamarin.Forms denetimine bağlı olduğunda çağrılır. Her platform özgü etkisi sınıfındaki bu yöntemi geçersiz kılınmış bir sürümünü etkisi belirtilen Xamarin.Forms denetimine uygulanamaz durumda özel durum işleme ile birlikte bu denetimin özelleştirme gerçekleştirmek için yerdir.
- [`OnDetached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnDetached()/) – bir efekt Xamarin.Forms denetimden ayrılmış olduğunda çağrılır. Her platforma özgü etkisi sınıfındaki bu yöntemi geçersiz kılınmış bir sürümünü hiçbir etkisi temizlenmesini olay işleyicisi XML'deki kaydetme gibi yerdir.

Ayrıca, `PlatformEffect` sunan [ `OnElementPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E.OnElementPropertyChanged/p/System.ComponentModel.PropertyChangedEventArgs/) de kılınabilir yöntemi. Öğesinin bir özelliği değiştirildiğinde bu yöntem çağrılır. Her platforma özgü etkisi sınıfındaki bu yöntemi geçersiz kılınmış bir sürümünü Xamarin.Forms denetimindeki bağlanabilirse özellik değişikliklerine yanıt verme yerdir. Bu geçersiz kılma birçok kez çağrılabilir olarak değiştirildiğinde bir özellik için bir onay her zaman yapılmalıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
