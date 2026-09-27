# Diffusion-Based Rollouts as a Stabilization Mechanism for Long-Horizon Environmental Forecasting

Code accompanying the manuscript:

**Diffusion-Based Rollouts as a Stabilization Mechanism for Long-Horizon Environmental Forecasting**

Marina Vicens-Miquel, Amy McGovern, Aaron J. Hill, Efi Foufoula-Georgiou, and Samuel S. P. Shen

*Submitted to Geophysical Research Letters.*

## Overview

This repository contains code supporting the experiments and evaluation presented in the manuscript.

The study investigates diffusion-based rollouts as a stabilization mechanism for long-horizon recursive environmental forecasting across two contrasting forecasting settings:

- **Low-dimensional water-level time series:** NOAA tide-gauge water-level forecasts at Bob Hall Pier, Port Isabel, and Rockport.
- **High-dimensional precipitation fields:** 1-km precipitation forecasts over Oklahoma, Washington, D.C., and Oregon.

The experiments compare deterministic and diffusion-based recursive forecasting using matched predictive architectures and input configurations within each forecasting modality.

The central goal is to evaluate whether diffusion-based forecasting can suppress recursive error amplification and how this stabilization relates to forecast fidelity as lead time increases.

## Main Findings

The experiments show that diffusion can moderate recursive error growth across both forecasting modalities.

However, rollout stability and forecast fidelity are distinct properties. In the water-level experiments, forecasts progressively lose access to external predictive information as the rollout becomes fully autoregressive. Diffusion can remain numerically stable under these conditions while forecast variability and event-level fidelity deteriorate.

In the precipitation experiments, advancing HRRR forecasts provide external predictive information throughout the rollout. In this setting, diffusion better preserves precipitation spatial organization and event-detection skill, particularly where deterministic rollouts degrade rapidly.

These results indicate that the practical benefit of diffusion-based stabilization depends on both recursive instability and the predictive information available during the rollout.

## Datasets

### Water-Level Time Series

The low-dimensional experiments use cleaned and gap-filled NOAA tide-gauge water-level datasets introduced in:

> Vicens-Miquel, M., Tissot, P. E., & Medrano, F. A. (2024).  
> *Exploring Deep Learning Methods for Short-Term Tide Gauge Water Level Predictions.*  
> Water, 16(20), 2886.

Data and associated resources are available from the `waterLevelJournal` repository.

The experiments use three Texas Gulf Coast stations:

- Bob Hall Pier
- Port Isabel
- Rockport

The models predict surge, defined as observed water level minus the harmonic tidal prediction.

### Precipitation

The high-dimensional experiments use:

- NOAA Multi-Radar Multi-Sensor (MRMS) precipitation observations
- High-Resolution Rapid Refresh (HRRR) forecasts

The dataset construction and preprocessing follow:

> Vicens-Miquel, M., McGovern, A., Hill, A. J., Foufoula-Georgiou, E., Guilloteau, C., & Shen, S. S. P. (2026).  
> *A Diffusion-Based Framework for High-Resolution Precipitation Forecasting over CONUS.*

Three 512 × 512 regions at 1-km spatial resolution are evaluated:

- Oklahoma
- Washington, D.C.
- Oregon

## Forecasting Framework

### Deterministic Models

The water-level experiments use a multilayer perceptron (MLP), while the precipitation experiments use a dilated attention U-Net.

### Diffusion Models

Diffusion experiments use the **Elucidated Diffusion Model (EDM)** framework.

Within each forecasting modality, the deterministic and diffusion configurations use the same predictive backbone. The diffusion models differ through noise-conditioned training and iterative denoising during inference.

## Recursive Rollouts

Multi-step forecasts are generated recursively by incorporating model predictions into the input state for subsequent forecasts.

For the **water-level experiments**, predicted surge values progressively replace observed surge values until the dynamic input state consists entirely of previous model predictions. The forecasts therefore become fully autoregressive without access to future meteorological forcing.

For the **precipitation experiments**, predicted precipitation fields are recursively incorporated into subsequent inputs while HRRR conditioning is advanced in forecast lead time. The precipitation forecasts therefore retain external forecast information throughout the rollout.

## Evaluation

Forecast performance is evaluated as a function of rollout lead time.

### Water Level

- Mean Absolute Error (MAE)
- Relative error growth
- Forecast trajectory behavior

### Precipitation

- Mean Absolute Error (MAE)
- Fractions Skill Score (FSS)
- Critical Success Index (CSI)
- Relative degradation with lead time

The evaluation considers both numerical stabilization and preservation of meaningful forecast structure.



## Code Availability

The code supporting this study is publicly available in this repository.
