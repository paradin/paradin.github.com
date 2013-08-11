---
layout: post
title: Unity3D学习随笔（一）
date: 2013-07-14 22:33
comments: true
categories: Unity3D 
---

学习随笔将记录学习过程中的简单记录，不是教程。
<!-- more -->

项目创建
--
        。创建一个项目
        。提供资源Assets：Font、Image、Shader
        。根据相应资源创建Materials：选择Texture 2d、Shader，Font的Mat不知道怎么创建，虽然可以从Font中选择其下的用
        。创建一个场景
        。场景增加元素，每个元素由Component组成，自由增加删除，还可以增加Script（直接将脚本拖入Inspector)

项目资源
--
       。相机 Camera，决定视角，近景、远景
       。背景 Plane， 采用Plane-Z，就不用设置旋转了；选择Material：自发光还是自定义Shader；是否有碰撞检查；
       。文字 2d Text，Mesh Text 选择字体； Text Render 选择Material：字体材质；删除原来的GUIText
       。文字网格 Plane，采用Plane-Z；调整大小、位置；设置碰撞检测；无Render
       。脚本 Script，一般有 start、 update；public成员可以在编辑器中设置值；update按帧刷新，根据事件处理游戏表现


多分辨率
--
        。发现可以选择常见的aspect进行预览
        。这方便很多
        。有一点不爽就是，显示器dpi和手机dpi不一样，导致界面大小不一致
        
        对于多分辨率屏的支持，百搭方法：
        。相机固定
        。增加背景相机进行填充空白区域
        。设定目标aspect，根据屏幕aspect，计算相机rect
        。target_aspect < screem_aspect 则左右填充 pillarbox
        。target_aspect > screem_aspect 则上下填充 letterbox

嵌套关系
--
        。parent 通过父子关系建立依附的相对关系，子对象相对父对象：位置、旋转等
        。prefab 当在场景中编辑好了复杂的对象（集），希望能够重复创建相同的对象，可以先建立模型：从hieracky中拖入project中。创建好prefab就可以拖入场景，创建相同的对象。对对象的修改，可以应用到prefab，此时所有对象都会变化

动作编辑
--
        。animation clip 可以在动作编辑器重编辑动作曲线；各种component的属性都可以编辑曲线，包括脚本变量。编辑好曲线，在运行过程中，相应的参数就会采样曲线数据进行设置。  
                选择要编辑的曲线、添加key设置keyframe、设置切线角
                旋转类型：欧拉、四元数（找球面最短路径，所以超过180度的旋转方向相反）