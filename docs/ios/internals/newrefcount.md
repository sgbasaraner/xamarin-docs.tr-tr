---
title: "Yeni başvuru sayım sistemi"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 723a9c4a052f7f432ba0f32ec501af3221b2696f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="new-reference-counting-system"></a>Yeni başvuru sayım sistemi

Xamarin.iOS 9.2.1 Gelişmiş başvuru sistemi tüm uygulamalar için varsayılan olarak sayım sunmuştur. İzleme ve Xamarin.iOS önceki sürümlerinde düzeltme zor birçok bellek sorunlarını ortadan kaldırmak için kullanılabilir.

## <a name="enabling-the-new-reference-counting-system"></a>Yeni başvuru sistemi sayım etkinleştirme

Xamarin 9.2.1 itibariyle sistem sayım yeni ref etkinleştirilmiş **tüm** uygulamalar varsayılan olarak.

Varolan bir uygulama geliştiriyorsanız, emin olmak için .csproj dosyasını kontrol edebilirsiniz tüm oluşumlarını `MtouchUseRefCounting` ayarlanır `true`gibi altında:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Bu ayarlanırsa `false` uygulamanızı yeni araçları kullanarak değil.

### <a name="using-older-versions-of-xamarin"></a>Xamarin'ın eski sürümlerini kullanan

Xamarin.iOS 7.2.1 ve yukarıdaki bizim yeni başvuru sistemi sayım Gelişmiş önizlemesini özellikleri.

**Klasik API:**

Bu yeni başvuru sistemi sayım etkinleştirmek için denetleyin **uzantısı sayım başvuru kullanın** onay kutusu bulunan **Gelişmiş** projenizin sekmesinde **iOS derleme seçenekleri** , aşağıda gösterildiği gibi: 

[![](newrefcount-images/image1.png "Yeni başvuru sayım Sistemi'ni etkinleştir")](newrefcount-images/image1.png#lightbox)

Bu seçenekler Mac için Visual Studio'nun daha yeni sürümleri kaldırılmış unutmayın

 **[Unified API:](~/cross-platform/macios/unified/index.md)**

 Yeni başvuru uzantısı sayım Unified API için gereklidir ve varsayılan olarak etkinleştirilmiş olmalıdır. Eski sürümleri, IDE, bu değeri otomatik olarak teslim olmayabilir ve tarafından kendiniz işaretleyin gerekebilir.

    
> [!IMPORTANT]
> MonoTouch 5.2 ancak yalnızca için kullanılabilir olduğundan, bu özellik önceki bir sürümünü geçici bırakıldı **sgen** Deneysel önizleme olarak. Bu yeni ve geliştirilmiş sürümünü artık de kullanılabilir **Boehm** atık toplayıcı.


Geçmişte olmuştur Xamarin.iOS tarafından yönetilen nesneleri iki tür: ek bellek durumu tutarak yalnızca yerel bir nesne (eş nesneler) ve genişletilmiş ya da yeni işlevsellik (türetilen nesneler) - birleştirilmiş çevresinde bir sarmalayıcı genellikle olan. Daha önce mümkün biz durumu ile bir eş nesnesi (örneğin bir C# olay işleyicisi ekleyerek) büyütmek, ancak biz başvurulmayan ve ardından toplanan Git nesne olanak verir. Bu daha sonra neden bir kilitlenme olabilir (örneğin Objective-C çalışma zamanı yönetilen nesnesine geri çağırdı varsa).

Yeni Sistem eş nesne herhangi bir ek bilgi depoladığınızda çalışma zamanı tarafından yönetilen nesneleri otomatik olarak yükseltir.

Bu gibi durumlarda oldu çeşitli kilitlenme çözer:

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

Başvuru sayısı uzantısı olmadan, çünkü bu kodu kilitlenme `cell` collectible, olur ve bu nedenle, `TouchDown` temsilci, hangi sallantıda işaretçi çevirir.

Yerel nesne yerel kod tarafından korunur sağlanan başvuru sayısı uzantısı Yönetilen Nesne Canlı kalır ve onun koleksiyonu engeller sağlar.

Yeni Sistem ayrıca gereksinimini ortadan kaldırır *çoğu* özel varsayılan yaklaşımına örneğini Canlı olan bağlama - kullanılan alanları yedekleme. Yönetilen bağlayıcı tüm bunları kaldırmak akıllı *gereksiz* yeni başvuru kullanan uygulamalar alanlardan uzantısı sayısı.

Bu, her yönetilen nesne örneklerini daha az bellek önce tüketen anlamına gelir. Ayrıca, bazı yedekleme alanları artık bazı belleği geri zor hale getirme Objective-C çalışma zamanı tarafından gerekmeyen başvuruları burada tutan ilgili bir sorunu çözer.
