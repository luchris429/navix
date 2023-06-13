# NAVIX

[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![CI](https://github.com/epignatelli/navix/actions/workflows/CI.yml/badge.svg)](https://github.com/epignatelli/navix/actions/workflows/CI.yml)
[![CD](https://github.com/epignatelli/navix/actions/workflows/CD.yml/badge.svg)](https://github.com/epignatelli/navix/actions/workflows/CD.yml)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/epignatelli/navix?color=%23216477&label=Release)

**[Quickstart](#what-is-navix)** | **[Installation](#installation)** | **[Examples](#examples)** | **[Cite](#cite)**

## What is NAVIX?
NAVIX is [minigrid](https://github.com/Farama-Foundation/Minigrid) in JAX, **~2000x** faster with Autograd and XLA support.
You can see a superficial performance comparison [here](docs/profiling.ipynb).


## Installation
We currently support the OSs supported by JAX.
You can find a description [here](https://github.com/google/jax#installation).

You might want to follow the same guide to install jax for your faviourite accelerator
(e.g. [CPU](https://github.com/google/jax#pip-installation-cpu),
[GPU](https://github.com/google/jax#pip-installation-gpu-cuda-installed-locally-harder), or
[TPU](https://github.com/google/jax#pip-installation-colab-tpu)
).

Then, install `navix` and its dependencies with:
```bash
pip install navix
```

---
## Examples

### XLA compilation
One straightforward use case is to accelerate the computation of the environment with XLA compilation.
For example, here we vectorise the environment to run multiple environments in parallel, and compile **the full training run**.

You can find a partial performance comparison with [minigrid](https://github.com/Farama-Foundation/Minigrid) in the [docs](docs/profiling.ipynb).

```python
import jax
import navix as nx


def run(seed)
  env = nx.environments.Room(16, 16, 8)
  key = jax.random.PRNGKey(seed)
  timestep = env.reset(key)
  actions = jax.random.randint(key, (N_TIMESTEPS,), 0, 6)

  def body_fun(timestep, action):
      timestep = env.step(timestep, jnp.asarray(action))
      return timestep, ()

  return jax.lax.scan(body_fun, timestep, jnp.asarray(actions, dtype=jnp.int32))[0]

final_timestep = jax.jit(jax.vmap(run))(jax.numpy.arange(1000))
```

### Backpropagation through the environment

Another use case it to backpropagate through the environment transition function, for example to learn a world model.

TODO(epignatelli): add example.


## Cite
If you use `helx` please consider citing it as:

```bibtex
@misc{pignatelli2023navix,
  author = {Pignatelli, Eduardo},
  title = {Navix: Accelerated gridworld navigation with JAX},
  year = {2023},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/epignatelli/navix}}
  }
```
