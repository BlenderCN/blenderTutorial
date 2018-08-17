# 高级插件开发

本章讨论高级插件开发中的各种主题。我们在本章的结尾部分深入介绍了Blender最受欢迎的一些插件。

主题包括在Blender的文件系统开发，在Blender的文本编辑器外部开发，将您的插件组织为传统的Python模块，
高级面板组织，数据存储最佳实践以及向Blender提交插件。

## 在Blender文件系统开发

到目前为止，我们已经在Blender Text Editor中开发了脚本和插件。我们已经处理了一个繁琐的任务，即调整我们的插件，
使其在文本编辑器中工作，并作为插件独立工作。最终，手动修改代码以将其从开发部署到部署是一种不安全的做法。
我们希望确保开发中的代码与部署中的代码完全相同。

对于模仿部署环境的开发环境，我们必须直接在Blender的文件系统中进行开发。当我们引用Blender的文件系统时，
我们引用Blender根目录中的非静态应用程序文件。

首先，导航到Blender安装。对于Linux上的64位Blender2.78c，它被称为blender-2.78c-linux-glibc219-x86_64。
名称因操作系统而异，因此我们将在整个讨论过程中将此目录称为blender-2.78c。插件位于blender-2.78c/2.78/scripts/addons中。
在此文件夹中，我们可以看到所有当前安装的插件，包括Blender发行版附带的插件。
一些插件是 单个脚本，一些是单级目录，另一些是复杂的多级目录。

放置在此目录中的任何有效插件都将显示在Blender用户首选项中。因此，如果我们从头开始构建有效的插件，
我们可以在用户首选项中激活它，而无需打开Blender的文本编辑器。我们在第5章中讨论了插件的要求，
但我们没有将插件讨论为多级目录。有关各种类型插件的ASCII文件树，请参见清单7-1。

清单7-1。各种插件类型的文件树

    ### Single Scripts ###
    ### e.g. Node Wrangler ###
    node_wrangler.py
    
    ### Single-level or Flat Directories ###
    ### e.g. Mesh Inset ###
    mesh_inset/
    |--geom.py
    |--__init__.py
    |--model.py
    |--offset.py
    |--triquad.py
    
    ### Multi-level Directories ###
    ### e.g. Rigify ###
    rigify
    |--CREDITS
    |--gonerate.py
    |--__init__.py
    |--metarig_menu.py
    |--metarigs
    |   |--human.py
    |   |--__init__.py
    |   '--pitchipoy_human.py
    |--README
    |--rig_lists.py
    |--rigs
    |   |--basic
    |   |   |--copy_chain.py
    |   |   |--copy.py
    |   |   '--__init__.py
    |   |--biped
    |   |   |--arm
    |   |   |   |--deform.py
    |   |   |   |--fk.py
    |   |   |   |--ik.py
    |   |   |   '--__init__.py
    |   |   |--__init__.py
    |   |   |--leg
    |   |   |   |--deform.py
    |   |   |   |--fk.py
    |   |   |   |--ik.py
    |   |   |   '--__init__.py
    |   |   '--limb_common.py
    |   |--finger.py
    |   |--__init__.py
    |   |--misc
    |   |   |--delta.py
    |   |   '--__init__.py
    |   |--neck_short.py
    |   |--palm.py
    |   |--pitchipoy
    |   |   |--__init__.py
    |   |   |--limbs
    |   |   |   |--arm.py
    |   |   |   |--__init__.py
    |   |   |   |--leg.py
    |   |   |   |--limb_utils.py
    |   |   |   |--paw.py
    |   |   |   |--super_arm.py
    |   |   |   |--super_front_paw.py
    |   |   |   |--super_leg.py
    |   |   |   |--super_limb.py
    |   |   |   |--super_rear_paw.py
    |   |   |   '__ui.py
    |   |   |--simple_tentacle.py
    |   |   |--super_copy.py
    |   |   |--super_face.py
    |   |   |--super_finger.py
    |   |   |--super_palm.py
    |   |   |--super_torso_turbo.py
    |   |   |--super_widgets.py
    |   |   '--tentacle.py
    |   '--spine.py
    |--rig_ui_pitchipoy_template.py
    |--rig_ui_template.py
    |--ui.py
    '__uitls.py
    
正如我们在清单7-1中看到的那样，可以使用传统Python模块的结构以及单个脚本和平面目录来构建插件。
最适合插件的解决方案不仅取决于代码库的大小，还取决于其功能的复杂性。Rigify是插件的一个很好的例子，需要多个目录。
插件旨在装配(或准备动画)许多不同类型的网格。filetree显示腿部，手臂，触角，爪子等的自定义模块，
每个模块都组织成一个子模块，如两足动物或四肢动物，用于组织。    

### 在文件系统创建插件

在本练习中，我们需要一个除Blender之外的文本编辑器。建议读者打开他们喜欢的IDE或文本编辑器并创建一个新项目。
在Blender的插件文件夹中直接创建一个名为sandbox/的目录，如blender-2.78c/2.78/scripts/addons/sandbox/。
从那里，创建一个名为__init__.py的文件，其中包含清单7-2的内容。

清单7-2。文件系统插件的最小初始化文件

    bl_info = {
        "name" : "Add-on Sandbox",
        "author" : "Chris Conlan",
        "version" : (1,0,0),
        "blender" : (2,78,0),
        "location" : "View3D",
        "description" : "Within-filesystem Add-on Development Sandbox",
        "category" : "Development",
    }
    def register():
        pass
    
    def unregister():
        pass
        
    # Not required and will not be called,
    # but good for consistency
    if __name__ == '__main__':
        register()
        
保存此文件，然后打开Blender并导航到Header Menu>File>User Preferences>Add-ons并按“开发”过滤以查看我们的插件Sandbox。
结果应如图7-1所示。单击复选框以激活我们的插件，然后检查终端是否有错误。
没有消息是好消息，因为我们应该看到我们的空白插件实例化而没有错误。

图7-1

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/7-1.png?raw=true)

单击复选框后，查看blender-2.78c/2.7/scripts/addons/sandbox/。我们看到一个名为__pycache__的文件夹，以及以下文件树:
    
    sandbox
        |--__init__.py
        '--__pycache__
            '--__init__.cpython-35.pyc

