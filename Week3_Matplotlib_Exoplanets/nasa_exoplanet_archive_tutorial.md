# NASA Exoplanet Archive Tutorial
## Week 3, Intro-to-Astro 2024

-1. What is the [Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/)?

In this tutorial, together we will: 

0. See an example of an exoplanet mass-radius diagram using data from the Exoplanet Archive

1. Learn how to **download** data from the Exoplanet Archive ourselves

2. **Visualize** the radius distribution of transiting exoplanets discovered by the *Kepler* space mission

On your own you will

3. Create an **orbital period vs radius** diagram for the transiting exoplanets from *Kepler*. Comment on any features that you see.

4. Create one more plot of **any two parameters** that you'd like. Write a few sentences explaining why you plotted the parameters you chose. Comment on any features that appear.

For your plots in 3. and 4. make sure you **label your axes with units** and **choose useful axis scales**!




```python
# It's always nice to have a cell at the top of your jupyter notebook for housing all of your import statements
# and "magic commands" (lines that start with %), if any.

# For bonus points: add a comment to your future self on why you're importing each module

# File/directory handling
import os 

# Data handling
import pandas as pd
pd.set_option('display.max_columns', None) # Display all of the columns of a DataFrame

# For math
import numpy as np

# Plotting
import matplotlib.pyplot as plt

# "Magic command" to make the plots appear *inline* in the notebook
%matplotlib inline
```

# 0. Mass-radius diagram example


```python
# Create a variable for the path to the file containing our example's data
data_dir = 'data'
data_fname = 'confirmed_example.csv'
data_path = os.path.join(data_dir, data_fname) # os.path.join creates a valid path out of the directory and filename
print(f'Data will be loaded from: {data_path}')
```

    Data will be loaded from: data/confirmed_example.csv
    


```python
# Load the data that's stored in the .csv file from the cell above (csv == comma separated values).
# Load this data into a Pandas DataFrame object using the Pandas function read_csv().

# Use comment keyword argument in read_csv() to ignore the file header (more on this in a minute)
example_data = pd.read_csv(data_path, comment='#')
```


