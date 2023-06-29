# Baseline Algorithm Definition

Extensive research in the passive microwave soil moisture field has led to the development of numerous soil moisture retrievals that can be applied to CIMR TB data. The ESA's SMOS mission currently operates an aperture synthesis L-band radiometer that produces TB data at various incidence angles for identical ground locations. The core SMOS retrieval algorithm utilizes the tau-omega model and takes advantage of SMOS's ability to capture multiple incidence angles for soil moisture estimation. SMAP retrievals, on the other hand, are also based on the tau-omega model but employ the constant incidence angle TB data provided by the SMAP radiometer, an approach more similar to the forthcoming CIMR mission. Consequently, we propose a retrieval algorithm that incorporates elements from both the SMOS-IC and the SMAP multi-channel algorithm.

A microwave radiometer's primary function is to detect the inherent thermal radiation that originates from a surface. Under the Rayleigh-Jeans approximation, the intensity of the observed emission at microwave frequencies is directly proportional to the product of the surface's temperature and emissivity, commonly referred to as the brightness temperature (TB).

When the microwave sensor orbits the Earth, several factors contribute to the observed TB. These include the soil's emitted energy (attenuated by overlying vegetation), the emission from vegetation itself, downwelling atmospheric emission and cosmic background emission (reflected by the surface and attenuated by vegetation), and upwelling atmospheric emission.

In the case of L band frequencies, the atmosphere is virtually transparent, with an atmospheric transmissivity (τ_atm) of approximately 1. The cosmic background, or Tsky, is around 2.7 K. Additionally, atmospheric emission is minimal.

### Forward Model

The retrieval of soil moisture from CIMR surface TB observations relies on a widely recognized approximation to the radiative transfer equation referred to as the tau-omega model, which, in this case, is referred to as the forward model. In tau-omega, a soil layer covered by vegetation attenuates the soil's emission while contributing its own emission to the overall radiative flux. Given that scattering within vegetation is generally negligible at L band frequencies, the vegetation can be predominantly treated as an absorbing layer {cite:p}`kerr2006smos,oneill2020smap`. Thus, TB can be expressed as follows:

```{math}
:label: TB-tauomega
TB_p = T_s e_p exp{(-\tau_p \sec \theta)} + T_c (1 - \omega_p)[1 - \exp(-\tau_p \sec \theta)][1 + r_p \exp(-\tau_p \sec \theta)]
```

Where the subscript p refers to polarization (V or H), Ts denotes the soil temperature and Tc stands for the canopy temperature, 𝜏_p represents the nadir vegetation opacity, 𝜔_p corresponds to the vegetation single scattering albedo (ω), and rp is the soil reflectivity of a rough surface. The reflectivity is connected to the emissivity (ep) through the relation ep = (1 - rp). It must be noted that 𝜔_p will be treated here as an effective parameter {cite:p}`KURUM201366`.

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

It is important to note that an increase in soil moisture is accompanied by a proportional increase in the soil dielectric constant (ε). For instance, liquid water has a dielectric constant of 80, while dry soil possesses a dielectric constant of 5. Furthermore, it should be acknowledged that a low dielectric constant is not uniquely indicative of dry soil conditions. Frozen soil, regardless of water content, exhibits a dielectric constant similar to that of dry soil. Consequently, a freeze/thaw flag is required to resolve this ambiguity. Since TB is proportional to emissivity for a given surface soil temperature, TB decreases as soil moisture increases. In the CIMR algorithm, ε is expressed as a function of SM, soil clay fraction and soil temperature using the model developed by Mironov {cite:p}`mironov2012`.

This relationship between soil moisture and soil dielectric constant (and consequently microwave emissivity and brightness temperature) establishes the basis for passive remote sensing of soil moisture. With CIMR observations of TB and information on T_s and T_c, H_R, and ω_p from ancillary sources, soil moisture (SM) and vegetation optical depth (VOD) can be retrieved. The procedure for this retrieval is detailed in the following section, 'Retrieval Method'.


### Retrieval Method

Prior to implementing the soil moisture retrieval, a preliminary step is to perform a water body correction to the brightness temperature data for cases where a significant percentage of the grid cells contain open water. As it is well known, brightness temperature values notably decrease when the water fraction increases {cite:p}`Ulaby2014`, leading to an overestimation of the retrieved SM values {cite:p}`ye2015` and inducing artificial seasonal cycles of VOD {cite:p}`bousquet2021`. It is therefore important to correct the CIMR brightness temperatures for the presence of water, to the extent feasible, prior to using them as inputs to the Level-2 Soil Moisture retrieval. This correction needs to be performed using the CIMR Hydrology Target mask ([RD-1] MRD-854), as part of the optimal interpolation or re-sampling process (in the CIMR RGB toolbox). The hydrology target mask will include information from both permanent and transitory water surfaces that shall be identified with the surface water seasonality information provided by the CIMR Surface Water Fraction (SWF) product as well as ancillary information.  

