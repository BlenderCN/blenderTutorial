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

__pycache__文件夹是Python将已编译的.py文件存储为.pyc文件的位置。
考虑到Blender注册插件的方式，__pycache__目录中的*.pyc文件代表插件的内存中版本。
当我们单击用户首选项中的复选框时，Blender会确保磁盘上的Python源文件(例如，sandbox/__init__.py)没有更改。
如果它们已经改变，Python将重新编译相关的__pycache__目录，Blender将把编译好的Python加载到内存中。
因此，虽然它们不是严格相同的数据，但编译后的Python代表了插件的当前内存中版本。
这就是我们可以编辑Python源而不会实时影响插件的原因。

### 使用F8重新加载插件

现在我们正在编辑Blender文件系统中的插件源，我们可以重新编译插件以更新内存中的版本。
F8键将通过调用unregister()重新加载所有活动的插件，必要时重新编译.pyc文件，然后在已编译的.pyc文件上调用register()。
只需按F8即可重新加载所有活动的插件，而不仅仅是我们正在处理的插件。这对于复杂项目非常有用，
特别是那些依赖于其他插件的操作器和函数调用的项目。通常，使用此方法编辑插件是最佳做法。

当我们按F8时，我们应该看到来自旧内存插件的unregister()调用的终端输出，
然后是新内存插件的register()函数。如果插件已更新，Blender将在旧插件上运行unregister()后重新编译。
如果插件尚未更新，因此不需要重新编译，Blender仍将运行unregister()函数。

以下是此类操作的控制台输出。请注意，使用gc.collect()的最后一行是对Python垃圾收集器的调用。

    ### F8 after updating on disk... ###
    ### Other modules and add-ons...
    reloading addon: sandbox 1491112473.307823 1491116213.6005275 /blender-2.78c/2.78/scripts/addons/sandbox/__init__.py
    module changed on disk: /blender-2.78c/2.78/scripts/addons/sandbox/__init__.py reloading...
    Hello from Sandbox Registration!
    gc.collect() -> 19302
    
    ### F8 without updating on disk... ###
    Hello from Sandbox Unregistration!
    ### Other modules and add-ons...
    Hello from Sandbox Registration!
    gc.collect() -> 19302

### Important Takeaway

这似乎违反直觉，但开发Blender插件的最佳做法是完全避免使用Blender文本编辑器。
这引入了一些有关外部文本编辑器和IDE的逻辑问题，我们将在下面讨论。

### 管理导入

回顾第5章，清单5-5,XYZ-Select插件显示了一个示例插件，需要修改才能从Blender文本编辑器移动到插件。
清单7-3显示了在编辑文件系统时管理导入的正确方法。比方说，例如，
我们在一个平面目录中将__init__.py附近的ut.py放在一起。我们将导入它，如清单7-3所示。

清单7-3。在编辑文件系统时管理导入

    if "bpy" in locals():
        # Runs if add-ons are being reloaded with F8
        import importlib
        importlib.reload(ut)
        print("Reloaded ut.py")
    else:
        # Runs first time add-on is loaded
        from . import ut
        print('Imported ut.py')
        
    # bpy should be imported after this block of code
    import bpy
    
## 用于文件系统开发的IDE

在文件系统中进行开发从根本上改变了我们开发Blender Python脚本和插件的方式，
因为它消除了我们以前在Blender交互控制台和文本编辑器中喜欢的大部分可访问性和模块性。
尽管如此，文件系统是开发已发布插件的最佳方式，我们将调整我们的工具来帮助我们完成这项工作。

我们希望在Blender Python的IDE中使用的工具和功能:
    
    * 选项卡完成或自动完成，通常使用Ctrl+Space在交互式控制台中访问。
    
    * 使用bpy，bmesh，bgl等时不会创建错误标记或红色波浪线。
    
    * Python代码高亮显示，可能是特定于Blender的代码高亮显示，稍后我们将在几类选项中进行讨论。
    
### 轻量级(Notepad++,Gedit和vim)    

轻量级文本编辑器适用于简单的插件和脚本。通常，它们具有以下特征：
    
    * 支持Python的语法高亮显示。
    
    * 不会为Blender模块创建错误标签和红色波浪线。
    
    * 不支持项目管理和目录浏览
    
    * 没有内置的tab-completion
    
### 中量级(Sublime Text，Atom,和Spyder)