```python
example_data.head() # What sort of data is contained in this pandas.DataFrame?
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pl_hostname</th>
      <th>pl_letter</th>
      <th>pl_name</th>
      <th>pl_discmethod</th>
      <th>pl_controvflag</th>
      <th>pl_pnum</th>
      <th>pl_orbper</th>
      <th>pl_orbpererr1</th>
      <th>pl_orbpererr2</th>
      <th>pl_orbperlim</th>
      <th>pl_orbsmax</th>
      <th>pl_orbsmaxerr1</th>
      <th>pl_orbsmaxerr2</th>
      <th>pl_orbsmaxlim</th>
      <th>pl_orbeccen</th>
      <th>pl_orbeccenerr1</th>
      <th>pl_orbeccenerr2</th>
      <th>pl_orbeccenlim</th>
      <th>pl_orbincl</th>
      <th>pl_orbinclerr1</th>
      <th>pl_orbinclerr2</th>
      <th>pl_orbincllim</th>
      <th>pl_bmassj</th>
      <th>pl_bmassjerr1</th>
      <th>pl_bmassjerr2</th>
      <th>pl_bmassjlim</th>
      <th>pl_bmassprov</th>
      <th>pl_radj</th>
      <th>pl_radjerr1</th>
      <th>pl_radjerr2</th>
      <th>pl_radjlim</th>
      <th>pl_dens</th>
      <th>pl_denserr1</th>
      <th>pl_denserr2</th>
      <th>pl_denslim</th>
      <th>pl_ttvflag</th>
      <th>pl_kepflag</th>
      <th>pl_k2flag</th>
      <th>pl_nnotes</th>
      <th>ra_str</th>
      <th>ra</th>
      <th>dec_str</th>
      <th>dec</th>
      <th>st_dist</th>
      <th>st_disterr1</th>
      <th>st_disterr2</th>
      <th>st_distlim</th>
      <th>gaia_dist</th>
      <th>gaia_disterr1</th>
      <th>gaia_disterr2</th>
      <th>gaia_distlim</th>
      <th>st_optmag</th>
      <th>st_optmagerr</th>
      <th>st_optmaglim</th>
      <th>st_optband</th>
      <th>gaia_gmag</th>
      <th>gaia_gmagerr</th>
      <th>gaia_gmaglim</th>
      <th>st_teff</th>
      <th>st_tefferr1</th>
      <th>st_tefferr2</th>
      <th>st_tefflim</th>
      <th>st_mass</th>
      <th>st_masserr1</th>
      <th>st_masserr2</th>
      <th>st_masslim</th>
      <th>st_rad</th>
      <th>st_raderr1</th>
      <th>st_raderr2</th>
      <th>st_radlim</th>
      <th>rowupdate</th>
      <th>pl_facility</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2MASS J02192210-3925225</td>
      <td>b</td>
      <td>2MASS J02192210-3925225 b</td>
      <td>Imaging</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>156.00000</td>
      <td>10.00000</td>
      <td>-10.00000</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13.90000</td>
      <td>1.10000</td>
      <td>-1.10000</td>
      <td>0</td>
      <td>Mass</td>
      <td>1.44</td>
      <td>0.030</td>
      <td>-0.030</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>02h19m22.11s</td>
      <td>34.842106</td>
      <td>-39d25m22.5s</td>
      <td>-39.422928</td>
      <td>39.40</td>
      <td>2.60</td>
      <td>-2.60</td>
      <td>0</td>
      <td>40.09</td>
      <td>0.19</td>
      <td>-0.19</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>15.012</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>3064.0</td>
      <td>76.0</td>
      <td>-76.0</td>
      <td>0.0</td>
      <td>0.11</td>
      <td>0.01</td>
      <td>-0.01</td>
      <td>0.0</td>
      <td>0.28</td>
      <td>0.01</td>
      <td>-0.01</td>
      <td>0.0</td>
      <td>2015-05-14</td>
      <td>Cerro Tololo Inter-American Observatory</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2MASS J21402931+1625183 A</td>
      <td>b</td>
      <td>2MASS J21402931+1625183 A b</td>
      <td>Imaging</td>
      <td>0</td>
      <td>1</td>
      <td>7336.500000</td>
      <td>1934.500000</td>
      <td>-584.000000</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.26</td>
      <td>0.06</td>
      <td>-0.06</td>
      <td>0.0</td>
      <td>46.20</td>
      <td>2.50</td>
      <td>-8.70</td>
      <td>0.0</td>
      <td>20.95000</td>
      <td>83.79000</td>
      <td>-20.95000</td>
      <td>0</td>
      <td>Mass</td>
      <td>0.92</td>
      <td>0.390</td>
      <td>-0.360</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>21h40m29.32s</td>
      <td>325.122159</td>
      <td>+16d25m18.3s</td>
      <td>16.421759</td>
      <td>25.00</td>
      <td>10.00</td>
      <td>-10.00</td>
      <td>0</td>
      <td>33.12</td>
      <td>0.48</td>
      <td>-0.48</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>17.396</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>2300.0</td>
      <td>80.0</td>
      <td>-80.0</td>
      <td>0.0</td>
      <td>0.08</td>
      <td>0.06</td>
      <td>-0.06</td>
      <td>0.0</td>
      <td>0.12</td>
      <td>0.05</td>
      <td>-0.04</td>
      <td>0.0</td>
      <td>2014-05-14</td>
      <td>W. M. Keck Observatory</td>
    </tr>
    <tr>
      <th>2</th>
      <td>55 Cnc</td>
      <td>e</td>
      <td>55 Cnc e</td>
      <td>Radial Velocity</td>
      <td>0</td>
      <td>5</td>
      <td>0.736539</td>
      <td>0.000007</td>
      <td>-0.000007</td>
      <td>0.0</td>
      <td>0.01544</td>
      <td>0.00009</td>
      <td>-0.00009</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>83.30</td>
      <td>0.90</td>
      <td>-0.80</td>
      <td>0.0</td>
      <td>0.02542</td>
      <td>0.00098</td>
      <td>-0.00098</td>
      <td>0</td>
      <td>Mass</td>
      <td>0.17</td>
      <td>0.007</td>
      <td>-0.007</td>
      <td>0</td>
      <td>6.4</td>
      <td>0.8</td>
      <td>-0.7</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>08h52m35.81s</td>
      <td>133.149216</td>
      <td>+28d19m50.9s</td>
      <td>28.330818</td>
      <td>12.59</td>
      <td>0.01</td>
      <td>-0.01</td>
      <td>0</td>
      <td>12.59</td>
      <td>0.01</td>
      <td>-0.01</td>
      <td>0.0</td>
      <td>5.960</td>
      <td>0.010</td>
      <td>0.0</td>
      <td>V (Johnson)</td>
      <td>5.714</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>5196.0</td>
      <td>24.0</td>
      <td>-24.0</td>
      <td>0.0</td>
      <td>0.91</td>
      <td>0.01</td>
      <td>-0.01</td>
      <td>0.0</td>
      <td>0.94</td>
      <td>0.01</td>
      <td>-0.01</td>
      <td>0.0</td>
      <td>2019-01-31</td>
      <td>McDonald Observatory</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BD+20 594</td>
      <td>b</td>
      <td>BD+20 594 b</td>
      <td>Transit</td>
      <td>0</td>
      <td>1</td>
      <td>41.685500</td>
      <td>0.003000</td>
      <td>-0.003000</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>89.55</td>
      <td>0.16</td>
      <td>-0.16</td>
      <td>0.0</td>
      <td>0.07000</td>
      <td>0.03000</td>
      <td>-0.03000</td>
      <td>0</td>
      <td>Mass</td>
      <td>0.23</td>
      <td>0.010</td>
      <td>-0.010</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>03h34m36.23s</td>
      <td>53.650967</td>
      <td>+20d35m57.2s</td>
      <td>20.599232</td>
      <td>180.39</td>
      <td>1.25</td>
      <td>-1.25</td>
      <td>0</td>
      <td>180.39</td>
      <td>1.25</td>
      <td>-1.25</td>
      <td>0.0</td>
      <td>11.038</td>
      <td>0.047</td>
      <td>0.0</td>
      <td>V (Johnson)</td>
      <td>10.864</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>5766.0</td>
      <td>99.0</td>
      <td>-99.0</td>
      <td>0.0</td>
      <td>1.67</td>
      <td>0.40</td>
      <td>-0.40</td>
      <td>0.0</td>
      <td>1.08</td>
      <td>0.06</td>
      <td>-0.06</td>
      <td>0.0</td>
      <td>2018-04-26</td>
      <td>K2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CT Cha</td>
      <td>b</td>
      <td>CT Cha b</td>
      <td>Imaging</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>440.00000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>17.00000</td>
      <td>6.00000</td>
      <td>-6.00000</td>
      <td>0</td>
      <td>Mass</td>
      <td>2.20</td>
      <td>0.810</td>
      <td>-0.600</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>11h04m09.09s</td>
      <td>166.037877</td>
      <td>-76d27m19.4s</td>
      <td>-76.455383</td>
      <td>165.00</td>
      <td>30.00</td>
      <td>-30.00</td>
      <td>0</td>
      <td>191.78</td>
      <td>0.78</td>
      <td>-0.78</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.773</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2014-05-14</td>
      <td>Paranal Observatory</td>
    </tr>
  </tbody>
</table>
</div>




```python
# What are the dimensions of this DataFrame?
print(example_data.shape) # prints (# of rows, # of columns)
```

    (762, 72)
    

What do all of these columns mean? Let's checkout the .csv file itself for more information!

...

So we saw that this table was generated from the Exoplanet Archive, and whoever generated it enforced some constraints: the mass and radius (in units of Jupiter mass and radius) must **not** be *null* i.e. we only want to download planets that have both a radius **and** a mass measurement.

We also saw that there were a **ton** of columns that we didn't really mention in that table, and some are a little more useful than others. That's okay, for now let's just load *all* of the data into a Pandas DataFrame and then we can choose which columns we actually want to use.


```python
# Make a quick mass-radius diagram!

