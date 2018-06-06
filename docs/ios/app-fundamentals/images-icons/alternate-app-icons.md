---
title: Xamarin.iOS diğer uygulama simgeleri
description: Bu belge, diğer uygulama simgeleri Xamarin.iOS kullanın açıklar. Bir Xamarin.iOS projesi için bu simgeleri ekleme, Info.plist dosyasının nasıl değiştirileceğini ve uygulamanın simgesi programlı olarak yönetmek nasıl açıklanır.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 1d37a29982454367c35bfdfad205abce0eb025af
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784116"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin.iOS diğer uygulama simgeleri

_Bu makalede, alternatif uygulama simgeleri Xamarin.iOS kullanarak yer almaktadır._

Apple bazı geliştirmeler simgesini yönetmek bir uygulama sağlayan iOS 10.3 ekledi:

 - `ApplicationIconBadgeNumber` -Alır veya uygulama simgesini rozet Springboard ayarlar.
 - `SupportsAlternateIcons` -İf `true` simgeleri alternatif bir dizi uygulama vardır.
 - `AlternateIconName` -Şu anda seçili alternatif simgenin adını döndürür veya `null` birincil simgesi kullanıyorsanız.
 - `SetAlternameIconName` -Uygulamanın simgesi verilen alternatif simgesi geçiş yapmak için bu yöntemi kullanın.

![](alternate-app-icons-images/icons04.png "Bir uygulama simgesini değiştiğinde bir örnek Uyarısı")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Bir Xamarin.iOS projesi alternatif simgeler ekleme

Alternatif simge durumuna geçiş yapmak uygulama izin vermek için Xamarin.iOS uygulaması projeye dahil edilecek simge görüntüleri koleksiyonu gerekir. Bu görüntüleri tipik kullanarak proje eklenemiyor `Assets.xcassets` yöntemi, bunlar eklenmelidir **kaynakları** doğrudan klasör.

Aşağıdakileri yapın:

1. Gerekli simge görüntüleri bir klasör seçin, tümünü seçin ve bunlara sürükleyin **kaynakları** klasöründe **Çözüm Gezgini**:

    ![](alternate-app-icons-images/icons00.png "Simgeler görüntüleri bir klasör seçin")

2. İstendiğinde, seçin **kopya**, **aynı eylem seçilen tüm dosyaları için kullanan** tıklatıp **Tamam** düğmesi:

    ![](alternate-app-icons-images/icons02.png "Klasör iletişim kutusu için Dosya Ekle")

3. **Kaynakları** klasörü tamamlandığında aşağıdaki gibi görünmelidir:

    ![](alternate-app-icons-images/icons01.png "Kaynaklar klasörünü aşağıdaki gibi görünmelidir.")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Info.plist dosyasını değiştirme

Eklenen gerekli görüntülerle **kaynakları** klasörünü [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) anahtar projenin eklenmesi gerekir **Info.plist** dosya. Bu anahtar adını yeni simgesine ve onu oluşturan görüntüleri tanımlayacaksınız.

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift **Info.plist** dosyayı düzenlemek için açın.
2. Geçiş **kaynak** görünümü.
3. Ekleme bir **paketini simgeler** anahtarı ve bırakın **türü** kümesine **sözlük**.
4. Ekleme bir `CFBundleAlternateIcons` anahtarı ve ayarlama **türü** için **sözlük**.
5. Ekleme bir `AppIcons2` anahtarı ve ayarlama **türü** için **sözlük**. Bu yeni alternatif uygulama simgesi kümesinin adı olacaktır.
6. Ekleme bir `CFBundleIconFiles` anahtarı ve ayarlama **türü** için **dizisi**
7. Yeni bir dize eklemek `CFBundleIconFiles` dizi uzantısı bırakarak her simge dosyası için ve `@2x`, `@3x`, vb. sonekleri (örneğin `100_icon`). Alternatif simgesi ayarlama yapar her dosya için bu adımı yineleyin.
8. Ekleme bir `UIPrerenderedIcon` anahtarını `AppIcons2` sözlüğünü ayarlama **türü** için **Boolean** ve değeri **Hayır**.
9. Değişiklikleri dosyaya kaydedin.

Elde edilen **Info.plist** dosya tamamlandığında aşağıdaki gibi görünmelidir:

![](alternate-app-icons-images/icons03.png "Tamamlanan Info.plist dosyası")

Ya da bu IF gibi bir metin düzenleyicisinde açıldığında:

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>Uygulamanın simgesi yönetme 

Xamarin.iOS projeye dahil simgesi görüntülerle ve **Info.plist** doğru yapılandırılmış bir dosya, geliştirici birini kullanabilirsiniz iOS 10.3 eklenen birçok yeni özellik uygulamanın simgesi denetlemek için.

`SupportsAlternateIcons` Özelliği `UIApplication` sınıfı, bir uygulama alternatif simgeler destekleyip desteklemediğini görmek Geliştirici olanak tanır. Örneğin:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` Özelliği `UIApplication` sınıfı, almak veya Springboard uygulama simgesini geçerli rozet sayısını ayarlamak Geliştirici olanak tanır. Sıfır (0) varsayılan değerdir. Örneğin:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` Özelliği `UIApplication` sınıfı şu anda seçili alternatif uygulama simgenin adını almak Geliştirici izin verir veya bunu döndürür `null` uygulaması birincil simgesi kullanıyorsanız. Örneğin:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` Özelliği `UIApplication` sınıfı, uygulama simgesini değiştirmek Geliştirici olanak tanır. Seçmek için simgeye adını geçirmek veya `null` birincil simgesi dönün. Örneğin:

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

Uygulamayı çalıştırın ve kullanıcı alternatif bir simge seçtiğinizde, aşağıdaki gibi bir uyarı görüntülenir:

![](alternate-app-icons-images/icons04.png "Bir uygulama simgesini değiştiğinde bir örnek Uyarısı")

Kullanıcının birincil simgesine geçerse, aşağıdakine benzer bir uyarı görüntülenir:

![](alternate-app-icons-images/icons05.png "Bir uygulama için birincil simgesi değiştiğinde bir örnek Uyarısı")

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS projesi alternatif uygulama simgeleri ekleme ve uygulama içinde kullanarak ele.



## <a name="related-links"></a>İlgili bağlantılar

- [iOSTenThree örnek](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
