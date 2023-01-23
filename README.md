# ImageColorExtracter

Introducing a powerful tool for extracting colors from images using Python. This tool allows you to easily extract the dominant colors from any image, providing you with a palette of colors that can be used for a variety of purposes, such as color matching, color correction, or as a starting point for creating a color scheme.
The tool is built using Python and a variety of popular libraries, such as OpenCV and NumPy, making it easy to use and highly customizable. It uses advanced image processing techniques to analyze the image and extract the most dominant colors, providing you with a palette of colors that accurately represents the image.

![1_y3gk0WfLC-JImRFdgZMExw](https://user-images.githubusercontent.com/101708836/214133174-0ff784e9-eb5c-4915-b240-14f94884439b.jpg)

![1_y3MZS2OoB5kC4iF7viSCaw](https://user-images.githubusercontent.com/101708836/214133179-a2cbbaec-1726-46f8-8cab-833da98020dc.jpg)

The tool is highly flexible and can be used on a wide range of images, including photographs, digital art, and even screenshots. It can also be used to extract colors from video frames and live video feeds. Additionally, the tool includes a variety of options for customizing the color extraction process, such as setting the number of colors to extract, the color space to use, and the color distance metric.

![1_sgg3kv_vAH3WDnqA-npA5g](https://user-images.githubusercontent.com/101708836/214133183-18794bba-d6d0-4bc1-8538-f368dea64112.jpg)

![1_wIChselOePFsGDB5OP2AbA](https://user-images.githubusercontent.com/101708836/214133184-01951046-4bca-440a-8dad-9c27f061d329.jpg)


import cv2
import numpy as np
import pandas as pd

# Reading the image with opencv
img = cv2.imread("img.png")

# Declaring global variables (are used later on)
clicked = False
r = g = b = xpos = ypos = 0

# Reading csv file with pandas and giving names to each column
index=["color","color_name","hex","R","G","B"]
csv = pd.read_csv('colors.csv', names=index, header=None)

# Function to calculate minimum distance from all colors and get the most matching color
def getColorName(R,G,B):
    minimum = 10000
    for i in range(len(csv)):
        d = abs(R- int(csv.loc[i,"R"])) + abs(G- int(csv.loc[i,"G"]))+ abs(int(csv.loc[i,"B"]))
        if(d<=minimum):
            minimum = d
            cname = csv.loc[i,"color_name"]
    return cname

# Function to get x,y coordinates of mouse double click
def draw_function(event, x,y,flags,param):
    global b,g,r,xpos,ypos, clicked
    if event == cv2.EVENT_LBUTTONDBLCLK:
        clicked = True
        xpos = x
        ypos = y
        b,g,r = img[y,x]
        b = int(b)
        g = int(g)
        r = int(r)

# Set mouse callback for the image window
cv2.namedWindow('image')
cv2.setMouseCallback('image', draw_function)

# Main loop that waits for a key press event
while(1):
    cv2.imshow("image", img)
    if (clicked):
        # Draw a rectangle on the image
        cv2.rectangle(img,(20,20), (750,60), (b,g,r), -1)
        # Create text string to display (Color name and RGB values)
        text = getColorName(r,g,b) + ' R='+ str(r) + ' G='+ str(g) + ' B='+str(b)
        # Put the text string on the image
        cv2.putText(img, text,(50,50),2,0.8,(255,255,255),2,cv2.LINE_AA)
        # If the sum of the RGB values is greater than or equal to 600, change the text color to black
        if(r+g+b>=600):
            cv2.putText(img, text, (50, 50), 2, 0.8, (0, 0, 0), 2, cv2.LINE_AA)
    # If the user presses the 'Esc' key, exit the main loop
    if cv2.waitKey(20) & 0xFF ==27:
        break

# Destroy all windows when the main loop finishes
cv2.destroyAllWindows()

This tool is perfect for graphic designers, web developers, photographers, and anyone else who needs to extract colors from images for their work. With its powerful image processing capabilities and flexible customization options, it's an indispensable tool for anyone working with images and colors.
