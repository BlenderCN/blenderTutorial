# bpy模块

本章介绍和详细介绍了bpy模块的主要组件。在这样做时，我们解释了Blender的许多重要行为。我们涵盖选择和激活，创建和删除，场景管理和代码抽象。

可以通过在http://www.blender.org/api/上选择Blender版本来找到Blender Python API的官方文档。我们在本文中使用了Blender2.78c，
因此我们的文档可以在http://www.blender.org/api/blender_python_api_2_78c_release/找到。

## 模块概述

我们首先给出bpy每个子模块的一些背景知识。

### bpy.ops

如上所述，该子模块包含运算符。这些主要是用于操作对象的函数，类似于Blender艺术家在默认界面中操纵对象的方式。
子模块还可以操纵3D视窗，渲染，文本等等。

对于操作3D对象，两个最重要的类是bpy.ops.object和bpy.ops.mesh。对象类包含用于同时操作多个所选对象的函数以及许多常规实用程序。
网格类包含用于一次一个地操纵对象的顶点，边和面的函数，通常在编辑模式下。

bpy.ops子模块中目前有71个类，所有类都名称相当且组织良好

### bpy.context

bpy.context子模块用于通过各种状态标准访问Blender的对象和区域。
该子模块的主要功能是为Python开发人员提供一种访问用户正在使用的当前数据的方法。如果我们创建一个按钮来置换所有选定的对象，
我们可以允许用户选择他选择的对象，然后在bpy.context.select_objects中置换所有对象。

我们在构建插件时经常使用bpy.context.scene,因为它是某些Blender对象的必需输入。我们还可以使用bpy.context访问活动对象，
在对象模式和编辑模式之间切换，并从油脂笔接受数据。

### bpy.data

该子模块用于访问Blender的内部数据。解释这个特定模块(*/bpy.data.html页面直接指向一个单独的类)的文档可能很困难，
但在本文中我们将非常依赖它。bpy.data.objects类包含确定对象形状和位置的所有数据。
当我们说前面的子模块bpy.context非常适合将我们指向对象组时，我们的意思是bpy.context类将生成对bpy.data类的数据块的引用。

### bpy.app

这个子模块并没有完整记录，但是我们对目前所信的信息可以在脚本和插件开发中发挥很大的作用。
子模块bpy.app.handlers是我们在本文中唯一关注的问题。处理程序子模块包含用于触发自定义函数以响应Blender中的事件的特殊函数。
最常用的是帧更改句柄，每次更新3D视窗时(即，在帧更改后)执行某些函数。

### bpy.types,bpy.utils和bpy.props

这些模块将在后面的插件开发章节中详细讨论。读者目前可能会发现*/bpy.types.html中的文档对于描述我们在其他地方使用的对象类非常有用。

### bpy.path

此子模块与本机随Python一起提供的os.path子模块基本相同。它对于核心开发团队之外的Blender Python开发人员来说很少有用。

## 选择，激活和规范

Blender界面设计直观，同时还提供复杂功能。某些操作在逻辑上适用于单个对象，而其他操作可以在逻辑上同时用于一个或多个对象。
为了处理这些场景，Blender开发人员创建了三种访问对象及其数据的方法

1.选择：可以一次选择一个，多个或零个对象。使用所选对象的操作可以在单个对象或多个对象上同行执行该操作。

2.激活：在任何给定时间只能激活一个对象。在活动对象上工作的操作通常更具体和更激烈，因此不能一次性地在许多事物上直观地执行。

3.规范：(仅限Python)Python脚本可以通过名称访问对象并直接写入其数据块。虽然操作选定对象的操作通常是平移，旋转或缩放等差异操作，
但将数据写入特定对象通常是声明性操作，如位置，方向或大小。

### 选择对象

在继续之前，建议读者在3D视窗中创建一些不同的对象作为示例。转到3D Viewport>Add 以查看对象的创建菜单。

