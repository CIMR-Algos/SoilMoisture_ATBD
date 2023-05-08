# Algorithm Input and Output Data Definition (IODD)


### Input data

| Parameter                               | Unit | Description                                                                         |
|-----------------------------------------|------|-------------------------------------------------------------------------------------|
| Surface temperature                     | -    |                                                                                     |
| Soil texture (clay fraction)            | -    |                                                                                     |
| Land cover type classification          | -    |                                                                                     |
| Vegetation single scattering albedo     | -    |                                                                                     |
| Surface roughness information           | -    |                                                                                     |
| Data flags                              | -    | Identification of land, water, urban areas, permanent ice/snow and topographic effects. |


### Output data

| Parameter                   | Unit          | Description                                                |
|-----------------------------|---------------|------------------------------------------------------------|
| Vegetatio_optical_depth    | -             |                                                            |
| Latitude                    | degrees       |                                                            |
| Longitude                   | degrees       |                                                            |
| Soil_Moisture               | $m^3/m^3$     |                                                            |
| Soil_Moisture_StdError      | $m^3/m^3$     | Error on the derived Soil moisture                         |
| Quality flags               | -             |                                                            |
| Surface_Temperature | -             |                                                            |
| Days                        | -             |                                                            |
| UTC_time         | seconds -             |                                                            |
| EASE row index              | -             | Global 36-km EASE2 Grid 0-based row index EASE_column_index|
| EASE column index           | -             | Global 36-km EASE2 Grid 0-based column index EASE_column_index|
| Single scattering albedo    | -             |                                                            |
| Soil roughness              | -             |                                                            |
| Clay fraction               | -             |                                                            |



### Ancillary data

| ID | Parameter                   | Data Source                                                                                                      |
|----|-----------------------------|------------------------------------------------------------------------------------------------------------------|
| 1  | h Roughness Parameter (file)| pre-computed from SMOS-IC                                                                                        |
| 2  | Permanent Ice              | MODIS                                                                                                            |
| 3  | Mountainous Area [DEM]     | -                                                                                                                |
| 4  | Land Cover Class           | MODIS IGBP                                                                                                       |
| 5  | Soil Attributes (clay fraction) | FAO [replaced in R17 2020 data release by SoilGrid250m available at https://openlandmap.org]      