The procedure to acquire soil moisture (SM) and vegetation optical depth (VOD, also denoted as τ) requires the minimization of the cost function F, as shown in {eq}`cost_fun`. The method to minimize F is the Trust Region Reflective (TRR) algorithm {cite:p}`branch1999subspace`.

```{math}
:label: cost_fun
F(SM, \tau) = \frac{(TB_p^{obs} - TB_p)^2}{\sigma(TB)^2} + \sum_{i=1}^2\frac{(P_{i}^{ini} - P_i)^2}{\sigma(P_i)^2}
```

where the term $TB_p^\text{obs}$ refers to the observed value, while $\sigma(TB)$ denotes the standard deviation associated with the brightness temperature measurements (a constant value of 1 K is used). Additionally, $TB_p(\theta)$ is the brightness temperature calculated using Equation {eq}`TB-tauomega`. The equation also incorporates a regularization term, where $P_i$ ($i = 1, 2$) represents the retrieved parameter value (SM, VOD), $P_i^\text{ini}$ ($i = 1, 2$) is an a priori estimate of the parameter $P_i$, and $\sigma(P_i)$ is the standard deviation associated with this estimate.

An initial constant value of $0.2 m^3/m^3$ is assumed for SM and $\sigma(SM)$, while the value of $\tau_{NAD}$ is set to the average yearly value (calculated from previous runs). The $\sigma(\tau_{NAD})$ is computed as shown in Equation {eq}`sigma_tau`.

```{math}
:label: sigma_tau
\sigma(\tau_{NAD}) = \min(0.1 + 0.3 \cdot \tau_{NAD}, 0.3)
```

### CIMR Level-1b re-sampling approach

The CIMR Level-2 Soil Moisture retrieval algorithm will provide two soil moisture products: the first based on the inversion of L-band only TBs at its native resolution (<60 km, Hydroclimatological), the second one based on the inversion of L-band at an enhanced spatial resolution (~10 to 25 km Hydrometeorological). The enhanced L-band targets an effective mean spatial resolution of 15-km and is based on sharpening techniques that exploit the C-band and X-band channels (Zhang et al., in prep. 2023). 

Figure {numref}`resampling` illustrates the Level-1b resampling approach starting with a Backus-Gilbert or Scatterometer Image Reconstruction analysis applied at L-band with footprints matched to the C-band channel in swath geometry (at CIMR RGB Toolbox). The objective of this first step is to optimize L-band reconstruction to provide the highest possible spatial resolution at the lowest noise level ({cite:p}`Long2019`). The second step consists on the application of a sharpening algorithm to combine the reconstructed or optimally interpolated L-band to 15 km C/X-bands to estimate an equivalent 15 km L-band. The effective resolution of the TB_L and TB_L_E products will be evaluated and compared (e.g. ad in {cite:p}`Long2023`). Inputs to Level-2 Processor are initially being planned to be provided in C-band channel swath geometry although the convenience of using gridded products (including generation of Level-1c) needs to be assessed at a later phase during the project. The third step is the projection on an Earth-based map projection grid. CIMR Level-2 Soil Moisture products with an effective spatial resolution of <60 km (L-band only) and ~15 km (after sharpening using C/X bands) are planned to be projected on a 9 km EASE2 grid. The CIMR radiometer is conically scanning and its high degree of oversampling provides flexibility in resampling the data, supporting the use of a finer grid (posting resolution) than the TB effective resolution {cite:p}`Long2023`. At L-band, CIMR TB measurements are collected with an along-scan spacing of approximately 8 km, while there is an overlap of 29 % in the along-track direction (no spacing). The proposed 9 km gridding resolution is thus initially selected to preserve as much information as possible. Note that the use of the same gridding resolution for the two products will facilitate their direct comparison and algorithm prototype development at this stage of the project, but the use of an EASE2 grid with a kernel of 3 km (then multiples thereof), e.g. 9 km and 36 km as shown in Fig. {numref}`resampling` will also be considered upon characterization of the tradeoff between noise and spatial resolution of the 2-D gridded images.    

<!--
Level-2 products are planned to be projected on an EASE grid with a kernel of 3 km (then multiples thereof), i.e. Ka/Ku-bands at 3 Km, C/X-bands at 9 km, L-band at 36km. The benefit of using swath-based projections should be evaluated with dedicated experiments.
-->

```{figure} /images/Level1b_resampling.png
--- 
name: resampling
---
Conceptual flow of Level-1b resampling to exploit CIMR oversampling and nested spatial resolution and achieve global hydroclimatology and hydrometeorology soil moisture observational requirements of 60 and 15 km spatial resolution daily.
```


### Algorithm Assumptions and Simplifications