# Plot the data with mass (in units of Jupiter masses) on the x-axis and radius 
# (in units of Jupiter radii) on the y-axis.

# '.'   --> use small unconnected dots to plot the data points
# alpha --> governs the transparency of each datapoint: 0 = completely transparent, 1 = completely opaque.
# (alpha = 0.3 will let us see the density of the data points more easily)
plt.plot(example_data['pl_bmassj'], example_data['pl_radj'], '.', alpha=0.3)

# At the end, show the plot
plt.show()
```


    
![png](output_8_0.png)
    


The plot above is alright, but it seems like a lot of data is bunched up at low masses and the large range of the data makes it hard to see the finer structure... let's replot things with **log axes**. Let's also add **axis labels** so we actually know what is being plotted. As a bonus, we'll add some text to the plot to show **how many** exoplanets are being included.


```python
# Plot the data with mass (in units of Jupiter masses) on the x-axis
plt.plot(example_data['pl_bmassj'], example_data['pl_radj'], '.', alpha=0.3)

# Label your axes with units!
plt.xlabel('Mass [M$_J$]', fontsize=14)   # You can use LaTex formatting to make your plots look more professional...
plt.ylabel('Radius [R$_J$]', fontsize=14) # more on LaTex later in the summer! $_J$ creates a subscript J for Jupiter

