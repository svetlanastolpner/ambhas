# Content #


# Copula #
Copula is a function to couple two CDFs. The library is called using
```
ambhas.copula
```
The library has three copulas namely Clayton, Frank and Gumbel.

```
# import the required libraries
import numpy as np
from ambhas.copula import Copula  
import matplotlib.pyplot as plt

# generate some pseudo observations
x = np.random.uniform(size=(100))
y = np.log(x) + 0.5*np.random.normal(size=(100))

# use the copula
foo = Copula(x, y, 'clayton')
foo.tau

# generate ensemble using copula
x1,y1 = foo.generate_xy()

# plot the ensemble along with observations
plt.figure(figsize=(6, 4.5))
plt.scatter(x1,y1, color='g', label='Generated ensemble')  
plt.scatter(x,y, color='r', label='Observations')  
plt.xlabel('x')  
plt.ylabel('y')
plt.legend(loc='best')
plt.savefig('/home/tomer/svn/ambhas-wiki/images/copula.png')  

```

![http://ambhas.googlecode.com/svn/wiki/images/copula.png](http://ambhas.googlecode.com/svn/wiki/images/copula.png)

In the similar way 'frank' and 'gumbel' copula can also be used.

# csglm #
> CSGLM (Coupled Surface-Ground water Lumped Model) is a model developed for modelling linkage between surface and groundwater.

# easy\_gw\_1d #
This is an easy implementation of the one dimensional groundwater model. The data is passed to the module using the xls file and the output is also written in xls.

```
in_file = '/home/tomer/svn/ambhas/examples/input_easy_gw.xls'
out_file = '/home/tomer/svn/ambhas/examples/output/easy_gw.xls'    figure_dir = '/home/tomer/svn/ambhas-wiki/images/'
foo = gw_model_file(in_file, out_file, figure_dir)
```
Each worksheet in the input file is for each of the monitoring well. You can model as many as well you want by providing separate worksheet for them. You can fit the simulated groundwater level with the observed one by modifying the parameters. For the default parameters the output is:

![http://ambhas.googlecode.com/svn/wiki/images/berambadi.png](http://ambhas.googlecode.com/svn/wiki/images/berambadi.png)

The output file by the model are provided only for quick comparison. You can use the output xls file to make a better plot for your work.


# errlib #
This provides some simple error indices e.g. rmse, bias, NS coefficient etc.

```
#generate two random variable
obs = np.random.normal(size=100)    
sim = np.random.normal(size=100)  
   
# print error indices
print(pc_bias(sim,obs))
print(apb(sim,obs)) 
print(rmse(sim,obs)) 
print(mae(sim,obs)) 
print(bias(sim,obs)) 
print(NS(sim,obs)) 
print(L(sim,obs))
print(correlation(sim,obs))
```


# extract gis data #
Useful for extracting data over few locations from multiple gis file.
The script is written to extract the data from multiple points in the monitoring plots and then compute median and std over them. This becomes quiet useful when the shape of the monitoring plots is not regular. For instance see the monitoring plot below:

![http://ambhas.googlecode.com/svn/wiki/images/data_extraction.png](http://ambhas.googlecode.com/svn/wiki/images/data_extraction.png)

The points are generated at 5 m interval in both (x and y) direction.
The blue line shows the boundary of the plot. The + marks are the points inside the plot, on which data is extracted.

One example xls file for the monitoring field co-ordinate is given in example directory by the file name "plots\_66.xls", which is for the 66 monitoring plots of AMBHAS project.

Lets say you want to extract the data from all the images inside the directory for these 66 plots.

```
# import required libraries
from ambhas.extract_gis_data import extract_gis
import os

# location of files
in_dir = '/home/tomer/RADARSAT/bc/'
out_file = '/home/tomer/RADARSAT/output/bc.xls'

# get the name of the files in the input directory
# if you want only one file, or some selected files in the directory
# you can provide the name of those files as a list in this variable
in_files = os.listdir(in_dir)
in_files.sort()

# starting letters of the address for the RADARSAT files
ds_in = []
n = len(in_files)
for i in range(n):
    in_file = in_dir + in_files[i]
      
    ds_in.append(in_file)
   
xls_in = '/home/tomer/berambadi/plots_66/plots_66.xls'

    
extract_gis(xls_in, out_file, ds_in, in_files)
```
The output will be saved in the out\_file (a xls file) defined by you. The columns represents the different images. Row represents the different monitoring plots.

# gis #
Some GIS tool e.g. conversion of coordinate system from latlong to UTM.

## UTM to latlon ##
```
x = 60000
y = 1200000
lat,lon = utm2deg(x,y,utmzone=43)
```

## latlon to UTM ##
```
lat = 71
lon = 11
x,y = deg2utm(lat,lon,utmzone=43)
```

## Compute the area in square meters given the longitude of the center and size of the grid ##
```
lat = 11
size_cell = 0.125
geodetic_area(11,0.5)
```
The output is in square meter.

## Length of one degree of latitude and longitude for different latitudes ##

```
>>> latitude_length(11)
110614.71345469992
>>> longitude_length(11)
109287.5573212734
```
Notice that that input is latitudes in both the cases. Closer to the equator the length of one degree of latitude and longitude becomes nearly equal. As you move away form the equator towards the pole, the length of latitude remain nearly same, however the longitude starts decreasing.

## Read/Write ASCII Grid file ##
ASCII/Esri grid file (http://en.wikipedia.org/wiki/Esri_grid) is used by various program to write the gis data.

```
fname = '/home/tomer/svn/ambhas/examples/sample_ascii_grid.grd'
data, header = read_ascii_grid(fname)
print data
print header
```
The print output is:
```
[[  nan   nan    5.    2.]
 [  nan   20.  100.   36.]
 [   3.    8.   35.   10.]
 [  32.   42.   50.    6.]
 [  88.   75.   27.    9.]
 [  13.    5.    1.   nan]]
{'ncols': 4, 'cellsize': 50.0, 'nrows': 6, 'xllcorner': 0.0, 'yllcorner': 0.0, 'NODATA_value': -9999}
```

Same way the data can be written in ascii format. The header data (e.g. ncols, cellsize etc.) should be in dictionary format.
```
fname = '/home/tomer/svn/ambhas/examples/sample_ascii_grid_out.grd'
write_ascii_grid(fname, data, header)
```



# gw #
Groundwater model.

# krige #
Variogram and Kriging analysis.

# radarsat #
Process RADARSAT-2 data.

# rain\_disagg #
Rainfall disaggregation model based on the random multiplicative cascade (RMC) approach.

# richards #
This module provides the numerical solution of Richards' equation in python. The description about Richards' equation can be found in http://en.wikipedia.org/wiki/Richards_equation.

This module reads the input from a xls file, run the module and write the output into netcdf format. The example xls file is given in the examples subdirectory by the name maddur.xls. Lets first plot the rainfall and PET from the input file.
```
import matplotlib.pyplot as plt
from ambhas.xls import xlsread

in_file = 'maddur.xls'
xls_file = xlsread(in_file)
doy = xls_file.get_cells('B2:B366','forcing')
rain = xls_file.get_cells('C2:C366','forcing')
pet = xls_file.get_cells('D2:D366','forcing')

plt.clf()
plt.bar(doy,rain, color='m', edgecolor='m', label='Rainfall')
plt.plot(pet, label='PET')
plt.grid(True)
plt.legend()
plt.xlabel('DOY')
plt.ylabel('mm')
plt.savefig('output/richards_forcing.png')
```

The resulted graph is:

![http://ambhas.googlecode.com/svn/wiki/images/richards_forcing.png](http://ambhas.googlecode.com/svn/wiki/images/richards_forcing.png)


Now lets run the model. The input to the model is given via input xls file. A sample input xls file is provided in the examples directory.
```
from ambhas.richards import RICHARDS_1D 
import matplotlib.pyplot as plt
from scipy.io import netcdf as nc

# change the default font size of matplotlib
params = {'axes.labelsize': 15, 
          'text.fontsize': 15,
          'legend.fontsize': 15,
          'xtick.labelsize': 15,
          'ytick.labelsize': 15,
          'text.usetex': False}
plt.rcParams.update(params)

# run the model
maddur = RICHARDS_1D('/home/tomer/svn/ambhas/examples/maddur.xls')

# read the output
output_file = nc.NetCDFFile(maddur.ofile_name, 'r')
theta = output_file.variables['sm'][:]
doy = range(1,367)
rain = output_file.variables['rain'][:]

# main plot
plt.close()
fig = plt.figure()
ax = plt.axes([0.125, 0.15, 0.75, 0.6])
ax.plot(doy,theta[0,:],'b')
ax.plot(doy,theta[20,:],'g')
ax.plot(doy,theta[39,:],'c')
ax.set_ylabel('Soil Moisture (v/v)')
ax.set_ylim(ymax=0.4)
ax.set_xlim(xmax=366)
ax.set_xlabel('DOY')
fig.canvas.draw()
	
# precipitation plot
ax2 = plt.twinx()
ax2.bar(doy,rain*86400*1000, label='Precipitation', color='m', edgecolor='m')
ax2.set_ylabel('Precipitation (mm)')
ax2.set_ylim(ymax=100)
ax2.set_xlim(xmax=366)
ax2.invert_yaxis()

p1 = plt.Rectangle((0, 0), 1, 1, fc="m")
p2 = plt.Rectangle((0, 0), 1, 1, fc="b")
p3 = plt.Rectangle((0, 0), 1, 1, fc="g")
p4 = plt.Rectangle((0, 0), 1, 1, fc="c")
leg = plt.legend([p1,p2,p3,p4], ["Precipitation","SM at surface", "SM at 1m", "SM at 2m"], loc=(0.01,0.4))
frame = leg.get_frame()
frame.set_alpha(0.5)

plt.savefig('/home/tomer/svn/ambhas-wiki/images/run_richards.png')

```
The resulted graph is:

![http://ambhas.googlecode.com/svn/wiki/images/run_richards.png](http://ambhas.googlecode.com/svn/wiki/images/run_richards.png)

The input xls file contain the following sheets:
### ind ###
This sheet controls the the row that should be read from various inputs.
For example, soil\_hyd\_par = 2 means that from the soil\_hyd\_par the second row (ignoring the header row) would be read by the program. Using this one can write all the parameters ones, and by just making changes in the ind run for different parameters.

soil\_hyd\_par => indices for soil\_hyd\_par
units => indices for the units
temporal\_info => indices for temporal\_info
spatial\_info  => indices for spatial\_info
initial\_condition => indices for initial\_condition
output\_par => indices for output\_par


### soil\_hyd\_par ###
ind => indices to easily see the which one of row is read. Changing these values does not change anything.

theta\_r => residual soil moisture (volume/volume)

theta\_s => saturated soil moisture (volume/volume)

alpha => inverse of capilary fringe thickness (1/m)

n => n parameter of VG model (-)

ks => saturated hydraulic conductivity (m/s)

l => l parameter of VG model (-)

evap\_0 => The soil moisture below which the soil evaporation would be zero. Usually it value if taken to be equal to the wilting point.

evap\_1 => The soil moisture above which the soil evaporation would be equal to the PET. Usually it value if taken to be equal to field capacity.

### forcing ###
year => year of forcing
doy = > DOY (Day Of Year) of the forcing
rain => precipitation in length unit
pet => potential evaporation in length unit
The unit of rain and pet could be either mm or m, and the unit must be specified in the 'unit' sheet accordingly.

### initial\_condition ###
ind => Indices to be used by 'ind' sheet. This is just for easy reference, changing this does not affect anything.

theta => Initial soil moisture (volume/volume). For time being, model only takes the initial uniform soil moisture.

### units ###
Note that the units apply for the forcing. Units for everything else should be m for Length and s for Time.

### temporal\_info ###
ind => Indices to be used by 'ind' sheet. This is just for easy reference, changing this does not affect anything.

dt\_flux => Duration of the forcing in seconds i.e. for a daily forcing its value would be 86400 and for hourly its value would be 3600.

final\_time => Total time in seconds. The time is counted from the beginning. Hence the model would always start from the first forcing and would go upto the time defined in this.

### spatial\_info ###
ind => Indices to be used by 'ind' sheet. This is just for easy reference, changing this does not affect anything.

no\_layer => Number of soil layers.
dz => thickness of one soil node.
These two in combination would control the total length of the soil profile.

### output\_par ###
ind => Indices to be used by 'ind' sheet. This is just for easy reference, changing this does not affect anything.
output\_filename => Name of the output file.

# soil\_texture #
Provides the soil type and soil hydraulic properties based on pedotransfer function given the percentage of sand, silt and clay.

# stats #
Some statistical functions

# sunlib #
Modules to compute sunrise, sunset time etc.

# xls #
Read and write data to xls files.