对于不想花太多时间配置IDE的程序员来说，中量级编辑器是一个很好的默认选择。通常，它们与轻量级IDE相同，但具有项目管理工具。它们具有以下特征:

    * 支持Python的语法高亮
    
    * 通常不会为Blender模块创建错误标签和红色波浪线
    
    * 具有内建项目管理和目录浏览
    
    * 通常没有为Python内置的的tab-completion
    
### 重量级(Eclipse PyDev,PyCharm和NetBeans)

重量级编辑对已经习惯的程序员有好处。他们可能需要一些配置才能与Blender Python插件很好地协作。配置选项并非始终可用。它们具有以下特征：

    * 支持Python的语法高亮
    
    * 通常会为Blender模块创建错误标签和红色波浪线
    
    * 具有内建项目管和目录浏览
    
    * 具有为Python内置的tab-completion
    
    * 没有为Blender Python内置的tab-completion
    
    * 可以配置为使用Blender Python干净利落地工作

Eclipse PyDev在开发人员社区中很受欢迎，开发人员经常会问如何配置它以使用Blender Python。
特别是Eclipse在Blender Python模块调用上创建错误标记非常麻烦。
已经进行了各种尝试来为其创建配置文件，但是它们没有被主动维护。

### 编译Blender作为Python模块

到目前为止，缺少tab-completion的最佳catch-all解决方案(适用于所有重量级IDE)是将Blender编译为Python模块。
当编译为Python模块时，IDE可以下降bpy等子模块以建议更正并启用tab-completion。我们在此不详细介绍此解决方案，
因为无法保证在不同的操作系统上运行。鼓励对此解决方案感兴趣的Linux用户进行研究。

