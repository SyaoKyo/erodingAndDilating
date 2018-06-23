# erodingAndDilating
这次将使用OpenCV2.14提供的两种基本的形态学操作,腐蚀与膨胀(Erosion 与 Dilation):
<hr>
<h2>原理</h2>
<ul>
<li>简单来讲，形态学操作就是基于形状的一系列图像处理操作。通过将 结构元素 作用于输入图像来产生输出图像。
</li>
<li>最基本的形态学操作有二：腐蚀与膨胀(Erosion 与 Dilation)。 他们的运用广泛:
<br>消除噪声
<br>分割(isolate)独立的图像元素，以及连接(join)相邻的元素。
<br>寻找图像中的明显的极大值区域或极小值区域。
</li>
<li>通过以下图像，我们简要来讨论一下膨胀与腐蚀操作(译者注：注意这张图像中的字母为黑色，背景为白色，而不是一般意义的背景为黑色，前景为白色）:
<div align=center>
<img alt="Original image" src="./pic/Morphology_1_Tutorial_Theory_Original_Image.png" />
</div>
</li>
</ul>
<ol>
<li>
<h3>膨胀</h3>
<ul>
<li>此操作将图像 A 与任意形状的内核 (B)，通常为正方形或圆形,进行卷积。</li>
<li>内核 B 有一个可定义的 锚点, 通常定义为内核中心点。</li>
<li>进行膨胀操作时，将内核 B 划过图像,将内核 B 覆盖区域的最大相素值提取，并代替锚点位置的相素。显然，这一最大化操作将会导致图像中的亮区开始”扩展” (因此有了术语膨胀 dilation )。对上图采用膨胀操作我们得到:
<div align=center>
<img alt="Dilation result - Theory example" src="./pic/Morphology_1_Tutorial_Theory_Dilation.png" />
</div>
背景(白色)膨胀，而黑色字母缩小了
</li>
</ul>
</li>
<li>
<h3>腐蚀</h3>
<ul>
<li>腐蚀在形态学操作家族里是膨胀操作的孪生姐妹。它提取的是内核覆盖下的相素最小值。</li>
<li>进行腐蚀操作时，将内核 B 划过图像,将内核 B 覆盖区域的最小相素值提取，并代替锚点位置的相素。</li>
<li>以与膨胀相同的图像作为样本,我们使用腐蚀操作。从下面的结果图我们看到亮区(背景)变细，而黑色区域(字母)则变大了。
<div align=center>
<img alt="Erosion result - Theory example" src="./pic/Morphology_1_Tutorial_Theory_Erosion.png" />
</div>
</li>
</ul>
</li>
</ol>
<hr>
<h2>代码解释</h2>
<ol>
<li>腐蚀（Erosion）：
<code>
<pre>
void Erosion(int, void*)
{
	int erosion_type;
	if (erosion_elem == 0) 
	{
		erosion_type = MORPH_RECT; 
	}
	else if (erosion_elem == 1) 
	{
		erosion_type = MORPH_CROSS; 
	}
	else if (erosion_elem == 2) 
	{
		erosion_type = MORPH_ELLIPSE; 
	}
	Mat element = getStructuringElement(erosion_type,
		Size(2 * erosion_size + 1, 2 * erosion_size + 1),
		Point(erosion_size, erosion_size));
	// 腐蚀操作
	erode(src, erosion_dst, element);
	imshow("Erosion Demo", erosion_dst);
}
</pre>
</code>
<ul>
<li>进行腐蚀操作的函数是 <a class="reference external" href="http://opencv.jp/opencv-2.2_org/cpp/imgproc_image_filtering.html#cv-erode">erode</a> 。 它接受了三个参数:
<ul>
<li>src: 原图像</li>
<li>erosion_dst: 输出图像</li>
<li>element: 腐蚀操作的内核。 如果不指定，默认为一个简单的 3x3 矩阵。否则，我们就要明确指定它的形状，可以使用函数 <a class="reference external" href="http://opencv.jp/opencv-2.2_org/cpp/imgproc_image_filtering.html#cv-getstructuringelement">getStructuringElement</a>:
<code>
<pre>
Mat element = getStructuringElement(erosion_type,
		Size(2 * erosion_size + 1, 2 * erosion_size + 1),
		Point(erosion_size, erosion_size));
</pre>
</code>
我们可以为我们的内核选择三种形状之一:
<ul>
<li>矩形: MORPH_RECT</li>
<li>交叉形: MORPH_CROSS</li>
<li>椭圆形: MORPH_ELLIPSE</li>
</ul>
然后，我们还需要指定内核大小，以及 锚点 位置。不指定锚点位置，则默认锚点在内核中心位置。
</li>
</ul>
</li>
<li>就这些了，我们现在可以对图像进行腐蚀操作了。</li>
</ul>
</li>
<li>膨胀(Dilation):
<code>
<pre>
void Dilation(int, void*)
{
	int dilation_type;
	if (dilation_elem == 0) 
	{
		dilation_type = MORPH_RECT; 
	}
	else if (dilation_elem == 1) 
	{
		dilation_type = MORPH_CROSS;
	}
	else if (dilation_elem == 2) 
	{
		dilation_type = MORPH_ELLIPSE; 
	}
	Mat element = getStructuringElement(dilation_type,
		Size(2 * dilation_size + 1, 2 * dilation_size + 1),
		Point(dilation_size, dilation_size));
	//膨胀操作
	dilate(src, dilation_dst, element);
	imshow("Dilation Demo", dilation_dst);
}
</pre>
</code>
可见膨胀的代码与腐蚀的基本上是相似的，这里就不过多累述。
</li>
</ol>
