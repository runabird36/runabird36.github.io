---
layout: post
title: Houdini OpenPort
subtitle: communicate between houdini and standalone tool
excerpt_image: ../assets/images/img/houdini_logo.png
categories: research
tags: ["Houdini", "Python"]
---



### Keywords 
- houdini openport example

### Documentation 

> [OpenPort and communicate via Python](https://www.sidefx.com/forum/topic/46759/?page=1#post-384035)

>[houdini RPC docs](https://www.sidefx.com/docs/houdini/hom/rpc.html)

### Contents 
- hrpyc is wrapper version of python rpyc library
- Like below, connection is possible with only python rpyc module 
(아래와 같이 rpyc 만 사용해도 실제로 stdout 문자열 통신이 가능하다)
```
con  = rpyc.classic.connect("localhost", 18811)
rsys = con.modules.sys
rsys.stdout.write("hello world!!")
```
- How to use
    - step01 . In houdini
        ```
        import hrpyc
        hrpyc.start_server() # default : 127.0.0.1:18811
        ```

    - step02. In external python

        ```
        import socket, sys
        sys.path.append("/opt/hfs19.5/houdini/python3.9libs")
        import rpyc
        import hrpyc
        connection, hou = hrpyc.import_remote_module()
        hou.node("/obj").createNode("geo")
        ```
        With this, Could use hou commands in external python.
