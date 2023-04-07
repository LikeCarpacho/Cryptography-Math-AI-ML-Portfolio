# Vanilla Policy Gradient (VPG) for CartPole-v0

This code includes modifications from the spinningup repository:
[Spinning Up](https://github.com/openai/spinningup)

## Project Description

This project is an implementation of the Vanilla Policy Gradient (VPG) algorithm to solve the CartPole-v0 environment from OpenAI Gym. The VPG algorithm is a foundational reinforcement learning method that uses policy gradients to update an agent's policy directly. The agent is represented by a neural network with a customizable number of hidden layers and hidden units per layer.

### Main Components

- `mlp` function: Creates a multilayer perceptron (MLP) with specified layer sizes and activation functions.
- `train` function: Trains an agent in the CartPole-v0 environment using the VPG algorithm.
- `save_agent` function: Saves the trained agent to a file.
- `load_agent` and `load_latest_agent` functions: Load a saved agent from a file.
- `evaluate_agent` function: Evaluates the performance of a trained agent in the CartPole-v0 environment.
- `save_plot` function: Plots episodes per epoch during training or in an evaluation run.
- Training loop in the `if __name__ == '__main__'` block: Trains multiple agents with randomly generated hidden layer sizes and numbers of hidden layers, and saves them.
- `parse_config`, `create_logits_net`, `get_agent_results`, and `save_best_configs_to_csv` functions: These functions are used to test the saved agents, plot their evaluation results, and save the best-performing configurations to a file.

Results and best-performing agents can be viewed in `best_configurations.csv`.

## Dependencies

- Python 3.7+
- PyTorch
- OpenAI Gym
- NumPy
- Matplotlib
- imageio
- pyvirtualdisplay (optional, for rendering on a headless system)
- os
- glob
- pandas
- numpy
- re

## Usage

1. Ensure that all dependencies are installed.
2. Run `main.py` to train multiple agents with randomly generated hidden layer sizes and numbers of hidden layers. The trained agents and their training plots will be saved in the 'trained_agents' folder.
3. Run the following code block to load and evaluate the trained agent, by the name of the agent 'file', in the folder's path called 'root':

```python
import os
import torch
import pandas as pd
import numpy as np

env_name = 'CartPole-v0'
env = gym.make(env_name)
obs_dim = env.observation_space.shape[0]
n_acts = env.action_space.n
filepath = os.path.join(root, file)
config = os.path.basename(root)
hidden_size, num_hidden_layers = parse_config(config)
logits_net = create_logits_net(obs_dim, n_acts, hidden_size, num_hidden_layers)
agent = load_agent(logits_net, filepath)
evaluate_agent(env, agent)
```

To evaluate the best-performing agents, run the code block provided in the 'best_configs' section of the get_agent_results and save_best_configs_to_csv functions.
## License 
This project is licensed under the MIT : [License](../../../LICENSE). See the LICENSE file for details.