将Blender编译为模块可以为开发过程的低级控制提供更多机会。我们鼓励能够将Blender编译为模块的用户在他的Blender插件GitHub存储库(https://github.com/sybrenstuvel/random-blender-addons)中查看Sybren Stüvel的PyCharm远程调试器插件。
他的插件作为PyCharm中的开发人员提供了低级调试控制。

### 概述

在作者看来，对于对IDE没有特别忠诚度的用户来说，中量级IDE是Blender Python开发的最佳解决方案。
许多开发人员都在努力将Blender Python与重量级IDE集成在一起。解决中量级IDE并不困难，请参阅交互式控制台和官方文档以获取API技巧。

## 外部数据的最佳实践

我们在这里轮流分析一些流行的插件，并批评它们如何处理外部数据。我们将通过Blender Python开发人员社区的示例讨论如何最好地提供外部数据。

在第四章中，我们讨论了可以定义3D网格的各种方法。我们讨论中最值得注意的是.obj文件格式在不同软件之间传输网格，
法线和纹理数据时的可扩展性和简洁性。

Blender Python插件通常依赖于预定义的数据。例如，BlenderAid的Asset Flinger允许用户轻松地将资源从预定义列表生成到场景中。
我们讨论Asset Flinger和其他插件将数据导入Blender的方式。

### 使用文件交换格式

Asset Flinger插件通过.obj文件将网格导入Blender。如果我们筛选插件的assets/目录，
我们会看到几十个.obj文件及其截图。使用像.obj这样的交换格式是将外部数据导入Blender Python的好方法，
因为它是3D模型和3D艺术家的标准。

此插件允许用户通过添加自己的.obj文件来扩展它。使用交换格式是使用清晰的Python代码构建可扩展插件的最佳实践。
清单7-4中的函数是将.obj文件导入Blender场景所需的全部内容。

清单7-4。将obj文件导入到场景

    bpy.ops.import_scene.obj(filepath = myAbsolutefilepath)
    
正如我们将要看到的，导入数据的其他方法可能会混乱您的Python代码，并使其他开发人员难以进行协作。

### 使用硬编码的Python变量

正如我们在第4章讨论的那样，无论使用哪种文件格式，3D网格都需要最少的信息集来完全指定它。
一些开发人员将这些知识硬编码网格用作代码中的Python变量。

Antonio Vazquez(antonioya)的Archimesh插件允许用户使用自定义用户界面创建和编辑建筑物网格物体，
如墙壁，窗户和门。他没有以文件交换格式从外部保存这些门和窗口，而是将这些网格物体硬编码为元组列表。
有关此示例，请参阅https://github.com/Antonioya/blender/blob/master/archimesh/src/ 上的ArchimeshGitHub Repo。
此repo中许多Python文件的尾端包含由浮点数和整数表示的顶点和面数据元组的硬编码列表。

这种设计选择并非没有动机或后果。为了创建具有任意数量的墙和具有任意数量窗格的窗口的房间，这些Python变量以复杂的方式被复制，
子集化和变换多次。结果，这些物体不能容易地彼此替代。它们专门用于处理插件中列出的算法。

这里的核心API调用是bpy.data.meshes.new()和my_mesh_object.from_pydata().插件创建一个空白网格，
操纵大量Python数据以形成对象，然后使用网格上的from_pydata()函数实例化网格。有关此加载项如何操作的最小示例，请参见清单7-5。
清单7-5的底部显示了使用bpy.ops.object.add()的替代方法。

清单7-5。使用from_pydata()创建网格。

    # Adapted from antonio Vazquez's Archimesh
    import bpy
    
    # Clear scene
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Manipulate Python lists of vertex and face data...
    # Sample here creates a triangular pyramid
    myvertex = [(0.0,0.0,0.0),(1.0,0.0,0.0),(0.0,1.0,0.0),(0.0,0.0,1.0)]
    myfaces = [(1,2,3),(1,2,4),(1,3,4),(2,3,4)]
    
    ############################################
    
    # Option #1 -bpy.ops.object.add()
    bpy.ops.object.add(type = 'MESH')
    mainobject = bpy.context.object
    mainmesh = mainobject.data
    mainmesh.name = 'WindowMesh'
    mainobject.name = 'WindowObject'
    
    # Write the Python data to the mesh and update it 
    mainmesh.from_pydata(myvertex,[],myfaces)
    mainmesh.update(calc_deges=True)
    
    ###############################################
    
    # WARNING:Known to cause crashes and segmentation faults in
    # certain opetating systems. Linux builds are safe.
    # Option #2 —— bpy.data.meshes.new()
    mainmesh  = bpy.data.meshes.new("WindowMesh")
    mainobject = bpy.data.objects.new("WindowObject",mainmesh)
    
    # Link the object to the scene,activate it,and select it
    bpy.context.scene.objects.link(mainobject)
    bpy.context.scene.obeject.active = mainobject
    mainobject.select = True
    
    # Write the Python data to the mesh and update it
    mainmesh.from_pydata(myvertex,[],myfaces)
    mainmesh.update(calc_edges = True)
    
    ################################################
    
通过阅读Archimesh源代码，我们可以看到哦如清单7-5中的一个简单示例如何演变成能够在程序上生成架构模型的东西。
硬编码大量数据可能不是程序生成的最Pythonic方法，但它在Archimesh中得到了很好的利用。可以认为硬编码是不必要的，
并且数据可以容易地存储在外部文件中，同时仍然允许使用from_pydata()。
    
### 基元的算法操作。

将网格数据引入Blender的最终方法是对基元进行算法处理。在这种情况下，基元默认引用3D Viewport Header>add中的对象。
例如，可以通过算法调用平面上的编辑模式操作，将它们转换为窗口的详细模型。通过不断细分，平移和挤压平面，我们可以得到一个复杂的窗口模型。
当我们这样做时，算法成为网格的描述符，i并且可以修改它以创建网格的不同变体。

当我们编写算法过程来创建网格时，它们几乎是自然模块化的。例如，如果我们创建了一个算法来构建一个宽度为6英寸的20个柱子的围栏，
它自然会延伸到一个算法，该算法可以构建具有宽度为w的n个柱子的围栏。

有关算法生成迷宫的示例，请参见清单7-6.我们可以调整maze_size,maze_height,fp和buf来改变迷宫的构建方式。
脚本中有许多要点我们可以i自定义以进一步改变迷宫的生成方式。这就是程序生成的本质。参数化很自然。
有关输出的示例，请参见图7-2。请注意，这需要http://blender.chrisconlan.com/ut.py 上提供的ut.py模块。

清单7-2。平面，随机迷宫的算法操作。

    import bpy 
    import ut
    import random
    
    # Clear scene,must be in object mode
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # size of maze
    maze_height = 1.0
    
    # Create NxN plane
    bpy.ops.mesh.primitive_plane_add(radius = maze_size/2,location=(0,0,0.1))
    
    # Subdivide and deselect mesh
    bpy.ops.object.mode_set(mode = 'EDIT')
    bpy.ops.mesh.subdivide(number_cuts = maze_size -1)
    bpy.ops.mesh.select_all(action = 'DESELECT')
    
    # Set starting point
    v = [-maze_size / 2,-maze_size / 2]
    
    # Stop iterating if point strays buf away from plane
    buf = 5
    b = [-maze_size / 2 - buf, maze_size /2 + buf ]
    
    # Probability of point moving forward
    fp = 0.6
    
    while b[0] <= v[0] <= b[1] and b[0] <= v[1] <= b[1]:
        
        # Select square in front of v
        ut.act.select_by_loc(lbound = (v[0] - 0.5, v[1] - 0.5, 0),
                             ubound = (v[0] + 1.5, v[1] + 1.5, 0),
                             select_mode = 'FACE',
                             coords = 'GLOBAL',
                             additive = True)

        # Returns 0 or 1
        ind = random.randint(0,1)
        
        # Return -1 or 1 with probability 1 -fp or fp
        dir = (int(random.random() > 1 - fp)*2) -1
        
        # Adjusts point
        v[ind] += dir
        
    bpy.ops.mesh.select_all(action = 'INVERT')
    bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate = {"value" : (0,0,maze_height),
                                                               "constraint_axis" : (False,False,True)}
                                     )
    bpy.ops.object.mode_set(mode = 'OBJECT')                                     