当我们右键单击3D视窗中的对象时，对象会高亮显示并取消高亮显示。当我们按住Shift键并单击时，我们可以一次突出显示多个对象。
3D视窗中的这些高亮显示所选对象。要列出所选对象，请在清单2-1中键入交互控制台中的代码。

清单2-1。获取所选对象列表

    # Outputs bpy.data.objects datablocks
    bpy.context.selected_objects

正如我们我们前面提到的，bpy.context子模块非常适合根据Blender中的状态获取对象列表。在这种情况下，我们获取了所有选定的对象。

    # Example output of Listing 2-1,list of bpy.data.objects datablocks
    [bpy.data.objects['Sphere'],bpy.data.objects['Circle'],bpy.data.objects['Cube']]

在这种情况下，在3D视窗中选择了一个名为Sphere的球体，一个名为Circle的圆圈和一个名为Cube的立方体。
我们返回了一个bpy.data.objects数据块的Python列表。鉴于所有这种类型的数据块都具有名称值的知识，
我们可以遍历清单2-1的结果来访问所选对象的名称。请参阅清单2-2,我们在其中获取所选对象的名称和位置。

清单2-2。获取所选对象列表

    # Return the names of selected objects
    [k.name for k in bpy.context.selected_objects]
    
    # Return the locations of selected objects
    # (location of origin assuming no pending transformations)
    [k.location for k in bpy.context.selected_objects]
   
现在我们知道如何手动选择对象 ，我们需要根据某些条件自动选择对象。必需的函数在bpy.ops中。清单2-3创建了一个函数，
该函数将对象名称作为参数并选择它，默认情况下清除所有其他选择。如果用户指定additive=True，则该函数不会事先清除其他选择。

清单2-3。以编程方式选择对象
    
    import bpy
    
    def mySelector(objName,additive=False):
        
        # By default,clear other selections
        if not additive:
            bpy.ops.object.select_all(action='DESELECT')
            
        # set the 'select' property of the datablock ot True
        bpy.data.objects[objName].select = True
    
    # select only 'Cube'
    mySelector('Cube')
    
    # Select 'Sphere',keeping other selections
    mySelector('Sphere,additive=True)
    
    # Translate selected objects 1 unit along the x-axis
    bpy.ops.transform.translate(value=(1,0,0))

_____
Note: 要在没有Python脚本的情况下轻松查看对象的名称，请导航到“属性"窗口并选择橙色立方体图标。现在，
活动对象将在此子窗口顶部附近显示其名称，如图2-1中的情况。此外3D视窗的左下角将显示活动对象的名称。
我们将在本章的下一小节中讨论激活。
_____

图2-1

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/2-1.png?raw=true)

### 激活对象

激活，如选择是Blender中的对象状态。与选择不同，在任何给定时间只能有一个对象处于活动状态。此状态通常用于单个对象的顶点，边和面操作。
此状态还与编辑模式有密切关系，我们将在本章节后面详细讨论。

当我们在3D视窗周围单击鼠标左键时，我们单击的任何对象都将高亮显示。当我们以这种方式高亮显示单个对象时，
Blender会选择并激活该对象。如果我们按住Shift并在3D视窗周围单击鼠标左键，则只有我们单击的第一个对象才会处于活动状态。

请注意图2-1中所示的属性窗口区域，其中显示活动对象的名称。也可以通过图2-1底部的菜单激活对象。

要在Python中访问活动对象，请在交互式控制台中键入清单2-4.请注意，有两个等效的bpy.context类用于访问活动对象。
就像选择的对象一样，我们返回一个bpy.data.objects数据块，我们可以直接操作。

