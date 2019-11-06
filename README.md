# rnoaa-presentation   

### What is rnoaa?  
  
rnoaa is a client for multiple NOAA data sources, such as NCDC, sea ice, storms, buoys, and forecast data.  
I will be exhibiting the use of rnoaa to collect National Climatic Data Center (NCDC) data, as this is relevant to my project and also a widely-used data source.  
  
### Getting started with rnoaa   
  
```
install.packages('rnoaa')  
library(rnoaa)  
```  

Note that there are a LOT of packages here. R should be kept up to date, as any issues with dependencies may impede your work.   
  
All ncdc functions interact with the NCDC application programming interface (API)  
You will need an API key for all ncdc functions, and you can get it at this link: https://www.ncdc.noaa.gov/cdo-web/webservices/v2  
  
You will receive an email with your API key. This API key must be added to a .Rprofile file in order to access the data.  

The necessary steps here are:  
  
1. Request the API key  
2. Create an .Rprofile in your working directory for the project  
3. in the .Rprofile, enter options(noaakey="your_noaa_token")  
4. Restart .Rstudio in the directory that contains the .Rprofile  
  
I have included a .Rprofile with my NOAA token in this repository, so opening this repo as a project in Rstudio will automatically allow for NCDC data collection.  
  
### Collecting data using ncdc()  
  
ncdc() is the main argument. Other functions help us find data or provide metadata.   
These functions include:  
 - ncdc_datasets()  
 - ncdc_datatypes()  
 - ncdc_locs()  
 - ncdc_locs_cats()  
 - ncdc_stations()  
  
General rules of thumb:  
1. Must use quotes in ncdc() requests  
2. Date range must be less than one year  
3. Default output for all ncdc functions is 25. Have to specify if you want more; it will take longer. The alternative is you can piece it down by specifying more parameters. Max output is 1000.  
  
Let's start by finding out more about ncdc datasets.  
  
```
ncdc_datasets()
```  

```
$data
                    uid    mindate    maxdate                        name datacoverage         id
1  gov.noaa.ncdc:C00861 1763-01-01 2019-11-03             Daily Summaries         1.00      GHCND
2  gov.noaa.ncdc:C00946 1763-01-01 2019-09-01 Global Summary of the Month         1.00       GSOM
3  gov.noaa.ncdc:C00947 1763-01-01 2019-01-01  Global Summary of the Year         1.00       GSOY
4  gov.noaa.ncdc:C00345 1991-06-05 2019-11-03    Weather Radar (Level II)         0.95    NEXRAD2
5  gov.noaa.ncdc:C00708 1994-05-20 2019-11-02   Weather Radar (Level III)         0.95    NEXRAD3
6  gov.noaa.ncdc:C00821 2010-01-01 2010-01-01     Normals Annual/Seasonal         1.00 NORMAL_ANN
7  gov.noaa.ncdc:C00823 2010-01-01 2010-12-31               Normals Daily         1.00 NORMAL_DLY
8  gov.noaa.ncdc:C00824 2010-01-01 2010-12-31              Normals Hourly         1.00 NORMAL_HLY
9  gov.noaa.ncdc:C00822 2010-01-01 2010-12-01             Normals Monthly         1.00 NORMAL_MLY
10 gov.noaa.ncdc:C00505 1970-05-12 2014-01-01     Precipitation 15 Minute         0.25  PRECIP_15
11 gov.noaa.ncdc:C00313 1900-01-01 2014-01-01        Precipitation Hourly         1.00 PRECIP_HLY
```
  
I want stations in Australia. I will start by searching for the ncdc stations in Australia.  
  
```
ncdc_stations(locationid = 'FIPS:AS', limit = 500)
```

