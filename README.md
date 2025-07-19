# Automated Time Series Modelling for Count Data

Welcome to the official repository for the research and development from my Master's dissertation in Computational Engineering, **"Automatic Order Selection in Models for Time Series of Counts"**. This project was born out of a challenge in time series analysis: the complex and often subjective task of selecting the correct model orders for count data.

## 📜 The Problem & The Solution

Traditional time series modeling, especially for count data using **Integer-valued Generalised Autoregressive Conditional Heteroskedasticity (INGARCH)** models, requires significant manual effort and expertise. The process is often laborious, relying on trial-and-error and diagnostic interpretation.

Inspired by the success of the `auto.arima()` function for ARIMA models, this dissertation introduces the **'Automated Count Time Series' (ACTS) algorithm**. The algorithm is delivered as a powerful and user-friendly R function: `auto.ingarch()`.

The `auto.ingarch()` function automates the entire order selection process for INGARCH models, systematically finding the optimal `p` and `q` orders by adapting the core principles of the celebrated Hyndman-Khandakar algorithm.

## 🚀 How to Use `auto.ingarch`

The `auto.ingarch()` function is designed to be intuitive. It takes your count time series data and automatically finds the best INGARCH(p,q) model based on a chosen information criterion (like AIC).

### Main Arguments

* `y`: Your time series object (must be a vector or `ts` object of non-negative integers).
* `xreg` (optional): A matrix of external regressor variables.
* `stepwise` (optional): A boolean flag to determine the search strategy.
    * `TRUE` (default): Uses an efficient **stepwise search** that intelligently navigates the model space to find a locally optimal model quickly.
    * `FALSE`: Performs an exhaustive **grid search**, testing every possible (p,q) combination within the specified limits. This is more computationally intensive but guarantees finding the globally optimal model within the search space.
* `distr` (optional): The conditional distribution for the model. Defaults to `"poisson"`, but `"negbin"` (Negative Binomial) is recommended to handle overdispersed data.
* `link` (optional): The link function for the model. Defaults to `"identity"`, but `"log"` is recommended for flexibility as it ensures the conditional mean remains positive without constraining coefficient signs.

### Example Usage

Here is how you can apply the function to your data.

```R
# Assume 'my_counts' is your time series of counts
# and 'my_covariates' is a matrix of your external variables.

# --- Example 1: Default Stepwise Search ---
# This is the fastest and most common use case.
# We use the Negative Binomial distribution and a log link, which is a robust choice.

stepwise_model <- auto.ingarch(
  y = my_counts,
  xreg = my_covariates,
  distr = "nbinom", # Recommended for overdispersed counts
  link = "log",     # Recommended for flexibility
  stepwise = TRUE   # This is the default
)

# Print the results to see the selected model
print(stepwise_model)

# --- Example 2: Exhaustive Grid Search ---
# Use this if you want to be certain you've found the best model within the
# search space and computation time is not an issue.

grid_search_model <- auto.ingarch(
  y = my_counts,
  xreg = my_covariates,
  distr = "nbinom",
  link = "log",
  stepwise = FALSE # This triggers the grid search
)

# Print the results
print(grid_search_model)