The CIMR algorithm incorporates several simplifications, which are detailed below.

For both ascending and descending satellite passes, it is assumed that the air, vegetation, and near-surface soil are in thermal equilibrium, given that the canopy temperature (Tc) can be approximated to the soil temperature (Ts) {cite:p}`Hornbuckle2005,fernandez-moran2017b`. In this context, we can represent both temperatures with a single effective temperature (Teff). 

Regarding soil roughness parameterization, the formulation used is simplified to represent soil roughness with a single parameter, H, derived from the full formulation proposed by Wang and Choudhury {cite:p}`wang1981remote`. For simplification purposes, both soil roughness and vegetation scattering albedo are considered time invariant, despite their values varying on a global scale.

A zeroth-order radiative transfer model, also known as the tau-omega model, is utilized by CIMR to estimate soil moisture. This model is a zero-order solution of the microwave radiative transfer equations, which neglects multiple reflections within the vegetation canopy. Some studies have attempted to address this limitation by utilizing higher-order radiative transfer models, such as the Two Stream Emission Model (2S-EM) {cite:p}`schwank2018`, or the approach proposed by Feldman {cite:p}`feldman2018`. Feldman's approach proposes to quantify higher order scattering through the First Order Scattering Model (First Order RTM), introducing an additional term for multiple scattering (Ω1) alongside the zeroth order scattering term (ω), using a ray-tracing method.

### Level-2 end to end algorithm functional flow diagram

```{figure} /images/flow_diagram_SM.png
--- 
name: flow_diagram_SM
---
Functional flow diagram of Level-2 Soil moisture and VOD retrieval algorithm.
```

### Functional description of each Algorithm step

##### Pre-processing of input TB

The processing algorithm primarily relies on the CIMR L1b TB product that is calibrated, geolocated and undergoes several corrections, such as atmospheric effects, Faraday rotation and RFI effects. L, C, and X-bands are used as inputs to the Soil Moisture and VOD inversion. At each of these frequencies, fore- and aft-look TB data are merged and corrected for the presence of standing water. L-band undergoes an optimal interpolation or image reconstruction step and is resampled to C-band channel swath geometry (at the RGB toolbox). This product, TB_L, is directly used as input to the Level-2 retrieval algorithm to obtain the SM and L-VOD at coarse resolution (<60km). TB_L is also combined with C and X bands into an enhanced-resolution product TB_L_E, that is used as input to the Level-2 algorithm to obtain SM and L-VOD at an enhanced spatial resolution (~15km).

Ku/Ka bands are processed independently to obtain the effective land surface temperature that is used as input in the Soil Moisture and VOD retrieval step, together with other static ancillary data. This temperature can be initially derived from CIMR Ka band using the linear regression formulation of Holmes {cite:p}`holmes2009`, although the use of the CIMR LST product or ECMWF will also be considered.


##### Analyze surface quality and surface conditions

Ancillary data will be employed to help to determine whether masks are in effect for strong topography, urban, snow/ice, frozen soil. 

<!---
##### Mathematical description

SubSubsection Text
-->
##### Input data

The input data for the model consists of two primary parameters. The first is the Level 1b Brightness Temperature (L1b TB), which is observed by CIMR at L, C, and X-bands, covering both horizontal and vertical polarizations. This data represents a full swath or swath section with dimensions *(Nscans, Npos)* in a 2D array format. The second parameter, TB_err, represents the error associated with the Brightness Temperature. This error information is also organized with the same dimensions, *(Nscans, Npos)*. This information is showed in [IODD](algorithm_input_output_data_definition.md).


##### Output data

The model outputs include key parameters such as soil moisture and vegetation optical depth at a coarse and an enhanced resolution. The output data is presented in a 9 km EASE2 grid. Additional information like brightness temperature, geographical data, albedo, and data flags supplement these outputs. Data flags enable users to examine (a) the surface
conditions of a grid cell, (b) the potential impact of RFI, and (c) the quality of soil moisture estimate when retrieval is attempted.

More details can be found in [IODD](algorithm_input_output_data_definition.md).

<!--
##### Auxiliary data

SubSubsection Text
-->

##### Ancillary data

Two sets of Land Surface Temperature will be included as ancillary data: the one estimated from CIMR Ka/Ku bands and the one from ECMWF. This will allow for some flexibility in the design and validation phase of the algorithm prototype. 

The static global maps for CIMR H and ω are computed through a weighted method. This approach takes into consideration the fraction of the MODIS IGBP land cover class that is contained within a given CIMR pixel. The data used for these computations are derived from the IGBP classification as identified in the study conducted by Fernandez-Moran {cite:p}`fernandez-moran2017b`. In Table {ref}`wandH`, different values of ω and H are listed according to land cover type. Based on these criteria, static global maps of ω and H have been produced as part of the ancillary dataset.

