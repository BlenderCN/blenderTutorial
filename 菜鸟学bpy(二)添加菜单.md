学过了如何使用bpy添加网格，这次我们来学习如何把这个功能添加到Shift+A菜单。

### 1.添加Operator

添加Operator其实就是添加一个类(如果对什么是类不清楚的话建议先参阅资料中关于类部分的内容)。

这里，我们可以参考官方API里的实例([Basic Operator Example](https://link.jianshu.com/?t=http://www.blender.org/api/blender_python_api_2_76b_release/bpy.types.Operator.html#basic-operator-example))。

不过我们可以写的更简单一些，新建一个空类：

    import bpy #加载bpy，这个是必须有的 
    #添加一个Operator类AddPyramid 
    class AddPyramid(bpy.types.Operator): 
        bl_idname = 'mesh.pyramid_add' #定义ID名称 
        bl_label= 'Pyramid' #定义显示的标签名

### 2.注册Operator

上一步中新建Operator后，点击运行脚本(Run Script)是没有任何结果的。在3D窗口单击空格搜索Pyramid，不会得到任何结果：

![](https://upload-images.jianshu.io/upload_images/1241054-12f62b3f1b940ddc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/596)

这是为什么呢，因为我们只是新建了这个类，没有注册，所以我们搜索就无法找到了。下面我们把注册类的代码加进去：

    import bpy #加载bpy，这个是必须有的 
    
    #添加一个Operator类AddPyramid 
    class AddPyramid(bpy.types.Operator): 
        bl_idname = 'mesh.pyramid_add' #定义ID名称 
        bl_label= 'Pyramid' #定义显示的标签名 
        
    #定义注册类方法 
    def register(): 
        bpy.utils.register_class(AddPyramid) 
        
    #定义取消注册类方法 
    def unregister(): 
        bpy.utils.unregister_class(AddPyramid) #直接执行py文件时，注册Operator。 
    
    if __name__ == '__main__': 
        register()

我们再次点击运行脚本(Run Script)，在3D窗口单击空格键搜索Pyramid，就可以看到Pyramid菜单出现了。

![](https://upload-images.jianshu.io/upload_images/1241054-13f3f53e94adc68b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/595)

### 3添加方法

上一步中出现的Pyramid菜单，我们点击后是不会有任何动作的，因为我们前面新建的是一个空类，没有添加任何方法。

在前面的“新建网格物体”中，我们成功新建了一个四棱锥。我们把这部分的代码复制进去，并将其定义为Add——Pyramid方法，并为该方法添加一个height参数(默认值2），用于控制该方法添加的四棱锥高度。代码如下：

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
        
        pyramid=bpy.data.objects.new('Pyramid', mesh) #新建物体“Pyramid”，并使用“mesh”网格数据 
        scene=bpy.context.scene 
        scene.objects.link(pyramid) #将物体链接至场景

然后我们在AddPyramid类中定义如何执行Operator，关于execute方法，可参考API。

        def execute(self, context):
            Add_Pyramid()           #调用Add_Pyramid()方法
            return {'FINISHED'}     #执行结束后返回值
            
再次点击运行脚本(Run Script),在3D窗口单击空格键搜索Pyramid，并点击Pyramid菜单，就可以在场景中添加一个四棱锥了。

![](https://upload-images.jianshu.io/upload_images/1241054-0239c673cbfff01d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/615)

### 添加到标题栏菜单

    #定义添加菜单方法 
    def menu_func(self, context): 
        self.layout.operator(AddPyramid.bl_idname, icon = 'MESH_CONE') 
    
    #定义注册类方法 
    def register(): 
        bpy.utils.register_class(AddPyramid) 
        bpy.types.INFO_MT_mesh_add.append(menu_func) #添加菜单 
        
    #定义取消注册类方法 
    def unregister(): 
        bpy.utils.unregister_class(AddPyramid) 
        bpy.types.INFO_MT_mesh_add.remove(menu_func) #移除菜单

operator方法用法可参考[API文档]()。

关于菜单用法可参考]API文档]()。

修改后的最终代码如下：


