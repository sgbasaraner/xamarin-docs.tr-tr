---
title: Paylaşılan projeler kodu paylaşmak için kullan
description: Paylaşılan projeler farklı uygulama projeleri bir dizi tarafından başvurulan ortak kod yazmanıza olanak sağlar. Kod her başvurulan projenin bir parçası olarak derlenir ve platforma özgü işlevselliğine sahiptir paylaşılan kod tabanında yardımcı olmak için derleme yönergeleri dahil edebilirsiniz.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 61096635cd94d0fdd0abe6fda59c4efa41eeceb1
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270221"
---
# <a name="shared-projects-code-sharing"></a>Paylaşılan projeler, kod paylaşımı

_Paylaşılan projeler farklı uygulama projeleri bir dizi tarafından başvurulan ortak kod yazmanıza olanak sağlar. Kod her başvurulan projenin bir parçası olarak derlenir ve platforma özgü işlevselliğine sahiptir paylaşılan kod tabanında yardımcı olmak için derleme yönergeleri dahil edebilirsiniz._

Paylaşılan projeler (aynı zamanda paylaşılan varlık projeler olarak da adlandırılır) Xamarin uygulamaları dahil olmak üzere birden çok hedef projeleri arasında paylaşılan kod yazmanıza olanak sağlar.

Böylece, bir alt kümesini paylaşılan proje başvuru projelerinin derlenmesi için platforma özgü kodu koşullu olarak dahil edebileceğiniz derleyici yönergeleri destekledikleri. Derleyici yönergeleri yönetmek ve kodu her iki uygulamada nasıl görüneceğini görselleştirmenize yardımcı olması için IDE desteği yoktur.

Projeler arasında kod paylaşmak için geçmişte dosya bağlama kullandıysanız, paylaşılan projeleri benzer şekilde, ancak daha gelişmiş IDE desteği ile çalışır.

## <a name="what-is-a-shared-project"></a>Paylaşılan bir proje nedir?

Diğer çoğu proje türlerinin aksine, (DLL biçiminde) herhangi bir çıktı paylaşılan proje yok, bunun yerine kod başvurduğu her bir projeye derlenir. Bu, aşağıdaki diyagramda gösterilmiştir - kavramsal olarak paylaşılan proje öğesinin tüm içeriğini "her başvuran bir projeyi kopyalanmasını" ve bir parçası olduğu gibi sorgulamanıza derlenir.

![](shared-projects-images/sharedassetproject.png "Paylaşılan proje mimarisi")

Paylaşılan bir proje kodunda etkinleştiren veya bağlı olarak hangi uygulamanın diyagramını renkli platform kutularında tarafından önerilen kod proje kullanan kod bölümlerini devre dışı bırakma derleyici yönergeleri içerebilir.

Paylaşılan bir proje, kendi derlenmemiş, yalnızca diğer projelerde dahil edilebilecek kaynak kodu dosyalarının bir gruplandırma olarak bulunmaktadır. Başka bir proje tarafından başvurulan, kod olarak etkili bir şekilde derlenir *bölümü* projenin. Paylaşılan projeler (diğer paylaşılan projeleri dahil) başka bir proje türü başvuramaz.

