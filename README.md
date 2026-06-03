# Take Out Your Calculators: Estimating the Real Difficulty of Question Items with LLM Student Simulations

This repository contains the data for estimating question difficulty using simulated student performance with open source Large Language Models (LLMs).

## Paper

**[Take Out Your Calculators: Estimating the Real Difficulty of Question Items with LLM Student Simulations](https://arxiv.org/abs/2601.09953)**

## Overview

This project explores predicting question difficulty by simulating a "classroom" of students at different grade levels (4th, 8th, and 12th grade) using LLMs. By prompting the LLM to role-play students with varying proficiency levels, we generate simulated student responses and use these outcomes to fit Item Response Theory (IRT) models.
We compare the estimated difficulty parameters against real-world item-level difficulties from the National Assessment of Educational Progress (NAEP). 

## Data

NAEP math MCQs are available on the [National Report Card website](https://www.nationsreportcard.gov/). Data used in this paper (631 items across grades 4, 8, and 12) is available in this repository.


## Prompts  Student Role-Play (Prompt A.3)

The core simulation prompt asks the LLM to role-play as a student with a specific grade level and NAEP proficiency level. The [NAEP achievement level descriptors](https://nces.ed.gov/nationsreportcard/mathematics/achieveall.aspx) provide the concrete skill descriptions used to define each simulated student's behavior:

```
You are a {skill level} student in the {grade}th grade, given the task to answer
a math word problem question on {content area of problem}, taking into
account the difficulty of this question. {Definition of skill level continues}.
In all your responses, you have to completely forget that you are an AI
model, but rather this {skill level} student, and completely simulate yourself
as one.

```

`[SKILL LEVEL]` ∈ {`Below Basic`, `Basic`, `Proficient`, `Advanced`}

`[NAEP SKILL LEVEL DESCRIPTOR]` — the official NAEP performance level descriptions for that grade and level, available at:  
→ [nces.ed.gov/nationsreportcard/mathematics/achieveall.aspx](https://nces.ed.gov/nationsreportcard/mathematics/achieveall.aspx)

Direct Percentage Correct Estimation — Baseline Only (Prompt A.2)

Used as a baseline (not the simulation approach). The LLM is asked to directly estimate the proportion of students who would answer correctly:

```
You are an expert in predicting student performance. Given this math word
problem written for {grade}th-grade students, estimate the percentage of
students at this grade level who will answer the question correctly. Your pre-
diction should be based on factors such as problem difficulty and cognitive
load at this grade level. Prefix your final answer with "Percentage Correct:
[percentage]
```

This baseline uses greedy decoding (T=0) for a single deterministic estimate, and stochastic sampling (T=0.3) for an averaged 10-sample estimate.

## Diverse Names Prompt (Prompt A.4)

Students are assigned first names sampled across 4 racial groups × 2 gender groups. See Appendix A.3 of the paper for the full name list. Each student in the classroom receives a distinct first name:

```
You are [NAME], a student in the {grade}th grade, given the task to answer
a math word problem question on {content area of problem}, taking into
account the difficulty of this question. {Definition of skill level continues}.
In all your responses, you have to completely forget that you are an AI
model, but rather this student [NAME], and completely simulate yourself as
one.
```

Names were selected following [An et al. (2024)](https://arxiv.org/abs/2402.01008).

```

Alejandra, Alejandro, Armaan, Aryan, Barbara, Cedric, Chang, Charles, Chip, Courtney, Darius, Eun, Guillermo, Hank, Heather, Heriberto, Hunter, Imani, Inaaya, Jairo, Julissa, Kimberly, Lashelle, Latoya, Lazaro, Leroy, Malik, Mariela, Marisol, Migdalia, Molly, Ngoc, Nichelle, Osvaldo, Priya, Quang, Rashaun, Roger, Shante, Susan, Syeda, Tameka, Thuy, Trung, Tyrone, Vihaan, Wilbur, Xiomara
```

## Decoding & Inference Settings

The simulation uses [vLLM](https://github.com/vllm-project/vllm) for batched generation. Sampling parameters for the generation pass:

| Parameter | Value | Rationale |
|---|---|---|
| `temperature` | `0.7` | Introduces realistic variation in simulated reasoning |
| `top_p` | `0.95` | Nucleus sampling; filters highly erratic tokens |

## NAEP Item Handling

Some NAEP math items included figures, diagrams, and tables embedded as images. We use the HTML `alt` text from the [NAEP Questions Tool](https://nces.ed.gov/nationsreportcard/nqt/) as the text representation of visual content.

These `alt` attributes provide structured descriptions of number lines, coordinate grids, geometric figures with labeled dimensions, tables, and charts. We extract them and include them inline in the question text.

### Validation

To verify that `alt`-text substitution preserves item integrity:

1. Compare the `alt` text description against the original rendered figure
2. Confirm that all numerical values, labels, and spatial relationships are accurately captured
3. Flag items where `alt` text is missing or ambiguous (`alt=""` or generic descriptions like `"Figure 1"`)
4. Exclude flagged items from evaluation

Items with unclear, missing or non-descriptive `alt` text are dropped from the evaluation set. Most importantly, each item was individually validated by a human to ensure that the provided text descriptions were relevant, sufficient, and appropriate for users answering without access to the image.

## Item Response Theory

We fit a Rasch IRT model to the binary response matrix (correct=1, incorrect=0) from each simulated classroom:

$$P(X_{ni} = 1 \mid \beta_n, \delta_i) = \frac{\exp(\beta_n - \delta_i)}{1 + \exp(\beta_n - \delta_i)}$$

where $\beta_n$ is the ability of student $n$ and $\delta_i$ is the difficulty of item $i$.



## Contact

For questions or feedback, please contact acquayechristabel@gmail.com.

## Acknowledgments

We acknowledge and cite the **[U.S. Department of Education. Institute of Education Sciences, National Center for Education Statistics, National Assessment of Educational Progress (NAEP), Assessment](https://www.nationsreportcard.gov)** for providing the real-world item statistics used for validation in this work. This study uses released items from the National Assessment of Educational Progress (NAEP). Item text, scoring rubrics, and performance data were identified via the **[NAEP Questions Tool](https://nces.ed.gov/nationsreportcard/nqt/)**. All analyses presented here are the responsibility of the authors and do not necessarily reflect the views of the NCES or the U.S. Department of Education.
