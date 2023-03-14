# Background and justification of selected algorithm

## Introduction

Soil moisture is a crucial element of the Earth's water cycle and it has been used for agricultural monitoring, water supply management, drought and flood forecasting, forest fire prediction and land-atmosphere interactions. Soil moisture is defined as the amount of water held in the soil, and relates to precipitation, evaporation, and plant uptake. Changes in soil moisture can have substantial impacts on agricultural productivity, forest and the general ecosystem health.

Soil moisture was identified as an essential climate variable (ECV) by the Global Climate Observing System (GCOS), due to its importance for understanding and monitoring changes in the Earth's climate system. Soil moisture estimation over large areas and long periods is challenging due to its spatial and temporal variability. However, remote sensing technologies have revolutionized the ability to measure soil moisture over large areas, which has facilitated numerous applications in several fields, including hydrology, agriculture, and climate modeling.

Accurate and timely measurements of soil moisture are crucial for improving our understanding of the Earth's climate system and its associated processes. For instance, monitoring soil moisture can help predict droughts, floods, and landslides, which can save lives and reduce economic losses. It can also help optimize water management and irrigation practices, leading to increased agricultural productivity and efficiency. Furthermore, soil moisture data can improve weather and climate forecasting by improving the accuracy of precipitation estimates and address priority questions on climate change by identifying patterns and trends that affect the Earth's climate.

Passive remote sensing of soil moisture is a technique used to estimate soil moisture content over large areas and at high temporal resolution. Passive microwave sensors are used to measure the natural thermal radiation emitted from the soil surface. The intensity of this radiation varies depending on the dielectric properties and temperature of the target medium, which in the case of the near surface soil layer, is influenced by the amount of moisture present. The low microwave frequencies at L-band (~1 GHz) have additional benefits for soil moisture measurement: the atmosphere is almost entirely transparent, making it possible to sense soil moisture regardless of weather conditions; signals from the underlying soil can be transmitted through thin vegetation layers; and the measurements are not affected by solar illumination, making them suitable for day and night observations.

## Historical heritage
The history of passive remote sensing for soil moisture estimation dates back to the early 1960s. During this period [1], researchers began to look at ways of using microwaves to measure water content in the soil. The first successful experiments used a single-channel microwave radiometer to measure the brightness temperature of the surface of the Earth. This data was then used to calculate the soil moisture content.

In the 1970s and 1980s, the number of channels of the radiometer was increased and more sophisticated models were developed to better measure soil moisture. This included the use of multiple frequency bands and polarimetric techniques to better characterize the soil moisture.

Since then, passive remote sensing for soil moisture estimation has gone through several iterations [1], with advances in hardware and software technology allowing for more accurate and precise measurement. Today, passive remote sensing is one of the most widely used methods for soil moisture estimation, with satellites, aircraft, and ground-based instruments all contributing to the data.


CIMR observations will potentially provide continuity to the brightness temperature and soil moisture measurements from ESA’s SMOS (Soil Moisture Ocean Salinity) and NASA’s SMAP (Soil Moisture Active Passive) and  Aquarius missions.

1. 1. D. Entekhabi, S. Yueh, P. O'Neill, K. Kellogg, A. Allen, R. Bindlish, M. Brown, et al., SMAP Handbook, p. 400-1567, JPL, 2014.

## Physical approach

As mentioned, a microwave radiometer measures the natural thermal emission coming from the surface. At microwave frequencies, the intensity of the observed emission is proportional to the product of the temperature and emissivity of the surface (Rayleigh-Jeans approximation). This product is commonly called the brightness temperature TB. If the microwave sensor is in orbit above the earth, the observed TB is a combination of the emitted energy from the soil as attenuated by any overlying vegetation, the emission from the vegetation, the downwelling atmospheric emission and cosmic background emission as reflected by the surface and attenuated by the vegetation, and the upwelling atmospheric emission (Figure 2). At L band frequencies, the atmosphere is essentially transparent, with the atmospheric transmissivity τatm ≈ 1. The cosmic background Tsky is on the order of 2.7 K. The atmospheric emission is also very small. These small atmospheric contributions will be accounted for in the L1B_TB ATBD, since the primary inputs to the radiometer-derived soil moisture retrieval process described in this L2_SM_P ATBD are atmospherically-corrected surface brightness temperatures as described in Section 3.

At microwave frequencies, the intensity of emission is proportional to the surface temperature and emissivity, which is commonly referred to as brightness temperature (TB) using the Rayleigh-Jeans approximation. When the microwave sensor orbits above the Earth, the observed TB includes energy from the soil (attenuated by the vegetation), and vegetation, downwelling atmospheric emission and cosmic background emission reflected by the surface and attenuated by vegetation, and the upwelling atmospheric emission ({numref}`Figure1`). The atmosphere transmissivity (τ<sub>atm</sub>) is approximately equal to 1, and the cosmic background temperature (T<sub>sky</sub>) is around 2.7 K. 

```{figure} /images/TB-contributions.jpg
--- 
name: Figure1
---
Contributions to the Top Of Atmosphere (TOA) brigthness temperature [from SMOS ATBD, ref. 12 and SMAP ATBD, Figure 2]

The process of obtaining soil moisture information from CIMR TB observations involves using a the tau-omega model, which is commonly employed in the passive microwave soil moisture community. This model takes into account the impact of a layer of vegetation covering the soil, which affects the emission of the soil and adds to the overall radiative flux its own emission. When working with L band frequencies, it is generally assumed that the scattering within the vegetation is negligible, so the vegetation can be considered primarily as an absorbing layer.

## Optimal band selection

Compared to higher frequencies, L-band is able to penetrate through larger amounts of vegetation. Thus, in the presence or dense biomass, the transmissivity decays in a lower extent for L-band (1.4 GHz), as compared to higher frequencies: C-band (6 GHz), and X-band (10 GHz) frequencies. The results clearly indicate that L-band frequencies have a significant advantage over the higher C- and X-band frequencies that are currently provided by satellite instruments like AMSR-E, AMSR2 and WindSat. This helps to explain why both SMOS and SMAP use L-band sensors to estimate soil moisture globally under the widest range of vegetation conditions. Additionally, measuring soil moisture at L-band has the added benefit of capturing microwave emission from deeper within the soil, typically around 5 cm, whereas C- and X-band emissions primarily originate from a thinner layer (as illustrated in Figure 4).



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
