---
title: MAPS uygulama
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2017
ms.openlocfilehash: b94c65c079b28fe042a42ec04357c11f3516d205
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="maps-application"></a>MAPS uygulama

Aşağıda gösterilen yerleşik eşlemeleri uygulama yararlanmak için Xamarin.Android eşlemelerinin çalışmak için en basit yolu verilmiştir:

[![Örnek uygulamasının ekran görüntüsü yerleşik Google haritalar](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Harita eşlemeleri uygulama kullandığınızda, uygulamanızın parçası olmaz. Bunun yerine, uygulamanız eşlemeleri uygulamasını başlatma ve eşlemi dışarıdan yükleme. Sonraki bölümde Xamarin.Android yukarıdaki benzer eşlemeleri başlatmak için nasıl kullanılacağını inceler.


## <a name="creating-the-intent"></a>Amacı oluşturma

Maps ile çalışma uygulama amacına uygun bir URI ile oluşturma, ActionView için ayar eylemi ve StartActivity yöntemi çağırma olarak kadar kolaydır. Örneğin, aşağıdaki kod bir verilen enlem ve boylam ortalanmış eşlemeleri uygulamasını başlatır:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Bu kod, önceki ekran görüntüsünde gösterildiği harita başlatmak için gereken şeydir. Enlem ve boylam belirtmeye ek olarak birçok seçenek eşlemeleri için URI şeması destekler.


## <a name="geo-uri-scheme"></a>Coğrafi URI düzeni

Yukarıdaki kod coğrafi düzeni bir URI oluşturmak için kullanılır. Bu URI düzeni, aşağıda listelenen gibi çeşitli biçimlerde destekler:

-   `geo:latitude,longitude` &ndash; Lat/lon ortalanmış eşlemeleri uygulamasını açar. 

-   `geo:latitude,longitude?z=zoom` &ndash; Uygulama lat/lon Ortalanan ve belirtilen düzeye uzaklaştırılacağını eşlemeleri açar. Yakınlaştırma düzeyini 1 ile 23: 1 görüntüler için tüm dünya arasında olabilir ve en yakın yakınlaştırma düzeyini 23'tür.

-   `geo:0,0?q=my+street+address` &ndash; Bir adresi konumunu eşlemeleri uygulamaya açar. 

-   `geo:0,0?q=business+near+city` &ndash; Maps uygulamasını açar ve açıklamalı arama sonuçlarını görüntüler. 


Sorgulama (yani sokak adresi veya arama terimleri) URI sürümleri Google geocoder hizmet sonra haritada görüntülenmelerini konumunu almak için kullanın. Örneğin, URI `geo:0,0?q=coop+Cambridge` aşağıda gösterilen harita sonuçlanıyor:

[![Google Haritalar ile bir arama terimi gösteren örnek ekran görüntüsü](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



Coğrafi URI düzeni hakkında daha fazla bilgi için bkz: [bir konumu haritada Göster](http://developer.android.com/guide/components/intents-common.html#Maps).


## <a name="street-view"></a>Sokak görünümü

Coğrafi düzeni yanı sıra bir amaçtan Sokak görünümler yükleniyor Android de destekler. Xamarin.Android başlatılan Sokak görünümü uygulaması örneği aşağıda verilmiştir:

[![Sokak görünümünün örnek ekran görüntüsü](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

Sokak görünüm başlatmak için yalnızca kullanmak `google.streetview` aşağıdaki kodda gösterildiği gibi URI şeması:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Yukarıda kullanılan google.streetview URI şeması aşağıdaki biçimdedir:

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Gördüğünüz gibi birkaç parametre desteklenir, aşağıda listelenen gibi vardır:

-   `lat` &ndash; Sokak görünümünde gösterilecek konumun enlem.

-   `lng` &ndash; Sokak görünümünüzde gösterilmesi için konum boylam.

-   `pitch` &ndash; 90 derece olduğu aşağı düz derece ve-90 derece Merkezi'nden ölçülen Sokak görünüm panorama açısını düz çalışıyor.

-   `yaw` &ndash; Merkezi, görünümü Sokak görünüm panorama, saat yönünde cinsinden ölçülen Kuzey derece.

-   `zoom` &ndash; Sokak görünüm panorama çarpanı yakınlaştırma, 1.0 burada normal yakınlaştırma, 2.0 = yakınlaştırılmış 2 = x 3.0 yakınlaştırılmış 4 = x, vs.

-   `mz` &ndash; Maps uygulamaya Sokak görünümünden geçerken kullanılacak harita yakınlaştırma düzeyini.


Uygulama yerleşik çalışmak eşlemeleri veya Sokak görünümü hızla eşleme desteği eklemek için kolay bir yoludur. Ancak, Android'ın eşlemeleri API eşleme deneyimi üzerinde daha hassas denetim sunar.
