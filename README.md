# Glove-defect-detection
The algorithm aims to detect defects in gloves, including dirt, holes, nail cuts, stains, palm tears, rips, and missing finger parts. It involves processes like pre-processing, image segmentation, feature extraction, and defect detection. 

CHAPTER 2.1	PRE-PROCESSING AND TRANSFORMATION
CHAPTER 2.1.1	im2gray() FUNCTION
The function called im2gray() will be used to remove colour information by converting colour image into grayscale image- removing hue and saturation (The MathWorks Inc, 2022). The purpose of preserving only luminance is to ensure that subsequent image processing steps can be simplified or easy to implement such as image segmentation to separate the defective objects and gloves from the background (binarization) and morphological operation as the dimensionality of the image has been reduced from 3 channels into 1 channel.  
 
CHAPTER 2.1.2	im2double() FUNCTION
The grayscale image will be converted into a double format in the range of 0 to 1 with the help of the function im2double() (The MathWorks Inc, 2022). The purpose of converting to a double in the range of 0 to 1 is to ensure that the image is compatible with MATLAB functions because may require pixel values in the range of 0 to 1 of the data type double and want to achieve consistency across all the image operations.
 
CHAPTER 2.1.3	medfilt2() FUNCTION
To remove noise (salt and pepper) present in the grayscale image and enhance its quality while at the same time prevent remove edge intensively, medfilt2() is applied to perform the median calculation, converting the output pixel value to the median value (The MathWorks Inc, 2022).
 
CHAPTER 2.1.4	bwareaopen() FUNCTION
	To remove any unwanted tiny objects (white pixels) or noise present in the binary image, bwareaopen(BW,P)  function will be used to remove any  tiny object that is smaller or lesser than P pixels (The MathWorks Inc, 2022)

CHAPTER 2.1.5	imadjust() FUNCTION
	imadjust(img,[0,0.4], [0,1]) function will be used to increase the contrast for the defects (nail cuts) that easy to do segmentation or separate interested objects from the background by mapping the intensity value  between 0 and 0.4 (nail defects pixel value located) to the new values  between range of  0 and 1 (The MathWorks Inc, 2022)

CHAPTER 2.1.6	rgb2hsv() FUNCTION
rgb2hsv() function creates a full-color image whose pixel values are represented in the HSV color system (hue - color tone, saturation - saturation, value - brightness), from the original full-color image in the RGB color system (The MathWorks Inc, 2022).

CHAPTER 2.2	IMAGE SEGMENTATION
In this next process, the team separated the region of interests from the background such as glove and defects.

CHAPTER 2.2.1	graythresh() FUNCTION
To find the suitable binarized threshold that convert interested objects to 1 (white) and non-interested objects to 0 (black), The graythresh() function will be used to help find the best global threshold (minimized intraclass) that can be used as a threshold to best separate white and black to convert grayscale to a binary image (The MathWorks Inc, 2022).
 
CHAPTER 2.2.2	strel() and imdilate(), imclose(), imopen() FUNCTIONS
After perform segmentation, it is necessary to perform morphological operation for the purpose of object connection and noise cancellation. The team used strel() to define the structural element consisting of a user-defined shape and size of the kernel that used for morphological operations such as dilation, erosion, opening, and closing. The team used “strel("disk",2)” which belongs to disk shape structural element with the size of 2 neighbourhood. The team applied “strel('square',20)” which belongs to square shape structural element with a radius of 20. Moreover, the team used imdilate() function to expand the white pixels, which means that it enlarges white area to make region become larger, in which case the area of the object of interest (hole) is expanded to make the area larger (The MathWorks Inc, 2022). Apart from that, the team used imclose() function for the dirty image that will perform dilation first to fill the gap to make all the nearby defect regions connected and then erosion to erode the region to improve segmentation. Lastly, the team used imopen() function to erode the small noise present in the binary images while keeping large objects by performing expansion (The MathWorks Inc, 2022).
 
CHAPTER 2.2.3	bwconncomp() FUNCTION
After performing morphological operation that remove noise and make the region of interests (defects) more obvious, it is necessary to perform bwconncomp() function to identify the connected components present in the binary image, in this case it will not only identify but also provide unique id, pixel indices and count number of connected components (defects) (The MathWorks Inc, 2022). The reason doing this is to isolate region of interested from the background by understanding the number of defects, indices, and later passed the connected components to the next function called regionprops() to extract specific features.
 
CHAPTER 2.2.4	imfill() FUNCTION
Imfill() function filled in areas of the image. If the holes are specified, this function will help to fill in holes in the mask.
 

CHAPTER 2.3 	FEATURE EXTRACTION AND DEFECT DETECTION
CHAPTER 2.3.1	regionprops() FUNCTION
After perform segmentation that isolate the glove’s defects region using connected component analysis, regionprops() function will be used to extract the relevant features or properties of connected components that identified from bwconncomp() function (The MathWorks Inc, 2022). By doing this, the system is able to differentiate type of defects present in the surface of glove through properties extracted by the regionprops() function such as area, eccentricity, circularity, perimeter and so on. For example, when detecting hole defects, the team applied eccentricity and circularity to distinguish holes. Furthermore, by using bounding box properties extracted from region prop function, the system able to highlight the defects present on the gloves. 
 

CHAPTER 2.3.2	normxcorr2() FUNCTION
The normxcorr2() function was one of the methods to detect glove defects. The function works by performing normalized cross-correlation and  comparing similarity between the target image (search matrix) and the template image (template matrix) (The MathWorks Inc, 2022). The connected components that were extracted by bwconncomp() and regionprops() functions will be passed to the normxcorr2(template, A) parameter to return correlation matrix that consist of correlation coefficients. In other words, the higher the value, the more similarity between template and the target object. The team used the maximum value of all relevant coefficients return by “[max_similarity, position] = max(abs(matching(:)));” to check the similarity between template and each connected component. If the threshold is greater than a certain maximum similarity value, it will finally classify and detect the specific defect as shown below.
