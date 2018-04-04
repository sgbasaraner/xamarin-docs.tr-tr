---
title: Takvim
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d48af16f50c5a4482342d323b5c73b9c237cc83a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="calendar"></a>Takvim


## <a name="calendar-api"></a>Takvim API

Yeni bir takvim Android 4'te tanıtılan API kümesi okumak veya takvim sağlayıcının veri yazmak için tasarlanmış uygulamaları destekler. Bu API'leri bol miktarda olayları, katılımcılar ve anımsatıcıları okuma ve yazma yeteneği dahil olmak üzere Takvim verilerle etkileşimi seçeneklerini destekler. Uygulamanızda Takvim sağlayıcısı kullanarak API aracılığıyla eklemek veri Android 4 ile gelen yerleşik Takvim uygulamasını görünür.


## <a name="adding-permissions"></a>İzinleri ekleme

Uygulamanızda yeni takvim API'leri ile çalışırken, yapmanız gereken ilk şey uygun izinleri Android bildirim eklemektir. Eklemek için gereken izinler `android.permisson.READ_CALENDAR` ve `android.permission.WRITE_CALENDAR`, olup, okuma ve/veya takvim veri yazma bağlı olarak.


## <a name="using-the-calendar-contract"></a>Takvim sözleşme kullanma

İzinleri ayarladıktan sonra kullanarak Takvim verilerle etkileşime girebilecek `CalendarContract` sınıfı. Bu sınıf, Takvim sağlayıcı ile etkileşim kurduklarında, uygulamaların kullanabileceği bir veri modeli sağlar. `CalendarContract` URI'ler takvim ve etkinliklerine gibi takvim varlıklar çözümlemek uygulamaları sağlar. Ayrıca, her varlık, bir takvim adı ve kimliği, veya bir olayın başlangıç ve bitiş tarihi gibi çeşitli alanları ile etkileşim için bir yol sağlar.

Takvim API'sini kullanan bir örneğe bakalım. Bu örnekte, biz takvime yeni bir olay eklemek nasıl yanı sıra, takvimler ve bunların olaylarını Numaralandırılacak nasıl inceleyeceğiz.


## <a name="listing-calendars"></a>Takvimler listeleme

İlk olarak, Takvim uygulamada kayıtlı takvimleri Numaralandırılacak nasıl inceleyelim. Bunu yapmak için biz oluşturabileceğiniz bir `CursorLoader`. Android 3.0 (API 11) sunulan `CursorLoader` kullanmak için tercih edilen yolu bir `ContentProvider`. En azından, biz takvimleri ve döndürülecek istiyoruz sütunlar için URI içerik belirtmeniz gerekir; Bu sütun belirtimi olarak bilinen bir _projeksiyon_.

Çağırma `CursorLoader.LoadInBackground` yöntemi bize Takvim sağlayıcısı gibi veriler için bir içerik sağlayıcı sorgulamaya izin verir.
`LoadInBackground` gerçek yükleme işlemi gerçekleştirir ve döndüren bir `Cursor` sorgu sonuçlarına sahip.

`CalendarContract` Hem içerik belirtmenin bize yardımcı olduğunu `Uri` ve projeksiyon. İçerik almak için `Uri` takvimleri sorgulamaya biz yalnızca kullanabilirsiniz `CalendarContract.Calendars.ContentUri` özelliği şuna benzer:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

Kullanarak `CalendarContract` hangi takvim belirtmek için istiyoruz sütunları basittir eşit. Yalnızca alanları eklediğimiz `CalendarContract.Calendars.InterfaceConsts` sınıfı bir dizi. Örneğin, aşağıdaki kodu Takvim kimliği içerir, görüntü adı ve hesap adı:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id` Kullanıyorsanız içerecek şekilde önemli bir `SimpleCursorAdapter` kısa süre içinde göreceğiz gibi kullanıcı Arabirimi için veri bağlamak için. İçerik URI ve projeksiyon yerinde ile biz örneği `CursorLoader` ve arama `CursorLoader.LoadInBackground` yöntemi aşağıda gösterildiği gibi bir imleç Takvim verileri döndürmek için:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

Bu örnek için kullanıcı Arabirimi içeren bir `ListView`, tek bir takvim temsil eden listedeki her öğeye sahip. Aşağıdaki XML içeren biçimlendirme gösterir `ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

Ayrıca, biz ayrı bir XML dosyasında şu şekilde yerleştirin her liste öğesi için UI belirtmek ihtiyacımız var:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

Bu noktadan itibaren bu verileri imleci kullanıcı Arabirimine bağlamak için yalnızca normal Android kodudur. Kullanacağız bir `SimpleCursorAdapter` gibi:

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

Yukarıdaki kod içinde belirtilen sütun bağdaştırıcısı alır `sourceColumns` dizi ve kullanıcı arabirimi öğeleri Yazar `targetResources` imleci her takvim girişi için dizi. Etkinlik burada sınıfıdır `ListActivity`; içerdiği `ListAdapter` özelliği bağdaştırıcı için ayarlarız.

