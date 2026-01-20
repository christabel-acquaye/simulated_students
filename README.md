# Take Out Your Calculators: Estimating the Real Difficulty of Question Items with LLM Student Simulations

This repository contains the data for estimating question difficulty using simulated student performance with open source Large Language Models (LLMs).

## Paper

**[Take Out Your Calculators: Estimating the Real Difficulty of Question Items with LLM Student Simulations](https://arxiv.org/abs/2601.09953)**

## Overview

This project explores predicting question difficulty by simulating a "classroom" of students at different grade levels (4th, 8th, and 12th grade) using LLMs. By prompting the LLM to role-play students with varying proficiency levels, we generate simulated student responses and use these outcomes to fit Item Response Theory (IRT) models.
We compare the estimated difficulty parameters against real-world item-level difficulties from the National Assessment of Educational Progress (NAEP). 

This repository includes the subset of NAEP dataset used in our experiments.

## Contact

For questions or feedback, please contact acquayechristabel@gmail.com.

## Acknowledgments

We acknowledge and cite the **[U.S. Department of Education. Institute of Education Sciences, National Center for Education Statistics, National Assessment of Educational Progress (NAEP), Assessment](https://www.nationsreportcard.gov)** for providing the real-world item statistics used for validation in this work. This study uses released items from the National Assessment of Educational Progress (NAEP). Item text, scoring rubrics, and performance data were identified via the **[NAEP Questions Tool](https://nces.ed.gov/nationsreportcard/nqt/)**. All analyses presented here are the responsibility of the authors and do not necessarily reflect the views of the NCES or the U.S. Department of Education.
