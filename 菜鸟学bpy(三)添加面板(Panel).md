### 1.添加面板(Panel)

参考API文档的[例子](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/bpy.types.Panel.html?highlight=panel#bpy.types.Panel),
我们先定义一个简单的面板：
    import bpy #这个必须有 
    
    class AddPyramidPanel(bpy.types.Panel): 
        bl_idname = 'OBJECT_PT_Pyramid' #ID名称 
        bl_space_type = 'VIEW_3D' #面板所在窗口类型 
        bl_region_type = 'TOOLS' #面板所在区域 
        bl_category = 'Create' #面板所属分类 
        bl_context = 'objectmode' #面板作用情境 
        bl_label = 'Add Pyramid' #标签，也就是面板显示的标题 
        
        #定义面板元素         
        def draw(self, context): 
            pass #pass表示什么都没有，空面板 
    
    #注册面板 
    bpy.utils.register_class(AddPyramidPanel)

将上面的代码输入到文本编辑器，并运行脚本(Run Script)，就可以在3D视图窗口的工具栏下的Create分类找到新建的面板了。

![](https://upload-images.jianshu.io/upload_images/1241054-77c6d7ee8ad547bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/165)

写完这个面板之后，我试着把一些面板属性的代码注释(#)掉，发现也可以正常运行，在窗口中生成面板。

就像把bl_category = 'Create'这一行注释掉，不定义面板分类，面板就自动跑到Misc(杂项)分类下去了。

    import bpy #这个必须有 
    
    class AddPyramidPanel(bpy.types.Panel): 
    # bl_idname = 'OBJECT_PT_Pyramid' #ID名称 
    bl_space_type = 'VIEW_3D' #面板所在窗口类型 
    bl_region_type = 'TOOLS' #面板所在区域 
    # bl_category = 'Create' #面板所属分类 
    # bl_context = 'objectmode' #面板作用情境 
    bl_label = 'Add Pyramid' #标签，也就是面板显示的标题 
    
    #定义面板元素 
    def draw(self, context): 
        pass #pass表示什么都没有，空面板 
    
    #注册面板 
    bpy.utils.register_class(AddPyramidPanel)

![](https://upload-images.jianshu.io/upload_images/1241054-3d6efb4e6e96e5c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/170)

### 2.在面板下添加按钮

我们在draw方法中添加标签和按钮如下：

    import bpy #这个必须有 
    
    class AddPyramidPanel(bpy.types.Panel): 
        bl_idname = 'OBJECT_PT_Pyramid' #ID名称 
        bl_space_type = 'VIEW_3D' #面板所在窗口类型 
        bl_region_type = 'TOOLS' #面板所在区域 
        bl_category = 'Create' #面板所属分类 
        bl_context = 'objectmode' #面板作用情境 
        bl_label = 'Add Pyramid' #标签，也就是面板显示的标题 
        
        #定义面板元素 
        def draw(self, context): 
            layout = self.layout 
            layout.label(text = 'Height:') #添加标签 
            layout.operator(AddPyramid.bl_idname) #添加按钮 

    #注册面板 
    bpy.utils.register_class(AddPyramidPanel)

运行之后我们发现信息窗口报错了：

![](https://upload-images.jianshu.io/upload_images/1241054-d74a945806622a1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/548)

根据错误信息的指示，我们知道问题出在第16行，因为我们的AddPyramid这个operator并没有定义。

注： 关于layout的用法可以参考文档中的[UILayout(bpy_struct)](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/bpy.types.UILayout.html?highlight=layout.operator#bpy.types.UILayout.operator)
和[User Interface Layout](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/info_best_practice.html?highlight=layout#user-interface-layout),
界面元素比较多，你可以慢慢研究。
所以我们把上面的代码复制到上次写的添加四棱锥的operator代码里，并把无关代码行注释(#)掉：

    import bpy #加载bpy，这个是必须有的 
    
    #定义添加网格的方法 
    def Add_Pyramid(height = 2): 
        h = height #四棱锥高度 
        
        #顶点 
        verts = [(1,1,0), 
                 (-1,1,0), 
                 (-1,-1,0), 
                 (1,-1,0), 
                 (0,0,h)] 
        #边 edges = [(0,1), 
                     (1,2), 
                     (2,3), 
                     (3,0), 
                     (0,4), 
                     (1,4), 
                     (2,4), 
                     (3,4)] 
        
        #面 faces = [(0,1,4), 
                     (1,2,4), 
                     (2,3,4), 
                     (3,0,4), 
                     (0,1,2,3)] 
                     
        mesh = bpy.data.meshes.new('Pyramid_Mesh') #新建网格 
        mesh.from_pydata(verts, edges, faces) #载入网格数据 
        mesh.update() #更新网格数据 
        
        pyramid=bpy.data.objects.new('Pyramid', mesh) #新建物体“Pyramid”，并使用“mesh”网格数据 
        scene=bpy.context.scene 
        scene.objects.link(pyramid) #将物体链接至场景 

    #添加一个Operator类AddPyramid 
    class AddPyramid(bpy.types.Operator): 
        bl_idname = 'mesh.pyramid_add' #定义ID名称 
        bl_label= 'Pyramid' #定义显示的标签名 
        bl_options = {'REGISTER', 'UNDO'} 
        
        def execute(self, context): 
            Add_Pyramid() #调用Add_Pyramid()方法 
            return {'FINISHED'} #执行结束后返回值 

    #定义面板 
    class AddPyramidPanel(bpy.types.Panel): 
        bl_idname = 'OBJECT_PT_Pyramid' #ID名称 
        bl_space_type = 'VIEW_3D' #面板所在窗口类型 
        bl_region_type = 'TOOLS' #面板所在区域 
        bl_category = 'Create' #面板所属分类 
        bl_context = 'objectmode' #面板作用情境 
        bl_label = 'Add Pyramid' #标签，也就是面板显示的标题 
        
        #定义面板元素 
        def draw(self, context): 
            layout = self.layout 
            layout.label(text = 'Height:') #添加标签 
            layout.operator(AddPyramid.bl_idname) #添加按钮 

    #定义添加菜单方法 
    #def menu_func(self, context): 
    #   self.layout.operator(AddPyramid.bl_idname, icon = 'MESH_CONE') 
    
    #定义注册类方法 
    def register(): 
        bpy.utils.register_class(AddPyramid) 
        #注册面板 
        bpy.utils.register_class(AddPyramidPanel) 
        # bpy.types.INFO_MT_mesh_add.append(menu_func) #添加菜单 

    #定义取消注册类方法 
    def unregister(): 
        bpy.utils.unregister_class(AddPyramid)         
        #取消注册面板 
        bpy.utils.unregister_class(AddPyramidPanel) 
        # bpy.types.INFO_MT_mesh_add.remove(menu_func) #移除菜单 
        
    #直接执行py文件时，注册Operator 
    if __name__ == '__main__': 
        register()

再次点击运行脚本(Run Script),在工具栏创建(Create)分类下找到Pyramid按钮，单击运行就得到下面的结果了，我们成功了。

![](https://upload-images.jianshu.io/upload_images/1241054-a74351ac21239cfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

### 3.添加属性

还记得之前定义四棱锥operator里定义了一个height(高度)参数吗？

能不能把这个参数变成添加四棱锥时的可调节参数呢？

首先我们就得用到另一个bpy的模块——[Property](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/bpy.props.html),还可以参考[这里](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/info_tutorial_addon.html?highlight=property#bringing-it-all-together)。这玩意就是我界面上那些输入框和各种参数调整用到的东西。

这里我们要添加height应该是属于FloatProperty(浮点数，也就是小数)的，所以我们要添加的代码就有：

    from bpy.props import FloatProperty    #加载FloatProperty

operator的代码也得做一些添加和修改了：

    class AddPyramid(bpy.types.Operator): 
        bl_idname = 'mesh.pyramid_add' #定义ID名称 
        bl_label= 'Pyramid' #定义显示的标签名 
        bl_options = {'REGISTER', 'UNDO'} 
        
        height = FloatProperty( 
                    name = 'Height', 
                    description = 'Pyramid Height', 
                    min = 0.0, 
                    default = 2.0) 
        
        def execute(self, context): 
            Add_Pyramid(self.height) #调用Add_Pyramid()方法 
            return {'FINISHED'} #执行结束后返回值

完整代码如下：

    import bpy #加载bpy，这个是必须有的 
    from bpy.props import FloatProperty #加载FloatProperty 
    
    #定义添加网格的方法 
    def Add_Pyramid(height = 2): 
        h = height #四棱锥高度 
        
        #顶点 
        verts = [(1,1,0), (-1,1,0), (-1,-1,0), (1,-1,0), (0,0,h)] 
        
        #边 
        edges = [(0,1), (1,2), (2,3), (3,0), (0,4), (1,4), (2,4), (3,4)] 
        
        #面 
        faces = [(0,1,4), (1,2,4), (2,3,4), (3,0,4), (0,1,2,3)] 
        
        mesh = bpy.data.meshes.new('Pyramid_Mesh') #新建网格 
        mesh.from_pydata(verts, edges, faces) #载入网格数据 
        mesh.update() #更新网格数据 
        
        pyramid=bpy.data.objects.new('Pyramid', mesh) #新建物体“Pyramid”，并使用“mesh”网格数据 
        scene=bpy.context.scene 
        scene.objects.link(pyramid) #将物体链接至场景 
    #添加一个Operator类AddPyramid 
    class AddPyramid(bpy.types.Operator): 
        bl_idname = 'mesh.pyramid_add' #定义ID名称 
        bl_label= 'Pyramid' #定义显示的标签名 
        bl_options = {'REGISTER', 'UNDO'} 
        
        height = FloatProperty( 
                        name = 'Height', 
                        description = 'Pyramid Height', 
                        min = 0.0, 
                        default = 2.0) 
        
        def execute(self, context): 
            Add_Pyramid(self.height) #调用Add_Pyramid()方法 
            return {'FINISHED'} #执行结束后返回值 

    class AddPyramidPanel(bpy.types.Panel): 
        bl_idname = 'OBJECT_PT_Pyramid' #ID名称 
        bl_space_type = 'VIEW_3D' #面板所在窗口类型 
        bl_region_type = 'TOOLS' #面板所在区域 
        bl_category = 'Create' #面板所属分类 
        bl_context = 'objectmode' #面板作用情境 
        bl_label = 'Add Pyramid' #标签，也就是面板显示的标题 
        
        #定义面板元素 
        def draw(self, context): 
            layout = self.layout layout.label(text = 'Height:') #添加标签 
            layout.operator(AddPyramid.bl_idname) #添加按钮 
    #定义添加菜单方法 
    #def menu_func(self, context): 
    #   self.layout.operator(AddPyramid.bl_idname, icon = 'MESH_CONE') 
    
    #定义注册类方法 
    def register(): 
        bpy.utils.register_class(AddPyramid) 
        #注册面板 
        bpy.utils.register_class(AddPyramidPanel) 
        # bpy.types.INFO_MT_mesh_add.append(menu_func) #添加菜单 
    
    #定义取消注册类方法 
    def unregister(): 
        bpy.utils.unregister_class(AddPyramid) 
        bpy.utils.unregister_class(AddPyramidPanel) 
        # bpy.types.INFO_MT_mesh_add.remove(menu_func) #移除菜单 

    #直接执行py文件时，注册Operator 
    if __name__ == '__main__': 
        register()

运行后添加四棱锥效果如下：

![](https://upload-images.jianshu.io/upload_images/1241054-ebc99ef20da90c7f.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

### 4.添加变换调节

正常我们添加网格物体，除了一些基本的属性调节之外，还应该有对齐到屏幕、位移与旋转的变换调节。

![](https://upload-images.jianshu.io/upload_images/1241054-d14363d6075dd858.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

那我们要怎么在自己的脚本里实现这一点呢？

首先，对其到屏幕要用到BoolProperty，也就是布尔值;

位移和旋转则要用到FloatVectorProperty，也就是向量。

然后就是前面我们新建网格物体用到的是bpy.data.objects.new()[方法](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/bpy.types.BlendDataObjects.html?highlight=objects.new#bpy.types.BlendDataObjects.new),这里不能用了，我们得用object_utils.object_data_add()[方法](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/bpy_extras.object_utils.html?highlight=object_data_add#bpy_extras.object_utils.object_data_add)。区别在于后者可以使用operator作为参数，并使用自身位移和旋转作为物体的变换参数。

![](https://upload-images.jianshu.io/upload_images/1241054-48844db3d01814d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/1241054-d55f4c4584eca13c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

完整代码如下：

    import bpy #加载bpy，这个是必须有的 
    from bpy.props import FloatProperty, FloatVectorProperty, BoolProperty 
    
    #定义添加网格的方法 
    def Add_Pyramid(self, context, height = 2): 
        h = height #四棱锥高度 
        
        #顶点 
        verts = [(1,1,0), 
                 (-1,1,0), 
                 (-1,-1,0), 
                 (1,-1,0), 
                 (0,0,h)] 
        
        #边 
        edges = [(0,1), 
                 (1,2), 
                 (2,3), 
                 (3,0), 
                 (0,4), 
                 (1,4), 
                 (2,4), 
                 (3,4)] 
        
        #面 
        faces = [(0,1,4), 
                 (1,2,4), 
                 (2,3,4), 
                 (3,0,4), 
                 (0,1,2,3)] 
        
        mesh = bpy.data.meshes.new('Pyramid_Mesh') #新建网格 
        mesh.from_pydata(verts, edges, faces) #载入网格数据 
        mesh.update() #更新网格数据 
        
        from bpy_extras import object_utils 
        object_utils.object_data_add(context, mesh, operator=self) 
        # pyramid=bpy.data.objects.new('Pyramid', mesh) #新建物体“Pyramid”，并使用“mesh”网格数据 
        # scene=bpy.context.scene 
        # scene.objects.link(pyramid) #将物体链接至场景 
    #添加一个Operator类AddPyramid 
    class AddPyramid(bpy.types.Operator): 
        bl_idname = 'mesh.primitive_pyramid_add' #定义ID名称 
        bl_label= 'Pyramid' #定义显示的标签名 
        bl_options = {'REGISTER', 'UNDO'} 
        
        view_align = BoolProperty( 
                        name="Align to View", 
                        default=False, 
                        ) 
        
        height = FloatProperty( 
                        name = 'Height', 
                        description = 'Pyramid Height', 
                        min = 0.0, 
                        default = 2.0
                        ) 
        
        location = FloatVectorProperty( 
                        name = 'Location', 
                        subtype = 'TRANSLATION'
                        ) 
        
        rotation = FloatVectorProperty( 
                        name="Rotation", 
                        subtype='EULER', 
                        ) 
                        
        def execute(self, context): 
            Add_Pyramid(self, context, self.height) #调用Add_Pyramid()方法 
            return {'FINISHED'} #执行结束后返回值 
            
    class AddPyramidPanel(bpy.types.Panel): 
        bl_idname = 'OBJECT_PT_Pyramid' 
        bl_space_type = 'VIEW_3D' 
        bl_region_type = 'TOOLS' 
        bl_category = 'Create' 
        bl_context = 'objectmode' 
        bl_label = 'Add Pyramid' 
        
        def draw(self, context): 
            layout = self.layout 
            layout.label(text = 'Height:') 
            layout.operator(AddPyramid.bl_idname) 
    定义添加菜单方法 
    #def menu_func(self, context): 
    #   self.layout.operator(AddPyramid.bl_idname, icon = 'MESH_CONE') 
    
    #定义注册类方法 
    def register(): 
        bpy.utils.register_class(AddPyramid) 
        #注册面板 
        bpy.utils.register_class(AddPyramidPanel) 
        # bpy.types.INFO_MT_mesh_add.append(menu_func) #添加菜单 
   
    #定义取消注册类方法 
    def unregister(): 
        bpy.utils.unregister_class(AddPyramid) 
        bpy.utils.register_class(AddPyramidPanel) 
        # bpy.types.INFO_MT_mesh_add.remove(menu_func) #移除菜单 
        
    #直接执行py文件时，注册Operator 
    if __name__ == '__main__': 
        register()

运行后添加四棱锥效果如下

![](https://upload-images.jianshu.io/upload_images/1241054-f0783e53dc8b4107.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

这基本就是想要的结果了


[url](https://www.jianshu.com/p/3456b6fcc80d)
