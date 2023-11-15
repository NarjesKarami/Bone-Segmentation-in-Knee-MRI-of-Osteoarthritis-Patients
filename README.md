# Bone Segmentation in Knee MRI of Osteoarthritis Patients
Osteoarthritis (OA) is a chronic and debilitating joint disease characterized by degenerative changes to the bones, cartilage and other tissues. One of the effective methods to assess disease progression is monitoring of bone and cartilage changes such as shape, volume or thickness. However based on image processing of knee MRI, the disease progression can be diagnosed and programmed before knee surgery. This procedure is performed by bone and cartilage segmentation of knee MRI. The main challenge in this project, is the intrinsic ambiguity of knee MRI in showing the edges and features of the bone and cartilage precisely. In this project, several methods were proposed for automatic segmentation of bones in knee MRI’s. The segmentation methods were based on active shape models and active contours. Initialization of the contour was obtained from image registration techniques. Cartilage segmentation was done by graph-cut method. In the graph-cut method, a graph is made from the image and the problem is converted from the domain of the image to the domain of the graph. The cut in the graph is equivalent to the segmentation in the image. The methods were tested on 3 slices datasets from SKI10 database and collected dataset from a hospital and the results have been compared with medical diagnosis done by experts. We collaborated with radiologists during this project.
In the sections that follow, we will provide detailed descriptions of the methods employed for the segmentation of bones in knee MRI images.

