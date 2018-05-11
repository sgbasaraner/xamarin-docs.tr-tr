---
title: Xamarin.Forms görsel durum Yöneticisi
description: XAML öğeleri koddan ayarlanan visual durumlara göre değişiklik yapmak için görsel durum Yöneticisi'ni kullanın.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 14553bc9484ecc236fb4ceefd687ec7742109758
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms görsel durum Yöneticisi

_XAML öğeleri koddan ayarlanan visual durumlara göre değişiklik yapmak için görsel durum Yöneticisi'ni kullanın._

Görsel durum Yöneticisi'ni (VSM) Xamarin.Forms 3. 0'yeni bir özelliktir. VSM visual için kullanıcı arabirimi koddan değişiklik için yapılandırılmış bir yol sağlar. Çoğu durumda, uygulamanın kullanıcı arabirimi XAML içinde tanımlanır ve bu XAML işaretleme görsel durum Yöneticisi kullanıcı arabirimi görselleri etkilemesi açıklayan içerir.

VSM kavramını sunmaktadır _görsel durumlarını_. Xamarin.Forms görünümü gibi bir `Button` temel durumuna bağlı olarak birkaç farklı görsel görünümler olabilir &mdash; olup devre dışı bırakıldı veya basılı veya odak giriş. Bu düğmenin durumlarıdır.

Görsel durumlarını toplanır _visual durumu grupları_. Bir görsel durumuna grubundaki tüm görsel durumlarını karşılıklı olarak birbirini dışlar. Görsel durumlarını ve görsel durumuna grupları basit metin dizelerini tarafından tanımlanır.

Xamarin.Forms Visual durum Yöneticisi ile üç görsel durumlarını "CommonStates" adlı bir visual durum grubu tanımlar:

- "Normal"
- "Devre dışı"
- "Odaklanmış"

Bu görsel durum grubu öğesinden türetilen tüm sınıflar için desteklenen [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), için temel sınıfı olan [ `View` ](xref:Xamarin.Forms.View) ve [ `Page` ](xref:Xamarin.Forms.Page). 

Ayrıca kendi görsel durumuna grupları tanımlayabileceğiniz ve görsel durumlarını, bu makale olarak gösterilmektedir.

> [!NOTE]
> Xamarin.Forms geliştiricilerinin aşina [Tetikleyicileri](~/xamarin-forms/app-fundamentals/triggers.md) Tetikleyicileri de bir görünümün özellikler veya olaylarını tetikleme değişikliklere göre kullanıcı arabiriminde görselleri değişiklik yapabilir, farkında. Ancak, bu değişiklikleri çeşitli bileşimleri dağıtılacak Tetikleyicileri kullanarak oldukça karmaşık olabilir. Tarihsel olarak, Visual durum Yöneticisi Windows XAML tabanlı ortamları görsel durumlarını birleşimleri arasından kaynaklanan karışıklığı hafifletmek için sunulmuştur. VSM ile görsel durumlarını visual durum grubu içindeki her zaman karşılıklı olarak birbirini dışlar. Herhangi bir zamanda geçerli durumu her grubu yalnızca bir durumda değil.

## <a name="the-common-states"></a>Ortak durumları

Görsel durum Yöneticisi bölümleri görünümü normal veya devre dışı bırakıldı veya giriş odağını varsa, bir görünüm görsel görünümünü değiştirebilirsiniz XAML dosyanıza dahil sağlar. Bunlar olarak bilinir _ortak durumları_.

Örneğin, sahip olduğunuz varsayalım bir `Entry` , sayfa görünümü ve görsel görünümünü istediğiniz `Entry` aşağıdaki yollarla değiştirmek için:

- `Entry` Bir pembe olmalıdır ne zaman arka plan `Entry` devre dışı bırakılır.
- `Entry` Açık yeşil arka plan normalde olması gerekir.
- `Entry` Odak giriş olduğunda normal yüksekliğini iki kez genişletmeniz gerekir.

Birden çok görünüm için geçerliyse stilde tanımlayabilirsiniz veya ayrı bir görünümüne VSM biçimlendirme ekleyebilirsiniz. Sonraki iki bölümde Bu yaklaşımlar açıklanmaktadır.

