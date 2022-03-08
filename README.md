# EasyPhongShaderInUnity
UnityShader about PhongShader and halfLambert
In output.png(结果示例),from left to right, about Pre-Vertex, Pre-Pixel, Pre-Pixel with half lambert.
The Pre-Pixel is smoother than Pre-Vertex.
The Pre-Pixel with half lambert is the brightest.

逐像素比逐顶点过度更加平滑，灰白交界更清晰。
逐顶点由于顶点数目一般少于像素，故计算次数更少，但因为逐顶点这种方法需要依赖线性插值得到像素的光照、颜色，如果遇到模型存在非线性的现象会出现奇怪的问题。
而且由于插值的原因，像素颜色一般暗于顶点，会产生“棱角现象”。

即便上述两种都很好的模拟了漫反射光照，但是在光源背面的阴影处视觉效果不尽人意。
为了提高视觉效果可以采用Half Lambert方法，对于渲染方程c = C_light * M_diffuse * max(0,dot(n,l))中的max(0,dot(n,l)做一个改进。
正是为了防止物体背后的光影响正面的模拟效果，故对光线和法线做max限制，也正是这种符合物理的操作，可能会导致出现物体背后过暗的结果。
所有Half Lambert做了简单的映射来解决dot(n,l)存在负数的问题————a * dot(n,l) + b
不过这只是改善一下视觉效果，并非“正确”，没有物理依据，但是“如果你看到的是正确的，那么ta就是正确的”。
由于这种映射所以原本Pre-Pixel部分阴影被映射出更加明亮的着色，故而最右边的，即Half Lambert看起来是最亮的。

Phongshader 对于各向同性材质可以很好的模拟，但是对于各项异性材质就存在局限性，因为各项异性材质表面比较复杂，不同方向的光照和不同的观察角度结果可能是不同的。
PhongShader是一种很好的模拟，但还有更好的能反映、模拟现实的方法，比如RayTrance。
结束。
