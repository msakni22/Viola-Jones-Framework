# Viola-Jones-Framework
The <b>Viola-Jones Object Detection Framework</b>, developed by Paul Viola and Michael Jones in 2001, is an innovative machine learning algorithm specifically designed for fast, accurate face detection. It was primarily motivated by the problem of face detection, although it can be adapted to detect other classes of objects. Training the framework is relatively slow, but it enables objects to be detected quickly and accurately. In fact, it can detect human faces very effectively, and in real time.

Viola–Jones is essentially a boosted feature learning algorithm trained by running a modified AdaBoost algorithm on Haar feature classifiers to find a sequence of classifiers <i>f<sub>1</sub>, f<sub>2</sub>, ..., f<sub>n</sub></i>. Haar feature classifiers, while simple, allow for very fast computation. The modified AdaBoost algorithm constructs a strong classifier by combining many weak classifiers, enabling accurate and efficient object detection.

The framework consists of several key components that work together to achieve efficient and accurate object detection:
<ol>
  <li>Selecting Haar-like features</li>
  <li>Creating an Integral image</li>
  <li>Running AdaBoost training</li>
  <li>Creating classifier cascades</li>
</ol>
<h2>Core Components</h2>


<h2>References</h2>
<ol>
  <li>Viola, Paul, and Michael Jones. "Rapid object detection using a boosted cascade of simple features." Proceedings of the 2001 IEEE computer society conference on computer vision and pattern recognition. CVPR 2001. Vol. 1. Ieee, 2001.</li>
  <li>Viola, Paul, and Michael J. Jones. "Robust real-time face detection." International journal of computer vision 57 (2004): 137-154.</li>
</ol>
