---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Masaüstü uygulama Taşıma Kılavuzu
description: Var olan Windows Forms veya WPF uygulamaları macOS, iOS, Android, yanı sıra üzerinde UWP/Windows 10 çalıştırmak için platformlar arası uygulamalar oluşturmak için aynı şekilde nasıl basit bir açıklaması.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b9cfad9c046d4f2ad89506f7172a0418e90478f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781052"
---
# <a name="desktop-app-porting-guidance"></a>Masaüstü uygulama Taşıma Kılavuzu

Çoğu uygulama kodu aşağıdaki bölümlerden birine kategorilere ayrılabilir:

* Kullanıcı Arabirimi (ör kodu. Windows ve düğmeleri)
* 3 taraf denetimleri (ör.) Grafik)
* İş mantığı (ör.) doğrulama kuralları)
* Yerel veri depolama ve erişim
* Web Hizmetleri ve uzak veri erişimi

Windows Forms ve WPF için C# (veya Visual Basic.NET) iş mantığı şaşırtıcı bir miktarda yazılmış uygulamalar için yerel verilere erişmek ve web hizmetleri kodu platformları arasında paylaşılabilir.

## <a name="net-portability-analyzer"></a>.NET taşınabilirlik Çözümleyicisi

Visual Studio 2015 ve 2017 desteği [.NET taşınabilirlik Çözümleyicisi](https://docs.microsoft.com/en-us/dotnet/articles/standard/portability-analyzer) ([indirmek için Windows](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)), mevcut uygulamalarınızı inceleyin ve ne kadar kod "olduğu gibi" diğer platformlar için bağlantı noktası kurulmuş söyleyin . İlgili daha fazla bu bilgiyi [Channel 9 video](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

Yoktur ayrıca bir komut satırı aracı, adresinden yüklenebilir [taşınabilirlik Çözümleyicisi github'da](https://github.com/Microsoft/dotnet-apiport) ve aynı raporları sağlamak için kullanılır.

## <a name="x-of-my-code-is-portable-what-next"></a>"x kodumu % taşınabilir. Şimdi ne yapacaksınız?"

Neyse Çözümleyicisi büyük kodunuzu bölümüdür taşınabilir, var olan kesinlikle geçmeye her uygulama bazı bölümleri olması gösterir, ancak, _olamaz_ diğer platformlar için taşınamaz.

Kod farklı öbeklerini büyük olasılıkla aşağıdaki daha ayrıntılı olarak açıklanmıştır bu demet birine girer:

* Yeniden kullanışlı taşınabilir kodu
* Değişiklikleri gerektiren kod
* Taşınabilir değil ve yeniden yazma gerektirir kodu

### <a name="re-useable-portable-code"></a>Yeniden kullanışlı taşınabilir kodu

Platformlar arası değişmeden API'leri karşı tüm platformlarda yazılır .NET kodu alınabilir. İdeal olarak, bu kod bir taşınabilir sınıf kitaplığı, paylaşılan kitaplığı ya da .NET standart kitaplığı taşıyın ve mevcut uygulamanızda test etmek mümkün olur.

Bu paylaşılan kitaplık sonra (örneğin, Android, iOS, macOS) diğer platformlar için uygulama projeleri eklenebilir.

### <a name="code-that-requires-changes"></a>Değişiklikleri gerektiren kod

Bazı .NET API'lerini tüm platformlarda kullanılamayabilir. Bu API'leri kodunuzda yoksa, platformlar arası API'ları kullanmak için bu bölümler yeniden yazma gerekir.

Bunun örnekleri .NET 4.6 kullanılabilir, ancak tüm platformlarda üzerindeki kullanılabilir değil yansıma API kullanın.

Taşınabilir API'lerini kullanarak kodu yeniden yazdıktan sonra paylaşılan bir kitaplık kodda paketini ve mevcut uygulamanızda test yapabiliyor olmanız gerekir.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Taşınabilir değil ve yeniden yazma gerektirir kodu

Platformlar arası büyük olasılıkla değil kod örnekleri şunlardır:

- **Kullanıcı arabirimi** – Windows Forms veya WPF ekranlar kullanılamaz projelerine Android veya iOS, örneğin. Kullanıcı arabirimi yeniden yazılması için bu kullanarak gerekir [denetimleri karşılaştırma](~/cross-platform/desktop/controls/index.md) bir başvuru olarak.

- **Platforma özgü depolama** -(bir yerel SQL Server Express veritabanı gibi) bir platforma özgü teknolojisi kullanır kodu. Platformlar arası alternatif (örneğin, SQLite veritabanı altyapısı için) kullanarak bu yeniden yazma gerekir.
Bazı dosya sistemi işlemleri de ayarlanması, UWP Android ve iOS (ör. biraz farklı API'lerine olduğundan gerekebilir Bazı bağlanan dosya sistemlerinin büyük/küçük harfe duyarlıdır ve diğerleri değil).

- **3 taraf bileşenleri** – 3 taraf bileşenleri uygulamalarınızda diğer platformlarda kullanılabilir olup olmadığını denetleyin. Görsel olmayan NuGet paketleri gibi bazı kullanılabilir ancak başkalarının (özellikle visual denetimleri grafik ve medya oynatıcıları gibi) olabilir

## <a name="tips-for-making-code-portable"></a>Kod taşınabilir hale getirme için ipuçları

- **Bağımlılık ekleme** – her platform için farklı uygulamaları sağlamak ve

- **Katmanlı yaklaşımın** – MVVM, MVC, MVP veya yardımcı olacak bazı bir desen ayrı olup olmadığını taşınabilir kod platforma özgü kodu.

- **İleti** – uygulama farklı kısımlarını arasındaki etkileşimler XML'deki eşleştiği için ileti geçirme kodunuzda kullanabilirsiniz.
