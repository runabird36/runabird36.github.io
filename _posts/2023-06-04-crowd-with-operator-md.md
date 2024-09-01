---
layout: post
title: Crowd using Arnold render procedural
subtitle: assign shaders to crowd cache using operator
excerpt_image: ../assets/images/img/Operator_01.png
categories: Pipeline
tags: ["Crowd", "Maya", "Arnold", "Render Procedural", "Operator"]
top: 3
---




---

![yeti_whole](/assets/images/img/crowd_whole02.png)


## Summary

- render approximately 7500 characters 
- Data in used : 
    - crowd : exported from golaem in aistandin
    - lookdev : exported with aistandin (arnold scene description data)
    - arnold operator nodes : lookdev information and all connection information 


---

## Details

- A. developed tools for work
> ![dev tools](/assets/images/img/Operator_07.png)



- B. Each asset operator node tree
    - Selection : grouping shapes that is assigned to same material
    - Assignment Information : link between above selections and material
    - merge : merge all node

> ![Each asset node tree](/assets/images/img/Operator_05_resized.png)



- C. all materials informations
    - export all asset's lookdev into aistandin format (.ass)
    - and then import with aiInclude operator node

> ![lookdev information](/assets/images/img/Operator_08.png)



- D. merge all asset's node tree and all materials, and then connect to crowd aistandin node

> ![All node tree](/assets/images/img/Operator_04_resized.png)



- E. render results
    - In hypershade window, we could find that there is no materials and texture nodes

> ![render results 01](/assets/images/img/Operator_01.png)



- F. Zoom in and render results
    - render time : 00:01:05

> ![render results 02](/assets/images/img/Operator_03_render.png)