Görüntülenen takvim bilgisi ile nihai sonucu gösteren bir ekran görüntüsü işte `ListView`:

[![CalendarDemo iki takvim girişlerini görüntüleme öykünücüde çalıştırma](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>Liste takvim olayları

Sonraki olayları için belirtilen bir takvim numaralandırmak nasıl bakalım.
Yukarıdaki örnekte oluşturma, biz kullanıcı takvimlerine birini seçtiğinde olaylarının bir listesi sunması. Bu nedenle, biz önceki kodda öğe seçimi işlemeye gerekir:

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

Bu kodda bir etkinlik türünün açmak için bir hedefi oluşturuyoruz `EventListActivity`, Takvim kimliği hedefini geçirme. Biz sorgu olayları için hangi takvim bilinmesi gereken kimliği gerekir. İçinde `EventListActivity`'s `OnCreate` yöntemi, biz Kimliğinden alabilir `Intent` aşağıda gösterildiği gibi:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Şimdi bu sorgu olaylarını kimliği Takvim artık Sorgu olaylarını işleme benzer şekilde biz sorgulanan takvimleri daha önce bir listesi için yalnızca bu kez ki çalışabilirsiniz `CalendarContract.Events` sınıfı. Aşağıdaki kod olaylarını almak için bir sorgu oluşturur:

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

Bu kodda biz öncelikle içeriği elde `Uri` olayları için `CalendarContract.Events.ContentUri` özelliği. Ardından biz eventsProjection dizisinde almak istediğiniz olay sütunları belirtin.
Son olarak, biz örneği bir `CursorLoader` bu bilgileri ve çağrı yükleyicisi 's `LoadInBackground` döndürülecek yöntemi bir `Cursor` olay verileri ile.

Olay verileri kullanıcı Arabiriminde görüntülenecek biz biçimlendirme ve kodun hemen önce takvimleri listesini görüntülemek için yaptığımız gibi kullanabilirsiniz. Yeniden kullanırız `SimpleCursorAdapter` veri bağlamak için bir `ListView` aşağıdaki kodda gösterildiği gibi:

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

Bu kod ve daha önce Takvim listesini göstermek için kullanılan kod arasındaki temel fark kullanımıdır bir `ViewBinder`, satırda ayarlayın:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder` Sınıfı bize nasıl biz değerleri görünümlerine bağlamak daha fazla denetim sağlar. Bu durumda, biz bu olay başlangıç zamanı milisaniye bir tarih dizesi dönüştürmek için aşağıdaki uygulamasında gösterildiği gibi kullanın:

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

Aşağıda gösterildiği gibi olayların bir listesini görüntüler:

[![Üç takvim olayları görüntüleme örnek uygulamasının ekran görüntüsü](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>Takvim olay ekleme

Takvim verileri okumak nasıl gördük. Şimdi bir takvim için bir olay eklemek nasıl görelim. Bunun çalışması için eklediğinizden emin olun `android.permission.WRITE_CALENDAR` biz bahsedilen önceki izni. Bir takvim için bir olay eklemek için şunları yapacağız:

1.  Oluşturma bir `ContentValues` örneği.
1.  Anahtarları kullanmak `CalendarContract.Events.InterfaceConsts` doldurmak için sınıf `ContentValues` örneği.
1.  Bitiş saatlerini ve olay başlangıç saat dilimlerini ayarlayın.
1.  Kullanım bir `ContentResolver` takvimine olay verileri eklemek için.


Aşağıdaki kod, aşağıdaki adımları gösterilmektedir:

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

Unutmayın saat dilimi, türünde bir özel durum ayarlamayın `Java.Lang.IllegalArgumentException` oluşturulur. Biz oluşturur, bu dönem ifade edilmesi gerekir olay süre değerleri milisaniye cinsinden çünkü bir `GetDateTimeMS` yöntemi (içinde `EventListActivity`) bizim tarih belirtimleri milisaniyelik biçimine dönüştürmek için:

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

Bir düğme UI olay listesine eklemek ve yukarıdaki kodu çalıştırmak 's olay işleyicisi düğmesini, olay takvime eklenir ve aşağıda gösterildiği gibi bizim listesinde güncelleştirildi:

[![Örnek Olay Ekle düğmesine tıklayın ve ardından Takvim olaylarla örnek uygulamasının ekran görüntüsü](calendar-images/13.png)](calendar-images/13.png#lightbox)

Şu Takvim uygulamasını açın, sonra olay orada da yazılır göreceğiz:

[![Seçili takvim olayı görüntüleme Takvim uygulamasının ekran görüntüsü](calendar-images/14.png)](calendar-images/14.png#lightbox)

Gördüğünüz gibi Android uygulamaların sorunsuz bir şekilde Takvim özellikleri tümleştirmek izin almak ve takvim verilerini kalıcı hale getirmek güçlü ve kolay erişim sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Takvim Demo (örnek)](https://developer.xamarin.com/samples/CalendarDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
