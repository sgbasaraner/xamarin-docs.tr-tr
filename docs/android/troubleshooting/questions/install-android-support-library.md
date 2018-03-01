---
title: "Nasıl Xamarin.Android.Support paketleri tarafından gerekli olan Android desteği kitaplıklarını el ile yükleyebilir miyim?"
ms.topic: article
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26dd7e23352bf0911c2a7268518ddebf6626596a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Nasıl Xamarin.Android.Support paketleri tarafından gerekli olan Android desteği kitaplıklarını el ile yükleyebilir miyim?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.Android.Support.v4 örnek adımlar 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

İstenen Xamarin.Android.Support NuGet paketini (örneğin, NuGet Paket Yöneticisi ile yükleyerek) indirin.

Kullanım `ildasm` hangi sürümünü denetlemek için **android_m2repository.zip** NuGet paketi gerekir:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
Örnek çıktı:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Karşıdan **android\_m2repository.zip** Google URL'yi kullanarak döndürülen **ildasm**. Alternatif olarak, hangi sürümünün kontrol edebilirsiniz _Android destek depo_ Android SDK Yöneticisi'nde yüklü:

!["Android SDK'sı Android destek deposu sürümü yüklü 32 gösteren Manager"](install-android-support-library-images/sdk-extras.png)

Sürüm NuGet paketi için gereken bir eşleşirse, yeni bir şey karşıdan yüklemek gerekmez. Bunun yerine mevcut yeniden zip **m2repository** altında bulunan dizin **ek özellikler\\android** içinde _SDK yolu_ (Android üstündeki gösterildiği gibi SDK Yöneticisi penceresi).

Döndürülen URL MD5 karması hesaplanamıyor **ildasm**. Sonuç dizesini tümüyle büyük harfe ve boşluk kullanmak için biçimlendirin. Örneğin, Ayarla `$url` değişken olarak gereken ve ardından aşağıdaki 2 satırları çalıştırın (temel [Xamarin.Android özgün C# kod](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) PowerShell'de:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
Örnek çıktı:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Kopya **android\_m2repository.zip** içine **LOCALAPPDATA %\\Xamarin\\zips\\**  klasör. Adım hesaplama önceki MD5 karma değeri gelen MD5 karma değeri kullanmayı dosyayı yeniden adlandırın. Örneğin:

**% LOCALAPPDATA %\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(İsteğe bağlı) Dosyaya sıkıştırmasını **LOCALAPPDATA %\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\içerik\\**  (oluşturma bir **içerik\\m2repository** alt). Ardından bu adımı atlarsanız, bu adımı tamamlamak buna ihtiyacınız olacağından kitaplığını kullanan ilk yapı biraz daha uzun olarak sürer.
Alt sürüm numarasını (**23.4.0.0** Bu örnekte) oldukça aynı NuGet Paket sürümü değil. Kullanabileceğiniz `ildasm` doğru sürüm numarasını bulmak için:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
Örnek çıktı:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

İstenen Xamarin.Android.Support NuGet paketini (örneğin, NuGet Paket Yöneticisi ile yükleyerek) indirin.

Çift _Xamarin.Android.Support.v4_ altında derleme _başvuruları_ derlemenin derleme tarayıcıda açmak Mac için Visual Studio'da Android projesi kısmında. Emin _dil_ açılan ayarlanmış _C#_ ve üst düzey seçin _Xamarin.Android.Support.v4_ derleme tarayıcı Gezinti ağacında derlemesinden. Bulun `SourceUrl` biri altında özellik `IncludeAndroidResourcesFrom` veya `JavaLibraryReference` öznitelikleri:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Karşıdan **android\_m2repository.zip** Google kullanarak `SourceUrl` döndürülen **ildasm**. Alternatif olarak, hangi sürümünün kontrol edebilirsiniz _Android destek depo_ Android SDK Yöneticisi'nde yüklü:

!["Android SDK'sı Android destek deposu sürümü yüklü 32 gösteren Manager"](install-android-support-library-images/sdk-extras.png)

Sürüm NuGet paketi için gereken bir eşleşirse, yeni bir şey karşıdan yüklemek gerekmez. Bunun yerine mevcut yeniden zip **m2repository** altında bulunan dizin **ek özellikler/android** içinde _SDK yolu_ (Android SDK Manager penceresinin üst gösterildiği gibi) .

Döndürülen URL MD5 karması hesaplanamıyor **ildasm**. Sonuç dizesini tümüyle büyük harfe ve boşluk kullanmak için biçimlendirin. Örneğin, URL dizesi gerektiği gibi ayarlayın ve ardından şu komutu çalıştırın bir **Terminal.app** komut istemi:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Başka bir seçenek kullanmaktır `csharp` çalıştırmak için yorumlayıcı [Xamarin.Android kendisini kullanan aynı C# kod](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Bunu yapmak için ayarlamak `url` değişken olarak gerekli ve ardından şu komutu çalıştırın bir **Terminal.app** komut istemi:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
Örnek çıktı:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Kopya **android\_m2repository.zip** için **$HOME/.local/share/Xamarin/zips/** klasör. Adım hesaplama önceki MD5 karma değeri gelen MD5 karma değeri kullanmayı dosyayı yeniden adlandırın. Örneğin:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(İsteğe bağlı) Dosyaya sıkıştırmasını açın: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(oluşturma bir **içeriği/m2repository** alt). Ardından bu adımı atlarsanız, bu adımı tamamlamak buna ihtiyacınız olacağından kitaplığını kullanan ilk yapı biraz daha uzun olarak sürer.

Alt sürüm numarasını (**23.4.0.0** Bu örnekte) oldukça aynı NuGet Paket sürümü değil. Olarak **ildasm** önceki adımda, derleme tarayıcı Mac için Visual Studio'da doğru sürüm numarasını bulmak için kullanabilirsiniz. Ara `Version` biri altında özellik `IncludeAndroidResourcesFrom` veya `JavaLibraryReference` öznitelikleri:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>Ek başvurular

- [Hata 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – doğru olmayan "yükleme başarısız. Lütfen {0} indirin ve {1} dizinine yerleştirin." ve "Lütfen paketi yükleyin: '{0}' SDK'sı Yükleyicisi'nde kullanılabilir" hata iletileri ilgili Xamarin.Android.Support paketleri ekleme

### <a name="next-steps"></a>Sonraki Adımlar

Bu belge, Ağustos 2016'dan sonra şu anki davranışı açıklanır. Bu belgede açıklanan teknikleri gelecekteki uğratabilir şekilde xamarin, kararlı test paketinin bir parçası değil.

Bizimle iletişime geçin veya bile Yukarıdaki bilgilerin kullanılarak sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [için Xamarin hangi destek seçenekleri kullanılabilir?](~/cross-platform/troubleshooting/support-options.md) önerileri, iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Yeni bir hatanın gerekirse dosya.

