---
title: Xamarin.iOS arama çubukları
description: Bu belge, Xamarin.iOS içinde arama çubukları kullanmayı açıklar. Arama çubukları programlı ve film şeridi olarak nasıl oluşturulacağını açıklar.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: cd78c58ecb119c437296a0befe1d319d8837edae
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789931"
---
# <a name="search-bars-in-xamarinios"></a>Xamarin.iOS arama çubukları

UISearchBar değerler listesini aramak için kullanılır. 

Üç ana bileşenleri içerir: 

- Metin girmek için kullanılan bir alandır. Kullanıcılar kendi arama terimi girmeniz için bunu kullanabilirler.
- Clear düğme, herhangi bir metin arama alanından kaldırmak için.
- Arama işlevini çıkmak için İptal düğmesi.

![Arama çubuğu](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Arama çubuğunu uygulama

Yeni bir örneği tarafından arama çubuğu başlangıç uygulamak için:

```csharp
searchBar = new UISearchBar();
```

Ve ardından yerleştirin. Aşağıdaki örnek, gezinti çubuğunda veya bir tablonun HeaderView yerleştirmek gösterilmektedir:

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Özellikler arama çubuğunda ayar:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Arama çubuğu özellikleri](searchbar-images/image6.png)

Yükselt `SearchButtonClicked` arama düğmesine basıldığında olay. Bu arama mantığınızı çağırır:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Arama çubuğunu ve arama sonuçlarında sunumu yönetme hakkında daha fazla bilgi için bkz [arama denetleyicisi ](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/) tarif.

## <a name="using-the-search-bar-in-the-designer"></a>Tasarımcıda arama çubuğu'nu kullanma

Tasarımcı Tasarımcısı'nda arama çubuğunu uygulamak için iki seçenek sunar

- Arama çubuğu
- Arama çubuğunu arama görüntü denetleyicisiyle (kullanım dışı)

![Tasarımcısı'nda arama çubuğu denetimleri](searchbar-images/image2.png)

Arama çubuğunda özelliklerini ayarlamak için özellik panelini kullanın

![Arama çubuğu özellikleri Tasarımcısı](searchbar-images/image3.png)

Bu özellikler aşağıda açıklanmıştır:

- **Metin, yer tutucu, istemi** – bu özellikleri önermek ve kullanıcıları arama çubuğunu nasıl kullanmalıdır istemek için kullanılır. Örneğin, uygulamanızı Mağaza listesi görüntüleniyorsa kullanıcılar "Şehir, Öykü adı veya posta kodu girebilirsiniz," bildirmek için komut istemi özelliğini kullanabilirsiniz
- **Arama stili** – da için arama çubuğunda ayarlayabilirsiniz **Prominent** veya **en az**. Belirgin kullanarak şey ekranında, arama çubuğunda, arama çubuğunda çizilecek odağı neden dışında renk tonu. En az stili arama çubuğunda oturum çevresindeki öğelerden karışım.
- **Özellikleri** – bu özellikleri etkinleştirme yalnızca kullanıcı Arabirimi öğesi görüntüler. Bu içinde ayrıntılı olarak doğru olayı tetiklenmeden tarafından işlevselliği uygulanmalıdır [arama çubuğu API belgeleri](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Arama sonuçları gösterir / yer işaretleri düğmesi – arama çubuğunda bir yer İmlerinde veya arama sonuçları simgesi gösterir
    - İptal düğmesi – dışında arama işlevini çıkmak için verir kullanıcıları gösterir. Bu seçilir önerilir.
    - Kapsam çubuğu – kullanıcılar kendi arama kapsamını sınırlandırmak böylece gösterir. Örneğin, Apple müzik veya kendi kitaplığı arkıyı veya sanatçı için arama yapmak istediğinizi müzik uygulamada ararken kullanıcı seçebilirsiniz. Çeşitli seçenekleri görüntülemek için bir dizi başlıkları eklemek **ScopeBarTitles** özelliği.
    ![Arama çubuğu kapsam başlıkları](searchbar-images/image4.png)

- **Metin davranışı** – Bu seçenekler, bunlar yazarken kullanıcı girişini nasıl biçimlendirilmiş gidermek için kullanılır. Büyük/küçük harf, her bir sözcük veya cümle, başlangıcı ayarlayacak veya büyük harf olarak her karakter. Bunlar yazarken düzeltme ve yazım denetimi ile sözcüklerin önerilen yazım kullanıcıyla ister.
- **Klavye** – denetimleri klavye stili görüntülenen için giriş ve bu nedenle klavyede hangi anahtarları kullanılabilir. Bu, diğer seçenekleri yanı sıra numarası paneli, telefon defteri, e-posta, URL içerir.
- **Görünüm** – klavye Görünüm stilini denetler ve her iki koyu veya kaldırılacak konulu açık.
- **Return tuşunun** – daha iyi ne gerçekleştirilecek yansıtmak için Return tuşuna etikette değiştirin. Desteklenen değerler Git, birleştirme, İleri, Bitti, yol ve arama içerir.
- **Güvenli** – giriş maskelenir tanımlar (Bu tür bir parola girişi için olduğu gibi).

## <a name="related-links"></a>İlgili bağlantılar

- [Arama denetleyicisi](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
