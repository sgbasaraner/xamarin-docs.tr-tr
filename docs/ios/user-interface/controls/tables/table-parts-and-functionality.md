---
title: "Tablo bölümleri ve İşlevler"
ms.topic: article
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: fece27db2945ee7142296ec0872f395c127e318e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="table-parts-and-functionality"></a>Tablo bölümleri ve İşlevler

Bir UITableView 'gruplandırılmış' veya 'düz' stil olabilir ve aşağıdaki bölümlerden oluşur:

-  [Bölüm başlığı](#Section_Header)
-  [Hücreleri](#Cells) (veya tercih ederseniz satırlar)
-  [Section Footer](#Section_Footer)
-  [Index](#Index)
-  [Düzenleme modunda](#Edit_Features) ('Sil a doğru çekin' içerir ve satır sırasını değiştirmek için tanıtıcıları sürükleyin) 

Bu ekran görüntüleri bölüm satır, üstbilgiler, altbilgiler, düzenleme denetimleri ve dizin nasıl görüntüleneceğini gösterir.

 [![](table-parts-and-functionality-images/image1a.png "Bu ekran görüntüleri bölüm satır, üstbilgiler, altbilgiler, düzenleme denetimleri ve dizin nasıl görüntüleneceğini gösterir")](table-parts-and-functionality-images/image1a.png#lightbox)

Bu bölüm aşağıdaki daha ayrıntılı olarak açıklanmıştır:

<a name="Section_Header" />

## <a name="section-header"></a>Bölüm başlığı

Hücreleri isteğe bağlı olarak bölümlere gruplandırılmış, sahip özel bir üstbilgi etiketli ve/veya altbilgi ile etiketli. Üstbilgi bir dize değeriyle ayarlanabilir veya farklı düzeni veya stili için izin vermek için özel görünüm sağlanabilir.

<a name="Cells" />

## <a name="cells"></a>Hücreleri

Hücreleri bir tablo için ana kullanıcı arabirimi öğesi var. Düzgün şekilde uygulandığında, hücre bellek verimlilik için yeniden kullanılır. Dört yerleşik hücre stilleri vardır ve, kendi özel hücreleri – kodda ya da Tasarımcısı'nda film şeritleri kullanırken oluşturabilirsiniz.

<a name="Section_Footer"/>

## <a name="section-footer"></a>Bölüm altbilgi

Farklı düzeni veya stili için izin vermek için özel görünüm sağlanan veya isteğe bağlı bir bölüm altbilgi bir dize değeri ile ayarlayabilirsiniz. Bölüm üstbilgiler ve altbilgiler bağımsız olarak ayarlanabilir.

<a name="Index" />

## <a name="index"></a>Dizin

Dizin Şerit tablo sağ köşesine aşağı karakter olarak görünür.
Dokunma veya dizini sürükleyerek tablosunun bu bölümü kaydırma hızlandırır. Bir dizin isteğe bağlıdır ancak uzun listeler gidin yardımcı olmak için önerilir. Bir dizin Grouped stiliyle genellikle kullanılmaz.

<a name="Edit_Features" />

## <a name="editing-mode"></a>Düzenleme modu

Birkaç farklı düzenleme özellikleri vardır:

- Tek tek hücreleri silmek için geçirme.
- Silme düğmeleri her satırda ortaya çıkarmak için düzenleme moduna girme 
- Yeniden sıralama tanıtıcıları ortaya çıkarmak için düzenleme moduna girmeye. 
- Yeni hücrelerle (Animasyon) ekleniyor.

Bu belgenin geri kalanında, tüm bu UITableView özelliklerle Xamarin.iOS uygulaması gösterilmiştir.


## <a name="classes-overview"></a>Sınıfları genel bakış

Tablo görünümleri görüntülemek için kullanılan birincil sınıfların burada gösterilir:

[![](table-parts-and-functionality-images/classdiagram.png "Tablo görünümleri görüntülemek için kullanılan birincil sınıfların burada gösterilir")](table-parts-and-functionality-images/classdiagram.png#lightbox)

Her sınıf amacı, aşağıda belirtilmiştir:

- **UITableView** – kaydırma kapsayıcı içinde hücrelerinin koleksiyonu içeren bir görünüm. Tablo görünümünde genellikle tüm ekranı bir iPhone uygulamada ancak iPad daha büyük bir görünüm bir parçası olarak mevcut (veya bir popover görünür) kullanır. 
- **UITableViewCell** – tek bir hücre (veya satır) Tablo görünümünde temsil eden bir görünüm. Dört yerleşik hücre türü vardır ve C# veya iOS Tasarımcısı ile özel hücreleri oluşturmak mümkündür. 
- **UITableViewSource** – Xamarin.iOS özel soyut bir tablo satır sayısı dahil olmak üzere, her satır için bir hücre görünümü döndürme, işleme satır seçimi ve diğer birçok isteğe bağlı özellikleri görüntülemek için gereken tüm yöntemleri sağlayan sınıf. *Gerekir* alt UITableView çalışır hale getirilmesi için. 
- **NSIndexPath** – bir tablodaki bir hücreyi konumunu benzersizce içerir satır ve bölüm özelliklerini. 
- **UITableViewController** – UITableView sabit kodlanmış kendi görünüm olarak ve Tablo görünümü özelliği aracılığıyla erişilebilir olan bir kullanıma hazır UIViewController. 
- **UIViewController** – tablo çerçevesini ile tüm UIViewController için UITableView ayarlamak uygun şekilde ekleyebilirsiniz ekranın tamamını kaplayacak değil. 

UITableViewSource Xamarin.iOS içinde hala kullanılabilir, ancak normalde gerekli olmayan aşağıdaki iki sınıf değiştirir:

- **UITableViewDataSource** – Xamarin.iOS soyut bir sınıf olarak modellendi bir Objective-C protokolü. Bir tablo olan bir görünümü her hücre yanı sıra üstbilgileri, altbilgileri ve satır ve tablosunda bölüm sayısı hakkında bilgi sağlamak için sınıflandırma gerekir. 
- **UITableViewDelegate** – Xamarin.iOS bir sınıf olarak modellendi bir Objective-C protokolü. Seçimi, özellikleri ve diğer isteğe bağlı tablo özelliğini düzenleme işler. 

Bu belgedeki örneklerde tüm UITableViewSource kullanın ve bu iki sınıf yoksay. Ne yaptıklarını (ve Xamarin.iOS'ın UITableViewSource yerine kullanabilirsiniz) anlamak kullanışlıdır şekilde Apple belgelerinde bulunan tüm Objective-C örnekler bunları başvurur çünkü bunlar burada belirtilen.

## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithTables (örnek)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