```
$data
    elevation    mindate    maxdate latitude                               name datacoverage                id elevationUnit longitude
1       295.0 1975-10-03 2007-10-08 -18.3000         GEORGETOWN POST OFFICE, AS       0.9347 GHCND:ASM00094275        METERS  143.5500
2         9.0 1948-01-01 2019-11-02 -16.2880                  WILLIS ISLAND, AS       0.7740 GHCND:ASM00094299        METERS  149.9650
3        85.0 1973-01-01 2004-06-29 -35.0830                     JERVIS BAY, AS       0.6371 GHCND:ASM00094940        METERS  150.8000
4         7.0 1947-12-31 2019-11-02 -31.5420          LORD HOWE ISLAND AERO, AS       0.7725 GHCND:ASM00094995        METERS  159.0790
...
108       7.4 1939-08-01 2019-11-02 -17.9475                 BROOME AIRPORT, AS       1.0000 GHCND:ASN00003003        METERS  122.2353
109      25.0 1917-08-01 2018-10-19 -16.3961                   CAPE LEVEQUE, AS       0.8332 GHCND:ASN00003004        METERS  122.9278
110     120.0 1907-05-01 1996-12-27 -17.5731              FAIRFIELD STATION, AS       0.4873 GHCND:ASN00003005        METERS  125.0650
111     114.0 1893-03-01 2000-01-15 -18.1919         FITZROY CROSSING COMP., AS       0.9016 GHCND:ASN00003006        METERS  125.5644
 [ reached getOption("max.print") -- omitted 389 rows ]
 ```
   
Okay, there's a lot more stations than I thought. As it turns out, it's very difficult to find a specific station using this method with all of the data available. Let's try adding more parameters to decrease this list.  
   
 ```
 ncdc_stations(datasetid = 'GSOM', locationid = 'FIPS:AS', startdate = '1991-01-01', enddate= '1991-03-01', extent = c(-41,140,-40,145), limit = 500)
```

```
    elevation    mindate    maxdate latitude                      name datacoverage                id elevationUnit longitude
1       15.0 1888-04-01 2017-12-01 -40.6842   CAPE GRIM WOOLNORTH, AS       0.8773 GHCND:ASN00091011        METERS  144.7181
2       77.0 1952-01-01 2018-06-01 -40.9272    REDPA GREENES ROAD, AS       0.9499 GHCND:ASN00091082        METERS  144.7489
3        8.0 1941-01-01 2019-08-01 -40.4472  THREE HUMMOCK ISLAND, AS       0.3178 GHCND:ASN00091100        METERS  144.8400
4       30.0 1961-11-01 2001-04-01 -40.5278     HUNTER ISLAND NO2, AS       0.1139 GHCND:ASN00091124        METERS  144.7469
5       28.0 1961-09-01 2016-07-01 -40.7769  MONTAGU MONTAGU ROAD, AS       0.9894 GHCND:ASN00091128        METERS  144.9586
6       26.0 1966-01-01 2008-05-01 -40.9147   TOGARI RENISON ROAD, AS       0.9843 GHCND:ASN00091208        METERS  144.8681
7       60.0 1973-01-01 1991-07-01 -40.9650  CHRISTMAS HILLS NO 2, AS       0.9554 GHCND:ASN00091217        METERS  144.9475
8      107.3 1971-01-01 2019-09-01 -40.9089              MARRAWAH, AS       0.9932 GHCND:ASN00091223        METERS  144.7094
9       94.0 1985-09-01 2019-09-01 -40.6828        CAPE GRIM BAPS, AS       0.9610 GHCND:ASN00091245        METERS  144.6900
10      18.0 1936-02-01 2019-09-01 -40.0092 CITY OF MELBOURNE BAY, AS       0.9921 GHCND:ASN00098000        METERS  144.1111
11     111.0 1917-07-01 2005-01-01 -40.0511                GRASSY, AS       0.1674 GHCND:ASN00098008        METERS  144.0581
12        NA 1910-01-01 2019-08-01 -40.1167          SURPRISE BAY, AS       0.2660 GHCND:ASN00098012          <NA>  143.9167
```

Success!   
The key here was adding the extent - that narrowed the list more than most other factors, and can be useful for finding specific stations.  
The alternative to this process is to use NOAA's find-a-station web tool: https://www.ncdc.noaa.gov/cdo-web/datatools/findstation  
  
Now that we have stations for the area that we want to look at, let's pick Cape Grim BAPS and look at some data.  
  
```
ncdc(datasetid='GSOM', stationid = 'GHCND:ASN00091245', startdate = '1991-01-01', enddate = '1991-12-01')  
```

