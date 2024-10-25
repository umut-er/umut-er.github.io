---
title: Running Newt on My Locale
date: 2024-06-03 17:00:00 +0300
categories: # [TOP, SUB]
tags: gsoc
author: uue
description: In this blog post, I explained the obstacles I faced while running Newt on my local computer.
toc: true # Table of Contents
comments: false
math: true
mermaid: true 
---

## Introduction
Today, I was able to get Newt working on my local computer, which is certainly an important step. I mentioned in my proposal that there were a lot of dependency conflicts between certain parts of the project. Here, I go over more problems I encountered.

## Dependency Conflicts
In my proposal, I talked about the fact that most of the dependency conflicts were related to the forked cytoscape.js project, which I talked about in my first blog post. Quick reminder: The forked cytoscape.js project is below version 3.3.0, while certain extensions require a cytoscape.js that is above version 3.3.0. To understand the extent of the dependency conflict, I tried a temporary fix. I went to the cytoscape.js fork and, without altering anything else, changed its version number.  
This was not the only change I had to make though. I added the following:
```json
"overrides": {
    "graceful-fs": "^4.2.11"
  }
```
to the `package.json` of `sbgnviz.js`. The reason is related to some conflict related to `gulp`. To go into more detail, please review [this](https://stackoverflow.com/questions/55921442/how-to-fix-referenceerror-primordials-is-not-defined-in-node-js) stackoverflow post. Also, I had to update the `jquery` versions in `sbgnviz.js` and `chise.js` from `^2.2.4` to `^3.3.1`. After these dependency updates, I was able to run Newt on my local computer without any issues.  
This might not be an "honest" solution, because there was no merging changes from cytoscape.js 3.3.0. I am certainly planning to look at that as well now that I feel like I understand the project better. However, this shows that there are going to be no further issues related to dependency conflicts after these.

## sbgn.js (1) 
When I said "I was able to run Newt on my local computer without any issues," I lied. There is one more, which is related to the sbgn.isActive() method I was talking about in my previous blog post. This method is nowhere to be found. Even when I ran the entire application (The stack looks something like `cytoscape.js` -> `sbgnviz.js` -> `chise.js` -> `Newt`, however, the lower down projects are not necessarily independent of the projects utilizing them. For example, `cytoscape.js` cannot be used without `sbgnviz.js`), this method was not declared anywhere. I added a temporary fix, see below:
```js
// Temporary
$$.sbgn.isActive = function (node) {
    return false;
}
```
to `sbgn-cy-renderer.js` in `sbgnviz.js`, which seems to work just fine for now. Tracking this issue further, the active note stuff seems to be relatively recent. See the following git logs:
```zsh
commit 174418c24bec5e77d104e52352ee8fb39b2305c5 (origin/SBML)
Author: Selbi Ereshova <selbi.ereshova@ug.bilkent.edu.tr>
Date:   Wed Jul 27 15:06:43 2022 +0300

    Added isActive back

commit 87a417d549e3b5b794ed0333e198b79a02525ab2
Author: Selbi Ereshova <selbi.ereshova@ug.bilkent.edu.tr>
Date:   Tue Jul 5 11:13:50 2022 +0300

    Active node supported

commit d3fbf19906020d497f230168714247ac9f3f497f
Author: Selbi Ereshova <selbi.ereshova@ug.bilkent.edu.tr>
Date:   Mon Jul 4 14:28:24 2022 +0300

    Removed isActive check for now

commit 4b82f842598dd3c55e6c142269a45f4f70e7b37e
Author: Selbi Ereshova <selbi.ereshova@ug.bilkent.edu.tr>
Date:   Thu Jun 30 18:15:21 2022 +0300

    Recreated dist files
    
    Recreated dist files after adding support for active node type

commit 8eb065dd892b9397f9970268d9f4cd3aa8ee3939
Author: Selbi Ereshova <selbi.ereshova@ug.bilkent.edu.tr>
Date:   Thu Jun 30 15:38:14 2022 +0300

    documented new change
    
    documented new change: "active node support"
```
These commits are the ones just before I started working on this project (in terms of commits, not time). Inspecting the mail address, it seems that these commits are made by an undergraduate student, Selbi Ereshova, as part of another [GSoC project](https://summerofcode.withgoogle.com/archive/2022/projects/7cXTK3p5). These were in 2022. I don't know what happened to isActive() between then and now.

## sbgn.js (2) 
I mentioned in [my previous post]({% post_url 2024-05-24-initial-observations-on-newt's-backend %}) that I wanted to move sbgn implementations from `sbgnviz.js` to `cytoscape.js`. This turned out to be harder than expected. I think this is because of a cyclic dependency between `node-shapes.js` and `sbgn.js`. I have no idea how to solve this, or how moving these implementations to `sbgnviz.js` solves this issue. I have, in the commit history of my fork of cytoscape.js, [a commit](https://github.com/iVis-at-Bilkent/cytoscape.js/commit/e896df9cd78a0c14011d5b317b7c31725caa9255) where I moved all the sbgn implementations. I am not able to use this as a normal cytoscape.js version, possibly because of the said cyclic dependency issue. This will be something I will be investigating in the future for sure.

## Conclusion
I have detected some of the issues that were troubling me and found solutions to some of them. In the blog post I went over some of the things I am thinking about doing in the future. I am getting more familiar with the codebase, which is certainly going to be very helpful in the future. 
