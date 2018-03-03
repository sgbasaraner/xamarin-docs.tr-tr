---
title: "Android varlıkları kullanma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: bbf20155fe097f0229aa28c1f0d046cb3ef31a63
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="using-android-assets"></a>Android varlıkları kullanma

_Varlıklar_ metin, xml, yazı tipi, müzik ve video gibi isteğe bağlı dosyalar uygulamanıza eklemek için bir yol sağlar. Bu dosyaları "Kaynaklar" olarak eklemeye çalışırsanız, Android, kaynak sisteme işleyecek ve ham verileri almak mümkün olmaz. Veri dokunulmadan erişmek istiyorsanız, bunu yapmanın bir yolu varlıkları içerir.

Projenize eklenen varlıklar görünmesini sağlar yalnızca, uygulama kullanarak okuyabileceği bir dosya sistemi gibi [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/).
Bu basit gösteride kullanarak okuma metin dosyası varlık bizim projeye eklemek için olduğumuz `AssetManager`ve kutusu TextView içinde görüntüleyin.

<a name="Add_Asset_to_Project" />

## <a name="add-asset-to-project"></a>Varlık projeye ekleyin

Varlıklar Git `Assets` projenizin klasör. Yeni bir metin dosyası olarak adlandırılan bu klasöre eklemek `read_asset.txt`. "I bir varlık gelen!"gibi bazı metinleri içine yerleştirin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio ayarlamış **yapı eylemi** bu dosya için **AndroidAsset**:

![Derleme eylemi AndroidAsset için ayarlama](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio ayarlamış **yapı eylemi** bu dosya için **AndroidAsset**:

[![Derleme eylemi AndroidAsset için ayarlama](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png)

-----

Doğru seçme **BuildAction** dosyanın derleme zamanında APK paketlenirler sağlar.

<a name="Reading_Assets" />

## <a name="reading-assets"></a>Varlıklar okuma

Varlıklar kullanarak okunur bir [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/). Örneği `AssetManager` access tarafından kullanılabilir [varlıklar](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) özelliği bir `Android.Contet.Context`, bir etkinlik gibi.
Aşağıdaki kodda, biz açmak bizim **read_asset.txt** varlık, içeriği okumak ve bir kutusu TextView kullanarak görüntüleyin.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```

<a name="Running_the_Application" />

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulamayı çalıştırın ve aşağıdaki görmeniz gerekir:

![Örnek ekran görüntüsü](android-assets-images/screenshot.png)


## <a name="related-links"></a>İlgili bağlantılar

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [bağlam](https://developer.xamarin.com/api/type/Android.Content.Context/)
