

###Introduction
Julia is a high-level dynamic programming language for technical computing, with syntax that is familiar to users of other technical computing environments (such as matlab for example).
***
###List of Julia packages.
1. TestImages package
2. ImageView package
1. 
⋅⋅1. Ordered sub-list
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅
⋅⋅⋅(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

s 



***
###Image packages in Julia 
Julia has a few packages dedicated to image processing. First of all we will look at the **TestImages package**, which hosts a selection of sample images, then briefly visit the **ImageView** package before moving onto the Images package, which implements a range of functions for image manipulation.
***
###Testimages package
The TestImages package currently provides 25 sample images, this forms a basis for experimentation.
* **Step 1** We will load the archetypal test image.
* **Step 2** **ImageView package** allows us to take a look at the image we have loaded.  
* **Step 3** You can optionally specify the pixel spacing as a parameter to *view()*, which then ensures that the aspect ratio of the image is conserved on resizing. There are many tricks associated with *view()*: you can click-and-drag within the image to zoom in on a particular region;various simple transformations (flipping and rotation) are possible; images can be annotated and multiple images can be arranged on a canvas for simultaneous viewing.



###Image representation
Outside of the test images, an arbitrary image file can be loaded using **imread()** from the Images package. Naturally, there are also functions for writing images, **imwrite()** and **writemime()**.Training and test matrices can now be loaded using function **read_data()**.Information about the labels can be read using the **readtable()** function.

###Imread command and colour matrices
**imread()** allows us to read the image. **float32sc()** changes the image into real values.The default representation for the Image object tells us its dimensions, storage type and colour space. The result could be a single matrix if the image is black/white, or a triple array that contains three matrices representing each color **(Red, Green, Blue)**. 
It is easier to work with images with the same representation, so we convert all of the color images to grayscale by averaging the values across the three color matrices.The result is a single matrix per image. Changing each image matrix into a vector allows us to save all results in a single matrix that contains the data for all images. Using this method the result is a single matrix per image. 


***

<pre><code>

img = imread(nameFile)
temp = float32sc(img)
if ndims(temp) == 3
 temp = mean(temp.data, 1)
end
</code></pre>
 ***
The result is a single matrix per image. Changing each image matrix into a vector allows us to save all results in a single matrix that contains the data for all images.
<pre><code>
x[i, :] = reshape(temp, 1, imageSize)
</code></pre>

***
The labels are characters, but the algorithms recognize numbers,each character is converted into an integer. The data is loaded into a string type by default. We take the each element of the string and convert it to an integer number.
***

<pre><code>
Get only first character of string (convert from string to character).
Apply the function to each element of the column "Class"
yTrain = map(x -> x[1], labelsInfoTrain["Class"])

Convert from character to integer
yTrain = int(yTrain)
</pre></code>
***

###Training model
Since we now have both the images data and labels represented vectors of real numbers, we are ready to apply a machine learning algorithm. The algorithm should learn the patterns in the images that identify the character in the label.


Here we will use the Julia version of the popular Random Forest algorithm. This algorithm can usually achieve high performance without the need of tuning many parameters (more information about the algorithm can be found at Random Forest). The model requires that we set three parameters: the ***number of features*** to choose at each split, the ***number of trees*** , and the  ***ratio of subsampling*** . The number of features to try at each split is usually chosen to be 

***sqrt(number of features)***

The numbers of trees is chosen arbitrarily.Larger is better, but it takes more time to train.The ratio of subsampling is usually chosen to be 1.0. However, you may change any of these numbers and chose the ones that produce the highest performance.

Let's now train the model:
<pre> <code>

Pkg.add("DecisionTree")
using DecisionTree

Train random forest with
20 for number of features chosen at each random split,
50 for number of trees,
and 1.0 for ratio of subsampling.
model = build_forest(yTrain, xTrain, 20, 50, 1.0)




</pre></code>
***
###Julia V's Matlab
Red, Green and Blue.

| Colour        | Size of matrix| Actual size   |
| ------------- |:-------------:| -------------:|
| Red           | mxn           | 500x700       |
| Blue          | mxn           | 500x700       |
| Green         | mxn           | 500x700       |
