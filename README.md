# GEOL0013_Python_Practical

This is a directory with a collection of routines for the practical of week 4 of GEOL0013 Principles of Climate. 

We will be looking at reanalysis data from two of the main centres: 

NCEP (US): https://www.esrl.noaa.gov/psd/data/gridded/data.ncep.reanalysis.derived.surfaceflux.html

ERA5 (Europe): https://cds.climate.copernicus.eu/#!/home

Good-to-Know: What is the difference between Analysis, Forecast and Reanalysis?

An analysis, of the atmospheric conditions, is a blend of observations with a previous forecast. An analysis can only provide instantaneous parameters.

A forecast starts with an analysis at a specific time (the 'initialization time'), and a model computes the atmospheric conditions for many 'forecast steps', at increasing 'validity times', into the future. A forecast can provide instantaneous parameters (e.g. the temperature at the validity times), accumulated parameters (e.g. precipitation in a certain period up to the validity times) and min/max parameters (e.g. min/max of 2 metre temperature in a certain period up to the validity times).

A reanalysis gives a numerical description of the recent climate, produced by combining models with observations. A reanalysis typically extends over several decades or longer, and covers the entire globe from the Earth’s surface to well above the stratosphere. Reanalysis products are used extensively in climate research and services, including for monitoring and comparing current climate conditions with those of the past, identifying the causes of climate variations and change, and preparing climate predictions.

## Getting Started

All the jupyter notebooks can be run on your laptop 

ERA5_Maps/Maps_fields_GEOL0013_T2m.ipynb will read and plot averages and trends of the 2m surface temperature 

ERA5_Maps/Maps_fields_GEOL0013_total_precipitation.ipynb will read and plot averages and trends of the total surface precipitation 

ERA5_Maps/EOFs_fields_GEOL0013_T2m.ipynb will perform EOF analysis of the 2m temperature

Sample data are contained in 

ERA5_Data/

More extensive examples are provided in the following link

https://github.com/royalosyin/Python-Practical-Application-on-Climate-Variability-Studies

### Prerequisites

1.	Install anaconda (https://www.anaconda.com/distribution/#download-section) and select python 3.7

2.	To set up your environment type the following lines *exactly* into terminal/anaconda prompt:

conda create -n basemap_test python=3.6 basemap=1.2.0 proj4=5.2.0 scipy jupyter netCDF4=1.3.1 seaborn=0.9.0 pandas=0.23.4

Press Y to proceed 

conda activate basemap_test

conda install -c conda-forge cdsapi

Press Y to proceed

3.	In your home directory; create the cdsapirc file. 

Follow the instructions on https://cds.climate.copernicus.eu/api-how-to

You will need to create an account to generate a CDS API key. If you are not sure what your $HOME directory is, type $HOME into terminal/cmd. Ensure the url and key is copied *exactly* as it is, and that there is no file extension on the cdsapirc file you create.

If you are using mac, open TextEdit and paste the url and key, then click Format then Make Plain Text. Save the file as cdsapirc in $HOME, and in Finder click ‘Get Info’ and ensure there is no .txt extension, if there is then remove it. Open the cdsapirc file in TextEdit again and rename to .cdsapirc the file will now probably disappear from finder, this is normal.

If you are using windows, open Notepad and paste the url and key. Save this in your $HOME directory as .cdsapirc with no file extension.

4.	To test your installation, type jupyter notebook into your terminal/anaconda prompt. Create a new notebook, and in the first cell paste the following

from mpl_toolkits.basemap import Basemap, shiftgrid, interp, addcyclic

from netCDF4 import Dataset

from scipy import signal

import cdsapi

c = cdsapi.Client()

c.retrieve(

    'reanalysis-era5-single-levels-monthly-means',
    
    {
    
        'area'          : [60, -10, 50, 2], # North, West, South, East. Default: global
        
        'grid'          : [1.0, 1.0], # Latitude/longitude grid: east-west (longitude) and north-south resolution (latitude). Default: 0.25 x 0.25
        
        'product_type':'monthly_averaged_reanalysis',
        
        'variable':'2m_temperature',
        
        'year':'2003',
        
        'month':'06',
        
        'time':'00:00',
        
        'format':'netcdf'
        
    },
    
    'download_small.nc')

You will first need to agree to the terms and conditions. If you get an error with cdsapirc, ensure the file is in your $HOME directory, that it is formatted as plain text, that it does not have a file extension, and that you copied the url and key *exactly*. 

If this runs without error then you have successfully configured your environment. 

At any time, to enter your environment, type 

conda activate basemap_test

To exit your environment, type 

conda deactivate



For use of the exercises you must install dependencies (inside your environment) 

conda install -c conda-forge spectrum

conda install -c conda-forge eofs

conda install -c ulmo urllib3


You may also need to change the way files are loaded. If this is the case, insert the following in the fist cell of the notebook

import os

from pathlib import Path

p = Path(os.getcwd())

Next, change the filename from 

'data/<FILENAME>’

to

p.joinpath('Data/<FILENAME>')

Also, ensure that %matplotlib inline is not written as % matplotlib inline

5. In order to download ERA5 data yourselves you will need to set up an account on ECMWF as described in 3. 

Alternatively you can download sample data and test some example scripts from a web interface here

https://cds.climate.copernicus.eu/toolbox-editor/examples/02-plot-map

### Your turn

Using the techniques shown in the examples of this practical create your own simple jupyter notebook focusing on a region of your choice. You are encouraged to use one of the two reanalysis products used in this practical (ERA5 or NCEP). Illustrate some of the following: 

- Average values 
- Trends and significance 
- Climatology, seasonal cycle
- Compare with local and global values
- Compare with local and global CO2 levels 
- Apply higher level statistics (spectral analysis)
- Attempt a forecast 
- Compare results between different reanalysis
- Provide some brief text to put your results in context 
- Provide references for your region of interest

Please upload your jupyter notebook on Moodle for Thursday 27/2. You will be marked (10% of module) on clarity of presentation, relevant and concise literature, and of course your code should be running without issues.  


Help to get you going using examples (ex01, ex02…) provided in:
https://github.com/royalosyin/Python-Practical-Application-on-Climate-Variability-Studies

1)	Download and read data
- ex01 shows how to # Read SST NetCDF data, Subsample and Save regional array 
- Maps_fields_GEOL0013_T2m shows how to download NCEP data 

