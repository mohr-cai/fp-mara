# Financial Planning Framework
The fp-mara financial planning framework utilizes advanced technologies such as reinforcement learning, OpenAI Gymnasium, and Monte Carlo simulations to optimize and simulate financial planning scenarios effectively. Here's an in-depth look at these components of my fun project:

## Reinforcement Learning Approach

### Overview
Reinforcement learning in the fp-mara framework involves training an agent to make optimal financial decisions, such as how much to save or invest at different stages of life to maximize financial health. The agent learns from interactions with the financial environment, receiving rewards based on its performance relative to financial goals.

### Implementation
- **Agent**: The agent represents a user or an automated system that decides the proportion of income to save or invest.
- **Environment**: The `PlanEnv` class, based on OpenAI's Gymnasium interface, serves as the environment. It simulates the progression of a user's financial status over time, taking into account factors like investment returns, inflation, and personal financial goals.
- **State**: The state in this environment could include current savings, age, remaining years to retirement, and other pertinent financial indicators.
- **Actions**: Actions might range from the percentage of annual income saved to choices between different types of investments.
- **Rewards**: The reward function provides feedback based on how close the agent is to achieving its financial goals, encouraging strategies that increase the probability of meeting or exceeding these goals.

### Learning Process
Through a series of episodes, the RL agent experiments with different actions, learning from past rewards to refine its strategy. This iterative learning process is aimed at discovering a policy that optimizes long-term returns and stability.

## Gymnasium from OpenAI

The framework utilizes OpenAI’s Gymnasium to define and manage the financial planning environment (`PlanEnv`). Gymnasium is a toolkit for developing and comparing reinforcement learning algorithms. It provides a standard API for interacting with the agent and the environment, simplifying the process of implementing RL algorithms.

- **Custom Environments**: `PlanEnv` is a custom environment that models financial scenarios. This custom setup allows the integration of complex financial dynamics that are specific to personal financial planning.

## Monte Carlo Simulation

To assess the robustness of financial strategies over a range of possible future scenarios, the framework employs Monte Carlo simulation techniques.

### Usage
- **Scenario Analysis**: Monte Carlo simulations are used to generate a wide range of potential future financial outcomes based on random samples of return rates and other variables.
- **Risk Assessment**: This method helps in understanding the probability of reaching financial goals under varying conditions, thus providing a statistical basis for risk assessment and decision-making.

### Integration in fp-mara
In the `savings_fixed_policy` function, Monte Carlo simulation is implemented to project savings over multiple scenarios, each representing a different trajectory of investment returns and inflation rates. These projections help in visualizing potential outcomes and deciding on the best course of action under uncertainty.


## Installation

Before using the fp-mara framework, ensure you have the necessary dependencies installed:

```bash
poetry install
poetry shell
```

## Setup Agent

```python
from src.util import Profile

user_profile = Profile(
    current_age=30,
    goal_amount=1_000_000,
    target_year=30,
    current_balance=100_000,
    current_income=50_000,
    inflation_factor=0.02,
    portfolio_return=0.05,
    portfolio_stdv=0.1,
    inflation_adjust=True
)
```

## Running the Simulation

Import the necessary libraries and instantiate the environment with your user profile:

```python
import gymnasium as gym
import numpy as np
import pandas as pd
from src.env import PlanEnv
from src.util import create_one_hot, Profile, savings_fixed_policy
import src.plots as plots

# Number of simulations
n_sim = 10000
# Create an array to store simulation results
savings_simulations = np.zeros([n_sim, target_year])

# Set up multiple seeds for the simulations to ensure variability
seeds = np.arange(n_sim)

# Run simulations
for sim in range(n_sim):
    # Initialize the user profile for each simulation
    user_profile = Profile(
        current_age=example_age,  # Start age of the user
        goal_amount=example_goal_amount,  # Financial goal to reach by the end of the target year
        target_year=example_target_year,  # Number of years from now to reach the financial goal
        current_balance=example_starting_balance,  # Current savings or investment balance
        current_income=example_annual_income,  # Annual income expected to be invested
        inflation_factor=example_inflation_rate,  # Estimated annual inflation rate
        portfolio_return=example_expected_return,  # Expected return rate from the investment portfolio
        portfolio_stdv=example_return_std_dev,  # Standard deviation of the return on investment
        inflation_adjust=True  # Whether the calculations should adjust for inflation
    )
    # Run the fixed policy simulation for the given profile and seed
    savings_simulations[sim] = savings_fixed_policy(user_profile, contrib=0.1, seed=seeds[sim])

# After simulations, analyze the results
final_user_profile = Profile(
    current_age=final_example_age,
    goal_amount=final_example_goal_amount,
    target_year=final_example_target_year,
    current_balance=final_example_starting_balance,
    current_income=final_example_annual_income,
    inflation_factor=final_example_inflation_rate,
    portfolio_return=final_example_expected_return,
    portfolio_stdv=final_example_return_std_dev,
    inflation_adjust=True
)
year = [i + final_user_profile.current_age for i in list(range(final_user_profile.target_year))]
df = pd.DataFrame(savings_simulations)
df.columns = year
plots.plot_simulation_percentiles(final_user_profile, savings_simulations)
```