清单2-4。访问活动对象

    # Returns bpy.data.objects datablock
    bpy.context.object
    
    # Longers synonym for above line
    bpy.context.active_objects
    
    # Accessing the 'name' and 'loation' values of the datablock
    bpy.context.object.name
    bpy.context.object.location
    
 清单2-5与清单2-3类似，用于激活。由于在任何给定时间只能有一个对象处于活动状态，因此激活功能要简单得多。
 我们将bpy.data.objects数据块传递给一个场景属性，该场景属性处理激活时的内部数据。因为Blender只允许单个对象处于活动状态，
 所以我们可以对bpy.context.scene进行单个赋值，并允许Blender的内部引擎对其他对象的停用进行排序。
 
 清单2-5。以编程方式激活对象
 
    import bpy
    
    def myActivator(objName):
        
        # Pass bpy.data.objects datablock to scene class
        bpy.context.scene.objects.active = bpy.data.objects[objName]
        
    # Activate the object named 'Sphere'
    myActivator('Sphere')
    
    # Verify the 'Sphere' was activated
    print("Active object:",bpy.context.object.name)
    
    # Selected objects were unaffected
    print("Selected objects:",bpy.context.selected.objects)
 
### 指定对象(按名称访问)

本节详细介绍如何通过指定对象的名称来返回bpy.data.objects数据块。清单2-6显示了如何为给定名称的对象访问bpy.data.objects数据块。
根据我们到目前为止的讨论，清单2-6可能看起来微不足道。数据块引用的这种循环性质具有非常重要的目的。

清单2-6。按规范访问对象

    # bpy.data.objects datablock for an object named 'Cube'
    bpy.data.objects['Cube']
    
    # bpy.data.objects datablock for  an object named 'eyeballSpere'
    bpy.data.objects['eyeballSphere']
    
清单2-7与清单2-3和2-5类似，但适用于规范。mySelector()和myActivator()的目的是返回具有给定状态的对象的数据块或数据块。
在这种情况下，mySpecifier()通常会返回数据块。
 
清单2-7。按规范以编程方式访问对象
 
    import bpy
    
    def mySpecifier(objName):
        # Return the datablock
        return bpy.data.objects[objName]
 
    # Store a reference to the datablock
    myCube = mySpecifier('Cube')
    
    # Output the location of the origin
    print(myCube.location)
    
    # Works exactly the same as above
    myCube = bpy.data.objects['Cube']
    print(myCube.location)
    
## 伪循环引用和抽象

bpy.data.objects数据块有一个非常有趣的属性，突出了为Blender Python API做出的许多明智的架构决策。为了促进模块化，
可扩展性和自由抽象，bpy.data.objects数据块被构建为无限嵌套。我们将此称为伪循环引用，因为虽然引用是循环的，
但它们发生在对象内而不是对象之间，使得概念与循环引用不同。

有关伪循环引用的数据块的简单示例，请参见清单2-8。

清单2-8。伪循环引用

    # Each line will return the same object type and memory address
    bpy.data
    bpy.data.objects.data
    bpy.data.objects.data.objects.data
    bpy.data.objects.data.objects.data.objects.data

    # References to the same object can be made across datablock types
    bpy.data.meshs.data
    bpy.data.meshs.data.objects.data
    bpy.data.meshs.data.objects.data.scenes.data.worlds.data.materials.data
    
    # Different types of datablocks also nest
    # Each of these lines returns the bpy.data.meshs datablock for 'Cube'
    bpy.data.meshes['Cube']
    bpy.data.objects['Cube'].data
    bpy.data.objects['Cube'].data.vertices.data
    bpy.data.objects['Cube'].data.vertices.data.edges.data.materials.data
    
清单2-8展示了Blender Python API的强大功能。当我们将.data附加到对象时，它返回对父数据块的引用。此行为有一些限制。
例如，我们不能附加.data.data来从bpy.data.meshes[]数据块移动到bpy.data数据块。
但是，这种行为将帮助我们构建自然模块化的清晰可读的代码库。

我们将在文本中创建工具，使我们能够在Blender中构建和操作对象，而无需直接调用bpy模块。
虽然我们在清单2-8中提到伪循环引用似乎是微不足道的，但读者会发现它在抽象bpy模块时经常隐含在工具包中。

# 用bpy转换

本节讨论bpy.ops.transorm类的主要组件及其他地方的类似物。它自然地扩展了抽象主题，并介绍了一些有用的Blender Python技巧。