Hint: to find out what are the variable names in a netcdf file that you have downloaded use code in ex01: 

fh     = Dataset(ncfile, mode='r') # file handle, open in read only mode
print(fh)
fh.close() # close the file

Hint: NCEP2 data can be downloaded from 
https://www.esrl.noaa.gov/psd/data/gridded/data.ncep.reanalysis2.gaussian.html
ftp://ftp.cdc.noaa.gov/Datasets/ncep.reanalysis.derived/surface_gauss/

i.e 2m air temperature is ftp://ftp.cdc.noaa.gov/Datasets/ncep.reanalysis.derived/surface_gauss/air.2m.mon.mean.nc

Hint: ERA5 data can be downloaded from:
https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels-monthly-means?tab=overview
example CDS API:
import cdsapi

c = cdsapi.Client()

c.retrieve(
    'reanalysis-era5-single-levels-monthly-means',
    {
        'product_type': 'monthly_averaged_reanalysis',
        'variable': [
            '10m_u_component_of_wind', '10m_v_component_of_wind', '2m_temperature',
            'sea_surface_temperature', 'snowfall', 'total_precipitation',
        ],
        'year': [
            '1979', '1980', '1981',
            '1982', '1983', '1984',
            '1985', '1986', '1987',
            '1988', '1989', '1990',
            '1991', '1992', '1993',
            '1994', '1995', '1996',
            '1997', '1998', '1999',
            '2000', '2001', '2002',
            '2003', '2004', '2005',
            '2006', '2007', '2008',
            '2009', '2010', '2011',
            '2012', '2013', '2014',
            '2015', '2016', '2017',
            '2018', '2019',
        ],
        'month': [
            '01', '02', '03',
            '04', '05', '06',
            '07', '08', '09',
            '10', '11', '12',
        ],
        'time': '00:00',
        'format': 'netcdf',
    },
    'download.nc')


Hint: CMIP5 if you want to go further and look at the future conditions of your regions you can download CMIP5 data from:
https://cds.climate.copernicus.eu/cdsapp#!/dataset/projections-cmip5-monthly-single-levels?tab=overview
example CDS API:
import cdsapi

c = cdsapi.Client()

c.retrieve(
    'projections-cmip5-monthly-single-levels',
    {
        'ensemble_member': 'r3i1p1',
        'format': 'zip',
        'variable': '2m_temperature',
        'experiment': 'rcp_8_5',
        'model': 'hadgem2_cc',
        'period': [
            '200512-203011', '203012-205511', '205512-208011',
            '208012-209912',
        ],
    },
    'download.zip')
You might need a recent file extractor (unzip did not work for me and I used Keka https://www.keka.io/en/). 

2)	Select a region of interest and produce time series 
- ex02 shows how to # Read SST data, Create and Save regional time series

Hint: change the input file in ex02, the number of years (currently 30), the lat-lon boundaries to produce a timeseries of your field and region of interest. For example for the eastern Mediterranean sector for the whole 1970 to 2019 period of ERA5 I get: 
 

## Authors

* **Michel Tsamados** 
* **William Gregory**
* **Thomas Johnson**

## Acknowledgments

The series of python practicals were developped by Chonghua Yin https://github.com/royalosyin
