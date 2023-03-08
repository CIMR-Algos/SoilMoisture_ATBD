# Background and justification of selected algorithm

## Introduction

Soil moisture is a critical component of the Earth's water cycle and plays an essential role in agricultural monitoring, water supply management, drought and flood forecasting, forest fire prediction and land-atmosphere interactions. Soil moisture is defined as the amount of water held in the soil, and relates to precipitation, evaporation, and plant uptake. Changes in soil moisture can have substantial impacts on agricultural productivity, forest and the general ecosystem health. As a result, it is a key variable in understanding and modeling the Earth's climate system.

Soil moisture was identified as an essential climate variable (ECV) by the Global Climate Observing System (GCOS), due to its importance for understanding and monitoring changes in the Earth's climate system. Soil moisture estimation over large areas and long periods is challenging due to its spatial and temporal variability. However, remote sensing technologies have revolutionized the ability to measure soil moisture over large areas, which has facilitated numerous applications in several fields, including hydrology, agriculture, and climate modeling.

Accurate and timely measurements of soil moisture are crucial for improving our understanding of the Earth's climate system and its associated processes. For instance, monitoring soil moisture can help predict droughts, floods, and landslides, which can save lives and reduce economic losses. It can also help optimize water management and irrigation practices, leading to increased agricultural productivity and efficiency. Furthermore, soil moisture data can improve weather and climate forecasting by improving the accuracy of precipitation estimates and identifying patterns and trends that affect the Earth's climate.

Passive remote sensing of soil moisture is a technique used to estimate soil moisture content over large areas and at high temporal resolution. This approach involves measuring the naturally emitted microwave radiation from the Earth's surface and using the information to estimate soil moisture content.

Passive remote remote sensors measure microwave emissions from the Earth's surface, and have been successful in providing accurate and reliable estimates of soil moisture content at global scales.

## Historical heritage
The history of passive remote sensing for soil moisture estimation dates back to the early 1960s. During this period [1], researchers began to look at ways of using microwaves to measure water content in the soil. The first successful experiments used a single-channel microwave radiometer to measure the brightness temperature of the surface of the Earth. This data was then used to calculate the soil moisture content.

In the 1970s and 1980s, the number of channels of the radiometer was increased and more sophisticated models were developed to better measure soil moisture. This included the use of multiple frequency bands and polarimetric techniques to better characterize the soil moisture.

Since then, passive remote sensing for soil moisture estimation has gone through several iterations [1], with advances in hardware and software technology allowing for more accurate and precise measurement. Today, passive remote sensing is one of the most widely used methods for soil moisture estimation, with satellites, aircraft, and ground-based instruments all contributing to the data.

1. 1. D. Entekhabi, S. Yueh, P. O'Neill, K. Kellogg, A. Allen, R. Bindlish, M. Brown, et al., SMAP Handbook, p. 400-1567, JPL, 2014.


## Justification of selected algorithm




Text describing background

Example figure. Figure numbering should be chronological, following the numbering of the figures in previous chapters.

Starting points for the optimisation are sampled on a length-angle
regular grid around point $(0,0)$ as on {numref}`startpoints`.


```{figure} ./startpoints.png
--- 
name: startpoints
---
Location of the preliminary function evaluations for choosing starting points to the Nelder Mead algorithm. The circle locates the limit of vality domain $\mathbf{D}$.
```

We can also refer to the figure this way: {ref}`Starting points for the optimisation <startpoints>`

Here is a math block
$
  \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
$

Here are labeled and numbered equations

```{math}
:label: my_label
w_{t+1} = (1 + r_{t+1}) s(w_t) + y_{t+1}
```

```{math}
:label: my_other_label
w_{t+2} = (1 + r_{t+2}) s(w_t) + y_{t+2}
```