图7-2

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/7-2.png?raw=true)

清单7-6使用随机性和算法操作来生成对象。还应注意，算法操纵通常用于生成确定性对象。

### 概述

作为最佳实践，作者认为应该避免使用硬编码Python变量来支持其他两种方法：外部交换文件和算法操作。
应该避免硬编码，主要是因为外部交换文件是它的优越替代品。通过读取交换文件并将其数据保存在Python变量中，
可以实现硬编码的所有好处。

作为一个实际问题，作者认为作者认为应该使用外部交换文件来代替不需要大量参数化的算法操作。
事实上，任何一个对象都可以用任何一种方法获得，但在参数化是第二个想法的情况下，算法操作可能变得过于复杂(没有好处)。
例如，如果我们需要一个非常详细的窗口(1000个顶点)并且我们想要参数化的唯一事情是它的大小，
那么通过算法生成这个窗口将很难使用开发时间。这里首选的方法是从外部交换文件加载窗口并使用Blender工具调整大小。

相反，很容易识别外部交换文件何时不够。如果插件的原始目标是参数化网格，那么选择算法操作几乎总是最好的。

## 高级面板创建

我们在本章结束时讨论了高级面板创建。bpy.types.Panel类有一些有用的类方法，用于组织面板上的按钮。
在本讨论中，我们使用第五章中的插件模板。
可以在http://blender.chrisconlan.com/addon_template.py 下载用于此讨论的版本。

为了解释高级面板自定义，我们使用模板中已注册的属性和运算符。换句话说，我们完全专注于SimplePanel类draw()函数。

### 面板组织

我们已经讨论过如何调用operator()和prop()来分别i向画布添加按钮和特定于类型的GUI元素。
根据我们到目前为止所介绍的内容，读者只能在其面板中创建垂直堆叠的按钮和属性列表。
清单7-7显示了如何使用组织功能来自定义面板。结果见图7-3。

清单7-7。组织面板

    # Simple button in Tools panel
    class SimplePanel(bpy.types.Panel)
        bl_space_type = "VIEW_3D"
        bl_region_type = "TOOLS"
        bl_category = "Simple Addon"
        bl_label = "Call Simple Operator"
        
        def draw(self,context):
            # Store reference to context.scene
            scn = context.scene
            
            # Store reference ot self.layout
            lay = self.layout
            
            # Create box
            box = lay.box()
            box.operator("object.simple_operator",text= "Print #1")
            box.prop(scn,'encouraging_message')
            
            # Create another box
            box = lay.box()
            # Create a row within it
            row = box.row()
            # We can jam a few things on the same raw
            row.operator("object.simple_operator",text="Print #2")
            row.prop(scn,'encouraging_message')
            
            # Create yet another box
            box = lay.box()
            # Create a row just for a label
            row = box.row()
            row.label('There is a split row below me!')
            # Create a split row within it
            row = box.row()
            splitrow = row.split(percentage=0.2)
            # Store references to each column of the split row
            left_col = splitrow.column()
            right_col = splitrow.column()
            left_col.operator("object.simple_operator",text = "Print #3")
            right_col.prop(scn,'encouraging_message')
            
            # Throw a separator in for white space...
            lay.separator()
            
            # We can create columns within rows
            row = lay.row()
            col = row.column()
            col.prop(scn,'my_int_porp')
            col.prop(scn,'my_int_prop')
            col.prop(scn,'my_int_prop')
            col = row.column()
            col.prop(scn,'my_float_prop')
            col.label("I'm in the middle of a column")
            col.prop(scn,'my_float_prop')
            
            # Throw a  few separators in ...
            lay.separator()
            lay.separator()
            lay.separator()
            
            # Same as above but with boxes...
            row = lay.row()
            box = row.box()
            box.prop(scn,'my_int_prop')
            box.label("I'm in the box,bottom left.")
            box = row.box()
            box.prop(scn,'my_bool_prop')
            box.operator("object.simple_operator",text = "Print #4")
            
