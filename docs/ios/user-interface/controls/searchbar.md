---
title: Xamarin.iOS çubuklarında arama
description: Bu belge, arama çubuğu Xamarin.ios'ta kullanmayı açıklar. Bu program aracılığıyla ve bir görsel taslak arama çubukları oluşturma işlemini açıklar.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: fdd9fe647f1a2f63b2a86a64ad92d1e71d6fcd2e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242085"
---
# <a name="search-bars-in-xamarinios"></a>Xamarin.iOS çubuklarında arama

UISearchBar değerlerin bir listesi üzerinden aramak için kullanılır. 

Bu üç ana bileşenleri içerir: 

- Metin girmek için kullanılan bir alanı. Kullanıcılar, kendi arama terimi girmek için kullanabilir.
- NET düğme, herhangi bir metin arama alanı kaldırmak için.
- Arama işlevini çıkmak için İptal düğmesi.

![Arama çubuğu](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Arama çubuğuna uygulama

Yeni bir örnekleme tarafından arama çubuğu başlangıç uygulamak için:

```csharp
searchBar = new UISearchBar();
```

Ve ardından yerleştirin. Aşağıdaki örnekte, gezinti çubuğunda veya bir tablonun HeaderView yerleştirmek gösterilmektedir:

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Arama çubuğuna ayarı özellikleri:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Arama çubuğu özellikleri](searchbar-images/image6.png)

Raise `SearchButtonClicked` Ara düğmesine basıldığında olay. Bu, arama mantığı çağırır:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Arama çubuğu ve arama sonuçlarını sunumunu yönetme hakkında daha fazla bilgi için başvurmak [arama denetleyicisi ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) tarif.

## <a name="using-the-search-bar-in-the-designer"></a>Tasarımcıda arama çubuğunu kullanarak

Tasarımcı Tasarımcısı'nda arama çubuğu uygulamak için iki seçenek sunar.

- Arama çubuğu
- Arama çubuğunda arama görüntüleme denetleyicisi (kullanım dışı)

![Tasarımcıda arama çubuğu denetimleri](searchbar-images/image2.png)

Arama çubuğunda özelliklerini ayarlamak için özellik panelini kullanın

![Arama çubuğu özellikleri Tasarımcısı](searchbar-images/image3.png)

Bu özellikler aşağıda açıklanmıştır:

- **Yer tutucu metin istemi** – bu özellikler önermek ve kullanıcılar, arama çubuğuna nasıl kullanmalıdır istemeniz için kullanılır. Örneğin, uygulama depolarının listesi görüntülenirse kullanıcıların "bir şehir, hikaye adı veya posta kodu girebilirsiniz," bildirmek için komut istemi özelliğini kullanabilirsiniz
- **Arama stil** – ya da olacak şekilde arama çubuğunu ayarlayabilirsiniz **Prominent** veya **Minimal**. Tanınmış kullanarak ekranında, arama çubuğunda Arama çubuğuna çizilecek odağı neden dışında diğer her şey renk tonu. En az bir stil arama çubuğunda, ortamı açın birleşir.
- **Özellikleri** – bu özellikleri etkinleştirme yalnızca kullanıcı Arabirimi öğesi görüntüler. İşlevi bu içinde ayrıntılı olarak doğru olayı tarafından uygulanması gereken [arama çubuğu API belgeleri](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Arama sonuçlarını gösteren / yer işaretleri düğmesi: Arama çubuğunda bir yer İmlerinde veya arama sonuçlarını simgesi gösterir
    - İptal düğmesi – exit dışında arama işlevi sağlar kullanıcıların gösterir. Bunun seçili olmasına önerilir.
    - Kapsam çubuğu – bu, kullanıcıların kendi arama kapsamını sınırlamak sağlar gösterir. Örneğin, Apple Music veya kendi kitaplık belirli şarkı veya sanatçı için arama yapmak istediğinizi müzik uygulamada ararken kullanıcı seçebilirsiniz. Çeşitli seçenekleri görüntülemek için bir dizi başlığı ekleme **ScopeBarTitles** özelliği.
    ![Arama çubuğu kapsam başlıklar](searchbar-images/image4.png)

- **Metin davranışı** – Bu seçenekler, yazmakta olduğunuz zaman kullanıcı girişini nasıl biçimlendirildiğini ele almak için kullanılır. Büyük/küçük harf her sözcük veya tümce, ayarlar ya da her karakterin büyük harf olarak. Yazarken düzeltme ve yazım denetimi ile sözcük önerilen yazım kullanıcıyla ister.
- **Klavye** – denetimleri görüntülenen klavye stili için giriş ve bu nedenle klavyede hangi anahtarları kullanılabilir. Bu, diğer seçenekleri yanı sıra numarası paneli, telefon defteri, e-posta, URL içerir.
- **Görünüm** – klavye Görünüm stilini kontrol eder ve iki koyu veya kaldırılacak teması açık.
- **Anahtarı Döndür** – etiketi dönüş anahtar hangi eylemin gerçekleştirilecek daha iyi yansıtacak şekilde değiştirin. Desteklenen değerler, Git, birleştirme, Next, Bitti, rota ve arama içerir.
- **Güvenli** – girişi maskelenmiş tanımlar (Bu tür bir parola girişi için).

## <a name="related-links"></a>İlgili bağlantılar

- [Arama denetleyicisi](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
