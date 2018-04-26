---
title: Taşınabilir Visual Basic.NET
description: Bu kılavuz, Visual Basic Xamarin.iOS ve Xamarin.Android hedefleyen çözümlerinde kullanılabilmesi için taşınabilir sınıf kitaplığı (PCL) projeleri yazmak için nasıl kullanılabileceğini anlatılmıştır.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f37ff8a2d07681ba8e4ec3ed6ad7e5bbd85e6502
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="portable-visual-basicnet"></a>Taşınabilir Visual Basic.NET

Yerel olarak Xamarin iOS ve Android projeleri Visual Basic desteklemez; Ancak geliştiriciler mevcut Visual Basic kodu iOS ve Android geçirmek için veya Visual Basic'te önemli kısmını kendi uygulama mantığı yazmak için taşınabilir sınıf kitaplıkları kullanabilirsiniz. Xamarin.Forms uygulamalar tamamen Visual Basic (özel Oluşturucu, bağımlılık Hizmetleri ve XAML arkasındaki koda hariç) oluşturulabilir.

## <a name="requirements"></a>Gereksinimler

Taşınabilir sınıf kitaplığı desteği Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 ve Xamarin Studio 4.2, bu araçları ile oluşturulmuş tüm Xamarin projeleri Visual Basic PCL derlemeleri dahil edebilirsiniz anlamı eklendi.

Oluşturma ve Visual Basic taşınabilir sınıf kitaplıkları derlemek için Visual Studio Windows (Visual Studio 2012 veya daha yeni) kullanmanız gerekir.

> [!NOTE]
> Visual Basic kitaplıkları yalnızca oluşturulabilir ve Visual Studio kullanarak derlenir. Xamarin.iOS ve Xamarin.Android Visual Basic Dil desteklemez.
>
> Yalnızca Visual Studio'da çalışıyorsanız Xamarin.iOS ve Xamarin.Android projelerinden Visual Basic projesinde başvuruda bulunabilir.
>
> İOS ve Android projeleri ayrıca Visual Studio'da Mac için yüklenmesi gereken, Visual Basic PCL çıkış derlemesi başvuruda bulunmalıdır.


## <a name="creating-a-visual-basicnet-pcl"></a>Visual Basic.NET PCL oluşturma

Bu bölümde bir Visual Basic taşınabilir sınıf Visual Studio kullanarak kitaplığı oluşturmak nasıl aracılığıyla anlatılmaktadır.
Kitaplığı ardından Xamarin.iOS, Xamarin.Android ve Xamarin.Forms uygulamalar dahil olmak üzere diğer projelerinde başvurulabilir.

### <a name="creating-a-pcl"></a>Bir PCL oluşturma

Visual Studio'da bir Visual Basic PCL eklerken, hangi platformları kitaplığınızın ile uyumlu olmalıdır açıklayan bir profili seçmeniz gerekir. Profilleri PCL belge giriş açıklanmıştır.

Bir PCL oluşturmak ve kendi profilini seçmek için adımlar şunlardır:

1.  İçinde **yeni proje** ekran, select **Visual Basic > sınıf kitaplığı (taşınabilir)** seçeneği:

    [![](images/image1-sml.png "Yeni bir Visual Basic taşınabilir kitaplığı oluşturun")](images/image1.png#lightbox)

1.  Visual Studio ile aşağıdakileri hemen ister **taşınabilir sınıf kitaplığı eklemek** iletişim böylece profili yapılandırılır. Tuşuna basın ve desteklemek için gereken platformlar değer **Tamam**.

    [![](images/image2-sml.png "Platformlar seçerek PCL profili seçin")](images/image2.png#lightbox)

1.  Visual Basic PCL proje gösterildiği gibi görünür **Çözüm Gezgini** şöyle:

    [![](images/image3-sml.png "Boş Visual Studio PCL projesi")](images/image3.png#lightbox)


PCL artık Visual Basic kodu eklenmesi hazırdır. PCL projeler diğer projeler (uygulama projeleri, kitaplık projeleri ve hatta diğer PCL projeleri) tarafından başvurulabilir.

### <a name="editing-the-pcl-profile"></a>PCL profil düzenleme

PCL (PCL uyumlu hangi platformları denetimleri) profili görüntülenebilir ve projeye sağ tıklayıp seçerek değiştirilen **Özellikler > kitaplığı > Değiştir...** . Ortaya çıkan iletişim bu ekran görüntüsünde gösterilir:

 [![](images/image4-sml.png "Proje Özellikleri'nde PCL profilini düzenle")](images/image4.png#lightbox)

Kod PCL eklendikten sonra profili değiştirdiyseniz, kod yeni seçilen profil parçası olmayan özellikleri başvuruyorsa kitaplığı artık derlenir mümkündür.


## <a name="summary"></a>Özet

Bu makalede gösterilmiştir nasıl Visual Studio ve taşınabilir sınıf kitaplıkları kullanarak Xamarin uygulamaları Visual Basic kodunda kullanabilir. Xamarin Visual Basic doğrudan desteklemez olsa bile, PCL Visual Basic derleme iOS ve Android uygulamaları dahil edilecek Visual Basic ile yazılan kod sağlar.

Aşağıdaki sayfalarda Visual Basic.NET PCLs yerel veya Xamarin.Forms uygulamalar içinde nasıl kullanılacağını açıklar:

- [VB kullanan yerel Xamarin.iOS ve Xamarin.Android uygulamaları oluşturma](native-apps.md)
- [Yapı Xamarin.Forms VB uygulamalarla](xamarin-forms.md)


## <a name="related-links"></a>İlgili bağlantılar

- [TaskyPortableVB (örnek)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (örnek)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework (Microsoft) ile platformlar arası geliştirme](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)