bpy.types.Panel的核心组织函数是box(),row(),column(),separator()和label()。
这五个函数中的每一个都可以嵌套在box(),row()或column()中，以实现更精细的组织。总的来说，
这是一个非常直观的GUI开发工具包。它可以轻松构建美观的GUI。

图7-3。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/7-3.png?raw=true)

### 面板图标

环顾Blender GUI，我们注意到按钮左侧有许多不同的图标。Blender内置了超过550个图标，我们可以在自己的按钮旁边使用它们。
按钮由字符串表示，我们将通过icon=argument传递给prop()函数。在撰写本文时，
对可用图标的最全面的参考与Blender一起打包的图标插件。激活它后，在Blender文本编辑器中按Ctrl + F以查看属性面板，
它将位于底部。清单7-8显示了我们如何在操作符旁边的面板中绘制图标。结果见图7-4。

清单7-8。面板图标。

    class SimplePanel(bpy.types.Panel):
        bl_space_type = "VIEW_3D"
        bl_region_type = "TOOLS"
        bl_category = "Simple Addon"
        bl_label = "Call Simple Operator"
        
        def draw(self,context):
            # Store regerence to context.scene
            scn = context.scene
            
            # Store reference to self.layout
            lay = self.layout
            
            # Create a raw within it
            row = lay.row()
            row.operator("object.simple_operator", text = "#1",icon = "OBJECT_DATA')
            row.operator("object.simple_operator", text = "#2",icon = "WORLD_DATA")
            row.operator("object.simple_operator", text = "#3",icon = "LAMP_DATA")
            
            row = lay.row()
            row.operator("object.simple_operator", text = "#4",icon = "SOUND")
            row.operator("object.simple_operator", text = "#5",icon = "MATERIAL")
            rwo.operator("object.simple_operator", text = "#6",icon = "ERROR")
            
            row = lay.row()
            row.operator("object.simple_operator", text = "#7",icon = "CANCEL")
            row.operator("object.simple_operator", text = "#8",icon = "PLUS")
            row.operator("object.simple_opetator", text = "#9",icon = "LOCKED")
            
            row = lay.row()
            row.operator("object.simple_operator", text = "#10",icon = "HAND")
            row.operator("object.simple_operator", text = "#11",icon = "QUIT")
            row.operator("object.simple_operator", text = "#12",icon = "GAME")
            
            row = lay.row
            row.operator("object.simple_operator", text = "#13",icon = "PARTICLEMODE")
            row.operator("object.simple_operator", text = "#14",icon = "MESH_MONKEY")
            row.operator("object.simple_operator", text = "#15",icon = "FONT_DATA")
            
            row = lay.row()
            row.operator("object.simple_operator", text = "#16",icon = "SURFACE_NSPHERE")
            row.operator("object.simple_operaotr", text = "#17",icon = "COLOR_RED")
            row.operator("object.simple_operator", text = "#18",icon = "FORCE_LENNARDJONES")
            
            row = lay.row()
            row.operator("object.simple_operator", text = "#19",icon = "MODIFIER")
            row.operator("object.simple_operator", text = "#20",icon = "MOD_SOFT")
            row.operator("object.simple_operator", text = "#21",icon = "MOD_DISPLACE")
            
            row = lay.row()
            row.operator("object.simple_operator", text = "#22",icon = "IPO_CONSTANT")
            row.operator("object.simple_operator", text = "#23",icon = "GRID")
            row.operator("ojbect.simple_operator", text = "#24",icon = "FILTER")
                        
图7-4

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/7-4.png?raw=true)

## 结论

这样就结束了我们对高级插件的讨论。本指南并不全面，因为插件开发方面有很多地方和可能性可供探索。
重要的是要记住，Blender GUI本身是基于我们一直在讨论的Python类，因此我们看到的每个功能都可以被复制。

本章中有关插件组织的背景知识应该让读者更容易理解其他开发人员的源代码。Blender是一个开源平台，
鼓励用户共享代码并相互学习。鼓励读者复制和修改其他开发人员的工作，然后分享他们的工作供他人学习。

下一章以纹理和渲染的处理结束本文。
