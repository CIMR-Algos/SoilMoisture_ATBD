# Baseline Algorithm Definition

Extensive research in the passive microwave soil moisture field has led to the development of numerous soil moisture retrievals that can be applied to CIMR TB data. The ESA's SMOS mission currently operates an aperture synthesis L-band radiometer that produces TB data at various incidence angles for identical ground locations. The core SMOS retrieval algorithm utilizes the tau-omega model and takes advantage of SMOS's ability to capture multiple incidence angles for soil moisture estimation. SMAP retrievals, on the other hand, are also based on the tau-omega model but employ the constant incidence angle TB data provided by the SMAP radiometer, an approach more similar to the forthcoming CIMR mission. Consequently, we propose a retrieval algorithm that incorporates elements from both the SMOS-IC and the SMAP multi-channel algorithm.

A microwave radiometer's primary function is to detect the inherent thermal radiation that originates from a surface. Under the Rayleigh-Jeans approximation, the intensity of the observed emission at microwave frequencies is directly proportional to the product of the surface's temperature and emissivity, commonly referred to as the brightness temperature (TB).

When the microwave sensor orbits the Earth, several factors contribute to the observed TB. These include the soil's emitted energy (attenuated by overlying vegetation), the emission from vegetation itself, downwelling atmospheric emission and cosmic background emission (reflected by the surface and attenuated by vegetation), and upwelling atmospheric emission.

In the case of L band frequencies, the atmosphere is virtually transparent, with an atmospheric transmissivity (τ_atm) of approximately 1. The cosmic background, or Tsky, is around 2.7 K. Additionally, atmospheric emission is minimal.

### Forward Model

The retrieval of soil moisture from CIMR surface TB observations relies on a widely recognized approximation to the radiative transfer equation referred to as the tau-omega model, which, in this case, is referred to as the forward model. In tau-omega, a soil layer covered by vegetation attenuates the soil's emission while contributing its own emission to the overall radiative flux. Given that scattering within vegetation is generally negligible at L band frequencies, the vegetation can be predominantly treated as an absorbing layer {cite:p}`kerr2006smos;oneill2020smap`. Thus, TB can be expressed as follows:

```{math}
:label: TB-tauomega
$TB_p = T_s e_p exp{(-\tau_p \sec \theta)} + T_c (1 - \omega_p)[1 - \exp(-\tau_p \sec \theta)][1 + r_p \exp(-\tau_p \sec \theta)]$
```


Where the subscript p refers to polarization (V or H), Ts denotes the soil's effective temperature, Tc stands for the canopy temperature, 𝜏_p represents the nadir vegetation opacity, 𝜔_p corresponds to the vegetation single scattering albedo, and rp is the soil reflectivity of a rough surface. The reflectivity is connected to the emissivity (ep) through the relation ep = (1 - rp). 

According to Beer's law, the overlying canopy layer's transmissivity or vegetation attenuation factor , γ, is given by γ = exp(-𝜏_p sec 𝜃). Equation {eq}`TB-tauomega` assumes that vegetation multiple scattering and reflection at the vegetation-air interface are negligible. 

Surface roughness is modeled as r_p  = r*_p exp (-H_R), where H_R parameterizes the intensity of the roughness effects, r*_p stands for the reflectivity of a plane surface. Nadir vegetation opacity is related to the total vegetation water content (VWC, in kg/m2) by 𝜏_p = bp·VWC, with the coefficient bp dependent on vegetation type and microwave frequency (and polarization) {cite:p}Van De Griend2004.

The surface reflectance rp is characterized by the Fresnel equations, which detail the actions of an electromagnetic wave when interacting with a smooth surface. When an electromagnetic wave encounters a surface that separates two media with different dielectric properties (e.g., air and soil), part of the wave's energy is reflected at the surface, and part is transmitted through it. The Fresnel equations are used to calculate the amount of energy reflected based on the incident angle of the wave and the dielectric properties of the involved media. For horizontal polarization, the wave's electric field aligns parallel to the reflecting surface and perpendicular to the propagation direction. In contrast, for vertical polarization, the electric field of the wave has a component perpendicular to the surface. Equations {eq}`rh_theta`{eq}`rv_theta` show the Fresnel equations for both horizontal and vertical polarizations. 

```{math}
:label: rh_theta
r_H(\theta) = \left| \frac{\cos\theta - \sqrt{\epsilon - \sin^2\theta}}{\cos\theta + \sqrt{\epsilon - \sin^2\theta}} \right|^2
```

```{math}
:label: rv_theta
r_V(\theta) = \left| \frac{\epsilon\cos\theta - \sqrt{\epsilon - \sin^2\theta}}{\epsilon\cos\theta + \sqrt{\epsilon - \sin^2\theta}} \right|^2
```

where θ represents the CIMR incidence angle, while ε denotes the soil layer's complex dielectric constant.

It is important to note that an increase in soil moisture is accompanied by a proportional increase in the soil dielectric constant. For instance, liquid water has a dielectric constant of 80, while dry soil possesses a dielectric constant of 5. Furthermore, it should be acknowledged that a low dielectric constant is not uniquely indicative of dry soil conditions. Frozen soil, regardless of water content, exhibits a dielectric constant similar to that of dry soil. Consequently, a freeze/thaw flag is required to resolve this ambiguity. Since TB is proportional to emissivity for a given surface soil temperature, TB decreases as soil moisture increases. 

This relationship between soil moisture and soil dielectric constant (and consequently microwave emissivity and brightness temperature) establishes the basis for passive remote sensing of soil moisture. With CIMR observations of TB and information on T_s and T_c, H_R, and ω_p from ancillary sources, soil moisture (SM) and as explained in the "Retrieval Method" section. 


