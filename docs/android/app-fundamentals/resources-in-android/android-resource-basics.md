---
title: Android kaynakları temel bilgileri
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 207644f5a5d3d346214ba090dcd450e55fde2657
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241324"
---
# <a name="android-resource-basics"></a>Android kaynakları temel bilgileri

Neredeyse tüm Android uygulamaları kaynakları çeşit bunları erişebilir. en az bunlar genellikle kullanıcı arabirimi düzenleri dosyaları XML biçiminde sahiptir. Bir Xamarin.Android uygulaması ilk oluşturulduğunda varsayılan kaynaklar Xamarin.Android projesi şablonu tarafından Kurulum şunlardır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kaynak dosyaları](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Kaynak dosyaları](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Varsayılan kaynakları beş dosyaları kaynaklar klasörüne oluşturulmuştur:

-  **Icon.PNG** &ndash; uygulama varsayılan simgesi

-  **Main.AXML** &ndash; bir uygulama için varsayılan kullanıcı arabirimi Düzen dosyası. Kullanan Android sırasında Not **.xml** Xamarin.Android dosya uzantısı kullanan **.axml** dosya uzantısı.

-  **Strings.xml** &ndash; uygulama yerelleştirmesi ile yardımcı olmak için bir dize tablosu

-  **AboutResources.txt** &ndash; bu gerekli değildir ve güvenli bir şekilde silinebilir. Yalnızca, kaynak klasör ve içindeki dosyaların üst düzey bir genel bakış sağlar.

-  **Resource.Designer.cs** &ndash; bu dosya otomatik olarak oluşturulan ve Xamarin.Android ve ayrı tutma benzersiz tarafından tutulan kimliğin her kaynağa atanmış. Bu, çok benzer ve aynı amaca R.java dosyanın Java dilinde yazılmış bir Android uygulaması gerekir. Xamarin.Android araçları tarafından otomatik olarak oluşturulur ve zaman zaman yeniden oluşturulacak.


## <a name="creating-and-accessing-resources"></a>Oluşturma ve kaynaklara erişme

Kaynakları oluşturma, dosyaları söz konusu kaynak türü için dizine eklemek kadar kolaydır. Aşağıdaki ekran görüntüsünde, Alman yerel ayarlar için dize kaynakları bir projeye eklenen gösterir. Zaman **Strings.xml** dosyasına eklendi **derleme eylemi** otomatik olarak ayarlandığı **AndroidResource** Xamarin.Android araçları tarafından:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Derleme eylemi Strings.xml AndroidResource için ayarlamak için](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Derleme eylemi Strings.xml AndroidResource için ayarlamak için](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

Bu, düzgün bir şekilde derleyin ve kaynakları APK dosyası için ekleme için Xamarin.Android araçlar sağlar. Eğer herhangi bir nedenle **derleme eylemi** ayarlı değil **Android kaynak**, ardından apk'yı dosyalar tutulur ve yüklemek veya kaynaklara erişmek için her türlü girişim bir çalışma zamanı hatasına neden ve Uygulama kilitlenir.

Ayrıca, Android için kaynak öğeleri yalnızca küçük harf dosya adlarını desteklese de, Xamarin.Android biraz daha fazla forgiving olduğuna dikkat edin önemlidir; Bu, büyük ve küçük harflerin destekleyecektir. Görüntü adları alt çizgi ile küçük ayırıcı olarak kullanılacak kuraldır (örneğin, **my\_görüntü\_name.png**). Ayırıcı olarak tire veya boşluk kullandıysanız kaynak adları işlenemiyor unutmayın.

Kaynaklar için bir proje eklendikten sonra bunların bir uygulamada kullanmak için iki yolu vardır &ndash; program aracılığıyla (içinde kod) veya XML dosyaları.


## <a name="referencing-resources-programmatically"></a>Program aracılığıyla başvuru kaynakları

Bu dosyaları program aracılığıyla erişmek için bunlar benzersiz kaynak kimliği atanır. Bu kaynak kimliği olarak adlandırılan özel bir sınıf içinde tanımlanan bir tamsayıdır `Resource`, dosyada bulunan **Resource.designer.cs**, ve şuna benzer:

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

Her kaynak Kimliğinin kaynak türü için karşılık gelen bir iç içe geçmiş bir sınıf içinde yer alır. Örneğin, dosyayı **Icon.png** eklenen projeye Xamarin.Android güncelleştirilmiş `Resource` iç içe geçmiş bir sınıf oluşturma sınıfı, adlandırılan `Drawable` ile sabit bir iç adlı `Icon`.
Böylece, dosyanın **Icon.png** kod başvurulacak `Resource.Drawable.Icon`. `Resource` Sınıfı değil el ile düzenlenmesi gerekir, Xamarin.Android tarafından kendisine yapılan değişikliklerin üzerine yazılacak şekilde.

Program aracılığıyla (kodda kaynakları) başvururken aşağıdaki söz dizimini kullanan kaynakları sınıf hiyerarşisi erişilebilir:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash; kaynak sağlama ve yalnızca paketin ne zaman gerekli kaynakları diğer paketlerden kullanılmaktadır.

-  **ResourceType** &ndash; yukarıda açıklanan kaynak sınıfı içinde iç içe kaynak türü budur.

-  **Kaynak adı** &ndash; (uzantısı olmadan) kaynağının dosya adı veya bir XML öğesi olan kaynaklar için android: name özniteliğinin değerini budur.


## <a name="referencing-resources-from-xml"></a>XML'den başvuru kaynakları

Kaynakları bir XML dosyasında aşağıdaki tarafından erişilen özel bir sözdizimi:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **PackageName** &ndash; kaynak sağlama ve yalnızca paketin ne zaman gerekli kaynakları diğer paketlerden kullanılmaktadır.

-  **ResourceType** &ndash; kaynak sınıfı içinde iç içe kaynak türü budur.

-  **Kaynak adı** &ndash; bu dosya kaynak adını, (*olmadan* dosya türü uzantısı) ya da değerini `android:name` bir XML öğesi olan kaynaklar için özniteliği.

Örneğin bir düzen dosyası içeriğini **Main.axml**, aşağıdaki gibidir:

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

Bu örneğin bir [ `ImageView` ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) adlı drawable kaynak gerektiren **bayrağı**. `ImageView` Sahip kendi `src` özniteliğini **@drawable/flag**. Android etkinliği başlatıldığında, dizinin içinde görüneceğini **kaynak/Drawable** adlı bir dosya için **flag.png** (dosya uzantısı gibi başka bir görüntü biçimi olabilir **flag.jpg**) Bu dosyayı yüklemek ve görüntüleyin `ImageView`.
Bu uygulamayı çalıştırdığınızda, aşağıdaki görüntüde şöyle görünür:

![Yerelleştirilmiş ImageView](android-resource-basics-images/03-localized-screenshot.png)

