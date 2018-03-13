---
title: "ContentProviders giriş"
description: "Android işletim sistemi, ortam dosyaları, kişiler ve Takvim bilgileri gibi paylaşılan verilere erişimi kolaylaştırmak için içerik sağlayıcıları kullanır. Bu makalede ContentProvider sınıfını tanıtır ve nasıl kullanılacağını iki örnekler sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 53408220f1bbd3b0aa97e0c54bd8f4c09847b448
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="intro-to-contentproviders"></a>ContentProviders giriş

_Android işletim sistemi, ortam dosyaları, kişiler ve Takvim bilgileri gibi paylaşılan verilere erişimi kolaylaştırmak için içerik sağlayıcıları kullanır. Bu makalede ContentProvider sınıfını tanıtır ve nasıl kullanılacağını iki örnekler sağlar._


## <a name="content-providers-overview"></a>İçerik sağlayıcıları genel bakış

A *ContentProvider* bir veri deposu yalıtır ve erişmek için bir API sağlar. Genellikle de bir kullanıcı Arabirimi görüntüleme/veri yönetimi sağlayan bir Android uygulaması bir parçası olarak sağlayıcısı yok. Bir içerik sağlayıcı kullanmanın temel avantaj kolayca sağlayıcısı istemci nesnesi kullanılarak şifrelenmiş verilere erişmek diğer uygulamaları etkinleştirme (adlı bir *ContentResolver*). Birlikte, bir içerik sağlayıcı ve içerik çözümleyicisinin oluşturmak ve kullanmak basit veri erişimi için tutarlı bir arası uygulama API sunar. Herhangi bir uygulama kullanmayı tercih edebileceğiniz `ContentProviders` veri dahili olarak yönetmek ve diğer uygulamalar için kullanıma sunmak için.

A `ContentProvider` aynı zamanda özel arama önerileri sağlamak, uygulamanız için gerekli olan veya diğer uygulamalara yapıştırmak için uygulamanızdan karmaşık veri kopyalama yeteneği sağlamak istiyorsanız. Bu belge erişmek ve yapı gösterilmektedir `ContentProviders` Xamarin.Android ile.

Bu bölümde yapısı aşağıdaki gibidir:

- **Nasıl çalışır** &ndash; ne genel bakış `ContentProvider` için ve onu nasıl çalışır tasarlanmıştır.

- **Bir içerik sağlayıcı tüketen** &ndash; bir örnek kişi listesi erişme.

- **Veri paylaşımı ContentProvider kullanarak** &ndash; yazılırken ve alan bir `ContentProvider` aynı uygulama.

`ContentProviders` ve bunların veriler üzerinde işlem yaparsınız imleçler genellikle ListViews doldurmak için kullanılır. Başvurmak [ListViews ve bağdaştırıcıları Kılavuzu](~/android/user-interface/layouts/list-view/index.md) bu sınıfların kullanma hakkında daha fazla bilgi için.

`ContentProviders` tarafından sunulan Android (veya diğer uygulamalar), uygulamanızda başka kaynaklardan veri eklemek için kolay bir yoludur. Bunlar ve kişiler listesi, fotoğraf veya uygulamanızın takvim olayları gibi mevcut verilere erişimi için izin ve bu verilerle etkileşim kullanıcının izin verin.

Özel `ContentProviders` (özel arama ve kopyala/yapıştır gibi özel kullanımlar dahil) diğer uygulamalar tarafından kullanılmak üzere kendi uygulama içinde ya da kullanmak için verilerinizi paketlemek için kullanışlı bir yoludur.

Bu bölümdeki konular, kullanma ve yazma bazı basit örnekler sağlar `ContentProvider` kodu.



## <a name="related-links"></a>İlgili bağlantılar

- [ContactsAdapter Demo (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [İçerik sağlayıcılar Geliştirici Kılavuzu](http://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [ContentResolver sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [ListView sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [CursorAdapter sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [UriMatcher sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [ContactsContract sınıf başvurusu](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
