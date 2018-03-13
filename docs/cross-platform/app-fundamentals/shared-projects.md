---
title: "Paylaşılan projeleri"
description: "Paylaşılan projeleri, bir dizi farklı uygulama projeleri tarafından başvurulan ortak kod yazmanıza olanak tanır. Kod başvuruda bulunan her proje bir parçası olarak derlenmiş ve paylaşılan kod temeli platforma özgü işlevselliğine sahiptir yardımcı olmak için derleyici yönergeleri dahil edebilirsiniz."
ms.topic: article
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e77c5653171ec6c69608858805de28843fc0db56
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="shared-projects"></a>Paylaşılan projeleri

_Paylaşılan projeleri, bir dizi farklı uygulama projeleri tarafından başvurulan ortak kod yazmanıza olanak tanır. Kod başvuruda bulunan her proje bir parçası olarak derlenmiş ve paylaşılan kod temeli platforma özgü işlevselliğine sahiptir yardımcı olmak için derleyici yönergeleri dahil edebilirsiniz._

Paylaşılan projeleri (aynı zamanda paylaşılan varlık projeleri de denir), Xamarin uygulamaları dahil olmak üzere birden çok hedef projeleri arasında paylaşılan kod yazmanıza olanak tanır.

Bir alt kümesini paylaşılan projesine başvurma projelerin derlenecek platforma özgü kodu koşullu içerebilir böylece bunlar derleyici yönergeleri destekler. Derleyici yönergeleri yönetmek ve kodu her iki uygulamada nasıl görüneceğine görselleştirmenize yardımcı olmak için IDE desteği yoktur.

Kod projeler arasında paylaşmak için geçmiş dosyası olarak bağlama kullandıysanız, paylaşılan projeleri benzer şekilde, ancak daha gelişmiş IDE desteği ile çalışır.



## <a name="what-is-a-shared-project"></a>Paylaşılan bir proje nedir?

Diğer birçok proje türlerinin aksine, (DLL biçiminde) herhangi bir çıktı paylaşılan bir proje yok, bunun yerine başvurduğu her projeye derlenmiş kod. Bu diyagramda gösterildiği - kavramsal olarak paylaşılan proje tüm içeriğini "başvuruda bulunan her proje kopyalanır" ve bunları bir parçası olduğu gibi sorgulamanıza derlenir.

 ![](shared-projects-images/sharedassetproject.png "Paylaşılan proje mimarisi")

Paylaşılan bir proje kodda etkinleştirmek ya da bağlı olarak hangi uygulamanın diyagramdaki renkli platform kutuları tarafından önerilen kodu kullanarak projesi kodun bölümlerini devre dışı bırakma derleyici yönergeleri içerebilir.

Paylaşılan bir proje, kendi derlenmemiş, tamamen başka projelerde dahil edilebilir kaynak kodu dosyaları gruplandırması olarak bulunmaktadır. Başka bir project tarafından başvurulduğunda etkin olarak derlenmiş kod *bölümü* projenin. Paylaşılan projeleri (diğer paylaşılan projeleri dahil) diğer proje türü başvuramaz.

