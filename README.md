# Cartoonify-an-Image-using-OpenCV

This project aims at converting any image given by the user and converting it into a cartoon image.
The resulting cartoon image can be saved in the same place as the original image with the name "cartoonified_Image.png"

## Steps to develop the project

### Step 1 : Import the reqired modules

- CV2 : Import to use OpenCV for image processing.
- easygui: Imported to open a file box. It allows us to select any file from our system.
- Numpy: Images are stored and processed as numbers. These are taken as arrays. We use NumPy to deal with arrays.
- Imageio: Used to read the file which is chosen by file box using a path.
- Matplotlib: This library is used for visualization and plotting. Thus, it is imported to form the plot of images.
- OS: For OS interaction. Here, to read the path and save images to that path.

``` python
import cv2 #for image processing
import easygui #to open the filebox
import numpy as np #to store image
import imageio #to read image stored at particular path
import sys
import matplotlib.pyplot as plt
import os
import tkinter as tk
from tkinter import filedialog
from tkinter import *
from PIL import ImageTk, Image
```

### Step 2 : Building a File Box to choose a particular file

In this step, we will build the main window of our application, where the buttons, labels, and images will reside. We also give it a title by title() function.

``` python
def upload():
    ImagePath=easygui.fileopenbox()
    cartoonify(ImagePath)
```

### Step 3 :  How is an image stored?

For a computer, everything is just numbers. Thus, in the below code, we will convert our image into a numpy array.

``` python
#read the image
    originalmage = cv2.imread(ImagePath)
    originalmage = cv2.cvtColor(originalmage, cv2.COLOR_BGR2RGB)
#print(image)  # image is stored in form of numbers
# confirm that image is chosen
    if originalmage is None:
        print("Can not find any image. Choose appropriate file")
        sys.exit()
    ReSized1 = cv2.resize(originalmage, (960, 540))
#plt.imshow(ReSized1, cmap='gray')
```

### Step 4 : Transforming an image to grayscale

``` python
#converting an image to grayscale
grayScaleImage = cv2.cvtColor(originalmage, cv2.COLOR_BGR2GRAY)
ReSized2 = cv2.resize(grayScaleImage, (960, 540))
#plt.imshow(ReSized2, cmap='gray')
```

### Step 5 : Smoothening a grayscale image

``` python
#applying median blur to smoothen an image
smoothGrayScale = cv2.medianBlur(grayScaleImage, 5)
ReSized3 = cv2.resize(smoothGrayScale, (960, 540))
#plt.imshow(ReSized3, cmap='gray')
```

### Step 6: Retrieving the edges of an image

``` python
#retrieving the edges for cartoon effect
#by using thresholding technique
getEdge = cv2.adaptiveThreshold(smoothGrayScale, 255, 
  cv2.ADAPTIVE_THRESH_MEAN_C, 
  cv2.THRESH_BINARY, 9, 9)
ReSized4 = cv2.resize(getEdge, (960, 540))
#plt.imshow(ReSized4, cmap='gray')
```
### Step 7 : Preparing a Mask Image

``` python
#applying bilateral filter to remove noise 
#and keep edge sharp as required
colorImage = cv2.bilateralFilter(originalmage, 9, 300, 300)
ReSized5 = cv2.resize(colorImage, (960, 540))
#plt.imshow(ReSized5, cmap='gray')
```

### Step 8: Giving a Cartoon Effect

``` python
#masking edged image with our "BEAUTIFY" image
cartoonImage = cv2.bitwise_and(colorImage, colorImage, mask=getEdge)
ReSized6 = cv2.resize(cartoonImage, (960, 540))
#plt.imshow(ReSized6, cmap='gray')
```

### Step 9: Plotting all the transitions together

``` python
# Plotting the whole transition
images=[ReSized1, ReSized2, ReSized3, ReSized4, ReSized5, ReSized6]
fig, axes = plt.subplots(3,2, figsize=(8,8), subplot_kw={'xticks':[], 'yticks':[]}, gridspec_kw=dict(hspace=0.1, wspace=0.1))
for i, ax in enumerate(axes.flat):
    ax.imshow(images[i], cmap='gray')
//save button code
plt.show()
```

### Step 10: Functionally of save button

``` python
def save(ReSized6, ImagePath):
    #saving an image using imwrite()
    newName="cartoonified_Image"
    path1 = os.path.dirname(ImagePath)
    extension=os.path.splitext(ImagePath)[1]
    path = os.path.join(path1, newName+extension)
    cv2.imwrite(path, cv2.cvtColor(ReSized6, cv2.COLOR_RGB2BGR))
    I = "Image saved by name " + newName +" at "+ path
    tk.messagebox.showinfo(title=None, message=I)
```
### Step 11: Making the main window
``` python
top=tk.Tk()
top.geometry('400x400')
top.title('Cartoonify Your Image !')
top.configure(background='white')
label=Label(top,background='#CDCDCD', font=('calibri',20,'bold'))
```

### Step 12: Making the Cartoonify button in the main window

``` python
upload=Button(top,text="Cartoonify an Image",command=upload,padx=10,pady=5)
upload.configure(background='#364156', foreground='white',font=('calibri',10,'bold'))
upload.pack(side=TOP,pady=50)
```

### Step 13: Making a Save button in the main window

``` python
save1=Button(top,text="Save cartoon image",command=lambda: save(ImagePath, ReSized6),padx=30,pady=5)
save1.configure(background='#364156', foreground='white',font=('calibri',10,'bold'))
save1.pack(side=TOP,pady=50)
```

### Step 14: Main function to build the tkinter window

``` python
top.mainloop()
```


> We have successfully developed Image Cartoonifier with OpenCV in Python.
