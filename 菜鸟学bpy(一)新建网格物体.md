### 1.Blender网格数据

学过几何的都知道，连点成线，连线成面，blender的网格也是这个原理来的。

》点：

假设三维空间中有两个点，坐标为：

    verts = [(0,0,0),(1,0,0)]

bpy中点的坐标是作为列表数据存储的，列表中每个点依次赋予一个索引值(index),上面两个点的索引值就为0,1。

》 线：

在bpy数据中这两个点连线的表示为：

    edge=[[0,1]]
    
bpy中线的数据也是作为列表存储的，两点成线，取两个顶点的索引值，我们就得到了一条位于X轴的线段。

![](https://upload-images.jianshu.io/upload_images/1241054-630d3ca325f007d5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/452)

》面

大家都知道三个点才能决定一个平面，所以要得到一个面，我们必须至少有三个点：

    verts= [(0,0,0),(1,0,0),(0,1,0)]
然后我们用一个索引列表把三个点连起来，就得到一个面：

    face=[[0,1,2]]
    
![](https://upload-images.jianshu.io/upload_images/1241054-6254ee767ad0cf90.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/457)

bpy中面的数据也是作为列表存储的，把一个面包含的所有点作为面数据的一个子列表，blender会自动将其闭合，然后我们看到了一个位于XY平面的三角面。

### 2.用bpy新建一个四棱锥

首先四棱锥是由5个点组成的，有四个三角面和一个矩形面。

![](https://upload-images.jianshu.io/upload_images/1241054-a70306203921275e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/439)

![](https://upload-images.jianshu.io/upload_images/1241054-2835164d64ba7625.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/458)

下面话不多说，上代码。

    import bpy #加载bpy，这个是必须有的 
    #顶点
    verts = [(1,1,0), 
             (-1,1,0), 
             (-1,-1,0), 
             (1,-1,0), 
             (0,0,2)] 
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
    
    pyramid = bpy.data.objects.new('Pyramid', mesh) #新建物体“Pyramid”，并使用“mesh”网格数据 
    
    scene = bpy.context.scene 
    scene.objects.link(pyramid) #将物体链接至场景
    
运行代码，就得到下面的结果。

![](https://upload-images.jianshu.io/upload_images/1241054-574cb9a8dc85cabf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/408)

如果将载入网格数据代码中的面数据改为空列表：
    
    mesh.from_pydata(verts,edges,[])
    
再运行脚本就可以在3D视图窗口获得下面的结果：

![](https://upload-images.jianshu.io/upload_images/1241054-2bae1fd37410f805.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/431)



[url](https://www.jianshu.com/p/9b86079d39f7)
