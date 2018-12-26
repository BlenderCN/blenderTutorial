## Setup

Armory包里有你需要的一切。

*   [Download SDK](http://armory3d.org/download.html)

*   将Armory_version.zip解压缩到你的首选位置。（在Windows上，更偏向于像C：\Dev这样的短路径或使用7-zip解压缩以防止长路径错误并加快提取速度。）

*   运行位于解压缩SDK中的Blender。（在Window上，你可能需要首次启用它，方法是单击More info——Run anyway。在linux上，在解压缩的文件夹中打开终端并键入./blender。在macOS上，请参阅instructions.txt文件。）

![](https://armory3d.org/manual/getting_started/img/winrun.png)

*   要验证一切正常，请保存.blend文件，切换到Cycles Render或Eevee引擎（使用位于信息标题中的下拉菜单），然后点击位于Properties-Render-Armory Player面板中的Play（F5）按钮。

*   继续到[Playground tutorial](https://github.com/BlenderCN/blenderTutorial/blob/master/armory_docs/Playground.md)学习更多。

## Troubleshooting

*   对于旧图形卡，请在Blender-Properties-Render-Armory Render Path中选择Low预设。

*   在命名时，更偏向于不重音的字母A-Z。

*   如果你希望为捆绑的Blender隔离配置，请在Armory/2.80中创建空config文件夹。

*   如果你在设置方面遇到任何问题，[let us know](http://armory3d.org/community.html)！

## Window

*   要查看错误消息，请从控制台启动Blender或按Blender-Window-Toggle System Console。

*   未找到类型：主要错误-将.blend保存在C：\Users\user_name\Documents\test等安全路径中，否则Windows可能会阻止写入文件。

*   如果报丢失.dll错误，请尝试安装[Visual C++ Redistributable for Visual Studio 2017](https://go.microsoft.com/fwlink/?LinkId=746572)

## Linux

*   要查看错误信息，请使用./blender从终端启动Blender。

## macOS

*   要查看错误信息，请使用/path/to/Armory/blender.app/Contents/MacOS/blender 从终端启动Blender。

*   由于macOS的安全元素，你可能会收到file corrupted的错误。尝试从终端启动Blender，如上所述。

## Using Armory as add-on

如果你不需要内置player，Armory只能在标准Blender安装中用作插件。然后player将在单独的窗口中启动。

*   [下载](http://armory3d.org/download.html)并解压缩适用于你平台的SDK。

*   在Blender中，选择File-User Preferences...并导航到Add-ons选项卡。

*   单击Install from File...

*   选择位于my_unpacked_sdk/Armory/armsdk/armory/blender/addon中的armory.py（在MacOS上，位置为my_unpacked_sdk/Armory/Blender.app/armsdk/armory/blender/addon ）。

*   Enable Armory插件。

*   将SDK Path属性设置为my_unpacked_sdk/Armory/armsdk。

*   点击底部的Save User Settings并重新启动Blender。That's it！


<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/armory_docs/README.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/armory_docs/Playground.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>
