# Planette's C3S Seasonal Forecast Data

<div align="center">
  <img src="planette_icon.png" alt="Planette Logo" width="200"/>
</div>

## Overview

The C3S seasonal forecast dataset provides global, daily, probabilistic forecasts of the Earth system, enabling users to assess the likelihood of future climate states. These forecasts are particularly valuable for studying slowly evolving climate patterns such as El Niño, La Niña, and the North Atlantic Oscillation (NAO), which can be predicted with greater skill than the chaotic atmosphere.

This dataset is derived from the Copernicus Climate Change Service (C3S) archive and includes SEAS5 hindcasts (1981-2016) and forecasts (2017-present) at 1°x1° global resolution. More models from the C3S archive will be updated as they are processed into cloud native format.

The planette C3S archive stores this data in cloud native format for easy access and analysis using Zarr format with icechunk for efficient cloud access.

## Variables

### Atmospheric Variables at Pressure Levels

| Variable                | Units     | Description                                 |
|-------------------------|-----------|---------------------------------------------|
| **Temperature (K)**     |           |                                             |
| t1000                   | K         | Temperature at 1000 hPa                     |
| t925                    | K         | Temperature at 925 hPa                      |
| t850                    | K         | Temperature at 850 hPa                      |
| t700                    | K         | Temperature at 700 hPa                      |
| t500                    | K         | Temperature at 500 hPa                      |
| t400                    | K         | Temperature at 400 hPa                      |
| t300                    | K         | Temperature at 300 hPa                      |
| t200                    | K         | Temperature at 200 hPa                      |
| t100                    | K         | Temperature at 100 hPa                      |
| **Specific Humidity (g/kg)** |       |                                             |
| q1000                   | g/kg      | Specific humidity at 1000 hPa               |
| q925                    | g/kg      | Specific humidity at 925 hPa                |
| q850                    | g/kg      | Specific humidity at 850 hPa                |
| q700                    | g/kg      | Specific humidity at 700 hPa                |
| q500                    | g/kg      | Specific humidity at 500 hPa                |
| q400                    | g/kg      | Specific humidity at 400 hPa                |
| q300                    | g/kg      | Specific humidity at 300 hPa                |
| q200                    | g/kg      | Specific humidity at 200 hPa                |
| q100                    | g/kg      | Specific humidity at 100 hPa                |
| **Zonal Wind (m/s)**    |           |                                             |
| u850                    | m/s       | Zonal wind at 850 hPa                       |
| u500                    | m/s       | Zonal wind at 500 hPa                       |
| u200                    | m/s       | Zonal wind at 200 hPa                       |
| **Meridional Wind (m/s)** |         |                                             |
| v850                    | m/s       | Meridional wind at 850 hPa                  |
| v500                    | m/s       | Meridional wind at 500 hPa                  |
| v200                    | m/s       | Meridional wind at 200 hPa                  |

### Surface and Column Variables

| Variable                | Units     | Description                                 |
|-------------------------|-----------|---------------------------------------------|
| t2m                     | K         | 2 meter temperature                         |
| t2m_max                 | K         | Daily maximum 2 meter temperature           |
| t2m_min                 | K         | Daily minimum 2 meter temperature           |
| u10m                    | m/s       | 10 meter zonal wind                         |
| v10m                    | m/s       | 10 meter meridional wind                    |
| u100m                   | m/s       | 100 meter zonal wind                        |
| v100m                   | m/s       | 100 meter meridional wind                   |
| t2d                     | K         | 2 meter dew point temperature               |
| slp                     | hPa       | Sea level pressure                          |
| SST                     | K         | Sea surface temperature                     |
| tsoil                   | K         | Soil temperature (top layer)                |
| olr                     | W/m²      | Top of atmosphere net thermal radiation     |
| tcwv                    | kg/m²     | Total column water vapor                    |

## Temporal Coverage

- **SEAS5 Hindcasts:** 1981–2016
- **SEAS5 Forecasts:** 2017–present
- (Additional models may be added in the future.)

## Spatial Coverage

- **Global** coverage at **1°x1°** resolution

## Data Format and Access

- **Format:** Zarr (written with [icechunk](https://github.com/earth-mover/icechunk))
- **Storage:** Amazon S3 (`s3://planettebaikal/forecast_models/seasonal/seas5/prod/sys51/hindcasts/`)
- **Organization:** Data is organized by variable, frequency, grid, and year.  
  Example:  
  `s3://planettebaikal/forecast_models/seasonal/seas5/prod/sys51/hindcasts/pr/day/1latx1lon/seas5_sys51_pr_day_1latx1lon_1990.zarr`

### Data Structure

```
s3://planettebaikal/forecast_models/seasonal/seas5/prod/sys51/hindcasts/
├── {variable}/
│   └── day/
│       └── 1latx1lon/
│           └── seas5_sys51_{variable}_day_1latx1lon_{year}.zarr
```

## Getting Started

### Prerequisites

Install the required Python packages:

```bash
pip install xarray zarr s3fs
```

### Quick Start Example

```python
import xarray as xr
import s3fs

# Set up S3 access (no credentials needed for public data)
fs = s3fs.S3FileSystem(anon=True)

# Define path to a Zarr store (example: precipitation for 1990)
zarr_path = 's3://planettebaikal/forecast_models/seasonal/seas5/prod/sys51/hindcasts/pr/day/1latx1lon/seas5_sys51_pr_day_1latx1lon_1990.zarr'

# Open the dataset
ds = xr.open_zarr(fs.get_mapper(zarr_path), consolidated=True)

# Explore the data
print(ds)
```

For a complete tutorial, see the [Jupyter notebook tutorial](c3s_seasonal_forecast_tutorial.ipynb) in this repository.

## Usage Notes

- Data is provided as-is from the C3S archive.
- Please cite the Copernicus Climate Change Service (C3S) and relevant publications (see below).
- License: [Copernicus Licence](https://cds.climate.copernicus.eu/api/v2/terms/static/licence-to-use-copernicus-products.pdf) (similar to CC-BY-4.0).

## Contact

For questions about this dataset or the Planette C3S archive:
- **Email:** aodhan.sweeney@planette.ai
- **Organization:** Planette.ai

## References

- Copernicus Climate Change Service (C3S) (2017): C3S seasonal forecast data. [CDS Dataset](https://cds.climate.copernicus.eu/cdsapp#!/dataset/seasonal-original-single-levels)
- Johnson, S. J., et al. (2019). SEAS5: The new ECMWF seasonal forecast system. *Geoscientific Model Development*, 12(3), 1087–1117. [https://doi.org/10.5194/gmd-12-1087-2019](https://doi.org/10.5194/gmd-12-1087-2019)
- [C3S Seasonal Forecasts Documentation](https://climate.copernicus.eu/seasonal-forecasts)

## Acknowledgments

This work is supported by the Copernicus Climate Change Service (C3S) and the AWS Open Data Sponsorship Program. 