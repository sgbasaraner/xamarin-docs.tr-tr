---
title: Kullanıcı profili
ms.topic: article
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/22/2018
ms.openlocfilehash: 1407266f987b36b72e32a82c8f6f43b4a734af5d
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="user-profile"></a>Kullanıcı profili

Android numaralandırma kişilerle desteklenen [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) API Düzey 5 itibaren sağlayıcısı. Örneğin, kişiler listeleme kullanmak kadar basit [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) sınıfı aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Android 4 (API düzeyi 14) ile başlayan [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) sınıftır aracılığıyla kullanılabilen `ContactsContract` sağlayıcısı. `ContactsContact.Profile` Cihaz sahibinin adı ve telefon numarası gibi kişi verilerini içeren bir cihaz sahibinin kişisel profil erişim sağlar.


## <a name="required-permissions"></a>Gerekli İzinler

Kişi verilerini okuma ve yazma için uygulamaları istemelidir `READ_CONTACTS` ve `WRITE_CONTACTS` izinleri, sırasıyla.
Ayrıca, okuma ve kullanıcı profili düzenlemek için uygulamaları istemelidir `READ_PROFILE` ve `WRITE_PROFILE` izinleri.


## <a name="updating-profile-data"></a>Profil verileri güncelleştirme

Bu izinleri ayarladıktan sonra uygulamanın kullanıcı profilinin verileriyle etkileşim kurmak için normal Android teknikleri kullanabilirsiniz. Örneğin, profilin görünen adı güncelleştirmek için arama [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) ile bir `Uri` üzerinden alınan [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) gösterildiği gibi özelliği Aşağıda:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Profil verileri okuma

Bir sorgu verme [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) okuma profil verileri yedekleyin. Örneğin, aşağıdaki kod, kullanıcı profilinin görünen adı şöyle olacaktır:

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>Kullanıcı profili gezinme

Son olarak, kullanıcı profili gitmek için bir hedefi ile oluşturun bir `ActionView` eylem ve `ContactsContract.Profile.ContentUri` ona geçirmek `StartActivity` yöntemi şuna benzer:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Yukarıdaki kod çalıştırırken, kullanıcı profili aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

[![John Doe kullanıcı profili görüntüleme profilinin ekran görüntüsü](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Kullanıcı profili ile çalışma Android diğer verilerle etkileşim için benzer ve cihaz kişiselleştirme, ek bir düzeyi sunar.



## <a name="related-links"></a>İlgili bağlantılar

- [ContactsProviderDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
