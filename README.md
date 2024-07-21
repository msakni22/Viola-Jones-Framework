# Viola-Jones-Framework
<p>The <b>Viola-Jones Object Detection Framework</b>, developed by <b>Paul Viola</b> and <b>Michael Jones</b> in <a href="#ref1">2001</a>, is an innovative machine learning algorithm specifically designed for fast, accurate face detection. It was primarily motivated by the problem of face detection, although it can be adapted to detect other classes of objects. <br> Training the framework is relatively slow, but it enables objects to be detected quickly and accurately. In fact, it can detect human faces very effectively, and in real time.<br></p>

Viola–Jones is essentially a <b>boosted feature learning algorithm</b> trained by running a modified <b>AdaBoost</b> algorithm on <b>Haar feature classifiers</b> to find a sequence of classifiers <i>f<sub>1</sub>, f<sub>2</sub>, ..., f<sub>n</sub></i>.<br>

The framework consists of four key components that work together to achieve efficient and accurate object detection:
<ol>
  <li>Haar-like features</li>
  <li>Integral image</li>
  <li>AdaBoost Learning Algorithm</li>
  <li>Cascade Classifier</li>
</ol>
<h3>1. Haar-like features</h3>
Most images contain universally similar patterns recognizable from a human perspective.<br>
For example:
<ul>
  <li><b>Human Faces</b>: Common patterns include eyes, nose, cheeks, and mouth.</li>
  <li><b>Four-Wheeler Vehicles</b>: Recognizable patterns include wheels, doors, and steering wheels.</li>
  <li><b>Buildings</b>: Consistent features include doors, windows, and walls.</li>
</ul>
The concept of <b>Haar-like features</b> was introduced by <b>Alfred Haar</b> in 1909. He developed the "Haar wavelet," a matrix of rescaled square-shaped functions with values ranging between 0 and 1.<br>
<b>Haar-like features</b>, named for their resemblance to 2D Haar wavelets, use simple rectangular patterns to detect structural components of objects, such as edges, lines, and textures. They are applied to various sub-windows of the image to assess the presence of specific patterns crucial for object recognition.<br>

There are several types of Haar-like features (see figure), including:
<ul>
  <li>Edge features</li>
  <li>Line features</li>
  <li>Four rectangle features</li>
  <li>Center features</li>
</ul>
<img src="images/haar.png" height=300px />
<img src="images/haar.jfif" height=300px />

For example, human faces share several common attributes, such as the eye region being darker than the bridge of the nose, and cheeks being brighter than the eye region. Similarly, every part of the face can be represented as haar like a feature considering their generic patterns.<br>

<img src="images/faces.jpg" height=350px />

A haar-like feature will be represented as a matrix where all white-colored pixels will be represented as 0 and black-colored pixels will be represented as 1.
By summing the pixel values of different regions and comparing them, the algorithm can effectively identify areas where these attributes hold true.<br>

<img src="images/rectangle.webp" height=200px />

For instance, the sum of pixel values in a darker region will be smaller than in a lighter region, which can be used to detect specific facial features.<br>

For example, We can see in the image below that there is an edge formation near the nose to cheek part. The intensity becomes larger when it comes from left to right.<br>

<img src="images/apply_haar.jfif" height=300px />

After detecting an object using Haar-like features, it is crucial to evaluate the model to assess its accuracy. This involves subtracting the sum of pixel values in the black region from the sum of pixel values in the white region. If the feature value is close to the expected value (near 1), it indicates a strong presence of the property being detected. For a uniform surface(e.g., a wall) this value will be close to zero and won't provide any significant information.<br>

$Feature\textunderscore Value = (\sum Pixel\textunderscore White\textunderscore Region - \sum Pixel\textunderscore Black\textunderscore Region) / Number\textunderscore of\textunderscore Pixel$

