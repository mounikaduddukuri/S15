# S15
Final Assignment
## Data set of 100 backgroung images of streets and roads      
Background images were downloaded and resized to 224* 224 uing GIMP
![Image](https://github.com/mounikaduddukuri/S15A/blob/master/9.png)


## Data set of 100 foreground images of cars   
   
car images were downloaded, backgrounds were deleted uing 3Dpaint and car images with tranparent background were created.
 *Regreted this step in later stages of creating depth images- as it created blur margins
 
 ![Image](https://github.com/mounikaduddukuri/S15A/blob/master/97.png)




## Data set of 100 foreground masks
Tried using 3Dpaint and GIMP for creating masks of foreground images
![Image](https://github.com/mounikaduddukuri/S15A/blob/master/masks.png)





## Data set of 400k overlay images of fg on bg  
Attempted various methodologies for overlay of forefront on foundation pictures. 

Attempted GIMP-water mark instrument. figured out how to make 1000 pictures one after another. 

Looking for better methodology utilized Photoshop. Recorded activities and executed in contents for robotization for taking a shot at numerous arrangements of pictures. Figured out how to make 10000 pictures one after another for example putting one forefront picture at 10 better places on all foundation pictures. Issue with this methodology was it took almot 1hour for making usage and contents and creating 10000 pictures. What's more, all the more disappointing part is, need to creat contents for new frontal area every single time. That implies need to work 100 hour to actualize with 100 frontal area pictures. 

At last comprehended the benefit of coding and circle usage. 

Begun coding in colab. 

Next issue experienced was to deal with huge number of pictures produced in colab. 

Utilized compress organizer, to flush, produced pictures into zipfile. 

At last after a few endeavors ready to produce 400k fgbg and fgbg_mask pictures jokingly 1 hour 30 minutes and ready to store them in compress record with-out colapsing the drive.
![Image](https://github.com/mounikaduddukuri/S15A/blob/master/fgbg.png)


## Depth estimation of 400k overlay images   
Tried the Depth model reference given (https://github.com/ialhashim/DenseDepth/blob/master/DenseDepth.ipynb) and with few modifications able to implement nyu-h5 on the overlay bg-fg images.  
Depth predictions were not prominent. Tried other options for better predictions.
Tried KITTI ICCV (https://github.com/nianticlabs/monodepth2) and foundout better depth predictions than nyu-h5. My intusion for poor depth prediction of few fg images is (*As menctioned erlier) becaue of poor selection of foregroung images i.e with some what blur margins
![Image](https://github.com/mounikaduddukuri/S15A/blob/master/depth.png)

## Links to datasets and masks
- [FG](https://drive.google.com/drive/folders/1RNx8BeqfDozTj3x-u4hcmBTgYNnQ3j3u?usp=sharing)
- [BG](https://drive.google.com/drive/folders/1LcmPUh3VkEmv_9ewti4CHgpu8HlJIVnJ?usp=sharing)
- [FG_BG](https://drive.google.com/file/d/1S6Wwd_9-JD6vlIFZSO5wPDPNUcl2N_qZ/view?usp=sharing)
- [DEPTH_MASK](https://drive.google.com/file/d/1XyfWCBXgQlXQlYDiT9JcQJggf5Y3jcBv/view?usp=sharing)
- [FB_BG_MASK](https://drive.google.com/file/d/1ln63ZcMPfoKHou6K1OFAJ3xmdy_CnIPt/view?usp=sharing)


## Model 


### Model Architecture

Our network is made of two component stacks, shown in Fig. 1. A coarse-scale network first predicts the depth of the scene at a global level. This is then refined within local regions by a fine-scale network. Both stacks are applied to the original input, but in addition, the coarse network’s output is passed to the fine network as additional first-layer image features. In this way, the local network can edit the global prediction to incorporate finer-scale details.

### Global Coarse-Scale Network

The task of the coarse-scale network is to predict the overall depth map structure using a global view of the scene. The upper layers of this network are fully connected, and thus contain the entire image in their field of view. Similarly, the lower and middle layers are designed to combine information from different parts of the image through max-pooling operations to a small spatial dimension. In so doing, the network is able to integrate a global understanding of the full scene to predict the depth. Such an understanding is needed in the single-image case to make effective use of cues such
 


### Model architecture.

as vanishing points, object locations, and room alignment. A local view (as is commonly used for stereo matching) is insufficient to notice important features such as these.

As illustrated in Fig. 1, the global, coarse-scale network contains five feature extraction layers of convolution and max-pooling, followed by two fully connected layers. The input, feature map and output sizes are also given in Fig. 1. The final output is at 1/4-resolution compared to the input (which is itself downsampled from the original dataset by a factor of 2), and corresponds to a center crop containing most of the input (as we describe later, we lose a small border area due to the first layer of the fine-scale network and image transformations).

[Example of the image with the framework](https://github.com/mounikaduddukuri/S15/blob/master/Capture.PNG)

Note that the spatial dimension of the output is larger than that of the topmost convolutional feature map. Rather than limiting the output to the feature map size and relying on hardcoded upsampling before passing the prediction to the fine network, we allow the top full layer to learn templates over the larger area (74x55 for NYU Depth). These are expected to be blurry, but will be better than the upsampled output of a 8x6 prediction (the top feature map size); essentially, we allow the network to learn its own upsampling based on the features.

All hidden layers use rectified linear units for activations, with the exception of the coarse output layer 7, which is linear. Dropout is applied to the fully-connected hidden layer 6. The convolu-tional layers (1-5) of the coarse-scale network are pretrained on the ImageNet classification task.

