---
title: Xamarin.Forms görsel durum Yöneticisi
description: XAML öğeleri koddan ayarlamak görsel durumlar göre değişiklik yapmak için görsel durum Yöneticisi'ni kullanın.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 0fdcbd6467547647089b436a894b1bc490ba5ee1
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854824"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms görsel durum Yöneticisi

_XAML öğeleri koddan ayarlamak görsel durumlar göre değişiklik yapmak için görsel durum Yöneticisi'ni kullanın._

Görsel durum Yöneticisi'ni (VSM) Xamarin.Forms 3. 0'da yenidir. VSM görsel değişiklikler için kullanıcı arabirimi koddan yapmak için yapılandırılmış bir yol sağlar. Çoğu durumda, uygulamanın kullanıcı arabirimi XAML içinde tanımlanır ve bu XAML görsel durum Yöneticisi kullanıcı arabirimi görsellerin nasıl etkilediğini açıklayan biçimlendirme içerir.

VSM kavramını sunar _görsel durumlar_. Bir Xamarin.Forms görünümü gibi bir `Button` temel alınan durumuna bağlı olarak birkaç farklı görsel görünümlerini olabilir &mdash; olup devre dışı bırakıldı veya basılı veya odak giriş. Bu düğmenin durumlarıdır.

Görsel durumlar toplanır _görsel durum grupları_. Bir görsel durum grubundaki tüm görsel durumları karşılıklı olarak birbirini dışlar. Görsel durumları ve görsel durum grupları hem basit metin dizelerini tarafından tanımlanır.

Xamarin.Forms görsel durum Yöneticisi ile üç görsel durumlar "CommonStates" adlı bir görsel durum grubu tanımlar:

- "Normal"
- "Devre dışı"
- "Odaklı"

Bu görsel durum grubu, türetilen tüm sınıflar için desteklenen [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), temel sınıf için [ `View` ](xref:Xamarin.Forms.View) ve [ `Page` ](xref:Xamarin.Forms.Page). 

Görsel durum grupları da tanımlayabilirsiniz ve görsel durumlar, bu makale olarak gösterilecektir.

> [!NOTE]
> Xamarin.Forms geliştiricileri aşina [Tetikleyicileri](~/xamarin-forms/app-fundamentals/triggers.md) Tetikleyicileri de bir görünümün özellikleri veya olayları eşleştirilebilir değişikliklere dayalı kullanıcı arabirimi görsellerde değişiklik yapabilir, farkındayız. Ancak, bu değişiklikleri çeşitli birleşimlerini uğraşmak için Tetikleyiciler kullanma oldukça karmaşık olabilir. Tarihsel olarak, görsel durum Yöneticisi görsel durumlar birleşimlerini kaynaklanan karmaşıklığı hafifletmek için Windows XAML tabanlı ortamlarda kullanılmaya başlandı. VSM ile görsel durum grubu içinde görsel durumları her zaman birbirini dışlar. Herhangi bir zamanda, geçerli durumu her grubu yalnızca bir durumda değil.

## <a name="the-common-states"></a>Ortak durumları

Görsel durum Yöneticisi görünümü, normal veya devre dışı bırakıldı veya giriş odağı olan bir görünümü görsel görünümünü değiştirebilirsiniz, XAML dosyasında bölümler dahil etmenize imkan sağlar. Bunlar olarak bilinen _ortak durumları_.

Örneğin, sahip olduğunuz varsayalım. bir `Entry` sayfanızdaki görünümü ve görsel görünümünü istiyorsanız `Entry` aşağıdaki yollarla değiştirmek için:

- `Entry` Bir pembe olmalıdır ne zaman arka planda `Entry` devre dışı bırakıldı.
- `Entry` Küf arka plan normalde olması gerekir.
- `Entry` Odak giriş olduğunda normal yüksekliğinin iki katı genişletmeniz gerekir.

Tek bir görünüm VSM biçimlendirme ekleyebilir veya birden çok görünüm için geçerliyse bir stilde tanımlayabilirsiniz. Sonraki iki bölümde Bu yaklaşımları açıklanmaktadır.