```
$data
# A tibble: 25 x 10
   date                datatype station           value fl_S  fl_a  fl_cc fl_d  fl_M  fl_Q 
   <chr>               <chr>    <chr>             <dbl> <chr> <chr> <chr> <chr> <chr> <chr>
 1 1991-01-01T00:00:00 CDSD     GHCND:ASN00091245   0.7 a     NA    NA    NA    NA    NA   
 2 1991-01-01T00:00:00 CLDD     GHCND:ASN00091245   0.7 a     5     NA    NA    NA    NA   
 3 1991-01-01T00:00:00 DP01     GHCND:ASN00091245   9   a     ""    NA    NA    NA    NA   
 4 1991-01-01T00:00:00 DP10     GHCND:ASN00091245   4   a     ""    NA    NA    NA    NA   
 5 1991-01-01T00:00:00 DP1X     GHCND:ASN00091245   1   a     ""    NA    NA    NA    NA   
 6 1991-01-01T00:00:00 DT00     GHCND:ASN00091245   0   a     5     NA    NA    NA    NA   
 7 1991-01-01T00:00:00 DT32     GHCND:ASN00091245   0   a     5     NA    NA    NA    NA   
 8 1991-01-01T00:00:00 DX32     GHCND:ASN00091245   0   a     5     NA    NA    NA    NA   
 9 1991-01-01T00:00:00 DX70     GHCND:ASN00091245   2   a     5     NA    NA    NA    NA   
10 1991-01-01T00:00:00 DX90     GHCND:ASN00091245   0   a     5     NA    NA    NA    NA   
# ... with 15 more rows
```
  
Woah! what does all of this mean?  
  
This is a strangely formatted set of data - each observation has its own row. We could go through and collect just the datatype we want, but it's easier to specify "datatypeid" in the request. In addition, we will also ask for units, just to be sure we know what we're looking at.   
  
```
ncdc(datasetid='GSOM', stationid = 'GHCND:ASN00091245', datatypeid='TAVG', startdate = '1991-01-01', enddate = '1991-12-01', limit = 100, add_units=TRUE)
```

```
 date                datatype station           value fl_a  fl_S  units  
   <chr>               <chr>    <chr>             <dbl> <chr> <chr> <chr>  
 1 1991-01-01T00:00:00 TAVG     GHCND:ASN00091245 15.9  5     a     celsius
 2 1991-02-01T00:00:00 TAVG     GHCND:ASN00091245 15.6  4     a     celsius
 3 1991-03-01T00:00:00 TAVG     GHCND:ASN00091245 14.8  2     a     celsius
 4 1991-04-01T00:00:00 TAVG     GHCND:ASN00091245 13.3  ""    a     celsius
 5 1991-05-01T00:00:00 TAVG     GHCND:ASN00091245 12.4  3     a     celsius
 6 1991-06-01T00:00:00 TAVG     GHCND:ASN00091245 12.1  5     a     celsius
 7 1991-08-01T00:00:00 TAVG     GHCND:ASN00091245  9.61 ""    a     celsius
 8 1991-09-01T00:00:00 TAVG     GHCND:ASN00091245 10.7  1     a     celsius
 9 1991-10-01T00:00:00 TAVG     GHCND:ASN00091245 11.7  2     a     celsius
10 1991-11-01T00:00:00 TAVG     GHCND:ASN00091245 12.4  2     a     celsius
11 1991-12-01T00:00:00 TAVG     GHCND:ASN00091245 13.7  ""    a     celsius
```

Cool! Now, we will save this to a variable and then plot it, using ncdc_plot()  
 
 ```
capeGrimTemp <- ncdc(datasetid='GSOM', stationid = 'GHCND:ASN00091245', datatypeid='TAVG', startdate = '1991-01-01', enddate = '1991-12-01', limit = 100, add_units=TRUE)
ncdc_plot(capeGrimTemp)
```
   
You can even request multiple datatypes by using a vector or list. Plotting doesn't work well on this, though, and observations aren't grouped together, so data would need to be split if collected this way.   

```
ncdc(datasetid='GSOM', stationid = 'GHCND:ASN00091245', datatypeid=c('TAVG','PRCP'), startdate = '1991-01-01', enddate = '1991-12-01', limit = 500, add_units=TRUE)
```
