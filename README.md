# S15
Final Assignment


Skip to content
Search or jump to…

Pull requests
Issues
Marketplace
Explore
 
@mounikaduddukuri 
mounikaduddukuri
/
S15-1
forked from DrVenkataRajeshKumar/S15
0
01
 Code
 Pull requests 0 Actions
 Projects 0
 Wiki
 Security 0
 Insights
 Settings
S15-1
/
README.md
 

1
# S15
2
​
3
400000 images were created using only 100 foreground images and 100 background images
4
​
5
​
6
​
7
set of 7800 images were taken as a group to tryout the initial architecture of code.
8
100 bg images, 7800 fgbg images and corresponding 7800 mask images of fgbg, 7800 depth images were taken as dataset.
9
​
10
​
11
Tried transforms resize to small size to handle burden on the GPU. Tried gray scale transform because of 2 reasons.
12
1. as our predictions i.e mask images and depth images are in gray scale
13
2. to reduce the burden on GPU by reducing the weights, channels
14
​
15
only bg image and fgbg images were trained.
16
1st issue encountered was to train 2 sets of images simultaniouly.
17
Modified DNNcode so that it trains both set of images(bg and fgbg images) simultaniouly.
18
​
19
to predict mask, Depth images thought of comparing the output layers to the actual masks and depth images and calculate the loss.
20
this loss will be append to the next batch of images and so on..
21
Tried serching documentations for different loss functions and their implimentations.
22
tried different loss functons and endedup with using "BCEWithLogitsLoss"
23
​
24
​
25
Then the next issue to be addressed is ploting the loss n predictions..
26
tried implementing tensorboard, but couldnt figureout cirtain issues.
27
endedup in just ploting the image per each epoch i.e loss for mask images, loss for depth images, then mask prediction and deph prediction anf original fgbg image.
28
​
29
​
30
​
31
​
32
​
33
## A Large Custom DataSet is created using few images with fallowing steps
34
* Cars were chosed as Foreground(fg) images and
35
* Streets & Roads were chosed a Backgroung(bg) images.
36
​
37
## Data set of 100 Backgroung images of streets & roads   
@mounikaduddukuri
Commit changes
Commit summary
Update README.md
Optional extended description
Add an optional extended description…
 Commit directly to the master branch.
 Create a new branch for this commit and start a pull request. Learn more about pull requests.
 
© 2020 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
Pricing
API
Training
Blog
About
