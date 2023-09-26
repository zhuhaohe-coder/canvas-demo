## 邂逅Canvas

### 什么是Canvas

Canvas 最初由Apple于2004 年引入，用于Mac OS X WebKit组件，同时给Safari浏览器等应用程序提供支持。后来，它被 Gecko内核的浏览器（尤其是Mozilla Firefox），Opera和Chrome实现，并被W3C提议为下一代的**标准元素（HTML5新增元素）**。 

Canvas提供了非常多的**JavaScript绘图API**（比如：绘制路径、矩形、圆、文本和图像等方法）, 可以绘制各种2D图形。 

**Canvas API 主要用于绘制 2D 图形。**当然也可以使用 Canvas 提供给的 WebGL API 来绘制 3D 图形。

### 应用场景

- 动画
- 游戏画面
- 数据可视化
- 图片编辑
- 实时视频处理

### 优点

- Canvas提供的功能更加原始，==适合像素级处理，动态渲染和量大数据的绘制==，如：图片编辑、热力图、炫光尾迹特效等。
- Canvas非常适合==图像密集型的游戏开发==，适合频繁重绘大量的图形对象。
- Canvas能够以 .png 或 .jpg 格式保存图像，适合==对图片进行像素级的处理==

### 缺点

- 在移动端如果Canvas使用数量过多，会使==内存占用==超出了手机承受范围，可能会导致浏览器崩溃。
- Canvas 绘图==只能通过JavaScript脚本操作==。
- Canvas 是由一个个像素点构成的图形，==放大会使图形变得颗粒状和像素化（导致图形模糊）==。

### 使用Canvas的注意事项

- `<canvas>`和`<img>` 元素很相像，不同就是它**没有 src 和 alt 属性**。
- `<canvas>`标签只有两个属性——width和height( 单位默认为px )。没宽高时，canvas 会==初始化宽为 300px 和高为 150px==
- `<canvas>`元素必须需要结束标签`</canvas>`, 如结束标签不存在，则文档其余部分会被认为是替代内容，将不显示出来。
- 可以通过判断 canvas.getContext() 方法是否存在来检查浏览器是否支持Canvas

## Canvas Grid

假如，HTML 模板中有个宽 150px, 高 150px 的`<canvas>`元素。`<canvas>`元素默认被网格所覆盖。

通常来说网格中的一个单元相当于` canvas `元素中的一像素

网格的原点位于坐标 (0,0) 的左上角。所有图形都相对于该原点绘制。

网格也可理解为是坐标空间（坐标系），坐标原点位于canvas元素左上角（被称为初始坐标系）

网格或坐标空间是可以变换的, ==移动、旋转、缩放坐标系后，默认所有后续变换都将基于新坐标系的变换==。

## 矩形方法

> x 与 y 指定了在canvas画布上所绘制矩形的左上角（相对于原点）的坐标（不支持 undefined ）。

- fillRect(x, y, width, height)： 绘制一个填充的矩形
- strokeRect(x, y, width, height)： 绘制一个矩形的边框
- clearRect(x, y, width, height)： 清除指定矩形区域，让清除部分完全透明。

## 路径

图形的基本元素是路径。==路径是点列表，由线段连接==。这些线段可以具有不同形状、弯曲或不弯曲的、连续或不连续的、不同的宽度和颜色。

 路径是可由很多子路径构成，这些子路径都是在一个列表中，列表中所有子路径（线、弧形等）将构成图形。

 绘制一个路径，甚至一个子路径，通常都是闭合的（会==调用closePath来闭合==）。

### 路径绘制图形的步骤

1. 创建路径起始点（beginPath）
2. 使用绘图命令去画出路径( arc 、lineTo )
3. 把路径闭合( closePath , 不是必须)
4. 通过描边(stroke)或填充路径区域(fill)来渲染图形

### 路径绘制函数

