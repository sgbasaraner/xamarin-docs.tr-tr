---
title: "Kişiler ContentProvider kullanma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 677d672b3f00d4c3f3505ab2adf977f16fca4de5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="using-the-contacts-contentprovider"></a>Kişiler ContentProvider kullanma

Kod kullanan tarafından sunulan veri erişim bir `ContentProvider` başvuru gerektirmeyen `ContentProvider` hiç sınıfı. Bunun yerine, bir URI tarafından kullanıma sunulan veriler üzerinde bir imleç oluşturmak için kullanılan `ContentProvider`. Android kullanıma sunan bir uygulama için sistem aramak için URI kullanan bir `ContentProvider` kimlikle. URI biçiminde bir dize, tipik olarak bir geriye doğru DNS olduğu gibi `com.android.contacts/data`.

Bu dize, Android unutmayın geliştiriciler yapmak yerine *kişiler* sağlayıcısı gösterir, meta verilerde `android.provider.ContactsContract` sınıfı. Bu sınıf URI'sini belirlemek için kullanılan `ContentProvider` tablolar ve sorgulanabilir sütun adları yanı sıra.

Bazı veri türleri için özel erişim izni de gerekir. Yerleşik kişiler listesi gerektiren `android.permission.READ_CONTACTS` izin **AndroidManifest.xml** dosya.

URI'den bir imleç oluşturmanın üç yolu vardır:

1. **ManagedQuery()** &ndash; tercih edilen yaklaşım Android 2.3 (API düzey 10) ve önceki sürümlerinde, bir `ManagedQuery` bir imleç döndürür ve verileri yenileme ve imleci kapatma da otomatik olarak yönetir. Bu yöntem, Android 3.0 (API düzeyi 11) kullanım dışıdır.

1. **ContentResolver.Query()** &ndash; yenilenir ve açıkça kodda kapalı gerekir anlamına gelir yönetilmeyen bir imleç döndürür.

1. **CursorLoader().LoadInBackground()** &ndash; Introduced in Android 3.0 (API Level 11), `CursorLoader` is now the preferred way to consume a `ContentProvider` . `CursorLoader` sorguları bir `ContentResolver` UI engellenmiş değil için bir arka plan iş parçacığı üzerinde.
   Bu sınıf, Android v4 uyumluluk kitaplığını kullanarak, daha eski sürümlerini içinde erişilebilir.


Bu yöntemlerin her biri aynı temel girişleri kümesi vardır:

-  **URI** &ndash; tam adını `ContentProvider` .
-  **Projeksiyon** &ndash; için imleci seçmek için hangi sütunların belirtimi.
-  **Seçim** &ndash; benzer bir SQL `WHERE` yan tümcesi.
-  **SelectionArgs** &ndash; seçim atanması parametreleri.
-  **SortOrder** &ndash; göre sıralamak için sütun.


<a name="Creating_Inputs_for_a_Query" />

## <a name="creating-inputs-for-a-query"></a>Giriş için bir sorgu oluşturma

`ContactsProvider` Örnek kod Android'ın yerleşik kişiler sağlayıcısı çok basit bir sorgu gerçekleştirir. Gerçek URI veya sütun adları - kişileri sorgulamak için gerekli tüm bilgileri bilmeniz gerekiyor mu `ContentProvider` tarafından sunulan sabitleri olarak kullanılabilir `ContactsContract` sınıfı.

Hangi yöntemin bağımsız olarak imleci almak için kullanılan, bu aynı nesneler parametre olarak gösterildiği gibi kullanılır *ContactsProvider/ContactsAdapter.cs* dosyası:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

Bu örnek için `selection`, `selectionArgs` ve `sortOrder` ayarlanarak yoksayılacak `null`.


<a name="Creating_a_Cursor_from_a_Content_Provider_Uri" />

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Bir içerik sağlayıcı Uri'den imleç

Parameter nesnelerini oluşturduktan sonra aşağıdaki üç yoldan biriyle kullanılabilir:


<a name="Using_a_Managed_Query" />

