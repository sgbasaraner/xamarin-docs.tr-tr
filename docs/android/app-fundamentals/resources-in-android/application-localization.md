---
title: "Uygulama yerelleştirme ve dize kaynakları"
ms.topic: article
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: a8d25d8780a62e54780d7aa03d81f89fa0f668a4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="application-localization-and-string-resources"></a>Uygulama yerelleştirme ve dize kaynakları

Uygulama yerelleştirme belirli bir bölge veya yerel ayar hedeflemek için alternatif kaynakları sağlama işlemidir. Örneğin, çeşitli ülkeler için yerelleştirilmiş dil dizeleri sağlayabilir veya renkleri veya belirli kültürler eşleşecek şekilde düzenini değiştirebilirsiniz. Android yükleyin ve kaynak koduna herhangi bir değişiklik yapılmadan çalışma zamanında cihazın yerel ayarlar için uygun kaynakları kullanın.

Örneğin, aşağıdaki resimde üç farklı cihaz yerlerde aynı uygulamayı gösterir, ancak her düğmesini görüntülenen metin her bir cihaz kümesine yerel özeldir:

[![Üç farklı yerel ayarlara örnekleri](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

Bu örnekte, bir düzen dosyasının içeriğini **Main.axml** şuna benzer:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

Yukarıdaki örnekte, dize düğmesi için kaynak kimliği dizesi sağlayarak kaynaklardan yüklendi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Üç diller için kaynak dizeleri](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Üç diller için kaynak dizeleri](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Android uygulamalarını yerelleştirme

Okuma [yerelleştirme giriş](~/cross-platform/app-fundamentals/localization.md) ipuçları ve mobil uygulamaları yerelleştirme konusunda yönergeler için.

[Android uygulamalarını yerelleştirme](~/android/app-fundamentals/localization.md) Kılavuzu dizeleri Çevir ve Xamarin.Android kullanarak görüntüleri yerelleştirme hakkında daha ayrıntılı örnekler içerir.



## <a name="related-links"></a>İlgili bağlantılar

- [Android uygulamalarını yerelleştirme](~/android/app-fundamentals/localization.md)
- [Platformlar arası yerelleştirme genel bakış](~/cross-platform/app-fundamentals/localization.md)
