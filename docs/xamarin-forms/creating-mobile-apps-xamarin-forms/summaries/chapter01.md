---
title: Bölüm 1 özeti. Xamarin.Forms nasıl uygunluk sağlar?
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 1 özeti. Xamarin.Forms nasıl uygunluk sağlar?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: abf30f2cd828d67ef6fb04f809fce6235e1add9b
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156489"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Bölüm 1 özeti. Xamarin.Forms nasıl uygunluk sağlar?

> [!NOTE] 
> Bu sayfadaki notları kitapta tanıtılan malzeme gelen Xamarin.Forms nerede ayrıldığını alanları gösterir.

Özellikle, platform farklı bir programlama dilinde içeriyorsa programlamada en kötü işlerden biri diğerine bir platformdan diğerine temel bir kod taşımaktır. Kodu yeniden düzenleme de taşırken bir dürtüsüne olduğu, ancak her iki platform paralel olarak tutulması gereken, ardından iki kod tabanlarında arasındaki farklar gelecek bakım daha zor hale getirir.

## <a name="cross-platform-mobile-development"></a>Platformlar arası mobil geliştirme

Bu sorun, mobil platformu hedeflerken yaygındır. Şu anda iki önemli mobil platformlara, iPhone ve iPad iOS işletim sistemi ve çeşitli telefonlar ve tabletler üzerinde çalışan Android işletim sistemi çalıştıran Apple ailesi var. Başka bir önemli Microsoft'un Evrensel Windows Platformu (Windows 10 ve Windows 10 Mobile hedeflemek tek bir programda sağlayan UWP), platformudur.

Bu üç platformlarını hedeflemek isteyen bir yazılım satıcısı ilgilenmesi gerekir farklı kullanıcı arabirimi paradigmalarını, üç farklı geliştirme ortamlarında, üç farklı programlama arabirimleri ve&mdash;belki de en awkwardly&mdash; üç farklı programlama dilleri: Objective-C iPhone ve iPad, Android için Java ve Windows için C# için.

## <a name="the-c-and-net-solution"></a>C# ve .NET çözümü

Objective-C, Java ve C# tüm C programlama dilini türetilmiş olsa da, bunlar çok farklı yollarla göstermiştir. C# bu dillerden en yeni ve oldukça faydalı yollarla teknolojilerdeki. Ayrıca, C# matematik, hata ayıklama, yansıma, koleksiyonlar, Genelleştirme, dosya g/ç, ağ, güvenlik, iş parçacığı, web Hizmetleri, veri işleme ve XML için destek sağlayan .NET adlı tüm programlama altyapısı ile yakından ilişkilidir ve Okuma ve yazma JSON.

Xamarin, şu anda yerel Mac, iOS ve Android C# ve .NET kullanarak API'leri hedeflemek için araçlar sağlar. Bu araçlar, Xamarin.Mac, Xamarin.iOS ve Xamarin.Android, topluca Xamarin platformu olarak bilinen adı verilir. Bu, kitaplıkları ve .NET deyimleri ile Bu platformların yerel API express bağlamaları ücretlerdir.

Geliştiriciler, uygulamaları C# ', hedef Mac, iOS veya Android yazmak için Xamarin platformunu kullanabilir. Ancak, birden çok platformu hedeflerken bazı Hedef platformlar arasında kod paylaşmak için mantıklı hale getirir. Bu programın platforma bağımlı kod (genellikle kullanıcı arabirimi içeren) ve genel olarak yalnızca temel .NET framework gerektiren platformdan bağımsız kod ayırarak içerir. Bu bir platformdan bağımsız kod ya da taşınabilir sınıf kitaplığı (PCL) ya da genellikle paylaşılan varlık projesine veya SAP olarak adlandırılan bir paylaşılan proje içinde bulunabilir.

> [!NOTE] 
> Taşınabilir sınıf kitaplıkları, .NET standart kitaplıkları tarafından değiştirilmiştir. Kayıt defterinden tüm örnek kod, .NET standart kitaplıkları kullanmak için dönüştürüldü.

## <a name="introducing-xamarinforms"></a>Xamarin.Forms ile tanışın

Birden çok mobil Platform hedefleme, daha fazla kod paylaşımı Xamarin.Forms sağlar. Xamarin.Forms için yazılmış tek bir programda beş farklı platformları hedefleyebilir:

- iPhone, iPad ve iPod touch çalışan programlar için iOS
- Android telefonlar ve tabletlerde çalışan programlar için Android
- Hedef Windows 10 ve Windows 10 Mobile için evrensel Windows platformu
- Windows 8.1, Windows çalışma zamanı API
- Windows Phone 8.1, Windows çalışma zamanı API