清单2-9是用于创建，选择和转换对象的最小工具集。脚本的底部运行一些示例转换。图2-2显示了3D视窗中最小工具包的测试运行输出。

清单2-9。创建和转换的最小工具包(ut.py)。

    import bpy
    
    # Selecting  objects by name
    def select(objName):
        bpy.ops.object.select_all(action='DESELECT')
        bpy.data.objects[objName].select = True
        
    # Activating objects by name
    def activate(objName):
        bpy.context.scene.objects.active = bpy.data.objects[objName]
        
    class sel:
        """Function Class for operating on SELECTED objects"""
        
        # Differential
        def translate(v):
            bpy.ops.transform.translate(value=v,constraint_axis=(True,True,True))
        
        # Differential
        def scale(v):
            bpy.ops.transform.resize(value=v,constraint_axis=(True,True,True))
            
        # Differential
        def rotate_x(v):
            bpy.ops.transform.rotate(value,axis=(1,0,0))
            
        # Differential
        def rotate_y(v):
            bpy.ops.transform.rotate(value=v,axis=(0,1,0))
            
        # Differential
        def rotate_z(v):
            bpy.ops.transform.rotate(value=v,axis=(0,0,1))
            
    class act:
        """Function Class for operating on ACTIVE objects"""
        
        # Declarative
        def location(v):
            bpy.context.object.location = v
            
        # Declarative
        def scale(v):
            bpy.context.object.scale = v
            
        # Declarative
        def rotation(v):
            bpy.context.object.rotation_euler = v
            
        # Rename the active object
        def rename(objName):
            bpy.context.object.name = objName
            
    class spec:
        """Function Class for operating on SPECIFIED objects"""
        
        # Declarative
        def scale(objName,v):
            bpy.data.objects[objName].scale = v
            
        # Declarative
        def location(objName,v):
            bpy.data.objects[objName].location = v
            
        # Declarative
        def rotation(objName,v):
            bpy.data.objects[objName].rotation_euler = v
            
    class create:
        """Function Class for CREATING Objects"""
        
        def cube(objName):
            bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(0,0,0))
            act.rename(objName)
            
        def sphere(objName):
            bpy.ops.mesh.primitive_uv_sphere_add(size=0.5,location=(0,0,0))
            act.rename(objName)
            
        def cone(objName):
            bpy.ops.mesh.primitive_cone_add(radius1=0.5,location=(0,0,0))
            act.rename(objName)
        
        # Delete an object by name        
        def delete(objName):
        
            select(objName)
            bpy.ops.object.delete(use_global=False)
            
        # Delete all objects
        def delete_all():
            
            if(len(bpy.data.objects) != 0):
                bpy.ops.object.select_all(action='SELECT')
                bpy.ops.object.delete(use_global=False)
                
    if __name__ == "__main__":
        
        # Create a cube
        create.cube('PerfectCube')
        
        # Differential transformations combine
        sel.translate((0,1,2))
        
        sel.scale((1,1,2))
        sel.scale((0.5,1,1))
        
        sel.rotate_x(3.1415/8)
        sel.rotate_x(3.1415/7)
        
        sel.rotate_z(3.1415/3)
        
        # Create a cone
        create.cone('PointyCone')
            
        # Declarative transformations overwrite
        spec.location('SmoothSphere',(2,0,0))
        act.rotation((0,0，3.1415/3))
        act.scale((1,3,1))

图2-2

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/2-2.png?raw=true) 

注意注释标签差异和声明。在Blender Python中有很多方法可以旋转，缩放和转换对象，
但重要的是要记住哪些函数指示一个表单(声明性)以及哪些函数修改表单(差异性)。值得庆幸的是，bpy函数和类值的含义相当直观。
例如，rotate是动词，因此是差异性的，而旋转是名词，因此是声明性的。

清单2-9,我们将调用ut.py,是自定义实用程序类的一个很好的起点。

