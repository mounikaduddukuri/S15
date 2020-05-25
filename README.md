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
