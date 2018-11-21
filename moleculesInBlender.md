首先，让我们看看我们将如何指定化学结构。化学世界充斥着可怕的文件格式，并且由于传统标准的普遍弹性，我们需要考虑它们。
然而，在我们开始之前，让我们决定分子数据的良好扩展表示。编码中使用的常用表示称为Javascript Objiect Notation或json，
它非常擅长简洁地表示任意数据。如果你以前从未听说过，我强烈建议你继续阅读它。这是
[wiki](http://en.wikipedia.org/wiki/JSON)
由于分子是任意数据的一个子集，json应该适合我们的需要。我们的drawer需要原子元素类型和3D位置，以及键连接和顺序。
因此，让我们使用如下所示的东西（例子是乙烷）

    {
        "atoms": [
            { "element": "C", "location": [ 0.252, -0.116, -0.704 ] },
            { "element": "C", "location": [ -0.252, 0.116, 0.704 ] }
        ],
        "bonds": [
            { "atoms": [ 0, 1 ], "order": 1 }
        ]
    }

这很容易在Python中解析

    import json
    data =json.loads(string)

在这两行中，我们现在有一个Python数据结构！如果我们想要打印第一个原子的y坐标，我们可以这样做

    pirnt（data["atoms"][0]["location"][1]).



[link](https://patrickfuller.github.io/molecules-from-smiles-molfiles-in-blender/)
