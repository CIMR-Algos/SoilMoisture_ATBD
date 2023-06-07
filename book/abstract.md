# Abstract

This Algorithm Theoretical Basis Document (ATBD) presents the theoretical basis for the Copernicus Imaging Microwave Radiometer (CIMR) soil moisture (SM) retrieval algorithm. In doing so, the forthcoming CIMR mission carries on the legacy of successful L-band missions such as SMOS, SMAP, and Aquarius.

The proposed algorithm is grounded in the tau-omega model and the SMOS-IC algorithm for SMOS and the Dual Channel Algorithm for SMAP. It utilizes both H-polarized and V-polarized brightness temperature (TB) observations at the L-band frequency (~1.4 GHz) to estimate soil moisture.

The aim is to disaggregate L-band using C/X bands at a higher spatial resolution, leading to two soil moisture products: the first is based on the inversion of L-band-only TBs at its native resolution (~60 km, Hydroclimatological), and the second involves the inversion of L-band at an enhanced spatial resolution (~10 to 25 km, Hydrometeorological). Besides soil moisture, the vegetation opacity, τ, estimated from L to Ka band, can be associated with various vegetation attributes.