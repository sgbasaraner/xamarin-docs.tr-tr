---
title: Xamarin.iOS alternatif uygulama simgeleri
description: Bu belge, alternatif uygulama simgeleri Xamarin.ios'ta kullanmayı açıklar. Bir Xamarin.iOS projesi için bu simgeler ekleme, Info.plist dosyasında değişiklik yapma ve uygulamanın simgesi programlı olarak yönetmek nasıl ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 3f009d84d1d38c379f65de52949c66f3e86fc654
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275995"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin.iOS alternatif uygulama simgeleri

_Bu makalede alternatif uygulama simgeleri Xamarin.ios'ta incelemektedir._

Apple bazı geliştirmeler simgesini yönetmek için uygulama izin veren iOS 10.3 eklemiştir:

 - `ApplicationIconBadgeNumber` -Alır veya içinde Springboard uygulama simgesine rozet ayarlar.
 - `SupportsAlternateIcons` -Eğer `true` simgeler alternatif bir dizi uygulama vardır.
 - `AlternateIconName` -Şu anda seçili alternatif simge adını döndürür veya `null` birincil simgesi kullanıyorsanız.
 - `SetAlternameIconName` -Belirli bir alternatif simge için uygulamanın simgesi geçiş yapmak için bu yöntemi kullanın.

![](alternate-app-icons-images/icons04.png "Uygulama simgesi değiştiğinde bir örnek uyarı")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Alternatif simgeleri için bir Xamarin.iOS projesi ekleme

Alternatif simge durumuna geçiş yapmak uygulama izin vermek için bir simge görüntü koleksiyonunu Xamarin.iOS uygulama projesinde eklenmesi gerekir. Bu görüntüler, tipik kullanılarak projeye eklenemez `Assets.xcassets` yöntemi, bunlar eklenmelidir **kaynakları** doğrudan klasör.

Aşağıdakileri yapın:

1. Gerekli simge görüntüleri bir klasör seçin, Tümünü Seç ve bunları sürükleyin **kaynakları** klasöründe **Çözüm Gezgini**:

    ![](alternate-app-icons-images/icons00.png "Simge görüntüleri bir klasör seçin")

2. Sorulduğunda, **kopyalama**, **aynı eylem seçilen tüm dosyaları için kullanan** tıklatıp **Tamam** düğmesi:

    ![](alternate-app-icons-images/icons02.png "Klasör iletişim kutusu için Dosya Ekle")

3. **Kaynakları** klasör tamamlandığında aşağıdaki gibi görünmelidir:

    ![](alternate-app-icons-images/icons01.png "Kaynaklar klasörünü şu şekilde görünmelidir.")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Info.plist dosyasını değiştirme

Eklenen gerekli görüntülerle **kaynakları** klasöründe [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) anahtar projenin eklenmesi gerekiyor **Info.plist** dosya. Bu anahtarın yeni simge hem de onu oluşturan görüntüleri adını tanımlar.

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift **Info.plist** dosyayı düzenlemek için açın.
2. Geçiş **kaynak** görünümü.
3. Ekleme bir **paket simgeleri** bırakın ve anahtar **türü** kümesine **sözlük**.
4. Ekleme bir `CFBundleAlternateIcons` ayarlayın ve anahtar **türü** için **sözlük**.
5. Ekleme bir `AppIcon2` ayarlayın ve anahtar **türü** için **sözlük**. Bu yeni alternatif uygulama simge kümesi adı olacaktır.
6. Ekleme bir `CFBundleIconFiles` ayarlayın ve anahtar **türü** için **dizi**
7. Yeni bir dize Ekle `CFBundleIconFiles` dizi her simge dosyasının uzantısı bırakarak ve `@2x`, `@3x`, vb. sonekleri (örnek `100_icon`). Alternatif simge Ayarla yaptığı her dosya için bu adımı yineleyin.
8. Ekleme bir `UIPrerenderedIcon` anahtarını `AppIcon2` sözlüğünü ayarlama **türü** için **Boole** ve değeri **Hayır**.
9. Değişiklikleri dosyaya kaydedin.

Ortaya çıkan **Info.plist** dosya tamamlandığında aşağıdaki gibi görünmelidir:

![](alternate-app-icons-images/icons03.png "Tamamlanan Info.plist dosyası")

Veya bu IF gibi bir metin Düzenleyicisi'nde açılır:

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

Xamarin.iOS projeye dahil simgesi görüntülerle ve **Info.plist** doğru yapılandırılmış dosya, geliştirici iOS 10.3 eklenen birçok yeni özelliklerden biri, uygulamanın simgesi denetlemek için kullanabilirsiniz.

`SupportsAlternateIcons` Özelliği `UIApplication` sınıfı, bir uygulama alternatif simgeleri destekleyip desteklemediğini görmek developer olanak tanır. Örneğin:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` Özelliği `UIApplication` Geliştirici almak veya içinde Springboard uygulama simgesine rozet geçerli sayısını ayarlamak sınıf sağlar. Sıfır (0) varsayılan değerdir. Örneğin:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` Özelliği `UIApplication` sınıfı, şu anda seçili alternatif uygulama simgenin adını almak Geliştirici olanak tanır veya üretebiliyorsa `null` birincil simgesi uygulaması kullanıyorsanız. Örneğin:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` Özelliği `UIApplication` sınıfı, uygulama simgesine değiştirmek Geliştirici olanak tanır. Seçilecek simgenin adını geçirin veya `null` birincil simgeyi dönün. Örneğin:

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

Uygulamayı çalıştırın ve kullanıcı alternatif bir simgeyi seçtiğinizde, aşağıdaki gibi bir uyarı görüntülenir:

![](alternate-app-icons-images/icons04.png "Uygulama simgesi değiştiğinde bir örnek uyarı")

Birincil simgeyi kullanıcı anahtarları, aşağıdakine benzer bir uyarı görüntülenir:

![](alternate-app-icons-images/icons05.png "Uygulama birincil simgeyi değiştiğinde bir örnek uyarı")

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS projesi için alternatif uygulama simgeleri ekleme ve bunları kullanarak uygulama kapsamına.



## <a name="related-links"></a>İlgili bağlantılar

- [iOSTenThree örnek](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
