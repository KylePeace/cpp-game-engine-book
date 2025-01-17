﻿## 前言

### 为什么写这本书？

在与同事沟通时，会提到如何使用Unity Profile做性能优化，对于内存、耗时大家都能理解到，打包图集、减少DrawCall这些都可以按照一定数值参考。但是对于内存为什么占用这么多，CPU使用率为什么会随着DrawCall数量上升而上升，大家又了解的比较少。

12年末我大学实习进入手游公司，那个时候公司项目是自研引擎，做完项目任务后，我们就会挂上引擎源码，调试学习。一年后总监让我把UI渲染改成图集形式，我信心满满答应下来结果毫无头绪，事情就这么搁置下来，我们也全面转向了Unity。

15年到了新公司，总监也会时常和我吐槽Unity不合理之处，并拿出他的自研引擎炫耀一番。大家已经没有心思研究自研引擎，Unity才是这个快速迭代上线所需要的完美工具。

后来去小公司带项目，团队里就剩我一个人会去想引擎的事，只有讨论优化的时候，我才会讲一些原理，其余都是让他们看我博客。

后手游时代的客户端程序，和前端游时代的程序出现了断层。

Unity可以很方便的做出商业产品，但是了解它背后的原理，才能避开它的一些坑，让做出的产品更加完美。甚至在拥有了它的源码之后，可以对它做更多定制化开发。这份知识不仅限于Unity，任何引擎都是建立在其之上，这能帮你在不同引擎之间快速跳转。

### 本书内容

本书完整介绍一个游戏引擎的所有模块，从最基础的OpenGL环境搭建，到骨骼动画、多线程渲染、阴影实现等等，最后实现一个完整的游戏引擎。

在部分章节结束后，会基于当前章节功能，开发游戏实例。

本书主要内容如下：

第 1 章介绍游戏引擎框架，以Unity为例，介绍游戏引擎组成。

第 2 章介绍OpenGL开发环境搭建，创建一个OpenGL空窗口来入坑。

第 3 章介绍使用OpenGL绘制三角形、正方形、立方体，来熟悉游戏渲染的最基础元素。

第 4 章介绍Shader的概念，编译链接，以及Shader格式、关键字。

第 5 章介绍贴图格式，从直接读取PNG、JPG渲染，然后介绍GPU所使用的的压缩纹理。

第 6 章介绍索引与缓冲区对象，索引就是多个顶点的下标，使用索引可以复用顶点渲染。而缓冲区则是将顶点数据存储于显存中，不用再每一帧都从内存上传到GPU。

第 7 章介绍引擎自定义的Mesh文件格式以及材质的组成。将原来写死在代码中的顶点数据存储到Mesh文件中，将原来写死在代码中的Shader参数存储到材质中。

第 8 章介绍使用Blender制作模型并编写Python代码导出为Mesh文件。

第 9 章介绍如何实现GameObject-Component模式。

第 10 章介绍什么是相机，以及多相机渲染排序。

第 11 章介绍获取鼠标、键盘输入。

第 12 章介绍如何将Demo代码，拆分为引擎源码与项目源码。

第 13 章介绍使用FreeType对指定字符生成单Alpha通道的纹理并渲染，以及使用顶点色实现彩色文字。

第 14 章介绍基础GUI控件的实现，包括UIImage、UIMask、UIText、UIButton。有了这几个基础，可以实现其他复杂的控件。

第 15 章介绍使用FMOD播放MP3、Wav音乐，以及使用FMOD专业音频编辑器制作音效与解析播放。

第 16 章介绍easy_profiler这个C++性能分析库。

第 17 章介绍使用Sol2这个开源库，将Lua集成到引擎，后续使用C++开发引擎，使用Lua编写测试代码。

第 18 章介绍骨骼动画原理，并介绍使用Blender制作骨骼动画、导出到skeleton_anim文件，在引擎解析。

第 19 章介绍骨骼蒙皮动画，在Blender刷权重，导出到weight文件，在引擎解析，渲染。

第 20 章介绍如何从FBX文件导出Mesh、骨骼动画、权重，实例导出古风少女并渲染。

第 21 章介绍多线程渲染，将OpenGL API放到单独的渲染线程，从主线程发命令到渲染线程，将DrawCall的影响分出去，减轻主线程负担。

第 22 章介绍不定时更新。

### 适用读者

写这本书的目的是普及游戏引擎基础知识，面向的读者有一定的Unity经验，对引擎某些点感兴趣即可。

每一章节介绍的知识点，都是供入门了解，并没有深挖。

简单、容易上手、短期目标、不枯燥，是这本书的追求的，太复杂的很容易中途放弃，我花了很多时间在这个上。

1. IDE选择了CLion，开箱即用跨平台。
2. 每一小节开头就贴上了这一节项目地址，找到后把文件夹拖到CLion里面立即可以调试。
3. 文章中的代码片段，开头就是这个代码所在文件以及行数。
4. 一些名词，能和其他名词关联的我都会尽量关联。例如Shader的编译与Link，我用C语言的编译来对比。OpenGL的CS模型，我用网络消息来对比。
5. 对于需要用到其他软件的，例如Blender建模、FMOD、WWise音频制作，我都录制了视频在B站，并贴在文章里。这应该是首次在引擎书里教你如何建模、音频编辑。其他的有Substance、Toolbag插件开发，Renderdoc GPU分析工具都有简单介绍。
6. 渲染的模型动画用美女、帅哥、色彩丰富的物体，更有成就感。
7. 有了一定功能后，就会基于现有功能开发一个小游戏，阶段性成就才能长时间坚持。

不过在简单的同时，很多基础知识没有介绍到，C++、Python、Lua、js、多线程、文件读取，这些书中就不再提了。

只要不放弃，一章一章看下去，一定能做出自己的游戏引擎。

### 学习方式推荐

本以实战为主，大部分章节都有CLion实例项目，项目路径在章节开头或结尾给出。

个人推荐的学习方式如下：
1. CLion打开项目，编译运行，看看效果。
2. 过一遍代码，断点调试一下。
3. 看一遍章节内容。
4. 再过一遍代码。

### 资源下载

本书Markdown以及章节配套项目托管在Github上，读书过程中有疑问、发现错误都可以提Issues。

```bash
Github：https://github.com/ThisisGame/cpp-game-engine-book
```

### 致谢

从社区中来到社区中去，感谢各位开源人。

