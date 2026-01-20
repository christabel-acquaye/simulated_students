# Take Out Your Calculators: Estimating the Real Difficulty of Question Items with LLM Student Simulations

This repository contains the data for estimating question difficulty using simulated student performance with open source Large Language Models (LLMs).

## Paper

**[Take Out Your Calculators: Estimating the Real Difficulty of Question Items with LLM Student Simulations](https://arxiv.org/abs/2601.09953)**

## Overview

This project explores predicting question difficulty by simulating a "classroom" of students at different grade levels (4th, 8th, and 12th grade) using LLMs. By prompting the LLM to role-play students with varying proficiency levels, we generate simulated student responses and use these outcomes to fit Item Response Theory (IRT) models.
We compare the estimated difficulty parameters against real-world item-level difficulties from the National Assessment of Educational Progress (NAEP). 
### Key Features

- **Simulated Student Responses**: LLMs role-play students at different proficiency levels across multiple grade levels
- **IRT Model Fitting**: Uses simulated performance data to estimate item difficulty parameters
- **Validation with Real Data**: 


This repository includes the dataset used in our experiments.


## Contact

For questions or feedback, please open an issue or contact acquayechristabel@gmail.com.

## Acknowledgments

We acknowledge the National Assessment of Educational Progress (NAEP) for providing the real-world item statistics used for validation in this work.