In our example, the sum of pixel values in the black region and the white region:<br>
$B = 0.6+0.8+0.8+0.6+0.6+0.8+0.8+0.9=5.9$<br>
$W = 0.1+0.2+0.2+0.3+0.2+0.1+0.2+0.1=1.4$<br>
$Metric = (B - W)/8 = (5.9-1.4)/8 = 0.56$<br>
Therefore, we can infer that there is a 56% probability of detecting the Haar-like feature in the specific area of the image.

Each haar feature type is applied to different locations and sizes within all images. By sliding these rectangular windows over various positions and scales of the image, the algorithm computes feature values based on the intensity differences between the regions. 

This process is repeated across multiple locations and sizes, allowing the detection of features with varying scales and orientations, which helps in recognizing patterns and structures crucial for object detection.

<img src="images/example.png" height=600px />

The Viola-Jones algorithm calculates many such features across multiple subregions of an image, making the process computationally intensive. To address this, the algorithm employs the concept of <b>Integral Images</b>, which allows for rapid calculation of these features.

<h3>2. Integral image</h3>
Let's assume an image size of 24 × 24 pixels (as used in the <a href="#ref2">paper</a>) a horizontal two-rectangle feature can have the following 12 valid sizes given a pixel wide window: 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22 and 24. Since the feature is of height 1, there are 24 possible combinations for the height without violating symmetry. 
However, given that we also have to position the feature, not all positions are valid:
<ul>
  <li>A 2 pixel feature can be positioned at $x$ coordinates $0...22$ , resulting in 23 possible combinations.</li>
  <li>A 24 pixel feature can be only positioned at $x$ coordinate $0$, resulting in 1 possible combination.</li>
  <li>A 22 pixel feature can be positioned at $x$ coordinates $0, 1, 2$, resulting in 3 possible combinations.</li>
</ul>
This yields<br>

$n_{x}(width_{feature}) = width_{window} - width_{feature} + 1$ <br>

possible combinations for a specific $width_{feature}$, giving<br>
$N_{x,2h} = n_{x}(2) + n_{x}(4) + n_{x}(6) + ... + n_{x}(24)$<br>
$=23 + 21 + 19 ... + 1$<br>
$= 144$<br>
possible combinations for the $x$ position alone. The same reasoning can be applied to the height, which evaluates as<br>
$n_{x}(height_{feature}) = height_{window} - height_{feature} + 1$<br>
and<br>
$N_{y,2h} = n_{y}(1) + n_{y}(2) + n_{y}(3) + ... + n_{y}(24)$<br>
$=24 + 23 + 22 ... + 1$<br>
$= 300$<br>
Consequently, the total number of combinations is<br>
$N_{2h} = $N_{x,2h} \times $N_{y,2h} = 144 \times 300 = 43200$<br>

If we do the same for the other type of Haar features, which leads to over more than 180,000 features being calculated in this window as claimed in the <a href="#ref2">paper</a>. It becomes very computationally expensive to calculate the pixel difference for all the features in order to evaluate the features. To avoid such costly computations the concept of an <b>integral image</b> (also known as a summed-area table) was introduced.

An integral image gives a fast and simple way to calculate the value of any haar-like feature. Instead of computing at every pixel, it instead creates sub-rectangles and creates array references for each of those sub-rectangles. These are then used to compute the Haar features. The value for location (x, y) on the integral image is the sum of the pixels above and to the left of the (x, y) on the original image plus itself.

For example, from below to calculate the (2,2) = 16: 1+3+12+4 = 20.
<img src="images/integral.png" />

<h2>References</h2>
<ol>
  <li id="ref1">Viola, Paul, and Michael Jones. "Rapid object detection using a boosted cascade of simple features." Proceedings of the 2001 IEEE computer society conference on computer vision and pattern recognition. CVPR 2001. Vol. 1. Ieee, 2001.</li>
  <li id="ref2">Viola, Paul, and Michael J. Jones. "Robust real-time face detection." International journal of computer vision 57 (2004): 137-154.</li>
</ol>
