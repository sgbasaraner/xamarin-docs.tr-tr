---
title: Bölüm 1 özeti. Xamarin.Forms nasıl sığmayan?
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 534c36a16acdc10ffb6f6b6703296a672875286e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Bölüm 1 özeti. Xamarin.Forms nasıl sığmayan?

Özellikle, platform farklı bir programlama dilinde içeriyorsa programlamada en kötü işlerden biri diğerine platformdan diğerine temel bir kod taşımaktır. Yapıldığında bir buradaki eðilim de yeniden düzenlemeniz için kod bağlantı noktası oluşturma, ancak her iki platform paralel olarak korunmalıdır, ardından iki kod temelleri arasındaki farklar gelecek bakım daha zor hale getirir.

## <a name="cross-platform-mobile-development"></a>Platformlar arası Mobil Geliştirme

Bu sorun, mobil platformu hedeflerken yaygındır. Şu anda iki önemli mobil platformlar, iPhone ve iOS işletim sistemi ve, çeşitli telefonlar ve tabletler üzerinde çalışan Android işletim sistemi çalıştıran iPad cihazları Apple ailesi var. Başka bir önemli Microsoft'un Evrensel Windows Platformu (Windows 10 ve Windows 10 Mobile hedeflemek tek bir program sağlayan UWP), bir platformdur.

Bu üç platformlar hedef istediği bir yazılım satıcısı ilgilenmeniz gerekir farklı kullanıcı arabirimi örneklerinde, üç farklı geliştirme ortamları, üç farklı programlama arabirimleri ve&mdash;belki de en awkwardly&mdash; üç farklı programlama dillerini: Objective-C iPhone ve iPad, Android için Java ve C# Windows için.

## <a name="the-c-and-net-solution"></a>C# ve .NET çözümü

Objective-C, Java ve C# tüm C programlama dili türetilen rağmen çok farklı yollarla gelişim göstermiştir. C# bu diller en güncel olduğundan ve çok kullanışlı şekilde maturing. Ayrıca, C# matematik, hata ayıklama, yansıma, koleksiyonları, Genelleştirme, dosya g/ç, ağ, güvenlik, iş parçacığı oluşturma, web Hizmetleri, veri işleme ve XML için destek sağlayan .NET adlı tüm programlama altyapısına yakından ilişkilidir ve Okuma ve yazma JSON.

Xamarin şu anda yerel Mac, iOS ve Android C# .NET ile API'leri hedeflemek için araçlar sağlar. Bu araçları Xamarin.Mac, Xamarin.iOS ve topluca Xamarin platform olarak bilinen Xamarin.Android denir. Kitaplıklar ve bu platformları .NET deyimleri ile yerel API'lerinin express bağlamaları bunlar.

Geliştiriciler, uygulamaları C# ' ta, hedef Mac, iOS veya Android yazmak için Xamarin platform kullanabilir. Ancak, birden çok platformu hedeflerken çok bazı kodları Hedef platformlar arasında paylaşmak için algılama sağlar. Bu programın (genellikle kullanıcı arabirimi içeren) platforma bağımlı kod ve genellikle yalnızca temel .NET framework gerektirir platformdan bağımsız kodu ayıran içerir. Bu platformdan bağımsız kodu ya da taşınabilir sınıf kitaplığı (PCL) veya bir paylaşılan varlık proje veya SAP adlandırılırlar paylaşılan bir proje bulunabilir.

## <a name="introducing-xamarinforms"></a>Xamarin.Forms Tanıtımı

Birden çok mobil Platform hedeflerken Xamarin.Forms daha da fazla kod paylaşımını sağlar. Xamarin.Forms için yazılmış tek bir program beş farklı platformlar hedef alabilirsiniz:

- iOS iPhone, iPad ve iPod touch çalışan programlar için
- Android Android telefonlar ve tabletler çalışan programlar için
- Hedef Windows 10 ve Windows 10 Mobile için evrensel Windows platformu
- Windows 8.1, Windows çalışma zamanı API
- Windows Phone 8.1, Windows çalışma zamanı API

