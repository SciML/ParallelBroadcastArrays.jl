# ParallelBroadcastArrays.jl

[![Build Status](https://github.com/SciML/ParallelBroadcastArrays.jl/workflows/CI/badge.svg)](https://github.com/SciML/ParallelBroadcastArrays.jl/actions?query=workflow%3ACI)

[![Coverage Status](https://coveralls.io/repos/ChrisRackauckas/ParallelBroadcastArrays.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/ChrisRackauckas/ParallelBroadcastArrays.jl?branch=master)

[![codecov.io](http://codecov.io/github/ChrisRackauckas/ParallelBroadcastArrays.jl/coverage.svg?branch=master)](http://codecov.io/github/ChrisRackauckas/ParallelBroadcastArrays.jl?branch=master)

ParallelBroadcastArrays.jl is a lightweight package to give a thin wrapper for
`AbstractArray` types to control the broadcasting behavior. For example,
on a `MultithreadArray(X)`, then we would have that

```julia
@. X = a*Y + b*Z
```

would lower to something equivalent to

```julia
Threads.@threads for i in eachindex X
  X[i] = a*Y[i] + b*Z[i]
end
```

while `MultiProcessArray` would perform a `pmap` or `@parallel`. `SplitThreaded`
would give a mixture option to users for multithreading amongst different
nodes of a cluster simultaneously.

Overloads for mathematical expressions allow for this to be used in sufficiently
abstractly typed scientific computing libraries like DifferentialEquations.jl.
