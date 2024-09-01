---
layout: post
title: USD Crowd Test
subtitle: build usd using python from MDL to Crowd
excerpt_image: ../assets/images/img/magician_group.png
categories: Pipeline
tags: ["Houdini", "Maya", "USD", "Python"]
top: 2
---

> <iframe title="vimeo-player" src="https://player.vimeo.com/video/870657351?h=61d3eb02ff" width="700" height="420" frameborder="0"    allowfullscreen></iframe>


## summary
- building animated asset with python
    - Ani variant : only use usdPreviewSurface material
    - Render Variant : apply all shaders which are worked by loodev artist including displacement map

- arrange 10000 assets with iteration


## Detail

**1. Export lookdev and Animated geometry with Maya usd plugin**
- Do Material binding in lookdev data

| Lookdev fragment | Geo fragment |
| :--------------- | :---------- |
| ![]({{ '/assets/images/img/04_usd_ldv_fragment.png' | relative_url }}) | ![]({{ '/assets/images/img/05_usd_ani_fragment.png' | relative_url }}) |


- Asset - variant ( Ani / Render )
![USD_asset](/assets/images/img/magician_asset.png)

**2. Make Entity USD 
( Add Look Variant in Look Entity USD )**


![USD Lookdev Entity](/assets/images/img/03_usd_ldv_entity.png)
![USD Animated geo Entity](/assets/images/img/02_usd_geo_entity.png)



**3. Do Sublayering two entity usd into Asset USD**


![USD Asset USD](/assets/images/img/01_usd_asset.png)


**4. Arrange 10000 assets with instancing in maya and houdini**


![USD_crowd](/assets/images/img/magician_group.png)