# Add some text to the plot so we know how many data points there are
n_planets = len(example_data) # Number of planets being plotted

# plt.text(x-coordinate, y-coordinate, text string, **kwargs) # x and y coordinates are in data units
plt.text(1e-3, 1, f'N = {n_planets}', fontsize=14)

# Set the x and y-axis scales to be log so we can see structure more easily
plt.xscale('log')
plt.yscale('log')

# At the end, show the plot
plt.show()
```


    
![png](output_10_0.png)
    


# 1. Downloading a sample of exoplanets subject to constraints

Now that we've plotted some of the data from the Exoplanet Archive, how do we download it ourselves? How did I get that complicated-looking .csv file with all of the data in the first place?

Our mission: 

1. Go to the [Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/) website and navigate to the **Confirmed Planets** table after clicking the **Data** tab at the top of the home page.
2. This time, let's **add columns** so we have **planet mass and radius** in **units of Earth mass and Earth radius**
3. Enter constraints into the query boxes at the top of the columns to get all of the planets **discovered by *Kepler*** via the **transit method**. Make sure to **exclude** any rows that might have a **null radius measurement** for one reason or another.
4. **Download** the table as a **.csv** and **save** it to a **subdirectory** named **"data"**. Be sure to give your .csv file an informative name.

# 2. Visualize the radius distribution of transiting *Kepler* planets


```python
# Like before, let's first create the path to our new .csv file
data_fname = 'confirmed_kepler_transiting.csv'
data_path = os.path.join(data_dir, data_fname)
print(f'Data will be loaded from: {data_path}')
```

    Data will be loaded from: data/confirmed_kepler_transiting.csv
    


```python
# Load the data into a Pandas DataFrame object
kepler_data = pd.read_csv(data_path, comment='#')

