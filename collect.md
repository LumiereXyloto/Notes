##### 1.0.1+0.2===0.3吗，为什么？

js采用IEEE 754的双精度64位浮点数标准，存在精度缺失。解决：设置一个误差范围值，通常称为”机器精度“，js中这个值通常是2^-52 ，ES6中Number.EPSILON这个值正等于2^-52 。只要判断(0.1+0.2)-0.3小于Number.EPSILON，在这个误差的范围内就可以判定0.1+0.2===0.3为true



##### 2.Number()的存储空间是多大，如果后台发送了一个超过最大字节的数字怎们办？

js的number类型有个最大值（安全值）。即2的53次方，为9007199254740992。如果超过这个值，那么js会出现不精确的问题。这个值为16位。 

解决大数运算：

+ 将大数的每一位存进数组，然后对两个数组的每一个位单独运算，得到运算结果。 
+ 使用插件
+ 字符串转换



##### 3.垂直居中实现方式

+ 通过设置div的transform: translateY(-50%)，意思是使得div向上平移（translate）自身高度的一半(50%) ，不必提前知道居中元素的尺寸

    ```css
    html,body {
                width: 100%;
                height: 100%;
                margin: 0;
                padding: 0;
               }
    .content {
                width: 300px;
                height: 300px;
                background: orange;
                margin: 0 auto; /*水平居中*/
                position: relative;
                top: 50%; /*偏移*/
                margin-top: -150px; / transform: translateY(-50%);
              }
    ```



+ flex

    ```
            body {
                display: flex;
                align-items: center; /*定义body的元素垂直居中*/
                justify-content: center; /*定义body的里的元素水平居中*/
            }
            .content {
                width: 300px;
                height: 300px;
                background: orange;        
            }
    ```

    ```
    #box {
        width: 300px;
        height: 300px;
        background: #ddd;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }
    #child {
        width: 300px;
        height: 100px;
        background: #08BC67;
        line-height: 100px;
    }
    ```



+ 绝对定位和负外边距。兼容性不错，但是有一个小缺点：必须提前知道被居中块级元素的尺寸 

    ```
    #box {
        width: 300px;
        height: 300px;
        background: #ddd;
        position: relative;
    }
    #child {
        width: 150px;
        height: 100px;
        background: orange;
        position: absolute;
        top: 50%;
        margin: -50px 0 0 0;// 居中高度的二分之一
        line-height: 100px;
    }
    ```



+ 绝对定位结合margin: auto。实现方式的两个核心是：把要垂直居中的元素相对于父元素绝对定位，top和bottom设为相等的值 

    ```
    #box {
        width: 300px;
        height: 300px;
        background: #ddd;
        position: relative;
    }
    #child {
        width: 200px;
        height: 100px;
        background: #A1CCFE;
        position: absolute;
        top: 0;
        bottom: 0;
        margin: auto;
        line-height: 100px;
    }
    ```



+ 使用 line-height 对单行文本进行垂直居中

    ```
    #box{
        width: 300px;
        height: 300px;
        background: #ddd;
        line-height: 300px;
    }
    #box img {
        vertical-align: middle;
    }
    ```



+ 使用 display 和 vertical-align 对容器里的文字进行垂直居中

    ```
    #box {
        width: 300px;
        height: 300px;
        background: #ddd;
        display: table;
    }
    #child {
        display: table-cell;
        vertical-align: middle;
    }
    ```

+ vertical-align属性只对拥有valign特性的html元素起作用，例如表格元素中的<td><th>等等，而像<div><span>这样的元素是不行的。

　　valign属性规定单元格中内容的垂直排列方式，语法：<td valign="value">，value的可能取值有四种：

　　top：对内容进行上对齐

　　middle：对内容进行居中对齐

　　bottom：对内容进行下对齐

　　baseline：基线对齐



##### 4.什么是BFC，形成原理是什么？

