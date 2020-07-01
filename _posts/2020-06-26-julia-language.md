---
layout: default
title:  First Project using the Julia Language
author: William F Godoy
---

#  {{ page.title }}

> **_Reading time:_** 11 minutes.

It's been a few times that I heard about the Julia language in the past. Labeled as a programming language for "data science" and "scientific computing". I felt intrigued and decided to use it for one of my projects at work considering that it's been nearly 2 years since [Julia reached a stable 1.0 version](https://julialang.org/blog/2018/08/one-point-zero/). I hope sharing my experience will help other people interested in the language for their applications.

## My background and why I should mention it.

I'm a long time user of Fortran, C++, C, and I have used Python, R, Matlab, and Java in the past, mostly for scientific computing projects. I'm far from being an expert in any language and I just try to keep up with project needs.

This is important to point out because people with different backgrounds may have completely different opinions or I could just be dead wrong. It could also help the reader come up with its own mental mapping to some the features Julia "borrow" from other languages as I describe in [this section](#Julia-syntax-and-other-languages).
 
## How did I come across Julia?

The first time I heard about Julia was around 2015-2016 when I was working at Intel Corporation. One of my colleagues mentioned as a really good language that is suitable for Artificial Intelligence (AI) and Machine Learning (ML) applications. I must admit I didn't pay much attention since it was far from stable and I remember reading blogs and posts about the language. My first thought was to give it more time as it's still work in progress.

The second time was around 2018 a request came in one of the open source project for Julia bindings. It renewed my interest, but we declined the request given the lack of actual projects using Julia and the fact that Julia wasn't stable, yet. Around the same time, I attended [Professor Alan Edelman's](https://math.mit.edu/directory/profile.php?pid=63) talk at Oak Ridge National Laboratory. He is one of the founders of Julia and a renowned authority in the field. I had a much better idea of what Julia does. My impression was that it's a really nice dynamic interface for numerical computing and linear algebra abstractions. Fast forward to 2020 and I decided to finally give it a shot.
 
## Setting up my first Julia project

I was ready to jump as I had a project that I started drafting with Python, but soon enough I restarted it in Julia. My first impressions of each aspect of the process:

- **Installation**: very easy. I use Ubuntu 18, which doesn't have a Julia package by default, so I had to download and install [Julia v1.4](https://julialang.org/downloads/) which is not a big deal. Since the release cycle is rather [short](https://github.com/JuliaLang/julia/releases), I installed julia under `/opt/julia/1.4/` and set a symbolic link in `/usr/bin/julia` to `/opt/julia/1.4/bin/julia` anticipating that I'd update the default version periodically. Other options are snap or adding source package repos, but I didn't use them.

- **License**: Julia has a MIT license which is a free and permissive license, which I see as a strong point. Most of my projects use either Apache, BSD or MIT permissive licenses.

- **Learning resources**: there are plenty of tutorials, examples, posts, podcasts. I settled with [From zero to Julia!](https://techytok.com/from-zero-to-julia/) for a quick tutorial as it breaks down each aspect of the language with examples very nicely. For the most part, if you're coming from Python the syntax is very easy to pick up (Fortran, R or Matlab help for the 1-index and column-major syntax). I listened to some [podcasts](https://juliacomputing.com/media/podcasts.html) trying to understand the creators' philosophy and design decisions. For online resources, I'd come frequently to the [official docs](https://docs.julialang.org/en/v1/) and I subscribed to [https://discourse.julialang.org/](https://discourse.julialang.org/), which is very active. 

- **Integrated Development Environment (IDE)**: I use the [Eclipse CDT IDE](https://www.eclipse.org/cdt/) on a regular basis as I work in multiple language projects which are covered with Eclipse's rich ecosystem of plugins (photran, pydev, CDT). To my surprise the [JuliaDT plug-in](https://github.com/JuliaComputing/JuliaDT) has not been touched since 2016. Instead all efforts have been put in the [Juno IDE](https://junolab.org/). I continued using JuliaDT, not without a minor installation [issue](https://github.com/JuliaComputing/JuliaDT/issues/38) my colleague helped me solve. They are also not responsive in the issue tracker. JuliaDT hasn't been updated with the latest language syntax and it's essentially an incomplete editor, not a proper IDE with all the bells and whistles. As Julia grows I hope for renewed community activity in JuliaDT.

- **Installing packages**: this is one of the really strong points of Julia. Similarly to [R Cran](https://cran.r-project.org/) and [Rust's Cargo](https://doc.rust-lang.org/cargo/), packaging is part of the standard language ecosystem through the [`Pkg` builtin package manager](https://docs.julialang.org/en/v1/stdlib/Pkg/index.html). I can think of the many times I encountered problems mixing different Python package managers: pip, conda, synaptic; and versions for numpy, scipy and others. Not to mention dependency hell with Fortran, C and C++. Having a unified approach really helps for well-established stable libraries. Projects in Julia have a [`Project.toml` file](https://julialang.github.io/Pkg.jl/v1/toml-files/) which I see as a project manifest meaning that configuration and dependencies are also addressed by the language.

- **Testing**: another strong point. Testing, in particular unit testing, is [built into the language](https://docs.julialang.org/en/v1/stdlib/Test/). I found it similar to `pytest` in the sense that your unit tests are detected under a `test` directory from your project base directory, but there are slight differences. Julia projects require a `test/runtests.jl` file which is executed from your base directory as: 
  
  ```bash
  # Running tests from a project base directory
  $ julia --project=. test/runtests.jl
  ```
  
  `test/runtests.jl` is a single point of entry to include other test files as required for your project. Test commands are `@` macro -based, I ended up using `@testset` for a family of tests and `@test` for each assertion. `--project=.` is required as I was working on a module and this argument allows Julia to look for your module under the `src` directory by default.

  My tests were very simple so I didn't dig further. My `test/runtests.jl` file looked like this:
  
  ```julia
  using Test, Base.Filesystem

  import Exio

  @testset "test_AmrexCastro" begin
	# include just copies and paste code, just like C, C++ and Fortran
	include("test_AmrexCastro.jl")
  end;

  @testset "test_Exio.input_parser_docstring" begin
  @test println(@doc Exio._input_parser) === nothing
  end;
  ```

- **Built-in documentation**: another strong point, similar to [Python's docstring](https://www.python.org/dev/peps/pep-0257/) Julia provides [built-in documentation](https://docs.julialang.org/en/v1/manual/documentation/index.html) for functions, types that is easy to pick up and the fact that it's picked up from the text above the function in question.

- **REPL**: Julia read-eval-print loop REPL, or interactive command-line environment, is similar to what you expect from Python and R. I personally don't use this mode often and I was more interested in a "package" project running from terminal as "scripting". Still, end-users should become familiar with Julia's REPL as it's more natural for interactive usage rather than what I call "scripting".

## Julia syntax and other languages

In this section I try to draw parallels with other languages. Take it with a grain of salt, this is just my own mental map.

- **"Statically" typed as HPC languages**: this is one of the reasons I find Julia compelling for scientific computing. Performance, Julia's stronghold, requires to know as much as you can about your application in terms of memory alignment of underlying types. Also, reading large scientific code bases without types makes it a bit difficult to follow (think Fortran without `implicit none`). It's common for scientific code to be written once by a one or a few individuals and read many times by a group of people, thus typing enhance group communication. I prefer a good balance between defining types and no type information at all. For example, in C++ I try to use, but not abuse, the `auto` keyword. Even Python is allowing some sort of [typing since v3.5](https://docs.python.org/3/library/typing.html). It's great that some form of "static" typing baked into the Julia language to be able to do checks the first time a program runs and for code readability. 

  For example, I had to write a helper function that reuses the Julia's [Filesystem `walkdir`](https://docs.julialang.org/en/v1/base/file/#Base.Filesystem.walkdir). 

  Statically typed, input must be a `String`, output is `Int64`:
  ```julia
    function helper_get_directory_size(directory::String)::Int64
      size::Int64 = 0
      for (root, dirs, files) in walkdir(directory)
        size += sum(map(filesize, joinpath.(root, files)))
       end
      return size
    end
  ```
  Notice that it helps users of this function to know the input and output types, while internal variables from `walkdir` `(root,dirs,files)` are self-explanatory.

  For completeness, here is a slightly more ambiguous version without types. Is directory a string?, is size 32-bits or 64-bits?
  
  ```julia
  function helper_get_directory_size(directory)
      size = 0
      for (root, dirs, files) in walkdir(directory)
          size += sum(map(filesize, joinpath.(root, files)))
      end
      return size
  end
  ```

- **Fortran for numbers, Python for other things**: Julia follows the Fortran column-major, 1-index syntax for multidimensional arrays, in the same line of R and Matlab. I think it makes sense since much of the underlying numerical software originated in Fortran and these languages were designed with arrays as first-class citizens. Julia doesn't change this (which is a good thing). On the other hand, Julia feels close to high-level functionality in Python or C++ for things like data structures: [Dictionaries](https://docs.julialang.org/en/v1/base/collections/#Dictionaries-1), [exception handling](https://docs.julialang.org/en/v1/manual/control-flow/#Exception-Handling-1). For example, looping through the keys of a dictionary looks like this:

  ```julia
  for key in keys(extractor.outputs)
    if (key == "plots_size")
      _run_linear_model_plots_size(extractor, X)
    end
  end
  ```

  I used [Julia's Filesystem](https://docs.julialang.org/en/v1/base/file/) and the syntax is a bit different from Python's [os](https://docs.python.org/3/library/os.html) or [pathlib](https://docs.python.org/3/library/pathlib.html), but equally intuitive. Overall, there is an adjustment if coming from Python since Julia is not object-oriented. Ultimately, both approaches get the job done with a flat learning curve.  

- **Modules as in Fortran and Python**: Julia isolates code namespace in [modules](https://docs.julialang.org/en/v1/manual/modules/index.html#modules-1). This is very similar to Fortran and Python, with a few syntax differences but the goal is clearly the same. Here is an example:

  Declaring a module:
  ```julia
  module MyModule

  export MyStruct, mymodule_init

  include("mymodule/functions.jl")
  
  end
  ```
   
  Using a module:
  ```julia
  import MyModule
  
  MyModule.mymodule_init(x,y)
  ```
  
  I must admit I try not to use the `using` keyword, only in few cases (testing). I prefer seeing where things come from for code clarity and to avoid name clashes. It's [frowned upon in C++](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rs-using).  

- **structs as in C**: Julia [structs or composite types](https://docs.julialang.org/en/v1/manual/types/#Composite-Types-1) are similar to C structs, or Fortran derived types pre-object-oriented Fortran. They can only contain data, not functions. This is part of Julia's philosophy, each struct must be a concrete container of data, abstract structs on the other hand can't contain data. Also, structs are immutable by default, [similar to Rust approach for variables](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html), you need to declare it using the [`mutable struct` keyword](https://docs.julialang.org/en/v1/manual/types/#Mutable-Composite-Types-1). You can implement constructors for initialization which I prefer as it aligns with [C++ RAII principle](https://isocpp.org/blog/2018/02/to-raii-or-not-to-raii-jonathan-boccara).


- **Abstract Inheritance: Behavior not Data, and Composition**: Julia is not exactly "object-oriented" as Python, Java, or C++. Inheritance is limited to functions on abstract types (which have no members), while composition, "has-a", is favored over member inheritance "is-a". There is a [github issue](https://github.com/JuliaLang/julia/issues/4935) for the reasoning behind these decisions on not supporting abstract types with fields. It essentially leads to a more complex model (like Python or C++) that the Julia designers don't want to enforce or maintain. I personally find silly when I hear that composition "is better" than inheritance. These are just tools to express the relationship between your objects and using the two appropriately can lead to a powerful design. Each one scales differently, but I agree that inheritance can be easily abused since developers overlook the "is-a" aspect. In any case, Julia model is simple, but implies that you must let go field inheritance. Treat each struct as a container of data and expect a lot of replication or third-party macro usage to "mimic" field inheritance.

- **Multiple dispatch as in Fortran, C++**: abstract structs serve the purpose of providing a template for functionality of the derived structs. This leads to multiple dispatch or duck typing, essentially similar to function overloads and template traits in C++. I like the fact that Julia docs provide a section on the [dangers of abusing multiple dispatch]("https://docs.julialang.org/en/v1/manual/performance-tips/#The-dangers-of-abusing-multiple-dispatch-(aka,-more-on-types-with-values-as-parameters)-1") which I highly recommend before applying multiple dispatch everywhere in your program.


## Julia Packages Used

Overall, the package stack was kept simple and beyond the Julia Base I only used a few packages to solve a linear fitting model, plotting and file pattern search. They are listed here:

- [DataFrames.jl](https://juliadata.github.io/DataFrames.jl/stable/)

- [Plots.jl](http://docs.juliaplots.org/latest/)

- [GLM.jl](https://juliastats.org/GLM.jl/latest/) Generalized linear model

- [Glob.jl](https://github.com/vtjnash/Glob.jl) File pattern search


## Julia Performance

Performance is presented as Julia's big selling point as it solves the "two-language problem". Personally, the "two-language problem" wasn't my biggest problem or concern. Julia itself can be seen as a nice front-end to LLVM which is written in C++, and usually Python underlying libraries (written in C, C++ or Fortran) are rich enough for common tasks with numerical structures. I'd expect Julia or Python libraries would be calling BLAS or LAPACK somewhere under the hood. However, I can see the value in many areas in which **CPU runtime** could be important as you just write code in "one language" to maintain a single codebase.

- **Just-in-time (JIT) and start-up times**: I have to be honest, at first it was a bit dissapointing to experience slow startup times for common packages, in particular [Plots](http://docs.juliaplots.org/latest/) with honorable mentions to [DataFrames](https://juliadata.github.io/DataFrames.jl/stable/) and [GLM](https://juliastats.org/GLM.jl/latest/). In any case, I found the best workaround is to pre-compiled your stack into a shared-library. To do this in the REPL:

  ```julia
  julia> using PackageCompiler
  julia> create_sysimage([:Glob, :Plots, :GLM, :DataFrames], sysimage_path = "jexio_deps.so")
  ```
  To later run tests using:
  
  `$ julia -Jjexio_deps.so --project=. test/runtests.jl`

  The above would reduce roughly 9s to 1s of my "precious" time everytime I run my unit tests.

- **Parallel and GPU support**: this is another strong point. [Multithreading is part of the language](https://docs.julialang.org/en/v1/base/multi-threading/), and great efforts with [JuliaGPU](https://github.com/JuliaGPU) for, well you guess, GPU computing. Granted many of these efforts are experimental, they are constantly evolving to become more stable. I didn't use these for my project, but it's nice to know they are being actively developed.


## Final Thoughts and Wishlist

Overall, I like what Julia offers for scientific computing. It's well designed and easy to grasp if you're coming from a numerical background (Fortran, Matlab, R). I find comparisons with Python a bit unfair, since Python is a well-established general-purpose dynamic language with a rich ecosystem which has numerical capabilities later added into the language. Would I migrate Python production code to Julia? Certainly not due to the associated costs. Each language is fine on its own, I think of Julia as being domain-specific for data analysis and number crunching. Would I write a graphical interface in Julia or a parallel I/O library or a game engine? Probably not. Would I use it for data analysis and a quick prototype for number crunching applications, certainly yes.

My wishlist is:

- Have more IDE support available besides Juno
- Have more robust libraries for I/O, machine learning
- Have some sort of member inheritance formalized into the language (there are third-party macros available)
- Learn more about current capabilities and future roadmap at JuliaCon 2020


# [Back to main page]({{ site.url }}/)

