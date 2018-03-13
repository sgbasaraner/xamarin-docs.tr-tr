---
title: "Android erişilebilirliği"
ms.topic: article
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 2dea77b4c52db0c032aba9bde471e76eb36ba3ad
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="accessibility-on-android"></a>Android erişilebilirliği

Bu sayfayı göre uygulamalar oluşturmak için Android erişilebilirlik API'ları kullanmayı açıklar [erişilebilirlik denetim listesi](~/cross-platform/app-fundamentals/accessibility.md).
Başvurmak [iOS erişilebilirlik](~/ios/app-fundamentals/accessibility.md) ve [OS X erişilebilirlik](~/mac/app-fundamentals/accessibility.md) diğer platform API'leri için sayfa.


## <a name="describing-ui-elements"></a>Kullanıcı Arabirimi öğeleri açıklayan

Android sağlayan bir `ContentDescription` ekran API'leri okuma tarafından erişilebilir bir denetimin amacı açıklaması sağlamak için kullanılan özellik.

İçerik açıklama ya da C# veya AXML düzeni dosyasında ayarlayabilirsiniz.

**C#**

Açıklama herhangi bir dize (veya bir dize kaynağı) kodda ayarlayabilirsiniz:

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML düzeni**

XML'de düzenleri kullanmanız `android:contentDescription` özniteliği:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>İpucu kutusu TextView için kullanın

İçin `EditText` ve `TextView` veri girişi için denetimleri kullanın `Hint` özelliği hangi giriş beklenen bir açıklama sağlayın (yerine `ContentDescription`).
Bazı metinleri girildiğinde metin "yerine ipucu okunacak".

**C#**

Ayarlama `Hint` özelliğinin kodda:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML düzeni**

XML'de düzeni dosyaları kullanmanız `android:hint` özniteliği:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>Giriş alanları etiketlerle LabelFor bağlantılar

Bir etiketi veri giriş denetimi ile ilişkilendirmek için kullanmak `LabelFor` özelliği

**C#**

C# ' ta ayarlamak `LabelFor` bu bu içerik denetimi kaynak kimliği özelliği açıklar (genellikle bu özellik üzerindeki bir etikette ayarlanır ve başka bir giriş denetiminin başvuran):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML düzeni**

XML kullanım düzeninde `android:labelFor` başka bir denetimin tanımlayıcısını başvurmak için özellik:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>İçin erişilebilirlik Duyurusu

Kullanım `AnnounceForAccessibility` herhangi bir yöntemini Erişilebilirlik etkinleştirildiğinde, bir olay ya da durum değişiklik kullanıcılara iletişim kurmak için Denetim görüntüleyin. Bu yöntem, burada yerleşik Konuşmayı yeterli geri bildirim sağlar, ancak burada ek bilgi kullanıcı için yararlı olacaktır kullanılmalıdır işlemlerinin çoğu için gerekli değildir.

Aşağıdaki kod, bir basit bir örnek arama gösterir `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Odağı ayarlarını değiştirme

Kullanıcının hangi işlemleri kullanılabilir anlaşılmasına yardımcı olmak için odak sahip denetimlerinde erişilebilir Gezinti kullanır. Android sağlayan bir `Focusable` denetimleri sırasında Gezinti odaklanması özellikle yapabileceksiniz olarak etiketleyebilirsiniz özelliği.

**C#**

Denetim odağı C# ile kazanmasını önlemek için ayarlanmış `Focusable` özelliğine `false`:

```csharp
label.Focusable = false;
```

**AXML düzeni**

Düzende kümesi XML dosyaları `android:focusable` özniteliği:

```xml
<android:focusable="false" />
```

Odağı siparişle kontrol edebilirsiniz `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` öznitelikleri, genellikle AXML düzende ayarlayın. Kullanıcı denetimleri ekranında aracılığıyla kolayca gidebilirsiniz emin olmak için bu öznitelikler kullanın.


## <a name="accessibility-and-localization"></a>Erişilebilirlik ve yerelleştirme

İpucu ve içerik açıklama olan Yukarıdaki örneklerde doğrudan görüntü değerine ayarlayın. Değerleri kullanmak için tercih edilir bir **Strings.xml** bu gibi dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

Metin dizelerini dosyasından kullanarak C# ve AXML düzeni dosyaları aşağıda verilmiştir:

**C#**

Dize değişmez değerleri kodda kullanmak yerine, çevrilmiş değerleri dizeler dosyalarını arayın `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

Düzen XML erişilebilirlik öznitelikleri ister `hint` ve `contentDescription` bir dize tanımlayıcı ayarlayabilirsiniz:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

Metin ayrı bir dosyaya depolayarak faydası, dosyanın birden çok dil çevirileri uygulamanızda sağlanabilir ' dir. Bkz: [Android yerelleştirme Kılavuzu](~/android/app-fundamentals/localization.md) öğrenmek için nasıl bir uygulama projesine yerelleştirilmiş dize dosyaları ekleyin.


## <a name="testing-accessibility"></a>Erişilebilirlik test etme

İzleyin [adımları](http://developer.android.com/training/accessibility/testing.html#how-to) TalkBack ve Araştır Touch tarafından erişilebilirlik Android cihazlarda test etmek etkinleştirmek için.

Yüklemeniz gerekebilir [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) içinde görünmüyorsa, Google play'den **ayarlar > Erişilebilirlik**.


## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası erişilebilirlik](~/cross-platform/app-fundamentals/accessibility.md)
- [Android erişilebilirlik API'leri](http://developer.android.com/guide/topics/ui/accessibility/index.html)
