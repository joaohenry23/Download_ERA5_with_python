# Download ERA data with python

Since the ECMWF currently advice user to migrate from ERA-interim to ERA5 ([due ERA Interim is being phased out](https://apps.ecmwf.int/datasets/data/interim-full-daily/levtype=sfc/)) and also to [migrate from ECMWF Web API to CDS API](https://confluence.ecmwf.int/display/CKB/How+to+migrate+from+ECMWF+Web+API+to+CDS+API), in this tutorial I will teach you how to download ERA5 data with python using CDS API.

## Instalation

The CDS API can be installed with Conda or Pip, and it is recommended to install it within a python environment.

Installation using onda:
```
conda config --add channels conda-forge
conda install cdsapi
```

Installation using pip:
```
pip install cdsapi
```

## Creates an account

Creates a CDS account [here](https://cds.climate.copernicus.eu/user/register).

## Install the CDS API key

- You need to login to continue, login [here](https://cds.climate.copernicus.eu/user/login?destination=%2F%23!%2Fhome).
- After login, click [here](https://cds.climate.copernicus.eu/user).
- Scroll down in the pageweb until the API key section and copy the **UID** and **API Key** codes.
- Creates .cdsapirc file

  **For Linux**:
      creates the file $HOME/.cdsapirc

  **For Windows**:
      creates the file at %USERPROFILE%\\.cdsapirc

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;More information for windows [here](https://confluence.ecmwf.int/display/CKB/How+to+install+and+use+CDS+API+on+Windows) and [here](https://confluence.ecmwf.int/pages/viewpage.action?pageId=139068264).
<br>
- Copy the next code within .cdsapirc

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url: https://cds.climate.copernicus.eu/api/v2 <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;key: {**uid**}:{**api-key**}


For more detail see [here](https://cds.climate.copernicus.eu/api-how-to).

## Accept data licence and generate a basic CDS API script using the CDS web interface
- Go to the C3S [climate data store (CDS)](https://cds.climate.copernicus.eu/#!/home)
- On the top menu bar, click on **Datasets**.
- On the left-hand side menu, expand **Product type** and select product type of interest (e.g. **Seasonal forecasts** to get all seasonal forecasts data listed, **Reanalysis** for ERA5 datasets)
- Follow the dataset title link of interest to the full dataset record
- Accept dataset licence and generate basic CDS API script

In my case, I selected **Reanalysis** in **Product type**, and I selected 
**ERA5 hourly data on pressure levels from 1979 to present** as a dataset. Then, selected the variables that I want to download and I did click on **Show API request** to see the python script.


```python
import cdsapi

c = cdsapi.Client()

c.retrieve(
    'reanalysis-era5-pressure-levels',
    {
        'product_type': 'reanalysis',
        'format': 'netcdf',
        'variable': [
            'geopotential', 'potential_vorticity', 'temperature',
            'u_component_of_wind', 'v_component_of_wind',
        ],
        'pressure_level': [
            '100', '125', '150',
            '175', '200', '225',
            '250', '300', '350',
            '400', '450', '500',
            '550', '600', '650',
            '700', '750', '775',
            '800', '825', '850',
            '875', '900', '925',
            '950', '975', '1000',
        ],
        'year': '1979',
        'month': '01',
        'day': [
            '01', '02', '03',
            '04', '05', '06',
            '07', '08', '09',
            '10', '11', '12',
            '13', '14', '15',
            '16', '17', '18',
            '19', '20', '21',
            '22', '23', '24',
            '25', '26', '27',
            '28', '29', '30',
            '31',
        ],
        'time': [
            '00:00', '01:00', '02:00',
            '03:00', '04:00', '05:00',
            '06:00', '07:00', '08:00',
            '09:00', '10:00', '11:00',
            '12:00', '13:00', '14:00',
            '15:00', '16:00', '17:00',
            '18:00', '19:00', '20:00',
            '21:00', '22:00', '23:00',
        ],
        'area': [
            10, -120, -60,
            -20,
        ],
    },
    'download.nc')
```

Finally, I did run the python code from python environment where was installed CDS API, and I downloaded the ERA5 data.
