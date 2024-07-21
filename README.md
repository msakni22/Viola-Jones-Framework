# Viola-Jones-Framework
<p>The <b>Viola-Jones Object Detection Framework</b>, developed by <b>Paul Viola</b> and <b>Michael Jones</b> in <a href="#ref1">2001</a>, is an innovative machine learning algorithm specifically designed for fast, accurate face detection. It was primarily motivated by the problem of face detection, although it can be adapted to detect other classes of objects. <br> Training the framework is relatively slow, but it enables objects to be detected quickly and accurately. In fact, it can detect human faces very effectively, and in real time.<br></p>

Viola–Jones is essentially a <b>boosted feature learning algorithm</b> trained by running a modified <b>AdaBoost</b> algorithm on <b>Haar feature classifiers</b> to find a sequence of classifiers <i>f<sub>1</sub>, f<sub>2</sub>, ..., f<sub>n</sub></i>.<br>

The framework consists of four main steps which are mainly:
<ol>
  <li>Selecting Haar-like features</li>
  <li>Creating an Integral image</li>
  <li>Running AdaBoost training</li>
  <li>Creating classifier cascades</li>
</ol>
<h3>1. Selecting Haar-like features</h3>
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

After detecting features using Haar-like features, it is crucial to evaluate their effectiveness. This involves calculating the feature value by subtracting the sum of pixel values in the black region from the sum of pixel values in the white region. If the feature value is close to the expected value (e.g., near 1), it indicates a strong presence of the property being detected.<br>

$Feature\textunderscore Value = (\sum Pixel\textunderscore White\textunderscore Region - \sum Pixel\textunderscore Black\textunderscore Region) / Number\textunderscore of\textunderscore Pixel$

In our example, the sum of pixel values in the black region and the white region:<br>
$B = 0.6+0.8+0.8+0.6+0.6+0.8+0.8+0.9=5.9$<br>
$W = 0.1+0.2+0.2+0.3+0.2+0.1+0.2+0.1=1.4$<br>
$Metric = (B - W)/8 = (5.9-1.4)/8 = 0.56$<br>
Therefore, we can infer that there is a 56% probability of detecting the Haar-like feature in the specific area of the image.

The Viola-Jones algorithm calculates many such features across multiple subregions of an image, making the process computationally intensive. To address this, the algorithm employs the concept of <b>Integral Images</b>, which allows for rapid calculation of these features.

<h3>2. Creating an Integral image</h3>
<h2>References</h2>
<ol>
  <li id="ref1">Viola, Paul, and Michael Jones. "Rapid object detection using a boosted cascade of simple features." Proceedings of the 2001 IEEE computer society conference on computer vision and pattern recognition. CVPR 2001. Vol. 1. Ieee, 2001.</li>
  <li id="ref2">Viola, Paul, and Michael J. Jones. "Robust real-time face detection." International journal of computer vision 57 (2004): 137-154.</li>
</ol>
