---
layout: post
title: Examination of xG for forecasting
feature-img: "assets/img/portfolio/smotheredpitch.png"
img: "assets/img/portfolio/smotheredpitch.png"
date: April 2023
---

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
from ballab.dfl.team_name_dicts import team_names_dict_pref
```


```python
df_matches =  pd.read_csv('X:/sciebo/local/data/understat/xg_matches_long.csv')
df_matches = df_matches[['matchid', 'league', 'season', 'team', 'opp_team', 'goals', 'opp_goals', 'xG', 'opp_xG', 'date']]
```


```python
df_matches['date'] = pd.to_datetime(df_matches.date)
```


```python
df_matches['gdiff'] = df_matches.goals - df_matches.opp_goals
df_matches['xgdiff'] = df_matches.xG - df_matches.opp_xG
```


```python
df_matches.sort_values('date', inplace=True)
```


```python
df_matches['dummy'] = 1
```


```python
df_matches['no_matches'] = df_matches.groupby(['season', 'team']).dummy.transform('cumsum')

df_matches['cum_xgdiff'] = df_matches.groupby(['season', 'team']).xgdiff.transform('cumsum')
df_matches['cum_gdiff'] = df_matches.groupby(['season', 'team']).gdiff.transform('cumsum')
```


```python
df22 = df_matches[(df_matches.season == "2022/2023")].copy()
#df_matches.no_matches <= 34)].copy()
df22.sort_values(['no_matches', 'team'], inplace=True)
```


```python
df22
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
      <th>matchid</th>
      <th>league</th>
      <th>season</th>
      <th>team</th>
      <th>opp_team</th>
      <th>goals</th>
      <th>opp_goals</th>
      <th>xG</th>
      <th>opp_xG</th>
      <th>date</th>
      <th>gdiff</th>
      <th>xgdiff</th>
      <th>dummy</th>
      <th>no_matches</th>
      <th>cum_xgdiff</th>
      <th>cum_gdiff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30319</th>
      <td>eng_CRY_ARS_2223</td>
      <td>EPL</td>
      <td>2022/2023</td>
      <td>eng_ARS</td>
      <td>eng_CRY</td>
      <td>2</td>
      <td>0</td>
      <td>1.436010</td>
      <td>1.206370</td>
      <td>2022-08-05 19:00:00</td>
      <td>2</td>
      <td>0.229640</td>
      <td>1</td>
      <td>1</td>
      <td>0.229640</td>
      <td>2</td>
    </tr>
    <tr>
      <th>30321</th>
      <td>eng_BOU_AVA_2223</td>
      <td>EPL</td>
      <td>2022/2023</td>
      <td>eng_AVA</td>
      <td>eng_BOU</td>
      <td>0</td>
      <td>2</td>
      <td>0.488895</td>
      <td>0.588341</td>
      <td>2022-08-06 14:00:00</td>
      <td>-2</td>
      <td>-0.099446</td>
      <td>1</td>
      <td>1</td>
      <td>-0.099446</td>
      <td>-2</td>
    </tr>
    <tr>
      <th>14509</th>
      <td>eng_BOU_AVA_2223</td>
      <td>EPL</td>
      <td>2022/2023</td>
      <td>eng_BOU</td>
      <td>eng_AVA</td>
      <td>2</td>
      <td>0</td>
      <td>0.588341</td>
      <td>0.488895</td>
      <td>2022-08-06 14:00:00</td>
      <td>2</td>
      <td>0.099446</td>
      <td>1</td>
      <td>1</td>
      <td>0.099446</td>
      <td>2</td>
    </tr>
    <tr>
      <th>30327</th>
      <td>eng_LEI_BRE_2223</td>
      <td>EPL</td>
      <td>2022/2023</td>
      <td>eng_BRE</td>
      <td>eng_LEI</td>
      <td>2</td>
      <td>2</td>
      <td>0.931067</td>
      <td>0.455695</td>
      <td>2022-08-07 13:00:00</td>
      <td>0</td>
      <td>0.475372</td>
      <td>1</td>
      <td>1</td>
      <td>0.475372</td>
      <td>0</td>
    </tr>
    <tr>
      <th>30326</th>
      <td>eng_MUN_BRH_2223</td>
      <td>EPL</td>
      <td>2022/2023</td>
      <td>eng_BRH</td>
      <td>eng_MUN</td>
      <td>2</td>
      <td>1</td>
      <td>1.728900</td>
      <td>1.421030</td>
      <td>2022-08-07 13:00:00</td>
      <td>1</td>
      <td>0.307870</td>
      <td>1</td>
      <td>1</td>
      <td>0.307870</td>
      <td>1</td>
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
    </tr>
    <tr>
      <th>15560</th>
      <td>NaN</td>
      <td>Serie_A</td>
      <td>2022/2023</td>
      <td>NaN</td>
      <td>ita_SAM</td>
      <td>2</td>
      <td>2</td>
      <td>2.173960</td>
      <td>1.513100</td>
      <td>2023-02-06 19:45:00</td>
      <td>0</td>
      <td>0.660860</td>
      <td>1</td>
      <td>1675713600000000000</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>15545</th>
      <td>NaN</td>
      <td>Serie_A</td>
      <td>2022/2023</td>
      <td>NaN</td>
      <td>ita_INT</td>
      <td>1</td>
      <td>2</td>
      <td>0.723453</td>
      <td>2.375670</td>
      <td>2023-01-28 17:00:00</td>
      <td>-1</td>
      <td>-1.652217</td>
      <td>1</td>
      <td>1677337200000000000</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>31360</th>
      <td>ita_JUV__2223</td>
      <td>Serie_A</td>
      <td>2022/2023</td>
      <td>NaN</td>
      <td>ita_JUV</td>
      <td>2</td>
      <td>0</td>
      <td>1.373960</td>
      <td>1.377090</td>
      <td>2023-01-29 14:00:00</td>
      <td>2</td>
      <td>-0.003130</td>
      <td>1</td>
      <td>1678629600000000000</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>31393</th>
      <td>ita_TOR__2223</td>
      <td>Serie_A</td>
      <td>2022/2023</td>
      <td>NaN</td>
      <td>ita_TOR</td>
      <td>2</td>
      <td>2</td>
      <td>0.488613</td>
      <td>2.407610</td>
      <td>2023-02-20 19:45:00</td>
      <td>0</td>
      <td>-1.918997</td>
      <td>1</td>
      <td>1678631400000000000</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>31412</th>
      <td>ita_SAS__2223</td>
      <td>Serie_A</td>
      <td>2022/2023</td>
      <td>NaN</td>
      <td>ita_SAS</td>
      <td>2</td>
      <td>3</td>
      <td>1.258320</td>
      <td>1.048180</td>
      <td>2023-03-06 17:30:00</td>
      <td>-1</td>
      <td>0.210140</td>
      <td>1</td>
      <td>1679082300000000000</td>
      <td>NaN</td>
      <td>-1</td>
    </tr>
  </tbody>
