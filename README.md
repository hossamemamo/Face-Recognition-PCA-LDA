# Face-Recognition-PCA-LDA

**Face Recognition** 

**using PCA & LDA**

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.001.jpeg)

Contributors: 

Adham Amr Mohamed (6713) Hossam Eldin Ahmed Shaaban (6721) Saeed Elsayed Abokhatwa (6829)

1. Download the dataset & understand the format: 

The AT&T Database Of Faces, (formerly 'The ORL Database of Faces'), contains a set of face images taken between April 1992 and April 1994. 

There are ten different images of each of 40 distinct subjects. 

For some subjects, the images were taken at different times, varying the lighting, facial expressions (open / closed eyes, smiling / not smiling) and facial details (glasses / no glasses). All the images were taken against a dark homogeneous background with the subjects in an upright, frontal position (with tolerance for some side movement). 

The files are in PGM format. The size of each image is 92x112 pixels, with 256 grey levels per pixel. The images are organised in 40 directories (one for each subject), which have names of the form sX, where X indicates the subject number (between 1 and 40). In each of these directories, there are ten different images of that subject, which have names of the form Y.pgm, where Y is the image number for that subject (between 1 and 10). 

**The dataset was imported as input data to our Kaggle notebook to perform the required face recognition tasks.** 

2. Generate the data matrix & the label vector: 
1. To be able to operate on the data, the loadImages function was implemented. loadImages() converts every image in the dataset into a row vector of 10304 values corresponding to the image size, stacks the 400 row vectors into a single data matrix and returns the data matrix as a numpy array. 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.002.png)

2. To generate the label vector y, the createLabelVector() fn. was implemented. y consists of 400 rows: each row is a numpy array containing one integer value from 1:40; each value representing the subject id of an image. **This way, the integer in the *i*th row in y is the label of the *i*th row vector in the data matrix X.** 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.003.png)

3. Split the dataset into training & test sets: ![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.004.png)

The Train\_Test\_Split() function iterates over the data matrix X and the label vector y, assigns their even rows to training sets, their odd rows to test sets and returns the resulting matrices. This way, 50% of the data is used for training, and the other 50% is kept for testing. 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.005.png)

4. Classification Using PCA : 

Points (a) through (e) : 



|Alpha |Number of eig vectors to consider |Accuracy (FNN) |
| - | :- | - |
|0\.8 |36 |0\.97 |
|0\.85 |52 |0\.975 |
|0\.9 |76 |0\.97 |
|0\.95 |116 |0\.965 |
Table 4.1 

e) when alpha increases , the number of eig vector to consider will also increase however … considering more eig vectors will cause our model to overfit on the training data .. which makes the accuracy drop in the testing data so it’s wise to consider accuracy measure on testing data as a indicator of which value of alpha to consider. 

4-2 demonstration of the data:   eigen vectors to consider are on axis of the arrow 

10304 500 50 ![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.006.jpeg)

in figure 4-2.1 the less eigen vectors we consider the less visual the face becomes 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.007.png)

*Figure 4-2.1* 

In figure 4-2.2 we can see the eigen faces where they contain the most important features of a face and we use them for PCA. 

the projection and reconstruction of images is done by the code below : 

`    `def project(self,X): 

`        `return X @ self.Ur 

`            `#(200,10304) x (10304, r) = (200,r) 

`    `def reconstruct\_images(self,x\_reduced): 

`        `return x\_reduced @ self.Ur.T 

`            `#(200,r) x (r, 10304) = (200,10304) to be able to view it 

5. Classification using LDA: 

5-a. Linear Discriminant Analysis (LDA) is a popular supervised learning technique used in statistics, pattern recognition, and machine learning. It is used to classify data into distinct categories based on a set of features. LDA seeks to find a linear combination of features that maximizes the separation between different categories while minimizing the variation within each category.  

LDA is often used as a dimensionality reduction technique as well. By projecting the data onto a lower-dimensional subspace while maintaining the separability of the categories, LDA can help reduce the complexity of a classification problem and improve classification accuracy. 