Geçerli Xamarin.Forms çözümünü şablonları projeleri Şablonları Windows 8.1 ve Windows Phone 8.1 platformları içermez.

Xamarin.Forms program toplu bir PCL veya bir SAP bulunmaktadır. Her platformları PCL çağıran küçük uygulama saplama oluşur. Xamarin.Forms API'ları eşleme her platformda yerel denetimlere böylece her platform özellik, görünüm korur:

[![Üçlü ekran paylaşımı platform görselleri görüntüsü](images/ch01fg03-small.png "Xamarin.Forms denetimleri her platformda")](images/ch01fg03-large.png#lightbox "her platformda Xamarin.Forms denetimleri")

Soldan sağa ekran bir iPhone, Android telefonla ve Windows 10 cep telefonu gösterir. Her ekranında, bir Xamarin.Forms sayfa içeriyor [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) metin görüntülemek için bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Eylemler, başlatma için bir [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) için bir açık/kapalı değer seçmesini ve [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) sürekli bir aralıkta bir değer belirtmek için. Bu görünüm tüm dört alt öğeleri olan bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) üzerinde bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Ayrıca sayfaya bağlı olan birkaç oluşan bir Xamarin.Forms araç [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) nesneleri. Bu, iOS ve Android ekranlar üst kısmında ve Windows 10 Mobile ekranın simgeler olarak görünür.

Xamarin.Forms XAML de destekler, Genişletilebilir uygulama biçimlendirme dili Microsoft'taki birkaç uygulama platform için geliştirilmiştir. Yukarıda gösterilen programın tüm Görsellere örnekte gösterildiği gibi XAML içinde tanımlanan [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) örnek.

Bir Xamarin.Forms programın üzerinde çalıştırıldığı hangi platformu belirlemek ve farklı kod uygun şekilde yürütür. Daha güçlü bir şekilde geliştiriciler çeşitli platformlar için özel kod yazmanıza ve bu kodu bir platformdan bağımsız şekilde bir Xamarin.Forms programı çalıştır. Geliştiriciler, her platform için işleyiciler yazarak ek denetimler de oluşturabilirsiniz.

Xamarin.Forms veya prototipi oluşturulurken veya hızlı bir kavram kanıtı tanıtım oluşturma--iş kolu uygulamaları için iyi bir çözüm olsa da, vektör grafikleri veya karmaşık dokunma etkileşimi gerektiren uygulamalar için daha az idealdir.

## <a name="your-development-environment"></a>Geliştirme ortamınızı

Geliştirme ortamınızı ne hedeflemek istediğiniz platformları ve hangi makineleri kullanmak isteyip istememenize bağlıdır.

Hedef iOS istiyorsanız, Xcode ve Xamarin platform yüklü Mac gerekir. Android de destek, Java ve gerekli SDK'ları yüklenmesi gerekir. İOS ve Mac için Visual Studio kullanarak Android sonra hedef

Visual Studio yükleme bilgisayarda, iOS, Android ve tüm Windows platformları hedef olanak sağlar. Ancak, iOS Visual Studio'dan hedefleme hala Xcode ve Xamarin platform yüklü Mac gerektirir.

Programların bilgisayarına USB ile bağlı ya da gerçek bir cihazı veya bir simulator test edebilirsiniz.

## <a name="installation"></a>Yükleme

Oluşturma ve bir Xamarin.Forms uygulaması oluşturma önce ve ayrı ayrı bir iOS uygulaması, bir Android uygulaması ve hedef ve geliştirme ortamınızı istediğiniz platformları bağlı olarak bir UWP uygulaması oluşturmak denemelisiniz.

Xamarin ve Microsoft web siteleri bunun nasıl yapılacağı hakkında bilgi içerir:

- [İOS ile çalışmaya başlama](~/ios/get-started/index.md)
- [Android ile çalışmaya başlama](~/android/get-started/index.md)
- [Windows Geliştirici Merkezi](http://dev.windows.com)

Bir kez oluşturabilir ve tek tek bu platformlar için projelerini çalıştırmak, oluşturma ve bir Xamarin.Forms uygulaması çalıştıran herhangi bir sorun olması gerekir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 1 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Bölüm 1 örnek](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
