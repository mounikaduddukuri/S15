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
Tried different approaches for overlay of foreground on background images.  
Tried GIMP- water mark tool. managed to create 1000 images at a time.  
In search of better approach used Photoshop. Recorded actions and implemented in scripts for automation for working on multiple sets of images. Managed to create 10000 images at a time i.e. placing one foreground image at 10 different places on all background images. Problem with this approach was it took almot 1hour for creating implementations and scripts and generating 10000 images. And more frustrating part is, have to creat scripts for new foreground each and every time. That means have to work 100 hour to implement with 100 foreground images.  
Finally understood the value of coding and loop implementation. 

Started coding in colab. 
Next problem encountered was to handle large number of images generated in colab.
Used zip folder, to flush, generated images into zipfile.
Finally after several attempts able to generate 400k fgbg and fgbg_mask images in jest 1 hour 30 minutes and able to store them in zip file with-out colapsing the drive.
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
3	Approach

3.1	Model Architecture

Our network is made of two component stacks, shown in Fig. 1. A coarse-scale network first predicts the depth of the scene at a global level. This is then refined within local regions by a fine-scale network. Both stacks are applied to the original input, but in addition, the coarse network’s output is passed to the fine network as additional first-layer image features. In this way, the local network can edit the global prediction to incorporate finer-scale details.

3.1.1	Global Coarse-Scale Network

The task of the coarse-scale network is to predict the overall depth map structure using a global view of the scene. The upper layers of this network are fully connected, and thus contain the entire image in their field of view. Similarly, the lower and middle layers are designed to combine information from different parts of the image through max-pooling operations to a small spatial dimension. In so doing, the network is able to integrate a global understanding of the full scene to predict the depth. Such an understanding is needed in the single-image case to make effective use of cues such
 

2
 

96		256		384		384		256	1	Coarse
									4096	
11x11 conv	5x5 conv	3x3 conv	3x3 conv	3x3 conv	full	full	
4 stride	2x2 pool									
2x2 pool						Coarse 4	Coarse 5	Coarse 6	Coarse 7
Coarse 1	Coarse 2	Coarse 3				
		63			64				64	1	Refined
											
9x9 conv				Concatenate		5x5 conv		5x5 conv	
2 stride		Fine 1			Fine 2			Fine 3	Fine 4
2x2 pool										
Input											
				Coarse			Fine
Layer	  input	  1	  2,3,4   	5	6	7	1,2,3,4

Size (NYUDepth)	304x228	37x27	18x13	8x6	1x1	74x55	74x55

Size (KITTI)	576x172	71x20	35x9	17x4	1x1	142x27	142x27

Ratio to input	/1	/8	/16	/32	–	/4	/4

Figure 1: Model architecture.

as vanishing points, object locations, and room alignment. A local view (as is commonly used for stereo matching) is insufficient to notice important features such as these.

As illustrated in Fig. 1, the global, coarse-scale network contains five feature extraction layers of convolution and max-pooling, followed by two fully connected layers. The input, feature map and output sizes are also given in Fig. 1. The final output is at 1/4-resolution compared to the input (which is itself downsampled from the original dataset by a factor of 2), and corresponds to a center crop containing most of the input (as we describe later, we lose a small border area due to the first layer of the fine-scale network and image transformations).

Note that the spatial dimension of the output is larger than that of the topmost convolutional feature map. Rather than limiting the output to the feature map size and relying on hardcoded upsampling before passing the prediction to the fine network, we allow the top full layer to learn templates over the larger area (74x55 for NYU Depth). These are expected to be blurry, but will be better than the upsampled output of a 8x6 prediction (the top feature map size); essentially, we allow the network to learn its own upsampling based on the features. Sample output weights are shown in Fig. 2

All hidden layers use rectified linear units for activations, with the exception of the coarse output layer 7, which is linear. Dropout is applied to the fully-connected hidden layer 6. The convolu-tional layers (1-5) of the coarse-scale network are pretrained on the ImageNet classification task [1]

— while developing the model, we found pretraining on ImageNet worked better than initializing randomly, although the difference was not very large1.

