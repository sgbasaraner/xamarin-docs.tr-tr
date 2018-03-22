---
title: "İOS için Xamarin Tasarımcısı ile otomatik Düzenle"
description: "Bu kılavuz, iOS otomatik düzeni ve yeni kısıtlamaları iş akışı iOS için Xamarin Tasarımcısı'nda kullanılabilir tanıtır."
ms.topic: article
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: ff19048504ee76db614306adebb71b7237139091
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>İOS için Xamarin Tasarımcısı ile otomatik Düzenle

_Bu kılavuz, iOS otomatik düzeni ve yeni kısıtlamaları iş akışı iOS için Xamarin Tasarımcısı'nda kullanılabilir tanıtır._

Otomatik Düzen ("Uyarlamalı düzeni" olarak da bilinir) bir esnek tasarım yaklaşımdır. Her öğenin konumu olduğu sabit kodlanmış bir noktasına ekranında, geçici düzeni sisteminden farklı olarak, Otomatik Yerleşim hakkındadır *ilişkileri* -öğe diğer öğeleri tasarım yüzeyine göreli konumlarını. Otomatik düzeni Kalp kısıtlamaları veya diğer öğeleri ekranında bağlamında, bir öğenin yerleştirme veya öğelerin tanımlayan kurallar olur. Öğeleri ekranında belirli bir konuma bağlı olmak zorunda değildir çünkü kısıtlamalar farklı ekran boyutlarına ve cihaz yönler iyi görünür Uyarlamalı bir düzen oluşturmak yardımcı olur.

