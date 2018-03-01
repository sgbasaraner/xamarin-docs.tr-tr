---
title: "Paketleme yıpranması uygulamalar"
ms.topic: article
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: a3eb5cd5b4202db8c58870c2b2c679b47f79d4aa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="packaging-wear-apps"></a>Paketleme yıpranması uygulamalar

Android yıpranması uygulamaları Google play'de dağıtım için bir tam Android uygulaması ile paketlenmiştir. 

## <a name="automatic-packaging"></a>Otomatik paketleme

Proje başvurusu yıpranması projesine el projeden oluşturduğunuzda, Xamarin Android 5.0 ile başlayarak, yıpranması uygulamanızı otomatik olarak el uygulamanıza bir kaynak olarak paketlenir. Bu ilişki oluşturmak için aşağıdaki adımları kullanın: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Yıpranması uygulamanızı zaten el çözümünüzü parçası değilse çözüm düğümünü sağ tıklatın ve seçin **Ekle > Varolan Proje Ekle...** .

2. Gidin **.csproj** dosya yıpranması uygulamanızın seçin ve tıklatın **açık**. Yıpranması uygulama projesi artık el çözümünüzde görünür olması gerekir.

3. Sağ **başvuruları** düğümü ve select **Başvuru Ekle**.

4. İçinde **başvuru Yöneticisi** iletişim, yıpranması projenizin (bir onay işareti eklemek için tıklatın), ardından Etkinleştir **Tamam**.

5. Taşınabilir proje paket adıyla eşleşen şekilde yıpranması projeniz için paket adı değiştirin (paket adı altında değiştirilebilir **Özellikler > Android derleme bildirimi**).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Yıpranması uygulamanızı zaten el çözümünüzü parçası değilse çözüm düğümünü sağ tıklatın ve seçin **Ekle > Varolan Proje Ekle...** .

2. Gidin **.csproj** dosya yıpranması uygulamanızın seçin ve tıklatın **açık**. Yıpranması uygulama projesi artık el çözümünüzde görünür olması gerekir.

3. Çözüm ve tıklatın el proje düğümüne sağ tıklayın **başvuruları Düzenle...** .

4. İçinde **Düzenle başvuruları** iletişim, yıpranması projenizin (bir onay işareti eklemek için tıklatın), ardından Etkinleştir **Tamam**.

5. Taşınabilir proje paket adıyla eşleşen şekilde yıpranması projeniz için paket adı değiştirin (paket adı altında değiştirilebilir **proje Seçenekleri > Android uygulaması**).

-----


Karşılaşırsınız Not bir **XA5211** yıpranması uygulama paketinin adını el uygulama paket adı eşleşmiyorsa hata. Örneğin:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

Bu hatayı düzeltmek için paket adını el ile eşleşen şekilde yıpranması uygulama paketinin adını değiştirin.

Tıkladığınızda **Yapı > Yapı tüm**, bu ilişki yıpranması proje otomatik paketleme ana avuçiçi (telefon) projesine tetikler. Yıpranması uygulama otomatik olarak oluşturulan ve bir kaynak el uygulama olarak dahil.

Yıpranması uygulama projesi oluşturur derleme bir derleme başvurusu avuçiçi (telefon) projesinde olarak kullanılmaz. Bunun yerine, yapı işlemi aşağıdakileri yapar:

-   Paket adlarının olduğunu doğrular. 

-   XML oluşturur ve onu yıpranması uygulamayla ilişkilendirmek için taşınabilir projeye ekler. Örneğin: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Yıpranması uygulama olarak ekler bir **ham** el projeye kaynak. 


## <a name="manual-packaging"></a>El ile paketleme

Sürüm 5.0 önce içinde Xamarin.Android uygulamaları Android takmak yazabilirsiniz, ancak uygulama dağıtmak için bu el ile paketleme yönergeleri izleyin: 

1. Wearable proje ve avuçiçi (telefon) projeleri aynı sürüm numarasını ve paket adı olduğundan emin olun.

2. El ile olarak Wearable Projeyi derlemek bir **sürüm** oluşturun.

3. Yayın el ile eklemeniz **. APK** adım (2) **kaynakları/ham** avuçiçi (telefon) proje dizininde.

4. Yeni bir XML kaynağını el ile eklemeniz **Resources/xml/wearable_app_desc.xml** üstte taşınır için başvuran el projesinde **APK** adım (3):

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. El ile eklemeniz bir `<meta-data />` el projenin öğesine **AndroidManifest.xml** `<application>` yeni XML kaynağa başvuran öğe:

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Ayrıca bkz. Android Geliştirici sitenin [el ile packging yönergeleri](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).

