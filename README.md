<div><img src="https://files.gitter.im/musm/CniM/blob" alt="Libm.jl Logo" width="350"></img> </div>

A pure Julia math library. Implementations of the functions provided by the C math library (aka libm) in Julia.


[![Travis Build Status](https://travis-ci.org/JuliaMath/Libm.jl.svg?branch=master)](https://travis-ci.org/JuliaMath/Libm.jl)
[![Appveyor Build Status](https://ci.appveyor.com/api/projects/status/307l6b799amrpvks/branch/master?svg=true)](https://ci.appveyor.com/project/simonbyrne/libm-jl/branch/master)
[![Coverage Status](https://coveralls.io/repos/JuliaMath/Libm.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/JuliaMath/Libm.jl?branch=master)
[![codecov.io](http://codecov.io/github/JuliaMath/Libm.jl/coverage.svg?branch=master)](http://codecov.io/github/JuliaMath/Libm.jl?branch=master)



# Usage

We recommend running julia with `-O3` for maximal performance using `Libm.jl` and to also build a custom system image by running
```julia
# Pkg.add("WinRPM"); WinRPM.install("gcc")  # on Windows please first run this line
julia> include(joinpath(dirname(JULIA_HOME),"share","julia","build_sysimg.jl"))
julia> build_sysimg(force=true)
```
and then to restart `julia`; this will ensure you are taking full advantage of hardware [FMA](https://en.wikipedia.org/wiki/FMA_instruction_set)  if your CPU supports it.


To use  `Libm.jl`
```julia
julia> Pkg.clone("https://github.com/JuliaMath/Libm.jl.git")
```

```julia
julia> using Libm

julia> Libm.sin(2.3)
0.7457052121767203

julia> Libm.sin(2.3f0)
0.74570525f0

julia> Libm.exp(3.0)
20.085536923187668

julia> Libm.exp(3f0)
20.085537f0
```

The exported functions include (within 1 ulp)
```julia
sin, cos, tan, asin, acos, atan, atan2, sincos, sinh, cosh, tanh,
    asinh, acosh, atanh, log, log2, log10, log1p, ilog2, exp, exp2, exp10, expm1, ldexp, cbrt, pow
 ```
 Faster variants include (within 3 ulp)

 ```julia
sin_fast, cos_fast, tan_fast, sincos_fast, asin_fast, acos_fast, atan_fast, atan2_fast, log_fast, cbrt_fast
```

You can also access `Libm.Musl.log(x)`  for a different implementation of the logarithmic function and `Libm.Musl.erf(x)` and `Libm.Musl.erfc(x)` for the error function and the complementary error function. 

# Benchmarks

You can benchmark the performance of the `Libm.jl` math library on your machine by running
```julia
include(joinpath(Pkg.dir("Libm"), "benchmark", "benchmark.jl"))
```
Note this will take some time to run for the first time (subsequent runs will be much faster). Please ensure you run the benchmarks under `-O3` and that you are not concurrently running other cpu intensive tasks.
**Feel free to post your benchmark results at https://github.com/JuliaMath/Libm.jl/issues/34**

Sample benchmark results (commit 053528ab9b9e673bca99d1dc7db5445e39ad0d77)
```julia
julia> versioninfo()
Julia Version 0.5.0
Commit 3c9d753 (2016-09-19 18:14 UTC)
Platform Info:
  System: NT (x86_64-w64-mingw32)
  CPU: Intel(R) Core(TM) i7-4510U CPU @ 2.00GHz
  WORD_SIZE: 64
  BLAS: libopenblas (USE64BITINT DYNAMIC_ARCH NO_AFFINITY Haswell)
  LAPACK: libopenblas64_
  LIBM: libopenlibm
  LLVM: libLLVM-3.7.1 (ORCJIT, haswell)

Benchmarks: median ratio Libm/Base
sin                
time: 0.63 Float64 
time: 1.69 Float32 
                   
cos                
time: 0.71 Float64 
time: 1.73 Float32 
                   
tan                
time: 0.44 Float64 
time: 1.53 Float32 
                   
asin               
time: 1.82 Float64 
time: 2.38 Float32 
                   
acos               
time: 2.82 Float64 
time: 2.66 Float32 
                   
atan               
time: 1.59 Float64 
time: 1.91 Float32 
                   
exp                
time: 0.67 Float64 
time: 0.96 Float32 
                   
exp2               
time: 1.77 Float64 
time: 4.80 Float32 
                   
exp10              
time: 0.21 Float64 
time: 0.30 Float32 
                   
expm1              
time: 1.95 Float64 
time: 2.27 Float32 
                   
log                
time: 1.66 Float64 
time: 2.01 Float32 
                   
log2               
time: 1.37 Float64 
time: 2.04 Float32 
                   
log10              
time: 0.82 Float64 
time: 1.22 Float32 
                   
log1p              
time: 1.07 Float64 
time: 1.21 Float32 
                   
sinh               
time: 0.67 Float64 
time: 0.86 Float32 
                   
cosh               
time: 0.78 Float64 
time: 0.85 Float32 
                   
tanh               
time: 0.93 Float64 
time: 0.98 Float32 
                   
asinh              
time: 1.08 Float64 
time: 1.06 Float32 
                   
acosh              
time: 0.62 Float64 
time: 0.95 Float32 
                   
atanh              
time: 1.54 Float64 
time: 1.92 Float32 
                   
cbrt               
time: 2.49 Float64 
time: 3.75 Float32 

```

