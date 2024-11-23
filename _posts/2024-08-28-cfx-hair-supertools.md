---
layout: post
title: KATANA plugins - hair
subtitle: three types of supertools
excerpt_image: ../assets/images/img/cfx_hair_supertools_001.png
categories: Pipeline
tags: ["Katana", "C++", "lua", "Python", "Math"]
top: 1
---


![banner](/assets/images/img/cfx_hair_supertools_002.png)

## summary
- Category : Katana plugin
- Plugin list
  - **TweakHair** : tweak hair parameters (density / length / thickness / root thickness / tip thickness)
  - **SimGuideDeform** : deform static hair based on simulated guides
  - **GuideDeform** : deform static hair based on animated meshes
- Features
  - **GuideRotationOffset** : the offset that rotate the points of guide after the deformation proces
  - **MeshPointOffset** : the offset that move points of mesh after the deformation process



## Offset Features
- When an animated mesh is **stretched too much**, Guides penetrate the mesh. and This is when offset features are needed
  
| Deformation Only | applying MeshPointOffset | applying GuideRotationOffset |
| ---------------- | ------------------------ | ---------------------------- |
| ![origin](/assets/images/img/offset_001.png) | ![meshPointOffset](/assets/images/img/offset_002.png) | ![GuideRotationOffset](/assets/images/img/offset_003.png) |

---

## Optimization
- Issue : deformation calculation time is too long when move frame one by one.
- Solution : Implement deformation formula using C++
- Performance comparison. 
  - feel free to refer to attached images which show 'Render performance' in katana (if you click image, you can zoom in )

| Spec of Character | using lua script | using C++ |
| ----------------- | ---------------- | --------- |
| <span style="display:block; text-align:center;">- Character A <br> - point count : **37,180,792** <br> - strand count : **3,380,072**</span> | - Elapsed Time : **15 mins** <br> ![CharALua](/assets/images/img/char_A_lua.PNG) | - Elapsed Time : **28.73 sec** <br> ![CharACpp](/assets/images/img/char_A_cpp.PNG) |
| <span style="display:block; text-align:center;">- Character B <br> - point count : **5,465,140** <br> - strand count : **1,477,814**</span> | - Elapsed Time : **2 mins** <br> ![CharALua](/assets/images/img/char_B_lua.PNG) | - Elapsed Time : **6.02 sec** ![CharACpp](/assets/images/img/char_B_cpp.PNG) |



## Demonstration
- Data : simple demo character

<iframe width="650" height="405" src="https://www.youtube.com/embed/KuFu3CsN1XI?si=Re3n1-9RRBcuYu23" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


## Render results
- Data : simple demo character

<iframe width="650" height="405" src="https://www.youtube.com/embed/Nz1TM1Bx6HA?si=qsFgU1II_wzTKL-1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
