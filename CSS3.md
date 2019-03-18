### 常用CSS3属性汇总

##### CSS3边框

+ border-radius 圆角边框

  ``` border-radius:25px;
  border-radius:25px;
  ```


+ box-shadow 边框阴影
  ```
  box-shadow: h-shadow v-shadow blur spread color inset;
  ```

  + h-shadow    必需。水平阴影的位置。允许负值。
  + v-shadow    必需。垂直阴影的位置。允许负值。
  + blur    可选。模糊距离。
  + spread    可选。阴影的尺寸。(**blur模糊距离是算在盒子的长宽里面的，spread阴影的尺寸会增大阴影盒子的长宽**)
  + color    可选。阴影的颜色。请参阅 CSS 颜色值。
  + inset    可选。将外部阴影 (outset) 改为内部阴影。



+ border-image 边框图片

  border-image 属性是一个简写属性，用于设置以下属性：

  - border-image-source    边框的图片的路径 
  - border-image-slice    图片边框向内偏移 
  - border-image-width    图片边框的宽度 
  - border-image-outset    边框图像区域超出边框的量 
  - border-image-repeat    图像边框是否应平铺(repeated)、铺满(rounded)或拉伸(stretched) 

  
#####  CSS3背景

+ background-clip 规定背景的绘制区域

  + border-box 背景被裁剪到边框盒。 
  + padding-box 背景被裁剪到内边距框。 
  + content-box  背景被裁剪到内容框。 

+ background-origin 规定 background-position 属性相对于什么位置来定位 

  + padding-box  背景图像相对于内边距框来定位。 
  + border-box  背景图像相对于边框盒来定位。
  + content-box  背景图像相对于内容框来定位。 

+ background-size 规定背景图像的尺寸 

+ CSS3 多重图片

  ```
  body
  { 
  background-image:url(bg_flower.gif),url(bg_flower_2.gif);
  }
  ```

##### CSS3文本

+ text-overflow 规定当文本溢出包含元素时发生的事情 

  + clip  修剪文本。 
  + ellipsis  显示省略符号来代表被修剪的文本。 
  + *string*  使用给定的字符串来代表被修剪的文本。 

+ text-shadow  向文本设置阴影 

  ```
  text-shadow: h-shadow v-shadow blur color;
  ```
  + h-shadow 必需。水平阴影的位置。允许负值。
  + v-shadow 必需。垂直阴影的位置。允许负值。
  + blur 可选。模糊的距离。
  + color 可选。阴影的颜色。
+ white-space（不是css3属性）
  + normal    默认。空白会被浏览器忽略。
  + pre    空白会被浏览器保留。其行为方式类似 HTML 中的 pre标签。
  + nowrap    文本不会换行，文本会在在同一行上继续，直到遇到 br标签为止。
  + pre-wrap    保留空白符序列，但是正常地进行换行。
  + pre-line    合并空白符序列，但是保留换行符。
  + inherit    规定应该从父元素继承 white-space 属性的值。

##### CSS3字体

+ @font-face 规则，使用方式：首先定义字体的名称（比如 myFirstFont），然后指向该字体文件。

  如需为 HTML 元素使用字体，请通过 font-family 属性来引用字体的名称 (myFirstFont)

  ```
  <style> 
  @font-face
  {
  font-family: myFirstFont;
  src: url('Sansation_Light.ttf'),
       url('Sansation_Light.eot'); /* IE9+ */
  }
  
  div
  {
  font-family:myFirstFont;
  }
  </style>
  ```

##### CSS3 2D转换

+ 通过 CSS3 转换，我们能够对元素进行移动、缩放、转动、拉长或拉伸。 
+ transform
  + translate(x, y) 方法，元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数 
  + rotate(n deg) 方法，元素顺时针旋转给定的角度。允许负值，元素将逆时针旋转。 （**以旋转元素中心为旋转点**）
  + scale(x, y) 方法，元素宽高变化为原始的倍数，根据给定的宽度（X 轴）和高度（Y 轴）参数 
  + scale(x)
  + scale(y)

##### CSS3 3D转换

+ transform
  + rotateX(n deg) 方法，元素围绕其 X 轴以给定的度数进行旋转。
  + rotateY(n deg) 方法，元素围绕其 Y 轴以给定的度数进行旋转。 
  + rotateZ(n deg) 方法，元素围绕其 Z 轴以给定的度数进行旋转。 