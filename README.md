# Explore, Discover and Learn: Unsupervised Discovery of State-Covering Skills
Authors: Víctor Campos, Alexander Trott, Caiming Xiong, Richard Socher, Xavier Giro-i-Nieto, Jordi Torres

Link to paper: https://arxiv.org/abs/2002.03647


## Abstract

Acquiring abilities in the absence of a task-oriented reward function is at the frontier of reinforcement learning research. 
This problem has been studied through the lens of empowerment, which draws a connection between option discovery and information theory. 
Information-theoretic skill discovery methods have garnered much interest from the community, but little research has been conducted in understanding their limitations. 
Through theoretical analysis and empirical evidence, we show that existing algorithms suffer from a common limitation -- they discover options that provide a poor coverage of the state space. 
In light of this, we propose 'Explore, Discover and Learn' (EDL), an alternative approach to information-theoretic skill discovery. 
Crucially, EDL optimizes the same information-theoretic objective derived from the empowerment literature, but addresses the optimization problem using different machinery. 
We perform an extensive evaluation of skill discovery methods on controlled environments and show that EDL offers significant advantages, 
such as overcoming the coverage problem, reducing the dependence of learned skills on the initial state, 
and allowing the user to define a prior over which behaviors should be learned.


## Code

This code provides an implementation of EDL and can be used to run the experiments presented in the paper. 
Experiments are run using Python 3.5.2 and the packages listed in `requirements.txt`.

### Running experiments

We extend the [Sibling Rivalry codebase](https://github.com/salesforce/sibling-rivalry). 
The pieces of the original Sibling Rivalry code that require a valid mujoco license are disabled by default, 
and can be enabled with `export WITH_MUJOCO=1`. 

To run a Reinforcement Learning experiment, use the following command:
```
python main.py --config-path <PATH.json> --log-dir <LOG DIRECTORY PATH> --dur <NUM EPOCHS TO TRAIN> --N <NUM WORKERS>
```

Similarly, use the following command to train a VQ-VAE:
```
python train_vqvae.py --config-path <PATH.json> --log-dir <LOG DIRECTORY PATH> --dur <NUM ITERATIONS TO TRAIN> 
```

The type of experiment and algorithm are controlled by the settings provided in the config file (json).
Several example configuration files are provided in `examples/`.
A directory corresponding to the name of the config file will be created inside of the log directory (second argument), and results will be stored there.


### Inspecting results

We provide Jupyter Notebooks that allow to visualize the trained agents. By default, they assume that all results are stored in `logs/`.
The location of `logs/` can be set through an environment variable, `ROOT_DIR`.


### Example: replicating results on the Square Maze

The provided configuration files can be used to train the baselines and EDL on the Square Maze:

```
# Baseline: Forward MI
python main.py --config-path examples/square_maze/forward_mi.json --log-dir logs/square_maze --dur 50 --N 1

# Baseline: Reverse MI
python main.py --config-path examples/square_maze/reverse_mi.json --log-dir logs/square_maze --dur 50 --N 1

# EDL using sample from an oracle and Sibling Rivalry
python train_vqvae.py --config examples/square_maze/vqvae_oracle.json --log-dir logs/square_maze/ --dur 50000
python main.py --config-path examples/square_maze/edl_sr_oracle.json --log-dir logs/square_maze --dur 50 --N 1

# EDL using samples from SMM and Sibling Rivalry
python main.py --config-path examples/square_maze/smm.json --log-dir logs/square_maze --dur 50 --N 1
python train_vqvae.py --config examples/square_maze/vqvae_smm.json --log-dir logs/square_maze/ --dur 50000
python main.py --config-path examples/square_maze/edl_sr_smm.json --log-dir logs/square_maze --dur 50 --N 1
```

We provide logs and checkpoints for agents trained with the example configuration files.
These can be extracted with the following command:
```
tar -xf logs/square_maze.tar.gx -C logs/
```

Results can be inspected using the Jupyter Notebooks under `notebooks/`. 
We recommend using [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) with support for [ipywidgets](https://ipywidgets.readthedocs.io/en/latest/user_install.html) for this purpose:
```
jupyter lab notebooks/
```
