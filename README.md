# GaussianCouplingMessagePassingRules

[![CI](https://github.com/ReactiveBayes/GaussianCouplingMessagePassingRules.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/ReactiveBayes/GaussianCouplingMessagePassingRules.jl/actions/workflows/CI.yml)
[![Docs: stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://reactivebayes.github.io/GaussianCouplingMessagePassingRules.jl/stable/)
[![Docs: dev](https://img.shields.io/badge/docs-dev-blue.svg)](https://reactivebayes.github.io/GaussianCouplingMessagePassingRules.jl/dev/)
[![Coverage](https://codecov.io/gh/ReactiveBayes/GaussianCouplingMessagePassingRules.jl/graph/badge.svg)](https://codecov.io/gh/ReactiveBayes/GaussianCouplingMessagePassingRules.jl)
[![Code style: Runic](https://img.shields.io/badge/code_style-%E1%9A%B1%E1%9A%A2%E1%9A%BE%E1%9B%81%E1%9A%B2-black)](https://github.com/fredrikekre/Runic.jl)

The `GaussianCoupling` node, the bilinear potential `φ(out, in, a) = exp(out ⋅ a ⋅ in)`, and its
message passing rules. It is the edge potential of Gaussian belief propagation: with
`NormalWeightedMeanPrecision(b[i], A[i, i])` priors and `a = -A[i, j]`, message passing solves
`A x = b`. Its messages are improper normals, and its rules take the structured factorisation
`q(out, in) q(a)` with a constant `a`.

```julia
using GaussianCouplingMessagePassingRules, MessagePassingRulesBase, ExponentialFamily, BayesBase

result = @call_message_update_rule(
    node = GaussianCoupling, target = :in,
    m = (out = NormalMeanVariance(2.0, 3.0),), q = (a = PointMass(-0.5),),
)
getresult(result)   # NormalWeightedMeanPrecision(-1.0, -0.75)
```

```julia
import Pkg; Pkg.add("GaussianCouplingMessagePassingRules")
```

- Documentation: <https://reactivebayes.github.io/GaussianCouplingMessagePassingRules.jl/stable/>.
  `make docs` builds it locally, into `docs/build`.
- Tests: `make test` runs the suite as CI does, ending with the rule-coverage gate: every rule
  must be selected by some test. `make test test_args="name:rules"` selects items by tag, by
  name or by file; `make help` lists the other targets.
- Depends on MessagePassingRulesBase, BayesBase, ExponentialFamily and Distributions. Julia 1.10
  or later. MIT licence.