Bu kılavuzda, kısıtlamalar ve Xamarin iOS Tasarımcısı bunlarla çalışmak nasıl tanıtır. Bu kılavuz, kısıtlamalarıyla program aracılığıyla çalışma kapsamaz. Otomatik düzeni program aracılığıyla kullanma hakkında daha fazla bilgi için bkz [Apple belgeleri](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>Gereksinimler

İOS için Xamarin Tasarımcısı, Visual Studio 2015 ve 2017 Mac Windows için Visual Studio'da kullanılabilir.

Bu kılavuz tasarımcının bileşenlerini bilgi varsayar [Tasarımcısı iOS giriş](~/ios/user-interface/designer/introduction.md) Kılavuzu.

## <a name="introduction-to-constraints"></a>Kısıtlamaları giriş

Bir kısıtlamadır ekranında iki öğe arasındaki ilişkiyi matematiksel bir gösterimi. Bir kullanıcı Arabirimi temsil eden sabit bir UI öğenin konumu kodlama ile ilgili bazı sorunlar öğenin konumunu matematiksel bir ilişki olarak çözer. Ekranın en altındaki düğmesi 20px dikey moduna bulamadığımız ise, örneğin, düğmenin konumu ekranında yatay modu devre dışı olabilir. Bu durumu önlemek için görünümün Alttan düğmesi 20px kenar yerleştirir bir kısıtlama ayarlayabilirsiniz. Düğme kenar konumunu ardından olarak hesaplanır *button.bottom view.bottom - 20px =*, hangi yerleştirin görünümün Alttan düğmesi 20px dikey ve yatay modunda. Bir matematik ilişkisine dayanarak yerleştirme hesaplamak için özelliği kısıtlamaları UI tasarımında kadar kullanışlı kılan unsurdur.

Bir kısıtlama ayarlarız biz oluşturduğunuzda bir `NSLayoutConstraint` kısıtlanacak nesneleri ve özellikleri bağımsız değişken olarak alan nesne veya *öznitelikleri*, kısıtlama üzerinde olacak. Kenarları öznitelikleri iOS Tasarımcısı'nda gibi içeren *sol*, *sağ*, *üst*, ve *alt* bir öğenin. Bunlar ayrıca boyut öznitelikleri gibi içeren *yükseklik* ve *genişliği*ve konum, işaret Merkezi *centerX* ve *centerY*. Örneğin, şu iki düğmelerinin sol sınır konumunu bir kısıtlama eklediğinizde, Tasarımcı kapsar altında aşağıdaki kodu oluşturuluyor:

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

Sonraki bölümde Tasarımcısı etkinleştirme ve Otomatik Yerleşim devre dışı bırakma ve kısıtlamaları araç çubuğunu kullanarak da dahil olmak üzere, iOS kullanılarak kısıtlamaları çalışmak kapsar.

## <a name="enable-auto-layout"></a>Otomatik düzeni etkinleştir

Varsayılan iOS Designer yapılandırması kısıtlaması modu etkin olan. Ancak, etkinleştirin veya el ile devre dışı bırakmak gerekir iki adımda bunu yapabilirsiniz:

1.  Tasarım yüzeyine üzerinde boş bir alanı tıklayın. Bu, tüm öğelerin seçimini kaldırır ve film şeridi belge özelliklerini getirir.
1.  İşaretleyin veya işaretini kaldırın **kullanım otomatik düzenleme** onay kutusu özelliği panelinde:

    ![](designer-auto-layout-images/image01.png "Özellik panelinde kullanım otomatik düzenleme onay kutusu")


Varsayılan olarak, hiçbir kısıtlamaları oluşturulan ya da yüzey üzerinde görünür. Bunun yerine, bunlar otomatik olarak derleme zamanında çerçeve bilgilerinden algılanır. Kısıtlamalar eklemek için kimliğinizi tasarım yüzeyine bir öğe seçin ve kısıtlamalar eklemek gerekiyor. Bu kullanarak yapabiliriz **kısıtlaması araç**.

## <a name="constraints-toolbar"></a>Kısıtlamaları araç çubuğu

 [![](designer-auto-layout-images/toolbarnew.png "Bağlam menü komutları")](designer-auto-layout-images/toolbarnew.png#lightbox)

Kısıtlamaları araç güncelleştirildi ve artık iki ana bölümden oluşur:

- **Kısıtlamaları modu düğmesi geçiş**: daha önce seçilen görünümünde tasarım yüzeyine yeniden tıklayarak kısıtlamaları modu girilen. Şimdi bu iki durumlu düğme kısıtlamaları çubuğunda kullanmanız gerekir:

  ![contraints modları Değiştir](designer-auto-layout-images/constraints.png)

- **Bir "Güncelleştirme kısıtlamaları" düğmesi:** değişikliklerin IF bağlı olarak, kısıtlamalar düzenleme modunda olduğunu dikkate almak önemlidir.
  - Kısıtlama düzenleme modunda bu düğme öğesi çerçeve eşleşecek şekilde kısıtlamaları ayarlar.
  - Düzenleme modunda çerçevede bu düğme öğesi çerçeve kısıtlamaları tanımlama konumu eşleşecek şekilde ayarlar.


## <a name="surface-based-constraint-editing"></a>Yüzey tabanlı kısıtlaması düzenleme

Önceki bölümde biz varsayılan kısıtlamalar eklemek ve kısıtlamaları araç çubuğunu kullanarak kısıtlamaları kaldırmak öğrendiniz. Daha fazla denetleyecek şekilde kısıtlaması düzenlemek için biz kısıtlamalar doğrudan tasarım yüzeyi ile etkileşebilirsiniz. Bu bölüm yüzeyini tabanlı kısıtlaması düzenleme, PIN-spacing denetimleri, bırakma alanları ve farklı türlerde sınırlamaları ile çalışma dahil olmak üzere temel kavramları tanıtır.

### <a name="creating-constraints"></a>Sınırlamalar oluşturma

İOS Tasarımcısı aracını iki tür tasarım yüzeyine öğelerde düzenleme denetimi sunar. *Denetimleri sürükleme* ve *PIN-spacing denetimleri*aşağıdaki görüntüde gösterildiği gibi:

![Görünümü denetimleri](designer-auto-layout-images/controls.png)

Bu kısıtlamalar çubuğunda kısıtlamaları Modu düğmesini seçerek yükseğe.

4 T şeklinde tanıtıcıları öğesi her tarafında tanımlamak *üst*, *sağ*, *alt*, ve *sol* için öğenin köşelerindeki bir kısıtlama. İki ı şekillendirilmiş tanıtıcıları sağ ve alt öğenin tanımlamak *yükseklik* ve *genişliği* kısıtlaması sırasıyla. Orta kare hem de işler *centerX* ve *centerY* kısıtlamaları.

Bir kısıtlama oluşturmak için bir tanıtıcı seçin ve herhangi bir yerde tasarım yüzeyine sürükleyin. Sürükle başlattığınızda yeşil satırları/kutularını bir dizi ne söyleyen yüzeyinde görünür, kısıtlayabilirsiniz. Örneğin, aşağıdaki ekran görüntüsünde, biz Orta Düğme üst tarafındaki sınırlama:

 [![](designer-auto-layout-images/image07.png "Orta Düğme üst tarafındaki sınırlama")](designer-auto-layout-images/image07.png#lightbox)

Üç kesikli çizgiler diğer iki düğmeleri arasında unutmayın. Çizgiler belirtmek *bırakma alanları*, veya biz sınırlamak diğer öğeleri özniteliklerini. Yukarıdaki ekran görüntüsünde, diğer iki düğmeleri 3 Dikey bırakma alanlarını sunma ( *alt*, *centerY*, *üst*) bizim düğmesi sınırlamak için. Üst görünümünün yeşil kesikli çizgi kısıtlaması üst görünümün görünüm denetleyicisi sunar ve Düz yeşil kutunun üst düzen kılavuzu aşağıda bir kısıtlama görünümü denetleyicisi sunar anlamına gelir anlamına gelir.

> [!IMPORTANT]
> Düzen kılavuzları, bize üst oluşturmak izin kısıtlaması hedefleri ve durum çubukları veya araç çubukları gibi sistem çubukları varlığını dikkate alt kısıtlamalar özel türleridir. Ana kullanır durum çubuğunun altında genişletme kapsayıcı görünümü en yeni sürüme sahip olduğundan iOS 6 ve iOS 7 arasında uyumlu bir uygulama için biridir. Üst düzen kılavuzu üzerinde daha fazla bilgi için bkz [Apple belgeleri](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).



Sonraki üç bölümde farklı türlerde sınırlamaları ile çalışma tanıtır.

### <a name="size-constraints"></a>Boyutu sınırlaması

Boyutu sınırlaması ile- *yükseklik* ve *genişliği* -iki seçeneğiniz vardır. İlk seçenek, yukarıdaki örnekte gösterildiği gibi bir komşu öğesi boyutu sınırlamak için tutamacını sürükleyin oluşturmaktır. Diğer self kısıtlama oluşturmak için tanıtıcı çift seçenektir. Bu bize bir sabit boyut değeri belirtmek aşağıdaki ekran görüntüsüne gösterildiği gibi sağlar:

 [![](designer-auto-layout-images/sizec.png "Aşağıda gösterildiği gibi bir komşu öğesi boyutu sınırlamak için tutamacını sürükleyin")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Merkezi kısıtlamaları

Kare tutamacı oluşturacak bir *centerX* veya *centerY* bağlam bağlı olarak kısıtlaması. Kare tutamacı sürükleyerek aşağıdaki ekran görüntüsüne gösterildiği gibi her iki dikey ve yatay bırakma alanları sunmak için diğer öğelerini açık:

 [![](designer-auto-layout-images/centerc.png "Merkezi kısıtlamaları")](designer-auto-layout-images/centerc.png#lightbox)

Bir dikey bırakma alanı seçerseniz bir *centerY* kısıtlaması oluşturulur. Yatay bırakma alanı seçerseniz, kısıtlama dayalı olacak *centerX*.

### <a name="combinational-constraints"></a>Combinational kısıtlamaları

Hizalama ve iki öğeler arasında boyutu eşitlik kısıtlamalar oluşturmak için öğeleri - sırayla - yatay hizalama, dikey hizalama ve boyutu equalities belirtmek üzere bir üst araç çubuğundan aşağıdaki ekran görüntüsüne gösterildiği gibi seçebilirsiniz:

 [![](designer-auto-layout-images/image06.png "Combinational kısıtlamaları")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>Kısıtlamaları düzenleme ve görselleştirme

Bir kısıtlama eklediğinizde, bir öğe seçtiğinizde, tasarım yüzeyine mavi bir çizgi görüntülenir:

 [![](designer-auto-layout-images/image09.png "Görselleştirme kısıtlamaları")](designer-auto-layout-images/image09.png#lightbox)

Mavi bir satıra tıklayarak ve kısıtlama değerlerinin özelliği panelinde doğrudan düzenleyerek bir kısıtlama seçebilirsiniz. Alternatif olarak, mavi bir satıra çift tasarım yüzeyine değerlerine doğrudan düzenlemenize olanak sağlayan bir popover getirir:

 [![](designer-auto-layout-images/image08.png "Kısıtlamaları düzenleme")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>Kısıtlama sorunları

Çeşitli sorunlar kısıtlamaları kullanırken ortaya çıkabilir:

-  **Çakışan kısıtlamaları** — bu öznitelik için çakışan değerleri varsa öğe birden çok kısıtlama zorlamak ve kısıtlama altyapısı bunları mutabık kılmak olduğunda gerçekleşir.
-  **Underconstrained öğeleri** — bir öğenin özelliklerini (konum + boyutu) tamamen sınırlamalar ve kısıtlamalar geçerli olması için iç boyutlar, dizi kapsamında olmalıdır. Bu değerler belirsiz ise, öğenin underconstrained kabul edilir.
-  **Çerçeve misplacement** — bir öğenin çerçeve ile kısıtlamaları kümesini iki farklı sonuç dikdörtgenler tanımlanırken bu oluşur.


Bu bölüm, yukarıda listelenen üç sorunları elaborates ve bunları nasıl ele alınacağını hakkında ayrıntılar sağlar.

### <a name="conflicting-constraints"></a>Çakışan kısıtlamaları

Çakışan kısıtlamaları kırmızı olarak işaretlenir ve bir uyarı simgesi vardır. Çakışma hakkında bilgi içeren bir popover yukarı uyarı simgeleri üzerine gelerek veya onları getirir:

 [![](designer-auto-layout-images/image11.png "Çakışan kısıtlamaları uyarı")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Underconstrained öğeleri

Underconstrained öğeleri turuncu içinde görünür ve görünüm denetleyicisini nesne çubuğunda turuncu işaretçi simge görünümünü tetiklemek:

 [![](designer-auto-layout-images/image02.png "İçinde turuncu underconstrained öğeler görünür")](designer-auto-layout-images/image02.png#lightbox)

Bu işaretçi simgesine tıklayın, Sahne underconstrained öğeleri hakkında bilgi almak ve sorunlar ya da tam olarak bunları kısıtlamadır veya kendi kısıtlamaları kaldırarak aşağıdaki ekran görüntüsüne gösterildiği gibi çözmek:

 [![](designer-auto-layout-images/image10.png "Underconstrained öğeleri çözme")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>Çerçeve Misplacement

Çerçeve misplacement underconstrained öğeleri olarak aynı renk kodunu kullanır. Öğe her zaman yerel çerçevesini kullanarak yüzeyinde işlenir ancak çerçeve misplacement söz konusu olduğunda, uygulama çalıştığında, burada öğesi aşağıdaki ekran görüntüsüne gösterildiği gibi son bulur kırmızı bir dikdörtgen işaretler:

 [![](designer-auto-layout-images/image05.png "Örnek çerçeve Misplacement görünümü")](designer-auto-layout-images/image05.png#lightbox)

Çerçeve misplacement hataları gidermek için seçin **güncelleştirme çerçeveler kısıtlamaları temel** (sağdaki düğme) kısıtlamaları araç çubuğu düğmesinden:

 [![](designer-auto-layout-images/image03.png "Kısıtlamaları araç çubuğu düğmesini tabanlı çerçeve güncelleştir")](designer-auto-layout-images/image03.png#lightbox)

Bu ayarlar otomatik olarak denetimleri tarafından tanımlanan konumları eşleşecek şekilde öğesi çerçevesi.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Kodda kısıtlamaları değiştirme

Uygulamanızın gereksinimlerine bağlı olarak, kodda bir kısıtlama değiştirmek gerektiğinde zamanlar olabilir. Örneğin, yeniden boyutlandırmak veya görünümü yeniden konumlandırmak için bir kısıtlaması, bir sınırlaması önceliğini değiştirmek veya kısıtlama tamamen devre dışı bırakmak bağlı.

Kodda bir kısıtlama erişmek için öncelikle aşağıdakileri yaparak iOS Tasarımcısı kullanıma vardır:

1. Kısıtlama (yukarıda listelenen yöntemlerden birini kullanarak) normal olarak oluşturun.
2. İçinde **Belge Anahattı Gezgini**, istenen kısıtlaması bulun ve seçin:

    [![](designer-auto-layout-images/modify01.png "Belge Anahattı Gezgini")](designer-auto-layout-images/modify01.png#lightbox)
3. Ardından, Ata bir **adı** kısıtlama **pencere öğesi** sekmesinde **özellikleri Explorer**:

    [![](designer-auto-layout-images/modify02.png "Pencere öğesi sekmesi")](designer-auto-layout-images/modify02.png#lightbox)
4. Değişikliklerinizi kaydedin.

Yerinde yukarıdaki değişikliklerle kod kısıtlamasında erişmek ve özelliklerini değiştirin. Örneğin, sıfır olarak ekli görünümün yüksekliğini ayarlamak için aşağıdakileri kullanın:

```csharp
ViewInfoHeight.Constant = 0;
```

Kısıtlama için şu ayarı iOS Tasarımcısı verilen:

[![](designer-auto-layout-images/modify03.png "Özellik Explorer kısıtlamasında düzenleme")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>Ertelenmiş Düzen geçişi

Anında güncelleştirme kısıtlaması değişikliklere yanıt ekli görünümünde yerine otomatik yerleşim altyapısı zamanlar bir _ertelenmiş Düzen geçişi_ yakın gelecek için. Hiyerarşideki her görünüm yeniden hesaplanması ve yeni düzenini ayarlamak için güncelleştirilmiş ertelenmiş bu geçişi sırasında güncelleştirilen verilen görünümün kısıtlaması kısıtlamalar yalnızca belirtilir.

Herhangi bir noktada çağırarak kendi ertelenmiş Düzen geçişi zamanlayabilirsiniz `SetNeedsLayout` veya `SetNeedsUpdateConstraints` görünüm üst yöntemleri. 

Ertelenmiş Düzen geçişi hiyerarşisini görüntüleme iki benzersiz geçer oluşur:

- **Güncelleştirme geçişi** -bu geçiş Otomatik Yerleşim altyapısı görünüm hiyerarşisine erişir ve çağırır `UpdateViewConstraints` tüm görünüm denetleyicilerinde yöntemi ve `UpdateConstraints` tüm görünümleri yöntemi.
- **Düzen geçişi** - yeniden hiyerarşisini görüntüleme Otomatik Yerleşim altyapısı çapraz geçiş yapıyorsa, ancak bu kez çağırır `ViewWillLayoutSubviews` tüm görünüm denetleyicilerinde yöntemi ve `LayoutSubviews` tüm görünümleri yöntemi. `LayoutSubviews` Yöntemi güncelleştirmeleri `Frame` her alt görünüm dikdörtgen özelliğinin Otomatik Yerleşim altyapısı tarafından hesaplanır.

### <a name="animating-constraint-changes"></a>Kısıtlama değişiklikleri animasyon ekleme

Kısıtlama özellikleri değiştirmenin yanı sıra, bir görünümün kısıtlamaları değişiklikleri animasyon için çekirdek animasyon kullanabilirsiniz. Örneğin:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

Anahtarı buraya çağırma `LayoutIfNeeded` yöntemi üst görünüm animasyon bloğu içinde. Animasyonlu konum veya boyut değişikliği her "Çerçeve" çizmek için Görünüm söyler. Bu satırı görünümü yalnızca en son sürüme animasyon olmadan Daya.

## <a name="summary"></a>Özet

Bu kılavuzda sunulan iOS otomatik (veya "Uyarlamalı") düzeni ve kısıtlamaları tasarım yüzeyine öğeleri arasında ilişkiler matematiksel gösterimlerini olarak kavramı. İle çalışma iOS Tasarımcısı'nda Otomatik Yerleşim etkinleştirme açıklanan **kısıtlamaları araç**ve tasarım yüzeyine tek tek kısıtlamalar düzenleme. Ardından, üç genel kısıtlamalar sorunlarının nasıl giderileceği açıklanmaktadır. Son olarak, kodda kısıtlamaları değiştirmek nasıl oluşturulacağını gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [Görsel Taslaklara Giriş](~/ios/user-interface/storyboards/index.md)
- [iOS Designable denetimleri gözden geçirme](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android Tasarımcı genel bakış](~/android/user-interface/android-designer/index.md)
- [Programsal sınırlamalar](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple - otomatik düzen kılavuzu](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