### <a name="using-a-managed-query"></a>Yönetilen bir sorgu kullanarak

Android 2.3 (API düzey 10) hedefleme uygulamaları ya da daha önce bu yöntem kullanmanız gerekir:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Kapatmak gerek yoktur bu imleç Android tarafından yönetilmez.


<a name="Using_ContentResolver" />

### <a name="using-contentresolver"></a>ContentResolver kullanma

Erişme `ContentResolver` doğrudan bir imleç karşı almak için bir `ContentProvider` olabilir benzer:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Bu imleç yönetilmeyen, olduğundan artık gerekli olduğunda kapatılmalıdır.
Açık bir imleç kodu kapatır, aksi takdirde bir hata ortaya çıkar emin olun.

```csharp
cursor.Close();
```

Alternatif olarak, çağırabilirsiniz `StartManagingCursor()` ve `StopManagingCursor()` 'imleci yönetmek için '. Yönetilen imleçler otomatik olarak devre dışı bırakıldı ve etkinlikleri olduğunda durdurulur ve yeniden yeniden sorgulanan.


<a name="Using_CursorLoader" />

### <a name="using-cursorloader"></a>CursorLoader kullanma

Uygulamaların Android 3.0 (API düzeyi 11) için oluşturulmuş veya daha yeni bu yöntem kullanmanız gerekir:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` Tüm imleç işlemleri arka plan iş parçacığında yapılır ve bir etkinlik (örn. bir yapılandırma değişikliği nedeniyle) yerine, verileri yeniden yeniden başlatıldığında akıllıca var olan bir imleç etkinlik örnekleri arasında yeniden kullanabilirsiniz sağlar.

Android sürümlerde de kullanabilir `CursorLoader` kullanarak sınıfı [v4 destek kitaplıkları](http://developer.android.com/tools/support-library/index.html).


<a name="Displaying_the_Cursor_Data_with_a_Custom_Adapter" />

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Özel bağdaştırıcı ile imleç verileri görüntüleme

Böylece biz el ile çözebilirsiniz kişi görüntüyü görüntülemek için özel bir bağdaştırıcı kullanacağız `PhotoId` bir görüntü dosyası yolu referansı.

Özel bir bağdaştırıcı ile verileri görüntülemek için örnek kullanan bir `CursorLoader` yerel bir koleksiyon tüm kişi veri almak için **FillContacts** yönteminden **ContactsProvider/ContactsAdapter.cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

BaseAdapter'ın yöntemlerini kullanarak uygulama `contactList` koleksiyonu. Herhangi bir koleksiyonu ile olduğu gibi bağdaştırıcı uygulanır &ndash; gelen veri kaynağı için hiçbir özel işleme olan bir `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

Görüntüyü, (varsa) görüntülenir cihazdaki resim dosyasının URI'ı kullanarak. Uygulama şuna benzer:

[![Uygulamasının ekran görüntüsü kişiler ListView içinde görüntüleme; görüntüyü bir giriş solunda görüntülenir](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png)

Benzer bir kod desen kullanarak uygulamanızı sistem verileri kullanıcının fotoğrafları, videoları ve müzik dahil olmak üzere çok çeşitli erişebilir.
Bazı veri türleri projesinin istenmesi için özel izinleri gerektiren **AndroidManifest.xml**.


<a name="Displaying_the_Cursor_Data_with_a_SimpleCursorAdapter" />

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Bir SimpleCursorAdapter ile imleç verileri görüntüleme

İmleç ile de görüntülenemedi bir `SimpleCursorAdapter` (yalnızca ad görüntülenmesine karşın, fotoğraf). Bu kodu nasıl kullanılacağını gösteren bir `ContentProvider` ile `SimpleCursorAdapter` (Bu kod örnek görünmez):

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Başvurmak [ListViews ve bağdaştırıcıları](~/android/user-interface/layouts/list-view/index.md) uygulama konusunda daha fazla bilgi için `SimpleCursorAdapter`.


## <a name="related-links"></a>İlgili bağlantılar

- [ContactsAdapter Demo (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
