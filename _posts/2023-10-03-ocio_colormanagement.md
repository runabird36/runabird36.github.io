---
layout: post
title: Common Colormanagement setting
excerpt_image: ../assets/images/img/colormanagement_houdini.png
categories: research
tags: ["OCIO", "Colorspace", "Maya", "Houdini"]
---



## summary
- Purpose : 
    - with .ocio file, set same color management
    - artist could see same view color



## Detail

**1. Make .ocio file**



**2. Set maya colormanagement preference** 


> ![maya colormanagement preference](/assets/images/img/colormanagement_maya.png)



```
if os.path.exists(OCIO_file_path):
    cmds.colorManagementPrefs(e=True, configFilePath=OCIO_file_path)
else:
    mel.eval('changeColorMgtPrefsConfigFilePath("legacy");')
```


**3. Set houdini colormanagement setting**



- [Edit]-[Color Settings...]
- Caution : 
    - In hfs19.5, Setting .ocio file in 'color setting' window does not work !
    - Use **OCIO** enviroment variable
    - ```
        ocio_path = ".../config.ocio"
        os.putenv('OCIO', ocio_path)
        hou.Color.reloadOCIO()
        ```
    - also could check input .ocio file with python
        ```
        hou.Color.ocio_configPath()
        ```



> ![houdini colormanagement setting](/assets/images/img/colormanagement_houdini.png)