LDA assumes that the data follows a normal distribution and that the covariance matrix of the features is equal across all categories. LDA has been successfully applied to a wide range of applications, including face recognition, text classification, and bioinformatics.

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.008.jpeg)

Figure 5.1 LDA pseudocode 

We will replace B matrix in Figure 5.1 line 3 by Sb given by the following formula: 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.009.png)

Where n is the number of samples in each class, μk is the mean of class K and μ is the overall mean. 

2. After computing W, we will use the dominant 39 eigenvectors. 
2. After selecting the dominant 39 vector we will project our data on the 39 vectors to     reduce our data from 200x10304 to 200x39. 
2. Using a simple classifier like KNN we will get a 95.5% Accuracy using 1KNN.

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.010.png)

5. Comparing the results to PCA using 1st NN:

`     `As shown in table 4.1, using different alphas yields a higher accuracy than LDA.

6-Classifier Tuning: 

6-1 PCA: 



|Alpha |Graph |
| - | - |
|<p>0\.8 </p><p>[0.97, 0.905, 0.825, 0.78] </p>|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.011.png)|
|<p>0\.85 </p><p>[0.975, 0.91, 0.84, 0.765] </p>|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.012.png)|
|<p>0\.9 </p><p>[0.97, 0.905, 0.83, 0.775] </p>|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.013.png)|
|<p>0\.95 </p><p>[0.965, 0.9, 0.835, 0.79] </p>|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.014.png)|
If it is found that two neighbors, neighbor k+1 and k, have identical distances but different labels, the results will depend on the ordering of the training data. 

In the case of ties, the answer will be the class that happens to appear first in the set of neighbors. 

6-2. LDA: 

In this part we Set the number of neighbors in the K-NN classifier to 1,3,5,7. Sklearn the K-NN classifier was used to classify and handle the tie breaking. Figure 6.1 shows that accuracy drops while increasing K value. 

.  ![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.015.jpeg)

Figure 6.1: graph between the number of neighbors on x-axis and accuracy on y-axis.

As shown in figure 6.1, The accuracy drops while increasing the value of K. 

7. Compare faces vs. non-faces images: 

7-1. PCA:

In addition to the 400 faces images, we added another 400 images consisting of shoes, furniture, pepsi and cocacola cans as non-faces images. Our new data matrix has 800 row vectors, each row consists of 10304 values corresponding to the image size. A new label vector was created, with 1 as the label for faces, and 0 as the label for non-faces. Following are the results for PCA using the 50% split: 

Failure & success cases: 

200 faces vs 200 non-faces: 



|<p>Alpha = 0.8 </p><p># eigenvectors to consider = 18 </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.016.png)</p>|<p>Alpha = 0.9 </p><p># eigenvectors to consider = 32</p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.017.jpeg)</p>|
| - | - |
|<p>Alpha = 0.85 </p><p># eigenvectors to consider = 58 </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.018.png)</p>|<p>Alpha = 0.95 </p><p># eigenvectors to consider = 120 </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.019.jpeg)</p>|

200 faces vs 100 non-faces: 
|<p>Alpha = 0.8 </p><p>14 eigenvectors to consider = </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.020.jpeg)</p>|<p>Alpha = 0.9 </p><p>45 eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.021.jpeg)</p>|
| - | - |
|<p>Alpha = 0.85 </p><p>25 eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.022.png)</p>|<p>Alpha = 0.95 </p><p>93 eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.023.jpeg)</p>|


200 faces vs 150 non-faces: 



|<p>Alpha = 0.8 </p><p># eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.024.jpeg)</p>|<p>Alpha = 0.9 </p><p># eigenvectors to consider = </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.025.jpeg)</p>|
| - | - |
|<p>Alpha = 0.85 </p><p># eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.026.jpeg)</p>|<p>Alpha = 0.95 </p><p># eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.027.jpeg)</p>|

