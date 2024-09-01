---
layout: post
title: Scene Assembly using Render Procedural
subtitle: assemble scene data using arnold operator
excerpt_image: ../assets/images/img/OTX_summary_img.png
categories: Pipeline
tags: ["Arnold", "Maya", "Operator", "Render Procedural"]
---


<iframe title="vimeo-player" src="https://player.vimeo.com/video/870051101?h=103f71f263" width="640" height="360" frameborder="0"    allowfullscreen></iframe>


## Explanation
- Assemble data in lighting scene with Arnold operator and OTX tool
    - OTX 
        - Select published lookdev data
        - Set Arnold shape attribute


## Another case 
- Path mapping between Windows and Rocky linux
<iframe width="630" height="385" src="https://www.youtube.com/embed/2B-GWxfeVIk?si=h16btgVwRzYT6qZd" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- Specification
  - OS         : windows
  - Data       : maya scene lookdev asset data worked on linux
  - Issue      : textures are not recognized
  - Solution   : With aiStringReplace node which is one of the operator nodes, fix texture path at render time