# How many rows and columns do we have this time?
print(kepler_data.shape)
```

    (2317, 80)
    

Any idea why we have so many rows of planets this time than before? 

*Hint: We the confirmed_example.csv file was generated with a constraint on a different planet parameter that we didn't include here*


```python
kepler_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pl_hostname</th>
      <th>pl_letter</th>
      <th>pl_name</th>
      <th>pl_discmethod</th>
      <th>pl_controvflag</th>
      <th>pl_pnum</th>
      <th>pl_orbper</th>
      <th>pl_orbpererr1</th>
      <th>pl_orbpererr2</th>
      <th>pl_orbperlim</th>
      <th>pl_orbsmax</th>
      <th>pl_orbsmaxerr1</th>
      <th>pl_orbsmaxerr2</th>
      <th>pl_orbsmaxlim</th>
      <th>pl_orbeccen</th>
      <th>pl_orbeccenerr1</th>
      <th>pl_orbeccenerr2</th>
      <th>pl_orbeccenlim</th>
      <th>pl_orbincl</th>
      <th>pl_orbinclerr1</th>
      <th>pl_orbinclerr2</th>
      <th>pl_orbincllim</th>
      <th>pl_bmassj</th>
      <th>pl_bmassjerr1</th>
      <th>pl_bmassjerr2</th>
      <th>pl_bmassjlim</th>
      <th>pl_bmassprov</th>
      <th>pl_radj</th>
      <th>pl_radjerr1</th>
      <th>pl_radjerr2</th>
      <th>pl_radjlim</th>
      <th>pl_dens</th>
      <th>pl_denserr1</th>
      <th>pl_denserr2</th>
      <th>pl_denslim</th>
      <th>pl_ttvflag</th>
      <th>pl_kepflag</th>
      <th>pl_k2flag</th>
      <th>pl_nnotes</th>
      <th>ra_str</th>
      <th>ra</th>
      <th>dec_str</th>
      <th>dec</th>
      <th>st_dist</th>
      <th>st_disterr1</th>
      <th>st_disterr2</th>
      <th>st_distlim</th>
      <th>gaia_dist</th>
      <th>gaia_disterr1</th>
      <th>gaia_disterr2</th>
      <th>gaia_distlim</th>
      <th>st_optmag</th>
      <th>st_optmagerr</th>
      <th>st_optmaglim</th>
      <th>st_optband</th>
      <th>gaia_gmag</th>
      <th>gaia_gmagerr</th>
      <th>gaia_gmaglim</th>
      <th>st_teff</th>
      <th>st_tefferr1</th>
      <th>st_tefferr2</th>
      <th>st_tefflim</th>
      <th>st_mass</th>
      <th>st_masserr1</th>
      <th>st_masserr2</th>
      <th>st_masslim</th>
      <th>st_rad</th>
      <th>st_raderr1</th>
      <th>st_raderr2</th>
      <th>st_radlim</th>
      <th>rowupdate</th>
      <th>pl_masse</th>
      <th>pl_masseerr1</th>
      <th>pl_masseerr2</th>
      <th>pl_masselim</th>
      <th>pl_rade</th>
      <th>pl_radeerr1</th>
      <th>pl_radeerr2</th>
      <th>pl_radelim</th>
      <th>pl_facility</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KIC 10525077</td>
      <td>b</td>
      <td>KIC 10525077 b</td>
      <td>Transit</td>
      <td>0</td>
      <td>1</td>
      <td>854.0830</td>
      <td>0.01628</td>
      <td>-0.01697</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>89.861</td>
      <td>0.096</td>
      <td>-0.098</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.491</td>
      <td>0.080</td>
      <td>-0.071</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>19h09m30.74s</td>
      <td>287.378077</td>
      <td>+47d46m16.3s</td>
      <td>47.771187</td>
      <td>1525.31</td>
      <td>58.45</td>
      <td>-58.45</td>
      <td>0</td>
      <td>1525.31</td>
      <td>58.45</td>
      <td>-58.45</td>
      <td>0.0</td>
      <td>15.355</td>
      <td>NaN</td>
      <td>0</td>
      <td>Kepler-band</td>
      <td>15.304</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6091.0</td>
      <td>164.0</td>
      <td>-213.0</td>
      <td>0.0</td>
      <td>1.01</td>
      <td>0.12</td>
      <td>-0.12</td>
      <td>0.0</td>
      <td>1.01</td>
      <td>0.10</td>
      <td>-0.10</td>
      <td>0</td>
      <td>2016-01-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.5</td>
      <td>0.9</td>
      <td>-0.8</td>
      <td>0</td>
      <td>Kepler</td>
    </tr>
    <tr>
      <th>1</th>
      <td>KIC 3558849</td>
      <td>b</td>
      <td>KIC 3558849 b</td>
      <td>Transit</td>
      <td>0</td>
      <td>1</td>
      <td>1322.3000</td>
      <td>386.10000</td>
      <td>-11.20000</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>89.973</td>
      <td>0.023</td>
      <td>-0.031</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.616</td>
      <td>0.089</td>
      <td>-0.080</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>19h39m47.96s</td>
      <td>294.949847</td>
      <td>+38d36m18.7s</td>
      <td>38.605197</td>
      <td>1258.75</td>
      <td>35.41</td>
      <td>-35.41</td>
      <td>0</td>
      <td>1258.75</td>
      <td>35.41</td>
      <td>-35.41</td>
      <td>0.0</td>
      <td>14.218</td>
      <td>NaN</td>
      <td>0</td>
      <td>Kepler-band</td>
      <td>14.167</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6175.0</td>
      <td>168.0</td>
      <td>-194.0</td>
      <td>0.0</td>
      <td>0.98</td>
      <td>0.11</td>
      <td>-0.11</td>
      <td>0.0</td>
      <td>1.01</td>
      <td>0.11</td>
      <td>-0.11</td>
      <td>0</td>
      <td>2016-01-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.9</td>
      <td>1.0</td>
      <td>-0.9</td>
      <td>0</td>
      <td>Kepler</td>
    </tr>
    <tr>
      <th>2</th>
      <td>KIC 5437945</td>
      <td>b</td>
      <td>KIC 5437945 b</td>
      <td>Transit</td>
      <td>0</td>
      <td>2</td>
      <td>440.7813</td>
      <td>0.00563</td>
      <td>-0.00577</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>89.904</td>
      <td>0.066</td>
      <td>-0.086</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.571</td>
      <td>0.143</td>
      <td>-0.143</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>19h13m53.97s</td>
      <td>288.474857</td>
      <td>+40d39m04.9s</td>
      <td>40.651357</td>
      <td>1322.48</td>
      <td>29.86</td>
      <td>-29.86</td>
      <td>0</td>
      <td>1322.48</td>
      <td>29.86</td>
      <td>-29.86</td>
      <td>0.0</td>
      <td>13.771</td>
      <td>NaN</td>
      <td>0</td>
      <td>Kepler-band</td>
      <td>13.751</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6340.0</td>
      <td>176.0</td>
      <td>-199.0</td>
      <td>0.0</td>
      <td>1.07</td>
      <td>0.17</td>
      <td>-0.17</td>
      <td>0.0</td>
      <td>1.24</td>
      <td>0.29</td>
      <td>-0.29</td>
      <td>0</td>
      <td>2016-01-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.4</td>
      <td>1.6</td>
      <td>-1.6</td>
      <td>0</td>
      <td>Kepler</td>
    </tr>
    <tr>
      <th>3</th>
      <td>KIC 5951458</td>
      <td>b</td>
      <td>KIC 5951458 b</td>
      <td>Transit</td>
      <td>0</td>
      <td>1</td>
      <td>1320.1000</td>
      <td>12401.80000</td>
      <td>-152.50000</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>89.799</td>
      <td>0.090</td>
      <td>-0.120</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.589</td>
      <td>2.346</td>
      <td>-0.375</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>19h15m57.98s</td>
      <td>288.991582</td>
      <td>+41d13m22.9s</td>
      <td>41.223038</td>
      <td>758.78</td>
      <td>12.71</td>
      <td>-12.71</td>
      <td>0</td>
      <td>758.78</td>
      <td>12.71</td>
      <td>-12.71</td>
      <td>0.0</td>
      <td>12.713</td>
      <td>NaN</td>
      <td>0</td>
      <td>Kepler-band</td>
      <td>12.688</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6258.0</td>
      <td>170.0</td>
      <td>-183.0</td>
      <td>0.0</td>
      <td>0.98</td>
      <td>0.21</td>
      <td>-0.21</td>
      <td>0.0</td>
      <td>1.52</td>
      <td>0.82</td>
      <td>-0.82</td>
      <td>0</td>
      <td>2016-01-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.6</td>
      <td>26.3</td>
      <td>-4.2</td>
      <td>0</td>
      <td>Kepler</td>
    </tr>
    <tr>
      <th>4</th>
      <td>KOI-7892</td>
      <td>b</td>
      <td>KIC 8540376 b</td>
      <td>Transit</td>
      <td>0</td>
      <td>2</td>
      <td>31.8099</td>
      <td>0.00919</td>
      <td>-0.00933</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>89.300</td>
      <td>0.490</td>
      <td>-0.720</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.366</td>
      <td>0.196</td>
      <td>-0.170</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>18h49m30.61s</td>
      <td>282.377530</td>
      <td>+44d41m40.5s</td>
      <td>44.694592</td>
      <td>1106.37</td>
      <td>23.26</td>
      <td>-23.26</td>
      <td>0</td>
      <td>1106.37</td>
      <td>23.26</td>
      <td>-23.26</td>
      <td>0.0</td>
      <td>14.294</td>
      <td>NaN</td>
      <td>0</td>
      <td>Kepler-band</td>
      <td>14.217</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6474.0</td>
      <td>178.0</td>
      <td>-267.0</td>
      <td>0.0</td>
      <td>1.04</td>
      <td>0.20</td>
      <td>-0.20</td>
      <td>0.0</td>
      <td>1.26</td>
      <td>0.56</td>
      <td>-0.56</td>
      <td>0</td>
      <td>2016-01-05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.1</td>
      <td>2.2</td>
      <td>-1.9</td>
      <td>0</td>
      <td>Kepler</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Make a histogram of planet's in units of Earth radius
