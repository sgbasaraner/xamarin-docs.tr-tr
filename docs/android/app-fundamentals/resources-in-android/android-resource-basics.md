---
title: Android kaynak temelleri
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: f6be1001e5d3455a94e677f1bb5dc52ca574b873
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764898"
---
# <a name="android-resource-basics"></a>Android kaynak temelleri

Neredeyse tüm Android uygulamaları, bunları kaynakları çeşit sahip olur; en azından genellikle kullanıcı arabirimi düzenleri XML dosyaları biçiminde sahiptirler. Bir Xamarin.Android uygulaması ilk oluşturulduğunda varsayılan kaynaklar Kurulumu Xamarin.Android proje şablonu tarafından şunlardır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kaynak dosyaları](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Kaynak dosyaları](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Varsayılan kaynakları beş dosyaların kaynaklar klasörüne oluşturuldu:

-  **Icon.PNG** &ndash; uygulama varsayılan simgesi

-  **Main.AXML** &ndash; bir uygulama için varsayılan kullanıcı arabirimi Düzen dosyası. Kullanan Android çalışırken Not **.xml** Xamarin.Android dosya uzantısını kullanır **.axml** dosya uzantısı.

-  **Strings.xml** &ndash; uygulama yerelleştirme ile yardımcı olmak için bir dize tablosu

-  **AboutResources.txt** &ndash; bu gerekli değildir ve güvenle silinebilir. Yalnızca, kaynak klasörü ve içindeki dosyaların üst düzey bir genel bakış sağlar.

-  **Resource.Designer.cs** &ndash; bu dosyayı otomatik olarak oluşturulur ve Xamarin.Android ve ayrı tutma benzersiz tarafından tutulan kimliğin her kaynağa atanmış. Bu çok benzer ve aynı amaca Java'da yazılmış bir Android uygulaması olurdu R.java dosya adıdır. Xamarin.Android araçları tarafından otomatik olarak oluşturulur ve zaman zaman yeniden oluşturulacak.


## <a name="creating-and-accessing-resources"></a>Oluşturma ve kaynaklara erişme

Kaynakları oluşturma, söz konusu kaynak türü için dizine dosya ekleme olarak kadar basittir. Aşağıdaki ekran Almanca yerel ayarlar için dize kaynaklarını projeye eklenen gösterir. Zaman **Strings.xml** dosyasına eklenmişse **yapı eylemi** otomatik olarak ayarlandı **AndroidResource** Xamarin.Android araçları tarafından:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Derleme eylemi Strings.xml için AndroidResource ayarlamak için](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Derleme eylemi Strings.xml için AndroidResource ayarlamak için](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

Bu, düzgün şekilde derleyip kaynakları APK dosyasına katıştırma Xamarin.Android araçlar sağlar. Eğer herhangi bir nedenle **yapı eylemi** ayarlanmazsa **Android kaynak**, dosyalar APK sonra dışlanır ve yükleme veya kaynaklara erişmek için her türlü girişim çalışma zamanı hatası ile sonuçlanır ve uygulama kilitleniyor.

Ayrıca, Android yalnızca küçük harf dosya adları için kaynak öğeleri desteklerken, Xamarin.Android biraz daha forgiving olduğunu dikkate almak önemlidir; büyük ve küçük dosya adları destekleyecektir. Resim adları için kuralı küçük alt çizgi ile ayırıcı olarak kullanmaktır (örneğin, **my\_görüntü\_name.png**). Tire veya boşluk ayırıcı olarak kullanılıyorsa, kaynak adları işlenemiyor unutmayın.

Kaynakları projeye eklendikten sonra bir uygulamada kullanmak için iki yolu vardır &ndash; program aracılığıyla (içindeki kodu) veya XML dosyalarından.


## <a name="referencing-resources-programmatically"></a>Program aracılığıyla başvuru kaynakları

Bu dosyaları programlı olarak erişmek için bunlar benzersiz kaynak kimliği atanır. Bu kaynak kimliği adlı özel bir sınıf içinde tanımlanan bir tamsayıdır `Resource`, dosyada bulunan **Resource.designer.cs**, ve şuna benzer:

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

Her kaynak Kimliğinin kaynak türü için karşılık gelen bir iç içe geçmiş sınıf içinde yer alır. Örneğin, dosyayı **Icon.png** eklenen projeye Xamarin.Android güncelleştirilmiş `Resource` sınıfı, iç içe bir sınıf oluşturma adlı `Drawable` ile sabit bir iç adlı `Icon`.
Bu dosyayı sağlar **Icon.png** kod başvurulabilir için `Resource.Drawable.Icon`. `Resource` Sınıfı değil el ile düzenlenmesi gerekir, üzerinde yapılan değişiklikleri Xamarin.Android tarafından üzerine yazılacak şekilde.

Program aracılığıyla (kodda kaynakları) başvururken aşağıdaki söz dizimini kullanan kaynaklar sınıf hiyerarşisi erişilebilir:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash; kaynak sağlama ve yalnızca paketi ne zaman gerekli diğer paketleri kaynaklardan kullanılmaktadır.

-  **ResourceType** &ndash; yukarıda açıklanan kaynak sınıfı içinde iç içe kaynak türü budur.

-  **Kaynak adı** &ndash; (uzantısı olmadan) kaynak dosya adı veya bir XML öğesi olan kaynaklar için android: name özniteliği değeri budur.


## <a name="referencing-resources-from-xml"></a>XML'den başvuru kaynakları

Kaynakları bir XML dosyasında aşağıdaki tarafından erişilen özel bir sözdizimi:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **PackageName** &ndash; kaynak sağlama ve yalnızca paketi ne zaman gerekli diğer paketleri kaynaklardan kullanılmaktadır.

-  **ResourceType** &ndash; kaynak sınıfı içinde iç içe kaynak türü budur.

-  **Kaynak adı** &ndash; bu kaynağın dosya adıdır (*olmadan* dosya türü uzantısı) veya değerini `android:name` özniteliği için bir XML öğesi olan kaynaklar.

Örneğin bir düzen dosyanın içeriğini **Main.axml**, aşağıdaki gibidir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

Bu örnek sahip bir [ `ImageView` ](https://developer.xamarin.com/recipes/android/controls/imageview) adlı drawable bir kaynağı gerektiren **bayrağı**. `ImageView` Sahip kendi `src` özniteliğini **@drawable/flag**. Etkinlik başladığında, Android dizini içinde görüneceğini **kaynak/Drawable** adlı bir dosya için **flag.png** (dosya uzantısı gibi başka bir görüntü biçimi olabilir **flag.jpg**) Bu dosyayı yüklemek ve içinde görüntülemek `ImageView`.
Bu uygulamayı çalıştırdığınızda, aşağıdaki resimde şöyle görünür:

![Yerelleştirilmiş ImageView](android-resource-basics-images/03-localized-screenshot.png)

