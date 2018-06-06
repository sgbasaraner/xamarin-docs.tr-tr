---
title: Film şeritleri Xamarin.Mac içinde giriş
description: Bu makalede, bir Xamarin.Mac uygulamasında film şeritleri ile çalışmaya tanıtılmaktadır. Oluşturma ve uygulamanın UI film şeritleri ve Xcode'nın arabirimi Oluşturucusu'nu kullanarak koruma kapsar.
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 027998d6aff8aba4e5621b1cde51a24e18821ff9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792671"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Film şeritleri Xamarin.Mac içinde giriş

_Bu makalede, bir Xamarin.Mac uygulamasında film şeritleri ile çalışmaya tanıtılmaktadır. Oluşturma ve uygulamanın UI film şeritleri ve Xcode'nın arabirimi Oluşturucusu'nu kullanarak koruma kapsar._

Film şeritleri yalnızca penceresi tanımları ve denetimleri içerir, ancak ayrıca farklı windows arasındaki bağlantıları içerir Xamarin.Mac uygulamanız için kullanıcı arabirimi geliştirmek izin ver (aracılığıyla segues) ve durumlarını görüntüleyin.

[![](images/intro01.png "Örnek xcode'da kullanıcı Arabirimi")](images/intro01.png#lightbox)

Bu makalede bir Xamarin.Mac uygulamanın kullanıcı arabirimi tanımlamak için film şeritleri kullanarak giriş bilgileri sağlar.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Film şeritleri nelerdir?

Film şeritleri kullanarak tüm Xamarin.Mac uygulamanın UI tüm kullanıcı arabirimleri ve ayrı ayrı öğeler arasında gezinme tek bir konumda tanımlanabilir. Film şeritleri Xamarin.Mac için iş film şeritleri ile çok benzer bir şekilde Xamarin.iOS için. Ancak, farklı bir kümesini içerir _ü türleri_ farklı arabirimi deyimleri nedeniyle.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>Planda ile çalışma

Yukarıda belirtildiği gibi bir film şeridi tüm işlevsel bir bakış ayrıntılarıyla belirli bir uygulamanın kullanıcı Arabirimi tanımlar, _görünüm denetleyicileri_. Xcode'nın arabirimi Oluşturucu'da, bu denetleyicilerinden her birinin kendi yaşadığı _Sahne_.

[![](images/intro02.png "Bir örnek view controller")](images/intro02.png#lightbox)

Her Sahne arabiriminde, böylece ilişkilerini gösteren her Sahne bağlanmak (Segues denir) satırları kümesiyle olarak verilen bir görünümü ve görünüm denetleyicisi çifti temsil eder. Bazı Segues nasıl bir görünüm denetleyicisi tanımlayan bir veya daha fazla alt görünüm veya Görünüm denetleyicileri içeriyor. Diğer Segues View Controller (örneğin, bir popover ya da iletişim kutusunu görüntüleme) arasındaki geçişler tanımlayın. 

[![](images/intro03.png "Bir örnek segue")](images/intro03.png#lightbox)

Not etmek için en önemli her Segue bazı form uygulamanın UI verilen öğe arasındaki veri akışını temsil ettiğini şeydir.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>Görünüm denetleyicileri ile çalışma

Görünüm denetleyicileri bilgilerinin bir Mac uygulama içinde belirli bir görünümünü ve bu bilgileri sağlayan veri modeli arasındaki ilişkileri tanımlayın. Film şeridi her üst düzey Sahne Xamarin.Mac uygulamanın kodda bir görünüm denetleyicisini temsil eder.

[![](images/intro04.png "Görünüm denetleyicisini örnek fişleri")](images/intro04.png#lightbox)

Bu şekilde, her bir bağımsız, yeniden kullanılabilir hem bilgilerindeki görsel gösterimi (Görünüm) hem de sunar ve bu bilgileri denetlemek için mantığı eşleştirme görünümü denetleyicisidir.

Belirli bir Sahne içinde normalde kişi tarafından işlenen şeyleri yapabilirsiniz `.xib` dosyaları: 

 - Yerinde subviews ve (düğmeler ve metin kutuları gibi) denetler.
 - Öğesi konumlar ve Otomatik Yerleşim kısıtlamalarını tanımlayın.
 - Kablo Eylemler ve çıkışlar kod için UI öğeleri göstermek için.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>İle çalışma Segues

Yukarıda belirtildiği gibi Segues tüm uygulamanızın UI tanımla planda arasındaki ilişkileri sağlar. Film şeritleri içinde iOS birlikte çalışarak hakkında bilginiz varsa, bilmeniz iOS genellikle tam ekran görünümler arasında geçişler tanımlamak için Segues. Segues genellikle tanımlarken "kapsama (bir Sahne üst Sahne alt olduğu)" Bu macOS farklıdır.

MacOS içinde çoğu uygulamanın kendi görünümler birlikte bölünmüş görünümler ve sekmeler gibi kullanıcı Arabirimi öğeleri kullanarak aynı pencere içinde grup eğilimindedir. Burada görünümleri açma ve kapatma ekran geçmiş olması gerekir, iOS, sınırlı fiziksel nedeniyle alanı görüntüler.

MacOS'ın tendencies kapsama doğrultusunda verildiğinde, bazı durumlarda nerede _sunu Segues_ kalıcı Windows, sayfa görünümleri ve Popovers gibi kullanılır.

Sunu Segues kullanırken, geçersiz kılabilirsiniz `PrepareForSegue` View Controller üst yöntemi başlatmak için sunu ve değişkenleri ve herhangi bir veri görünümü sunulmasını denetleyiciye sağlayın.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>Tasarım ve çalışma saatleri

Tasarım zamanında (zaman Düzen UI Xcode'nın arabirimi Oluşturucusu'nda uğradı), her bir uygulamanın kullanıcı Arabirimi öğesi, bağlı öğeleri ayrılmıştır:

- **Planda** - hangi, oluşurlar:
    - **Görüntüleme denetleyicisi** -görünümleri ve bunları destekleyen veri arasındaki ilişkileri tanımlayın.
    - **Görünümleri ve Subviews** -kullanıcı arabirimini oluşturan gerçek öğeleri.
    - **Kapsama Segues** -planda arasındaki üst-alt ilişkileri tanımlayın.
- **Sunu Segues** -tek tek sunu modları tanımlayın. 

Yalnızca çalışma zamanı sırasında gerektiğinde bu şekilde her öğe tanımlayarak, bu geç yükleme için her öğenin sağlar. MacOS tüm işlem karmaşık, esnek kullanıcı kodu bunları çalıştırmak, yedekleme tam en az gerektiren arabirimleri oluşturmak geliştiricinin izin vermek için tasarlanmış mümkün olduğunca sistem kaynaklarla olarak etkin olan tüm oluştu.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>Film şeridi hızlı başlangıç

İçinde [film şeridi Hızlı Başlangıç](~/mac/platform/storyboards/quickstart.md) Kılavuzu, size bir kullanıcı arabirimi oluşturmak için film şeritleri ile çalışmanın anahtar kavramları tanıtır basit bir Xamarin.Mac uygulaması oluşturacaksınız. Örnek uygulaması bölün görünümü içeren oluşacak bir _içerik alanının_ ve bir _denetçisi alanı_ ve basit bir Tercihleri iletişim kutusu penceresinin sunacaktır. Biz Segues tüm kullanıcı arabirimi öğeleri birbirine bağlamak için kullanırsınız.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Film şeritleri ile çalışma

Bu bölümde ayrıntılı ayrıntılarını kapsayan [film şeritleri ile çalışma](~/mac/platform/storyboards/indepth.md) Xamarin.Mac uygulama. Biz planda ve nasıl görünüm denetleyicileri ve görünüm oluşan kapsamlı bir göz atın. Ardından, biz planda Segues ile birlikte nasıl bağlıdır bir göz atalım. Son olarak, size özel Segue türleriyle çalışma bir göz atalım. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Karmaşık film şeridi örneği

Karmaşık bir örnek bir Xamarin.Mac uygulamasında film şeritleri ile çalışmaya ilişkin bir örnek için lütfen bkz [SourceWriter örnek uygulaması](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter kod tamamlama ve basit sözdizimi vurgulama için destek sağlayan bir basit kaynak kod düzenleyicisidir.

SourceWriter kodu tam olarak geçersiz kılınan ve kullanılabilir olduğunda, bağlantılar sahip sağlanmalı temel teknolojileri veya yöntemlerini Xamarin.Mac kılavuzları belgelerinde ilgili bilgilere.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede Xamarin.Mac App'te film şeritleri ile çalışan bir Hızlı Bakış sürdü. Film şeritleri kullanarak yeni bir uygulama oluşturma ve bir kullanıcı arabirimi tanımlamak gördük. Ayrıca farklı pencereler arasında gezinmek nasıl gördüğümüz ve kullanarak görünüm durumları segues.


## <a name="related-links"></a>İlgili bağlantılar

- [Merhaba, Mac (örnek)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows ile birlikte çalışma](~/mac/user-interface/window.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