Android uygulama projeleri diğer Android uygulaması projeleri başvuramaz - Örneğin, bir Android birim testi projesi bir Android uygulaması projesi başvuramaz unutmayın. Bu sınırlama hakkında daha fazla bilgi için bkz [forum tartışmasına](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio Mac gözden geçirme


Bu bölümde bir paylaşılan Mac için Visual Studio kullanarak proje oluşturmak nasıl anlatılmaktadır Başvurmak için [paylaşılan proje örneği](#Shared_Project_Example) tam bir örnek için bölüm.


## <a name="creating-a-shared-project"></a>Paylaşılan bir proje oluşturma


Yeni bir paylaşılan projesi oluşturmak için gidin **Dosya > Yeni bir çözüm...**  ve bir ad seçin.


![](shared-projects-images/xs-newsolution.png "Yeni çözümü")


Ayrıca yeni bir paylaşılan proje yönelik bir çözüm üzerinde çözüm dosyasını sağ tıklayıp seçerek ekleyebileceğiniz **Ekle > Yeni Proje Ekle...** . Yeni bir paylaşılan proje görünüyor - aşağıda gösterildiği gibi başvuru olduğunu ya da bileşen düğümleri; Bu, paylaşılan projeleri için desteklenmiyor.


![](shared-projects-images/xs-empty.png "Boş paylaşılan bir proje")


Paylaşılan kullanışlı olması proje için bunun en az bir yapı mümkün projesi (örneğin, bir iOS veya Android uygulaması veya kitaplık veya PCL Proje) tarafından başvurulan gerekir. Bu, bunu sözdizimi (veya diğer) başvuran bir şey olduğunda paylaşılan bir proje derlenmemiş hataları değil vurgulanmış, başka bir şey tarafından başvuruldu kadar.



Paylaşılan bir proje için bir başvuru ekleyerek, normal bir kitaplık projesine başvurma olarak aynı şekilde yapılır. Bu ekran görüntüsü, paylaşılan bir proje başvuran bir Xamarin.iOS projesi gösterir.


![](shared-projects-images/xs-reference.png "Paylaşılan projeye proje başvurusu")


Paylaşılan proje başka bir kitaplık veya uygulama tarafından başvurulan sonra çözümü oluşturun ve kod hataları görüntüleyin. Ne zaman paylaşılan proje tarafından başvurulan _iki veya daha fazla_ diğer projeler menü gösterir projeleri bu dosyaya başvuran seçin kaynak kod düzenleyicisinde üst-sol içinde görünür.



## <a name="shared-project-options"></a>Proje seçenekleri paylaşılan


Ne zaman bir paylaşılan projeye sağ tıklayın ve seçin **seçenekleri** türleri var. proje diğer daha az ayarları. Paylaşılan projeleri (kendi başlarına) derlenmemiş için çıktı veya derleyici seçenekleri, proje yapılandırmaları, derleme imzalama veya özel komutlar ayarlanamıyor. Paylaşılan bir proje kodda ne olursa olsun bunları başvuruyor öğesinden bu değerleri etkili bir şekilde devralır.



**Seçenekleri** ekran proje - aşağıda gösterilen **adı** ve **varsayılan Namespace** genellikle değiştirecek yalnızca iki ayarlardır.


![](shared-projects-images/xs-sharedprojectoptions.png "Proje seçenekleri paylaşılan")



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)



## <a name="visual-studio-walkthrough"></a>Visual Studio'da izlenecek yollar


Bu bölümde, paylaşılan Visual Studio kullanarak bir proje oluşturmak nasıl aracılığıyla anlatılmaktadır. Başvurmak için [paylaşılan proje örneği](#Shared_Project_Example) tam bir uygulama için bölüm.


### <a name="creating-a-shared-project"></a>Paylaşılan bir proje oluşturma


Yeni bir paylaşılan projesi oluşturmak için gidin **Dosya > Yeni bir çözüm...**  ve proje ve çözüm için bir ad seçin.


![](shared-projects-images/vs-newsolution.png "Yeni çözümü")


Ayrıca yeni bir paylaşılan proje yönelik bir çözüm üzerinde çözüm dosyasını sağ tıklayıp seçerek ekleyebileceğiniz **Ekle > Yeni proje...** . (Bir sınıf dosyası eklendikten sonra), aşağıda gösterildiği gibi paylaşılan yeni bir proje arar - başvuru dikkat edin veya bileşen düğümleri; Bu, paylaşılan projeleri için desteklenmiyor.


![](shared-projects-images/vs-empty.png "Boş paylaşılan bir proje")


Paylaşılan kullanışlı olması proje için bunun en az bir yapı mümkün projesi (örneğin, bir iOS veya Android uygulaması veya kitaplık veya PCL Proje) tarafından başvurulan gerekir. Bu, bunu sözdizimi (veya diğer) başvuran bir şey olduğunda paylaşılan bir proje derlenmemiş hataları değil vurgulanmış, başka bir şey tarafından başvuruldu kadar.



Paylaşılan bir proje için bir başvuru ekleyerek, normal bir kitaplık projesine başvurma olarak aynı şekilde yapılır. Bu ekran görüntüsü, paylaşılan bir proje başvuran bir Xamarin.iOS projesi gösterir.


![](shared-projects-images/vs-reference.png "Paylaşılan projeye proje başvurusu")


Paylaşılan proje başka bir kitaplık veya uygulama tarafından başvurulan sonra çözümü oluşturun ve kod hataları görüntüleyin. Ne zaman paylaşılan proje tarafından başvurulan _iki veya daha fazla_ diğer projeler menü geçerli kod dosyası projeleri başvuru görmek için kaynak kod düzenleyicisinde üst-sol içinde görünür.


### <a name="shared-project-properties"></a>Paylaşılan proje özellikleri


Paylaşılan proje var. daha az ayarlarını diğer proje türleri daha Özellikleri panelinde seçtiğinizde. Paylaşılan projeleri (kendi başlarına) derlenmemiş için çıktı veya derleyici seçenekleri, proje yapılandırmaları, derleme imzalama veya özel komutlar ayarlanamıyor. Paylaşılan bir proje kodda ne olursa olsun bunları başvuruyor öğesinden bu değerleri etkili bir şekilde devralır.



**Özellikleri** Masası aşağıda gösterilmiştir - **kök Namespace** değiştirebileceğiniz yalnızca ayarıdır.


![](shared-projects-images/vs-sharedprojectproperties.png "Paylaşılan proje özellikleri")



-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Paylaşılan proje örneği

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) paylaşılan ortak kodun içerecek projesi kullanır kullanılan hem iOS tarafından Android ve Windows Phone uygulamaları örnek. Hem `SQLite.cs` ve `TaskRepository.cs` kaynak kodu dosyaları utilise derleyici yönergeleri (ör.) `#if __ANDROID__`) bunları başvuru uygulamaların her biri için farklı bir çıktı oluşturmak için.

Eksiksiz bir çözüm yapısı aşağıda verilmiştir (Mac ve Visual Studio için Visual Studio'da sırasıyla):

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 ![](shared-projects-images/xs-examplesolution.png "Visual Studio Mac çözüm için")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](shared-projects-images/vs-examplesolution.png "Visual Studio çözümü")

-----

Mac için Visual Studio'da derleme için bu proje türü desteklenmiyor olsa da Windows Phone proje sayfadan Visual Studio içinde Mac için çıkıldığında

Çalışan uygulamalar, aşağıda gösterilmektedir.

 ![](shared-projects-images/example.png "iOS, Android, Windows Phone Örnekleri")



## <a name="summary"></a>Özet

Bu belge, nasıl bunlar oluşturulabilir ve Mac için Visual Studio ve Visual Studio içinde kullanılan, paylaşılan projeleri nasıl çalıştığı açıklanmıştır ve eylem paylaşılan bir projede gösteren basit örnek uygulama sunulan.

## <a name="related-links"></a>İlgili bağlantılar

- [Tasky örnek uygulaması](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Taşınabilir sınıf kitaplıkları (örnek)](~/cross-platform/app-fundamentals/pcl.md)
- [Paylaşım kodu seçeneklerini (örnek)](~/cross-platform/app-fundamentals/code-sharing.md)
