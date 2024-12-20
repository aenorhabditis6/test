### Fall 2024, AS.171.501 Physics Independent Research  
# Stability of Filament Chain Shapes in Brownian Dynamics Simulations 
### Author: Xinming Shen, Advisor: Prof. Daniel Beller


## Abstract

This independent study uses Brownian dynamics simulations following Dr. Athani's work (2024) to investigate semi-flexible filament chain shapes under varying mechanical and thermal conditions. We looked at the interactions between bending stiffness, stretching moduli, and thermal fluctuations by setting up a new initialization filament configuration with 1 chain and many beads in a calculated curvature. Specifically, we examine how initial chain shapes evolve over time with differernt bending moduli based on our stability analysis. The simulation results showed that with the calculated curvature set-up the chain does not tend to have the expected stable shape. This is an ongoing project and further results would be tested and evaluated. 

## Introduction

Semiflexible filaments play a crucial role in numerous biological and synthetic systems, from cellular cytoskeletons to engineered nanomaterials. This study builds upon previous research exploring the emergent behaviors of active semiflexible filaments, specifically focusing on developing a framework for analyzing the stability of bead chain shape under different conditions.

### Theoretical Background

The fundamental challenge in modeling semiflexible filaments lies in tracking the exact behaviors of filament chains with mechanical constraints and material properties. My investigation is grounded in Chanania's mathematical framework that describes filament dynamics through a complex interplay of forces and time. 

The core of our analysis revolves around the curvature equation:

$$f(k) = \frac{\kappa_B}{v \sin(\alpha)} k^3 - \left(1 + \frac{\kappa_B}{\kappa_S} k^2\right)^2 = 0$$

Where:
- $\kappa_S$ is the stretching modulus
- $\kappa_B$ is the bending modulus
- $v$ is the velocity
- $k$ is the curvature
- $\alpha$ is an angular parameter

This equation incorporates the fundamental physical constraints governing filament deformation, and therefore allowing us to systematically review the conditions that lead to stable or unstable chain configurations over time. Rest of the work focuses on solving the equations and testing the chains' stability. 

## Theoretical Review: Mathematical Framework of Filament Dynamics

### Mathematical Modeling

Steinbock's work (2024) offers a rigorous mathematical framework for understanding semiflexible filament dynamics, rooted in continuum mechanics and differential geometry. There are several key mathematical constructs from the framework:

#### Curve Parameterization and Geometric Definitions

The filament is modeled as a continuous curve segment $\vec{r}(s)$ parameterized by arc length $s \in [0, L]$, where $L$ represents the rest length. Important common geometric properties are also defined:

1. **Arc-length parameter**: 
   $\vec{r}'(s) = \frac{\partial\vec{r}(s)}{\partial s}$

2. **Unit Tangent Vector**: 
   $\hat{T}(s) = \frac{\vec{r}'(s)}{|\vec{r}'(s)|}$

