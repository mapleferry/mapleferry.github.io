---
layout: post
title:  "JavaFX介绍，第一个HelloWorld"
date:   2017-11-28 16:32:25 +800
categories: [编程]
excerpt: "介绍JavaFX架构，以及编写第一个HelloWorld程序。"
share: true
---
### 概述
JavaFX是由甲骨文公司推出的一系列的产品和技术，该产品于2007年5月在JavaOne大会上首次对外公布。JavaFX技术主要应用于创建Rich Internet application（RIAs）。使用此库编写的应用程序可以跨多个平台一致运行。 使用JavaFX开发的应用程序可以在各种设备上运行，如台式计算机，手机，电视，平板电脑等。
为了使用Java编程语言开发GUI应用程序，程序员依赖于诸如高级窗口工具包和Swings之类的库 。 在JavaFX出现之后，这些Java程序员现在可以利用丰富的内容有效地开发GUI应用程序。

<!--more-->
---
### 架构
JavaFX提供了一个完整的API，提供了丰富的类和接口来构建具有丰富图形的GUI应用程序。
![图片](/img/javafx_first_01.jpg)
**这个API的重要包是：**
* javafx.animation - 包含向JavaFX节点添加基于过渡的动画（例如填充，淡化，旋转，缩放和平移）的类。
* javafx.application - 包含一组负责JavaFX应用程序生命周期的类。
* javafx.css - 包含向JavaFX GUI应用程序添加类似CSS样式的类。
* javafx.event - 包含用于传递和处理JavaFX事件的类和接口。
* javafx.geometry - 包含用于定义2D对象并对其执行操作的类。
* javafx.stage - 此包包含JavaFX应用程序的顶级容器类。
* javafx.scene - 这个包提供了类和接口来支持场景图。 此外，它还提供了诸如画布，图表，控件，效果，图像，输入，布局，媒体，油漆，形状，文本，变换，网络等子包。有几个组件支持这种丰富的JavaFX API 。

在JavaFX中，GUI应用程序使用场景图进行编码。 场景图是GUI应用程序构造的起点。 它保存被称为节点的（GUI）应用原语。
节点是一个视觉/图形对象，它可以包括：
* 几何（图形）对象 - （2D和3D），如圆形，矩形，多边形等。
* UI控件 - 例如按钮，复选框，选择框，文本区域等。
* 容器 - （布局窗格），例如边框窗格，网格窗格，流窗格等。
* 媒体元素 - 例如音频，视频和图像对象。
通常，节点的集合形成场景图。 所有这些节点按照如下所示的分级顺序排列。
![图片](/img/javafx_first_02.jpg)
场景图中的每个节点具有单个父节点，并且不包含任何父节点的节点被称为根节点 。同样，每个节点有一个或多个子节点，没有子节点的节点称为叶节点 ; 具有子节点的节点被称为分支节点 。节点实例只能添加到场景图中一次。 场景图的节点可以具有效果，不透明度，变换，事件处理程序，事件处理程序，应用程序特定状态。

### 应用程序结构
一般来说，JavaFX应用程序将有三个主要组件，即舞台，场景和节点 ，如下图所示。
![图片](/img/javafx_first_03.jpg)
stage有两个参数确定它的位置，即宽度和高度。它被分为内容区和装饰（标题栏和边框）。阶段（窗口）包含JavaFX应用程序的所有对象。它由javafx.stage包的Stage类表示。初级阶段由平台本身创建。创建的stage对象作为参数传递给Application类的start（）方法。必须调用show（）方法来显示stage的内容。

### HelloWorld
开发第一个javaFX例子--- HelloWorld。
{% highlight java %}
package com.mf.helloworld;  
  
import javafx.application.Application;  
import javafx.scene.Scene;  
import javafx.scene.control.Button;  
import javafx.scene.layout.StackPane;  
import javafx.stage.Stage;  
  
public class HelloWorld extends Application {  
  
    @Override  
    public void start(Stage primaryStage) throws Exception {  
        Button btn = new Button();  
        btn.setText("say hello world");  
          
          
        //为按钮添加事件  
        //匿名类方式添加事件  
        btn.setOnAction(new EventHandler<ActionEvent>(){  
            @Override  
            public void handle(ActionEvent event) {  
                System.out.println("Hello World!");  
            }  
        });  
                    
        //lambda表达式方式添加事件（java8可以使用这种语法）  
        btn.setOnAction((e) -> {  
            System.out.println("Hello World!");  
        });
  
        StackPane root = new StackPane();  
        root.getChildren().add(btn);  
  
        //场景  
        Scene scene = new Scene(root, 300, 250);  
        primaryStage.setTitle("Hello World!");  
        primaryStage.setScene(scene);  
  
          
        primaryStage.show();  
    }  
  
    public static void main(String[] args) {  
        launch(args);  
    }  
}  
{% endhighlight%}
1. JavaFX应用程序的主类扩展了javafx.application。应用程序类。start()方法是所有JavaFX应用程序的主入口点。
2. JavaFX应用程序定义了用户界面的容器的一个舞台,一个场景。JavaFX的Stage类是顶级JavaFX容器。JavaFX的Scene类是所有内容的容器。该例创造舞台和场景,使场景以给定的像素大小中可见。
3. 在JavaFX,场景的内容表示为一个层次场景图的节点。在这个例子中,根节点是一个StackPane对象,这是一个可调整大小的布局节点。这意味着,当用户改变场（Scene）景大小或者舞台（Stage）大小时，根节点也会跟着改变。
4. 根节点包含一个孩子节点,一个按钮控制文本,再加上一个事件处理程序来打印一个消息当按钮被按下。
5. main方法有时候不是必须得，但在一些集成不齐全的时候，我们建议使用main方法。



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
