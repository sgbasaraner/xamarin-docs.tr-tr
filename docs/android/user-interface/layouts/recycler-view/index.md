---
title: RecyclerView
description: "RecyclerView koleksiyonları görüntülemek için bir görünüm gruptur; ListView ve GridView gibi eski görünümü grupları için daha esnek yenileme olacak şekilde tasarlanmıştır.  Bu kılavuzda ve Xamarin.Android uygulamalarda RecyclerView özelleştirme nasıl kullanılacağı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/03/2018
ms.openlocfilehash: ec8b3a4655c8e8d9e492c9f7a1807dd64ecc6ae7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView koleksiyonları görüntülemek için bir görünüm gruptur; ListView ve GridView gibi eski görünümü grupları için daha esnek yenileme olacak şekilde tasarlanmıştır.  Bu kılavuzda ve Xamarin.Android uygulamalarda RecyclerView özelleştirme nasıl kullanılacağı açıklanmaktadır._

## <a name="recyclerview"></a>RecyclerView

Birçok uygulama koleksiyonları (örneğin, iletiler, kişiler, görüntüleri veya şarkıya); aynı türde görüntülemeniz gerekir Genellikle, bu koleksiyona koleksiyon sorunsuz koleksiyondaki tüm öğeler arasında gezinebilirsiniz küçük bir pencerede sunulur şekilde ekranda sığmayacak kadar büyük.
`RecyclerView` bir liste veya toplulukta kaydırın kullanıcının bir kılavuz öğeleri koleksiyonu görüntüler Android bir pencere öğesi değil. Bir uygulamasının ekran görüntüsü kullanan bir örnek verilmiştir `RecyclerView` dikey bir kaydırma listesinde e-posta gelen kutusuna içeriğini görüntülemek için:

[ ![Liste gelen iletiler için RecyclerView kullanarak örnek uygulama](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png)

`RecyclerView` iki ilgi çekici özellikleri sunar:

-  Tercih edilen bileşenlerinizi takarak davranışını değiştirmenize olanak sağlayan esnek bir mimari vardır.

-  Çünkü madde görünümleri yeniden kullanır ve kullanılmasını gerektiren büyük koleksiyonlarla verimli *görüntülemek sahiplerini* önbellek görünüm başvuruları.

Bu kılavuz nasıl kullanılacağını açıklar `RecyclerView` isteğe bağlı olarak Xamarin.Android uygulamalarda; nasıl ekleneceğini açıklar `RecyclerView` paketini Xamarin.Android projenizi ve onu açıklar nasıl `RecyclerView` bir genel uygulama işlevlerde. Gerçek kod örnekleri nasıl tümleştirileceği göstermek için sağlanan `RecyclerView` uygulamanız, nasıl öğesi görünümü tıklatın uygulanacağını ve nasıl yenileneceği `RecyclerView` , temel alınan veri değiştiğinde. Bu kılavuz ile Xamarin.Android geliştirme hakkında bilgi sahibi olduğunuzu varsayar.


### <a name="requirements"></a>Gereksinimler

Rağmen `RecyclerView` olan çoğunlukla Android 5.0 Lolipop ile ilişkili, destek kitaplık olarak önerilen &ndash; `RecyclerView` bu hedef API düzeyini 7 (Android 2.1) uygulamalarla çalışır ve daha sonra. Kullanmak için aşağıdaki gerekli `RecyclerView` Xamarin tabanlı uygulamalarda:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  Uygulama projenizi içermelidir **Xamarin.Android.Support.v7.RecyclerView** paket. NuGet paketlerini yükleme hakkında daha fazla bilgi için bkz: [izlenecek yol: de dahil olmak üzere bir NuGet projenizdeki](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Genel Bakış

`RecyclerView` yerini almaya yönelik olarak düşünülebilir `ListView` ve `GridView` pencere öğelerini Android de. Öncelleri gibi `RecyclerView` küçük bir pencerede büyük bir veri kümesini görüntülemek için tasarlanmıştır ancak `RecyclerView` daha fazla yerleşim seçenekleri sunar ve daha iyi büyük topluluklara görüntülemek için optimize edilmiştir. Hakkında bilginiz varsa `ListView`, birkaç önemli farklılıkları vardır `ListView` ve `RecyclerView`:

-   `RecyclerView` kullanmak için biraz daha karmaşıktır: kullanmak için daha fazla kod yazmak zorunda `RecyclerView` karşılaştırılan `ListView`.

-   `RecyclerView` önceden tanımlanmış bir bağdaştırıcısı sağlamaz; Veri kaynağınızı erişen bağdaştırıcısı kodu uygulamalıdır. Ancak, Android çalışmak birkaç önceden tanımlanmış bağdaştırıcıları içerir `ListView` ve `GridView`.

-   `RecyclerView` bir kullanıcı bir öğeyi dokunur bir öğeyi tıklatın olay sağlamaz; Bunun yerine, öğesi tıklama olaylarını yardımcı sınıfları tarafından işlenir. Bunun aksine, `ListView` bir öğeyi tıklatın olay sunar.

-   `RecyclerView` Performans görünümleri geri dönüştürme tarafından ve gereksiz Düzen kaynak aramalarını ortadan görünüm sahibi düzeni zorlayarak geliştirir. Görünüm sahibi desen kullanılması, isteğe bağlı olarak `ListView`.

-   `RecyclerView` özelleştirme kolaylaştırır modüler bir tasarım üzerinde temel alır. Örneğin, farklı yerleşim İlkesi önemli kod değişiklikleri olmadan uygulamanıza ekleyebilirsiniz.
    Bunun aksine, `ListView` yapısında görece tek yapılı değil.

-   `RecyclerView` öğe ekleme ve kaldırma için yerleşik bir animasyon içerir. `ListView` animasyon uygulama geliştiricisi bölümüne bazı ek çaba gerekir.


### <a name="sections"></a>Bölümler

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView bölümleri ve İşlevler](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

Bu konuda açıklanmaktadır nasıl `Adapter`, `LayoutManager`, ve `ViewHolder` iş desteklemeye yardımcı sınıfları birlikte `RecyclerView`.
Bu yardımcı sınıfların her biri üst düzey bir genel bakış sağlar ve bunları uygulamanızda kullanma açıklanmaktadır.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Temel bir RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Bu konuda sağlanan bilgileri inşa edilmiştir [RecyclerView bölümleri ve işlevler](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) gerçek kod örnekleri sağlayarak çeşitli `RecyclerView` öğeleri fotoğraf gözatma gerçek uygulamanızı oluşturmak için uygulanır.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView örnek genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

Bu konu içinde sunulan örnek uygulama ek kod ekler [A temel RecyclerView örnek](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) öğeyi tıklatın olayları işlemek ve güncelleştirmek nasıl yapılacağını göstermek üzere `RecyclerView` zaman temel alınan veri kaynağı değişiklikleri.


### <a name="summary"></a>Özet

Android bu kılavuzda sunulan `RecyclerView` pencere öğesi; nasıl ekleneceği açıklanmıştır `RecyclerView` kitaplığı Xamarin.Android projeleri için nasıl destek `RecyclerView` görünümleri, verimliliği için Görünüm sahibi deseni zorlar nasıl ve nasıl geri dönüştürüldüğünde çeşitli oluşturan yardımcı sınıfları `RecyclerView` koleksiyonları görüntülenecek işbirliği. Göstermek için örnek kod sağlanan nasıl `RecyclerView` tümleşiktir isteğe bağlı olarak bir uygulamaya uyarlamak nasıl açıklandığı `RecyclerView`farklı düzen yöneticileri ve bunun takma tarafından yerleşim İlkesi öğesi nasıl ele alınacağını açıklanan kullanıcının tıklama olayları ve Bildir`RecyclerView`veri kaynağı değişiklikleri.

Hakkında daha fazla bilgi için `RecyclerView`, bkz: [RecyclerView sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerViewer (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Lolipop giriş](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