### <a name="vsm-markup-on-a-view"></a>Bir görünümde VSM biçimlendirme

VSM biçimlendirme eklemek için bir `Entry` görüntülemek için önce ayrı `Entry` başlangıç ve bitiş etiketleri içinde:

```xaml
<Entry FontSize="18">

</Entry>
```

Kullanacağınızdan bu durumlardan biriyle açık yazı tipi boyutu verdiği `FontSize` metin boyutunu çift özellik `Entry`.

Ardından, ekleme `VisualStateManager.VisualStateGroups` bu etiketleri arasında etiketleri:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) bir ekli bağlanabilir özelliği tarafından tanımlanan [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) sınıfı. (Eklenen bağlanabilir özellikler hakkında daha fazla bilgi için bkz [iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md).) Bu, nasıl `VisualStateGroups` özelliği eklendiği `Entry` nesne.

`VisualStateGroups` Özelliği türüdür [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList), koleksiyonu olduğu [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) nesneleri. İçinde `VisualStateManager.VisualStateGroups` etiketler, bir çift Ekle `VisualStateGroup` görsel durumlar dahil etmek istediğiniz her grup için etiketler:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Dikkat `VisualStateGroup` etiketine sahip bir `x:Name` grubunun adını belirten özniteliği. `VisualStateGroup` Sınıfı tanımlayan bir `Name` bunların yerine kullanabileceğiniz özelliği:

```xaml
<VisualStateGroup Name="CommonStates">
```

Kullanabilirsiniz `x:Name` veya `Name` ancak her ikisini birden aynı öğe.

