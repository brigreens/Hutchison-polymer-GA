# Polymer-GA

This repository contains the code for the Hutchison Group's polymer genetic algorithm (GA). This GA will perform crossover, mutation, etc. on oligomers of 2 monomers with a length of 6. Any sequence is allowed. The population size, selection method, mutation rate, elitism percentage, and convergence criteria can be controlled via the command line.

There are 2 types of GA's here. 
1. `GA_code/GA_main_orig.py`, should be used when fitness evaluation is quick (xTB, RDKit, etc.). The calculations will be performed within the GA.
2. `GA_code_cron/GA_cron.py`, should be used if your fitness function requires lengthy calculations such as DFT, each generation should be submitted one at a time to wait until the calculations are done to proceed. This can be automated with cron. Please manually submit the first 3 generations before using cron to ensure no bugs.

The fitness function can be changed for your desired scoring property. Please edit the `scoring.py` files to edit for your desired property and file paths. If using cron, update the file paths in the slurm script too. The current fitness function is to calculate polarizability with GFN2-xTB. The goal of the GA is to maximize this property. If your GA is required to minimize a score, you will need to edit score to 1/x. 

# How to run the GA

### Without cron
To run the GA WITHOUT cron: `cd GA_code && python GA_main_orig.py --name <GA name> --run_label <A, B, C, etc..> [OPTIONS]`
- `--name`: name of GA, make it unique for each run
- `--run_label`: Sets the initial random state and allows for replication. Can be set as 'A', 'B', 'C', etc.
- `[OPTIONS]`
  - `--restart` use this flag if GA crashed and you want to restart GA from last run genertion 
  - `--init_restart` use this flag if you are restarting the GA from the first generation
  - `param_type`
    - `--pop_size`: This is the size of the populations. The default is 32 oligomers.
    - `--selection_method`: Can be 'random', k-way tournament with k=2 as'tournament_2', k=3 as 'tournament_3', and k=4 as'tournament_4', 'roulette', 'rank', or stochastic universal sampling as 'SUS'. Default is 'tournament_3'.
    - `--mutation_rate`: Sets the chance each oligomer in a generation will undergo mutation. Can be between 0 and 1. Default is 0.4.
    - `--elitism_perc`: Percentage of the top oligomers that will pass to the next generation. Can be between 0 and 1. Default is 0.5.
    - `--spear_thresh`: Spearman coefficient threshold - spearman coefficent must be greater than this value to trip the convergence counter. Default is 0.8.
    - `--conv_gen`: Convergence threshold - # of consecutive generations that need to meet Spearman coefficient threshold before termination. Default is 50.

Example usage: `cd GA_code && python GA_main_orig.py --name GA_polar_popsize_50 --run_label A param_type --pop_size 50`
 
### With cron
To run the GA WITH cron: `cd GA_code && python GA_main_orig.py --name <GA name> --run_label <A, B, C, etc..> [OPTIONS]`
- `--name`: name of GA, make it unique for each run
- `--run_label`: Sets the initial random state and allows for replication. Can be set as 'A', 'B', 'C', etc.
- `[OPTIONS]`
  - `--first_run` If this is the first time running cron, use this flag
  - `param_type`
    - `--pop_size`: This is the size of the populations. The default is 32 oligomers.
    - `--selection_method`: Can be 'random', k-way tournament with k=2 as'tournament_2', k=3 as 'tournament_3', and k=4 as'tournament_4', 'roulette', 'rank', or stochastic universal sampling as 'SUS'. Default is 'tournament_3'.
    - `--mutation_rate`: Sets the chance each oligomer in a generation will undergo mutation. Can be between 0 and 1. Default is 0.4.
    - `--elitism_perc`: Percentage of the top oligomers that will pass to the next generation. Can be between 0 and 1. Default is 0.5.
    - `--spear_thresh`: Spearman coefficient threshold - spearman coefficent must be greater than this value to trip the convergence counter. Default is 0.8.
    - `--conv_gen`: Convergence threshold - # of consecutive generations that need to meet Spearman coefficient threshold before termination. Default is 50.

Example usage for first run: `cd GA_code && python GA_main_orig.py --name GA_polar_popsize_50 --run_label A --first_run param_type --pop_size 50`
Example usage for second run and after: `cd GA_code && python GA_main_orig.py --name GA_polar_popsize_50 --run_label A param_type --pop_size 50`

# SMILES dataset
A csv containing SMILES is supplied (`updated_monomer_list.csv`) and can be changed for your dataset.