### Retrieval Method

The process of obtaining soil moisture (SM) and vegetation optical depth (VOD) requires minimizing the given cost function, x:

```{math}
:label: cost_fun
$$x=\sum_{i=1}^n\frac{(TBp(\theta)_{mes}-TBp(\theta))^2}{\sigma(TB)^2}+\sum_{i=1}^2\frac{(P_{ini,i}-P_i)^2}{\sigma(P_i)^2}$$
```

In this equation:

- N represents the total number of observations for different viewing angles (θ) and both polarizations (H, V).
- TB_p(θ)_mes is the measured value over the SMOS pixels from the SMOSL3 TB product (discussed in Section 2.2.2).
- σ(TB) is the standard deviation associated with the brightness temperature measurements (a constant value of 4 K was used in this study).
- TB_p(θ) is the brightness temperature calculated using Equation (1).
- P_i (i = 1, 2) is the retrieved parameter value (SM, VOD).
- P_inii (i = 1, 2) is an a priori estimate of the parameter P_i.
- σ(P_i) is the standard deviation associated with this estimate.

A constant initial value of 0.2 m³/m³ was considered for SM and σ(SM), and the value of τ_NAD was set equal to a yearly average value (calculated from previous runs). σ(τ_NAD) was determined as follows:



The regularization term 𝜆2(𝜏−𝜏∗)2 was applied to reduce the temporal and spatial noise caused by the nature of the cost function. 𝜏∗ is the initial guess for the unknown vegetation optical depth derived from MODIS NDVI vegetation water content climatology. The parameter 𝜆 is the regularization parameter weight which was assumed to be 𝜆=20. The 𝜆 value was selected trying to balance the noise reduction and at the same time to give the optimization process enough freedom to converge to values sufficiently distant from 𝜏∗. Figure 16 shows an example of the cost function for different values of 𝜆. The black point represents the initial guess and the red points are the solution when the observed brightness temperatures are perturbed with noise. While for 𝜆 = 0, the solutions jump along the blue valley (very noisy), the solutions for 𝜆 = 40 (center) get clustered very close to the 𝜏∗ initial guess value, a situation that is not desired due to the nature of the NDVI tau. Therefore, as a compromise, 𝜆 was set equal to 20. Figure 17 displays histograms of the standard deviation of the Nyquist frequency for the global daily climatology of DCA tau before and after regularization; the clear shift of the peak to the left of the histograms shows the reduction of the temporal noise in the retrievals of the vegetation optical depth. The selection of the parameter 𝜆 based on the standard deviation of the Nyquist frequency of DCA 𝜏 (without regularization, 𝜆 = 0 ) time series at a particular grid point and the initial guess 𝜏∗ based on DCA climatology will be the subject of future work.


### CIMR Level-1b re-sampling approach

Subsection Text


### Algorithm Assumptions and Simplifications

THERMAL EQUILIBRIUM !!!! ¿?¿¿????? 

Q and N parameters of the soil roughness model

### Level-2 end to end algorithm functional flow diagram

Subsection Text

### Functional description of each Algorithm step

Subsection Text

##### Mathematical description

SubSubsection Text
##### Input data

SubSubsection Text

##### Output data

SubSubsection Text

<!--
##### Auxiliary data

SubSubsection Text
-->

##### Ancillary data

| ID | MODIS IGBP land classification | SMAP SCA | SMAP L4 | SMAP MTDCA | CIMR | SMAP DCA |
|----|--------------------------------|----------|---------|------------|---------|----------|
| 0 | Water Bodies | 0.000 | -- | 0.00 | 0.00 | 0.00 |
| 1 | Evergreen Needleleaf Forests | 0.050 | 0.11 | 0.07 | 0.06 | 0.07 |
| 2 | Evergreen Broadleaf Forests | 0.050 | 0.07 | 0.08 | 0.06 | 0.07 |
| 3 | Deciduous Needleleaf Forests | 0.050 | 0.11 | 0.06 | 0.06 | 0.07 |
| 4 | Deciduous Broadleaf Forests | 0.050 | 0.09 | 0.07 | 0.06 | 0.07 |
| 5 | Mixed Forests | 0.050 | 0.10 | 0.07 | 0.06 | 0.07 |
| 6 | Closed Shrublands | 0.050 | 0.09 | 0.08 | 0.10 | 0.08 |
| 7 | Open Shrublands | 0.050 | 0.09 | 0.06 | 0.08 | 0.07 |
| 8 | Woody Savannas | 0.050 | 0.12 | 0.08 | 0.06 | 0.08 |
| 9 | Savannas | 0.080 | 0.13 | 0.07 | 0.10 | 0.10 |
| 10 | Grasslands | 0.050 | 0.06 | 0.06 | 0.10 | 0.07 |
| 11 | Permanent Wetlands | 0.000 | 0.13 | 0.16 | 0.10 | 0.10 |
| 12 | Croplands - Average | 0.050 | 0.10 | 0.10 | 0.12 | 0.06 |
| 13 | Urban and Built-up Lands | 0.030 | 0.10 | 0.08 | 0.10 | 0.08 |
| 14 | Crop-land/Natural Vegetation Mosaics | 0.065 | 0.14 | 0.09 | 0.12 | 0.10 |
| 15 | Snow and Ice | 0.000 | 0.09 | 0.11 | 0.10 | 0.00 |
| 16 | Barren | 0.000 | 0.07 | 0.02 | 0.12 | 0.00 |


##### Validation process

SubSubsection Text


