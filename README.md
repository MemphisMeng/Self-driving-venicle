---


---

<h1 id="self-driving-vehicle-training-project">Self-Driving Vehicle Training Project</h1>
<h2 id="introduction">Introduction</h2>
<p>In this project I am going to leverage the deep learning techniques in computer vision field and motivates the self-driving car engined by <a href="https://github.com/udacity/self-driving-car-sim">Udacity</a>. I am going to apply OpenCV to recognize the video information in the self-driving program, which enables the car to decide how big and which direction the steering angle should be. The decision making will be trained by a neural network powered by TensorFlow over 20 epochs.</p>
<h2 id="eda">EDA</h2>
<p>My training data is retrieved from this <a href="https://github.com/rslim087a/track.git">repo</a>. To begin with, I checked the distribution of the steering angles.<br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/original%20distribution.png" alt="enter image description here"><br>
The distribution is extremely imbalanced because a car inclines to run straight which does not need to change the steering angle. This probably get the model because it would be more likely to estimate the steering angle in an unknown situation to be zero. To minimize the error, I downsized the scale of the the records whose steering angles are zero. In this way we have a more balanced dataset.<br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/downsized%20distribution.png" alt="enter image description here"><br>
Meanwhile, I also made sure the stratification in both the training set and validation set is similar.<br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/train%20vs%20valid.png" alt="enter image description here"></p>
<h2 id="image-preprocessing">Image Preprocessing</h2>
<p>Since I had cut off the dataset, the dataset might not be sufficient and would cause variance error. Then augmentation was in need. I applied 4 different techniques to one image, e.g. zooming in, panning, flipping and modifying the brightness randomly.<br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/zoomed.png" alt="zooming"><br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/panned.png" alt="panning"><br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/random%20brightness.png" alt="random brightness"><br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/flipped.png" alt="Flippeding"><br>
When I got a larger dataset, I did the following things to remove irrelevant information (noises):</p>
<ul>
<li>Coverted the color panels of an image from RGB to YUV;</li>
<li>Filtered with Gaussian Blurring;</li>
<li>Resized the image so that the processed one only contains the information about the lanes.<br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/processed.png" alt="enter image description here"></li>
</ul>
<h2 id="modeling">Modeling</h2>
<p>I built a neural network with Tensorflow and here is the structure which turned out to perform the best.</p>

<table>
<thead>
<tr>
<th>Layer (type)</th>
<th>Output Shape</th>
<th>Param #</th>
</tr>
</thead>
<tbody>
<tr>
<td>conv2d(Conv2D)</td>
<td>(None, 31, 98, 24)</td>
<td>1824</td>
</tr>
<tr>
<td>conv2d(Conv2D)</td>
<td>(None, 14, 47, 36)</td>
<td>21636</td>
</tr>
<tr>
<td>conv2d(Conv2D)</td>
<td>(None, 5, 22, 48)</td>
<td>43248</td>
</tr>
<tr>
<td>conv2d(Conv2D)</td>
<td>(None, 3, 20, 64)</td>
<td>27712</td>
</tr>
<tr>
<td>conv2d(Conv2D)</td>
<td>(None, 1, 18, 64)</td>
<td>36928</td>
</tr>
<tr>
<td>flatten(Flatten)</td>
<td>(None, 1152)</td>
<td>0</td>
</tr>
<tr>
<td>dense(Dense)</td>
<td>(None, 100)</td>
<td>115300</td>
</tr>
<tr>
<td>dense(Dense)</td>
<td>(None, 50)</td>
<td>5050</td>
</tr>
<tr>
<td>dense(Dense)</td>
<td>(None, 10)</td>
<td>510</td>
</tr>
<tr>
<td>dense(Dense)</td>
<td>(None, 1)</td>
<td>11</td>
</tr>
</tbody>
</table><h2 id="results">Results</h2>
<p>I managed to reduce the losses to around 0.10, which is accpetable.<br>
<img src="https://github.com/MemphisMeng/Self-driving-venicle/blob/master/images/lines.png" alt="enter image description here"></p>