```{table} Values of ω and H
:name: wandH
| ID | MODIS IGBP land classification | SMAP MTDCA ω | SMAP DCA ω | CIMR ω | SMOS-IC H | CIMR H |
|----|--------------------------------|------------|----------|-------|------------|-----------------|
| 1  | Evergreen Needleleaf Forests   | 0.07       | 0.07     | 0.06  | 0.30       | 0.40            |
| 2  | Evergreen Broadleaf Forests    | 0.08       | 0.07     | 0.06  | 0.30       | 0.40            |
| 3  | Deciduous Needleleaf Forests   | 0.06       | 0.07     | 0.06  | 0.30       | 0.40            |
| 4  | Deciduous Broadleaf Forests    | 0.07       | 0.07     | 0.06  | 0.30       | 0.40            |
| 5  | Mixed Forests                  | 0.07       | 0.07     | 0.06  | 0.30       | 0.40            |
| 6  | Closed Shrublands              | 0.08       | 0.08     | 0.10  | 0.27       | 0.27            |
| 7  | Open Shrublands                | 0.06       | 0.07     | 0.08  | 0.17       | 0.10            |
| 8  | Woody Savannas                 | 0.08       | 0.08     | 0.06  | 0.30       | 0.40            |
| 9  | Savannas                       | 0.07       | 0.10     | 0.10  | 0.23       | 0.23            |
| 10 | Grasslands                     | 0.06       | 0.07     | 0.10  | 0.12       | 0.50            |
| 11 | Permanent Wetlands             | 0.16       | 0.10     | 0.10  | 0.19       | 0.19            |
| 12 | Croplands - Average            | 0.10       | 0.06     | 0.12  | 0.17       | 0.40           |
| 13 | Urban and Built-up Lands       | 0.08       | 0.08     | 0.10  | 0.21       | 0.21           |
| 14 | Crop-land/Natural Vegetation Mosaics | 0.09 | 0.10 | 0.12 | 0.22 | 0.50   |
| 15 | Snow and Ice                   | 0.11       | 0.00     | 0.10  | 0.12       | 0.12            |
| 16 | Barren                         | 0.02       | 0.00     | 0.12  | 0.02       | 0.10            |
```

Furthermore, a CIMR Hydrology Target mask, applied in Level-2 data processing, provides a ≤1 km resolution and covers both permanent and transitory inland water surfaces. The mask incorporates data from the MERIT Hydro {cite:p}`yamazaki2019merit` and the Global Lakes and Wetlands Database {cite:p}`lehner2004development`, and it will be updated up to four times annually to account for potential seasonal changes. Its calculation involves a previous estimation of the Surface Water Fraction (SWF) data.

The scene flag incorporate information about RFI, proximity to water body, urban, ice/snow, frozen soil, precipitation, medium and strong topographic effects. 

The rest of datasets that complement the ancillary information are the clay fraction (from FAO), the IGBP Land Cover type Classification (from MODIS) and the Digital Elevation Model obtained from the Shuttle Radar Topography Mission (SRTM) {cite:p}`jarvis2006,mialon2008`.


<!--
[MRD-854]

A CIMR Hydrology Target mask shall be used for Level-2 data processing
activities according to the following specification:
The Hydrology Target Mask shall have a spatial resolution of ≤1 km.
It shall include fractional water surfaces with a spatial resolution ≤ 1 x 1 km
Inland water surfaces (lakes reservoirs, rivers, as well as wetlands) shall be
identified in the database.
Both permanent and transitory water surfaces shall be identified with seasonality
information.
The baseline data set shall include the Yamazaki et al. (2019) MERIT Hydro at
~90m: Global Hydrography Datase available at http://hydro.iis.utokyo.
ac.jp/~yamadai/MERIT_Hydro
The baseline data set shall include The Global Lakes and Wetlands Database
(Lernher and Doll, 2004) at 1 km data set that can provide complementary
information on the water surfaces. A GLWD2 version should soon available at
500m spatial resolution, with improved information. Auxiliary data (e.g., Digital
elevation models) as required depending on the implementation of CIMR.
Other aspects as required depending on the implementation of CIMR.

Note 1: The CIMR Hydrology Target Mask specifies lake, reservoir, and wetland targets
for the CIMR mission.
Note 2: The mask considers large lakes that are greater than 5 km since Ka-band CIMR
measurement footprint are <5 km.
Note 3: The Hydrology Target Mask shall be re-evaluated and uploaded to the satellite up
to 4 (TBC) times per year. This is necessary since river spatial extent and
position may change seasonally.
Note 4: Example maps form Yamazaki et al (2019), “MERIT Hydro river width (right) are
shown below.
Note 5: Monthly masks could be considered, with possible updates during the mission.


    |
--> 

##### Validation process

SubSubsection Text