> [!NOTE] 
> Windows 8.1, Windows Phone 8.1 veya Windows 10 Mobile artık Xamarin.Forms destekler, ancak Xamarin.Forms uygulamalarını Windows 10 Masaüstü üzerinde çalıştırın. Ayrıca Önizleme desteği yoktur [Mac](~/xamarin-forms/platform/mac.md), [WPF](~/xamarin-forms/platform/wpf.md), [GTK #](~/xamarin-forms/platform/gtk.md), ve [Tizen](/xamarin-forms/platform/tizen.md) platformlar.

Bir Xamarin.Forms programın toplu bir kitaplığı veya bir SAP bulunmaktadır. Her platformdaki bu paylaşılan koda çağıran bir kısa uygulama saplama oluşur. 

Xamarin.Forms API'leri harita her platformda yerel denetimlere ve böylece her platform özellik, görünüm tutar:

[![Üç ekran paylaşımı platform görsellerin](images/ch01fg03-small.png "Xamarin.Forms denetimleri her platformda")](images/ch01fg03-large.png#lightbox "her platformda Xamarin.Forms denetimleri")

Ekran görüntüleri soldan sağa, iPhone, Android telefon ve Windows 10 Mobile telefon gösterir. 

> [!NOTE] 
> Xamarin.Forms, artık Windows 10 Mobile da destekler.

Her ekranda bir Xamarin.Forms sayfayı içeren [ `Label` ](xref:Xamarin.Forms.Label) metni görüntülemek için bir [ `Button` ](xref:Xamarin.Forms.Button) eylemleri başlatmak için bir [ `Switch` ](xref:Xamarin.Forms.Switch) için bir açma/kapatma değer seçme ve [ `Slider` ](xref:Xamarin.Forms.Slider) sürekli bir aralıkta bir değer belirtmek için. Bu görünüm tüm dört alt öğesi olan bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) üzerinde bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

Ayrıca sayfaya bağlanmış olan birkaç oluşan bir Xamarin.Forms araç [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) nesneleri. Bu, iOS ve Android ekranlar üst kısmında ve Windows 10 Mobile ekranın alt kısmındaki simgeler olarak görülebilir.

Xamarin.Forms XAML de destekler, Extensible Application Markup Language Microsoft'ta birçok uygulama platformları için geliştirilmiştir. Yukarıda gösterilen programın tüm görseller gösterildiği şekilde XAML içinde tanımlanan [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) örnek.

Xamarin.Forms programın üzerinde çalıştırıldığı platformdan belirleyebilir ve farklı kod uygun şekilde yürütür. Daha güçlü bir şekilde geliştiriciler çeşitli platformlar için özel kod yazmak ve bu kodu bir Xamarin.Forms programından platformdan bağımsız bir biçimde çalıştırın. Geliştiriciler her platform için oluşturucu yazarak ek denetimler de oluşturabilirsiniz.

Xamarin.Forms satır iş kolu uygulamaları için ya da prototip oluşturma ve hızlı bir kavram kanıtı sunum yapmak iyi bir çözüm olsa da, vektör grafik veya karmaşık bir dokunma etkileşimi gerektiren uygulamalar için daha az uygundur.

## <a name="your-development-environment"></a>Geliştirme ortamınızı

Geliştirme ortamınızı ne hedeflemek istediğiniz platformları ve hangi makineleri kullanmak istediğinize bağlıdır.

Hedef iOS istiyorsanız, Xcode ve Xamarin platformunu yüklü Mac gerekir. Android de destek, Java ve gerekli SDK'ları yüklenmesi gerekir. Hem iOS hem de Mac için Visual Studio kullanarak Android ardından hedef

Visual Studio yükleme PC, iOS, Android ve Windows platformlarını hedeflemek sağlar. Ancak, Visual Studio içinden İos'u hedefleyen hala Xcode ve Xamarin platformunu yüklü Mac gerektirir.

Programlar veya simülatör bilgisayara USB ile bağlı bir gerçek cihaz üzerinde test edebilirsiniz.

## <a name="installation"></a>Yükleme

Oluşturma ve bir Xamarin.Forms uygulaması oluşturmaya önce ve ayrı ayrı bir iOS uygulaması, bir Android uygulaması ve hedef ve geliştirme ortamınız için istediğiniz platformları bağlı olarak bir UWP uygulaması oluşturmayı denemelisiniz.

Xamarin ve Microsoft web siteleri, bunun nasıl yapılacağı hakkında bilgi içerir:

- [İOS kullanmaya başlama](~/ios/get-started/index.md)
- [Android'i kullanmaya Başlarken](~/android/get-started/index.md)
- [Windows Geliştirme Merkezi](http://dev.windows.com)

Bir kez oluşturun ve projeleri bu tek tek platformlarda çalıştırın, oluşturma ve bir Xamarin.Forms uygulaması çalıştıran sorun sahip olmalıdır.

## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 1 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Bölüm 1 örneği](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
