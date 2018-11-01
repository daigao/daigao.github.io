---
layout:     post
title:      WPF中使用AForge
subtitle:   了解AForge在WPF中的使用
date:       2018-11-01
author:     daigao
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - WPF
---

# 一、AForge.NET简介

AForge.NET是一个专门为开发者和研究者基于C#框架设计的，这个框架提供了不同的类库和关于类库的资源，还有很多应用程序例子，包括计算机视觉与人工智能，图像处理，神经网络，遗传算法，机器学习，机器人等领域。  
这个框架由一系列的类库组成。主要包括有：  
- AForge.Imaging —— 一些日常的图像处理和过滤器  
- AForge.Vision —— 计算机视觉应用类库  
- AForge.Neuro —— 神经网络计算库AForge.Genetic -进化算法编程库  
- AForge.MachineLearning —— 机器学习类库  
- AForge.Robotics —— 提供一些机器人的工具类库  
- AForge.Video —— 一系列的视频处理类库  
- AForge.Fuzzy —— 模糊推理系统类库  
- AForge.Controls—— 图像，三维，图表显示控件

# 二、下载与添加AForge引用

打开NuGet包管理器搜索Aforge->选择安装一下插件
- AForge
- AForge.Controls
- AForge.Imaging
- AForge.Math
- AForge.Video
- AForge.Video.DirectShow

# 三、WPF使用Windows Form中的控件

最方便的方法就是利用PictureBox显示摄像头画面，但在WPF中不能直接使用PictureBox。必须通过< WindowsFormsHost></ WindowsFormsHost>来提供交换功能。其解决方法如下：
1. 安装常规方法新建一个WPF应用

2. 添加如下引用
    > WindowsFormsIntegration (与WinForm交互的支持）  
    >System.Windows.Forms (WinForm控件支持)  
    >如果AForge下载的dll，请引用[标题二](#二、下载与添加AForge引用)中的组件

3. 在XAML中添加
xmlns:wf="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"  (用wf代替System.Windows.Forms,即可使用< wf:PictureBox/>添加PictureBox控件

4. 在界面相应位置添加

```xml
<WindowsFormsHost Name="winForm">
　　<wf:PictureBox Name="myPicture"/>
</WindowsFormsHost>
(至此，界面层的设置完成）
```

# 四、using引用

```
using AForge.Video;
using AForge.Video.DirectShow;
```

# 五、代码

### XAML代码
```XMl
<Window x:Class="VideoOrc.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:VideoOrc"
        xmlns:wf="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
        mc:Ignorable="d"
        Title="MainWindow" Height="720" Width="1280" Closed="Window_Closed">
    <Viewbox>
        <Canvas x:Name="canvas" Height="720" Width="1280">
            <WindowsFormsHost Name="winForm">
                <wf:PictureBox x:Name="picBox"  Height="720" Width="1280"></wf:PictureBox>
            </WindowsFormsHost>
        </Canvas>
    </Viewbox>
</Window>
```

### .cs代码
```C#
using System;
using System.Windows;
using AForge.Video;
using AForge.Video.DirectShow;
using System.Drawing;
using System.Threading;

namespace VideoOrc
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        //选择的设备
        public VideoCaptureDevice vcd = null;
        public MainWindow()
        {
            InitializeComponent();
            this.Loaded += MainWindow_Loaded;
            Thread th = new Thread(VideoT);
            th.Start();
            
        }
        

        private void VideoT()
        {
            Thread.Sleep(2000);
            vcd.Start();
        }

        private void MainWindow_Loaded(object sender, RoutedEventArgs e)
        {
            try
            {
                //获取并枚举所有摄像头设备
                FilterInfoCollection cameras =new FilterInfoCollection(FilterCategory.VideoInputDevice);
                //判断设备个，并选择默认设备
                if (cameras.Count>0)
                {
                    vcd = new VideoCaptureDevice(cameras[0].MonikerString);
                    vcd.NewFrame += Vcd_NewFrame;
                }
                else
                {
                    System.Windows.MessageBox.Show("未找到视频输入设备！");
                }
            }
            catch (Exception)
            {

                throw;
            }
        }
        ///<summary>
        ///摄像头设备Cam的NewFrame事件处理程序
        ///用于实时显示捕获的视频流
        ///</summary>
        ///<paramname="sender"></param>
        ///<paramname="eventArgs"></param>
        private void Vcd_NewFrame(object sender, NewFrameEventArgs eventArgs)
        {
            Bitmap bmp = (Bitmap)eventArgs.Frame.Clone();
            bmp.RotateFlip(RotateFlipType.Rotate180FlipY);
            this.picBox.Image = bmp;
            //资源回收/不然会爆内存
            GC.Collect();
        }

        private void Window_Closed(object sender, EventArgs e)
        {
            if (vcd!=null&&vcd.IsRunning)
            {
                Dispatcher.InvokeShutdown();
                vcd.Stop();
            }
        }
    }
}
```

# 参考

- [AForge.NET 入门](https://blog.csdn.net/iwbfaith/article/details/54838427)
- [在WPF中使用AForge.net控制摄像头拍照](http://www.cnblogs.com/xhwang2003/archive/2013/02/07/AForge.html)