+ BFC(block formatting context）块级格式化上下文 ，它是指一个独立的块级渲染区域，只有Block-level BOX参与，该区域拥有一套渲染规则来约束块级盒子的布局，且与区域外部无关。 

+ 在进行盒子元素布局的时候，**具有BFC特性的元素可以看做是隔离了的独立容器 ，在这个环境中按照一定规则进行布局不会影响到其它环境中的布局**。 比如浮动元素会形成BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。 也就是说，**如果一个元素符合了成为BFC的条件，该元素内部元素的布局和定位就和外部元素互不影响**(除非内部的盒子建立了新的 BFC)，是一个隔离了的独立容器。（在 CSS3 中，BFC 叫做 Flow Root）。 通俗一点来讲，可以把BFC理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海都不会影响到外部。 

+ BFC形成条件：

  0. body 根元素
  1. 浮动元素，float 除 none 以外的值 
  2. 绝对定位元素，position（absolute，fixed） 
  3. display 为以下其中之一的值 inline-blocks，table-cells，table-captions 
  4. overflow 除了 visible 以外的值（hidden，auto，scroll） 

+ **规则是作用于BFC内部的元素，而条件则是作用于BFC容器的，这点要理解**

+ BFC约束规则：
  1. 内部的Box会在垂直方向上一个接一个的放置
  2. 属于同一个BFC的两个相邻Box的margin会发生重叠，与方向无关。（margin取较大的那一边）
  3. 每个元素的左外边距与包含块的左边界相接触（从左向右），即使浮动元素也是如此。（这说明BFC中子元素不会超出他的包含块，而position为absolute的元素可以超出他的包含块边界）
  4. **BFC的区域不会与float的元素区域重叠**
  5. 计算BFC的高度时，浮动子元素也参与计算
  6. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素，反之亦然

+ BFC在布局中的应用
  1. 阻止外边距折叠

     在标准文档流中，块级标签之间竖直方向的margin会以大的为准，这就是margin的塌陷现象。可以用overflow：hidden产生bfc来解决。 

     ```
     <head>
     div{
         width: 100px;
         height: 100px;
         background: lightblue;
         margin: 100px;
     }
     </head>
     <body>
         <div></div>
         <div></div>
     </body>
     ```

     ![](https://img-blog.csdn.net/20180521165928794?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RGRjE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
     如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中。 

     ```html
     <div class="container">
         <p></p>
     </div>
     <div class="container">
         <p></p>
     </div>
     ```

     ```css
     .container {
         overflow: hidden;
     }
     p {
         width: 100px;
         height: 100px;
         background: lightblue;
         margin: 100px;
     }
     ```

     ![](https://img-blog.csdn.net/20180521170236501?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RGRjE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


  2. 包含浮动元素

     度塌陷问题，在通常情况下父元素的高度会被子元素撑开，而在这里因为其子元素为浮动元素所以父元素发生了高度坍塌，上下边界重合，这时就可以用BFC来清除浮动了。 

     ```html
     <div style="border: 1px solid #000;">
         <div style="width: 100px;height: 100px;background: grey;float: left;"></div>
     </div>
     ```

     ![](https://img-blog.csdn.net/20180521171344548?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RGRjE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

     ```html
     <div style="border: 1px solid #000;overflow: hidden">
         <div style="width: 100px;height: 100px;background: grey;float: left;"></div>
     </div>
     ```

     ![](https://img-blog.csdn.net/20180521171318521?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RGRjE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

  3. 阻止元素被浮动元素覆盖

     div浮动兄弟这该问题：由于左侧块级元素发生了浮动，所以和右侧未发生浮动的块级元素不在同一层内，所以会发生div遮挡问题。可以给右侧元素添加 overflow: hidden，触发BFC来解决遮挡问题。 这个方法可以用来实现两列自适应布局，效果不错，这时候左边的宽度固定，右边的内容自适应宽度。 

     ```html
     <div style="height: 100px;width: 100px;float: left;background: lightblue">我是一个左浮动的元素</div>
     <div style="width: 200px; height: 200px;background: grey">我是一个没有设置浮动, 
     也没有触发 BFC 元素, width: 200px; height:200px; background: grey;</div>
     ```

     ![](https://img-blog.csdn.net/20180521172620959?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RGRjE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

     ```html
     <div style="height: 100px;width: 100px;float: left;background: lightblue">我是一个左浮动的元素</div>
     <div style="width: 200px; height: 200px;background: grey;overflow:hidden">我是一个没有设置浮动, 
     也没有触发 BFC 元素, width: 200px; height:200px; background: grey;</div>
     ```

     ![](https://img-blog.csdn.net/20180521173010154?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RGRjE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 5.BFC与IFC

+ `Block Formatting Context`（BFC | 块级格式化上下文） 和 `Inline Formatting Context`（IFC |行内格式化上下文）。 

+ IFC布局规则：

  在行内格式化上下文中，框(boxes)一个接一个地水平排列，起点是包含块的顶部。水平方向上的 `margin`，`border`和 `padding`在框之间得到保留。框在垂直方向上可以以不同的方式对齐：它们的顶部或底部对齐，或根据其中文字的基线对齐。包含那些框的长方形区域，会形成一行，叫做行框。 

+ 上一段代码

  ```css
  .box{
      width:150px;
      height:150px;
      display:inline-block;
      word-wrap:break-word;
      background:green
  }
  ```

  ```html
  <div class='box'>asdfasdfasdfffffffffffffffffffffffffffffffffffffffffffffffff</div>
  <div class='box'>asdfasdfasdf</div>
  ```

  ![](https://img-blog.csdn.net/20180222092826923)

  可以看到第二个inline-block发生了下陷，这里的知识就涉及到了IFC。给第二个添加vertical-align:top;属性就可以解决问题。 

+ 行级盒子的宽度和高度: **高由font-size决定的，宽等于其子行级盒子的外宽度(margin+border+padding+content width)之和**。 inlinebox也有自己的宽高度计算的方法，宽度取决于内部元素的宽度，最大为父元素的宽度，linebox的高度取决于linebox元素，一般由最高的元素决定linebox的高度。 

+ 行内元素的对齐方式,默认是baseline对齐，如上图所示。

+ inline元素的baseline位置：

  1. inline元素的baseline，为内容盒content-box里面文本框的基线。
  2. inline-block的外边缘就是margin-box的外边缘，
  3. 如果inline-block内部有内容，则baseline为内容最下方的baseline，参照刚才给出的例子。
  4. 如果Inline-block内部无内容，则baseline与margin-box的下边缘重合。
  5. 如果overflow属性不为visible（默认）,则baseline与margin-box的下边缘重合。

##### 6.回流与重绘，如何避免回流？

+ Dom树和Render树：

  ![](https://ask.qcloudimg.com/http-save/1032637/ow59037ell.png?imageView2/2/w/1620)

+ 浏览器渲染过程：

  浏览器渲染过程 

+ visibility：hidden隐藏的元素还是会包含到render tree中，因为visibility：hidden会影响布局（layout），会占有空间。根据css2的标准，render tree中的每个节点都称为Box（Box demensions）,理解页面元素为一个具有填充，边距，边框和位置的盒子。 

+ 重绘：重绘是一个元素外观的改变所触发的浏览器行为，例如改变visibility，outline，背景色属性。浏览器会根据元素的新属性重新绘制，使元素呈现新的外观。 

+ 回流：回流是更明显的一种改变，可以理解为render tree需要重新计算。每个页面至少需要一次回流，就是在页面加载的时候。回流必将引起重绘，但重绘不一定引起回流。

  - 添加或删除可见的DOM元素
  - 元素的位置发生变化
  - 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
  - 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。
  - 页面一开始渲染的时候（这肯定避免不了）
  - 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）
  - 激活css伪类 
  - 计算offsetWidth和offsetHeight等属性 

+ **获取布局信息的操作的时候，会强制队列刷新**，比如当你访问以下属性或者使用以下方法： 

  - offsetTop、offsetLeft、offsetWidth、offsetHeight
  - scrollTop、scrollLeft、scrollWidth、scrollHeight
  - clientTop、clientLeft、clientWidth、clientHeight
  - getComputedStyle()
  - getBoundingClientRect

  以上属性和方法都需要返回最新的布局信息，因此浏览器不得不清空队列，触发回流重绘来返回正确的值。因此，我们在修改样式的时候，**最好避免使用上面列出的属性，他们都会刷新渲染队列。**如果要使用它们，最好将值缓存起来。 

+ 减少重绘和回流：

  1. 合并多次对DOM和样式的修改，然后一次处理掉。 使用CSSText或者替换class

     ```js
     const el = document.getElementById('test');
     el.style.padding = '5px';
     el.style.borderLeft = '1px';
     el.style.borderRight = '2px';
     ```

     ```javascript
     const el = document.getElementById('test'); 
     el.style.cssText += 'border-left: 1px; border-right: 2px; padding: 5px;';
     ```

     ```javascript
     const el = document.getElementById('test');
     el.className += ' active';
     ```

  2. 批量修改Dom，先使元素脱离文档流，再对其进行多次修改，最后将元素带回到文档中。

     该过程的第一步和第三步可能会引起回流，但是经过第一步之后，对DOM的所有修改都不会引起回流重绘，因为它已经不在渲染树了。

     + **隐藏元素，应用修改，重新显示** 
     + **使用文档片段(document fragment)在当前DOM之外构建一个子树，再把它拷贝回文档** 
     + **将原始元素拷贝到一个脱离文档的节点中，修改节点后，再替换原始的元素。** 
     + **现代浏览器会使用队列来储存多次修改，进行优化，所以对这个优化方案，我们其实不用优先考虑。** 

  3. 避免触发同步布局事件

     当我们访问元素的一些属性的时候，会导致浏览器强制清空队列，进行强制同步布局。在每次循环的时候，都读取了box的一个offsetWidth属性值，然后利用它来更新p标签的width属性。这就导致了每一次循环的时候，浏览器都必须先使上一次循环中的样式更新操作生效，才能响应本次循环的样式读取操作。**每一次循环都会强制浏览器刷新队列**。 

     ```javascript
     function initP() {
         for (let i = 0; i < paragraphs.length; i++) {
             paragraphs[i].style.width = box.offsetWidth + 'px';
         }
     }
     ```

     ```javascript
     const width = box.offsetWidth;
     function initP() {
         for (let i = 0; i < paragraphs.length; i++) {
             paragraphs[i].style.width = width + 'px';
         }
     }
     ```

  4. 对于复杂动画效果,使用绝对定位让其脱离文档流

  5. css3硬件加速（GPU加速）

     常见的触发硬件加速的css属性：

     - transform
     - opacity
     - filters
     - Will-change

##### 7.display: none ,visibility: hidden,opacity:0

+ display：none让元素消失，脱离文档流

+ visibility: hidden让元素隐藏，还在文档流中，占用空间，相当于透明化了

+ opacity:0透明度为0，即完全透明，还在文档流中，占用空间。与visibility: hidden的区别是opacity可设置不同程度的透明度

+ visibility:hidden比display:none性能上要好，display:none切换显示时产生回流，而visibility切换是否显示时则不会引起回流。

 

##### 8.从输入一个URL到页面渲染流程

**一、获取ip地址**

+ 首先会在浏览器的缓存中查找，是否缓存了URL，如果有，就直接向该URL对应的服务器发送请求；如果没有则进行下一步;
+ 在本地的hosts文件中是否保存了该URL和其对应的IP地址，如果保存了，就直接向该URL对应的服务器发送请求；如果没有则进行下一步；
+ 向本地DNS服务器（一般由本地网络接入服务器提供商提供，比如移动）发送DNS请求，本地DNS服务器会首先查询它的缓存记录，如果有就将该域名对应的IP地址返回给用户，如果没有则进行下一步；
+ 首先向根域名服务器发送DNS查询请求，根域名服务器返回给可能保存了该域名的一级域名服务器地址；本地主机再根据返回的地址，向一级域名服务器发送DNS查询请求；...一直迭代，直到找到对应的域名存放的服务器，向其发送DNS查询请求，该域名服务器返回该域名对应的IP地址；

**二、TCP/IP连接**

+ 三次握手？：如果两次握手，如下面的对话只有前两句，有可能出现的问题是：客户端之前发送了一个连接请求报文，由于网络原因滞留在网络中，后来到达服务器端，服务器接收到该请求，就会建立连接，等待客户端传送数据。而此时客户端压根就不知道发生了什么，白白造成了服务器资源浪费。

  ![](https://segmentfault.com/img/bV8dk5?w=640&h=497)

  相当于：

  1. 客户端：我要请求数据可以吗？
  2. 服务器：可以的
  3. 客户端：好的

**三、浏览器向web服务器发送http请求**

客户机与服务器建立连接后就可以通信了 

+ 静态资源：如果客户端请求的是静态资源，则web服务器根据URL地址到服务器的对应路径下查找文件，然后给客户端返回一个HTTP响应，包括状态行、响应头和响应正文。
+ 动态资源：如果客户端请求的是动态资源，则web服务器会调用CGI/VM执行程序完成相应的操作，如查询数据库，然后返回查询结果数据集，并将运行的结果--HTML文件返回给web服务器。Web服务器再将HTML文件返回给用户。

**四、浏览器渲染**

浏览器拿到HTML文件后，根据渲染规则进行渲染： 

1. 解析HTML，构建DOM树
2. 解析CSS，生成CSS规则树
3. 合并DOM树和CSS规则树，生成render树
4. 布局render树
5. 绘制render数、树，即绘制页面像素信息
6. GPU将各层合成，结果呈现在浏览器窗口中。

**五、四次挥手**

客户端没有数据发送时就需要断开连接，以释放服务器资源。 

![](https://segmentfault.com/img/bV8dj9?w=353&h=461)

相当于：

1. 客户端：我没有数据要发送了，打算断开连接
2. 服务器：你的请求我收到了，我这还有数据没有发送完成，你等下
3. 服务器：我的数据发送完毕，可以断开连接了
4. 客户端：ok，你断开连接吧（客户端独白：我将在2倍的最大报文段生存时间后关闭连接。如果我再次收到服务器的消息，我就知道服务器没有收到我的这句话，我就再发送一遍）。

最终服务器收到该客户端发送的消息断开连接，客户端也关闭连接。 



