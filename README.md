# WI.17 Pressure Data
<details>
<summary>T-factor calculator from CPP:</summary>

Test

*T-factor calculator from CPP is provided by FMZ, due to difficulties in AIM model, this data was not possible to be used. Instead, Low Pressure Events data was provided.*
- Source:
- Last Update: 07/10/2021
- Contact: Richard Tull
- Output File:
    -  [T-factor_calculator_from_CPP_pressures_AR22_q1.0.xlsm](https://thameswater.sharepoint.com/:u:/r/sites/StratPlannInvest/sysassstrat/RiskMod/Shared%20Documents/AIM/Model%20Update%20PR24/Water%20Inf/WI.17%20Pressure/T-factor_calculator_from_CPP_pressures_AR22_q1.0.zip?csf=1&web=1&e=MqLM1u)
</details>
<hr>

<details>
<summary>Low Pressure Events:</summary>

*This data was provided by the Water Interruptions sharepoint website, and then recalculated using python.*
- Source: Water Interruptions Team
  - Source file location:
    - [Current Report](https://thameswater.sharepoint.com/sites/waterinterruptions/Current%20Reports/Forms/AllItems.aspx)
    - [Previous Years](https://thameswater.sharepoint.com/sites/waterinterruptions/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fsites%2Fwaterinterruptions%2FShared%20Documents%2FArchives%2FLow%20pressure%20reports&FolderCTID=0x01200040B94D9386CBEA4EA1E922105C169703)
- Last Update: 09/03/2022
- Contact: Victoria Geddes <victoria.geddes@thameswater.co.uk>
- Output File:
    -  [Low Pressure Events](https://thameswater.sharepoint.com/:f:/r/sites/StratPlannInvest/sysassstrat/RiskMod/Shared%20Documents/AIM/Model%20Update%20PR24/Water%20Inf/WI.17%20Pressure/Low%20Pressure%20Events?csf=1&web=1&e=HzTkFQ)

### How to:

*Download data yearly and recalculate to add coordinates X and Y.*

### 1. Get CPP GIS Data:

- Source:
  - CleanWaterNetwork.gdb -> CW_wPressureFitting

```python
import pandas as pd
import numpy as np
import warnings
warnings.filterwarnings('ignore')
import glob, os
import plotly.express as px
```


```python
folder = r'C:\Temp\Santiago Luquez\AIM\Data Requirements\Low Pressure\New Data\\'
originalData = folder + 'original data\\'
```


```python
# GIS DATA
# TODO: Filter data by CPP on type field
# CPP = Critital Pressure Point
gisFile = pd.read_csv(folder + 'Calculations\\' + 'CW_wPressureFitting.csv')
gis = gisFile[['GISID', 'TYPE', 'REFERENCE', 'SHAPEX', 'SHAPEY']]
#Filter Type
gis2 = gis[gis['TYPE'] =='CPP']

# Get rid of characters CPP
gis2['Logger'] = gis2['REFERENCE'].str.extract('(\d+)', expand=False)
gis3 = gis2.dropna(subset=['Logger']).sort_values(['Logger'])

# Change Logger type to match other file
# gis = gis.dropna(subset=['Logger'])
gis3['Logger'] = gis3['Logger'].astype(int)
gis4 = gis3.sort_values(['Logger'])
gis4['Logger'] = gis4['Logger'].astype(str)
gis4

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
      <th>GISID</th>
      <th>TYPE</th>
      <th>REFERENCE</th>
      <th>SHAPEX</th>
      <th>SHAPEY</th>
      <th>Logger</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>128</th>
      <td>1554393</td>
      <td>CPP</td>
      <td>CPP0011</td>
      <td>465032.200000</td>
      <td>166468.100000</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2180</th>
      <td>1554395</td>
      <td>CPP</td>
      <td>CPP0012</td>
      <td>466686.017600</td>
      <td>173658.430300</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1843</th>
      <td>1554984</td>
      <td>CPP</td>
      <td>CPP0013</td>
      <td>470806.800000</td>
      <td>173167.400000</td>
      <td>13</td>
    </tr>
    <tr>
      <th>579</th>
      <td>1554994</td>
      <td>CPP</td>
      <td>CPP0014</td>
      <td>471387.800000</td>
      <td>180603.100000</td>
      <td>14</td>
    </tr>
    <tr>
      <th>472</th>
      <td>1554981</td>
      <td>CPP</td>
      <td>CPP0015</td>
      <td>471668.900000</td>
      <td>169998.000000</td>
      <td>15</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1252</th>
      <td>8509046</td>
      <td>CPP</td>
      <td>25421</td>
      <td>526022.485694</td>
      <td>179675.452382</td>
      <td>25421</td>
    </tr>
    <tr>
      <th>336</th>
      <td>8692204</td>
      <td>CPP</td>
      <td>25741</td>
      <td>531167.879119</td>
      <td>166954.290483</td>
      <td>25741</td>
    </tr>
    <tr>
      <th>339</th>
      <td>8692183</td>
      <td>CPP</td>
      <td>25795</td>
      <td>531248.013450</td>
      <td>166982.044079</td>
      <td>25795</td>
    </tr>
    <tr>
      <th>805</th>
      <td>8692218</td>
      <td>CPP</td>
      <td>27357</td>
      <td>531090.597489</td>
      <td>166970.547121</td>
      <td>27357</td>
    </tr>
    <tr>
      <th>692</th>
      <td>8510755</td>
      <td>CPP</td>
      <td>30643</td>
      <td>526178.829251</td>
      <td>179417.400000</td>
      <td>30643</td>
    </tr>
  </tbody>
</table>
<p>699 rows × 6 columns</p>
</div>




### 2. Download yearly data from source:
* Download data and run loop to get file location and names.*

- Source file location:
  - [Current Report](https://thameswater.sharepoint.com/sites/waterinterruptions/Current%20Reports/Forms/AllItems.aspx)
  - [Previous Years](https://thameswater.sharepoint.com/sites/waterinterruptions/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fsites%2Fwaterinterruptions%2FShared%20Documents%2FArchives%2FLow%20pressure%20reports&FolderCTID=0x01200040B94D9386CBEA4EA1E922105C169703)

```python
os.chdir(originalData)
originalFiles =[]
for file in glob.glob("*.xlsm"):
    originalFiles.append(file)
for file in glob.glob("*.xlsx"):
    originalFiles.append(file)
originalFiles1 = [word.replace('.xlsm','') for word in originalFiles]
originalFiles2 = [word.replace('.xlsx','') for word in originalFiles1]
print(originalFiles2)
```

    ['DG2 Weekly Report 2016-17', 'DG2 Weekly Report 2017-18', 'Low Pressure AR15 OLD', 'Low Pressure Weekly Report 2018-19', 'Low Pressure Weekly Report 2019-20', 'Low Pressure Weekly Report 2020-21', 'Low Pressure Weekly Report 2021-22']
    
### 3. Run new loop:
* Run new loop to merge GIS Data with Low Pressure Data.*


```python
for i in originalFiles2:    
    ext = '.xlsm'
    ext2 = '.xlsx'

    # Try multiple formats
    try:
        pressureFile = pd.read_excel(originalData + i + ext, sheet_name = 'DG2 Download', header = 2) 
    except:
        try:
            pressureFile = pd.read_excel(originalData + i + ext2, sheet_name = 'DG2 Download', header = 2) 
        except:
            try:
                pressureFile = pd.read_excel(originalData + i + ext, sheet_name = 'CPP Download', header = 2) 
            except:
                pressureFile = pd.read_excel(originalData + i + ext2, sheet_name = 'CPP Download', header = 2) 


    pressureFile2 = pressureFile.dropna(how='all').dropna(subset=['Logger'])
    pressureFile3 = pressureFile2[[
            'Logger',
            'Actual Number of Properties Affected',
            'ASM Area',
            'LPIClosedDate',
            'End Date',
            'End Time',
            'Logger Site Name',
            ' Lowest Pressure For At Least An Hour ',
            'Reason',
            'Start Date',
            'Start Time',
            'Status',
            'Sub Reason',
            'Threshold Pressure  ',
            'Zone'
            ]]
    pressureFile3.sort_values(by = 'Logger')
    pressureFile3['Logger'] = pressureFile3['Logger'].astype(int).astype(str)
    # pressureFile3['Logger'] = pressureFile3['Logger']#.astype(str)
    output = pd.merge(pressureFile3,gis4,on='Logger').rename(columns = {
            'Logger' : 'LOGGER_ID',
            'SHAPEX':'Northing',
            'SHAPEY':'Easting',
            'Actual Number of Properties Affected' : 'ACTUAL_PROPERTIES_AFFECTED',
            'ASM Area' : 'ASM_AREA',
            'LPIClosedDate' : 'CLOSED_DATE',
            'End Date' : 'END_DATE',
            'End Time' : 'END_TIME',
            'Logger Site Name' : 'LOGGER_SITE_NAME',
            ' Lowest Pressure For At Least An Hour ' : 'LOWEST_PRESSURE',
            'Reason' : 'REASON',
            'Start Date' : 'START_DATE',
            'Start Time' : 'START_TIME',
            'Status' : 'STATUS',
            'Sub Reason' : 'SUBREASON',
            'Threshold Pressure  ' : 'THRESHOLD',
            'Zone' : 'WPZ'
            })
    output['LOGGER_ID_2'] = output['LOGGER_ID'].astype(int)
    output2 = output.drop(columns = 'LOGGER_ID').rename(columns = {'LOGGER_ID_2' : 'LOGGER_ID'})
    output3 = output2.sort_values(by = 'LOGGER_ID').reset_index()
    output4 = output3[[
        'LOGGER_ID',
        'LOGGER_SITE_NAME',
        'GISID',
        'Northing',
        'Easting',
        'START_DATE',
        'START_TIME',
        'END_DATE',
        'END_TIME',
        'ASM_AREA',
        'WPZ',
        'REASON',
        'SUBREASON',
        'LOWEST_PRESSURE',
        'THRESHOLD',
        'ACTUAL_PROPERTIES_AFFECTED',
        'STATUS',
        'CLOSED_DATE'        
    ]]
    output4.to_csv(folder + 'output\\' + i + '.csv')
    
    print('File ' + i + ' created.')
    # Compare the data
#     print('Original Data:')
    pressureFile_comparison = pressureFile['Logger'].drop_duplicates()
#     print(pressureFile_comparison.describe())
#     print('''
    
#     ''')
#     print('New Data:')
    output_comparison = output4['LOGGER_ID'].drop_duplicates()
#     print(output_comparison.describe())
    dif = (pressureFile_comparison.count() - output_comparison.count()).astype(str)
    print('DIFFERENCE = ' + dif)
    print('''
    
    -------------------------
    
    ''')
```

    File DG2 Weekly Report 2016-17 created.
    DIFFERENCE = 8
    
        
        -------------------------
        
        
    File DG2 Weekly Report 2017-18 created.
    DIFFERENCE = 5
    
        
        -------------------------
        
        
    File Low Pressure AR15 OLD created.
    DIFFERENCE = 8
    
        
        -------------------------
        
        
    File Low Pressure Weekly Report 2018-19 created.
    DIFFERENCE = 6
    
        
        -------------------------
        
        
    File Low Pressure Weekly Report 2019-20 created.
    DIFFERENCE = 5
    
        
        -------------------------
        
        
    File Low Pressure Weekly Report 2020-21 created.
    DIFFERENCE = 3
    
        
        -------------------------
        
        
    File Low Pressure Weekly Report 2021-22 created.
    DIFFERENCE = 5
    
        
        -------------------------
        











</details>

<hr>
<details>
<summary>Distribution Main average pressure at DMA Level [ Yearly ]:</summary>

- Source: PI DataLink
- Last Update: 13/12/2021
- Contact: Santiago Luquez
- Helpers
  - Lukasz Swiezynski
  - Marcus Purnell
- Output File:
    -  [Yearly_Pressure_DMA_Level.xlsx](https://thameswater.sharepoint.com/:x:/r/sites/StratPlannInvest/sysassstrat/RiskMod/Shared%20Documents/AIM/Model%20Update%20PR24/Water%20Inf/WI.17%20Pressure/Yearly_Pressure_DMA_Level.xlsx?d=w3c5e3d9d7d7c48e098ee5337a2437415&csf=1&web=1&e=116uu1)

<hr>

### How to:

*Download data monthly and recalculate to yearly.*
<hr>


### 1. Download data from PI DataLink [ Microsoft Excel Add-in ]:
- DMA_Pressure_Query.xlsx
  - Update DMA_Metering_Set sheet: paths and data
    - Search query: * DMA * FSET * [ no spaces ]
  - Update Meters sheet: paths and data
    - Search query: * DM * PRESSURE * [ no spaces ]
- DMA_Pressure_Query_SOM.xlsx
  - Update Meters sheet: paths and data
    - Search query: * SOM * PRESSURE * [ no spaces ]
- DMA_Pressure_Query_CPP.xlsx
  - Update CCP sheet: paths and data
    - Search query: * CPP * PRESSURE * [ no spaces ]
- GIS_IDs.xlsx
  - Update PI Paths - GIS ID sheet: paths and data
    - Search query: * PRESSURE * [ no spaces ] [ *Everything with pressure*  ]
<hr>

### 2. Update data from GIS DATA:
*Check Metering.mxd*
- NetworkMeter.csv
  - Network Meters
- PressureFitting.csv
  - CPP: Query
    - Type = 'CPP'
- DMA_List.xlsx
  - DMAs Boundaries from Cleanwaternetwork
<hr>

### 3. Clean data and recalculate yearly (or monthly as needed):
```python
import pandas as pd
from pandas import Series
import numpy as np
import warnings
warnings.filterwarnings('ignore')
folder = 'C:\Temp\Santiago Luquez\AIM\Data Requirements\Pressure\PI_SL\Yearly_2\\'
```

## Heights


```python
### Meter Heights ###
NetworkMeterFile = 'NetworkMeter.csv'
NetworkMeterReadFile = pd.read_csv(folder + NetworkMeterFile, thousands=r',')
NetworkMeterFile = pd.DataFrame(NetworkMeterReadFile)

NetworkMeter_1 = NetworkMeterFile[['CONTROLREF','HEIGHT']].dropna()
NetworkMeter_2 = NetworkMeterFile[['ALTERNATIVEREF','HEIGHT']].dropna()

metersHeight_1 = NetworkMeter_1.rename(columns={"CONTROLREF": "Meters"})
metersHeight_2 = NetworkMeter_2.rename(columns={"ALTERNATIVEREF": "Meters"})

metersHeight_1['Meters'] = metersHeight_1['Meters'].str.replace('DMSOM','SOM')
metersHeight_2['Meters'] = metersHeight_2['Meters'].str.replace('DMSOM','SOM')
```


```python
### Meter Heights SOM  - Meters 1 ###

# Leave only Meter Number #
metersHeightSOM_Nbr = metersHeight_1[metersHeight_1["Meters"].str.contains("DM")==False]

metersHeightSOM_Nbr['Meters_Nbr'] = metersHeightSOM_Nbr['Meters']
metersHeightSOM_Nbr['Meters_Nbr'] = metersHeightSOM_Nbr['Meters_Nbr'].str.replace('OMS','').str.replace('SOM','').str.replace('ZM','').str.replace('_','')
metersHeightSOM_Nbr['Meters_Nbr'] = metersHeightSOM_Nbr['Meters_Nbr'].str.replace('(','').str.replace(')','').str.replace('DI','').str.replace('DI/','')
alfbt = 'L|X|N|G|M|A|W|C|x|/'
metersHeightSOM_1 = metersHeightSOM_Nbr[metersHeightSOM_Nbr["Meters_Nbr"].str.contains(alfbt)==False]

metersHeightSOM_10['Meters_Nbr_Int'] = metersHeightSOM_10['Meters_Nbr'].astype(int)
metersHeightSOM = metersHeightSOM_10.drop(columns = ['Meters','Meters_Nbr']).rename(columns={'Meters_Nbr_Int' : 'Meters'})

metersHeightSOM
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HEIGHT</th>
      <th>Meters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>60.99</td>
      <td>525</td>
    </tr>
    <tr>
      <th>1</th>
      <td>95.37</td>
      <td>656</td>
    </tr>
    <tr>
      <th>2</th>
      <td>145.64</td>
      <td>2112</td>
    </tr>
    <tr>
      <th>3</th>
      <td>145.65</td>
      <td>2113</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3.71</td>
      <td>4473</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4586</th>
      <td>138.91</td>
      <td>40124</td>
    </tr>
    <tr>
      <th>4587</th>
      <td>126.17</td>
      <td>40126</td>
    </tr>
    <tr>
      <th>4588</th>
      <td>18.30</td>
      <td>4068</td>
    </tr>
    <tr>
      <th>4589</th>
      <td>102.72</td>
      <td>540</td>
    </tr>
    <tr>
      <th>4590</th>
      <td>121.44</td>
      <td>14286</td>
    </tr>
  </tbody>
</table>
<p>1204 rows × 2 columns</p>
</div>




```python
### Meter Heights SOM  - Meters 2 ###

# Leave only Meter Alternative Number #
metersHeightSOM_Nbr_2 = metersHeight_2[metersHeight_2["Meters"].str.contains("DM")==False]
metersHeightSOM_Nbr_2['Meters_Nbr_2'] = metersHeightSOM_Nbr_2['Meters']
metersHeightSOM_Nbr_2['Meters_Nbr_2'] = metersHeightSOM_Nbr_2['Meters_Nbr_2'].str.replace('OMS','').str.replace('SOM','').str.replace('ZM','').str.replace('_','')
metersHeightSOM_Nbr_2['Meters_Nbr_2'] = metersHeightSOM_Nbr_2['Meters_Nbr_2'].str.replace('(','').str.replace(')','').str.replace('DI','').str.replace('DI/','')
metersHeightSOM_Nbr_2['Meters_Nbr_2'] = metersHeightSOM_Nbr_2['Meters_Nbr_2'].str.replace('WAL','')

alfbt = 'Q|/|W|N|H|C|d|A|B|E|K|S|G|c|X|k|M'

metersHeightSOM_2_1 = metersHeightSOM_Nbr_2[metersHeightSOM_Nbr_2["Meters_Nbr_2"].str.contains(alfbt)==False]
metersHeightSOM_2_1['Meters_Nbr_Int_2'] = metersHeightSOM_2_1['Meters_Nbr_2'].astype('int64')
metersHeightSOM_ALT = metersHeightSOM_2_1.drop(columns = ['Meters','Meters_Nbr_2']).rename(columns={'Meters_Nbr_Int_2' : 'Meters'})
metersHeightSOM_ALT
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HEIGHT</th>
      <th>Meters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>37.00</td>
      <td>20175</td>
    </tr>
    <tr>
      <th>96</th>
      <td>9.56</td>
      <td>60</td>
    </tr>
    <tr>
      <th>97</th>
      <td>16.00</td>
      <td>170</td>
    </tr>
    <tr>
      <th>99</th>
      <td>10.54</td>
      <td>1401</td>
    </tr>
    <tr>
      <th>101</th>
      <td>10.22</td>
      <td>385</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>11344</th>
      <td>14.45</td>
      <td>77719</td>
    </tr>
    <tr>
      <th>11345</th>
      <td>15.18</td>
      <td>109616</td>
    </tr>
    <tr>
      <th>11396</th>
      <td>34.35</td>
      <td>165426</td>
    </tr>
    <tr>
      <th>11403</th>
      <td>12.74</td>
      <td>104369</td>
    </tr>
    <tr>
      <th>11404</th>
      <td>13.51</td>
      <td>165422</td>
    </tr>
  </tbody>
</table>
<p>545 rows × 2 columns</p>
</div>



## Metering Set from PI


```python
### DMA Metering Set ###
meteringSetFile = pd.read_excel(folder + 'DMA_Pressure_Query.xlsx', sheet_name='Metering_Set') 
```


```python
meteringSet = meteringSetFile[['DMA_Set', 'Set']]
meteringSet['DMA_Set'] = meteringSet['DMA_Set'].str.replace('DMA~','').str.replace('~FSET','')
meteringSet['Set'] = meteringSet['Set'].str.replace(' ', 'hola')
meteringSet['Set'] = meteringSet['Set'].reset_index().replace('hola', np.nan, regex=True).set_index("index", drop=True).rename_axis(None, axis=0)
meteringSetClean = meteringSet.dropna()
# Ungroup Column "Set" #
meteringSetClean['Set'] = meteringSetClean['Set'].str.replace('+', ',').str.replace('-', ',').str.replace('–', ',')
s = meteringSetClean['Set'].str.split(',').apply(Series, 1).stack()
s.index = s.index.droplevel(-1)
s.name = 'Meter'
#meteringSetFile
```


```python
metering = meteringSetClean.join(s).reset_index()
DMA_metering = metering[['DMA_Set', 'Meter']]
DMA_metering['Meters'] = DMA_metering['Meter'].str.replace(r'^s*$','hola').replace('hola', np.nan, regex=True)
meteringDMA = DMA_metering.dropna().drop(columns='Meter')
meteringDMA
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Meters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>FINSLCA1</td>
      <td>DM09180</td>
    </tr>
    <tr>
      <th>2</th>
      <td>FINSLCA1</td>
      <td>DM09182</td>
    </tr>
    <tr>
      <th>4</th>
      <td>FINSLCA2</td>
      <td>DM09123</td>
    </tr>
    <tr>
      <th>5</th>
      <td>FINSLCA2</td>
      <td>DM09034</td>
    </tr>
    <tr>
      <th>7</th>
      <td>FINSLCA3</td>
      <td>DM09185</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7595</th>
      <td>ZWWICK07</td>
      <td>DM06414</td>
    </tr>
    <tr>
      <th>7597</th>
      <td>ZYEWTR01</td>
      <td>OMS01181</td>
    </tr>
    <tr>
      <th>7598</th>
      <td>ZYEWTR01</td>
      <td>OMS01279</td>
    </tr>
    <tr>
      <th>7599</th>
      <td>ZYEWTR01</td>
      <td>OMS10864</td>
    </tr>
    <tr>
      <th>7600</th>
      <td>ZYEWTR01</td>
      <td>OMS10862</td>
    </tr>
  </tbody>
</table>
<p>5495 rows × 2 columns</p>
</div>




```python
## Metering SOM ##
# Leave only Meter Number #
meteringDMA_SOM_0 = meteringDMA[meteringDMA["Meters"].str.contains("DM")==False]
meteringDMA_SOM_1 = meteringDMA_SOM_0[meteringDMA_SOM_0["Meters"].str.contains("L")==False]
meteringDMA_SOM = meteringDMA_SOM_1[meteringDMA_SOM_1["Meters"].str.contains("Shutdown")==False]
meteringDMA_SOM['Meters_Nbr'] = meteringDMA_SOM['Meters']
meteringDMA_SOM['Meters_Nbr'] = meteringDMA_SOM['Meters_Nbr'].str.replace('OMS','').str.replace('SOM','').str.replace('ZM','').str.replace('_','')
meteringDMA_SOM['Meters_Nbr'] = meteringDMA_SOM['Meters_Nbr'].str.replace('(','').str.replace(')','').str.replace('DI','')
meteringDMA_SOM['Meters_Nbr_Int'] = meteringDMA_SOM['Meters_Nbr'].astype(int)
meteringDMA_SOM_2 = meteringDMA_SOM.drop(columns=['Meters', 'Meters_Nbr']).rename(columns = {'Meters_Nbr_Int' : 'Meters'})
meteringDMASOM = meteringDMA_SOM_2
meteringDMASOM
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Meters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>ZABINB01</td>
      <td>503</td>
    </tr>
    <tr>
      <th>33</th>
      <td>ZABIND09</td>
      <td>974</td>
    </tr>
    <tr>
      <th>34</th>
      <td>ZABIND09</td>
      <td>12942</td>
    </tr>
    <tr>
      <th>38</th>
      <td>ZADDIN01</td>
      <td>16</td>
    </tr>
    <tr>
      <th>39</th>
      <td>ZADDIN01</td>
      <td>199</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7571</th>
      <td>ZWOODL01</td>
      <td>2188</td>
    </tr>
    <tr>
      <th>7597</th>
      <td>ZYEWTR01</td>
      <td>1181</td>
    </tr>
    <tr>
      <th>7598</th>
      <td>ZYEWTR01</td>
      <td>1279</td>
    </tr>
    <tr>
      <th>7599</th>
      <td>ZYEWTR01</td>
      <td>10864</td>
    </tr>
    <tr>
      <th>7600</th>
      <td>ZYEWTR01</td>
      <td>10862</td>
    </tr>
  </tbody>
</table>
<p>460 rows × 2 columns</p>
</div>



## Pressure Calculation


```python
### Pressure by Meter SOM ###
pressureFileSOM = pd.read_excel(folder + 'DMA_Pressure_Query_SOM.xlsx',
              sheet_name='Pressure') 
pressureDataSOM = pressureFileSOM.drop(columns = ['Path'])
pressureDataSOM.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meter</th>
      <th>2019-04-01 00:00:00</th>
      <th>2019-05-01 00:00:00</th>
      <th>2019-06-01 00:00:00</th>
      <th>2019-07-01 00:00:00</th>
      <th>2019-08-01 00:00:00</th>
      <th>2019-09-01 00:00:00</th>
      <th>2019-10-01 00:00:00</th>
      <th>2019-11-01 00:00:00</th>
      <th>2019-12-01 00:00:00</th>
      <th>...</th>
      <th>2021-06-01 00:00:00</th>
      <th>2021-07-01 00:00:00</th>
      <th>2021-08-01 00:00:00</th>
      <th>2021-09-01 00:00:00</th>
      <th>2021-10-01 00:00:00</th>
      <th>2021-11-01 00:00:00</th>
      <th>2021-12-01 00:00:00</th>
      <th>2022-01-01 00:00:00</th>
      <th>2022-02-01 00:00:00</th>
      <th>2022-03-01 00:00:00</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SOM_1_PRESSURE_R</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>...</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SOM_1005_PRESSURE_R</td>
      <td>46.134369</td>
      <td>46.532236</td>
      <td>45.70778</td>
      <td>45.94511</td>
      <td>46.582569</td>
      <td>46.878874</td>
      <td>46.577005</td>
      <td>46.420828</td>
      <td>46.148527</td>
      <td>...</td>
      <td>46.153019</td>
      <td>46.30881</td>
      <td>46.390326</td>
      <td>46.128987</td>
      <td>45.785641</td>
      <td>45.575423</td>
      <td>45.372127</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>2</th>
      <td>SOM_10201_PRESSURE_R</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>...</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>3</th>
      <td>SOM_10203_PRESSURE_R</td>
      <td>133.766564</td>
      <td>134.894132</td>
      <td>134.790159</td>
      <td>134.47429</td>
      <td>134.563036</td>
      <td>133.771248</td>
      <td>134.502817</td>
      <td>134.558622</td>
      <td>134.157751</td>
      <td>...</td>
      <td>136.230043</td>
      <td>136.201101</td>
      <td>136.38394</td>
      <td>136.2886</td>
      <td>136.415077</td>
      <td>136.787926</td>
      <td>136.803814</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>4</th>
      <td>SOM_10204_PRESSURE_R</td>
      <td>78.499735</td>
      <td>78.667799</td>
      <td>78.461599</td>
      <td>78.473796</td>
      <td>78.320949</td>
      <td>77.5777</td>
      <td>78.153884</td>
      <td>74.295769</td>
      <td>78.172815</td>
      <td>...</td>
      <td>80.19165</td>
      <td>80.150702</td>
      <td>80.027602</td>
      <td>80.154876</td>
      <td>79.160214</td>
      <td>80.009216</td>
      <td>80.024651</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 37 columns</p>
</div>




```python
pressureDataSOM['Meter'] = pressureDataSOM['Meter'].str.replace('SOM_','').str.replace('_PRESSURE_R','').str.replace('M','').str.replace('O','').str.replace('_Pressure_R','')
pressureDataSOM['Meters_Nbr_Int'] = pressureDataSOM['Meter'].astype(int)
pressureDataSOM_2 = pressureDataSOM.drop(columns = ['Meter'])
pressureDataSOM_3 = pressureDataSOM_2.rename(columns = {'Meters_Nbr_Int' : 'Meter'})
textData = ['[-11057] Not Enough Values For Calculation', 'Cannot do summary calculation on non-numeric data.','[-11059] No Good Data For Calculation','6.42120838954914E+29']
pressureDataSOM_3.replace(to_replace = textData, value = -1000, inplace = True)
meterPressureDataSOM = pressureDataSOM_3.set_index('Meter')
meterPressureDataSOM[meterPressureDataSOM < 0] = np.nan
meterPressureDataSOM[meterPressureDataSOM > 250] = np.nan
meterPressureDataSOM
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2019-04-01</th>
      <th>2019-05-01</th>
      <th>2019-06-01</th>
      <th>2019-07-01</th>
      <th>2019-08-01</th>
      <th>2019-09-01</th>
      <th>2019-10-01</th>
      <th>2019-11-01</th>
      <th>2019-12-01</th>
      <th>2020-01-01</th>
      <th>...</th>
      <th>2021-06-01</th>
      <th>2021-07-01</th>
      <th>2021-08-01</th>
      <th>2021-09-01</th>
      <th>2021-10-01</th>
      <th>2021-11-01</th>
      <th>2021-12-01</th>
      <th>2022-01-01</th>
      <th>2022-02-01</th>
      <th>2022-03-01</th>
    </tr>
    <tr>
      <th>Meter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
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
      <td>...</td>
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
    </tr>
    <tr>
      <th>1005</th>
      <td>46.134369</td>
      <td>46.532236</td>
      <td>45.707780</td>
      <td>45.945110</td>
      <td>46.582569</td>
      <td>46.878874</td>
      <td>46.577005</td>
      <td>46.420828</td>
      <td>46.148527</td>
      <td>46.166015</td>
      <td>...</td>
      <td>46.153019</td>
      <td>46.308810</td>
      <td>46.390326</td>
      <td>46.128987</td>
      <td>45.785641</td>
      <td>45.575423</td>
      <td>45.372127</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10201</th>
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
      <td>...</td>
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
    </tr>
    <tr>
      <th>10203</th>
      <td>133.766564</td>
      <td>134.894132</td>
      <td>134.790159</td>
      <td>134.474290</td>
      <td>134.563036</td>
      <td>133.771248</td>
      <td>134.502817</td>
      <td>134.558622</td>
      <td>134.157751</td>
      <td>134.023114</td>
      <td>...</td>
      <td>136.230043</td>
      <td>136.201101</td>
      <td>136.383940</td>
      <td>136.288600</td>
      <td>136.415077</td>
      <td>136.787926</td>
      <td>136.803814</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10204</th>
      <td>78.499735</td>
      <td>78.667799</td>
      <td>78.461599</td>
      <td>78.473796</td>
      <td>78.320949</td>
      <td>77.577700</td>
      <td>78.153884</td>
      <td>74.295769</td>
      <td>78.172815</td>
      <td>78.373407</td>
      <td>...</td>
      <td>80.191650</td>
      <td>80.150702</td>
      <td>80.027602</td>
      <td>80.154876</td>
      <td>79.160214</td>
      <td>80.009216</td>
      <td>80.024651</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>76670</th>
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
      <td>...</td>
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
    </tr>
    <tr>
      <th>77258</th>
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
      <td>...</td>
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
    </tr>
    <tr>
      <th>9686</th>
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
      <td>...</td>
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
    </tr>
    <tr>
      <th>10519</th>
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
      <td>...</td>
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
    </tr>
    <tr>
      <th>5185</th>
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
      <td>...</td>
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
    </tr>
  </tbody>
</table>
<p>977 rows × 36 columns</p>
</div>




```python
## Melt Data SOM ##
meterPressureDataSOM['Meters'] = meterPressureDataSOM.index
meltedPressureSOM = meterPressureDataSOM.melt(id_vars=["Meters"], var_name="Date", value_name="Pressure")
pressureSOM = meltedPressureSOM.sort_values(['Meters','Date']).reset_index(drop=True)
pressureSOM['Date'] = pd.to_datetime(pressureSOM['Date'], format='%d/%m/%Y')
pressureSOM
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2019-04-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2019-05-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2019-06-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2019-07-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2019-08-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>35167</th>
      <td>77258</td>
      <td>2021-11-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35168</th>
      <td>77258</td>
      <td>2021-12-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35169</th>
      <td>77258</td>
      <td>2022-01-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35170</th>
      <td>77258</td>
      <td>2022-02-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35171</th>
      <td>77258</td>
      <td>2022-03-01</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>35172 rows × 3 columns</p>
</div>




```python
### Meter Pressure + Height SOM ###
metersPressureHeightSOM = pd.merge(pressureSOM,metersHeightSOM,on='Meters')
metersPressureHeightSOM['Pressure+Height'] = metersPressureHeightSOM['Pressure'] + metersPressureHeightSOM['HEIGHT']

### Meter Pressure + Height SOM ALTERNATIVE ###
metersPressureHeightSOM_ALT = pd.merge(pressureSOM,metersHeightSOM_ALT,on='Meters')
metersPressureHeightSOM_ALT['Pressure+Height'] = metersPressureHeightSOM_ALT['Pressure'] + metersPressureHeightSOM_ALT['HEIGHT']

### Concat Original + Alt ###
metersPressureHeightSOM_FULL = pd.concat([metersPressureHeightSOM_ALT, metersPressureHeightSOM], ignore_index=True, sort=False)
metersPressureHeightSOM_FULL
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2019-04-01</td>
      <td>NaN</td>
      <td>42.29</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2019-04-01</td>
      <td>NaN</td>
      <td>40.99</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2019-04-01</td>
      <td>NaN</td>
      <td>180.13</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2019-05-01</td>
      <td>NaN</td>
      <td>42.29</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2019-05-01</td>
      <td>NaN</td>
      <td>40.99</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>17419</th>
      <td>40110</td>
      <td>2021-11-01</td>
      <td>56.694587</td>
      <td>66.25</td>
      <td>122.944587</td>
    </tr>
    <tr>
      <th>17420</th>
      <td>40110</td>
      <td>2021-12-01</td>
      <td>56.648172</td>
      <td>66.25</td>
      <td>122.898172</td>
    </tr>
    <tr>
      <th>17421</th>
      <td>40110</td>
      <td>2022-01-01</td>
      <td>NaN</td>
      <td>66.25</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17422</th>
      <td>40110</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>66.25</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17423</th>
      <td>40110</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>66.25</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>17424 rows × 5 columns</p>
</div>




```python
### Complete SOM Metering Pressure ###
DMAmetersPressureHeightSOM = pd.merge(metersPressureHeightSOM_FULL, meteringDMASOM, on='Meters')
DMAmetersPressureHeightSOM
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
      <th>DMA_Set</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>503</td>
      <td>2019-04-01</td>
      <td>74.946524</td>
      <td>188.84</td>
      <td>263.786524</td>
      <td>ZABINB01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>503</td>
      <td>2019-04-01</td>
      <td>74.946524</td>
      <td>188.84</td>
      <td>263.786524</td>
      <td>ZHURTW01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>503</td>
      <td>2019-04-01</td>
      <td>74.946524</td>
      <td>188.84</td>
      <td>263.786524</td>
      <td>ZHURTW04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>503</td>
      <td>2019-05-01</td>
      <td>77.246593</td>
      <td>188.84</td>
      <td>266.086593</td>
      <td>ZABINB01</td>
    </tr>
    <tr>
      <th>4</th>
      <td>503</td>
      <td>2019-05-01</td>
      <td>77.246593</td>
      <td>188.84</td>
      <td>266.086593</td>
      <td>ZHURTW01</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7843</th>
      <td>40102</td>
      <td>2021-11-01</td>
      <td>66.543827</td>
      <td>97.95</td>
      <td>164.493827</td>
      <td>ZCOLDA05</td>
    </tr>
    <tr>
      <th>7844</th>
      <td>40102</td>
      <td>2021-12-01</td>
      <td>64.862806</td>
      <td>97.95</td>
      <td>162.812806</td>
      <td>ZCOLDA05</td>
    </tr>
    <tr>
      <th>7845</th>
      <td>40102</td>
      <td>2022-01-01</td>
      <td>NaN</td>
      <td>97.95</td>
      <td>NaN</td>
      <td>ZCOLDA05</td>
    </tr>
    <tr>
      <th>7846</th>
      <td>40102</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>97.95</td>
      <td>NaN</td>
      <td>ZCOLDA05</td>
    </tr>
    <tr>
      <th>7847</th>
      <td>40102</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>97.95</td>
      <td>NaN</td>
      <td>ZCOLDA05</td>
    </tr>
  </tbody>
</table>
<p>7848 rows × 6 columns</p>
</div>




```python
# dfSOM = DMAmetersPressureHeightSOM.groupby(['Meters', pd.Grouper(key='Date', freq='12M')]).mean().reset_index()
# dfSOM.head(50)
```

## DM Pressure


```python
### Pressure by Meter ###
pressureFile = pd.read_excel(folder + 'DMA_Pressure_Query.xlsx',
              sheet_name='Pressure') 
pressureData = pressureFile.drop(columns = ['Path'])
```


```python
pressureData['Meter'] = pressureData['Meter'].str.replace(' Pressure','').str.replace('~PRESSURE','')
textData = ['[-11057] Not Enough Values For Calculation', 'Cannot do summary calculation on non-numeric data.','[-11059] No Good Data For Calculation','6.42120838954914E+29']
pressureData.replace(to_replace = textData, value = -1000, inplace = True)
meterPressureData = pressureData.set_index('Meter')
meterPressureData[meterPressureData < 0] = np.nan
meterPressureData[meterPressureData > 250] = np.nan
#meterPressureData
```


```python
## Melt Data ##
meterPressureData['Meters'] = meterPressureData.index
meltedPressure = meterPressureData.melt(id_vars=["Meters"], var_name="Date", value_name="Pressure")
pressure = meltedPressure.sort_values(['Meters','Date']).reset_index(drop=True)
pressure['Date'] = pd.to_datetime(pressure['Date'], format='%d/%m/%Y')
pressure
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DM00000</td>
      <td>2019-04-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>DM00000</td>
      <td>2019-05-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DM00000</td>
      <td>2019-06-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>DM00000</td>
      <td>2019-07-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DM00000</td>
      <td>2019-08-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>143599</th>
      <td>WWT:YARDP2ZZ:RY_YARDMEAD SPS.Rising Main</td>
      <td>2021-11-01</td>
      <td>0.778479</td>
    </tr>
    <tr>
      <th>143600</th>
      <td>WWT:YARDP2ZZ:RY_YARDMEAD SPS.Rising Main</td>
      <td>2021-12-01</td>
      <td>0.774191</td>
    </tr>
    <tr>
      <th>143601</th>
      <td>WWT:YARDP2ZZ:RY_YARDMEAD SPS.Rising Main</td>
      <td>2022-01-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>143602</th>
      <td>WWT:YARDP2ZZ:RY_YARDMEAD SPS.Rising Main</td>
      <td>2022-02-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>143603</th>
      <td>WWT:YARDP2ZZ:RY_YARDMEAD SPS.Rising Main</td>
      <td>2022-03-01</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>143604 rows × 3 columns</p>
</div>




```python
### Meter Pressure + Height 1 ###
metersPressureHeight = pd.merge(pressure,metersHeight_1,on='Meters')
metersPressureHeight['Pressure+Height'] = metersPressureHeight['Pressure'] + metersPressureHeight['HEIGHT']
metersPressureHeight
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DM00001</td>
      <td>2019-04-01</td>
      <td>84.518368</td>
      <td>124.50</td>
      <td>209.018368</td>
    </tr>
    <tr>
      <th>1</th>
      <td>DM00001</td>
      <td>2019-05-01</td>
      <td>95.766935</td>
      <td>124.50</td>
      <td>220.266935</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DM00001</td>
      <td>2019-06-01</td>
      <td>110.968271</td>
      <td>124.50</td>
      <td>235.468271</td>
    </tr>
    <tr>
      <th>3</th>
      <td>DM00001</td>
      <td>2019-07-01</td>
      <td>131.930612</td>
      <td>124.50</td>
      <td>256.430612</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DM00001</td>
      <td>2019-08-01</td>
      <td>52.204773</td>
      <td>124.50</td>
      <td>176.704773</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>118759</th>
      <td>DM19147</td>
      <td>2021-11-01</td>
      <td>26.595288</td>
      <td>157.67</td>
      <td>184.265288</td>
    </tr>
    <tr>
      <th>118760</th>
      <td>DM19147</td>
      <td>2021-12-01</td>
      <td>26.392765</td>
      <td>157.67</td>
      <td>184.062765</td>
    </tr>
    <tr>
      <th>118761</th>
      <td>DM19147</td>
      <td>2022-01-01</td>
      <td>NaN</td>
      <td>157.67</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>118762</th>
      <td>DM19147</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>157.67</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>118763</th>
      <td>DM19147</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>157.67</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>118764 rows × 5 columns</p>
</div>




```python
### Meter Pressure + Height ALTERNATIVES [2] ###
metersPressureHeight_ALT = pd.merge(pressure,metersHeight_2,on='Meters')
metersPressureHeight_ALT['Pressure+Height'] = metersPressureHeight_ALT['Pressure'] + metersPressureHeight_ALT['HEIGHT']
metersPressureHeight_ALT
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DM00006</td>
      <td>2019-04-01</td>
      <td>30.602044</td>
      <td>126.57</td>
      <td>157.172044</td>
    </tr>
    <tr>
      <th>1</th>
      <td>DM00006</td>
      <td>2019-05-01</td>
      <td>30.563021</td>
      <td>126.57</td>
      <td>157.133021</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DM00006</td>
      <td>2019-06-01</td>
      <td>32.085987</td>
      <td>126.57</td>
      <td>158.655987</td>
    </tr>
    <tr>
      <th>3</th>
      <td>DM00006</td>
      <td>2019-07-01</td>
      <td>32.499403</td>
      <td>126.57</td>
      <td>159.069403</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DM00006</td>
      <td>2019-08-01</td>
      <td>NaN</td>
      <td>126.57</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6439</th>
      <td>DM99998</td>
      <td>2021-11-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6440</th>
      <td>DM99998</td>
      <td>2021-12-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6441</th>
      <td>DM99998</td>
      <td>2022-01-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6442</th>
      <td>DM99998</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6443</th>
      <td>DM99998</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>6444 rows × 5 columns</p>
</div>




```python
### Concat metersPressureHeight ###
metersPressureHeight_FULL = pd.concat([metersPressureHeight, metersPressureHeight_ALT], ignore_index=True, sort=False)
metersPressureHeight_FULL
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DM00001</td>
      <td>2019-04-01</td>
      <td>84.518368</td>
      <td>124.50</td>
      <td>209.018368</td>
    </tr>
    <tr>
      <th>1</th>
      <td>DM00001</td>
      <td>2019-05-01</td>
      <td>95.766935</td>
      <td>124.50</td>
      <td>220.266935</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DM00001</td>
      <td>2019-06-01</td>
      <td>110.968271</td>
      <td>124.50</td>
      <td>235.468271</td>
    </tr>
    <tr>
      <th>3</th>
      <td>DM00001</td>
      <td>2019-07-01</td>
      <td>131.930612</td>
      <td>124.50</td>
      <td>256.430612</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DM00001</td>
      <td>2019-08-01</td>
      <td>52.204773</td>
      <td>124.50</td>
      <td>176.704773</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>125203</th>
      <td>DM99998</td>
      <td>2021-11-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125204</th>
      <td>DM99998</td>
      <td>2021-12-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125205</th>
      <td>DM99998</td>
      <td>2022-01-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125206</th>
      <td>DM99998</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125207</th>
      <td>DM99998</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>248.22</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>125208 rows × 5 columns</p>
</div>




```python
DMAmetersPressureHeight = pd.merge(metersPressureHeight_FULL, meteringDMA, on='Meters')
DMAmetersPressureHeight
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Meters</th>
      <th>Date</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
      <th>DMA_Set</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DM00001</td>
      <td>2019-04-01</td>
      <td>84.518368</td>
      <td>124.50</td>
      <td>209.018368</td>
      <td>ZWCTOW01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>DM00001</td>
      <td>2019-04-01</td>
      <td>84.518368</td>
      <td>124.50</td>
      <td>209.018368</td>
      <td>ZWCTOW04</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DM00001</td>
      <td>2019-05-01</td>
      <td>95.766935</td>
      <td>124.50</td>
      <td>220.266935</td>
      <td>ZWCTOW01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>DM00001</td>
      <td>2019-05-01</td>
      <td>95.766935</td>
      <td>124.50</td>
      <td>220.266935</td>
      <td>ZWCTOW04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DM00001</td>
      <td>2019-06-01</td>
      <td>110.968271</td>
      <td>124.50</td>
      <td>235.468271</td>
      <td>ZWCTOW01</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>177223</th>
      <td>DM18357</td>
      <td>2022-01-01</td>
      <td>NaN</td>
      <td>35.01</td>
      <td>NaN</td>
      <td>ZDARNH04</td>
    </tr>
    <tr>
      <th>177224</th>
      <td>DM18357</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>35.01</td>
      <td>NaN</td>
      <td>ZDARNH01</td>
    </tr>
    <tr>
      <th>177225</th>
      <td>DM18357</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>35.01</td>
      <td>NaN</td>
      <td>ZDARNH04</td>
    </tr>
    <tr>
      <th>177226</th>
      <td>DM18357</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>35.01</td>
      <td>NaN</td>
      <td>ZDARNH01</td>
    </tr>
    <tr>
      <th>177227</th>
      <td>DM18357</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>35.01</td>
      <td>NaN</td>
      <td>ZDARNH04</td>
    </tr>
  </tbody>
</table>
<p>177228 rows × 6 columns</p>
</div>



## Merge DM and SOM Datasets


```python
DMAmetersPressureHeightFULL = pd.concat([DMAmetersPressureHeight, DMAmetersPressureHeightSOM], ignore_index=True, sort=False)
DMAmetersPressureHeightFULL_0 = DMAmetersPressureHeightFULL.dropna()
DMAmetersPressureHeightFULL_0['Pressure+Height'] = DMAmetersPressureHeightFULL_0['Pressure+Height'].astype(float)
```


```python
# writer = pd.ExcelWriter(folder + 'DMAmetersPressureHeightFULL.xlsx')
# DMAmetersPressureHeightFULL.to_excel(writer, sheet_name = 'DMAmetersPressureHeightFULL')
# writer.save()
```


```python
# DMApressure = DMAmetersPressureHeightFULL.groupby(['DMA_Set','Date']).mean()
# pressureDMA = DMApressure.reset_index()
# pressureDMA

pressureDMA = DMAmetersPressureHeightFULL_0.groupby(['DMA_Set', pd.Grouper(key='Date', freq='12M')]).mean().reset_index()
pressureDMA.head(50)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Date</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>FINSLCA1</td>
      <td>2019-04-30</td>
      <td>18.080</td>
      <td>38.392496</td>
    </tr>
    <tr>
      <th>1</th>
      <td>FINSLCA1</td>
      <td>2020-04-30</td>
      <td>18.080</td>
      <td>39.221606</td>
    </tr>
    <tr>
      <th>2</th>
      <td>FINSLCA1</td>
      <td>2021-04-30</td>
      <td>18.080</td>
      <td>40.421397</td>
    </tr>
    <tr>
      <th>3</th>
      <td>FINSLCA1</td>
      <td>2022-04-30</td>
      <td>18.080</td>
      <td>41.237293</td>
    </tr>
    <tr>
      <th>4</th>
      <td>FINSLCA2</td>
      <td>2019-04-30</td>
      <td>12.965</td>
      <td>30.202797</td>
    </tr>
    <tr>
      <th>5</th>
      <td>FINSLCA2</td>
      <td>2020-04-30</td>
      <td>12.965</td>
      <td>34.553071</td>
    </tr>
    <tr>
      <th>6</th>
      <td>FINSLCA2</td>
      <td>2021-04-30</td>
      <td>12.965</td>
      <td>35.569250</td>
    </tr>
    <tr>
      <th>7</th>
      <td>FINSLCA2</td>
      <td>2022-04-30</td>
      <td>12.965</td>
      <td>35.861229</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ZABINB01</td>
      <td>2019-04-30</td>
      <td>188.840</td>
      <td>263.786524</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ZABINB01</td>
      <td>2020-04-30</td>
      <td>188.840</td>
      <td>265.039419</td>
    </tr>
    <tr>
      <th>10</th>
      <td>ZABINB01</td>
      <td>2021-04-30</td>
      <td>188.840</td>
      <td>262.008441</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ZABINB01</td>
      <td>2022-04-30</td>
      <td>188.840</td>
      <td>261.058924</td>
    </tr>
    <tr>
      <th>12</th>
      <td>ZABIND01</td>
      <td>2019-04-30</td>
      <td>57.100</td>
      <td>94.794388</td>
    </tr>
    <tr>
      <th>13</th>
      <td>ZABIND01</td>
      <td>2020-04-30</td>
      <td>57.100</td>
      <td>93.919386</td>
    </tr>
    <tr>
      <th>14</th>
      <td>ZABIND01</td>
      <td>2021-04-30</td>
      <td>57.100</td>
      <td>92.772825</td>
    </tr>
    <tr>
      <th>15</th>
      <td>ZABIND01</td>
      <td>2022-04-30</td>
      <td>57.100</td>
      <td>92.127974</td>
    </tr>
    <tr>
      <th>16</th>
      <td>ZABIND02</td>
      <td>2019-04-30</td>
      <td>52.080</td>
      <td>82.820692</td>
    </tr>
    <tr>
      <th>17</th>
      <td>ZABIND02</td>
      <td>2020-04-30</td>
      <td>52.080</td>
      <td>82.152689</td>
    </tr>
    <tr>
      <th>18</th>
      <td>ZABIND02</td>
      <td>2021-04-30</td>
      <td>52.080</td>
      <td>81.214243</td>
    </tr>
    <tr>
      <th>19</th>
      <td>ZABIND02</td>
      <td>2022-04-30</td>
      <td>52.080</td>
      <td>80.898201</td>
    </tr>
    <tr>
      <th>20</th>
      <td>ZABIND03</td>
      <td>2019-04-30</td>
      <td>62.420</td>
      <td>81.125872</td>
    </tr>
    <tr>
      <th>21</th>
      <td>ZABIND03</td>
      <td>2020-04-30</td>
      <td>62.420</td>
      <td>80.284251</td>
    </tr>
    <tr>
      <th>22</th>
      <td>ZABIND03</td>
      <td>2021-04-30</td>
      <td>62.420</td>
      <td>82.099515</td>
    </tr>
    <tr>
      <th>23</th>
      <td>ZABIND03</td>
      <td>2022-04-30</td>
      <td>62.420</td>
      <td>82.291773</td>
    </tr>
    <tr>
      <th>24</th>
      <td>ZABIND04</td>
      <td>2019-04-30</td>
      <td>68.880</td>
      <td>86.875076</td>
    </tr>
    <tr>
      <th>25</th>
      <td>ZABIND04</td>
      <td>2020-04-30</td>
      <td>68.880</td>
      <td>87.536237</td>
    </tr>
    <tr>
      <th>26</th>
      <td>ZABIND04</td>
      <td>2021-04-30</td>
      <td>68.880</td>
      <td>88.006322</td>
    </tr>
    <tr>
      <th>27</th>
      <td>ZABIND04</td>
      <td>2022-04-30</td>
      <td>68.880</td>
      <td>88.488848</td>
    </tr>
    <tr>
      <th>28</th>
      <td>ZABIND05</td>
      <td>2019-04-30</td>
      <td>65.920</td>
      <td>101.822485</td>
    </tr>
    <tr>
      <th>29</th>
      <td>ZABIND05</td>
      <td>2020-04-30</td>
      <td>65.920</td>
      <td>101.891672</td>
    </tr>
    <tr>
      <th>30</th>
      <td>ZABIND05</td>
      <td>2021-04-30</td>
      <td>65.920</td>
      <td>102.152982</td>
    </tr>
    <tr>
      <th>31</th>
      <td>ZABIND05</td>
      <td>2022-04-30</td>
      <td>65.920</td>
      <td>101.607956</td>
    </tr>
    <tr>
      <th>32</th>
      <td>ZABIND06</td>
      <td>2019-04-30</td>
      <td>62.190</td>
      <td>81.999392</td>
    </tr>
    <tr>
      <th>33</th>
      <td>ZABIND06</td>
      <td>2020-04-30</td>
      <td>62.190</td>
      <td>74.811822</td>
    </tr>
    <tr>
      <th>34</th>
      <td>ZABIND06</td>
      <td>2021-04-30</td>
      <td>62.190</td>
      <td>85.062348</td>
    </tr>
    <tr>
      <th>35</th>
      <td>ZABIND06</td>
      <td>2022-04-30</td>
      <td>62.190</td>
      <td>84.748137</td>
    </tr>
    <tr>
      <th>36</th>
      <td>ZABIND07</td>
      <td>2019-04-30</td>
      <td>55.485</td>
      <td>85.773525</td>
    </tr>
    <tr>
      <th>37</th>
      <td>ZABIND07</td>
      <td>2020-04-30</td>
      <td>55.485</td>
      <td>85.658740</td>
    </tr>
    <tr>
      <th>38</th>
      <td>ZABIND07</td>
      <td>2021-04-30</td>
      <td>55.485</td>
      <td>82.082176</td>
    </tr>
    <tr>
      <th>39</th>
      <td>ZABIND07</td>
      <td>2022-04-30</td>
      <td>55.485</td>
      <td>80.424276</td>
    </tr>
    <tr>
      <th>40</th>
      <td>ZABIND08</td>
      <td>2019-04-30</td>
      <td>50.270</td>
      <td>81.949759</td>
    </tr>
    <tr>
      <th>41</th>
      <td>ZABIND08</td>
      <td>2020-04-30</td>
      <td>50.270</td>
      <td>81.664571</td>
    </tr>
    <tr>
      <th>42</th>
      <td>ZABIND08</td>
      <td>2021-04-30</td>
      <td>50.270</td>
      <td>77.626392</td>
    </tr>
    <tr>
      <th>43</th>
      <td>ZABIND08</td>
      <td>2022-04-30</td>
      <td>50.270</td>
      <td>74.762184</td>
    </tr>
    <tr>
      <th>44</th>
      <td>ZADDIN01</td>
      <td>2019-04-30</td>
      <td>87.116</td>
      <td>136.486367</td>
    </tr>
    <tr>
      <th>45</th>
      <td>ZADDIN01</td>
      <td>2020-04-30</td>
      <td>87.116</td>
      <td>135.621262</td>
    </tr>
    <tr>
      <th>46</th>
      <td>ZADDIN01</td>
      <td>2021-04-30</td>
      <td>87.116</td>
      <td>136.199602</td>
    </tr>
    <tr>
      <th>47</th>
      <td>ZADDIN01</td>
      <td>2022-04-30</td>
      <td>87.116</td>
      <td>139.249310</td>
    </tr>
    <tr>
      <th>48</th>
      <td>ZADDIN02</td>
      <td>2019-04-30</td>
      <td>52.210</td>
      <td>129.335601</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ZADDIN02</td>
      <td>2020-04-30</td>
      <td>52.210</td>
      <td>125.930834</td>
    </tr>
  </tbody>
</table>
</div>



## DMA Original List


```python
### Merge with DMA List ###
dmaListFile = pd.read_excel(folder + 'DMA_List.xlsx',
              sheet_name='DMA_List')
dmaListFile_1 = dmaListFile[['DMAAREACODE']]
dmaList = dmaListFile_1.drop_duplicates().rename(columns={'DMAAREACODE':'DMA_Set'})
dmaList
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZWIDDN24</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ZSTRMR07</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ZWITNY11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ZSUHIL33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ZBOWSH04</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>1802</th>
      <td>ZPEWLY03</td>
    </tr>
    <tr>
      <th>1803</th>
      <td>ZCRCHH84</td>
    </tr>
    <tr>
      <th>1804</th>
      <td>ZHOGSB02</td>
    </tr>
    <tr>
      <th>1805</th>
      <td>ZTILEH08</td>
    </tr>
    <tr>
      <th>1806</th>
      <td>ZWINWD14</td>
    </tr>
  </tbody>
</table>
<p>1715 rows × 1 columns</p>
</div>



## Pressure at DMA Level from Meters


```python
pressureDMALevel = pd.merge(dmaList, pressureDMA, on = 'DMA_Set')
pressureDMALevel
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Date</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZWIDDN24</td>
      <td>2021-04-30</td>
      <td>171.000000</td>
      <td>199.728171</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ZWIDDN24</td>
      <td>2022-04-30</td>
      <td>171.000000</td>
      <td>199.817577</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ZSTRMR07</td>
      <td>2019-04-30</td>
      <td>14.270000</td>
      <td>65.081635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ZSTRMR07</td>
      <td>2020-04-30</td>
      <td>14.270000</td>
      <td>67.357334</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ZSTRMR07</td>
      <td>2021-04-30</td>
      <td>14.270000</td>
      <td>60.468982</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6236</th>
      <td>ZTILEH08</td>
      <td>2022-04-30</td>
      <td>45.580000</td>
      <td>95.975285</td>
    </tr>
    <tr>
      <th>6237</th>
      <td>ZWINWD14</td>
      <td>2019-04-30</td>
      <td>75.600000</td>
      <td>100.063879</td>
    </tr>
    <tr>
      <th>6238</th>
      <td>ZWINWD14</td>
      <td>2020-04-30</td>
      <td>75.600000</td>
      <td>99.808095</td>
    </tr>
    <tr>
      <th>6239</th>
      <td>ZWINWD14</td>
      <td>2021-04-30</td>
      <td>75.600000</td>
      <td>99.856741</td>
    </tr>
    <tr>
      <th>6240</th>
      <td>ZWINWD14</td>
      <td>2022-04-30</td>
      <td>76.966667</td>
      <td>109.968997</td>
    </tr>
  </tbody>
</table>
<p>6241 rows × 4 columns</p>
</div>



## CPP


```python
### CPP Height ##

PressureFittingFile = 'PressureFitting.csv'
PressureFittingReadFile = pd.read_csv(folder + PressureFittingFile, thousands=r',')
PressureFittingDF = pd.DataFrame(PressureFittingReadFile)

PressureFitting = PressureFittingDF[['REFERENCE','HEIGHT','NETWORKCODE']].dropna()

PressureFitting['REFERENCE'] = PressureFitting['REFERENCE'].str.replace('CPP','')

alfbt = 'S|L|M|P'

PressureFitting_0 = PressureFitting[PressureFitting["REFERENCE"].str.contains(alfbt)==False]
PressureFitting_0['REFERENCE_Nbr_Int'] = PressureFitting_0['REFERENCE'].astype('int64')
PressureFitting_1 = PressureFitting_0.drop(columns=['REFERENCE']).rename(columns={'REFERENCE_Nbr_Int' : 'CPP'})

PressureFitting_1

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HEIGHT</th>
      <th>NETWORKCODE</th>
      <th>CPP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>37.62</td>
      <td>ZRUSSH11</td>
      <td>2473</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.70</td>
      <td>ZBARHT45</td>
      <td>4027</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11.44</td>
      <td>ZBARHT45</td>
      <td>4169</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.21</td>
      <td>ZBARHT45</td>
      <td>4169</td>
    </tr>
    <tr>
      <th>4</th>
      <td>44.79</td>
      <td>ZRUSSH11</td>
      <td>4169</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>684</th>
      <td>67.65</td>
      <td>ZBRIBE01</td>
      <td>9995</td>
    </tr>
    <tr>
      <th>685</th>
      <td>141.53</td>
      <td>ZBLOCK02</td>
      <td>9996</td>
    </tr>
    <tr>
      <th>686</th>
      <td>55.87</td>
      <td>ZBERFD01</td>
      <td>9997</td>
    </tr>
    <tr>
      <th>687</th>
      <td>116.57</td>
      <td>ZAMERH01</td>
      <td>9998</td>
    </tr>
    <tr>
      <th>688</th>
      <td>55.09</td>
      <td>ZHGHBB01</td>
      <td>9999</td>
    </tr>
  </tbody>
</table>
<p>689 rows × 3 columns</p>
</div>



## CPP PRESSURE DATA


```python
### Pressure by CPP ###
cppFile = pd.read_excel(folder + 'DMA_Pressure_Query_CPP.xlsx',
              sheet_name='Pressure') 
cppData = cppFile.drop(columns = ['Path'])
cppData.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CPP</th>
      <th>2019-04-01 00:00:00</th>
      <th>2019-05-01 00:00:00</th>
      <th>2019-06-01 00:00:00</th>
      <th>2019-07-01 00:00:00</th>
      <th>2019-08-01 00:00:00</th>
      <th>2019-09-01 00:00:00</th>
      <th>2019-10-01 00:00:00</th>
      <th>2019-11-01 00:00:00</th>
      <th>2019-12-01 00:00:00</th>
      <th>...</th>
      <th>2021-06-01 00:00:00</th>
      <th>2021-07-01 00:00:00</th>
      <th>2021-08-01 00:00:00</th>
      <th>2021-09-01 00:00:00</th>
      <th>2021-10-01 00:00:00</th>
      <th>2021-11-01 00:00:00</th>
      <th>2021-12-01 00:00:00</th>
      <th>2022-01-01 00:00:00</th>
      <th>2022-02-01 00:00:00</th>
      <th>2022-03-01 00:00:00</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CPP_0074_PRESSURE_R</td>
      <td>29.008993</td>
      <td>29.010719</td>
      <td>28.958056</td>
      <td>28.671102</td>
      <td>28.996069</td>
      <td>28.910691</td>
      <td>28.956007</td>
      <td>29.011111</td>
      <td>29.112903</td>
      <td>...</td>
      <td>28.630486</td>
      <td>28.589688</td>
      <td>28.733837</td>
      <td>28.836528</td>
      <td>29.055638</td>
      <td>29.030969</td>
      <td>29.001903</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CPP_0077_PRESSURE_R</td>
      <td>44.560954</td>
      <td>44.506017</td>
      <td>44.418416</td>
      <td>44.479973</td>
      <td>44.54597</td>
      <td>44.561084</td>
      <td>44.560383</td>
      <td>44.591875</td>
      <td>46.49717</td>
      <td>...</td>
      <td>44.591667</td>
      <td>44.478233</td>
      <td>44.556956</td>
      <td>44.58382</td>
      <td>44.537798</td>
      <td>44.611046</td>
      <td>44.600758</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CPP_0078_PRESSURE_R</td>
      <td>35.728841</td>
      <td>35.581539</td>
      <td>35.380602</td>
      <td>34.297844</td>
      <td>34.902423</td>
      <td>35.266098</td>
      <td>35.986089</td>
      <td>35.763996</td>
      <td>36.084284</td>
      <td>...</td>
      <td>34.083264</td>
      <td>33.970373</td>
      <td>35.12631</td>
      <td>35.130382</td>
      <td>35.627584</td>
      <td>35.685868</td>
      <td>36.09803</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CPP_0079_PRESSURE_R</td>
      <td>24.448902</td>
      <td>24.453212</td>
      <td>24.485234</td>
      <td>24.706022</td>
      <td>24.529432</td>
      <td>24.615879</td>
      <td>24.588912</td>
      <td>24.674132</td>
      <td>24.755092</td>
      <td>...</td>
      <td>24.738317</td>
      <td>24.818952</td>
      <td>25.083569</td>
      <td>25.264003</td>
      <td>25.844428</td>
      <td>25.973611</td>
      <td>26.239318</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CPP_0080_PRESSURE_R</td>
      <td>37.269583</td>
      <td>37.086007</td>
      <td>36.891736</td>
      <td>36.387697</td>
      <td>36.809106</td>
      <td>37.169583</td>
      <td>37.542363</td>
      <td>37.367547</td>
      <td>37.306557</td>
      <td>...</td>
      <td>36.561512</td>
      <td>36.485455</td>
      <td>36.893044</td>
      <td>36.797371</td>
      <td>37.278758</td>
      <td>37.389268</td>
      <td>37.258939</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
      <td>[-11057] Not Enough Values For Calculation</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 37 columns</p>
</div>




```python
cppData['CPP'] = cppData['CPP'].str.replace('_','').str.replace('PRESSURER','').str.replace('CPP','')

alfbt = 'T|L'

cppData_0 = cppData[cppData["CPP"].str.contains(alfbt)==False]
cppData_0['CPP_Nbr_Int'] = cppData_0['CPP'].astype('int64')
cppData_1 = cppData_0.drop(columns=['CPP']).rename(columns={'CPP_Nbr_Int' : 'CPP'})

textData = ['[-11057] Not Enough Values For Calculation', 'Cannot do summary calculation on non-numeric data.','[-11059] No Good Data For Calculation','6.42120838954914E+29']
cppData_1.replace(to_replace = textData, value = -1000, inplace = True)
cppData_2 = cppData_1.set_index('CPP')
cppData_2[cppData_2 < 0] = np.nan
cppData_2[cppData_2 > 250] = np.nan
cppData_2
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2019-04-01</th>
      <th>2019-05-01</th>
      <th>2019-06-01</th>
      <th>2019-07-01</th>
      <th>2019-08-01</th>
      <th>2019-09-01</th>
      <th>2019-10-01</th>
      <th>2019-11-01</th>
      <th>2019-12-01</th>
      <th>2020-01-01</th>
      <th>...</th>
      <th>2021-06-01</th>
      <th>2021-07-01</th>
      <th>2021-08-01</th>
      <th>2021-09-01</th>
      <th>2021-10-01</th>
      <th>2021-11-01</th>
      <th>2021-12-01</th>
      <th>2022-01-01</th>
      <th>2022-02-01</th>
      <th>2022-03-01</th>
    </tr>
    <tr>
      <th>CPP</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>74</th>
      <td>29.008993</td>
      <td>29.010719</td>
      <td>28.958056</td>
      <td>28.671102</td>
      <td>28.996069</td>
      <td>28.910691</td>
      <td>28.956007</td>
      <td>29.011111</td>
      <td>29.112903</td>
      <td>29.141431</td>
      <td>...</td>
      <td>28.630486</td>
      <td>28.589688</td>
      <td>28.733837</td>
      <td>28.836528</td>
      <td>29.055638</td>
      <td>29.030969</td>
      <td>29.001903</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>77</th>
      <td>44.560954</td>
      <td>44.506017</td>
      <td>44.418416</td>
      <td>44.479973</td>
      <td>44.545970</td>
      <td>44.561084</td>
      <td>44.560383</td>
      <td>44.591875</td>
      <td>46.497170</td>
      <td>46.738741</td>
      <td>...</td>
      <td>44.591667</td>
      <td>44.478233</td>
      <td>44.556956</td>
      <td>44.583820</td>
      <td>44.537798</td>
      <td>44.611046</td>
      <td>44.600758</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>78</th>
      <td>35.728841</td>
      <td>35.581539</td>
      <td>35.380602</td>
      <td>34.297844</td>
      <td>34.902423</td>
      <td>35.266098</td>
      <td>35.986089</td>
      <td>35.763996</td>
      <td>36.084284</td>
      <td>35.875161</td>
      <td>...</td>
      <td>34.083264</td>
      <td>33.970373</td>
      <td>35.126310</td>
      <td>35.130382</td>
      <td>35.627584</td>
      <td>35.685868</td>
      <td>36.098030</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>79</th>
      <td>24.448902</td>
      <td>24.453212</td>
      <td>24.485234</td>
      <td>24.706022</td>
      <td>24.529432</td>
      <td>24.615879</td>
      <td>24.588912</td>
      <td>24.674132</td>
      <td>24.755092</td>
      <td>24.814022</td>
      <td>...</td>
      <td>24.738317</td>
      <td>24.818952</td>
      <td>25.083569</td>
      <td>25.264003</td>
      <td>25.844428</td>
      <td>25.973611</td>
      <td>26.239318</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>80</th>
      <td>37.269583</td>
      <td>37.086007</td>
      <td>36.891736</td>
      <td>36.387697</td>
      <td>36.809106</td>
      <td>37.169583</td>
      <td>37.542363</td>
      <td>37.367547</td>
      <td>37.306557</td>
      <td>37.388471</td>
      <td>...</td>
      <td>36.561512</td>
      <td>36.485455</td>
      <td>36.893044</td>
      <td>36.797371</td>
      <td>37.278758</td>
      <td>37.389268</td>
      <td>37.258939</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>32.444411</td>
      <td>32.264202</td>
      <td>32.257829</td>
      <td>32.588299</td>
      <td>32.779597</td>
      <td>33.055196</td>
      <td>33.188769</td>
      <td>33.218655</td>
      <td>...</td>
      <td>30.550069</td>
      <td>28.993116</td>
      <td>46.228427</td>
      <td>46.379757</td>
      <td>44.067953</td>
      <td>37.681910</td>
      <td>35.432424</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>60.915747</td>
      <td>63.002118</td>
      <td>63.162811</td>
      <td>...</td>
      <td>62.354062</td>
      <td>62.719678</td>
      <td>62.964785</td>
      <td>63.057569</td>
      <td>63.417523</td>
      <td>63.536250</td>
      <td>63.219788</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>26.720771</td>
      <td>26.578259</td>
      <td>26.787500</td>
      <td>26.716487</td>
      <td>26.825436</td>
      <td>26.952847</td>
      <td>27.193347</td>
      <td>27.489475</td>
      <td>...</td>
      <td>27.373611</td>
      <td>27.463878</td>
      <td>27.983401</td>
      <td>27.962083</td>
      <td>28.249664</td>
      <td>28.289792</td>
      <td>27.998106</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>40.501976</td>
      <td>40.543414</td>
      <td>40.662399</td>
      <td>40.822492</td>
      <td>40.929933</td>
      <td>40.814028</td>
      <td>41.357460</td>
      <td>41.531082</td>
      <td>...</td>
      <td>40.177257</td>
      <td>39.994224</td>
      <td>40.244422</td>
      <td>40.285451</td>
      <td>40.215503</td>
      <td>41.093090</td>
      <td>39.763936</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>78.325930</td>
      <td>79.399394</td>
      <td>78.125972</td>
      <td>77.825706</td>
      <td>80.882133</td>
      <td>81.198991</td>
      <td>81.668781</td>
      <td>81.413477</td>
      <td>81.572605</td>
      <td>81.651246</td>
      <td>...</td>
      <td>0.225486</td>
      <td>0.159321</td>
      <td>0.175328</td>
      <td>0.173750</td>
      <td>0.154295</td>
      <td>0.185243</td>
      <td>0.091591</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>639 rows × 36 columns</p>
</div>




```python
## Melt Data ##
cppData_2['CPP'] = cppData_2.index
meltedCPP = cppData_2.melt(id_vars=["CPP"], var_name="Date", value_name="Pressure")
pressureCPP = meltedCPP.sort_values(['CPP','Date']).reset_index(drop=True)
pressureCPP['Date'] = pd.to_datetime(pressureCPP['Date'], format='%d/%m/%Y')
pressureCPP
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CPP</th>
      <th>Date</th>
      <th>Pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>2019-04-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>2019-05-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11</td>
      <td>2019-06-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11</td>
      <td>2019-07-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>2019-08-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>22999</th>
      <td>9999</td>
      <td>2021-11-01</td>
      <td>0.185243</td>
    </tr>
    <tr>
      <th>23000</th>
      <td>9999</td>
      <td>2021-12-01</td>
      <td>0.091591</td>
    </tr>
    <tr>
      <th>23001</th>
      <td>9999</td>
      <td>2022-01-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23002</th>
      <td>9999</td>
      <td>2022-02-01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23003</th>
      <td>9999</td>
      <td>2022-03-01</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>23004 rows × 3 columns</p>
</div>




```python
### CPP Pressure + Height ###
cppPressureHeight = pd.merge(pressureCPP,PressureFitting_1,on='CPP')
cppPressureHeight['Pressure+Height'] = cppPressureHeight['Pressure'] + cppPressureHeight['HEIGHT']
cppPressureHeight
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CPP</th>
      <th>Date</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>NETWORKCODE</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>2019-04-01</td>
      <td>NaN</td>
      <td>95.88</td>
      <td>ZBURGT01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>2019-05-01</td>
      <td>NaN</td>
      <td>95.88</td>
      <td>ZBURGT01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11</td>
      <td>2019-06-01</td>
      <td>NaN</td>
      <td>95.88</td>
      <td>ZBURGT01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11</td>
      <td>2019-07-01</td>
      <td>NaN</td>
      <td>95.88</td>
      <td>ZBURGT01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>2019-08-01</td>
      <td>NaN</td>
      <td>95.88</td>
      <td>ZBURGT01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>22315</th>
      <td>9999</td>
      <td>2021-11-01</td>
      <td>0.185243</td>
      <td>55.09</td>
      <td>ZHGHBB01</td>
      <td>55.275243</td>
    </tr>
    <tr>
      <th>22316</th>
      <td>9999</td>
      <td>2021-12-01</td>
      <td>0.091591</td>
      <td>55.09</td>
      <td>ZHGHBB01</td>
      <td>55.181591</td>
    </tr>
    <tr>
      <th>22317</th>
      <td>9999</td>
      <td>2022-01-01</td>
      <td>NaN</td>
      <td>55.09</td>
      <td>ZHGHBB01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22318</th>
      <td>9999</td>
      <td>2022-02-01</td>
      <td>NaN</td>
      <td>55.09</td>
      <td>ZHGHBB01</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22319</th>
      <td>9999</td>
      <td>2022-03-01</td>
      <td>NaN</td>
      <td>55.09</td>
      <td>ZHGHBB01</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>22320 rows × 6 columns</p>
</div>




```python
# DMApressureCPP = cppPressureHeight.groupby(['NETWORKCODE','Date']).mean()
# CPPpressureDMA = DMApressureCPP.reset_index()
# CPPpressureDMA

CPPpressureDMA = cppPressureHeight.groupby(['NETWORKCODE', pd.Grouper(key='Date', freq='12M')]).mean().reset_index()
CPPpressureDMA.head(50)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NETWORKCODE</th>
      <th>Date</th>
      <th>CPP</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZABINB01</td>
      <td>2019-04-30</td>
      <td>5118.000000</td>
      <td>59.195761</td>
      <td>199.580000</td>
      <td>258.775761</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ZABINB01</td>
      <td>2020-04-30</td>
      <td>5118.000000</td>
      <td>61.445273</td>
      <td>199.580000</td>
      <td>261.025273</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ZABINB01</td>
      <td>2021-04-30</td>
      <td>5118.000000</td>
      <td>61.264390</td>
      <td>199.580000</td>
      <td>260.844390</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ZABINB01</td>
      <td>2022-04-30</td>
      <td>5118.000000</td>
      <td>60.077811</td>
      <td>199.580000</td>
      <td>259.657811</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ZABIND09</td>
      <td>2019-04-30</td>
      <td>7055.000000</td>
      <td>17.889236</td>
      <td>92.270000</td>
      <td>110.159236</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ZABIND09</td>
      <td>2020-04-30</td>
      <td>7055.000000</td>
      <td>17.672347</td>
      <td>92.270000</td>
      <td>109.942347</td>
    </tr>
    <tr>
      <th>6</th>
      <td>ZABIND09</td>
      <td>2021-04-30</td>
      <td>7055.000000</td>
      <td>17.590185</td>
      <td>92.270000</td>
      <td>109.860185</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ZABIND09</td>
      <td>2022-04-30</td>
      <td>7055.000000</td>
      <td>17.673469</td>
      <td>92.270000</td>
      <td>109.943469</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ZADDIN01</td>
      <td>2019-04-30</td>
      <td>191.000000</td>
      <td>29.898174</td>
      <td>107.285000</td>
      <td>137.183174</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ZADDIN01</td>
      <td>2020-04-30</td>
      <td>191.000000</td>
      <td>29.581908</td>
      <td>107.285000</td>
      <td>136.866908</td>
    </tr>
    <tr>
      <th>10</th>
      <td>ZADDIN01</td>
      <td>2021-04-30</td>
      <td>191.000000</td>
      <td>29.334707</td>
      <td>107.285000</td>
      <td>136.619707</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ZADDIN01</td>
      <td>2022-04-30</td>
      <td>191.000000</td>
      <td>29.507344</td>
      <td>107.285000</td>
      <td>136.792344</td>
    </tr>
    <tr>
      <th>12</th>
      <td>ZADDIN02</td>
      <td>2019-04-30</td>
      <td>50.000000</td>
      <td>32.108284</td>
      <td>101.070000</td>
      <td>133.178284</td>
    </tr>
    <tr>
      <th>13</th>
      <td>ZADDIN02</td>
      <td>2020-04-30</td>
      <td>50.000000</td>
      <td>32.263553</td>
      <td>101.070000</td>
      <td>133.333553</td>
    </tr>
    <tr>
      <th>14</th>
      <td>ZADDIN02</td>
      <td>2021-04-30</td>
      <td>50.000000</td>
      <td>31.392327</td>
      <td>101.070000</td>
      <td>132.462327</td>
    </tr>
    <tr>
      <th>15</th>
      <td>ZADDIN02</td>
      <td>2022-04-30</td>
      <td>50.000000</td>
      <td>31.086304</td>
      <td>101.070000</td>
      <td>132.156304</td>
    </tr>
    <tr>
      <th>16</th>
      <td>ZADDIN03</td>
      <td>2019-04-30</td>
      <td>3391.000000</td>
      <td>20.168730</td>
      <td>64.900000</td>
      <td>85.068730</td>
    </tr>
    <tr>
      <th>17</th>
      <td>ZADDIN03</td>
      <td>2020-04-30</td>
      <td>3391.000000</td>
      <td>20.193491</td>
      <td>64.900000</td>
      <td>85.093491</td>
    </tr>
    <tr>
      <th>18</th>
      <td>ZADDIN03</td>
      <td>2021-04-30</td>
      <td>3391.000000</td>
      <td>20.310931</td>
      <td>64.900000</td>
      <td>85.210931</td>
    </tr>
    <tr>
      <th>19</th>
      <td>ZADDIN03</td>
      <td>2022-04-30</td>
      <td>3391.000000</td>
      <td>21.317933</td>
      <td>64.900000</td>
      <td>86.217933</td>
    </tr>
    <tr>
      <th>20</th>
      <td>ZADDIN05</td>
      <td>2019-04-30</td>
      <td>1260.333333</td>
      <td>42.762292</td>
      <td>93.083333</td>
      <td>135.845626</td>
    </tr>
    <tr>
      <th>21</th>
      <td>ZADDIN05</td>
      <td>2020-04-30</td>
      <td>1260.333333</td>
      <td>42.248147</td>
      <td>93.083333</td>
      <td>135.331480</td>
    </tr>
    <tr>
      <th>22</th>
      <td>ZADDIN05</td>
      <td>2021-04-30</td>
      <td>1260.333333</td>
      <td>41.866558</td>
      <td>93.083333</td>
      <td>134.949891</td>
    </tr>
    <tr>
      <th>23</th>
      <td>ZADDIN05</td>
      <td>2022-04-30</td>
      <td>1260.333333</td>
      <td>42.176024</td>
      <td>93.083333</td>
      <td>135.259357</td>
    </tr>
    <tr>
      <th>24</th>
      <td>ZADDIN07</td>
      <td>2019-04-30</td>
      <td>1792.500000</td>
      <td>NaN</td>
      <td>123.670000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>ZADDIN07</td>
      <td>2020-04-30</td>
      <td>1792.500000</td>
      <td>20.856113</td>
      <td>123.670000</td>
      <td>137.128113</td>
    </tr>
    <tr>
      <th>26</th>
      <td>ZADDIN07</td>
      <td>2021-04-30</td>
      <td>1792.500000</td>
      <td>22.726937</td>
      <td>123.670000</td>
      <td>146.396937</td>
    </tr>
    <tr>
      <th>27</th>
      <td>ZADDIN07</td>
      <td>2022-04-30</td>
      <td>1792.500000</td>
      <td>22.785007</td>
      <td>123.670000</td>
      <td>146.455007</td>
    </tr>
    <tr>
      <th>28</th>
      <td>ZADDIN09</td>
      <td>2019-04-30</td>
      <td>196.000000</td>
      <td>20.762648</td>
      <td>96.000000</td>
      <td>116.762648</td>
    </tr>
    <tr>
      <th>29</th>
      <td>ZADDIN09</td>
      <td>2020-04-30</td>
      <td>196.000000</td>
      <td>19.515165</td>
      <td>96.000000</td>
      <td>115.515165</td>
    </tr>
    <tr>
      <th>30</th>
      <td>ZADDIN09</td>
      <td>2021-04-30</td>
      <td>196.000000</td>
      <td>20.820356</td>
      <td>96.000000</td>
      <td>116.820356</td>
    </tr>
    <tr>
      <th>31</th>
      <td>ZADDIN09</td>
      <td>2022-04-30</td>
      <td>196.000000</td>
      <td>21.578345</td>
      <td>96.000000</td>
      <td>117.578345</td>
    </tr>
    <tr>
      <th>32</th>
      <td>ZALDBN01</td>
      <td>2019-04-30</td>
      <td>3001.500000</td>
      <td>27.413229</td>
      <td>198.415000</td>
      <td>225.828229</td>
    </tr>
    <tr>
      <th>33</th>
      <td>ZALDBN01</td>
      <td>2020-04-30</td>
      <td>3001.500000</td>
      <td>26.766373</td>
      <td>198.415000</td>
      <td>225.181373</td>
    </tr>
    <tr>
      <th>34</th>
      <td>ZALDBN01</td>
      <td>2021-04-30</td>
      <td>3001.500000</td>
      <td>26.037706</td>
      <td>198.415000</td>
      <td>224.452706</td>
    </tr>
    <tr>
      <th>35</th>
      <td>ZALDBN01</td>
      <td>2022-04-30</td>
      <td>3001.500000</td>
      <td>25.027945</td>
      <td>198.415000</td>
      <td>223.442945</td>
    </tr>
    <tr>
      <th>36</th>
      <td>ZALDBR02</td>
      <td>2019-04-30</td>
      <td>5120.500000</td>
      <td>26.560451</td>
      <td>74.315000</td>
      <td>96.190451</td>
    </tr>
    <tr>
      <th>37</th>
      <td>ZALDBR02</td>
      <td>2020-04-30</td>
      <td>5120.500000</td>
      <td>32.259305</td>
      <td>74.315000</td>
      <td>105.905019</td>
    </tr>
    <tr>
      <th>38</th>
      <td>ZALDBR02</td>
      <td>2021-04-30</td>
      <td>5120.500000</td>
      <td>34.553774</td>
      <td>74.315000</td>
      <td>108.868774</td>
    </tr>
    <tr>
      <th>39</th>
      <td>ZALDBR02</td>
      <td>2022-04-30</td>
      <td>5120.500000</td>
      <td>34.704462</td>
      <td>74.315000</td>
      <td>109.019462</td>
    </tr>
    <tr>
      <th>40</th>
      <td>ZALDBR03</td>
      <td>2019-04-30</td>
      <td>5130.000000</td>
      <td>46.055746</td>
      <td>72.550000</td>
      <td>118.605746</td>
    </tr>
    <tr>
      <th>41</th>
      <td>ZALDBR03</td>
      <td>2020-04-30</td>
      <td>5130.000000</td>
      <td>42.490143</td>
      <td>72.550000</td>
      <td>116.271961</td>
    </tr>
    <tr>
      <th>42</th>
      <td>ZALDBR03</td>
      <td>2021-04-30</td>
      <td>5130.000000</td>
      <td>54.633762</td>
      <td>72.550000</td>
      <td>127.183762</td>
    </tr>
    <tr>
      <th>43</th>
      <td>ZALDBR03</td>
      <td>2022-04-30</td>
      <td>5130.000000</td>
      <td>40.201491</td>
      <td>72.550000</td>
      <td>112.751491</td>
    </tr>
    <tr>
      <th>44</th>
      <td>ZALDTW01</td>
      <td>2019-04-30</td>
      <td>35.000000</td>
      <td>33.425174</td>
      <td>164.230000</td>
      <td>197.655174</td>
    </tr>
    <tr>
      <th>45</th>
      <td>ZALDTW01</td>
      <td>2020-04-30</td>
      <td>35.000000</td>
      <td>33.566186</td>
      <td>164.230000</td>
      <td>197.796186</td>
    </tr>
    <tr>
      <th>46</th>
      <td>ZALDTW01</td>
      <td>2021-04-30</td>
      <td>35.000000</td>
      <td>33.702556</td>
      <td>164.230000</td>
      <td>197.932556</td>
    </tr>
    <tr>
      <th>47</th>
      <td>ZALDTW01</td>
      <td>2022-04-30</td>
      <td>35.000000</td>
      <td>33.576737</td>
      <td>164.230000</td>
      <td>197.806737</td>
    </tr>
    <tr>
      <th>48</th>
      <td>ZAMERH01</td>
      <td>2019-04-30</td>
      <td>9998.000000</td>
      <td>NaN</td>
      <td>116.570000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ZAMERH01</td>
      <td>2020-04-30</td>
      <td>9998.000000</td>
      <td>41.482946</td>
      <td>116.570000</td>
      <td>158.052946</td>
    </tr>
  </tbody>
</table>
</div>



### MATCH CPP WITH DMAS THAT ARE EMPTY


```python
### Return DMAs that don't match ###

notMatchDMAs_0 = dmaList.merge(pressureDMA,indicator = True, how='left').loc[lambda x : x['_merge']!='both']
notMatchDMAs = notMatchDMAs_0[['DMA_Set']].rename(columns = {'DMA_Set' : 'NETWORKCODE'})

## Merge DMAs that don't match with CPP Pressure
pressureDMA_CPP = pd.merge(notMatchDMAs, CPPpressureDMA, on = 'NETWORKCODE')
pressureDMA_CPP
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NETWORKCODE</th>
      <th>Date</th>
      <th>CPP</th>
      <th>Pressure</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZBOWSH07</td>
      <td>2019-04-30</td>
      <td>9990.0</td>
      <td>NaN</td>
      <td>62.27</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ZBOWSH07</td>
      <td>2020-04-30</td>
      <td>9990.0</td>
      <td>21.715330</td>
      <td>62.27</td>
      <td>83.985330</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ZBOWSH07</td>
      <td>2021-04-30</td>
      <td>9990.0</td>
      <td>22.665068</td>
      <td>62.27</td>
      <td>84.935068</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ZBOWSH07</td>
      <td>2022-04-30</td>
      <td>9990.0</td>
      <td>23.162576</td>
      <td>62.27</td>
      <td>85.432576</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ZGHILL01</td>
      <td>2019-04-30</td>
      <td>85.0</td>
      <td>49.535451</td>
      <td>186.52</td>
      <td>236.055451</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>423</th>
      <td>ZSTOKB01</td>
      <td>2022-04-30</td>
      <td>9976.0</td>
      <td>43.575254</td>
      <td>196.00</td>
      <td>239.575254</td>
    </tr>
    <tr>
      <th>424</th>
      <td>ZCIREN01</td>
      <td>2019-04-30</td>
      <td>1101.0</td>
      <td>27.280653</td>
      <td>128.77</td>
      <td>156.050653</td>
    </tr>
    <tr>
      <th>425</th>
      <td>ZCIREN01</td>
      <td>2020-04-30</td>
      <td>1101.0</td>
      <td>27.558514</td>
      <td>128.77</td>
      <td>156.328514</td>
    </tr>
    <tr>
      <th>426</th>
      <td>ZCIREN01</td>
      <td>2021-04-30</td>
      <td>1101.0</td>
      <td>27.120251</td>
      <td>128.77</td>
      <td>155.890251</td>
    </tr>
    <tr>
      <th>427</th>
      <td>ZCIREN01</td>
      <td>2022-04-30</td>
      <td>1101.0</td>
      <td>26.769212</td>
      <td>128.77</td>
      <td>155.539212</td>
    </tr>
  </tbody>
</table>
<p>428 rows × 6 columns</p>
</div>



## Concat CPP with the full data


```python
CPPpressureDMA = pressureDMA_CPP.rename(columns = {'NETWORKCODE' : 'DMA_Set'})
pressureDMA_Level = pd.concat([pressureDMALevel, CPPpressureDMA], ignore_index=True, sort=False).drop(columns = ['CPP'])
pressureDMA_Level
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Date</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
      <th>Pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZWIDDN24</td>
      <td>2021-04-30</td>
      <td>171.00</td>
      <td>199.728171</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ZWIDDN24</td>
      <td>2022-04-30</td>
      <td>171.00</td>
      <td>199.817577</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ZSTRMR07</td>
      <td>2019-04-30</td>
      <td>14.27</td>
      <td>65.081635</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ZSTRMR07</td>
      <td>2020-04-30</td>
      <td>14.27</td>
      <td>67.357334</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ZSTRMR07</td>
      <td>2021-04-30</td>
      <td>14.27</td>
      <td>60.468982</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6664</th>
      <td>ZSTOKB01</td>
      <td>2022-04-30</td>
      <td>196.00</td>
      <td>239.575254</td>
      <td>43.575254</td>
    </tr>
    <tr>
      <th>6665</th>
      <td>ZCIREN01</td>
      <td>2019-04-30</td>
      <td>128.77</td>
      <td>156.050653</td>
      <td>27.280653</td>
    </tr>
    <tr>
      <th>6666</th>
      <td>ZCIREN01</td>
      <td>2020-04-30</td>
      <td>128.77</td>
      <td>156.328514</td>
      <td>27.558514</td>
    </tr>
    <tr>
      <th>6667</th>
      <td>ZCIREN01</td>
      <td>2021-04-30</td>
      <td>128.77</td>
      <td>155.890251</td>
      <td>27.120251</td>
    </tr>
    <tr>
      <th>6668</th>
      <td>ZCIREN01</td>
      <td>2022-04-30</td>
      <td>128.77</td>
      <td>155.539212</td>
      <td>26.769212</td>
    </tr>
  </tbody>
</table>
<p>6669 rows × 5 columns</p>
</div>



## Check Values


```python
# ### Check Values ###
# oldPressure = pd.read_excel(folder + 'oldData.xlsx',
#               sheet_name='dma_by_month2018-2019') 
```


```python
# oldData = oldPressure.rename(columns={'dmacode':'DMA_Set', 'start_of_month':'Date'})
# oldData['Date'] = pd.to_datetime(oldData['Date'])
# oldDataPressure = oldData.sort_values(['DMA_Set','Date']).reset_index(drop=True)
# oldDataPressure.head()
```


```python
# diff = pd.merge(oldDataPressure,pressureDMA_Level, on=['DMA_Set','Date'])
# diff['Diff'] = diff['avgHead'] - diff['Pressure+Height']
# diff.head()
```

## Check unmatch DMAs


```python
notMatchDMAs_final = dmaList.merge(pressureDMA_Level,indicator = True, how='left').loc[lambda x : x['_merge']!='both']
notMatchDMAs_final.dropna(subset = ['DMA_Set']).drop_duplicates(subset = ['DMA_Set'])
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Date</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
      <th>Pressure</th>
      <th>_merge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>718</th>
      <td>ZBOWDN03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>783</th>
      <td>ZBEAHL06</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>933</th>
      <td>ZSTHFL08</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1053</th>
      <td>ZANGLB01</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1136</th>
      <td>ZFARDN03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1173</th>
      <td>ZPEWLY01</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1182</th>
      <td>ZHURTW07</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1564</th>
      <td>ZSTOW103</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1961</th>
      <td>ZBLOCK01</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2285</th>
      <td>ZCIREN05</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2422</th>
      <td>ZSEWRD50</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2605</th>
      <td>ZBLACK02</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2630</th>
      <td>ZHACKP04</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2635</th>
      <td>ZFLAXL04</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2887</th>
      <td>ZWOODF11</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2974</th>
      <td>ZSTMAS03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3051</th>
      <td>ZHOGSB03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3191</th>
      <td>ZCIREN03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3216</th>
      <td>ZBRETH08</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3291</th>
      <td>ZCIREN04</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3367</th>
      <td>ZCOLDA04</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3444</th>
      <td>ZSTMAS02</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3675</th>
      <td>ZPENHL05</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3894</th>
      <td>ZRAPSG02</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>4574</th>
      <td>ZBLUNS25</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>5795</th>
      <td>ZARDLY04</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>5917</th>
      <td>ZMILCM03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>5962</th>
      <td>ZELTHM26</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>6107</th>
      <td>ZFRNBO15</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>6343</th>
      <td>ZSUR3021</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>6436</th>
      <td>ZCHIGW70</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>6565</th>
      <td>ZNEWFM08</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>6694</th>
      <td>ZHOGSB02</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
  </tbody>
</table>
</div>




```python
# writer = pd.ExcelWriter(folder + 'pressureFULL_NEW.xlsx')
# pressureDMA_Level.to_excel(writer, sheet_name = 'pressureDMALevel')
# oldDataPressure.to_excel(writer, sheet_name='oldData')
# diff.to_excel(writer, sheet_name='diff')
# notMatchDMAs_final.to_excel(writer, sheet_name= 'unmatch')
# writer.save()
```

## Match GID IDs


```python
notMatchDMAs_final_DMA = notMatchDMAs_final[['DMA_Set']].dropna()

meterHeight_GISID_1 = NetworkMeterFile[['GISID', 'HEIGHT', 'DMA1CODE']].rename(columns={'DMA1CODE' : 'DMA_Set'})
gisIdMatch_1 = pd.merge(meterHeight_GISID_1,notMatchDMAs_final_DMA, on = 'DMA_Set')

meterHeight_GISID_2 = NetworkMeterFile[['GISID', 'HEIGHT', 'DMA2CODE']].rename(columns={'DMA2CODE' : 'DMA_Set'})
gisIdMatch_2 = pd.merge(meterHeight_GISID_2,notMatchDMAs_final_DMA, on = 'DMA_Set')

gisIdMatch = pd.concat([gisIdMatch_1, gisIdMatch_2], ignore_index=True, sort=False)
gisIdMatch
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>GISID</th>
      <th>HEIGHT</th>
      <th>DMA_Set</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1795767</td>
      <td>76.70</td>
      <td>ZCOLDA04</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1794336</td>
      <td>157.61</td>
      <td>ZCOLDA04</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1794104</td>
      <td>92.91</td>
      <td>ZCOLDA04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1795406</td>
      <td>86.93</td>
      <td>ZCOLDA04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1794008</td>
      <td>84.54</td>
      <td>ZCOLDA04</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>153</th>
      <td>1797261</td>
      <td>239.43</td>
      <td>ZRAPSG02</td>
    </tr>
    <tr>
      <th>154</th>
      <td>1797265</td>
      <td>266.90</td>
      <td>ZSTOW103</td>
    </tr>
    <tr>
      <th>155</th>
      <td>1796145</td>
      <td>144.68</td>
      <td>ZHOGSB03</td>
    </tr>
    <tr>
      <th>156</th>
      <td>1796144</td>
      <td>144.93</td>
      <td>ZHOGSB02</td>
    </tr>
    <tr>
      <th>157</th>
      <td>1789146</td>
      <td>146.67</td>
      <td>ZBRETH08</td>
    </tr>
  </tbody>
</table>
<p>158 rows × 3 columns</p>
</div>




```python
PI_GISID = pd.read_excel(folder + 'GIS_IDs.xlsx', sheet_name='PI_GISID').rename(columns={'GIS ID [usetint1]':'GISID'}) 
PI_GISID
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PI Data</th>
      <th>GISID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AEMS_SW_PDS_Sludge_Press_fd_PMP_PEAKTARRIF_C</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CEL_5003_PRESSURE_R</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>COPPER:1428:HL4-PRESSFINWO.Process Value</td>
      <td>7356320</td>
    </tr>
    <tr>
      <th>3</th>
      <td>COPPER:197:CS1-RACSPUMP1.Pressure</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>COPPER:205:CS1-RACSPUMP2.Pressure</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9213</th>
      <td>ZZORTB_NIGHT_PRESSURE</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9214</th>
      <td>ZZRFTW_DAY_PRESSURE</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9215</th>
      <td>ZZRFTW_NIGHT_PRESSURE</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9216</th>
      <td>ZZUTNY_DAY_PRESSURE</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9217</th>
      <td>ZZUTNY_NIGHT_PRESSURE</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>9218 rows × 2 columns</p>
</div>




```python
match_PiGisId_Height = pd.merge(gisIdMatch,PI_GISID, on = 'GISID').rename(columns = {'PI Data' : 'Meters'})
match_PiGisId_Height['Meters'] = match_PiGisId_Height['Meters'].str.replace('_PRESSURE_R','').str.replace('~PRESSURE','')
match_PiGisId_Height['Meters'] = match_PiGisId_Height['Meters'].str.replace('SOM_','')
match_PiGisId_Height
### Pressure DM ###
match_pressureDataDM = pd.merge(pressure, match_PiGisId_Height, on = 'Meters')
### Pressure SOM ###
match_pressureDataSOM_0 = match_PiGisId_Height[match_PiGisId_Height["Meters"].str.contains("DM")==False]
match_pressureDataSOM_0['Meters_Nbr_Int'] = match_pressureDataSOM_0['Meters'].astype(int)
match_pressureDataSOM_1 = match_pressureDataSOM_0.drop(columns=['Meters']).rename(columns = {'Meters_Nbr_Int' : 'Meters'})
match_pressureDataSOM = pd.merge(pressureSOM, match_pressureDataSOM_1, on = 'Meters')
### Concat Pressure ###
match_pressure = pd.concat([match_pressureDataDM, match_pressureDataSOM], ignore_index=True, sort=False)

match_pressure['Pressure+Height'] = match_pressure['Pressure'] + match_pressure['HEIGHT']

match_DMApressure = match_pressure.groupby(['DMA_Set', pd.Grouper(key='Date', freq='365D')]).mean().reset_index()
match_DMApressure.head(50)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Date</th>
      <th>GISID</th>
      <th>HEIGHT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZANGLB01</td>
      <td>2019-04-01</td>
      <td>1.796354e+06</td>
      <td>113.190000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ZANGLB01</td>
      <td>2020-03-31</td>
      <td>1.796354e+06</td>
      <td>113.190000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ZANGLB01</td>
      <td>2021-03-31</td>
      <td>1.796354e+06</td>
      <td>113.190000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ZBEAHL06</td>
      <td>2019-04-01</td>
      <td>1.796220e+06</td>
      <td>58.360000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ZBEAHL06</td>
      <td>2020-03-31</td>
      <td>1.796220e+06</td>
      <td>58.360000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ZBEAHL06</td>
      <td>2021-03-31</td>
      <td>1.796220e+06</td>
      <td>58.360000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>ZBLACK02</td>
      <td>2019-04-01</td>
      <td>1.789866e+06</td>
      <td>197.030000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ZBLACK02</td>
      <td>2020-03-31</td>
      <td>1.789866e+06</td>
      <td>197.030000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ZBLACK02</td>
      <td>2021-03-31</td>
      <td>1.789866e+06</td>
      <td>197.030000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ZBLUNS25</td>
      <td>2019-04-01</td>
      <td>1.789372e+06</td>
      <td>108.070000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>ZBLUNS25</td>
      <td>2020-03-31</td>
      <td>1.789372e+06</td>
      <td>108.070000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ZBLUNS25</td>
      <td>2021-03-31</td>
      <td>1.789372e+06</td>
      <td>108.070000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>ZBOWDN03</td>
      <td>2019-04-01</td>
      <td>1.796076e+06</td>
      <td>65.000000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>ZBOWDN03</td>
      <td>2020-03-31</td>
      <td>1.796076e+06</td>
      <td>65.000000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>ZBOWDN03</td>
      <td>2021-03-31</td>
      <td>1.796076e+06</td>
      <td>65.000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>ZBRETH08</td>
      <td>2019-04-01</td>
      <td>1.789146e+06</td>
      <td>146.670000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>ZBRETH08</td>
      <td>2020-03-31</td>
      <td>1.789146e+06</td>
      <td>146.670000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>ZBRETH08</td>
      <td>2021-03-31</td>
      <td>1.789146e+06</td>
      <td>146.670000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>ZCHIGW70</td>
      <td>2019-04-01</td>
      <td>5.134268e+06</td>
      <td>43.730000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>ZCHIGW70</td>
      <td>2020-03-31</td>
      <td>5.134268e+06</td>
      <td>43.730000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>ZCHIGW70</td>
      <td>2021-03-31</td>
      <td>5.134268e+06</td>
      <td>43.730000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>ZCIREN04</td>
      <td>2019-04-01</td>
      <td>1.796387e+06</td>
      <td>84.350000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>ZCIREN04</td>
      <td>2020-03-31</td>
      <td>1.796387e+06</td>
      <td>84.350000</td>
    </tr>
    <tr>
      <th>23</th>
      <td>ZCIREN04</td>
      <td>2021-03-31</td>
      <td>1.796387e+06</td>
      <td>84.350000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>ZCOLDA04</td>
      <td>2019-04-01</td>
      <td>1.795767e+06</td>
      <td>76.700000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>ZCOLDA04</td>
      <td>2020-03-31</td>
      <td>1.795767e+06</td>
      <td>76.700000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>ZCOLDA04</td>
      <td>2021-03-31</td>
      <td>1.795767e+06</td>
      <td>76.700000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>ZELTHM26</td>
      <td>2019-04-01</td>
      <td>4.671219e+06</td>
      <td>4.445000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>ZELTHM26</td>
      <td>2020-03-31</td>
      <td>4.671219e+06</td>
      <td>4.445000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>ZELTHM26</td>
      <td>2021-03-31</td>
      <td>4.671219e+06</td>
      <td>4.445000</td>
    </tr>
    <tr>
      <th>30</th>
      <td>ZFARDN03</td>
      <td>2019-04-01</td>
      <td>1.796494e+06</td>
      <td>131.210000</td>
    </tr>
    <tr>
      <th>31</th>
      <td>ZFARDN03</td>
      <td>2020-03-31</td>
      <td>1.796494e+06</td>
      <td>131.210000</td>
    </tr>
    <tr>
      <th>32</th>
      <td>ZFARDN03</td>
      <td>2021-03-31</td>
      <td>1.796494e+06</td>
      <td>131.210000</td>
    </tr>
    <tr>
      <th>33</th>
      <td>ZFLAXL04</td>
      <td>2019-04-01</td>
      <td>1.795065e+06</td>
      <td>83.310000</td>
    </tr>
    <tr>
      <th>34</th>
      <td>ZFLAXL04</td>
      <td>2020-03-31</td>
      <td>1.795065e+06</td>
      <td>83.310000</td>
    </tr>
    <tr>
      <th>35</th>
      <td>ZFLAXL04</td>
      <td>2021-03-31</td>
      <td>1.795065e+06</td>
      <td>83.310000</td>
    </tr>
    <tr>
      <th>36</th>
      <td>ZFRNBO15</td>
      <td>2019-04-01</td>
      <td>1.791892e+06</td>
      <td>66.226667</td>
    </tr>
    <tr>
      <th>37</th>
      <td>ZFRNBO15</td>
      <td>2020-03-31</td>
      <td>1.791892e+06</td>
      <td>66.226667</td>
    </tr>
    <tr>
      <th>38</th>
      <td>ZFRNBO15</td>
      <td>2021-03-31</td>
      <td>1.791892e+06</td>
      <td>66.226667</td>
    </tr>
    <tr>
      <th>39</th>
      <td>ZHACKP04</td>
      <td>2019-04-01</td>
      <td>1.795772e+06</td>
      <td>140.920000</td>
    </tr>
    <tr>
      <th>40</th>
      <td>ZHACKP04</td>
      <td>2020-03-31</td>
      <td>1.795772e+06</td>
      <td>140.920000</td>
    </tr>
    <tr>
      <th>41</th>
      <td>ZHACKP04</td>
      <td>2021-03-31</td>
      <td>1.795772e+06</td>
      <td>140.920000</td>
    </tr>
    <tr>
      <th>42</th>
      <td>ZPEWLY01</td>
      <td>2019-04-01</td>
      <td>1.789751e+06</td>
      <td>44.350000</td>
    </tr>
    <tr>
      <th>43</th>
      <td>ZPEWLY01</td>
      <td>2020-03-31</td>
      <td>1.789751e+06</td>
      <td>44.350000</td>
    </tr>
    <tr>
      <th>44</th>
      <td>ZPEWLY01</td>
      <td>2021-03-31</td>
      <td>1.789751e+06</td>
      <td>44.350000</td>
    </tr>
    <tr>
      <th>45</th>
      <td>ZSEWRD50</td>
      <td>2019-04-01</td>
      <td>1.791073e+06</td>
      <td>23.200000</td>
    </tr>
    <tr>
      <th>46</th>
      <td>ZSEWRD50</td>
      <td>2020-03-31</td>
      <td>1.791073e+06</td>
      <td>23.200000</td>
    </tr>
    <tr>
      <th>47</th>
      <td>ZSEWRD50</td>
      <td>2021-03-31</td>
      <td>1.791073e+06</td>
      <td>23.200000</td>
    </tr>
    <tr>
      <th>48</th>
      <td>ZSTHFL08</td>
      <td>2019-04-01</td>
      <td>1.791556e+06</td>
      <td>54.880000</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ZSTHFL08</td>
      <td>2020-03-31</td>
      <td>1.791556e+06</td>
      <td>54.880000</td>
    </tr>
  </tbody>
</table>
</div>



## Concat GIS ID Match with the full data


```python
pressureDMA_Level_Full = pd.concat([pressureDMA_Level, match_DMApressure], ignore_index=True, sort=False).drop(columns = ['GISID'])
pressureDMA_Level_Full
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Date</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
      <th>Pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZWIDDN24</td>
      <td>2021-04-30</td>
      <td>171.000000</td>
      <td>199.728171</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ZWIDDN24</td>
      <td>2022-04-30</td>
      <td>171.000000</td>
      <td>199.817577</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ZSTRMR07</td>
      <td>2019-04-30</td>
      <td>14.270000</td>
      <td>65.081635</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ZSTRMR07</td>
      <td>2020-04-30</td>
      <td>14.270000</td>
      <td>67.357334</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ZSTRMR07</td>
      <td>2021-04-30</td>
      <td>14.270000</td>
      <td>60.468982</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6752</th>
      <td>ZSUR3021</td>
      <td>2022-04-30</td>
      <td>8.712857</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6753</th>
      <td>ZWOODF11</td>
      <td>2019-04-30</td>
      <td>13.680000</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6754</th>
      <td>ZWOODF11</td>
      <td>2020-04-30</td>
      <td>13.680000</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6755</th>
      <td>ZWOODF11</td>
      <td>2021-04-30</td>
      <td>13.680000</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6756</th>
      <td>ZWOODF11</td>
      <td>2022-04-30</td>
      <td>13.680000</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>6757 rows × 5 columns</p>
</div>



### Check data again


```python
# diff = pd.merge(oldDataPressure,pressureDMA_Level_Full, on=['DMA_Set','Date'])
# diff['Diff'] = diff['avgHead'] - diff['Pressure+Height']
# diff.head()
```


```python
notMatchDMAs_final = dmaList.merge(pressureDMA_Level_Full,indicator = True, how='left').loc[lambda x : x['_merge']!='both']
notMatchDMAs_final.dropna(subset = ['DMA_Set']).drop_duplicates(subset = ['DMA_Set'])
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DMA_Set</th>
      <th>Date</th>
      <th>HEIGHT</th>
      <th>Pressure+Height</th>
      <th>Pressure</th>
      <th>_merge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1200</th>
      <td>ZHURTW07</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>1982</th>
      <td>ZBLOCK01</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>2306</th>
      <td>ZCIREN05</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3090</th>
      <td>ZHOGSB03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3230</th>
      <td>ZCIREN03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3726</th>
      <td>ZPENHL05</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>3945</th>
      <td>ZRAPSG02</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>5849</th>
      <td>ZARDLY04</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>5971</th>
      <td>ZMILCM03</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>6631</th>
      <td>ZNEWFM08</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <th>6760</th>
      <td>ZHOGSB02</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
  </tbody>
</table>
</div>




```python
writer = pd.ExcelWriter(folder + 'pressureFULL_NEW_GIS_ID_Yrl.xlsx')
pressureDMA_Level.to_excel(writer, sheet_name = 'pressureDMALevel')
notMatchDMAs_final.to_excel(writer, sheet_name= 'unmatch')
writer.save()
```
</details>
