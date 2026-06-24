# Awesome-MPC-RL

[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com)

A curated list of papers and tools on combining reinforcement learning with model-based control, including MPC, optimal control, differentiable control, and GPU-parallel planning.

The primary application domain is robotics, including locomotion, manipulation, and contact-rich control. This list is intentionally small in v1: the goal is a high-signal research map, not an exhaustive paper dump.

## Core Questions

- How can model-based controllers serve as policies, planners, teachers, or optimization layers for reinforcement learning?
- How can reinforcement learning improve model-based controllers without replacing their structure?
- When should the combination happen at training time, test time, or both?
- How does GPU-parallel model-based control change the scaling behavior of MPC, optimal control, and planning?
- Which libraries, solvers, benchmarks, and accelerators make these combinations practical in robotics?

## Scope

### In Scope

- MPC and optimal control as policy classes or differentiable modules.
- MPC-guided RL, optimal-control-guided RL, and training-time controller guidance.
- RL or learning modules that augment model-based controllers, such as learned costs, terminal values, residuals, warm starts, or high-level references.
- GPU-parallel MPC, batched optimal control, test-time planning, MPPI, CEM, iLQR, DDP, and related solver scaling.
- Libraries, solvers, benchmarks, and accelerators that directly support the above research lines.

### Out of Scope

- Generic model-based RL and world-model RL, including Dreamer-style or PlaNet-style latent world models.
- Pure model-free RL without a model-based control component.
- Pure safe-control surveys or safety-only papers unless they are strongly tied to MPC/RL combination.
- Generic robotics awesome resources.
- Papers that only mention "MPC", "model-based", or "large-scale" without making the control-learning combination central.

## Why Combine RL with Model-Based Control?

Model-based control brings structure: dynamics models, constraints, receding-horizon optimization, trajectory optimization, and interpretable control objectives. Reinforcement learning brings adaptation: long-horizon task optimization, robustness through large-scale interaction, and learned components that can handle model mismatch or difficult-to-hand-code objectives.

The most interesting work does not simply put these two tools side by side. It asks what role each component should play: MPC can become the policy, a differentiable layer, a training-time teacher, a reward or critic guide, a deployment-time planner, or a GPU-parallel test-time compute module. RL can become an actor, critic, residual policy, cost learner, value learner, warm starter, or high-level command generator.

## Table of Contents

