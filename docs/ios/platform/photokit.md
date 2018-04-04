---
title: PhotoKit
description: Fotoğraf Seti sistem resim kitaplığı sorgulamak ve görüntülemek ve içeriğini değiştirmek için özel kullanıcı Arabirimi oluşturmak uygulamaları sağlar.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: c721064f62f8e2255de2b4ea2d0438e3ed630d39
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="photokit"></a>PhotoKit

_Fotoğraf Seti sistem resim kitaplığı sorgulamak ve görüntülemek ve içeriğini değiştirmek için özel kullanıcı Arabirimi oluşturmak uygulamaları sağlar._

Fotoğraf Seti sistem resim kitaplığı sorgulamak ve görüntülemek ve içeriğini değiştirmek için özel kullanıcı arabirimleri oluşturmak için uygulamalarına izin veren yeni bir çerçevedir. Birkaç görüntü ve video varlıklar yanı sıra, Albümler ve klasörler gibi varlıklar koleksiyonu temsil eden sınıfları içerir.

## <a name="model-objects"></a>Model nesneleri
Fotoğraf Seti ne model nesneleri çağırır içinde bu varlıkları temsil eder. Fotoğraflarınıza ve videolarınıza kendilerini temsil model nesneleri türlerinin `PHAsset`. A `PHAsset` varlığın medya türü ve oluşturma tarihi gibi meta veriler içeriyor.
Benzer şekilde, `PHAssetCollection` ve `PHCollectionList` sınıfları varlık koleksiyonları ve koleksiyon listeleri hakkındaki meta verileri sırasıyla içerir. Varlık koleksiyonlar tüm fotoğraf ve videoları verilen yıl gibi varlıklar gruplarıdır. Benzer şekilde, koleksiyon listeleri fotoğraflarınıza ve videolarınıza yıla göre gruplandırılan gibi varlık koleksiyonlar gruplarıdır.

## <a name="querying-model-data"></a>Model verileri Sorgulama
Fotoğraf Seti sorgu model verileri çeşitli fetch yöntemler kolaylaştırır. Örneğin, tüm görüntüleri almak için çağırırdı `PFAsset.Fetch`, geçen `PHAssetMediaType.Image` medya türü.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

`PHFetchResult` Örneği sonra içerecektir tüm `PFAsset` görüntüleri temsil eden örnekleri. Görüntüleri almak için kullandığınız `PHImageManager` (veya önbelleğe alma sürümü `PHCachingImageManager`) çağırarak görüntüsü için bir istek yapmak için `RequestImageForAsset`. Örneğin, aşağıdaki kodu her varlık için bir görüntü alır bir `PHFetchResult` bir koleksiyon görünümü hücrede görüntülemek için:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

Bu kılavuz, aşağıda gösterildiği gibi görüntülerinin sonuçlanır:

![](photokit-images/image4.png "Görüntüleri oluşan bir kılavuz görüntüleme çalışan uygulama")
 
## <a name="saving-changes-to-the-photo-library"></a>Fotoğraf Kitaplığı'na değişiklikler kaydediliyor

Sorgulamak ve verileri okuma nasıl ele alınacağını olmasıdır. Değişiklikleri de geri ve kitaplığa yazabilirsiniz. Birden çok ilgi uygulamaları sistem fotoğraf kitaplığı ile etkileşim mümkün olduğundan, bir PhotoLibraryObserver kullanarak değişikliklerin bildirilmesi için bir gözlemci kaydedebilirsiniz. Ardından, değişiklikleri geldiğinizde, uygulamanızın buna göre güncelleştirebilirsiniz. Örneğin, yukarıdaki koleksiyon görünümü yeniden yüklemek için basit bir uygulama şöyledir:

    class PhotoLibraryObserver : PHPhotoLibraryChangeObserver
    {
        readonly PhotosViewController controller;
        public PhotoLibraryObserver (PhotosViewController controller)
        
        {
            this.controller = controller;
        }
    
        public override void PhotoLibraryDidChange (PHChange changeInstance)
        {
            DispatchQueue.MainQueue.DispatchAsync (() => {
            var changes = changeInstance.GetFetchResultChangeDetails (controller.fetchResults);
            controller.fetchResults = changes.FetchResultAfterChanges;
            controller.CollectionView.ReloadData ();
            });
        }
    }
    
Gerçekte değişiklikleri uygulamanızdan yazmak için bir değişiklik isteği oluşturun. Model sınıfların her biri bir ilgili değişiklik isteği sınıfı vardır. Örneğin, bir PHAsset değiştirmek için bir PHAssetChangeRequest oluşturun. Fotoğraf Kitaplığı'na geri yazılan ve gözlemcilerin biri yukarıdaki gibi gönderilen değişiklikleri gerçekleştirme adımları şunlardır:

-   Düzenleme işlemi gerçekleştirin.
-   Filtrelenmiş görüntü verilerini PHContentEditingOutput örneğine kaydedin.
-   Değişiklikleri formu düzenleme çıkış yayımlamak için bir değişiklik isteği oluşturun.

Bir değişiklik çekirdek görüntü noir filtre uygulayan bir görüntüye geri yazan örnek aşağıda verilmiştir:

    void ApplyNoirFilter (object sender, EventArgs e)
    {
            
            Asset.RequestContentEditingInput (new PHContentEditingInputRequestOptions (), (input, options) => {
            
        // perform the editing operation, which applies a noir filter in this case
            var image = CIImage.FromUrl (input.FullSizeImageUrl);
            image = image.CreateWithOrientation((CIImageOrientation)input.FullSizeImageOrientation);
            var noir = new CIPhotoEffectNoir {
                Image = image
            };
            var ciContext = CIContext.FromOptions (null);
            var output = noir.OutputImage;
            var uiImage = UIImage.FromImage (ciContext.CreateCGImage (output, output.Extent));
            imageView.Image = uiImage;
        //
        // save the filtered image data to a PHContentEditingOutput instance
            var editingOutput = new PHContentEditingOutput(input);
            var adjustmentData = new PHAdjustmentData();
            var data = uiImage.AsJPEG();
            NSError error;
            data.Save(editingOutput.RenderedContentUrl, false, out error);
            editingOutput.AdjustmentData = adjustmentData;
        //
        // make a change request to publish the changes form the editing output
            PHPhotoLibrary.GetSharedPhotoLibrary.PerformChanges (() => {
                PHAssetChangeRequest request = PHAssetChangeRequest.ChangeRequest(Asset);
                request.ContentEditingOutput = editingOutput;
          },
          (ok, err) => Console.WriteLine ("photo updated successfully: {0}", ok));
      });
    }
    
Kullanıcı düğmesini seçtiğinde, filtre uygulanır:

![](photokit-images/image5.png "Uygulanan filtrenin örneği")
 
Ve kullanıcı geri gittiğinde PHPhotoLibraryChangeObserver sayesinde koleksiyon görünümünde değişiklik yansıtılır:

![](photokit-images/image6.png "Kullanıcı geri gittiğinde değişiklik koleksiyonu görünümünde yansıtılır")
