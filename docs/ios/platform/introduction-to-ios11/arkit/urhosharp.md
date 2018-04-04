---
title: ARKit UrhoSharp ile kullanma
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 95c9c602d0bfe1b77fda453a137dfdfc12a975c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="using-arkit-with-urhosharp"></a>ARKit UrhoSharp ile kullanma

Girişiyle [ARKit](https://developer.apple.com/arkit/), Apple yaptı, geliştiricilerin genişletilmiş gerçekte uygulamaları oluşturmak için basit. ARKit Cihazınızı tam konumunu izleyebilir ve dünya üzerindeki çeşitli yüzeyleri algılamak ve sonra kodunuza dışında ARKit gelen veriler blend Geliştirici kadar olur.

[UrhoSharp](~/graphics-games/urhosharp/index.md) 3B uygulamaları oluşturmak için kullanabileceğiniz bir kapsamlı ve kullanımı kolay 3B API sağlar.   Bunların her ikisi de olabilir birlikte, dünya hakkında fiziksel bilgi sağlamak için ARKit ve sonuçları işlemek için Urho karıştırılan.

Bu sayfa, birlikte harika genişletilmiş gerçekte uygulamaları oluşturmak için bu iki dünyanın bağlanmak açıklanmaktadır.


## <a name="the-basics"></a>Temeller

Yapmak istiyoruz mevcut 3B dünyanın en üstünde iPhone tarafından görülen içeriktir.   3B içerikle telefonun kameradan gelen içeriği karıştırmak için olur ve kullanıcının telefon emin olmak için yer taşınırken 3B nesne davranır olduğu gibi bu yer parçası - bu nesneleri bu dünyaya sabitleme tarafından gerçekleştirilir.

![Animasyonlu şekilde ARKit](urhosharp-images/image1.gif)


Urho kitaplığı biz bizim 3B varlıklar yüklemek ve bunları dünya yerleştirmek için kullanacağınız ve biz ARKit kamera ve bunun yanı sıra telefon dünyadaki konumunu'ten gelen video akışına almak için kullanır.   Kullanıcı kendi telefon ile taşınırken, Urho altyapısı görüntüleme koordinat sistemini güncelleştirmek için konumda değişiklikleri kullanacağız.

Bu şekilde, 3B alanda bir nesne getirin ve kullanıcı geçerse, 3B nesnenin konumunu yerinde ve burada yerleştirilen konumu yansıtır.

## <a name="setting-up-your-application"></a>Uygulamanızı kurma

### <a name="ios-application-launch"></a>iOS uygulamasını başlatma

İOS uygulamanızı oluşturmak ve 3B içeriğiniz başlatmak gereken, bir uygulama öğesinin bir alt kümesi oluşturarak bunu [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) ve Kurulum kodunuzu kılarak sağlamak `Start` yöntemi.  Burada, Sahne verilerle, olay işleyicileri olan kurulum vb. doldurulmuş budur.

Size sunulan bir `Urho.ArkitApp` alt sınıf `Urho.Application` ve kendi `Start` yöntemi ağır lifting yapar.   Tüm yapmanız gereken uygulama, varolan Urho türünde olması için temel sınıf değiştirin `Urho.ArkitApp` ve, urho Sahne dünyada çalışacak bir uygulamaya sahip.

### <a name="the-arkitapp-class"></a>ArkitApp sınıfı

Bu sınıf, işletim sistemi tarafından teslim edilir uygun Varsayılanları, hem bir Sahne bazı anahtar nesnelerle kümesi ve aynı zamanda ARKit olayları işleme sağlar.

Kurulum gerçekleşir `Start` sanal yöntemi.   Bu yöntem, bir alt kümesi üzerinde geçersiz kıldığınızda, ana zinciri için kullanarak emin olmanız gerekir `base.Start()` kendi uygulama.

`Start` Yöntemi Sahne, Görünüm penceresi, kamera ve tek yönlü ışığı ayarlar ve bu ortak özellikleri olarak ortaya çıkarır:

- bir [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) , nesneleri depolamak için
- bir yön [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) gölgeleri ve konumu aracılığıyla kullanılabilir olan `LightNode` özelliği
- bir [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) bileşenleri ARKit bir güncelleştirme uygulamaya teslim ederken güncelleştirilir ve
- bir [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) sonuçları görüntüleme.


### <a name="your-code"></a>Kodunuz

Ardından alt sınıf gereken `ArkitApp` sınıfı ve geçersiz kılma `Start` yöntemi.   Yönteminizi yapmanız gereken ilk şey zinciri kadar olan `ArkitApp.Start` çağırarak `base.Start()`.  Bundan sonra ArkitApp tarafından özellikler Kurulumu nesnelerinizi Sahne eklemek için ışık, gölge veya ele almak istediğiniz olayları özelleştirme için kullanabilirsiniz.

ARKit/UrhoSharp örnek doku animasyonlu bir karakterle yükler ve aşağıdaki uygulama ile animasyon oynar:

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

Ve gerçekten genişletilmiş gerçekte görüntülenmesini 3B içeriğinizi sağlamak için bu noktada yapmanız gereken tüm bu.

Varlıklarınızı bu biçimine dışa aktarmak gereken şekilde Urho 3B modeller ve animasyonları için özel biçimler kullanır.   Gibi araçları kullanabilirsiniz [Urho3D Blender eklenti](https://github.com/reattiva/Urho3D-Blender) ve [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , dönüştürebilir bu varlıkları DBX, DAE, OBJ, harmanlama, gibi popüler biçimlerden 3B-Max biçime Urho tarafından gerekli.

Urho kullanarak 3B uygulamaları oluşturma hakkında daha fazla bilgi için [UrhoSharp giriş](~/graphics-games/urhosharp/introduction.md) Kılavuzu.

## <a name="arkitapp-in-depth"></a>Derinlemesine ArkitApp

> [!NOTE]
> Bu bölüm, UrhoSharp ve ARKit varsayılan deneyimini özelleştirmek istiyorsanız veya tümleştirme nasıl çalıştığı hakkında daha derin bir hakkında bilgi edinme geliştiriciler için hazırlanmıştır.   Bu bölümde okumak gerekli değildir.

ARKit API oldukça basittir, oluşturma ve yapılandırma bir [ARSession](https://developer.apple.com/documentation/arkit/arsession) hangi nesne sonra teslim başlatın [ARFrame](https://developer.apple.com/documentation/arkit/arframe) nesneleri.   Bu cihaz tahmini gerçek konumunu yanı sıra kamera tarafından yakalanan görüntü, her iki içerir.

Biz kamera tarafından bize bizim 3B içerikle teslim edilmesini görüntüleri oluşturma ve olması olasılığını aygıt konumu ve konumu eşleşecek şekilde UrhoSharp kamerayı ayarlayın.

Aşağıdaki diyagramda yer aldığı gösterilmektedir `ArkitApp` sınıfı:

[![Sınıfları ve ArkitApp ekranları diyagramı](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Çerçeve oluşturma

Basit bir fikirdir, kamera birleştirilmiş görüntü üretmek için bizim 3B grafik ile dışında gelen video birleştirin.     Şu anda yakalanan bu görüntüleri bir dizi sırayla alınıyor ve biz bu girişi ile Urho Sahne karışık.

Bunu yapmanın en kolay yolu eklemektir bir [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) ana [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Bu, tek bir çerçeve çizmek için gerçekleştirilen komutlar kümesidir.  Bu komut, görünüm penceresinin biz geçirmek doku ile doldurur.    Bu işlem ilk çerçevesi ayarlar yapılıyor ve gerçek tanımı içinde th yapılır **ARRenderPath.xml** bu noktada yüklenen dosyası.

Ancak, biz bu iki dünyanın birlikte karıştırmak için iki sorunları karşılaştığı:


1. İos'ta, GPU dokuları iki gücünü çözünürlüğü olmalıdır, ancak biz kameradan alırsınız çerçeveler güç iki, örneğin: çözümleme gerekmez: 1280 x 720.
2. Çerçeve içinde kodlanır [YUV](https://en.wikipedia.org/wiki/YUV) iki görüntü tarafından - luma ve berraklığının temsil biçimi.

YUV çerçeveler iki farklı çözümler gelir.  aydınlatma (gri ölçek görüntü temelde) ve çok daha küçük 640 x 360 chrominance bileşeni için temsil eden bir 1280 x 720 görüntüsü:

![Birleştirme Y ve UV bileşenleri gösteren resim](urhosharp-images/image3.png)


OpenGL ES kullanarak tam bir renkli resim çizmek için biz aydınlatma (Y bileşeni) ve chrominance (UV düzlemleri) doku yuvaları geçen küçük bir gölgelendirici yazmak zorunda.  UrhoSharp içinde adları - "sDiffMap" ve "sNormalMap" olması ve RGB biçime Dönüştür:

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

İki çözüm gücünü yok doku işlemek için aşağıdaki parametrelerle Texture2D tanımlamak sahibiz:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Böylece biz yakalanan görüntülerin arka plan olarak işlemek ve yukarıdaki tüm Sahne gibi korkutucu mutant işlemek kullanabilirsiniz.

### <a name="adjusting-the-camera"></a>Kamera ayarlama

`ARFrame` Nesneleri tahmini aygıt konumu da içerir.  Biz oyun kamera ARFrame taşımak ihtiyacımız - ARKit önce bir sorun teşkil izleme cihaz yönlendirmesini (Top, aralık ve yaw) ve işleme sabitlenmiş hologramı video üstünde - ancak Cihazınızı biraz taşırsanız - edilmesi değildi artık kayma.

Jiroskop gibi yerleşik algılayıcılar hareketleri izleyebilmesi için olmadığından bu durum, yalnızca hızlandırmasını olabilir.  Her çerçeve ve ayıklar özellik noktalarını izlemek için ve bu nedenle bize doğru bir vermek ARKit çözümlemeler taşıma ve dönüş verileri içeren matris dönüştürün.

Örneğin, bu size geçerli konumu nasıl elde edebilirsiniz.

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Kullanırız `-row.Z` çünkü ARKit sağ koordinat sistemi kullanır.


### <a name="plane-detection"></a>Düzlemi algılama

ARKit yatay düzlemleri algılayabilir ve bu özellik, gerçek dünya ile etkileşime olanak sağlar, örneğin, biz mutant gerçek bir tablo veya kat yerleştirebilirsiniz. Bunu yapmanın en kolay yolu, HitTest yöntemi (raycasting) kullanmaktır. Ekran koordinatları dönüştürür (0,5; 0,5 Center) gerçek dünya koordinat içine (0; 0 0 olan ilk çerçeve konumu).

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

Şimdi biz nereye bağlı olarak yatay yüzeyini biz dokunun aygıt ekranında mutant yerleştirebilirsiniz:

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

![Görünüm hareket ettikçe değiştirme animasyonlu şekil düzeyi](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Gerçekçi ışık

Gerçek dünya aydınlatma koşullarına bağlı olarak sanal Sahne daha iyi çevresindeki öğelerden eşleşecek şekilde koyu veya açık olması gerekir. ARFrame içeren biz Urho ortam ışığı ayarlamak için kullanabileceğiniz bir LightEstimate özelliği bu yapılır şöyle:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>İOS - HoloLens

UrhoSharp [önde gelen tüm işletim sistemlerinde çalışan](~/graphics-games/urhosharp/platform/index.md), başka bir yerde mevcut kodunuzu yeniden kullanabilirsiniz.

HoloLens, üzerinde çalıştığı en heyecan verici platformları biridir.   Bu, kolayca UrhoSharp kullanarak harika Engagement'ta gerçekte uygulamaları oluşturmak için HoloLens ve iOS arasında geçiş yapabilirsiniz, anlamına gelir.

MutantDemo kaynakta bulabileceğiniz [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>İlgili bağlantılar

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [(İle UrhoSharp) ARKitXamarinDemo (örnek)](https://github.com/EgorBo/ARKitXamarinDemo)
