---
title: GridLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 013db64add615e94ef3494f14bc82fc17ec2dca1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="gridlayout"></a>GridLayout

`GridLayout` Yeni `ViewGroup` alt kümesi, aşağıda gösterildiği gibi bir HTML tablosu için benzer 2B bir kılavuz görünümlerde düzenlemeyi destekler:

 [ ![Dört görüntüleme GridLayout kırpılmış](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png)

 `GridLayout` Düz görünüm hiyerarşisiyle alt görünümleri konumlarına kılavuzdaki satırları ve sütunları içinde olmalıdır belirterek belirlendiği çalışır. Bu şekilde *GridLayout* görünümleri, tüm ara görünüm tablo yapısı sağlayan TableLayout kullanılan tablo satırı görüldüğü gibi gerek kalmadan kılavuzda konum yapabiliyor. Düz bir hiyerarşi Bakımı tarafından *GridLayout* daha hemen düzene bulabildiği alt görünümlerini. Bu kavram gerçekte kodda anlamını gösteren örnek bir göz atalım.

<a name="Creating_a_Grid_Layout" />

## <a name="creating-a-grid-layout"></a>Bir kılavuz düzeni oluşturma

Aşağıdaki XML birkaç ekler `TextView` için denetimler bir *GridLayout*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

Böylece hücreleri içeriklerine sığabilecek düzeni tarafından Aşağıdaki diyagramda gösterildiği gibi satır ve sütun boyutları ayarlanır:

 [ ![Sol tarafta iki hücre sağdaki daha küçük gösteren düzeni diyagramı](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png)

Bu uygulamayı çalıştırdığınızda, aşağıdaki kullanıcı arabirimi ile sonuçlanır:

 [ ![Ekran görüntüsü, GridLayoutDemo uygulama dört görüntüleme](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png)

 <a name="Specifying_Orientation" />


## <a name="specifying-orientation"></a>Yönlendirme belirtme

Yukarıdaki XML her fark `TextView` bir satır veya sütun belirtmiyor. Bu belirtilmezse, `GridLayout` her alt Görünüm yönlendirmesini dayalı sırayla atar. Örneğin, yatay, bu gibi çok dikey varsayılan şimdi GridLayout'ın yönü değiştirin:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Şimdi, `GridLayout` soldan sağa yerine her bir sütunun alt üstten hücrelere aşağıda gösterildiği gibi getirin:

 [ ![Hücreleri dikey yönde nasıl konumlandırılacağını gösteren diyagram](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png)

Bu çalışma zamanında aşağıdaki kullanıcı arabirimi ile sonuçlanır:

 [ ![Dikey yönde konumlandırılmış hücrelerle GridLayoutDemo ekran görüntüsü](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png)

 <a name="Specifying_Explicit_Position" />


### <a name="specifying-explicit-position"></a>Açık konum belirtme

Biz açıkça alt görünümlerde konumlarını denetlemek istiyorsanız `GridLayout`, biz ayarlayabilirsiniz kendi `layout_row` ve `layout_column` öznitelikleri. Örneğin, aşağıdaki XML (yukarıda gösterilen), ilk ekran yönünü bağımsız olarak gösterilen düzeni neden olur.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```

 <a name="Specifying_spacing" />


### <a name="specifying-spacing"></a>Aralık belirtme

Birkaç alt arasındaki boşluğu görünümlerini sağlayacak seçenekleri sahibiz `GridLayout`. Biz kullanabilirsiniz `layout_margin` kenar her alt görünümünde doğrudan, aşağıda gösterildiği gibi ayarlamak için özniteliği

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Ayrıca, Android 4'te yeni bir genel amaçlı aralığı görünümü adlı `Space` kullanıma sunulmuştur. Kullanmak için yalnızca bir alt görünüm olarak ekleyin.
Örneğin, aşağıdaki XML için ek bir satır ekler `GridLayout` ayarlayarak kendi `rowcount` 3 ve ekleyen bir `Space` arasındaki boşluğu sağlar Görünüm `TextViews`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

Bu XML aralığı oluşturur `GridLayout` aşağıda gösterildiği gibi:

 [ ![Aralık büyük hücrelerle gösteren GridLayoutDemo ekran görüntüsü](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png)

Yeni kullanmanın faydası `Space` görünümdür aralığını sağlar ve her alt görünüm öznitelikleri ayarlamak bize gerektirmez.

 <a name="Spanning_Columns_and_Rows" />


### <a name="spanning-columns-and-rows"></a>Satırları ve sütunları yayma

`GridLayout` Birden çok sütun ve satırlar span hücreleri de destekler. Örneğin, bir düğmeye içeren başka bir satır eklediğimiz deyin `GridLayout` aşağıda gösterildiği gibi:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

Bu ilk sütununda sonuçlanır `GridLayout` Burada gördüğünüz gibi düğmesi boyutuna uygun şekilde uzatılmış:

[ ![Yalnızca ilk sütununu kapsayıcı düğmesiyle GridLayoutDemo ekran görüntüsü](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png)

İlk sütun uzatma gelen tutmak için bu gibi kendi columnspan ayarlayarak iki sütun kapsanacak düğmesi ayarlayabilirsiniz:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Bunun yapılması sonuçları için bir düzende `TextViews` , benzer biz sahip daha önce altına eklenen düğmesiyle Düzen `GridLayout` aşağıda gösterildiği gibi:

 [ ![Her iki sütun kapsayıcı düğmesiyle GridLayoutDemo ekran görüntüsü](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png)


## <a name="related-links"></a>İlgili bağlantılar

- [GridLayoutDemo (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
