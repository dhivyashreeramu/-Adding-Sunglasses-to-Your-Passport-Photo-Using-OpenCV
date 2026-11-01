# Workshop -1 Adding Sunglasses to Your Passport Photo Using OpenCV

## AIM

To perform face augmentation using Python, OpenCV, NumPy, and Matplotlib by overlaying a transparent sunglasses image onto a face image. The experiment demonstrates image resizing, alpha channel extraction, mask generation, pixel arithmetic operations, and image blending to produce a realistic augmented image.

## SOFTWARE REQUIRED

* Python

* OpenCV

* NumPy

* Matplotlib

* Jupyter Notebook

* Anaconda (optional)

## INSTALLATION

Install the required libraries using the following command:

```
pip install opencv-contrib-python-headless numpy matplotlib
```

## ALGORITHM

### Step 1: Import Libraries

* Import OpenCV for image processing.

* Import NumPy for numerical and pixel operations.

* Import Matplotlib for displaying images.

### Step 2: Load and Display Images

* Load the face image using `cv2.imread()`.

* Display the original face image using Matplotlib.

* Load the sunglasses image with its alpha channel using `cv2.imread('sunglass.png', -1)`.

* Display the sunglasses image.

### Step 3: Resize the Sunglasses Image

* Resize the sunglasses image to 190 × 50 pixels using `cv2.resize()`.

* Check and display the resized image dimensions.

### Step 4: Separate Color and Alpha Channels

* Extract the three color channels from the sunglasses image.

* Extract the alpha channel to obtain the transparency information.

* Display the color image and alpha channel separately.

### Step 5: Apply Sunglasses Using Direct Replacement

* Create a copy of the original face image.

* Select the eye region using array slicing.

* Replace the selected eye region with the sunglasses image.

* Display the resulting image.

### Step 6: Create and Process the Mask

* Merge the alpha channel into a three-channel mask.

* Convert the mask values into binary values of 0 and 1.

* Create an inverse mask to preserve the original eye region outside the sunglasses.

### Step 7: Perform Image Arithmetic

* Extract the eye region from the original face image.

* Apply the inverse mask to the eye region using `cv2.multiply()`.

* Apply the mask to the sunglasses image.

* Combine both masked images using `cv2.add()`.

### Step 8: Generate the Augmented Image

* Replace the original eye region with the combined image.

* Display the masked eye region, masked sunglasses region, and final augmented eye region.

* Display the original face image alongside the final image with sunglasses.

## PROGRAM

# step 1: Import Libraries and Load the Face Image
# Import libraries
```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

# Load the Face Image
```
faceImage = cv2.imread('you.jpeg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")
faceImage.shape
```
# Step 2: Load the Sunglass Image with Alpha Channel
# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
```
glassPNG = cv2.imread('sunglass.png',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")
```
# Step 3: Resize the Image to Fit Over the Eye Region
# Resize the image to fit over the eye region
```
glassPNG = cv2.resize(glassPNG,(250, 80))
print("image Dimension ={}".format(glassPNG.shape))
```
# Step 4: Separate the Color and Alpha Channels
# Separate the Color and alpha channels
```
glassBGR = glassPNG[:,:,0:3]
glassMask1 = glassPNG[:,:,3]

# Display the images for clarity
plt.figure(figsize=[15,15])

plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels')

plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel')
```
# Step 5: Apply Sunglasses Using Direct Replacement
# Make a copy
```
#faceWithGlassesNaive = resized_faceImage.copy()
faceWithGlassesNaive = faceImage.copy()

# Replace the eye region with the sunglass image
faceWithGlassesNaive[260:340, 330:580]=glassBGR

plt.imshow(faceWithGlassesNaive[...,::-1])
```
# Step 6: Create the Mask and Perform Image Arithmetic
# Make the dimensions of the mask same as the input image.
# Since Face Image is a 3-channel image, we create a 3 channel image for the mask
```
glassMask = cv2.merge((glassMask1,glassMask1,glassMask1))

# Make the values [0,1] since we are using arithmetic operations
glassMask = np.uint8(glassMask/255)

# Make a copy
faceWithGlassesArithmetic = faceImage.copy()

# Get the eye region from the face image
eyeROI= faceWithGlassesArithmetic[260:340, 330:580]

# Use the mask to create the masked eye region
maskedEye = cv2.multiply(eyeROI,(1-  glassMask ))

# Use the mask to create the masked sunglass region
maskedGlass = cv2.multiply(glassBGR,glassMask)

# Combine the Sunglass in the Eye Region to get the augmented image
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Display the intermediate results
plt.figure(figsize=[20,20])
plt.subplot(131);plt.imshow(maskedEye[...,::-1]);plt.title("Masked Eye Region")
plt.subplot(132);plt.imshow(maskedGlass[...,::-1]);plt.title("Masked Sunglass Region")
plt.subplot(133);plt.imshow(eyeRoiFinal[...,::-1]);plt.title("Augmented Eye and Sunglass")
```
# Step 7: Display the Final Augmented Image
# Replace the eye ROI with the output from the previous section
```
faceWithGlassesArithmetic[260:340, 330:580]=eyeRoiFinal
```

# Display the final result
```

plt.figure(figsize=[8,10])

plt.subplot(121);plt.imshow(faceImage[:,:,::-1]); plt.title("Original Image")

plt.subplot(122);plt.imshow(faceWithGlassesArithmetic[:,:,::-1]);plt.title("With Sunglasses")
```

## OUTPUT

# The following outputs are obtained:

# 1. Original face image displayed using Matplotlib.
<img width="362" height="434" alt="image" src="https://github.com/user-attachments/assets/4a794bc8-d5a0-44f2-a789-15e0a8da2e39" />


# 2. Sunglasses image loaded and displayed.
<img width="552" height="280" alt="image" src="https://github.com/user-attachments/assets/ded7f07d-3c3a-47a9-b092-54b2f91604d6" />


# 3. Resized sunglasses image with dimensions 190 × 50 pixels.
<img width="1210" height="234" alt="image" src="https://github.com/user-attachments/assets/bca08e5f-3388-4d09-af90-13404db79c94" />

# 4. Face image with sunglasses applied using direct pixel replacement.
<img width="362" height="418" alt="image" src="https://github.com/user-attachments/assets/efaee72c-08dd-45af-8746-2025a436e092" />


# 5. Masked sunglasses region displayed.
<img width="1597" height="210" alt="image" src="https://github.com/user-attachments/assets/dd43ec9f-75a3-4782-8273-f4ae1627d1c4" />


# 6. Original face image and final augmented image displayed side by side.
<img width="685" height="415" alt="image" src="https://github.com/user-attachments/assets/3593699e-b5a4-4737-a69c-5ba02dbe593f" />


## RESULT

Thus, face augmentation was successfully performed using Python, OpenCV, NumPy, and Matplotlib. The experiment demonstrated image loading, resizing, alpha channel extraction, mask creation, pixel arithmetic, and image blending to overlay sunglasses onto a face image.
