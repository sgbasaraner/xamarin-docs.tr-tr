---
title: ARKit Xamarin.iOS içinde UrhoSharp kullanma
description: Bu belgede bir Xamarin.iOS ARKit uygulamada ayarlama açıklanır ve nasıl çerçeveler, kamera ayarlamak nasıl işlendiğini konumunda görünür nasıl düzeyi, aydınlatma ve daha fazlası ile çalışmayı öğrenin. Ayrıca, HoloLens için kod yazma ve UrhoSharp anlatılmaktadır.
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2017
ms.openlocfilehash: 728082eb27684c2176feb2038b7948986ce6a694
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351697"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>ARKit Xamarin.iOS içinde UrhoSharp kullanma

Sunulmasıyla birlikte [ARKit](https://developer.apple.com/arkit/), Apple vermiştir, genişletilmiş gerçeklik uygulamaları oluşturmak, geliştiriciler için basit. ARKit cihazınızın görselinizin tam konumunu izleyebilir ve tüm dünyaya çeşitli yüzeyleri algılamak ve ardından dışında ARKit kodunuza gelen verileri karıştırmak için geliştirici kadar olan.

[UrhoSharp](~/graphics-games/urhosharp/index.md) 3B uygulamalar oluşturmak için kullanabileceğiniz kapsamlı ve kullanımı kolay 3B API'si sağlar.   Bunların her ikisi de olabilir birlikte dünyanın fiziksel bilgi sağlamak için ARKit ve sonuçları işlemek için Urho karışık.

Bu sayfada birlikte harika genişletilmiş gerçeklik uygulamaları oluşturmak için bu iki platformdan da bağlanma açıklanır.


## <a name="the-basics"></a>Temeller

Yapmak istediğimiz dünyanın en üstünde mevcut 3B içerik iPhone tarafından görülen aynıdır.   3B içeriğe sahip telefonun kameradan gelen içeriği gibi işlevlerle birleştirmek ister uygulamadır ve telefonun kullanıcı emin olmak için odaya taşınırken 3B nesne davranış olduğu gibi bu yer parçası - bu nesneler bu dünyaya bağlama tarafından gerçekleştirilir.

![ARKit animasyonlu şekilde](urhosharp-images/image1.gif)


Biz Urho kitaplığı bizim 3B varlıkları yükleme ve bunları dünya üzerinde yerleştirmek için kullanacağınız ve kamera ve bunun yanı sıra telefon, dünya konumu geldiğini video akışını almak için ARKit biz kullanacaklardır.   Kullanıcının kendi telefon ile hareket ettikçe Urho altyapısı görüntüleme koordinat sistemini güncelleştirmek için konumda değişiklikleri kullanacağız.

Bu şekilde bir nesneyi 3B alanda yerleştirmek ve kullanıcı taşır, bu yerleştirildiği konum ve yer 3B nesnenin konumu yansıtır.

## <a name="setting-up-your-application"></a>Uygulamanızı ayarlama

### <a name="ios-application-launch"></a>iOS uygulamasını başlatma

İOS uygulamanızın oluşturun ve 3B içeriğinize başlatmak için gereken, öğesinin bir uygulama oluşturarak bunu [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) ve Kurulum kodunuzu geçersiz kılarak sağlamak `Start` yöntemi.  Burada, Sahne, olay işleyicileri olan kurulum vb. doldurulmuş budur.

Ekledik bir `Urho.ArkitApp` alt sınıflara ayıran sınıfı `Urho.Application` ve kendi `Start` yöntemi kaynaklanan ağır yüklerden yapar.   Tüm yapmanız gereken uygulama, var olan Urho temel sınıf türünde olması değiştirmek `Urho.ArkitApp` ve dünyada, urho Sahne çalışır bir uygulamanız varsa.

### <a name="the-arkitapp-class"></a>ArkitApp sınıfı

Bu sınıf, işletim sistemi tarafından teslim edilen haliyle bir dizi kullanışlı Varsayılanları, bazı anahtar nesneleri ile Sahne hem yanı sıra ARKit olayların işlenmesini sağlar.

Kurulumu gerçekleşir `Start` sanal yöntem.   Ebeveyniniz zincirdeki kullanarak emin olmak gerekir, alt üzerinde bu yöntemi geçersiz kıldığınızda, `base.Start()` kendi uygulama.

`Start` Yöntemi görünüm, Görünüm penceresi, kamera ve bir Yönlü ışık ayarlar ve ortak özellikler olarak ortaya çıkarır:

- bir [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) , nesneleri depolamak için
- yönlü [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) shadows ve konumu aracılığıyla kullanılabilir olan `LightNode` özelliği
- bir [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) bileşenlerinde ARKit uygulamaya bir güncelleştirme sunduğunda güncelleştirilir ve
- bir [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) sonuçları görüntüleme.


### <a name="your-code"></a>Kodunuzu

Ardından alt sınıfı için ihtiyacınız `ArkitApp` sınıf ve geçersiz kılma `Start` yöntemi.   Yönteminizi yapmanız gereken ilk şey zinciri kadar olan `ArkitApp.Start` çağırarak `base.Start()`.  Bundan sonra Özellikler Kurulumu tarafından ArkitApp dilediğinizi kullanabilirsiniz nesnelerinizi sahneye eklemek için ışıkları, gölgeler veya kullanmak istediğiniz olayları özelleştirme.

ARKit/UrhoSharp örnek dokular animasyonlu bir karakterle yükler ve aşağıdaki uygulama ile animasyon oynar:

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

Ve gerçekten tüm genişletilmiş gerçeklik modunda görüntülenen 3B içeriğinize sağlamak için bu noktada yapmanız gerekir.

Bu biçime varlıklarınızı dışarı aktarmak gereken şekilde Urho 3B modeller ve animasyonlar için özel biçimler kullanır.   Araçları gibi kullanabileceğiniz [Urho3D Blender eklentisi](https://github.com/reattiva/Urho3D-Blender) ve [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , dönüştürebilir bu varlıkları DBX, DAE, OBJ, Blend gibi popüler biçimlerinden 3B-Max biçime Urho tarafından gerekli.

Urho kullanarak 3B uygulamalar oluşturma hakkında daha fazla bilgi edinmek için [UrhoSharp giriş](~/graphics-games/urhosharp/introduction.md) Kılavuzu.

## <a name="arkitapp-in-depth"></a>Derinlemesine ArkitApp

> [!NOTE]
> Bu bölümde UrhoSharp ARKit ve varsayılan deneyim özelleştirmek istiyorsanız ya da tümleştirme birlikte nasıl çalıştığı hakkında daha ayrıntılı bir öngörü almak istediğiniz geliştiricilere yöneliktir.   Bu bölümde okumak gerekli değildir.

ARKit API oldukça basittir, oluşturma ve yapılandırma bir [ARSession](https://developer.apple.com/documentation/arkit/arsession) hangi nesne sonra sunmaya başlayın [ARFrame](https://developer.apple.com/documentation/arkit/arframe) nesneleri.   Bu cihaz tahmini gerçek konumunu yanı sıra kamera tarafından yakalanan görüntü, her iki içerir.

Biz bir kamera tarafından bize 3B içeriklerimizde teslim edilen görüntüleri oluşturma ve kameraya UrhoSharp olasılığını cihaz konumu ve konumu eşleşecek şekilde ayarlayın.

Yer alan aşağıdaki diyagramda gösterilmiştir `ArkitApp` sınıfı:

[![Sınıfları ve ArkitApp ekranlara diyagramı](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Çerçeve işleme

Basit bir uygulamadır, birleştirilmiş görüntü üretmek için sunduğumuz 3B grafikler ile kamera dışında yakında video birleştirin.     Biz bu yakalanan görüntülerin bir dizi sırayla alma ve biz bu giriş Urho Sahne ile karışık.

Eklemek için bunu yapmanın en kolay yolu olan bir [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) ana [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Bu, tek bir çerçeve çizmek için gerçekleştirilen komutları kümesidir.  Bu komut, görünüm penceresinin biz geçirin herhangi bir doku ile doldurur.    Bu işlem ilk karede ayarlar ve gerçek tanımı içinde th yapılır **ARRenderPath.xml** bu noktada yüklenen dosyası.

Ancak Biz bu iki platformdan da karıştırarak için iki sorunu kalmaktadır:


1. İOS, GPU dokular ikinin üssü bir çözüm sahip olmalıdır, ancak biz kameradan alacak çerçeveleri gibi ikinin, cihazlar çözüm yok: 1280 x 720.
2. Çerçeve içinde kodlanır [YUV](https://en.wikipedia.org/wiki/YUV) iki görüntü tarafından - luma ve kroma temsil biçimi.

İki farklı çözünürlükte YUV çerçeveleri gelir.  aydınlatma (gri tonlamalı görüntü temelde) ve çok daha küçük 640 x 360 chrominance bileşeni için temsil eden bir 1280 x 720 görüntüsü:

![UV bileşenleri ve birleştirme Y gösteren görüntü](urhosharp-images/image3.png)


OpenGL ES kullanarak tam renkli görüntüsünü çizmek için biz aydınlatma (Y bileşeni) ve chrominance (UV düzlemleri) doku yuvaları alan küçük bir gölgelendirici yazmanız gerekir.  İçinde UrhoSharp adlarına - "sDiffMap" ve "sNormalMap" sahiptir ve bunları RGB biçime Dönüştür:

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

İki çözüm gücünü yok doku işlemek için aşağıdaki parametrelerle Texture2D tanımlamak sunuyoruz:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Bu nedenle arka plan olarak yakalanan görüntülerini işlemek ve gibi scary mutant üzerindeki herhangi bir Sahneyi işleme olanağına duyuyoruz.

### <a name="adjusting-the-camera"></a>Kamera ayarlama

`ARFrame` Nesneleri tahmini cihaz konumu da içerir.  Biz ARFrame game kamerayı hareket ihtiyacımız - ARKit önce bir sorun teşkil izleme cihaz yönü (toplama, aralık ve yaw) ve işleme video üzerine sabitlenmiş bir hologramı - fakat Cihazınızı biraz taşırsanız - hologramları değildi artık farklı.

Jiroskop gibi yerleşik sensör hareketlerini izlemek mümkün değildir çünkü bu durumda, yalnızca hızlandırma yapabilirler.  Her bir dilimi ve ayıklar özellik noktalarını izlemek için ve bu nedenle doğru bir bulunun mümkün ARKit çözümlemeleri taşıma ve dönüş verileri içeren matris dönüştürün.

Örneğin, bu size geçerli konumun nasıl elde edebilirsiniz.

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Kullandığımız `-row.Z` çünkü ARKit bir sağ taraf yönelimli koordinat sistemini kullanır.


### <a name="plane-detection"></a>Düzlem algılama

ARKit yatay düzlemleri algılayabilir ve gerçek dünyayla etkileşimde bulunmak bu yeteneği sağlar, örneğin, biz mutant gerçek bir tablo ya da bir zemin üzerinde yerleştirebilirsiniz. Bunu yapmanın en kolay yolu HitTest yöntemi (koruyabilmesi) kullanmaktır. Ekran koordinatları dönüştürür (0,5; 0,5 Merkezi) gerçek dünya koordinatları içinde (0; 0 0, ilk çerçeve konumdur).

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

Şimdi biz bağlı olarak yatay bir yüzeyi biz dokunun cihaz ekranında mutant yerleştirebilirsiniz:

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![Animasyonlu şekil değiştirme görünümü hareket ettikçe düzeyi](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Gerçekçi aydınlatma

Gerçek dünya ışık koşullara göre daha açık veya koyu kendi ortamı daha iyi eşleşecek şekilde sanal Sahne olmalıdır. ARFrame Urho çevresel ışık ayarlamak için kullanabileceğiniz bir LightEstimate özelliği içeren yapıldığını şöyle:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>İOS - HoloLens

UrhoSharp [tüm önemli işletim sistemlerinde çalışan](~/graphics-games/urhosharp/platform/index.md), başka bir yerde mevcut kodunuzu yeniden kullanabilirsiniz.

HoloLens en heyecan verici platformları üzerinde çalıştığı biridir.   Bu, kolayca UrhoSharp kullanma harika genişletilmiş gerçeklik uygulamaları oluşturmak için HoloLens ve iOS arasında geçiş yapabilirsiniz, anlamına gelir.

MutantDemo kaynakta bulabilirsiniz [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>İlgili bağlantılar

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (ile UrhoSharp) (örnek)](https://github.com/EgorBo/ARKitXamarinDemo)