- beginPath()：新建一条路径，生成之后，图形绘制命令被指向到新的路径上绘图，不会关联到旧的路径
- closePath()：闭合路径之后图形绘制命令又重新指向到 beginPath 之前的上下文中。
  - closePath() 方法会==通过绘制一条从当前点到开始点的直线来闭合图形==。
  - 如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做。
- stroke()：通过线条来绘制图形描边（针对当前路径图形）
- fill()：通过填充路径的内容区域生成实心的图形（针对当前路径图形）

- moveTo(x，y) : 当 canvas 初始化或者beginPath()调用后，通常会==使用moveTo(x, y)函数设置绘画的起点==。
- lineTo(x，y) : 绘制一条从当前位置到指定 (x ，y)位置的直线;==开始点和之前绘制的路径有关，之前路径结束点就是接下来的开始点。开始点可以通过moveTo(x, y)函数改变==。
- arc(x, y, radius, startAngle, endAngle, anticlockwise) : 绘制圆弧或者圆
  - x、y：圆心坐标。
  - radius：圆弧半径。
  - startAngle、endAngle：指定开始 和 结束的弧度。以 x 轴为基准（注意：单位是弧度，不是角度）。
  - anticlockwise：为一个布尔值。为 true ，是逆时针方向，为false，是顺时针方向，默认为false。

>  角度与弧度的 JS 表达式：弧度 = ( Math.PI / 180 ) * 角度 ，即 1角度= Math.PI / 180 个弧度

- rect(x, y, width, height) : 绘制一个左上角坐标为（x,y），宽高为 width 、 height 的矩形。

> 当该方法执行的时候，moveTo(x, y) 方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置回默认坐标。

## 样式

### Colors

- fillStyle = color： 设置图形的填充颜色，==需在 fill() 函数前调用==。
- strokeStyle = color： 设置图形轮廓的颜色，==需在 stroke() 函数前调用==。

如果要给图形上不同的颜色，你需要重新设置 fillStyle 或 strokeStyle 的值。

### Transparent

- strokeStyle 和 fillStyle属性结合RGBA
- globalAlpha = 0 ~ 1:  这个属性影响到 canvas 里==所有图形的透明度==

### Line styles

调用lineTo()函数绘制的线条，是可以通过一系列属性来==设置线的样式==。

- lineWidth = value： 设置线条宽度。

- lineCap = type： 设置线条末端样式。

  - butt 截断，默认是 butt
  - round 圆形
  - square 正方形

- lineJoin = type： 设定线条与线条间接合处的样式。

  - round 圆形
  - bevel 斜角
  - miter 斜槽规，默认是 miter

## 文本和图片

### 文本

canvas 提供了两种方法来渲染文本：

- fillText(text, x, y [, maxWidth])
  - 在 (x,y) 位置，填充指定的文本
  - 绘制的最大宽度（可选）
- strokeText(text, x, y [, maxWidth])
  - 在 (x,y) 位置，绘制文本边框
  - 绘制的最大宽度（可选）

#### 文本的样式（需在绘制文本前调用）

- font = value： 当前绘制文本的样式。和 CSS font 属性相同的语法。默认的字体是：10px sans-serif
- textAlign = value：文本对齐选项。可选的值包括：start, end, left, right or center. 默认值是 start
- textBaseline = value：基线对齐选项。可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。==默认值是 alphabetic。==

### 图片

绘制图片，可以使用 drawImage 方法将它渲染到 canvas 里。drawImage 方法有三种形态：

- drawImage(image, x, y): 其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的==起始坐标==。
- drawImage(image, x, y, width, height): width 和 height这两个参数用来控制当向 canvas 画入时应该==缩放的大小==
- drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight): 前 4 个 是定义图像源的切片位置和大小，后 4 个则是定义切片的目标显示位置和大小。

图片的来源，canvas 的 API 可以使用下面这些类型中的一种作为图片的源：