- [Core Questions](#core-questions)
- [Scope](#scope)
- [Why Combine RL with Model-Based Control?](#why-combine-rl-with-model-based-control)
- [Overview and Surveys](#overview-and-surveys)
- [MPC / Optimal Control as Policy](#mpc--optimal-control-as-policy)
- [MPC-Guided and Learning-Augmented Control](#mpc-guided-and-learning-augmented-control)
- [GPU-Parallel MPC and Test-Time Scaling](#gpu-parallel-mpc-and-test-time-scaling)
- [Libraries, Solvers, Benchmarks, and Accelerators](#libraries-solvers-benchmarks-and-accelerators)
- [Contribution Note](#contribution-note)

## Overview and Surveys

| Title | Venue | Year | Role / Finding | Code | Project |
|:------|:-----:|:----:|:---------------|:----:|:-------:|
| <br>[**Synthesis of Model Predictive Control and Reinforcement Learning: Survey and Classification**](https://arxiv.org/abs/2502.02133)<br> | arXiv | 2025 | `[survey]` Classifies how MPC and RL can be combined, with actor-critic RL as a useful organizing lens. | - | - |

## MPC / Optimal Control as Policy

Work where the controller is the policy or a central policy computation module.

| Title | Venue | Year | Role / Finding | Code | Project |
|:------|:-----:|:----:|:---------------|:----:|:-------:|
| <br>[**Differentiable MPC for End-to-end Planning and Control**](https://arxiv.org/abs/1810.13400)<br> | arXiv | 2018 | `[policy]` Uses MPC as a differentiable policy class and learns controller cost and dynamics terms end to end. | [GitHub](https://github.com/locuslab/mpc.pytorch) | [Project](https://locuslab.github.io/mpc.pytorch/) |
| <br>[**Policy Search for Model Predictive Control with Application to Agile Drone Flight**](https://arxiv.org/abs/2112.03850)<br> | T-RO | 2022 | `[policy]` Learns high-level decision variables for a parameterized MPC controller instead of replacing the controller. | [GitHub](https://github.com/uzh-rpg/high_mpc) | - |
| <br>[**Actor-Critic Model Predictive Control: Differentiable Optimization meets Reinforcement Learning**](https://arxiv.org/abs/2306.09852)<br> | arXiv | 2023 | `[policy]` Embeds differentiable MPC in an actor-critic RL loop, combining short-horizon predictive optimization with learned long-horizon value structure. | - | - |

## MPC-Guided and Learning-Augmented Control

Training-time or learning-time combinations where model-based control guides RL, or RL augments a model-based controller.

| Title | Venue | Year | Role / Finding | Code | Project |
|:------|:-----:|:----:|:---------------|:----:|:-------:|
| <br>[**Accelerating and Scaling MPC-Guided Reinforcement Learning for Humanoid Locomotion and Manipulation**](https://arxiv.org/abs/2606.05687)<br> | arXiv | 2026 | `[training-time]` Uses centroidal MPC guidance during RL training and introduces a batched GPU MPC solver for thousands of parallel environments. | [GitHub](https://github.com/junhengl/mpc-rl) | - |
| <br>[**Residual MPC: Blending Reinforcement Learning with GPU-Parallelized Model Predictive Control**](https://arxiv.org/abs/2510.12717)<br> | arXiv | 2025 | `[both]` Keeps MPC as a structured controller while training an RL residual policy to correct model mismatch at the torque-control level. | - | - |
| <br>[**Learning-based MPC from Big Data Using Reinforcement Learning**](https://arxiv.org/abs/2301.01667)<br> | arXiv | 2023 | `[learning-augmented]` Uses RL tools to learn parameterized MPC schemes from data while preserving the MPC controller structure. | - | - |

## GPU-Parallel MPC and Test-Time Scaling

Deployment-time or test-time combinations where model-based control remains active during action selection and benefits from more online compute.

| Title | Venue | Year | Role / Finding | Code | Project |
|:------|:-----:|:----:|:---------------|:----:|:-------:|
| <br>[**CusADi: A GPU Parallelization Framework for Symbolic Expressions and Optimal Control**](https://arxiv.org/abs/2408.09662)<br> | arXiv | 2024 | `[GPU-parallel]` Extends CasADi-style symbolic expressions toward CUDA-parallel optimal control, enabling scalable MPC evaluation inside RL pipelines. | - | - |
| <br>[**Differentiable Model Predictive Control on the GPU**](https://arxiv.org/abs/2510.06179)<br> | arXiv | 2025 | `[GPU-parallel] [differentiable]` Introduces a GPU-accelerated differentiable MPC solver using SQP and custom PCG for RL and domain-randomization tuning of MPC controllers. | [GitHub](https://github.com/ToyotaResearchInstitute/diffmpc) | - |
| <br>[**CA-AC-MPC: CUDA-Accelerated Actor-Critic Model Predictive Control**](https://arxiv.org/abs/2605.29155)<br> | arXiv | 2026 | `[GPU-parallel]` Accelerates actor-critic MPC by moving the differentiable MPC layer toward CUDA execution for lower training and inference latency. | - | - |
| <br>[**Predictive Sampling: Real-time Behaviour Synthesis with MuJoCo**](https://arxiv.org/abs/2212.00541)<br> | arXiv | 2022 | `[test-time]` Introduces MuJoCo MPC as a real-time predictive-control framework with shooting-based planners for behavior synthesis. | [GitHub](https://github.com/google-deepmind/mujoco_mpc) | [Project](https://www.deepmind.com/blog/mujoco-mpc) |

## Libraries, Solvers, Benchmarks, and Accelerators

Supporting tools for model-based control and RL combinations. This is not a general robotics resource list.

| Tool | Type | Role | Language / Stack | Repo |
|:-----|:----:|:-----|:-----------------|:----:|
| **CasADi** | Symbolic modeling / optimization | Symbolic expressions, automatic differentiation, and optimal-control problem construction. | C++, Python, MATLAB/Octave | [GitHub](https://github.com/casadi/casadi) |
| **CusADi** | GPU-parallel optimal-control accelerator | CUDA-oriented acceleration framework for CasADi-style symbolic expressions and optimal control. | CUDA, Python | - |
| **diffmpc** | GPU differentiable MPC accelerator | Differentiable MPC package using SQP and custom PCG for GPU-parallel MPC tuning and benchmarking. | Python, JAX/CUDA | [GitHub](https://github.com/ToyotaResearchInstitute/diffmpc) |
| **acados** | Nonlinear MPC / optimal control | Fast embedded solvers for nonlinear optimal control and nonlinear MPC. | C, Python, MATLAB, CasADi interface | [GitHub](https://github.com/acados/acados) |
| **Crocoddyl** | Robot optimal control | DDP-like optimal-control algorithms for contact-rich robot control. | C++, Python | [GitHub](https://github.com/loco-3d/crocoddyl) |
| **mpc.pytorch** | Differentiable MPC | Fast differentiable MPC solver used as a policy layer in PyTorch. | Python, PyTorch | [GitHub](https://github.com/locuslab/mpc.pytorch) |
| **PyTorch MPPI** | Sampling-based MPC | Parallel/batched MPPI controller with approximate dynamics support. | Python, PyTorch | [GitHub](https://github.com/UM-ARM-Lab/pytorch_mppi) |
| **MuJoCo MPC** | Real-time predictive control | Interactive MPC framework with iLQG, gradient descent, and predictive sampling planners on MuJoCo dynamics. | C++, MuJoCo | [GitHub](https://github.com/google-deepmind/mujoco_mpc) |
| **TinyMPC** | Embedded MPC solver | Small-footprint MPC solver for resource-constrained robots and microcontrollers. | C++, Python | [GitHub](https://github.com/TinyMPC/TinyMPC) |

## Contribution Note

This v1 list is intentionally small. A new entry should be added only if it directly studies or supports the combination of reinforcement learning with model-based control. Please include a concise `Role / Finding` sentence and avoid generic world-model RL, pure model-free RL, generic robotics resources, or papers whose control-learning connection is only incidental.