Android uygulaması projeleri, diğer Android uygulaması projelere başvuramaz - Örneğin, bir Android birim testi projesi bir Android uygulaması projesinin başvuramaz unutmayın. Bu sınırlama hakkında daha fazla bilgi için bkz. Bu [forum tartışmasına](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio Mac gözden geçirme

Bu bölümde Mac için Visual Studio kullanarak bir paylaşılan proje oluşturulacağı ve kullanılacağı konusunda yol göstermektedir Başvurmak için [paylaşılan proje örneği](#Shared_Project_Example) tam bir örnek bölüm.

## <a name="creating-a-shared-project"></a>Paylaşılan bir proje oluşturma

Yeni bir paylaşılan proje oluşturmak için gidin **Dosya > Yeni çözüm...**  (veya varolan bir çözümü sağ tıklatın ve seçerek **Ekle > Yeni Proje Ekle...** ):

[![Yeni Paylaşılan proje](shared-projects-images/xs-newsolution-sml.png "yeni çözüm")](shared-projects-images/xs-newsolution.png#lightbox)

Sonraki ekranda, proje adını seçin ve **Oluştur**.

Yeni bir paylaşılan proje aşağıda gösterilen - olduğunu unutmayın: başvuru veya bileşen düğümlerini; Bu, paylaşılan projeler için desteklenmez.

![Boş proje paylaşılan](shared-projects-images/xs-empty.png "boş paylaşılan proje")

Paylaşılan kullanışlı olması için bir proje, bunun (örneğin, bir iOS veya Android uygulama veya kitaplık veya PCL projesine) en az bir yapı mümkün proje tarafından başvurulan gerekir. Bu, bunu sözdizimi (veya diğer) başvuran bir şey olduğunda bir paylaşılan proje derlenmemiş hataları Vurgulanmayan kadar bir şey tarafından başvuruldu.

Paylaşılan bir projeye bir başvuru eklemeyi, normal bir kitaplık projesine başvurma olarak aynı şekilde yapılır. Bu ekran görüntüsünde, paylaşılan bir projeye başvuran bir Xamarin.iOS projesi gösterilmektedir.

![](shared-projects-images/xs-reference.png "Proje paylaşılan proje başvurusu")

Başka bir kitaplık veya uygulama tarafından başvurulan paylaşılan projenin sonra Çözümü derleyin ve kod hataları görüntüleyin. Ne zaman paylaşılan proje tarafından başvurulan _iki veya daha fazla_ diğer projeler, hangi projelerin bu dosyaya başvuran gösterir seçin üst sol Kaynak Kod Düzenleyicisi, bir menü görünür.

## <a name="shared-project-options"></a>Paylaşılan proje seçenekleri

Paylaşılan bir proje üzerinde sağ tıklatın ve seçin, **seçenekleri** diğer değerinden daha az ayar proje türlerinin vardır. Paylaşılan projeler (kendi) derlenmemiş olduğundan, çıkış veya derleyici seçenekleri, proje yapılandırmaları, derleme imzalama veya özel komutları ayarlanamaz. Paylaşılan bir proje kodu etkili bir şekilde bu değerleri ne olursa olsun bunları başvuruyor öğesinden devralır.



**Seçenekleri** ekran proje - aşağıda gösterilen **adı** ve **varsayılan Namespace** genellikle değiştirecek yalnızca iki ayar.


![](shared-projects-images/xs-sharedprojectoptions.png "Paylaşılan proje seçenekleri")



# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)



## <a name="visual-studio-walkthrough"></a>Visual Studio gözden geçirme


Bu bölümde, Visual Studio kullanarak bir paylaşılan proje oluşturulacağı ve kullanılacağı hakkında bilgi vermektedir. Başvurmak için [paylaşılan proje örneği](#Shared_Project_Example) tam bir uygulama bölümü.

### <a name="creating-a-shared-project"></a>Paylaşılan bir proje oluşturma

Yeni bir paylaşılan proje oluşturmak için gidin **Dosya > Yeni çözüm...**  ve proje ve çözüm için bir ad seçin.

![](shared-projects-images/vs-newsolution.png "Yeni çözüm")

Ayrıca yeni bir paylaşılan proje çözüm çözüm dosyaya sağ tıklayıp seçerek ekleyebileceğiniz **Ekle > Yeni proje...** . Yeni bir paylaşılan proje (bir sınıf dosyası eklendikten sonra), aşağıda gösterildiği gibi görünür - başvuru olduğuna dikkat edin veya bileşen düğümlerini; Bu, paylaşılan projeler için desteklenmez.

![](shared-projects-images/vs-empty.png "Boş bir paylaşılan proje")

Paylaşılan kullanışlı olması için bir proje, bunun (örneğin, bir iOS veya Android uygulama veya kitaplık veya PCL projesine) en az bir yapı mümkün proje tarafından başvurulan gerekir. Bu, bunu sözdizimi (veya diğer) başvuran bir şey olduğunda bir paylaşılan proje derlenmemiş hataları Vurgulanmayan kadar bir şey tarafından başvuruldu.

Paylaşılan bir projeye bir başvuru eklemeyi, normal bir kitaplık projesine başvurma olarak aynı şekilde yapılır. Bu ekran görüntüsünde, paylaşılan bir projeye başvuran bir Xamarin.iOS projesi gösterilmektedir.

![](shared-projects-images/vs-reference.png "Proje paylaşılan proje başvurusu")

Başka bir kitaplık veya uygulama tarafından başvurulan paylaşılan projenin sonra Çözümü derleyin ve kod hataları görüntüleyin. Ne zaman paylaşılan proje tarafından başvurulan _iki veya daha fazla_ diğer projeler, üst sol geçerli kod dosyası hangi projelerin başvuru görmek için kaynak kod Düzenleyicisi, bir menü görünür.


### <a name="shared-project-properties"></a>Paylaşılan proje özellikleri


Bir paylaşılan proje var. daha az ayar Özellikler bölmesinde diğer proje türleri daha seçtiğinizde. Paylaşılan projeler (kendi) derlenmemiş olduğundan, çıkış veya derleyici seçenekleri, proje yapılandırmaları, derleme imzalama veya özel komutları ayarlanamaz. Paylaşılan bir proje kodu etkili bir şekilde bu değerleri ne olursa olsun bunları başvuruyor öğesinden devralır.

**Özellikleri** paneli - aşağıda gösterilmektedir **kök Namespace** değiştirebileceğiniz yalnızca ayardır.

![](shared-projects-images/vs-sharedprojectproperties.png "Paylaşılan proje özellikleri")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Paylaşılan proje örneği

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) paylaşılan proje ortak kod içeren bir hem iOS, Android ve Windows Phone uygulamaları kullanılan örnek. Hem `SQLite.cs` ve `TaskRepository.cs` kaynak kodu dosyaları kullanmasını derleyici yönergeleri (örn.) `#if __ANDROID__`) her bunları başvuruda bulunan uygulamaları için farklı bir çıktı oluşturmak için.

Eksiksiz bir çözüm yapısı aşağıda yer almaktadır (Mac ve Visual Studio için Visual Studio'da sırasıyla):

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Çözüm Mac için Visual Studio")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Visual Studio çözümü")

-----

Mac için Visual Studio'da derleme için bu proje türü desteklenmiyor olsa da Windows Phone proje içinde Visual Studio Mac için çıktığınız

Çalışan uygulamalar aşağıda verilmiştir:

![](shared-projects-images/example.png "iOS, Android, Windows Phone Örnekleri")

## <a name="summary"></a>Özet

Bu belge, paylaşılan projeler, nasıl bunlar oluşturulabilir ve Mac için Visual Studio ve Visual Studio, kullanılan nasıl açıklanmaktadır ve eylem paylaşılan projede gösteren basit bir örnek uygulama kullanıma sunulmuştur.

## <a name="related-links"></a>İlgili bağlantılar

- [Tasky örnek uygulaması](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Taşınabilir sınıf kitaplıkları (örnek)](~/cross-platform/app-fundamentals/pcl.md)
- [Paylaşım kod seçeneklerini (örnek)](~/cross-platform/app-fundamentals/code-sharing.md)