- HTMLImageElement：这些图片是由Image()函数构造出来的，或者任何的`<img>`元素。
- HTMLVideoElement：用一个 HTML 的 `<video>`元素作为你的图片源，可以从视频中==抓取当前帧==作为一个图像。
- HTMLCanvasElement：可以使用另一个`<canvas>`元素作为你的图片源。

## 绘画状态

> Canvas绘画状态是当前绘画时所产生的==样式和变形==的一个快照。

Canvas在绘画时，会产生相应的绘画状态，其实我们是可以将某些绘画的状态存储在栈中来为以后复用

Canvas 绘画状态的可以调用 save 和 restore 方法是用来保存和恢复，这两个方法都没有参数，并且它们是成对存在的。

- save()：保存画布 (canvas) 的所有绘画状态
- restore()：恢复画布 (canvas) 的所有绘画状态

Canvas绘画状态包括:

- 当前应用的变形（即移动，旋转和缩放）
- 以及这些属性：strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, font, textAlign, textBaseline ......
- 当前的裁切路径（clipping path）

## 形变

Canvas和CSS3一样也是支持变形，形变是一种更强大的方法，可以将坐标原点移动到另一点、形变可以对网格进行旋转和缩放。

 Canvas的形变有4种方法实现：

- translate(x, y)：用来移动 canvas 和它的原点到一个不同的位置。
  - x 是左右偏移量，y 是上下偏移量（无需要单位）
- rotate(angle)：用于以原点为中心旋转 canvas，即沿着z轴旋转。
  - angle是旋转的弧度，是顺时针方向，以弧度为单位。
- scale(x, y)：用来增减图形在 canvas 中像素数目，对图形进行缩小或放大。
  - x 为水平缩放因子，y 为垂直缩放因子。如果比 1 小，会缩小图形，如果比 1 大会放大图形。默认值为 1，也支持负数。
- transform(a, b, c, d, e, f)： 允许对变形矩阵直接修改。这个方法是将当前的变形矩阵乘上一个基于自身参数的矩阵。

### 注意事项

- 在==做变形之前先调用 save 方法保存状态==是一个很好的习惯。
- 大多数情况下，调用 restore 方法比手动恢复原先的状态要简单得多。
- 如果在一个循环中做位移，但没有保存和恢复canvas状态，很可能到最后会发现有些东西不见了，因为它很可能已超出canvas画布以外了。
- 形变需要==在绘制图形前调用==。

## 动画

canvas最大的限制就是图像一旦绘制出来，它就是一直保持那样了, 如需要执行动画，不得不==对画布上所有图形进行一帧一帧的重绘==（比如在1秒绘60帧就可绘出流畅的动画了）。为了实现动画，我们需要一些可以定时执行重绘的方法。在Canvas中有三种方法可以实现：

- setInterval
- setTimeout
- requestAnimationFrame

 Canvas 画出一帧动画的基本步骤:

1. ==用 clearRect 方法清空 canvas==。除非接下来绘制的内容会完全充满 canvas（如背景图），否则需要清空所有。
2. ==保存 canvas 状态==，如果加了 canvas 的状态（例如：样式，变形之类），又想在每画一帧之时都是原始状态的话， 需要先保存一下canvas状态，后面再恢复为原始状态。
3. ==绘制动画图形==（animated shapes）， 即绘制动画中的一帧
4. ==恢复 canvas 状态==，如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。

### 定时器的缺陷

定时器不是非常精准的，因为setTimeout的回调函数是放到了宏任务中等待执行。

如果微任务中一直有未处理完成的任务，那么setTimeout的回调函数就有可能不会在指定时间内触发回调。

如果想要更加平稳和更加精准的定时执行某个任务的话，可以使用requestAnimationFrame函数。

### requestAnimationFrame

告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用该函数的回调函数来更新动画。

该方法需要传入一个回调函数作为参数，==该回调函数会在浏览器下一次重绘之前执行==

若想在浏览器下次重绘之前继续更新下一帧动画，那么在回调函数自身内必须再次调用 requestAnimationFrame()