### <a name="vsm-markup-on-a-view"></a>Bir görünümde VSM biçimlendirme

VSM biçimlendirme eklemek için bir `Entry` görüntülemek için önce ayrı `Entry` başlangıç ve bitiş etiketleri içine:

```xaml
<Entry FontSize="18">

</Entry>
```

Durumlardan birini kullanacağından açık yazı tipi boyutunu verdiği `FontSize` metin boyutunu iki katına çıkarma özelliğini `Entry`.

Ardından, INSERT `VisualStateManager.VisualStateGroups` bu etiketlerin arasındaki etiketler:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) tarafından tanımlanan bir ekli bağlanabilirse özellik [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) sınıfı. (Ekli bağlanabilir özellikler hakkında daha fazla bilgi için bkz: [ekli özellikler](~/xamarin-forms/xaml/attached-properties.md).) Bunun nasıl `VisualStateGroups` özelliği eklendiği `Entry` nesnesi.

`VisualStateGroups` Özelliği türüdür [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList), koleksiyonu olduğu [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) nesneleri. İçinde `VisualStateManager.VisualStateGroups` etiketler, bir çift ekleyin `VisualStateGroup` görsel durumlarını eklemek istediğiniz her grup için etiketler:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Dikkat `VisualStateGroup` etikete sahip bir `x:Name` grubunun adını belirten özniteliği. `VisualStateGroup` Sınıfı tanımlayan bir `Name` yerine kullanabileceğiniz özelliği:

```xaml
<VisualStateGroup Name="CommonStates">
```

Kullanabilirsiniz `x:Name` veya `Name` ancak ikisini aynı öğede.

