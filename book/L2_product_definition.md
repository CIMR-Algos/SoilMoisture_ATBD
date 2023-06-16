# Level-2 product definition

| Parameter                 | Description                                                | Units     | Dimensions                                              |
|---------------------------|------------------------------------------------------------|-----------|---------------------------------------------------------|
| lon | Longitude. range: $[0^{\circ}, 360^{\circ}]$ | *degrees East* | $[xdim_{grid},ydim_{grid}]$ |
| lat | Latitude. range: $[90^{\circ}S, 90^{\circ}N]$ | *degrees North* | $[xdim_{grid},ydim_{grid}]$ |
| time     | seconds of observation since YYYY-MM-DD 00:00:00 UTC. | *seconds* | $[xdim_{grid},ydim_{grid}]$ |
| SM             | Soil Moisture         | $m^3/m^3$ | $[xdim_{grid},ydim_{grid}]$ |
| SM_err    | Uncertainty associated with the soil moisture measurement  | $m^3/m^3$ | $[xdim_{grid},ydim_{grid}]$ |
| VOD  | Vegetation Optical Depth      | -         | $[xdim_{grid},ydim_{grid}]$ |
| VOD_err    | Uncertainty associated with the vegetation optical depth measurement  | - | $[xdim_{grid},ydim_{grid}]$ |
| albedo    | Vegetation single scattering albedo  | - | $[xdim_{grid},ydim_{grid}]$ |
| EASE row index            | Row index in the global EASE2 grid                   | -         | $[xdim_{grid},ydim_{grid}]$ |
| EASE column index         | Column index in the global EASE2 grid                | -         | $[xdim_{grid},ydim_{grid}]$ |
| status_flags             | Identification of urban and frozen areas, permanent ice/snow and topographic effects.                      | *4-bits flag*      | $[xdim_{grid},ydim_{grid}]$ |