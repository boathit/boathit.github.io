---
layout: post
comments: false
title: "Julia Setup"
date: 2021-08-26 00:00:00
tags: julia program
---

> Julia Setup.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

### Add Revise.jl

```sh
mkdir -p ~/.julia/config/ && echo "using Revise" >> ~/.julia/config/startup.jl
```

### Create system image to reduce package loading latency

* Create the folder `juliaimages`
    ```sh
    mkdir -p ~/.julia/juliaimages
    ```

* Create file `~/.julia/juliaimages/precompile_image.jl`

    ```julia
    using Plots

    display(plot(rand(5), rand(5)))
    ```

    and file `~/.julia/juliaimages/create_image.jl`

    ```julia
    using PackageCompiler, IJulia

    create_sysimage([:Plots, :StatsPlots, :Turing], sysimage_path="sysimage.so", 
        precompile_execution_file="precompile_image.jl")

    ## jupyter kernelspec list 
    IJulia.installkernel("JuliaFast", "--sysimage=" * joinpath(homedir(), ".julia/juliaimages/sysimage.so"))
    ```

* Run `julia create_image.jl` to generate `sysimage.so` as well as install the corresponding jupyter notebook kernel. 

The `sysimage.so` file can also be used as follows in a REPL session.

```sh
julia --sysimage .julia/juliaimages/sysimage.so
```
