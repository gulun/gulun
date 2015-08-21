---
title : Core Graphics
---

#####简介

1. Core Graphics 是一组基于C的API框架，使用Quartz作为绘画引擎，提供了低级别、轻量级、高保真度的2D渲染。

2. 该框架可以用于基于路径的绘图、变换、颜色管理、脱屏渲染，模板、渐变、遮蔽、图像数据管理、图像的创建、遮罩、以及PDF文档的创建、显示和分析。

3. 创建任何界面元素时，iOS 都是用Core Graphics 来将这些元素绘制到窗口中去的。通过实现和重载Core Graphics的方法，可以创建自定义的界面元素。

#####与 Quarz 的关系
1. Quartz 2D 是一组二维绘图与渲染引擎，它是Core Graphics Framework 的一部分。Core Graphics 会使用到这组API。

2. Quartz Core 是一组动画引擎，Core Animation 会使用到这组API。

#####与 UIKit 的关系
1. UIKit 和 Core Graphics 都是cocoa框架提供的绘图API。  
2. UIKit 构建于Core Graphics 之上，能够通过简单的方法使用Core Graphics的大量功能。  
3. UIKit 可以和Core Graphics无缝的协同工作。  
4. Core Graphics 是iOS上所有绘图工作的基石，包括UIKit。  

#####图像上下文  Context
1. 类似于画家的画布，承载绘画的动作。  
2. 绘画动作顺序执行，新动作在前一个动作的基础上完成。
3. 在图形上下文之外，是无法进行绘图操作的。  
虽然可以自己创建一个图形上下文，但是这样做的效率很低（非万不得已不要用）。基本上用于对图片的处理。

#####图形上下文的属性
* 路径 path  
* 阴影 shadow  
* 笔画 stroke  
* 剪裁路径 clip path  
* 线条粗细 line width  
* 混合模式 blend mode  
* 填充色 fill color
* 当前形变矩阵 current transform matrix
* 线条图案 line dash

属性是状态相关的，改变属性会影响所有的后续操作。

#####图形上下文栈
    UIGraphicsPushContext(ctx) //将ctx设置为当前上下文（栈顶）
    UIGraphicsPopContext() //将ctx还原，
    
    CGContextSaveGState(context) //将图形上下文的当前状态保存
    CGContextRestoreGState(context) //将图形上下文状态恢复
    
1. 图形上下文context是以堆栈的形式存放。  
2. 只有在当前的context上绘图才有效。
3. UIKit 只会对当前上下文栈顶的context进行操作，所以需要注意当前栈顶的context是否是你需要操作的上下文    

#####使用场景
用Core Graphics来绘图的一个通常原因就是只是用图片或是图层效果不能轻易地绘制出矢量图形

* 任意多边形 （不仅仅是矩形）
* 曲线、斜线
* 文本
* 渐变


#####使用方式
三个场景，其中每个场景有两种方法绘图（UIKit & CoreGraphics）

* 2种方法绘图  
使用UIKit在context上绘制，UIKit 只会对当前上下文栈顶的context进行操作  
使用Core Graphics 函数在 context上绘制，Core Graphics 函数需要把context作为参数，只绘制在指定使用的context。

* 3种场景  
  1. drawRect  

  2. CALayer delegate: - (void)drawLayer:(CALayer *)layer 
inContext:(CGContextRef )ctx  

  3. 自己创建context来绘制，通常用于对图片的处理。
  
#####绘图步骤
1. 获取上下文（画布）
2. 创建路径（自定义或者调用系统的API）并添加到上下文中。
3. 进行绘图内容的设置（画笔颜色、粗细、填充区域颜色、阴影、连接点形状等）
4. 开始绘图（CGContextDrawPath）
5. 释放路径（CGPathRelease）

#####关于 CGContextDrawPath

CGContextDrawPath  两个 参数 决定填充规则

1. kCGPathFill表示用非零绕数规则，
2. kCGPathEOFill表示用奇偶规则，
3. kCGPathFillStroke     表示填充，
4. kCGPathEOFillStroke   表示描线，不是填充

或者

 1. CGContextFillPath 
  使用非零绕数规则填充当前路径 
 2. CGContextEOFillPath 
  使用奇偶规则填充当前路径 
 3. CGContextStrokePath
  使用非零绕数描线   
 4. CGContextFillRect 
  填充指定的矩形 
 5. CGContextFillRects 
  填充指定的一些矩形 
 6. CGContextFillEllipseInRe ct 
  填充指定矩形中的椭圆 
   