3. **Curvature**: 
   $k(s) = \frac{1}{|\vec{r}'(s)|} \hat{T}'(s) \cdot \hat{N}(s)$

A crucial theoretical insight emerges from the Frenet–Serret formulas: given $|\vec{r}'(s)|$, $\hat{T}(s)$, and $k(s)$ for all $s$, one can uniquely reconstruct the entire curve up to rigid motions.

#### Evolution Equations

The filament's evolution is governed by a force-based equation:

$\frac{\partial \vec{r}(s)}{\partial t} = \vec{F}(s)$

where the total force $\vec{F}(s)$ comprises three critical components:
- Stretching force $\vec{F}_S(s)$
- Bending force $\vec{F}_B(s)$
- Active force $\vec{F}_A(s)$

And each of them can be written as equations for the time evolution. As a result, we only need to treat the time evolution equations for $|\vec{r}'(s)|$ and $k(s)$.

#### Nondimensionalization

To simplify the notation, the euqations are nondimentionalized and scaled: 

- $g_S = \frac{\kappa_S}{Lv}$ (stretching parameter)
- $g_B = \frac{\kappa_B}{L^3v}$ (bending parameter)

### Constant Curvature Solutions

From theoretical prediction, the curvature solved from curvature equation reveals insights into potential filament configurations:

1. **Uncurved Solution**: 
   - $w = 0$ (zero curvature)
   - $u = 1$ (unstretched)
   - Provably stable to small perturbations
   
   And they are the trivial stable solutions to the system. 

2. **Curved Constant Curvature Solutions**:
   Exist under specific conditions determined by a quartic polynomial equation:

   $\frac{g_B}{\sin(\alpha)} w_0^4 = w_0 - \frac{\sin(\alpha)}{g_S}$

   Depending on parameter values, there are zero, one, or two real solutions. 

### Stability Regions

Critically, not all solutions are stable. The stability depends on complex interactions between the stretching modulus $g_S$, bending modulus $g_B$, and active force angle $\alpha$. The theoretical analysis reveals that there exist a phase graph for stability of curvature, where solutions can become linearly stable linearly unstable, and partially stable. 

The graph that demonstrates the number of solution region and stability is attached. 


Figure 1, Phase Diagram of Constant Curvature Solution with $\alpha = 0.1$ (Steinbock, 2024). 
<img width="617" alt="PIcture at Stability regions" src="https://github.com/user-attachments/assets/63744148-d180-4061-93d7-69b00191fc5a" />

### Research Objectives

As an anxiliary to the work, the primary objectives of this independent study were to:
1. Develop (or adapt) a computational method for initializing filament chains with predefined bending configurations;
2. Create a method to monitor the stability of this configuration by checking if the chain retains its shape over time.
3. Investigate how varying mechanical parameters influence filament shape stability;
4. Identify the transition points where chain configurations undergo significant structural changes.

## Experiment Method

### Simulation Setup

Our Brownian dynamics simulation utilized a bead-spring chain model with the following key parameters:
- Chain length: 50 beads
- Initial configuration: Uniform bending shape
- Configurable parameters:
  <!-- - Bending stiffness ($\kappa = $) -->
  - Self-propulsion velocity ($v_0 = 10$)
  - Tangential velocity offset ($v_{tan\_angle} = 90$, directed outward)
  - Bending parameter ($g_B = 0.1$)

Results and visualization of the chain evolution are stored by the `snapshot` and `animate` functions. 

#### Curvature Initialization

The chain initialization process involves calculating bead positions based on:
- Curvature calculation using the derived equation
- Radius determination
- Arc length computation

The `fix_bend_new()` function initializes bead positions with curvature which is solved by the curvature equation and ensuring a relaxed initial configuration. For this study purposes, there is only one curved chain with many beads. An example of the initialization is shown below with visualization of the initial velocity: 

Figure 2, Initialization Configuration with Visualization, an Example. 
<img width="609" alt=" Curvature Initialization" src="https://github.com/user-attachments/assets/849ab53e-827e-40ec-bcc9-5bab5851274e" />


### Computational Approach

I implemented a systematic exploration of the curvature behavior by iteratively solving $f(k) = 0$ for different bending modulus ($g_B$) values, $g_S$ is currently fixed. Just to confirm the correctness of finding roots, I also made a visualization of root finding across a logarithmic range $10^{-3}$ to $1.0$ of $g_B$ values. 

### Visualization and Analysis

We used multiple visualization techniques to analyze filament dynamics:
- Velocity vector plotting
- Animation of filament evolution through snapshot
- Root detection and transition point analysis

## Results and Discussion

### Observed Configurations
We implemented the experiment so that the simulation has zero temperature and some small initial velocity. The curvature equation exhibits complex root behavior dependent on $g_B$, and transition points exist where multiple solution branches emerge. I noticed that small changes in bending and stretching moduli can dramatically alter filament stability. 

After correctly setting up the initialization and finding all the roots of the curvature equation, I observed that the filament was able to form a circular shape for a period, which aligns with the theoretical predictions for the bending and stretching parameters. However, all chains deform after a short time and gradually evolve into a straight chain. In addition, in the 2 solution case, there is always one chain with small curvature that wraps and collapses. We suspect that this might be affected by the initial tangential velocity offset. 

Figure 3, Circular Chain Initialization, an Example. 
<img width="304" alt="figure 3" src="https://github.com/user-attachments/assets/2e592bae-3df8-459a-9a18-616643dfa58b" />


An example of 2 curvature solutions with one of them wrapped can be seen in Figure 4. 


Figure 4, Chain Initialization for 2 Curvature Solutions with $\frac{g_B}{\sin(\alpha)} = 0.099$ and $\frac{g_S}{\sin(\alpha)} = 10.000$.  
<img width="915" alt="results" src="https://github.com/user-attachments/assets/4ed1bb4a-a498-46ad-b38b-e524fd833013" />



## Conclusion

The independent study developed a comprehensive computational framework for investigating semiflexible filament dynamics. By combining advanced mathematical modeling with systematic computational exploration, we have provided new insights into the complex world of filament mechanics.

Future work should focus on:
- Extending the model to multiple interacting filaments
- Exploring more complex initial configurations
- Investigating the impact of thermal fluctuations at varying intensities

## Reference
[1] Steinbock, C. (2024). *Nov 2024 summary*. [Unpublished manuscript]. Department of Physics, Johns Hopkins University.


[2] Athani, M. G. (2024). *Emergent, decaying, and spinning nematic order in collective motion of active semiflexible filaments* (Doctoral dissertation, Johns Hopkins University). Johns Hopkins University.


