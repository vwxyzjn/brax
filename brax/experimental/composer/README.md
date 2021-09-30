# Braxlines Composer

<img src="https://github.com/google/brax/raw/main/docs/img/composer/ant_push.gif" width="150" height="107"/><img src="https://github.com/google/brax/raw/main/docs/img/composer/ant_chase.gif" width="150" height="107"/><img src="https://github.com/google/brax/raw/main/docs/img/composer/pro_ant2.gif" width="150" height="107"/><img src="https://github.com/google/brax/raw/main/docs/img/composer/pro_ant1.gif" width="150" height="107"/>

Braxlines Composer allows modular composition of environments.
The composed environment is compatible with training algorithms in
[Brax](https://github.com/google/brax) and
[Braxlines](https://github.com/google/brax/tree/main/brax/experimental/braxlines). See:
* [composer.py](https://github.com/google/brax/tree/main/brax/experimental/composer/composer.py) for descriptions of the API
* [observers.py](https://github.com/google/brax/tree/main/brax/experimental/composer/observers.py) for observation definition utilities
* [reward_functions.py](https://github.com/google/brax/tree/main/brax/experimental/composer/reward_functions.py) for reward definition utilities
* [env_descs.py](https://github.com/google/brax/tree/main/brax/experimental/composer/env_descs.py) for examples of environment composition configs


## API Example

*This is an illustrative example. For full examples, see [env_descs.py]((https://github.com/google/brax/tree/main/brax/experimental/composer/env_descs.py)*

```python
from brax.experimental.composer import composer

env_desc = dict(
    components=dict(  # component information
        agent1=dict(
            component='pro_ant',  # components/pro_ant.py
            component_params=dict(num_legs=6)),  # 6 legs
        cap1=dict(
            component='singleton',  # components/singleton.py
            component_params=dict(size=0.5),  # adjust object size
            pos=(1, 0, 0),  # where to place a capsule object
            reward_fns=dict(
                goal=dict(  # reward1: a target velocity for the object
                    reward_type='root_goal',
                    sdcomp='vel',
                    target_goal=(4, 0, 0))))),
    edges=dict(  # edge information
        agent1__cap1=dict(
            extra_observers=[  # add agent-object position diff as an extra obs
                dict(observer_type='root_vec')],
            reward_fns=dict(  # reward2: make the agent close to the object
                dist=dict(reward_type='root_dist')),),))
env = composer.create(env_desc=env_desc)
```

## Colab Notebooks

Explore Composer easily and quickly through:
* [Composer Basics](https://colab.research.google.com/github/google/brax/blob/main/notebooks/composer/composer.ipynb) dynamically composes an environment and trains it using PPO within a few minutes.
* [Experiment Sweep](https://colab.research.google.com/github/google/brax/blob/main/notebooks/braxlines/experiment_sweep.ipynb) provides a basic example for running a hyperparameter sweep. Set `agent_module`=`composer`.
* [Experiment Viewer](https://colab.research.google.com/github/google/brax/blob/main/notebooks/braxlines/experiment_viewer.ipynb) provides a basic example for visualizing results from a hyperparameter sweep.

Tips:
* for debugging, use:
```python
from jax.config import config
config.update("jax_debug_nans", True)
```

## Learn More

<img src="https://github.com/google/brax/raw/main/docs/img/braxlines/sketches.png" width="540" height="220"/>

For a deep dive into Composer, please see
our paper, [Braxlines: Fast and Interactive Toolkit for RL-driven Behavior Generation Beyond Reward Maximization](https://openreview.net/forum?id=-W0LCm8wE2S).

## Citing Composer

If you would like to reference Braxlines in a publication, please use:

```
@article{gu2021braxlines,
  title={Braxlines: Fast and Interactive Toolkit for RL-driven Behavior Generation Beyond Reward Maximization},
  author={Gu, Shixiang Shane and Diaz, Manfred and Freeman, C Daniel and Furuta, Hiroki and Ghasemipour, Seyed Kamyar Seyed and Raichuk, Anton and David, Byron and Frey, Erik and Coumans, Erwin and Bachem, Olivier},
  year={2021}
}
@software{brax2021github,
  author = {C. Daniel Freeman and Erik Frey and Anton Raichuk and Sertan Girgin and Igor Mordatch and Olivier Bachem},
  title = {Brax - A Differentiable Physics Engine for Large Scale Rigid Body Simulation},
  url = {http://github.com/google/brax},
  version = {0.0.5},
  year = {2021},
}
```