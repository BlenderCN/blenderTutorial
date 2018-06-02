Blender 是一个开放的三维建模软件，支持python脚本扩展。而Sublime具有优秀的代码编辑功能，这二者的结合犹如干将莫邪，
这里做一个简单的介绍。这里所做的是通过socket将Blender和Sublime连接起来。

### 安装软件的插件插件

在blender中安装[SublimeBlenderAddon](https://github.com/supergis/SublimeBlenderAddon)插件。

在Sublime中安装[SublimeBlender](https://github.com/supergis/SublimeBlender)插件

#### 启动Blender中的TCP服务器

按ctrl+alt+u启动配置对话框，选中Addon，然后选中启用“SublimeBlenderAddon"插件。

在视窗中单击，再按空格键，在弹出框选择——输入”Sublime“，可显示出一个”SublimeBlender open connections“，选中运行，就运行起来了。

#### 启动Sublime中的TCP客户端

在Sublime中输入shift+command+p启动命令执行器，可以重启模块或者连接到Blender。如果不成功，需要重新启动sublime。

#### 编辑、运行Python脚本

在Sublime创建文件，保存为sublimetest.py,然后输入：

  import bpy

  bpy.ops.mesh.primitive_cube_add(radius=1, view_align=False, 
      enter_editmode=False, location=(2.02796, -0.0329399, 1.75504), 
      layers=(True, False, False, False, False, False, False, False, 
          False, False, False, False, False, False, False, False, 
          False, False, False, False))
          
按alt+p运行该脚本，将在Blender中创建一个几何对象。目前该版本效果已经很好，可以直接按“ctrl+空格键”弹出提示。
但还是比较容易中断，有感兴趣的可以fork该项目，进行完善，然后提交pull request回去。

[url](https://my.oschina.net/u/2306127/blog/372605)