在本书中，我们感兴趣的是教授Blender Python API，而不是作者的ut.py模块。虽然ut.py模块是一个很好的参考和教学工具，
但我们将在以后的章节中避免使用其单行函数调用。虽然这些函数调用可以在短期内解决我们的问题，
但它们模糊了我们希望通过重复强化的类结构和参数。

现在，我们将使用ut.py进行一些很酷的可视化。在以后的章节中，我们将为其添加笨重且有意义的实用功能，同时将单行功能视为占位符。

## 使用最小化工具箱可视化多变量数据

在本节中，我们使用清单2-9中的工具包可视化多变量数据。在开始之前，使用文本编辑器底部的栏为此工具箱提供ut.py的Python文件名。
现在，单击文本编辑器底部的加号以创建新脚本。文件ut.py现在是Blender Python环境中的链接脚本，我们可以将其导入环境的其他脚本。

我们将可视化著名的Fisher‘s Iris数据集。该数据集有五列数据。前四列是描述花的尺寸的数值，最后一列是描述花的类型的分类值。
该数据集中有三种类型的花：setosa，versicolor和virginica。

清单2-10用作此示例的标头代码。它导入必要的模块：我们的工具包ut,csv模块和urllib.request。我们将使用urllib从文件存储库中获取数据，
然后使用csv解析它。没有必要理解清单2-10中的所有代码来从这个例子获益。

清单2-10。在iris.csv中阅读练习

    import ut
    import csv
    import urllib.request
    
    ###################
    # Reading in Data #
    ###################
    
    # Read iris.csv from file repository
    url_str = 'http://blender.chrisconlan.com/iris.csv'
    iris_csv = urllib.request.urlopen(url_str)
    iris_ob = csv.reader(iris_csv.read().decode('utf-8').splitlines())
    
    # Store header as list,and data as list of lists
    iris_header = []
    iris_data = []
    
    for v in iris_ob:
        if not iris_header:
            iris_header = v
        else:
            v = [
                float(v[0]),
                float(v[1]),
                float(v[2]),
                float(v[3]),
                str(v[4])]
            iris_data.append(v)
            
### 可视化数据的三个维度
由于Blender是一个3D建模套件，因此将三维数据可视化似乎最合乎逻辑。清单2-11将球体放置在由每个观察的萼片长度，
萼片宽度和花瓣长度指定的3D视窗的（x,y,z)值处。

清单2-11。可视化数据的三个维度

    # Columns：
    # ’Sepal.Length','Sepal.Width',
    # 'Peta.Length','Petal.Width','Species'
    
    # Visualize 3 dimensions
    # Sepal.Length,Sepal.Width,and 'Petal.Length'
    
    # Clear scene
    ut.delete_all()
    
    # Place data
    for i in range(0,len(iris_data)):
        ut.create.sphere('row-' + str(i))
        v = iris_data[i]
        ut.act.scale((0.25,0.25,0.25))
        ut.act.location(v[0],v[1],v[2]))
        
得到的球体集出现在3D视窗中，如图2-3所示。显然，本文中印刷的2D图片并不是这种模型的正义。使用Blender的鼠标和键盘移动工具，
用户可以非常直观地浏览这些数据。

图2-3

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/2-3.png?raw=true)

### 可视化数据的四个维度

幸运的是，我们可以通过Blender Python参数化对象的方法有三种以上。为了考虑最终的数值变量，花瓣宽度，我们将按花瓣宽度缩放球体。
这将使我们能够在Blender中可视化和理解数据的四个维度。清单2-12是对前一个的略微修改。

清单2-12。可视化数据的四个维度

    # Columns:
    # 'Sepal.Length','Sepal.Width',
    # 'Petal.Length','Petal.Width','Species'
    # Visualize 4 dimensions
    # Sepal.Length,Sepal.Width,'Petal.Length',
    # and scale the object by a factor of 'Petal.Width'
    
    # Clear scene
    ut.delete_all()
    
    # Place data
    for i in range(0,len(iris_data)):
        ut.create.sphere('row-' + str(i))
        v = iris_data[i]
        scale_factor = 0.2
        ut.act.scale((v[3]*scale_factor,)*3)
        ut.act.location((v[0],v[1],v[2]))