`VisualStateGroup` Sınıfı tanımlar adlı bir özellik [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States), koleksiyonu olduğu [ `VisualState` ](xref:Xamarin.Forms.VisualState) nesneleri. `States` olan _içerik özelliği_ , `VisualStateGroups` ekleyebilirsiniz, böylece `VisualState` doğrudan arasında etiketleri `VisualStateGroup` etiketler. (İçerik özellikleri makalesinde açıklanan [temel XAML sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Sonraki adım, gruptaki her görsel durum etiketlerini çifti eklemektir. Bunlar ayrıca kullanılarak tanımlanabilir `x:Name` veya `Name`:

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

`VisualState` adlı bir özellik [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters), koleksiyonu olduğu [ `Setter` ](xref:Xamarin.Forms.Setter) nesneleri. Bunlar aynı `Setter` kullandığınız nesneleri bir [ `Style` ](xref:Xamarin.Forms.Style) nesne.

`Setters` olan _değil_ içerik özelliğinin `VisualState`, özellik öğesi etiketleri dahil etmek gerekli olacak şekilde `Setters` özelliği:

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

Artık bir veya daha fazla yerleştirebilirsiniz `Setter` her çifti arasında nesneleri `Setters` etiketler. Bunlar `Setter` daha önce açıklanan görsel durumlar tanımlama nesneler:

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

Her `Setter` etiket, bu durum geçerli olduğunda belirli bir özellik değerini belirtir. Tarafından başvurulan herhangi bir özelliği bir `Setter` bağlanabilir bir özelliğe göre nesne desteklenir.

Biçimlendirme benzer olan temel **görünümünde VSM** sayfasını **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** örnek program. Sayfanın üç içerir `Entry` görünümleri, ancak yalnızca ikinci bir bağlı VSM biçimlendirme vardır:

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

Dikkat ikinci `Entry` de sahip bir `DataTrigger` parçası olarak kendi `Trigger` koleksiyonu. Bu neden `Entry` üçüncü bir şey yazılana kadar devre dışı bırakılması `Entry`. İOS, Android ve evrensel Windows Platformu (UWP) çalışan başlangıçta sayfa şöyledir:

[![Görünüm VSM: devre dışı](vsm-images/VsmOnViewDisabled.png "VSM görünümünde - devre dışı")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Geçerli görsel durumu "devre dışı bırakıldı" Bu nedenle ikinci arka planını `Entry` iOS ve Android ekranlar pembe olduğu. UWP uygulaması `Entry` arka planını ayarlamaya izin vermiyor ne zaman renk `Entry` devre dışı bırakıldı. 

Üçüncü metin girdiğinizde `Entry`, ikinci `Entry` anahtarları "Normal" durumu ve arka plan, artık Küf:

[![Görünüm VSM: Normal](vsm-images/VsmOnViewNormal.png "görünümünde - normal VSM")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Touch ne zaman ikinci `Entry`, giriş odağı alır. "Focused" durumuna geçer ve yüksekliğinin iki katı genişletir:

[![Görünüm VSM: odaklanmış](vsm-images/VsmOnViewFocused.png "odaklı görünümü - VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Dikkat `Entry` giriş odağını aldığında Küf arka plan sürdürmez. Görsel durum Yöneticisi görsel durumları arasında geçiş gibi önceki durumuna göre özellikleri ayarlama. Görsel durumlar karşılıklı olarak birbirini dışlar göz önünde bulundurun. Yalnızca, "Normal" duruma gelmez `Entry` etkinleştirilir. Anlamına `Entry` etkinleştirilir ve giriş odağını sahip değil. 

İsterseniz `Entry` "Focused" durumda Küf arka plan için başka bir ekleme `Setter` , görsel durum:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Bu sırada `Setter` düzgün bir şekilde çalışması için nesneler bir `VisualStateGroup` içermelidir `VisualState` nesneler için o gruptaki tüm durumları. Yok görsel bir durum ise `Setter` nesneler dahil boş bir etiket olarak yine de:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Görsel durum Yöneticisi biçimlendirme stil

Genellikle, aynı görsel durum Yöneticisi biçimlendirme iki veya daha fazla görünüm arasında paylaşmak gereklidir. Bu durumda, biçimlendirme koymak isteyebilirsiniz bir `Style` tanımı.

İşte var olan örtük `Style` için `Entry` öğelerinde **VSM üzerinde görünümü** sayfası:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Ekleme `Setter` için etiketler `VisualStateManager.VisualStateGroups` bağlanılabilir özellik bağlı:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

İçerik özelliği için `Setter` olduğu `Value`, bu nedenle değerini `Value` özelliği doğrudan bu etiketleri içinde belirtilebilir. Özelliğin türü olduğunu `VisualStateGroupList`:

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

Bu etiketleri içinde bir veya daha fazla dahil edebileceğiniz `VisualStateGroup` nesneler:

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

VSM biçimlendirme geri kalanında önce aynıdır.

İşte **stilde VSM** tam VSM Biçimlendirme gösteren sayfa:

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

Artık tüm `Entry` görünümler bu sayfadaki görsel durumlarını aynı şekilde yanıt verin. Ayrıca "Focused" durumu Şimdi ikinci içerdiğini fark `Setter` her gösteren `Entry` ne zaman, odak Giriş bir Küf Ayrıca arka:

[![VSM stilde](vsm-images/VsmInStyle.png "VSM stili")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Kendi görsel durumlar tanımlama

Öğesinden türetilen her sınıf `VisualElement` üç yaygın durum "Normal", "Odaklı" ve "Devre dışı" destekler. Dahili olarak [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) sınıfı, etkin veya devre dışı veya odaklanmış veya plana odaklanmadan gelmektedir ve statik çağırır olduğunda algılar [ `VisualStateManager.GoToState` ](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) yöntemi:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

İçinde bulabileceğiniz tek bir görsel durum Yöneticisi kod budur `VisualElement` sınıfı. Çünkü `GoToState` türetilen her bir sınıfın temel her nesneye ilişkin çağrılır `VisualElement`, görsel durum Yöneticisi ile kullanabilirsiniz `VisualElement` nesne bu değişikliklerine yanıt verme.

İlginçtir ki, görsel durum grubu "CommonStates" adı açıkça içinde başvurulmuyor `VisualElement`. Grup adı API görsel durum Yöneticisi için bir parçası değil. Şu ana kadar gösterilen iki örnek program biri, "CommonStates" gruptan başka bir şeye adını değiştirebilir ve program çalışmaya devam eder. Grup durumları o gruptaki yalnızca genel bir açıklamasını adıdır. Görsel durumlar herhangi bir grup içindeki karşılıklı olarak birbirini dışlar örtük olarak anlaşılır: bir durum ve yalnızca bir herhangi bir zamanda geçerli durumudur.

Kendi görsel durumlar uygulamak istiyorsanız, çağrı gerekecektir `VisualStateManager.GoToState` koddan. Genellikle bu arama sayfası sınıfınızın arka plan kod dosyasından yapacağız.

**VSM doğrulama** sayfasını **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** örnek, giriş doğrulaması bağlantılı olarak görsel durum Yöneticisi'ni kullanmayı gösterir. XAML dosyası iki oluşur `Label` öğeleri bir `Entry`, ve `Button`:

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

VSM biçimlendirme ikinci bağlı `Label` (adlı `helpLabel`) ve `Button` (adlı `submitButton`). "Geçerli" ve "Geçersiz" adlı iki birbirini dışlayan durumlar vardır. Her iki "ValidationState" gruplarının içerdiğine dikkat edin `VisualState` etiketleri "hem geçerli" ve "Geçersiz" için bunlardan birinin her durumda boş olmasına rağmen. 

Varsa `Entry` geçerli bir telefon numarası, geçerli durum "Geçersiz" ise ve bu nedenle içermiyor ikinci `Label` görünür ve `Button` devre dışı bırakıldı:

[![VSM doğrulaması: Geçersiz durumu](vsm-images/VsmValidationInvalid.png "VSM doğrulama - geçersiz")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Geçerli bir telefon numarası girildiğinde, geçerli durum haline gelir "Geçerli". İkinci `Entry` kaybolur ve `Button` şimdi etkinleştirildi:

[![VSM doğrulama: Geçerli durumu](vsm-images/VsmValidationValid.png "VSM doğrulama - geçerli")](vsm-images/VsmValidationValid-Large.png#lightbox)

Reponsible işlemek için arka plan kod dosyasıdır `TextChanged` olaydan `Entry`. İşleyici, Giriş dizesinin geçerli olup olmadığını belirlemek için normal bir ifade kullanır. Adlı arka plan kod dosyasında yöntemi `GoToState` statik çağırır `VisualStateManager.GoToState` yöntemi hem de `helpLabel` ve `submitButton`:

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

Ayrıca dikkat `GoToState` durumu başlatmak için oluşturucudan yöntemi çağrılır. Her zaman geçerli bir durum olmalıdır. Ancak "ValidationStates" anlaşılabilir olması adına, amaçlar için XAML içinde başvuruluyor ancak hiçbir yerde kodda herhangi bir görsel durum grubu adını başvuru. 

Bu görsel durumları ve aramak için arka plan kod dosyasında her nesnenin bir hesabı etkilenen sayfasında gerçekleştirmeleri gerektiğini fark `VisualStateManager.GoToState` bu nesnelerin her biri için. İsteğe bağlı olarak bu örnekte, yalnızca iki nesneleri, ( `Label` ve `Button`), çeşitli olabilir, ancak daha fazla.

Merak ediyor: Bu görsel durumlar tarafından etkilenen her nesne arka plan kod dosyasına başvurmalıdır, neden arka plan kod dosyası yalnızca erişemiyor nesneleri doğrudan? Elbette olabilir. Ancak, nasıl görsel öğeleri denetleyebilirsiniz VSM kullanmanın avantajı olan tamamen XAML, tüm kullanıcı Arabirimi tasarımı tek bir konumda saklar farklı durumda tepki verin. Bu ayar görsel görünümünü doğrudan arka plan kod görsel öğeleri erişerek önler.

Bir sınıftan türetme dikkate alınması gereken daha cazip olabilir `Entry` ve belki de bir dış doğrulama işlevi için ayarlayabileceğiniz özellik tanımlama. Öğesinden türetilen sınıf `Entry` sonra çağırabilirsiniz `VisualStateManager.GoToState` yöntemi. Bu düzen ince, ancak yalnızca işe yarar `Entry` farklı görsel durumlar tarafından etkilenen yalnızca nesneymiş. Bu örnekte, bir `Label` ve `Button` de olması etkilenir. Bir yolu yoktur VSM biçimlendirme bağlı için bir `Entry` VSM biçimlendirme bu diğer nesneleri başka bir nesneden visual durumundaki bir değişikliği başvurmak için eklenen için diğer nesneler üzerinde sayfa ve hiçbir şekilde denetlemek için.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Görsel durum Yöneticisi için Uyarlamalı düzenini kullanma

Bir Xamarin.Forms uygulaması bir telefonda çalışan genellikle bir dikey veya yatay en boy oranı ve masaüstünde çalışan bir Xamarin.Forms program görüntülenebilir, birçok farklı boyut ve en boy oranlarına varsaymak boyutlandırılabilir. İyi tasarlanmış bir uygulama bu çeşitli sayfası veya pencere form faktörleri için farklı içeriğini görüntüleyebilir. 

Bu teknik bazen olarak bilinen _Uyarlamalı Düzen_. Uyarlamalı düzeni yalnızca bir programın görseller içerir, görsel durum Yöneticisi, ideal bir uygulama olmasıdır.

Basit bir örnek uygulama içeriğinin etkileyen düğmelerini küçük bir koleksiyonunu görüntüleyen bir uygulamadır. Dikey modda bu düğmeler yatay bir satırda sayfanın üstündeki görüntülenebilir:

[![VSM Uyarlamalı Düzen: Dikey](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM Uyarlamalı Düzen - dikey")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

Yatay modda düğmeler dizisi bir tarafa taşınmasına ve bir sütunda görüntülenir:

[![VSM Uyarlamalı Düzen: Yatay](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM Uyarlamalı Düzen - yatay")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Üstten alta program Evrensel Windows platformu, Android ve iOS üzerinde çalışıyor.

**VSM Uyarlamalı Düzen** sayfasını [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) örnek "Dikey" ve "Yatay" adlı iki görsel durumları ile "OrientationStates" adlı bir grubu tanımlar. (Daha karmaşık bir yaklaşım birkaç farklı sayfası veya pencere genişliklerini uyarlanabilir.) 

XAML dosyasında dört basamak VSM biçimlendirme oluşuyor. `StackLayout` Adlı `mainStack` menü hem olan içeriği içeren bir `Image` öğesi. Bu `StackLayout` dikey modda dikey durumuna ve yatay modda bir yatay yönde olması gerekir:

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

İç `ScrollView` adlı `menuScroll` ve `StackLayout` adlı `menuStack` menüsü düğmesi uygular. Bu düzenleri yönünü ters, `mainStack`. Menü dikey modda yatay ve dikey, yatay modda olmalıdır.

Dördüncü VSM biçimlendirme düğmeler için örtük bir stilde bölümüdür. Bu işaretleme ayarlar `VerticalOptions`, `HorizontalOptions`, ve `Margin` portait ve yatay yönleri için belirli özellikleri.

Arka plan kod dosyası kümeleri `BindingContext` özelliği `menuStack` uygulamak için `Button` komut vermeye genel ve ayrıca bir işleyici ekler `SizeChanged` sayfanın olay:

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

`SizeChanged` İşleyicisi çağrılarını `VisualStateManager.GoToState` iki `StackLayout` ve `ScrollView` öğeleri ve ardından döner alt `menuStack` çağrılacak `VisualStateManager.GoToState` için `Button` öğeleri.

Arka plan kod dosyasına yönlendirme değişikliklerinden daha doğrudan bir XAML dosyasında öğelerin özelliklerini ayarlayarak kullanabilirsiniz, ancak görsel durum Yöneticisi daha yapılandırılmış bir yaklaşım kesinlikle olduğu gibi görünebilir. Tüm görsellerin nerede bunlar incelemek daha kolay hale, XAML dosyasında tutulur, Bakım ve değişiklik.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin.University ile görsel durum Yöneticisi

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 görsel durum Yöneticisi tarafından [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