### Bone segmentation
For the segmentation of bones in knee MRI, prior information such as the location and shape of the bones was used. As mentioned earlier, in some areas of the MRI of the knee, the edge between the bones and other tissues is not clear. The use of prior information improves the quality of segmentation, especially when image information is lost. The image registration techniques were used to obtain the initial shape.
### Initial shape of the bones with the image registration technique
The goal of image registration is to find the geometric transformation between two images so that the points of one image are best aligned with their corresponding points in the second image. The first image is called the fixed or target image and the second image is called the floating or source image. In the image registration process, the floating image aligns the fixed image as much as possible.
For the segmentation process, the specialist has created a new image by assigning a label to each floating image object. We call this image an atlas. In order to make the atlas, the femur, tibia, and other tissues have been assigned labels 1, 2, and 0, respectively.
Then the fixed image was adapted to the floating image and the geometric transformation function was obtained. By applying this transformation function to the atlas image, the objects in the fixed image were segmented. We modeled the global displacement (deformation) of the tissue by an affine transformation, while the local transformation was described by a non-rigid deformation based on B-splines transformation function. In the Fig.1, the process of obtaining the initial shape of the bones for a test data were shown.
![image](https://github.com/NarjesKarami/Bone-Segmentation-in-Knee-MRI-of-Osteoarthritis-Patients/assets/78353927/730e0dd0-f412-4941-8861-30400ab7de45)\
Fig.1. The process of obtaining the initial shape of the bones

### Proposed method 1
In the proposed method, the active shape models (ASM) has been improved. In the normal ASM method, the initial shape is obtained by averaging the shapes of the training images. Here, image registration methods were used to obtain the initial shape.
In this method, the goal is to introduce a model that changes its shape in such a way that it does not lose the characteristics of the object being searched for. During an iterative process, the initial shape is changed in such a way to reach the best location of the image object and the shape model is also preserved. The ASM model is based on the principal component analysis of the corresponding landmark points on the object contour in the training images.
The ASM models consists of two stages of training and segmentation. In the training phase, a number of images were selected. The first step in this step was to describe the contour of the object in each image of the training group with sets of points. Each point in the training set must had a corresponding point in other images. To create a correspondence between the points, the automatic method of minimizing the length of the description has been used. Then the images of the training group matched each other. After adapting the images of the training group, the statistical properties and gray profile of the images were obtained. This obtained statistical information was used in the segmentation stage to maintain the shape model. In the phase of segmentation, an approximation of the initial shape was tested on the new image and the original shape was searched for. For each test image, a local search step was performed around each landmark of the original shape. In this case, the best point of occurrence of the gray level profile was obtained. The closer the initial shape is to the desired shape, the better the segmentation result. If the initial shape is far from the target shape, the algorithm will not converge to the correct result. Therefore, choosing the appropriate initialization is important.

## Proposed method 2
In the second proposed method, bones were segmented using the local region-based active contour. The initial contour was obtained from the image registration technique as described earlier. In this active contour, a local region was considered at each point along the contour, and each point moved in such a way that the calculated energy was minimized in its local region. In this algorithm, the boundaries of the objects in the image were determined based on the information of the local disk area. In the vicinity of each pixel along the contour, a local region with a certain radius was selected. In the local active contour algorithm, some parameters such as the initial contour and the radius of the local disc had to be carefully selected. Choosing the right radius has an effective role in the results of this algorithm. Choosing the best radius is a challenging task. If the local radius is chosen too small or too large, the algorithm will not converge to the correct region. In the proposed method, a method with multi resolution accuracy was provided that eliminates the sensitivity of local radius selection and uses local region information at multiple scales.

### Results and validation
In this project, images from the SKI10 database were used. These images were obtained from different centers and using different devices. In this section, the performance of the algorithms was compared with the method of normal ASM. The accuracy of the proposed method had been compared with manual segmentation by an expert. The sensitivity and specificity index and Dice similarity coefficient were used to evaluate the results. The validation of bone segmentation was presented using three measures:\
Sensitivity= TP/(TP+FN)       (1)\
Specificity=TN/(TN+FP)       (2)\
DSC=2TP/(2TP+FP+FN)      (3)\
Where TP is a true positive and represents the number of pixels that have been correctly identified as bones. TN is a true negative and represents the number of pixels that are not correctly identified as bones. FP is a false positive and represents the number of pixels that are incorrectly identified as bones. FN is a false negative and indicates the number of pixels that are incorrectly identified as background.
The average accuracy for femur and tibia bones is shown in Fig2 to Fig4. The comparison of the obtained results shows that the proposed methods have better performance. Also, the second proposed method leads to a more accurate segmentation compared to the results of the first proposed method.

![image](https://github.com/NarjesKarami/Bone-Segmentation-in-Knee-MRI-of-Osteoarthritis-Patients/assets/78353927/b4d515fc-987a-4d77-a2d9-ee7ebad9ece1)\
Fig.2. The comparison of average of Dice similarity coefficient in femural and tibial bones

![image](https://github.com/NarjesKarami/Bone-Segmentation-in-Knee-MRI-of-Osteoarthritis-Patients/assets/78353927/637ede29-d9b3-4c6b-a0ec-6e5942d34574)\
Fig.3. The comparison of average of sensitivity in femoral and tibial bones.

![image](https://github.com/NarjesKarami/Bone-Segmentation-in-Knee-MRI-of-Osteoarthritis-Patients/assets/78353927/69d8217e-ce03-4d64-b5c6-b6972a0a9666)\
Fig.4. The comparison of average of specificity in femoral and tibial bones.

In addition, to evaluate the accuracy of the methods, the distance between the pixels on the border of the image segmented by the algorithm and the image segmented by the specialist have been compared. For this purpose, two criteria of average boundary distance (AvgD) and root mean square of boundary distance (RMSD) were considered. Fig.5 and Fig.6 show the average of the two parameters AvgD and RMSD for femur and tibia, respectively. According to the diagrams, it is clear that in the two proposed methods, the distance between the boundaries and the actual boundary determined by the specialist is less than the normal ASM method, and these two methods have appropriate accuracy.

![image](https://github.com/NarjesKarami/Bone-Segmentation-in-Knee-MRI-of-Osteoarthritis-Patients/assets/78353927/b806b663-b444-4418-be73-ea6ebecf39fb)\
Fig.5. The comparison of average of AvgD in femur and tibia

![image](https://github.com/NarjesKarami/Bone-Segmentation-in-Knee-MRI-of-Osteoarthritis-Patients/assets/78353927/6701c424-2fdd-4a43-b771-94396f41d75c)\
Fig.6. The comparison of average of RMSD in femur and tibia






