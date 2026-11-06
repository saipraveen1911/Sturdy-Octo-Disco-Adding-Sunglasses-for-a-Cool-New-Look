## Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look:

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:

* Detects the face in an image.
* Places a stylish sunglass overlay perfectly on the face.
* Works seamlessly with individual passport-size photos.
* Customizable for different sunglasses styles or photo types.

## Technologies Used:

* Python
* OpenCV for image processing
* Numpy for array manipulations

## How to Use:

1.Clone this repository.
2.Add your passport-sized photo to the images folder.
3.Run the script to see your "cool" transformation!

## Applications:

* Learning basic image processing techniques.
* Adding flair to your photos for fun.
* Practicing computer vision workflows.

## Program:

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```
```
faceImage = cv2.imread('sai2.JPEG')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")
```
```
faceImage.shape
glassPNG = cv2.imread('sunglass.png',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")
```
```
glassPNG = cv2.resize(glassPNG,(152,48))
print("image Dimension ={}".format(glassPNG.shape))
glassBGR = glassPNG[:, :, 0:3].copy()
cyan_bg = (glassBGR[:, :, 0] > 150) & (glassBGR[:, :, 1] > 150) & (glassBGR[:, :, 2] < 120)
glassMask1 = np.where(cyan_bg, 0, 255).astype(np.uint8)
height, width = glassBGR.shape[:2]
gradient = np.linspace(0, 1, height)[:, None]
top_color = np.array([120, 55, 30], dtype=np.float32)    
bottom_color = np.array([190, 115, 60], dtype=np.float32)  
gradient_blue = (1 - gradient) * top_color + gradient * bottom_color
gradient_blue = np.tile(gradient_blue[:, None, :], (1, width, 1)).astype(np.uint8)
lens_area = (glassBGR[:, :, 2] > 150) & (glassMask1 == 255)
glassBGR[lens_area] = gradient_blue[lens_area]
frame_area = (glassBGR[:, :, 2] <= 150) & (glassMask1 == 255)
glassBGR[frame_area] = [50, 50, 50]
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');
```
```
faceWithGlassesNaive = faceImage.copy()
faceWithGlassesNaive[132:180,108:260]=glassBGR
plt.imshow(faceWithGlassesNaive[...,::-1])
```
```
glassMask = cv2.merge((glassMask1, glassMask1, glassMask1))
glassMask = np.uint8(glassMask / 255)
faceWithGlassesArithmetic = faceImage.copy()
eyeROI = faceWithGlassesArithmetic[132:180, 108:260]
tinted_glasses = cv2.addWeighted(glassBGR, 0.75, eyeROI, 0.25, 0)
maskedEye = cv2.multiply(eyeROI, (1 - glassMask))
maskedGlass = cv2.multiply(tinted_glasses, glassMask)
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)
plt.figure(figsize=[20, 20])
plt.subplot(131); plt.imshow(maskedEye[..., ::-1]); plt.title("Masked Eye Region")
plt.subplot(132); plt.imshow(maskedGlass[..., ::-1]); plt.title("Masked Sunglass Region")
plt.subplot(133); plt.imshow(eyeRoiFinal[..., ::-1]); plt.title("Augmented Eye and Sunglass")
```
```
faceWithGlassesArithmetic[132:180, 108:260] = eyeRoiFinal
plt.figure(figsize=[20, 20])
plt.subplot(121); plt.imshow(faceImage[:, :, ::-1]); plt.title("Original Image")
plt.subplot(122); plt.imshow(faceWithGlassesArithmetic[:, :, ::-1]); plt.title("With Sunglasses")
```

## Output:

## Face:

<img width="357" height="459" alt="image" src="https://github.com/user-attachments/assets/2dada245-8fa4-4d6e-817e-0d178c0e5151" />

## Glass:

<img width="570" height="309" alt="image" src="https://github.com/user-attachments/assets/0052a577-720c-488c-a5ee-c91e5209bebb" />

## Sunglass Color and Alpha Channel:

<img width="1106" height="214" alt="image" src="https://github.com/user-attachments/assets/1bdd183f-3d7e-4af3-80d7-d1ff40eec92b" />

## Glass and Eye Region:

<img width="358" height="438" alt="image" src="https://github.com/user-attachments/assets/6227df3b-feb0-4fdd-9688-e8f8d5d249e0" />

<img width="1105" height="166" alt="image" src="https://github.com/user-attachments/assets/1f16402f-3044-4aaf-b096-a794d2ef2f45" />

## Original Image And With Glass Image:

<img width="840" height="558" alt="image" src="https://github.com/user-attachments/assets/efcbd612-ff75-47e1-ac14-c7ef2b6ec8c2" />

## Result:

The project successfully detected the face in the input image and added sunglasses automatically using OpenCV. The output image showed the sunglasses correctly positioned on the face, demonstrating the effectiveness of basic image processing techniques.
