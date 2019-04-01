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



##### 9.JavaScript脚本加载defer与async特性

+ JavaScript代码的执行会阻塞页面的解析渲染以及其他资源的下载

+ script标签的位置：

  传统所有script元素都放在head元素中，必须等到全部js代码都被下载、解析、执行完毕后，才能开始呈现网页的内容（浏览器在遇到<body>标签时才开始呈现内容），这在需要很多js代码的页面来说，会造成浏览器在呈现页面时出现明显的延迟，而延迟期间的浏览器窗口将是一片空白。因此。一般把script标签放在</body>标签前面。

+ defer&async：

  + 除IE9以下，其他浏览器不支持内部脚本defer属性，IE10+才支持async特性，opera mini不支持async特性，另外，async是不支持内部脚本的。
  + defer="defer"和async="true/false"
  + 没有defer或async，浏览器会立即加载并执行指定的JS脚本，也就是说，不等待后续载入的文档元素，读到JS脚本就加载并执行。
  + 有async，加载后续文档元素的过程将和JS的加载与执行并行进行（异步）。
  + 有defer，加载后续文档元素的过程将和JS的加载并行进行（异步），但JS的执行要在所有文档元素解析完成之后，DOMContentLoaded 事件触发之前完成。

+ 共同点

  + 不会阻塞文档元素的加载。
  + 使用这两个属性的脚本中不能调用document.write方法。指定异步脚本的目的是不让页面解析来等待脚本的下载和执行，从而异步加载页面其他内容，因此，建议异步脚本不要在加载期间修改DOM。
  + 允许不定义属性值，仅仅使用属性名。
  + 只适用于外部脚本（IE9以下支持，但是把延迟脚本放在页面底部仍然是最佳选择）

+ 不同点

  + async让浏览器能并行下载脚本且不阻塞浏览器的文档解析和渲染，下载完成后脚本立即执行，可能无序执行，取决于下载完成的时间。一定会在window.onload之前执行，但可能在document的DOMContentLoaded之前或之后执行。
  + defer使得浏览器能延迟脚本的下载，等document文档载入和解析完成后，按照他们在文档中出现顺序再去下载解析。也就是说defer属性的<script>就类似于将<script>放在body底部的效果，会在document的DOMContentLoaded事件之前执行。

+ 不支持async的浏览器可以动态创建script，通过window.onload方法确保页面加载完毕再将script标签插入到DOM中。实现脚本的异步载入和执行。

  ```javascript
  window.onload = function(){
    var script = document.createElement('script');
    script.setAttribute("type","text/javascript");
    script.src = url;
    document.body.appendChild(script);
  }
  ```



##### 10.判断一个对象是否为空

1. ES6的Object.keys()方法

   ```javascript
   var data = {};
   var arr = Object.keys(data);
   console.log(arr.length == 0);// true
   ```

2. Object.getOwnPropertyNames()方法

   ```javascript
   var data = {};
   var arr = Object.getOwnPropertyNames(data);
   console.log(arr.length == 0);// true
   ```

3. JSON.stringify()

   ```javascript
   var data = {};
   var result = (JSON.stringify(data) == "{}");
   console.log(result);// true
   ```

4. for in遍历

   ```javascript
   var data = {};
   var result = function() {
     for(var key in data) {
       return false;
     }
     return true;
   }
   alert(result);// true
   ```


##### 11.事件处理机制和事件委托

+ 事件流：指从页面中接收事件的顺序，有冒泡流和捕获流。