200 faces vs 340 non-faces: 
|<p>Alpha = 0.8 </p><p># eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.028.jpeg)</p>|<p>Alpha = 0.9 </p><p># eigenvectors to consider = </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.029.png)</p>|
| - | - |
|<p>Alpha = 0.85 </p><p># eigenvectors to consider = </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.030.jpeg)</p>|<p>Alpha = 0.95 </p><p># eigenvectors to consider =  </p><p>![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.031.png)</p>|


Accuracy measures: best results are obtained when the data is balanced (200 F vs 200 NF) 



|PCA alpha = 0.8 |![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.032.jpeg)|
| - | - |
|PCA alpha = 0.85 |![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.033.jpeg)|
|PCA alpha = 0.9 |![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.034.jpeg)|



|<p>PCA alpha = 0.95 Best results is when the data is balanced (200 faces vs 200 non- faces) </p><p>Increasing or decreasing CNN </p>||![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.035.jpeg)||
| :- | :- | - | :- |
7-2. LDA: 

Failure & success cases: 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.036.jpeg) ![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.037.jpeg)

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.038.png)

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.039.jpeg)![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.040.jpeg)

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.041.jpeg)![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.042.png)

` `![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.043.png)![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.044.png)

` `![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.045.png)![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.046.png)



|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.047.jpeg)|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.048.jpeg)|
| - | - |
|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.049.png)|![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.050.png)|
![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.051.png)

Figure 7.1 

As shown in Figure 7.1, The accuracy drops while increasing the number of none faces in the training set. 

We chose 1 dominant vector because number of components (eigenvectors) must be less than or equal to min(numberofclass-1,numberOfFeatures). 

8. Bonus: 
1. Different split (70% training – 30% testing): PCA: 



|alpha |# Eigenvectors to consider |Accuracy (FNN) |
| - | - | - |
|0\.8 |40 |0\.983 |
|0\.85 |60 |0\.983 |
|0\.9 |92 |0\.983 |
|0\.95 |148 |0\.975 |
`      `Classifier tuning: As the k value increases, the accuracy decreases. 

LDA: ![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.052.jpeg)

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.053.jpeg)

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.054.png)

As shown in the figure above, The accuracy of the new split is much higher than the old split. 

2. PCA different algorithm: 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.055.jpeg)

*Figure 1online example of what Kernel PCA does*

We recall that PCA transforms the data linearly. Intuitively, it means that the coordinate system will be centered, rescaled on each component with respected to its variance and finally be rotated. The obtained data from this transformation is isotropic and can now be projected on 

its *principal components*. 

Thus, looking at the projection made using PCA (i.e. the middle figure), we see that there is no change regarding the scaling; indeed the data being two concentric circles centered in zero, the original data is already isotropic. However, we can see that the data have been rotated. As a conclusion, we see that such a projection would not help if define a linear classifier to distinguish samples from both classes. 

Using a kernel allows to make a non-linear projection. Here, by using an RBF kernel, we expect that the projection will unfold the dataset while keeping approximately preserving the relative distances of pairs of data points that are close to one another in the original space. 

We observe such behaviour in the figure on the right: the samples of a given class are closer to each other than the samples from the opposite class, untangling both sample sets. Now, we can use a linear classifier to separate the samples from the two classes. 



|Number of eig vectors to consider |PCA |KernelPCA d=2 polynomail |
| :- | - | :- |
|36 |0\.97 |0\.955  |
|52 |0\.975 |0\.96  |
|76 |0\.97 |0\.96  |
|116 |0\.965 |0\.95  |
Kernel PCA was developed in an effort to help with the classification of data whose decision boundaries are described by non-linear function. 

Since data is separated better by a linear function , PCA performs better 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.056.jpeg)

*KernelPCA tunning number of components to consider* 

![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.057.png)

LDA different algorithm: 

Using LDA algorithm from Sklearn which by fiting a Gaussian density to each class. It was so much fast the our naïve approach 



||**Naïve**  |**Sklearn** |
| :- | - | - |
|Time |5\.1 mins |0\.7 seconds |
|Accuracy |95\.5% |96\.5% |
![](Aspose.Words.25631b85-bff2-4b4c-b40a-ecc4cadad80c.058.png)