`VisualStateGroup` Sınıfı tanımlayan adlı bir özellik [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States), koleksiyonu olduğu [ `VisualState` ](xref:Xamarin.Forms.VisualState) nesneleri. `States` olan _içerik özelliği_ , `VisualStateGroups` dahil edebileceğiniz şekilde `VisualState` doğrudan arasında etiketler `VisualStateGroup` etiketler. (İçerik özellikleri makalesinde açıklanan [temel XAML sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Sonraki adım, o grupta visual her durum için etiketler çifti eklemektir. Bunlar ayrıca kullanılarak tanımlanabilir `x:Name` veya `Name`:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` adlı bir özelliğini tanımlar [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters), koleksiyonu olduğu [ `Setter` ](xref:Xamarin.Forms.Setter) nesneleri. Aynı bunlar `Setter` kullandığınız nesneleri bir [ `Style` ](xref:Xamarin.Forms.Style) nesnesi.

`Setters` olan _değil_ içerik özelliği `VisualState`, özellik öğesi etiketleri dahil etmek gerekli olmayacak biçimde `Setters` özelliği:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
    
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Bir veya daha fazla artık ekleyebilirsiniz `Setter` her çifti arasında nesneleri `Setters` etiketler. Bunlar `Setter` daha önce açıklanan görsel durumlarını tanımlamak nesneler:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Her `Setter` etiketi, bu durum geçerli olduğunda belirli bir özellik değerini gösterir. Tarafından başvurulan herhangi bir özelliği bir `Setter` bağlanabilir özelliği tarafından nesne yedeklenir.

Biçimlendirme şuna benzer olduğunu temeli **görünümündeki VSM** sayfasındaki **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** örnek program. Sayfa üç içerir `Entry` görünümleri ancak yalnızca ikinci bir bağlı VSM biçimlendirme sahiptir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />

        <Label Text="Entry with VSM: " />

        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Dikkat ikinci `Entry` de sahip bir `DataTrigger` parçası olarak kendi `Trigger` koleksiyonu. Bu neden `Entry` bir şey üçüncü yazılan kadar devre dışı bırakılması `Entry`. İOS, Android ve evrensel Windows Platformu (UWP) çalıştıran başlangıçta sayfa şöyledir:

[![Görünüm VSM: devre dışı](vsm-images/VsmOnViewDisabled.png "VSM görünüm - devre dışı")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Geçerli visual durumu "devre dışı" böylece ikinci arka planı `Entry` iOS ve Android ekranlar pembe değil. UWP uygulaması `Entry` arka plan ayarlama izin verme ne zaman renk `Entry` devre dışı bırakılır. 

Bazı metinleri üçüncü girdiğinizde `Entry`, ikinci `Entry` "Normal" durumu ve arka plan anahtarıdır şimdi açık yeşil:

[![Görünüm VSM: Normal](vsm-images/VsmOnViewNormal.png "VSM görünüm - normal")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Touch zaman ikinci `Entry`, giriş odağını alır. "Focused" durumuna geçer ve yüksekliği için iki kez genişletir:

[![Görünüm VSM: odaklanmış](vsm-images/VsmOnViewFocused.png "VSM odaklanmış Görünüm -")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Dikkat `Entry` giriş odağını aldığında açık yeşil arka plan sürdürmez. Görsel durum Yöneticisi visual durumlar arasında geçişler gibi özellikleri önceki durumuna göre ayarlanmış ayarlanmamış. Görsel durumlarını karşılıklı olarak birbirini dışlar göz önünde bulundurun. "Normal" durum yalnızca, gelmez `Entry` etkinleştirilir. Bu anlamına gelir `Entry` etkin ve giriş odağını yok. 

İsterseniz `Entry` açık yeşil arka plan "Focused" durumda olmasını başka eklemek `Setter` bu görsel durumuna:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Bu sırada `Setter` düzgün çalışması için nesneleri bir `VisualStateGroup` içermelidir `VisualState` o gruptaki tüm durumlarıyla nesneleri. İçermediği görsel bir durum ise `Setter` nesneleri, boş bir etiket olarak yine de dahil etme:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Stil görsel durum Yöneticisi biçimlendirme

Genellikle, aynı Visual durum Yöneticisi biçimlendirme iki veya daha fazla görünümler arasında paylaşmak gereklidir. Bu durumda, biçimlendirme koymak istersiniz bir `Style` tanımı.

İşte varolan örtük `Style` için `Entry` öğelerinde **VSM üzerinde Görünüm** sayfa:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Ekleme `Setter` için etiketler `VisualStateManager.VisualStateGroups` bağlanabilirse özelliği eklenmiş:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

İçerik özelliği için `Setter` olan `Value`, bu nedenle değerini `Value` özelliği doğrudan içindeki etiketleri belirtilebilir. Özellik türü olduğunu `VisualStateGroupList`:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style> 
```

Aşağıdakilerden biri veya birkaçı dahil içinde bu etiketlerin `VisualStateGroup` nesneler:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style> 
```

VSM biçimlendirme kalanı önce aynıdır.

Burada **VSM stilde** tam VSM Biçimlendirme gösteren sayfa:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">

                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />
        
        <Label Text="Entry with VSM: " />

        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Şimdi tüm `Entry` görünümler bu sayfadaki görsel durumlarını aynı şekilde yanıt. Ayrıca "Focused" durumu şimdi saniyenin içerdiğine dikkat edin `Setter` her gösteren `Entry` bir açık yeşil arka plan da odak zaman giriş:

[![Stilde VSM](vsm-images/VsmInStyle.png "VSM stilde")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Görsel kendi durumlarını tanımlama

Öğesinden türetilen her sınıf `VisualElement` üç ortak durumları "Normal", "Odaklanmış" ve "Devre dışı" destekler. Dahili olarak, [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) sınıfı, etkin veya devre dışı veya odaklanmış veya Odaksız olma ve statik çağırır algılar [ `VisualStateManager.GoToState` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualStateManager.GoToState/p/Xamarin.Forms.VisualElement/System.String/) yöntemi:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Bu, bulabilirsiniz yalnızca Visual durum Yöneticisi koddur `VisualElement` sınıfı. Çünkü `GoToState` türetilen her sınıfın temel her nesne için çağrılır `VisualElement`, tüm görsel durum Yöneticisi'ni kullanabilirsiniz `VisualElement` yapılan bu değişiklikleri yanıt nesnesi.

İlginçtir ki, "CommonStates" visual durumu grubunun adı açıkça başvuru yok `VisualElement`. Grup adı görsel durum Yöneticisi API'si bir parçası değil. Şu ana kadar gösterilen iki örnek program biri içinde başka bir şey için "CommonStates" grubundan adını değiştirebilirsiniz ve program çalışmaya devam edecektir. Grup durumları bu gruptaki yalnızca genel bir açıklamasını adıdır. Örtük olarak herhangi bir grup içindeki görsel durum karşılıklı olarak birbirini dışlar anlaşılır: durum ve yalnızca bir herhangi bir zamanda geçerli durumudur.

Görsel kendi durumlarını uygulamak istiyorsanız, çağrı gerekir `VisualStateManager.GoToState` kodundan. Genellikle bu çağrı, sayfa sınıfının arka plan kodu dosyasından hale getireceğiz.

**VSM doğrulama** sayfasındaki **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** örnek Visual durum Yöneticisi giriş doğrulaması bağlantılı olarak kullanmayı gösterir. İki XAML dosyası oluşur `Label` öğeleri, bir `Entry`, ve `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">
        
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

VSM biçimlendirme ikinci bağlı `Label` (adlı `helpLabel`) ve `Button` (adlı `submitButton`). "Geçerli" ve "Geçersiz" adlı iki birbirini dışlayan durumlar vardır. Her iki "ValidationState" gruplarının içerdiğine dikkat edin `VisualState` etiketler "hem geçerli" ve "Geçersiz" için bunlardan birini her durumda boş olmasına rağmen. 

Varsa `Entry` geçerli bir telefon numarası, geçerli durum "geçersiz" sonra ve böylece içermiyor ikinci `Label` görünür ve `Button` devre dışı bırakılır:

[![VSM doğrulaması: Geçersiz durumu](vsm-images/VsmValidationInvalid.png "VSM doğrulama - geçersiz")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Geçerli bir telefon numarası girildiğinde, geçerli durumu olur "Geçerli". İkinci `Entry` kaybolur ve `Button` şimdi etkinleştirildi:

[![VSM doğrulaması: Geçerli durumunu](vsm-images/VsmValidationValid.png "VSM doğrulama - geçerli")](vsm-images/VsmValidationValid-Large.png#lightbox)

Arka plan kodu dosyadır işleme reponsible `TextChanged` olayından `Entry`. İşleyici, giriş dizesi geçerli olup olmadığını belirlemek için normal bir ifade kullanır. Adlı arka plan kod dosyasına yöntemi `GoToState` statik çağırır `VisualStateManager.GoToState` yöntemi her ikisi için de `helpLabel` ve `submitButton`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

Ayrıca, fark `GoToState` yöntemi durumu başlatmak için oluşturucusundan çağrılır. Her zaman geçerli bir durumda olmalıdır. Ancak "ValidationStates" netlik biri amacıyla XAML'de başvuruluyor ancak hiçbir yerde kodda görsel durumuna grubunun adı için herhangi bir referans yoktur. 

Bu görsel durumlarını ve çağırmak için arka plan kod dosyasına her nesne hesap etkilenir sayfasında yapmaları gereken işlemi fark `VisualStateManager.GoToState` bu nesnelerin her biri için. İsteğe bağlı olarak bu örnekte, yalnızca iki nesne olan ( `Label` ve `Button`), ancak çeşitli olabilir daha fazla.

Merak ediyor: arka plan kod dosyasına her nesne bu görsel durumlarını tarafından etkilenen sayfasında başvurmalıdır, neden arka plan kodu dosyanın yalnızca erişemiyor nesneleri doğrudan? Kötülerinden verebilir. Ancak nasıl görsel öğeleri denetleyebilirsiniz VSM kullanmanın avantajı olduğu tamamen XAML, tüm kullanıcı Arabirimi tasarımı tek bir konumda saklar farklı durumda tepki. Bu ayar görsel görünümünü doğrudan arka plan kod görsel öğeleri erişerek önler.

Öğesinden bir sınıf türetme dikkate alınması gereken tempting olabilir `Entry` ve belki de bir dış doğrulama işlevi için ayarlayabileceğiniz bir özellik tanımlama. Öğesinden türetilen sınıf `Entry` sonra çağırabilirsiniz `VisualStateManager.GoToState` yöntemi. Bu düzen ince, ancak yalnızca çalışır `Entry` farklı görsel durumlarını tarafından etkilenen yalnızca nesne yoktu. Bu örnekte, bir `Label` ve `Button` de olması etkilenir. Bir yolu yoktur VSM biçimlendirme bağlı için bir `Entry` VSM biçimlendirme bunlar için başka bir nesneden visual durumundaki bir değişikliği başvurmak için diğer nesnelerin bağlı için sayfa ve hiçbir şekilde diğer nesnelerin denetlemek için.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Uyarlamalı düzeni için görsel durum Yöneticisi'ni kullanma

Telefonda çalışma uygulama genellikle bir dikey veya yatay en boy oranını ve masaüstünde çalışan bir Xamarin.Forms program görüntülenebilir Xamarin.Forms birçok farklı boyutlarda ve en boy oranlarına varsayılır yeniden boyutlandırılabileceği. İyi tasarlanmış bir uygulama içeriğini bu çeşitli sayfa veya pencere form faktörleri için farklı şekilde görüntülenebilir. 

Bu teknik bazen olarak bilinen _Uyarlamalı düzeni_. Uyarlamalı düzeni bir programın görselleri yalnızca içerdiğinden görsel durum Yöneticisi'nin ideal bir uygulama bulunur.

Basit bir örnek uygulama içeriğinin etkileyen düğmeleri küçük koleksiyonunu görüntüleyen bir uygulamadır. Dikey modda yatay bir satırda sayfanın üst kısmında bu düğmeleri görüntülenebilir:

[![VSM Uyarlamalı Düzen: Dikey](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM Uyarlamalı Düzen - dikey")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

Yatay modunda düğmeler dizisi bir tarafa doğru taşınır ve bir sütunda görüntülenir:

[![VSM Uyarlamalı Düzen: Yatay](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM Uyarlamalı Düzen - yatay")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Üstten alta, programın Evrensel Windows platformu, Android ve iOS üzerinde çalışıyor.

**VSM Uyarlamalı düzeni** sayfasındaki [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) örnek "" ve "Dikey" adlı iki visual durumlarıyla "OrientationStates" adlı bir grup tanımlar. (Daha karmaşık bir yaklaşım birkaç farklı sayfa veya pencere genişlikleri dayalı olabilir.) 

XAML dosyasındaki dört basamak VSM biçimlendirme gerçekleşir. `StackLayout` Adlı `mainStack` menü ve olan içeriği içeren bir `Image` öğesi. Bu `StackLayout` dikey moda dikey yönde ve yatay modu yatay yönde olması gerekir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">

                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>

                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        
        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">

                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>

                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">

                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>

                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

İç `ScrollView` adlı `menuScroll` ve `StackLayout` adlı `menuStack` düğmeleri menüsünü uygulamak. Bu düzenleri yönünü ters, `mainStack`. Menü dikey modunda yatay ve dikey yatay modunda olmalıdır.

Dördüncü VSM biçimlendirme düğmeleri kendileri için örtük bir stilde bölümüdür. Bu biçimlendirme ayarlar `VerticalOptions`, `HorizontalOptions`, ve `Margin` portait ve yatay yönler özgü özellikleri.

Arka plan kodu dosya kümeleri `BindingContext` özelliği `menuStack` uygulamak için `Button` kumanda ve ayrıca bir işleyici iliştirir `SizeChanged` sayfasının olay:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged` İşleyicisi çağrılarını `VisualStateManager.GoToState` iki için `StackLayout` ve `ScrollView` öğeleri ve alt aracılığıyla sonra döngüler `menuStack` çağırmak için `VisualStateManager.GoToState` için `Button` öğeleri.

Arka plan kodu dosyayı XAML dosyasında öğelerin özellikleri ayarlayarak daha doğrudan yönlendirme değişiklikleri işleyebilir ancak Visual durumu kesinlikle daha yapılandırılmış bir yaklaşım yöneticisidir gibi görünebilir. Tüm görsel burada incelemek daha kolay olduklarında, XAML dosyalarında saklanacağını Bakım ve değişiklik.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Görsel durum Yöneticisi Xamarin.University ile

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 görsel durum Yöneticisi, göre [Xamarin Üniversitesi](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

