---
layout: default
title:  The Non-Turing-Complete Python-based Meson Build System
author: William F Godoy
date: 2020-07-22
categories: Programming Scientific-Computing
tag: Programming
---

#  {{ page.title }} 
{{ page.date | date: '%B %d, %Y' }}  
**_Reading time:_** 11 minutes. 


> [Meson](https://mesonbuild.com/), officially "The Meson Build system", has been a great addition to my projects requiring a build system (mostly C or C++). Meson has been around for a few years and has been [adopted by several major projects](https://mesonbuild.com/Users.html) as a modern alternative to GNU Make and, why not, some parts of CMake. In this post I talk about its user-friendly syntax, CMake interoperability, and readily available functionality to cut a lot of the boilerplate in past build systems. Overall, it's a great productivity tool that anyone should consider when selecting a modern build system.
![]({{ site.baseurl }}/images/meson_logo.png){:height="120px" width="150px"}   
<small>*Meson's Logo, source: [Meson repo on GitHub.](https://github.com/mesonbuild/meson)*</small>

## Some background.
I'm a long time user of Fortran, C++, C, for scientific computing so dealing with Makefiles has always been a requirement. More modern software stacks I deal with use CMake (legacy) while I got the opportunity to collaborate with Kitware Inc. (CMake creators) working on projects using modern CMake. I used Java ant (another neat tool) back in the 2000s. Overall, rather than being an expert in build systems, I just know enough to get the job done. 
 
## How did I come across Meson?

I have to be honest, I don't remember the exact moment I discovered Meson. My best guess is that I was probably googling "CMake alternatives" or "Modern portable C++ build system" in 2018 and perhaps a reddit thread brought me to Bazel, Waf and Meson. 

## Why Meson?

I did a little more digging and got really impressed with all the available features and design decisions. The most important aspects:

- Extremely simple Python-based syntax
- Non-Turing Complete (no functions, so more declarative)
- Very clear [documentation](https://mesonbuild.com/)
- Automatic options for sanitizers and code coverage
- Portability through multiple backend support (VS, ninja)


## Why not Meson?

## Meson and CMake

This section is unavoidable. CMake is refer as to the "defactor standard".
Comparing CMake and Meson is apples to oranges. My mental model is that CMake tries to be everything including packaging and flexible configuration, while Meson is only equivalent to the build part with a restrictive model (not necessarily a bad thing) for standard dependencies that relies on [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config/) for package lookup and packaging, [ninja](https://ninja-build.org/) as a backend for the actual build on Linux (no make support), and a single flat "subprojects" directory for custom dependencies.

Therefore, we can split CMake into several components with a much larger scope than Meson:

`CMake ~ Meson + functions + Pkg.Config + flexible path configuration + CPack + Make backend option`


# [Back to main page]({{ site.url }}/)
