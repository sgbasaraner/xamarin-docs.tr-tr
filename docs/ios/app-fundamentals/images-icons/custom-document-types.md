---
title: Özel belge simgeler
description: Bu makale, bir özel belge türü simgesi olarak kullanılacak bir görüntü varlığı bir Xamarin.iOS uygulaması yönetme ve dahil kapsar.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: b369667bd728f7c8b6e8bcfed9cf5bca2916bf69
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="custom-document-icons"></a>Özel belge simgeler

_Bu makale, bir özel belge türü simgesi olarak kullanılacak bir görüntü varlığı bir Xamarin.iOS uygulaması yönetme ve dahil kapsar._

Belirli bir belge türü yüklenirken bir Xamarin.iOS uygulaması destekliyorsa, geliştirici zaman aşağı bir ek olarak bir kullanıcı tutan gibi bu belge türü karşılaştığında, sistem kullanacağı simgeler sağlayabilir *posta uygulaması* olarak Burada gösterilen:

 [![](custom-document-types-images/17.png "Belge türü simgelerini örneği")](custom-document-types-images/17.png#lightbox)

Bir dosya biçimi uygulama dictionary girişlerinin dahil ederek açılmasını özellikli için geliştirici belge türü bilgileri ekleyebilirsiniz `CFBundleTypeName` dize ve `LSItemContentTypes` uygulamanın dizisinde `Info.plist`. Belge türü için simgeleri Git `CFBundleTypeIconFiles` dizi. Bir belge simgesine sağlanan değil, iOS uygulama simgesini birinden türetilir.
Simgeler, çeşitli aygıt çözünürlükleri için optimize çeşitli boyutlarda için sağlanabilir. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da bu değerleri atamak için kullandığınız **belge türleri** altında bölümünde **Gelişmiş** sekmesi `Info.plist` Düzenleyicisi belge türü ekleyin ve görüntü simgeler atayın. Örneğin, PDF desteği için kayıt gösteren ekran görüntüsü aşağıdadır:

 [![](custom-document-types-images/18.png "Belge türü Bölümü 'Info.plist' Düzenleyicisi'ni Gelişmiş sekmesinde altında")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da bu değerleri atamak için kullandığınız **belge türleri** altında bölümünde **Gelişmiş** sekmesi `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Gelişmiş sekmesi altındaki belge türlerinin bölümünü açın")

Tıklatın **belge türü Ekle** düğmesini tıklatın ve gerekli alanları doldurun:

![](custom-document-types-images/doc02w.png "Belge Türü Ekle formu")

-----


Apple'nın belge türleri hakkında daha fazla bilgi için bkz: [Tekdüzen tür tanımlayıcıları başvurusu](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) ve [programlama belge etkileşim konuları iOS için](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>İlgili bağlantılar

- [Görüntüleri (örnek) ile çalışma](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Özel bir simge ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