</table>
<p>2610 rows × 16 columns</p>
</div>




```python
# m = 1
for m in np.arange(1,18):
    plt.scatter(df22[df22.no_matches == m].cum_xgdiff, df22[df22.no_matches == 25].cum_gdiff)
    plt.scatter(df22[df22.no_matches == m].cum_gdiff,  df22[df22.no_matches == 25].cum_gdiff)
    plt.axhline()
    plt.axvline()
    plt.show()
```


    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_0.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_1.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_2.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_3.png)
    



    
![png](xG_forecasting_analysis_files/![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_4.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_5.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_6.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_7.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_8.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_9.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_10.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_11.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_12.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_13.png)



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_14.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_15.png)
    



    
![png]({{ site.baseurl }}/assets/img/xG_forecasting_analysis_files/xG_forecasting_analysis_10_16.png)
    



```python
df2122[['season', 'league', 'no_matches', 'team', 'cum_xgdiff', 'cum_gdiff']]
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
      <th>season</th>
      <th>league</th>
      <th>no_matches</th>
      <th>team</th>
      <th>cum_xgdiff</th>
      <th>cum_gdiff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13596</th>
      <td>2021/2022</td>
      <td>Ligue_1</td>
      <td>1</td>
      <td>fra_AMO</td>
      <td>1.529275</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28103</th>
      <td>2021/2022</td>
      <td>Ligue_1</td>
      <td>1</td>
      <td>fra_NAN</td>
      <td>-1.529275</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28104</th>
      <td>2021/2022</td>
      <td>Ligue_1</td>
      <td>1</td>
      <td>fra_B29</td>
      <td>-0.155550</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13597</th>
      <td>2021/2022</td>
      <td>Ligue_1</td>
      <td>1</td>
      <td>fra_LYO</td>
      <td>0.155550</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13598</th>
      <td>2021/2022</td>
      <td>Ligue_1</td>
      <td>1</td>
      <td>fra_TRY</td>
      <td>-0.868337</td>
      <td>-1</td>
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
      <th>14370</th>
      <td>2021/2022</td>
      <td>Bundesliga</td>
      <td>34</td>
      <td>ger_WOB</td>
      <td>-4.202169</td>
      <td>-11</td>
    </tr>
    <tr>
      <th>28877</th>
      <td>2021/2022</td>
      <td>Bundesliga</td>
      <td>34</td>
      <td>ger_FCB</td>
      <td>61.295887</td>
      <td>60</td>
    </tr>
    <tr>
      <th>28878</th>
      <td>2021/2022</td>
      <td>Bundesliga</td>
      <td>34</td>
      <td>ger_BSC</td>
      <td>-20.896175</td>
      <td>-34</td>
    </tr>
    <tr>
      <th>14363</th>
      <td>2021/2022</td>
      <td>Bundesliga</td>
      <td>34</td>
      <td>ger_DSC</td>
      <td>-42.441165</td>
      <td>-26</td>
    </tr>
    <tr>
      <th>14368</th>
      <td>2021/2022</td>
      <td>Bundesliga</td>
      <td>34</td>
      <td>ger_FCU</td>
      <td>15.376970</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
<p>3332 rows × 6 columns</p>
</div>




```python
ls_cor_xd = []
ls_cor_rd = []
for i in range(34):
    ls_cor_xd.append(np.corrcoef(df22[df22.no_matches == i+1].cum_xgdiff, df22[df22.no_matches == 34].cum_gdiff)[0,1])
    ls_cor_rd.append(np.corrcoef(df22[df22.no_matches == i+1].cum_gdiff, df22[df22.no_matches == 34].cum_gdiff)[0,1])
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-15-a9f2e4609716> in <module>
          2 ls_cor_rd = []
          3 for i in range(34):
    ----> 4     ls_cor_xd.append(np.corrcoef(df22[df22.no_matches == i+1].cum_xgdiff, df22[df22.no_matches == 34].cum_gdiff)[0,1])
          5     ls_cor_rd.append(np.corrcoef(df22[df22.no_matches == i+1].cum_gdiff, df22[df22.no_matches == 34].cum_gdiff)[0,1])


    <__array_function__ internals> in corrcoef(*args, **kwargs)


    x:\sciebo\local\coding\ballab_venv_win\lib\site-packages\numpy\lib\function_base.py in corrcoef(x, y, rowvar, bias, ddof, dtype)
       2632         warnings.warn('bias and ddof have no effect and are deprecated',
       2633                       DeprecationWarning, stacklevel=3)
    -> 2634     c = cov(x, y, rowvar, dtype=dtype)
       2635     try:
       2636         d = diag(c)


    <__array_function__ internals> in cov(*args, **kwargs)


    x:\sciebo\local\coding\ballab_venv_win\lib\site-packages\numpy\lib\function_base.py in cov(m, y, rowvar, bias, ddof, fweights, aweights, dtype)
       2426         if not rowvar and y.shape[0] != 1:
       2427             y = y.T
    -> 2428         X = np.concatenate((X, y), axis=0)
       2429 
       2430     if ddof is None:


    <__array_function__ internals> in concatenate(*args, **kwargs)


    ValueError: all the input array dimensions for the concatenation axis must match exactly, but along dimension 1, the array at index 0 has size 96 and the array at index 1 has size 0



```python
pd.DataFrame({'xGDiff' : ls_cor_xd,
              'GDiff'  : ls_cor_rd}).plot()
```




    <AxesSubplot:>




    
![png](xG_forecasting_analysis_files/xG_forecasting_analysis_13_1.png)
    



```python

```


```python

```
