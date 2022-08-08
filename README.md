# Image-compression
In this project, I used the van Gogh colors dataset that contains 986 different colors and clustered these colors into 16 colors using kmean algorithm.
##### All images
![all_images](https://user-images.githubusercontent.com/83555471/183481065-638827a7-bf00-40a9-a927-a635d37f1359.PNG)
### How does this compression work.
As known each pixel is represented using an RGB tuple (Red, Green, Blue) each color from the tuple is represented using an integer value from 0 to 255 which means that it can be represented using 8 bits so each color is represented using 24 bits.
The new representation requires overhead storage in the form of a dictionary of 16 colors, each of which requires 24 bits, but the image only requires 4 bits per pixel location.
#### The Centroids images
![centroids_imges](https://user-images.githubusercontent.com/83555471/183481206-83f99ce4-d3d7-49a9-bf73-c87e842de34e.PNG)
#### Example 
![Capture](https://user-images.githubusercontent.com/83555471/183440769-4a848167-dae3-4e03-b353-5ff2a2c765f1.PNG)
The upper four images are the original ones and the lower four are the compressed ones that are represented using 16 colors only.
Each image consists of 500 X 500 pixels.For the old representation each image needs 500 X 500 X 24 = 6000000 bits to be represented however for the new representation each image needs only 500 X 500 X 4 + 16 X 24= 1000384 bits only containing 384 bits for the overhead.
#### Dataset
Dateset :https://www.kaggle.com/datasets/pointblanc/colors-of-van-gogh?select=color_space.csv