得到的球体集出现在3D视窗中，如图2-4所示。很明显，较低的球体组具有非常小的萼片宽度。图2-5放大了这个数据集。

图2-4

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/2-4.png?raw=true)

图2-5

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/2-5.png?raw=true)

### 可视化数据的五个维度

从我们看到的这一点来看，在这些数据中至少存在两个非常不同的聚类。我们将挖掘花种数据以寻找关系。
为了轻松区分3D视窗中的花朵类型，我们可以为每种花朵类型指定几何形状。请参见清单2-13。

清单2-13。可视化数据的五个维度

    # Columns:
    # 'Sepal.Length','Sepal.Width',
    # 'Petal.Length','Petal.Width','Species'
    
    # Visualize 5 dimensions
    # Sepal.Length,Sepal.Width,'Petal.Length',
    # and scale the object by a factor of 'Petal.Width'
    # setosa = sphere,versicolor = cube,virginica = cone
    
    # Clear scene
    ut.delete_all()
    
    # Place data
    for i in range(0,len(iris_data)):
        v = iris_data[i]
        
        if v[4] == 'setosa':
            ut.create.sphere('setosa-' + str(i))
            
        if v[4] == 'versicolor':
            ut.create.cube('versicolor-' + str(i))
            
        if v[4] == 'virginica':
            ut.create.cone('virginica-' + str(i))
            
        scale_factor = 0.2
        ut.act.scale((v[3]*scale_factor,)*3)
        ut.act.location((v[0],v[1],v[2]))

3D视窗中的结果输出（2-6）揭示了数据中维度和物种之间的关系。我们在较大的星团的顶峰看到许多锥体，弗吉尼亚花，
我们在那个较大的星团的底部看到许多立方体，杂色花。这两个物种的维度之间存在一些重叠。球体，setosa花，
构成完全分离的花簇，具有较小的尺寸。
 
 图2-6
 
 ![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/2-6.png?raw=true)
 
 ### 讨论
 
 由于代码少于200行，我们为交互式多变量数据可视化软件构建了强大的概念验证。
 像这样的概念可以通过我们尚未涉及的高级API函数进行扩展，包括纹理，GUI开发和顶点级操作。目前，我们的示例软件可以在以下方面使用改进。
 
 1.无法为可视化工具缩放数据。鸢尾花数据很好地工作，因为数值方便地在(0,0,0)±10的范围内，
 这大约是默认情况下可以容易地查看多少个Blender单元。
 
 2.我们可以研究一个更好的系统来拓展对象，使它们最好地代表数据。例如，球体的体积与半径的立方体成比例，
 因此我们可以考虑将数据值的立方根作为半径传递给scale()函数。可以认为这可以创建更直观的可视化。
 可以使用相同的参数来获取数据值的平方根，因为3D视窗中的球体所覆盖的区域与其半径的平方成比例。
 
 3.在我们的五维可视化中，更改球体的颜色会更直观，而不是为每个物种指定形状。
 
 4.我们读取数据的方法是静态的，无GUI。插件开发人员自然希望将此方法应用于任何数据集，使用户能够全面控制他的观点以及他如何观察。
 
 请注意，通过ut.py,主脚本能够在Blender中操作模型而无需调用或导入bpy。这不是任何建议的做法，
 但它是Blender Python环境如何将bpy视为函数和数据的全局集合的示例。
 
 ## 结论
 
 本章介绍了许多关于Blender Python API的重要高级概念，以及bpy模块的详细核心功能。在下一章中，
 我们将详细讨论编辑模式和bmesh模块。到第三章结束时，用户应该能够使用API创建任何形状。
 随着我们引入更复杂和相互依赖的过程，抽象将变得更加重要和更加费力。
 
