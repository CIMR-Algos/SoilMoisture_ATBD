# Level-2 product definition

```{table} L2-product
:name: L2product
| Parameter                 | Description                                                | Units     | Dimensions                                              |
|---------------------------|------------------------------------------------------------|-----------|---------------------------------------------------------|
| lon | Longitude. range: [0$^{\circ}$, 360$^{\circ}$] | *degrees East* | $[xdim_{grid},ydim_{grid}]$ |
| lat | Latitude. range: $[90^{\circ}S, 90^{\circ}N]$ | *degrees North* | $[xdim_{grid},ydim_{grid}]$ |
| time     | seconds of observation since YYYY-MM-DD 00:00:00 UTC. | *seconds* | $[xdim_{grid},ydim_{grid}]$ |
| TB_L             | L1b Brightness Temperature at L-band                      | K      | $[xdim_{grid},ydim_{grid}]$ |
| TB_L_E             | L1b Enhanced Brightness Temperature at L-band                      | K      | $[xdim_{grid},ydim_{grid}]$ |
| SM             | Soil Moisture         | $m^3/m^3$ | $[xdim_{grid},ydim_{grid}]$ |
| SM_E             | Enhanced Soil Moisture         | $m^3/m^3$ | $[xdim_{grid},ydim_{grid}]$ |
| VOD  | Vegetation Optical Depth      | -         | $[xdim_{grid},ydim_{grid}]$ |
| VOD_E  | Enhanced Vegetation Optical Depth      | -         | $[xdim_{grid},ydim_{grid}]$ |
| albedo    | Vegetation single scattering albedo  | - | $[xdim_{grid},ydim_{grid}]$ |
| EASE row index            | Row index in the global EASE2 grid                   | -         | $[xdim_{grid},ydim_{grid}]$ |
| EASE column index         | Column index in the global EASE2 grid                | -         | $[xdim_{grid},ydim_{grid}]$ |
| TB_L_RMSE            | Root Mean Squared Error between measured TB and modeled TB                      | K      | $[xdim_{grid},ydim_{grid}]$ |
| TB_L_E_RMSE          | Root Mean Squared Error between enhanced TB and modeled TB                      | K      | $[xdim_{grid},ydim_{grid}]$ |
| scene_flags             | RFI, proximity to water body, urban, ice/snow, frozen soil, precipitation, medium and strong topographic effects.                      | *8-bits flag*      | $[xdim_{grid},ydim_{grid}]$ |
| status_flag             | Product quality flag                      | n/a      | $[xdim_{grid},ydim_{grid}]$ |
```