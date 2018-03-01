
Visual Studio ve Mac derleme aracısı iOS uygulamaları oluştururken, .app paketinde Windows makinesine kopyalanmaz. Visual Studio 7.4 için Xamarin araçları ekler yeni `CopyAppBundle` CI veren özelliği derlemeler .app paketleri geri Windows kopyalamak için.

Bu işlevselliği kullanmak için ekleme`CopyAppBundle` bu işlev için uygulamak istediğiniz özellik grubu altında .csproj özelliğine. Örneğin, aşağıdaki örnek .app paketinde geri Windows bilgisayarı nasıl kopyalanacağını gösterir bir **hata ayıklama** hedefleme yapı **iPhoneSimulator**:

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