plt.hist(kepler_data['pl_rade']) # Plot the histogram

# Label the axes
plt.xlabel('Radius [R$_\oplus$]', fontsize=14)
plt.ylabel('N', fontsize=14)

# Show the plot
plt.show()
```


    
![png](output_17_0.png)
    


Well this histogram isn't very informative... What about using different **bins** for the histogram.


```python
# Create bins that are uniformly-sized in log space
log_bins = np.logspace(np.log10(0.3), np.log10(20), 50)
plt.hist(kepler_data['pl_rade'], bins = log_bins)

# Log scale on the x-axis
plt.xscale('log')

# Label the axes
plt.xlabel('Radius [R$_\oplus$]', fontsize=14) # More LaTex. \oplus gives us the symbol for Earth
plt.ylabel('N', fontsize=14)

# Show the plot
plt.show()
```


    
![png](output_19_0.png)
    


It looks like the distribution of planets has some sort of **bimodality** between about 1 and 2 Earth masses. Let's zoom in on that area by **restricting our x-axis limits**.


```python
plt.hist(kepler_data['pl_rade'], bins = log_bins) # Using the same bins as above

# Let's zoom in on the distribution of planet radii in this region
plt.xlim([1., 5]) # Units of Earth radius

# Same plot housekeeping as above
# -------------------------------
# Log scale on the x-axis
plt.xscale('log')

# Label the axes
plt.xlabel('Radius [R$_\oplus$]', fontsize=14) # More LaTex. \oplus gives us the symbol for Earth
plt.ylabel('N', fontsize=14)

# Show the plot
plt.show()
```


    
![png](output_21_0.png)
    


Are there **any notable features** in this distribution of planets by radius?

# 3. Orbital period vs Radius for transiting *Kepler* planets

Now it's your turn! Make a plot of the transiting planets discovered by *Kepler* with orbital period on the x-axis and planet radius (in units of Earth radius) on the y-axis. 

Be sure to label your axes with units! Refer to the header in the .csv file for more information. 


```python

```

# 4. Make your own plot of whatever two parameters you want!

Now make a plot visualizing the relationship (or lack thereof) of any two parameters that you want. Go back to the [Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/) to input constraints (if any) in the Confirmed Table that are relevant to your parameters and download your new data. Spend some time looking at all of the columns available in the Exoplanet Archive data table to see what interests you. (We didn't even get a chance to make use of any of the stellar parameters!)

Make sure you include axis labels and use appropriate axis scales so that you can see features of the data across a wide range of values. Why did you plot what you plotted? Do you notice any interesting features?


```python

```
