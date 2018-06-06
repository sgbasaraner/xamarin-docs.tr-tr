---
title: MonoTouch.Dialog Json biçimlendirme
description: Bu belge MonoTouch.Dialog kullanarak bir Xamarin.iOS kullanıcı arabirimi oluşturmak için kullanılan JSON söz dizimi açıklanmıştır.
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dc3f4ea87bbd381a4a1767fb9179fb1bcf0c56d8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790763"
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json biçimlendirme

Bu sayfayı MonoTouch.Dialog tarafından ait kabul Json biçimlendirme açıklar [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

Bize bir örneğiyle başlatın. JsonElement aktarılabilecek tam bir Json dosyası verilmiştir.

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
                             "type": "string",
                             "caption": "Welcome to the nested controller"
                           }
                         ]
                       }
                     ]
                   }
          ]
      }, {
          "header": "Entries",
          "elements" : [
              {
                  "type": "entry",
                  "caption": "Username",
                  "value": "",
                  "placeholder": "Your account username"
              }
          ]
      }
  ]
}
```

Yukarıdaki biçimlendirme aşağıdaki UI üretir:

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "Belirtilen biçimlendirme tarafından oluşturulan kullanıcı Arabirimi")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

Her öğe ağacında özelliği içerebilir `"id"`. Bölümleri tek tek veya JsonElement Dizin Oluşturucu kullanarak öğeleri başvurmak için çalışma zamanında mümkündür. Böyle:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>Kök öğesi sözdizimi

Kök öğesi aşağıdaki değerleri içerir:

-  `title`
-  `sections` (isteğe bağlı)


Kök öğe bir bölüm içinde iç içe geçmiş denetleyicisi oluşturmak için bir öğe görünür. Bu durumda, ek özellik `"type"` ayarlanmalıdır `"root"`

 <a name="url" />


### <a name="url"></a>URL

Varsa `"url"` özelliği ayarlanmış, bu RootElement üzerinde kullanıcı dokunur varsa, kodu bir dosya belirtilen URL'den isteyecek ve içeriği görüntülenen yeni bilgiler hale getirir. Bu oluşturmak için kullanabileceğiniz ne kullanıcı dokunur tabanlı sunucudan kullanıcı arabirimini genişletir.

 <a name="group" />


### <a name="group"></a>grup

Ayarlama, bu kök öğesi için groupname ayarlar varsa. Grup adları öğesi içinde iç içe öğelerin birinden kök öğesinin değeri olarak gösterilen bir özeti almak için kullanılır. Bu, bir onay kutusu değeri veya radyo düğmesi değeri değil.

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

İç içe geçmiş öğe seçili radyo öğesi tanımlar

 <a name="title" />


### <a name="title"></a>Başlık

Varsa, RootElement için kullanılan başlık olacaktır

 <a name="type" />


### <a name="type"></a>türü

Ayarlanmalıdır `"root"` bu (Bu denetleyicileri yerleştirmek için kullanılır) bölümünde görüntülendiğinde.

 <a name="sections" />


### <a name="sections"></a>sections

Bu, tek tek bölümlü Json dizisidir

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>Bölümün sözdizimi

Bu bölüm içerir:

-  `header` (isteğe bağlı)
-  `footer` (isteğe bağlı)
-  `elements` Dizi


 <a name="header" />


### <a name="header"></a>üstbilgi

Varsa, üstbilgi metni bölümünün başlığı olarak görüntülenir.

 <a name="footer" />


### <a name="footer"></a>Altbilgi

Varsa, alt bölümünün altında görüntülenir.

 <a name="elements" />


### <a name="elements"></a>öğeler

Bu öğeleri dizisidir. Her öğe en az bir anahtar içermelidir `"type"` oluşturmak için öğe türünü tanımlamak için kullanılan anahtar.
Öğelerin bazıları gibi yaygın bazı özellikleri paylaşır `"caption"` ve `"value"`. Bu, desteklenen öğeleri listesi verilmiştir:

-  `string` öğeleri (her ikisi de ile ve stil olmadan)
-  `entry` satırları (normal veya parola)
-  `boolean` (anahtarlar veya görüntüleri kullanarak) değerler


Dize öğesi düğmeleri gibi bir yöntem sağlayarak kullanıcı hücrenin veya donatıyı dokunur gerektiğinde çağrılacak kullanılabilir,

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>Öğe işleme

Öğe işleme C# StringElement ve StyledStringElement temel alır ve çeşitli şekillerde bilgi hale getirebilir ve çeşitli şekillerde oluşturmak mümkündür. En basit öğelerini aşağıdaki gibi oluşturulabilir:

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

Bu tüm varsayılanları basit bir dizeyle gösterir: yazı tipi, arka plan, metin rengini ve dekorasyonlar. Bu öğelere Eylemler bağlanacağını ve ayarlayarak düğmeleri gibi davranır yapmanız da mümkündür `"ontap"` özelliği veya `"onaccessorytap"` özellikleri:

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

Yukarıdaki "Acme.PhotoLibrary" sınıftaki "ShowPhotos" yöntemi çağırır. `"onaccessorytap"` Benzer, ancak hücreyi dokunma yerine donatıyı üzerinde kullanıcı dokunur varsa yalnızca çağrılır. Bu ayarı etkinleştirmek için de donatıyı ayarlamanız gerekir:

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Öğelerin işlenmesi iki dizeyi tek seferde görüntüleyebilir, resim yazısını biridir ve başka bir değerdir. Bu dizeler işlendiğini nasıl stilini bağlıdır, bu kullanılarak ayarlanan `"style"` özelliği. Varsayılan başlık sol ve sağ taraftaki değeri gösterir. Daha fazla ayrıntı için stil bölümüne bakın. Renkleri kırmızı, yeşil, mavi ve belki de alfa değerleri değerlerini temsil eden onaltılık sayı arkasından '#' simgesini kullanarak kodlanır. İçeriği RGB veya RGBA değerlerini temsil eden kısa biçiminde (3 veya 4 onaltılık basamak) kodlanabilir. Ya da RGB veya RGBA değerlerini temsil eden uzun formunu (6 veya 8 basamak). Kısa aynı onaltılık basamak iki kez yazma için bir toplu özelliktir. "#1bc" sabiti kırmızı olarak ait olacak şekilde 0x11, yeşil = 0xbb = ve mavi 0xcc =. Alfa değeri mevcut değilse rengi donuk olur. Bazı örnekler:

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>Aksesuar

Gösterilmesi için işleme öğesinde olası değerler donatıyı türünü belirler:

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


Değer yoksa, hiçbir donatıyı gösterilir

 <a name="background" />


### <a name="background"></a>arka plan

Arka plan özelliği hücrenin arka plan rengini ayarlar. Ya da bir görüntü URL değeridir (Bu durumda, zaman uyumsuz resim Yükleyici'yi çağrılır ve arka plan görüntü yüklendikten sonra güncelleştirilir) veya renk sözdizimini kullanarak belirtilen renk olabilir.

 <a name="caption" />


### <a name="caption"></a>Açıklamalı alt yazı

İşleme öğede gösterilecek ana dize. Yazı tipi ve renk ayarlayarak özelleştirilebilir `"textcolor"` ve `"font"` özellikleri. İşleme stili tarafından belirlenen `"style"` özelliği.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>renk ve detailcolor

Ana metni veya ayrıntılı metni için kullanılacak rengi.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>detailfont ve yazı tipi

Resim yazısını veya ayrıntı metni için yazı tipi. Yazı tipi belirtimi ardından isteğe bağlı olarak bir tire ve nokta boyutunu yazı tipi adı biçimidir.
Geçerli yazı tipi özellikleri şunlardır:

-  "Helvetica"
-  "Helvetica-14"


 <a name="linebreak" />


### <a name="linebreak"></a>linebreak

Satırları nasıl ayrılır belirler. Olası değerler şunlardır:

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


Her ikisi de `character-wrap` ve `word-wrap` ile birlikte kullanılan `"lines"` işleme öğesi çok satırlı öğeye açmak için sıfır özellik kümesi.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>OnTap ve onaccessorytap

Bu özellikleri bir nesne bir parametre olarak alır, uygulamanızdaki bir statik yöntem adı işaret etmelidir. JsonDialog.FromFile veya JsonDialog.FromJson yöntemlerini kullanarak hiyerarşinizi oluşturduğunuzda, isteğe bağlı nesne değeri geçirebilirsiniz. Bu nesne değeri daha sonra yöntemlerinizi geçirilir. Bu, bazı bağlam static yönteminize geçirmek için kullanabilirsiniz. Örneğin:

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />


### <a name="lines"></a>satırlar

Bu sıfır olarak ayarlanırsa, öğesi otomatik boyutu bulunan dizeleri içeriğini bağlı olarak hale getirir. Bunun çalışması için de ayarlamalısınız `"linebreak"` özelliğine `"character-wrap"` veya `"word-wrap"`.

 <a name="style" />


### <a name="style"></a>stili

İçerik işlemek için kullanılan hücre stili tür stilini belirler ve bunlar UITableViewCellStyle numaralandırma değerlerine karşılık gelir.
Olası değerler şunlardır:

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : bir alt başlık metni.


 <a name="subtitle" />


### <a name="subtitle"></a>Alt Başlığı

Alt başlığı için kullanılacak bir değer. Stil kümesi için bir kısayol budur `"subtitle"` ve `"value"` bir dize özelliği.
Bu ikisi ile tek bir giriş yapar.

 <a name="textcolor" />


### <a name="textcolor"></a>TextColor

Metin için kullanılacak rengi.

 <a name="value" />


### <a name="value"></a>value

İşleme öğede gösterilecek ikincil değer. Bu düzenini etkilenir `"style"` ayarı. Yazı tipi ve renk ayarlayarak özelleştirilebilir `"detailfont"` ve `"detailcolor"`.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>Boole öğeleri

Boole öğeleri tür ayarlamalıdır `"bool"`, içerebilir bir `"caption"` görüntülenecek ve `"value"` true veya false değerine ayarlanır. Varsa `"on"` ve `"off"` özelliklerinin, görüntüleri olduğu varsayılır. Görüntüleri uygulamanın geçerli çalışma dizininde göre çözümlenir. Paket göreli dosyaları başvurmak istiyorsanız, kullanabileceğiniz `"~"` uygulama paket dizini göstermek için bir kısayol olarak. Örneğin `"~/favorite.png"` paket dosyasında yer alan favorite.png olacaktır. Örneğin:

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>türü

Türü ayarlanabilir olarak `"boolean"` veya `"checkbox"`. Boolean değerine ayarlayın, bir UISlider veya görüntüleri kullanıp kullanmayacağını (her iki `"on"` ve `"off"` ayarlanır). Onay kutusu ayarlanırsa, bu onay kutusu kullanıp kullanmayacağını. `"group"` Özelliği, belirli bir gruba ait olarak bir boolean öğesi etiketlemek için kullanılabilir. Bu içeren kök de varsa yararlı bir `"group"` özelliği kök olarak tüm Boole değerlerini (veya onay kutularını) sayısı ile aynı gruba ait sonuçları özetlemek.

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Giriş öğeleri

Giriş öğeleri veri girmesini izin vermek için kullanın. Giriş öğelerin ya da türüdür `"entry"` veya `"password"`. `"caption"` Özelliği ayarlanmış metni sağ tarafta göstermek için ve `"value"` giriş ayarlamak için başlangıç değerine ayarlanır. `"placeholder"` (Bunu gösterilir gri) boş girişler için kullanıcıya bir ipucu göstermek için kullanılır. Bazı örnekler şunlardır:

```csharp
{
        "type": "entry",
        "caption": "Username",
        "value": "",
        "placeholder": "Your account username"
}, {
        "type": "password",
        "caption": "Password",
        "value": "",
        "placeholder": "You password"
}, {
        "type": "entry",
        "caption": "Zip Code",
        "value": "01010",
        "placeholder": "your zip code",
        "keyboard": "numbers"
}, {
        "type": "entry",
        "return-key": "route",
        "caption": "Entry with 'route'",
        "placeholder": "captialization all + no corrections",
        "capitalization": "all",
        "autocorrect": "no"
}
```

 <a name="autocorrect" />


### <a name="autocorrect"></a>Otomatik Düzeltme

Girişini kullanmak için otomatik düzeltme stilini belirler. Olası değerler şunlardır: true veya false (veya dizelerin `"yes"` ve `"no"`).

 <a name="capitalization" />


### <a name="capitalization"></a>Büyük/küçük harf

Girişini kullanmak için büyük/küçük harf stili. Olası değerler şunlardır:

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>Açıklamalı alt yazı

Girişini kullanmak için açıklamalı alt yazı

 <a name="keyboard" />


### <a name="keyboard"></a>klavye

Veri girişi için kullanılacak klavye türü. Olası değerler şunlardır:

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>Yer tutucu

Giriş boş bir değer olduğunda gösterilen İpucu metni.

 <a name="return-key" />


### <a name="return-key"></a>return anahtar

Return anahtar için kullanılan etiket. Olası değerler şunlardır:

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>value

İlk değer girdisi

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>Radyo öğeleri

Radyo öğe türüne sahip `"radio"`. Seçili öğe tarafından çekilir `radioselected` içeren, kök öğesi özelliği.
Ayrıca, için bir değer ayarlarsanız `"group"` özellik, bu radyo düğmesi bu gruba ait.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>Tarih ve saat öğeleri

Öğe türleri `"datetime"`, `"date"` ve `"time"` kez tarihleri, tarihleri veya saatleri işlemek için kullanılır. Bu öğeleri parametre bir başlık ve bir değer alır. Değer .NET DateTime.Parse işlevi tarafından desteklenen herhangi bir biçimde yazılabilir. Örnek:

```csharp
"header": "Dates and Times",
"elements": [
        {
                "type": "datetime",
                "caption": "Date and Time",
                "value": "Sat, 01 Nov 2008 19:35:00 GMT"
        }, {
                "type": "date",
                "caption": "Date",
                "value": "10/10"
        }, {
                "type": "time",
                "caption": "Time",
                "value": "11:23"
                }                       
]
```

 <a name="Html/Web_Element" />


## <a name="htmlweb-element"></a>HTML/Web öğesi

Dokunduğunuz zaman, bir hücre oluşturabilirsiniz belirtilen bir URL'nin içeriğini işleyen bir UIWebView katıştırır yerel veya uzak kullanarak `"html"` türü. Bu öğe için yalnızca iki özellik `"caption"` ve `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
