学习如何使用blender的python脚本创建模型，为blender的脚本入门做个总结，以后可以慢慢深入学习研究。

打开一个任意模型的blender窗口。
![](https://imgsa.baidu.com/exp/w=500/sign=511835a297510fb378197797e932c893/f9198618367adab4fd38605882d4b31c8601e4c5.jpg)

在左下角切换到python控制台，这里可以在控制台一行一行的敲python代码，如果你喜欢的话也可以直接这么来。
![](https://imgsa.baidu.com/exp/w=500/sign=157f75ace050352ab16125086342fb1a/9a504fc2d5628535ca51a83a99ef76c6a7ef6373.jpg)

还有一种方法在左下角切换到文本编辑器
！[](https://imgsa.baidu.com/exp/w=500/sign=045d9677193853438ccf8721a312b01f/8435e5dde71190ef47b1f7c4c71b9d16fdfa6066.jpg)

创建一个文本框，当然如果你有外部写好的文本也可以直接打开。
![](https://imgsa.baidu.com/exp/w=500/sign=defee939c1fcc3ceb4c0c933a244d6b7/83025aafa40f4bfb6203fb4f0a4f78f0f63618dd.jpg)
然后在里面输入如下的脚本内容(在对应xyz范围内随机创建100个立方体）:

    import bpy

    from random import randint

    bpy.ops.mesh.primitive_cube_add()

    #创建模型的数量

    count = 100

    for c in range(0,count):

        x = randint(-100,100)

        y = randint(-100,100)

        z = randint(-100,100)

        bpy.ops.mesh.primitive_cube_add(location=(x,y,z))
        
![](https://imgsa.baidu.com/exp/w=500/sign=48f2b4d59025bc312b5d01986edf8de7/71cf3bc79f3df8dcb407ee8ac411728b471028f6.jpg)

运行脚本，就会生成100个立方体模型
![](https://imgsa.baidu.com/exp/w=500/sign=4a1080148e94a4c20a23e72b3ef51bac/79f0f736afc3793196cde732e2c4b74543a91107.jpg)

