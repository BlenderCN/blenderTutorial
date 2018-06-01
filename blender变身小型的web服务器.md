在blender的文本编辑器里面输入：

    import http.server
    import socketserver
    PORT = 8000
    Handler = http.server.SimpleHTTPRequestHandler
    httpd = socketserver.TCPServer(("", PORT), Handler)
    print("serving at port", PORT)
    httpd.serve_forever()
运行脚本

打开浏览器，输入http://127.0.0.1:8000   现在你的blender文件夹就在浏览器里面可以浏览了

[url](bbs.blendercn.org/forum.php?mod=viewthread&tid=2089)