+ DOM2级事件流：事件捕获阶段、处于目标阶段、事件冒泡阶段

  ![](https://images2018.cnblogs.com/blog/1414709/201808/1414709-20180818123542724-735687837.png)

  假设定义的事件绑定在图中div上，实际的div元素在捕获阶段不会接收到事件，意味着在捕获阶段，事件从document到<div>后就停止了。下一个阶段是“处于目标阶段”，于是事件在div上发生，并在事件处理中被看成是冒泡阶段的一部分。最后，冒泡阶段发生，事件又传播回文档。

+ DOM2级事件处理程序：

  addEventListener(eventName,handlers,boolean)和removeEventListener(),两个方法都一样接收三个参数,要处理的事件名,第二个是事件处理程序函数,第三个值为布尔值。布尔值是true,表示在捕获阶段调用事件处理程序。false时表示在事件冒泡阶段调用事件处理程序,一般建议在冒泡阶段使用,特殊情况才在捕获阶段。

  通过addEventListener()添加的事件处理程序只能用removeEventListener()来移除，并且需要参数**完全一致**，比如参数都是同一个函数的引用。

+ 事件对象：

  事件被触发时，会默认给事件处理程序传入一个参数e , 表示事件对象。**在事件处理程序内部，对象this始终等于currentTarget的值，target包含事件的实际目标。**（e.currentTarget === this）

  + preventDefault() 阻止事件的默认行为
  + event.stopPropagation()可以阻止事件的传播.,取消进一步的事件冒泡或者捕获

+ 事件委托：

  + 利用事件冒泡，只在外层父元素指定一个事件处理程序，就可以管理某一类型的所有事件。

  + 优点：

    + 减少内存消耗：如果每个dom节点都绑定事件，那么会增加很多与dom的交互，同时会保存很多对象占用内存，这两者都将导致页面性能变低。而我们如果只使用一个代理，那么会减少很多dom交互和内存占用。
    + 动态绑定：
      新生dom节点如果用原来的方式是无法绑定事件的，用委托的方式这方面可以轻松实现，保证父元素存在即可。

  + 例子：

    ```html
    <ul id="myLinks">
      <li id="goSomewhere">Go somewhere</li>
      <li id="doSomething">Do something</li>
      <li id="sayHi">Say hi</li>
    </ul>
    ```

    ```javascript
    var item1 = document.getElementById("goSomewhere");
    var item2 = document.getElementById("doSomething");
    var item3 = document.getElementById("sayHi");
    
    document.addEventListener("click", function (event) {
      var target = event.target;
      switch (target.id) {
        case "doSomething":
          document.title = "事件委托";
          break;
        case "goSomewhere":
          location.href = "http://www.baidu.com";
          break;
        case "sayHi": 
          alert("hi");
          break;
      }
    })
    ```

  + 注意事项：

    使用“事件委托”时，并不是说把事件委托给的元素越靠近顶层就越好。事件冒泡的过程也需要耗时，越靠近顶层，事件的”事件传播链”越长，也就越耗时。如果DOM嵌套结构很深，事件冒泡通过大量祖先元素会导致性能损失。

##### 12.load和DOMContentLoaded事件

+ Dom文档加载步骤：
  1. 解析html结构
  2. 加载外部脚本和样式表文件
  3. 解析并执行脚本代码
  4. 构造HTML DOM模型   //DOMContentLoaded执行点
  5. 加载图片等外部文件
  6. 页面加载完毕  //load

+ 应用场景：

  给元素绑定事件处理函数，将绑定的函数放在这两个事件的回调中，保证能在页面元素加载完毕之后再绑定事件的函数。



##### 13.HTTP状态码

+ 1xx：指示信息–表示请求已接收，继续处理。
+ 2xx：成功–表示请求已被成功接收、理解、接受。
+ 3xx：重定向–要完成请求必须进行更进一步的操作。
+ 4xx：客户端错误–请求有语法错误或请求无法实现。
+ 5xx：服务器端错误–服务器未能实现合法的请求。
+ 常见状态码：
  + 200：OK。请求成功，一般用于GET与POST请求。
  + 204：No Content。无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档。
  + 301：Moved Permanently。永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替。
  + 302：Found。临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI。
  + 304：Not Modified。未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源。
  + 400：Bad Request。客户端请求的语法错误，服务器无法理解。
  + 401：Unauthorized。请求要求用户的身份认证。
  + 403：Forbidden。服务器理解请求客户端的请求，但是拒绝执行此请求。
  + 404：Not Found。服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面
  + 500：Internal Server Error。服务器内部错误，无法完成请求。



##### 14.移动端触摸事件

+ touchstart: 当手指触摸屏幕时候触发；

+ touchmove: 当手指在屏幕上滑动的时候连续触发；可以调用阻止默认事件event.preventDefault()阻止屏幕滚动；

+ touchend: 手指离开屏幕时触发；

+ touchcancel: 系统停止跟踪触摸时触发；

  以上事件都会冒泡，而且都可以取消冒泡。

+ 触摸事件属性：

  + touches: 跟踪返回Touch对象的数组；
  + targetTouchs: 特定事件目标的Touch对象的数组；
  + changeTouchs: 上次触摸以来改变了的Touch对象的数组；

+ Touch对象包含的属性：

  + clientX: 触摸目标在浏览器中的x坐标
  + clientY: 触摸目标在浏览器中的y坐标
  + pageX: 触摸目标在当前DOM中的x坐标
  + pageY: 触摸目标在当前DOM中的y坐标
  + screenX: 触摸目标在屏幕中的x坐标
  + screenY: 触摸目标在屏幕中的y坐标
  + target: 触摸的DOM节点目标



##### 15.meta标签用法

1. SEO搜索引擎优化
2. 定义页面的language
3. 自动刷新并指向新的页面
4. 实现网页转换时的动态效果
5. 控制页面缓冲
6. 网页的顶级定级评价
7. 控制窗口显示

+ name/content属性：

  + keywords告诉搜索引擎网页的关键字

  + description告诉搜索引擎网站的主要内容

  + robots告诉搜索机器人哪些页面需要索引，哪些页面不需要索引

    对应的content有all,none,index,noindex,follow,nofollow。默认是all

  + author

  + renderer告诉浏览器渲染模式

  + viewport视图模式

+ http-equiv/content属性：

  + X-UA-Compatible浏览模式

  + expires设定网页到期时间，到期后必须到服务器上重新传输，必须使用GMT时间格式

    ```html
    <meta http-equiv="expires"content="Fri,01Jan201618:18:18GMT">
    ```

  + Pragma(cache模式)

    如果禁止浏览器从本地缓存访问则将content设为no-cache

  + Refresh自动刷新并在多少秒后且指向新的页面

    ```html
    <meta http-equiv="Refresh" content="2;URL=http://www.lbw.com">
    ```

  + Set-cookie 网页如果过期，存盘的cookie也将被删除，GMT时间格式

    ```html
    <meta http-equiv="Set-Cookie"content="cookievalue=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/">
    ```

  + Window-target   当content为_blank，强制当前窗口以独立页面显示

  + content-Type 显示字符集

  + content-Language 显示语言

  + Cache-Control 指定请求和响应遵循的缓存机制

