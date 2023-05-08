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

The procedure to acquire soil moisture (SM) and vegetation optical depth (VOD, also denoted as τ) requires the minimization of the cost function F, as shown in {eq}`cost_fun`.

```{math}
:label: cost_fun
$$
F(SM, \tau) = \frac{(TB_p^{obs} - TB_p)^2}{\sigma(TB)^2} + \sum_{i=1}^2\frac{(P_{i}^{ini} - P_i)^2}{\sigma(P_i)^2}
$$
```

where the term $TB_p^\text{obs}$ refers to the observed value, while $\sigma(TB)$ denotes the standard deviation associated with the brightness temperature measurements (a constant value of 1 K is used). Additionally, $TB_p(\theta)$ is the brightness temperature calculated using Equation {eq}`TB-tauomega`. The equation also incorporates a regularization term, where $P_i$ ($i = 1, 2$) represents the retrieved parameter value (SM, VOD), $P_i^\text{ini}$ ($i = 1, 2$) is an a priori estimate of the parameter $P_i$, and $\sigma(P_i)$ is the standard deviation associated with this estimate.

An initial constant value of 0.2 m^3/m^3 is assumed for SM and $\sigma(SM)$, while the value of $\tau_{NAD}$ is set to the average yearly value (calculated from previous runs). The $\sigma(\tau_{NAD})$ is computed as shown in Equation {eq}`sigma_tau`.

```{math}
:label: sigma_tau
\sigma(\tau_{NAD}) = \min(0.1 + 0.3 \cdot \tau_{NAD}, 0.3)
```

### CIMR Level-1b re-sampling approach

Subsection Text


### Algorithm Assumptions and Simplifications

- THERMAL EQUILIBRIUM 

- Q and N parameters of the soil roughness model

### Level-2 end to end algorithm functional flow diagram

Subsection Text

### Functional description of each Algorithm step

Subsection Text

##### Mathematical description

SubSubsection Text

##### Input data

The processing algorithm primarily relies on the CIMR TB product that is calibrated, geolocated, and adjusted to the EASE2 grid. Previously, the TB data undergoes several corrections, such as atmospheric effects, Faraday rotation and RFI effects.

Besides TB measurements, the CIMR retrieval algorithm uses supplementary datasets for accurate soil moisture extraction. The required data encompasses:

• Surface temperature
• Soil texture (clay fraction)
• Land cover type classification
• Vegetation single scattering albedo
• Surface roughness information
• Data flags for identification of land, water, RFI, urban areas, mountainous terrain and permanent ice/snow.

The specific parameters and sources of these parameters are detailed in the Ancillary data section.


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


