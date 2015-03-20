title: 使用Mactype渲染字体
date: 2015-03-15 13:27:48
tags:
---

Mactype是在Windows下实现类似Mac下字体渲染效果的程序。长期以来，我一直都使用这个软件替代系统自带的ClearType渲染功能。渲染得到的字体相当不错。

但是，


## 字体

Windows下面自带的雅黑字体效果还是挺不错的，但是还是建议使用其他修改过的字体。字体可以去[极限社区](http://bbs.themex.net/forumdisplay.php?f=90)下载。该论坛也是重要的讨论字体修改和Mactype的网站。

之前用的比较多的是XHei，貌似现在出了新的字体，以后下载看下效果。

## 程序优化

### Firefox

Firefox默认开启的硬件加速，因此无法直接使用Mactype字体渲染。关于具体的设置，参考文档[MacType.Source](https://github.com/renkun-ken/MacType.Source)

打开`about:config`，修改设置如下

| Key                                                      | Value |
|----------------------------------------------------------|-------|
| gfx.direct2d.disabled                                    | true  |
| gfx.font_loader.delay                                    | -1    |
| gfx.font_rendering.cleartype.always_use_for_content;true | true  |
| gfx.font_rendering.cleartype_params.cleartype_level      | 100   |
| gfx.font_rendering.cleartype_params.enhanced_contrast    | 100   |
| gfx.font_rendering.cleartype_params.gamma                | 1400  |
| gfx.font_rendering.cleartype_params.pixel_structure      | 1     |
| gfx.font_rendering.cleartype_params.rendering_mode       | 5     |
| gfx.font_rendering.fallback.always_use_cmaps             | true  |
| gfx.use_text_smoothing_setting                           | true  |

### VisualStudio

与Firefox一样，同样使用了硬件加速，而且偏偏没有任何设置。

在[VS 怎么使用 MacType 的字体渲染？](http://www.zhihu.com/question/24251313)中提出了解决方案：对于ClearType设置为Smoothing的字体，且Visualstudio在显示比例不为100%时可以直接通过Mactype进行渲染。

所以需要使用这样特别制作的字体，从[自制FantasqueSansMono字体+VS缩放插件,完美解决VS2013+Mactype](http://tieba.baidu.com/p/3366845989)下载特别制作过的字体FantasqueSansMono字体。

但是默认VS都会在新开一个窗口的时候重置缩放比例，因此需要使用插件让VS保持显示比较为99%。插件可以参考帖子[Visual Studio 2010 default zoom level](https://stackoverflow.com/questions/4771750/visual-studio-2010-default-zoom-level)，固定了显示比例之后，防止不小心按了`Ctrl+滚轮`而改变了缩放，可以安装插件[Disable Mouse Wheel Zoom](https://visualstudiogallery.msdn.microsoft.com/d088791c-150a-4834-8f28-462696a82bb8)，装上了这几个插件，基本可以欣赏漂亮的字体渲染了。
