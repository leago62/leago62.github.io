---
layout: post
title:  "Alzheimer's Caregiver Data Exploration"
date:   2019-10-17
permalink: /projects/Alz_Caregiver_Exp
---

## Project Overview:

I thought that this project would be a good exercise in getting data from an
online source and then exploring it on my own using pandas. This
topic choice extends the Caregiver Collective project that we were working on
by looking deeper at the population of caregivers aged 50+.

Goals: My goal is to be able to understand the organization of this dataset and
what information it conveys. After this first step, I hope to be able to learn a
 little more about the people who are caregivers.

Challenges: One of my biggest challenges with this data set was figuring out
how the data was organized. The documentation was unclear about how the numbers
were generated and gave no indication of sample sizes for the percentages.

This is the link to the original dataset: [Original Dataset](https://chronicdata.cdc.gov/Healthy-Aging/Alzheimer-s-Disease-and-Healthy-Aging-Data/hfr9-rurv)

The following is the code I used.


# Race and Gender Data Exploration

## Data Cleaning Process

Take only the data that is stratified by either race or gender (those with labels in 'Stratification2'). Remove any null rows.


```python
# Set up
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

AD = pd.read_csv('Alzheimer_s_Disease_and_Healthy_Aging_Data.csv')

AD = AD[AD['Class'] == 'Caregiving']

Cdf_ID2 = AD[AD['Stratification2'].notnull()]
Cdf_ID2 = Cdf_ID2[['YearStart', 'YearEnd', 'LocationAbbr', 'Topic', 'Question','Data_Value_Unit', 'Data_Value', 'Data_Value_Footnote','Low_Confidence_Limit', 'High_Confidence_Limit', 'Stratification1','StratificationID1','Stratification2','StratificationID2']]
Cdf_ID2 = Cdf_ID2.dropna(subset=['Data_Value'])

print(Cdf_ID2.shape)
print(Cdf_ID2['Stratification2'].value_counts())
Cdf_ID2.describe()
```

    (537, 14)
    Male                        80
    Black, non-Hispanic         80
    White, non-Hispanic         80
    Female                      80
    Hispanic                    78
    Native Am/Alaskan Native    72
    Asian/Pacific Islander      67
    Name: Stratification2, dtype: int64





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YearStart</th>
      <th>YearEnd</th>
      <th>Data_Value</th>
      <th>Low_Confidence_Limit</th>
      <th>High_Confidence_Limit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>537.000000</td>
      <td>537.000000</td>
      <td>537.000000</td>
      <td>537.000000</td>
      <td>537.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2015.780261</td>
      <td>2016.227188</td>
      <td>32.168901</td>
      <td>24.758473</td>
      <td>40.595717</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.833164</td>
      <td>0.831160</td>
      <td>23.034737</td>
      <td>20.262124</td>
      <td>25.280843</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2015.000000</td>
      <td>2015.000000</td>
      <td>0.700000</td>
      <td>0.200000</td>
      <td>2.300000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2015.000000</td>
      <td>2015.000000</td>
      <td>15.100000</td>
      <td>10.100000</td>
      <td>21.300000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2016.000000</td>
      <td>2016.000000</td>
      <td>23.400000</td>
      <td>18.400000</td>
      <td>30.700000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2017.000000</td>
      <td>2017.000000</td>
      <td>42.200000</td>
      <td>29.400000</td>
      <td>61.600000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2017.000000</td>
      <td>2017.000000</td>
      <td>89.800000</td>
      <td>75.500000</td>
      <td>97.800000</td>
    </tr>
  </tbody>
</table>
</div>




```python
Cdf_ID2.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YearStart</th>
      <th>YearEnd</th>
      <th>LocationAbbr</th>
      <th>Topic</th>
      <th>Question</th>
      <th>Data_Value_Unit</th>
      <th>Data_Value</th>
      <th>Data_Value_Footnote</th>
      <th>Low_Confidence_Limit</th>
      <th>High_Confidence_Limit</th>
      <th>Stratification1</th>
      <th>StratificationID1</th>
      <th>Stratification2</th>
      <th>StratificationID2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26471</th>
      <td>2015</td>
      <td>2015</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>24.5</td>
      <td>Fewer than 50 States reporting</td>
      <td>22.4</td>
      <td>26.8</td>
      <td>50-54 years</td>
      <td>5054</td>
      <td>Male</td>
      <td>MALE</td>
    </tr>
    <tr>
      <th>26531</th>
      <td>2015</td>
      <td>2015</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>22.4</td>
      <td>Fewer than 50 States reporting</td>
      <td>20.6</td>
      <td>24.3</td>
      <td>55-59 years</td>
      <td>5559</td>
      <td>Male</td>
      <td>MALE</td>
    </tr>
    <tr>
      <th>26591</th>
      <td>2015</td>
      <td>2015</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>19.4</td>
      <td>Fewer than 50 States reporting</td>
      <td>17.8</td>
      <td>21.1</td>
      <td>60-64 years</td>
      <td>6064</td>
      <td>Male</td>
      <td>MALE</td>
    </tr>
    <tr>
      <th>26651</th>
      <td>2015</td>
      <td>2015</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>17.6</td>
      <td>Fewer than 50 States reporting</td>
      <td>16.6</td>
      <td>18.6</td>
      <td>65 years or older</td>
      <td>65PLUS</td>
      <td>Male</td>
      <td>MALE</td>
    </tr>
    <tr>
      <th>26652</th>
      <td>2015</td>
      <td>2015</td>
      <td>US</td>
      <td>Expect to provide care for someone in the next...</td>
      <td>Percentage of older adults currently not provi...</td>
      <td>%</td>
      <td>20.5</td>
      <td>Fewer than 50 States reporting</td>
      <td>18.1</td>
      <td>23.0</td>
      <td>50-54 years</td>
      <td>5054</td>
      <td>Male</td>
      <td>MALE</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(Cdf_ID2.Topic.unique())
print(Cdf_ID2.Question.unique())
```

    ['Provide care for a friend or family member in past month'
     'Expect to provide care for someone in the next two years'
     'Duration of caregiving among older adults'
     'Intensity of caregiving among older adults'
     'Provide care for someone with cognitive impairment within the past month']
    [ 'Percentage of older adults who provided care for a friend or family member within the past month'
     'Percentage of older adults currently not providing care who expect to provide care for someone with health problems in the next two years'
     'Percentage of older adults who provided care to a friend or family member for six months or more'
     'Average of 20 or more hours of care per week provided to a friend or family member'
     'Percentage of older adults who provided care for someone with dementia or other cognitive impairment within the past month']


## Analysis on the basis of Gender


```python
Cdf_G = Cdf_ID2[(Cdf_ID2.Stratification2 == 'Male') | (Cdf_ID2.Stratification2 == 'Female')]
print(Cdf_G.shape)
print(Cdf_G['Stratification2'].value_counts())
print(Cdf_G['Topic'].value_counts())
```

    (160, 14)
    Male      80
    Female    80
    Name: Stratification2, dtype: int64
    Expect to provide care for someone in the next two years                    32
    Duration of caregiving among older adults                                   32
    Intensity of caregiving among older adults                                  32
    Provide care for a friend or family member in past month                    32
    Provide care for someone with cognitive impairment within the past month    32
    Name: Topic, dtype: int64



```python
Cdf_G17 = Cdf_G[Cdf_G['YearStart']==2017]
```


```python
Cdf_G17['Low_Err'] = Cdf_G17['Data_Value']-Cdf_G17['Low_Confidence_Limit']
Cdf_G17['High_Err'] = Cdf_G17['High_Confidence_Limit']-Cdf_G17['Data_Value']
Cdf_G17.head()
```

    C:\Users\sugar\Anaconda3\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    C:\Users\sugar\Anaconda3\lib\site-packages\ipykernel\__main__.py:2: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      from ipykernel import kernelapp as app





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YearStart</th>
      <th>YearEnd</th>
      <th>LocationAbbr</th>
      <th>Topic</th>
      <th>Question</th>
      <th>Data_Value_Unit</th>
      <th>Data_Value</th>
      <th>Data_Value_Footnote</th>
      <th>Low_Confidence_Limit</th>
      <th>High_Confidence_Limit</th>
      <th>Stratification1</th>
      <th>StratificationID1</th>
      <th>Stratification2</th>
      <th>StratificationID2</th>
      <th>Low_Err</th>
      <th>High_Err</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>41944</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>21.9</td>
      <td>Fewer than 50 States reporting</td>
      <td>19.0</td>
      <td>25.1</td>
      <td>50-54 years</td>
      <td>5054</td>
      <td>Male</td>
      <td>MALE</td>
      <td>2.9</td>
      <td>3.2</td>
    </tr>
    <tr>
      <th>41945</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>22.2</td>
      <td>Fewer than 50 States reporting</td>
      <td>19.3</td>
      <td>25.5</td>
      <td>55-59 years</td>
      <td>5559</td>
      <td>Male</td>
      <td>MALE</td>
      <td>2.9</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>41946</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>19.4</td>
      <td>Fewer than 50 States reporting</td>
      <td>16.9</td>
      <td>22.2</td>
      <td>60-64 years</td>
      <td>6064</td>
      <td>Male</td>
      <td>MALE</td>
      <td>2.5</td>
      <td>2.8</td>
    </tr>
    <tr>
      <th>41947</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>17.9</td>
      <td>Fewer than 50 States reporting</td>
      <td>16.4</td>
      <td>19.5</td>
      <td>65 years or older</td>
      <td>65PLUS</td>
      <td>Male</td>
      <td>MALE</td>
      <td>1.5</td>
      <td>1.6</td>
    </tr>
    <tr>
      <th>41948</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Expect to provide care for someone in the next...</td>
      <td>Percentage of older adults currently not provi...</td>
      <td>%</td>
      <td>19.1</td>
      <td>Fewer than 50 States reporting</td>
      <td>15.5</td>
      <td>23.3</td>
      <td>50-54 years</td>
      <td>5054</td>
      <td>Male</td>
      <td>MALE</td>
      <td>3.6</td>
      <td>4.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
def compare_gender_by_topic(Cdf_G17_topic):
    male = Cdf_G17_topic[Cdf_G17_topic['Stratification2']=='Male']
    female = Cdf_G17_topic[Cdf_G17_topic['Stratification2']=='Female']

    barWidth = 0.4
    fig,ax = plt.subplots(figsize=(8,5))

    barsM = male['Data_Value']
    barsF = female['Data_Value']
    errM = [np.transpose(male['Low_Err'].values),np.transpose(male['High_Err'].values)]
    errF = [np.transpose(female['Low_Err'].values),np.transpose(female['High_Err'].values)]

    pos = list(range(len(male['Data_Value'])))

    plt.bar(pos,barsM,barWidth,color='blue',alpha=0.5,label='Male')
    plt.bar([p+barWidth for p in pos],barsF,width=barWidth,color='red', alpha=0.5,label="Female")

    plt.errorbar([p+0.5*barWidth for p in pos], barsM, yerr=errM, fmt='.k',label='_ nolegend_')
    plt.errorbar([p+1.5*barWidth for p in pos], barsF, yerr=errF, fmt='.k',label='_nolegend_')

    title = 'Comparison of Females and Male by Age, Topic: ' + male.Topic.unique()
    ax.set_title(title[0])
    ax.set_xlabel('Age')
    ax.set_ylabel('Percent')
    ax.set_xticks([r+1*barWidth for r in range(len(barsF))])
    ax.set_xticklabels(male['StratificationID1'])

    plt.xlim(min(pos)-barWidth, max(pos)+barWidth*3)
    plt.ylim([0,100])
    plt.xticks(rotation=45, ha='right')

    plt.legend(loc='upper left')
    plt.grid()
    plt.show()
```


```python
compare_gender_by_topic(Cdf_G17[Cdf_G17['Topic'] == 'Provide care for a friend or family member in past month'])
```


![png](/projects/images/output_10_0.png)



```python
compare_gender_by_topic(Cdf_G17[Cdf_G17['Topic'] == 'Duration of caregiving among older adults'])
```


![png](/projects/images/output_11_0.png)



```python
compare_gender_by_topic(Cdf_G17[Cdf_G17['Topic'] == 'Expect to provide care for someone in the next two years'])
```


![png](/projects/images/output_12_0.png)



```python
compare_gender_by_topic(Cdf_G17[Cdf_G17['Topic'] == 'Intensity of caregiving among older adults'])
```


![png](/projects/images/output_13_0.png)



```python
compare_gender_by_topic(Cdf_G17[Cdf_G17['Topic'] == 'Provide care for someone with cognitive impairment within the past month'])
```


![png](/projects/images/output_14_0.png)


#### Initial Observations/Thoughts

In most cases, there does not appear to be a statistical difference between male and female caregivers at any age group. Generally, more females tend to be associated with caregiving than their male counterparts, but it is also important to note that this data may not be representative of the whole US considering less than half of the states are included in the data.

It is also interesting to note that the topic 'Duration of caregiving among older adults' reported much higher numbers than any other topic in this section, especially in comparison with those in the 'Provide care for a friend or family member in past month'. This infers that although most older people may not be currently or recently giving care, the majority of them will do so for a 6+ month duration during their lives. This information has implications on how to design products that cater to these people. It is also interesting to note that these numbers do not appear to increase with age.

## Analysis on the basis of Race


```python
Cdf_R = Cdf_ID2.loc[Cdf_ID2.index.difference(Cdf_G.index)]
print(Cdf_R.shape)
print(Cdf_R['Stratification2'].value_counts())
print(Cdf_R['Topic'].value_counts())
```

    (377, 14)
    Black, non-Hispanic         80
    White, non-Hispanic         80
    Hispanic                    78
    Native Am/Alaskan Native    72
    Asian/Pacific Islander      67
    Name: Stratification2, dtype: int64
    Duration of caregiving among older adults                                   79
    Expect to provide care for someone in the next two years                    78
    Provide care for a friend or family member in past month                    77
    Intensity of caregiving among older adults                                  76
    Provide care for someone with cognitive impairment within the past month    67
    Name: Topic, dtype: int64



```python
print(Cdf_R['StratificationID2'].unique())
```

    ['WHT' 'BLK' 'HIS' 'ASN' 'NAA']



```python
Cdf_R17 = Cdf_R[Cdf_R['YearStart']==2017]
```


```python
Cdf_R17['Low_Err'] = Cdf_R17['Data_Value']-Cdf_R17['Low_Confidence_Limit']
Cdf_R17['High_Err'] = Cdf_R17['High_Confidence_Limit']-Cdf_R17['Data_Value']
Cdf_R17.head()
```

    C:\Users\sugar\Anaconda3\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    C:\Users\sugar\Anaconda3\lib\site-packages\ipykernel\__main__.py:2: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      from ipykernel import kernelapp as app





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YearStart</th>
      <th>YearEnd</th>
      <th>LocationAbbr</th>
      <th>Topic</th>
      <th>Question</th>
      <th>Data_Value_Unit</th>
      <th>Data_Value</th>
      <th>Data_Value_Footnote</th>
      <th>Low_Confidence_Limit</th>
      <th>High_Confidence_Limit</th>
      <th>Stratification1</th>
      <th>StratificationID1</th>
      <th>Stratification2</th>
      <th>StratificationID2</th>
      <th>Low_Err</th>
      <th>High_Err</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>42455</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>25.5</td>
      <td>Fewer than 50 States reporting</td>
      <td>23.0</td>
      <td>28.1</td>
      <td>50-54 years</td>
      <td>5054</td>
      <td>White, non-Hispanic</td>
      <td>WHT</td>
      <td>2.5</td>
      <td>2.6</td>
    </tr>
    <tr>
      <th>42515</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>26.9</td>
      <td>Fewer than 50 States reporting</td>
      <td>24.5</td>
      <td>29.4</td>
      <td>55-59 years</td>
      <td>5559</td>
      <td>White, non-Hispanic</td>
      <td>WHT</td>
      <td>2.4</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>42575</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>25.6</td>
      <td>Fewer than 50 States reporting</td>
      <td>23.4</td>
      <td>27.9</td>
      <td>60-64 years</td>
      <td>6064</td>
      <td>White, non-Hispanic</td>
      <td>WHT</td>
      <td>2.2</td>
      <td>2.3</td>
    </tr>
    <tr>
      <th>42635</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Provide care for a friend or family member in ...</td>
      <td>Percentage of older adults who provided care f...</td>
      <td>%</td>
      <td>20.5</td>
      <td>Fewer than 50 States reporting</td>
      <td>19.4</td>
      <td>21.7</td>
      <td>65 years or older</td>
      <td>65PLUS</td>
      <td>White, non-Hispanic</td>
      <td>WHT</td>
      <td>1.1</td>
      <td>1.2</td>
    </tr>
    <tr>
      <th>42636</th>
      <td>2017</td>
      <td>2017</td>
      <td>US</td>
      <td>Expect to provide care for someone in the next...</td>
      <td>Percentage of older adults currently not provi...</td>
      <td>%</td>
      <td>19.6</td>
      <td>Fewer than 50 States reporting</td>
      <td>16.8</td>
      <td>22.7</td>
      <td>50-54 years</td>
      <td>5054</td>
      <td>White, non-Hispanic</td>
      <td>WHT</td>
      <td>2.8</td>
      <td>3.1</td>
    </tr>
  </tbody>
</table>
</div>




```python
def compare_race_by_topic(Cdf_R17_topic):
    wht = Cdf_R17_topic[Cdf_R17_topic['StratificationID2']=='WHT']
    blk = Cdf_R17_topic[Cdf_R17_topic['StratificationID2']=='BLK']
    his = Cdf_R17_topic[Cdf_R17_topic['StratificationID2']=='HIS']
    asn = Cdf_R17_topic[Cdf_R17_topic['StratificationID2']=='ASN']
    naa = Cdf_R17_topic[Cdf_R17_topic['StratificationID2']=='NAA']


    barWidth = 0.15
    fig,ax = plt.subplots(figsize=(8,5))

    barsW = wht['Data_Value']
    barsB = blk['Data_Value']
    barsH = his['Data_Value']
    barsA = asn['Data_Value']
    barsN = naa['Data_Value']
    errW = [np.transpose(wht['Low_Err'].values),np.transpose(wht['High_Err'].values)]
    errB = [np.transpose(blk['Low_Err'].values),np.transpose(blk['High_Err'].values)]
    errH = [np.transpose(his['Low_Err'].values),np.transpose(his['High_Err'].values)]
    errA = [np.transpose(asn['Low_Err'].values),np.transpose(asn['High_Err'].values)]
    errN = [np.transpose(naa['Low_Err'].values),np.transpose(naa['High_Err'].values)]

    pos = list(range(len(asn['Data_Value'])))

    plt.bar(pos,barsA,barWidth,color='red',alpha=0.5,label='Asian/Pacific Islander')
    plt.bar([p+barWidth for p in pos],barsB,width=barWidth,color='yellow', alpha=0.5,label="Black, non-hispanic")
    plt.bar([p+2*barWidth for p in pos],barsH,width=barWidth,color='green', alpha=0.5,label="Hispanic")
    plt.bar([p+3*barWidth for p in pos],barsN,width=barWidth,color='blue', alpha=0.5,label="Native Am/Alaska Native")
    plt.bar([p+4*barWidth for p in pos],barsW,width=barWidth,color='purple', alpha=0.5,label="White, non-hispanic")

    plt.errorbar([p+0.5*barWidth for p in pos], barsA, yerr=errA, fmt='.k',label='_nolegend_')
    plt.errorbar([p+1.5*barWidth for p in pos], barsB, yerr=errB, fmt='.k',label='_nolegend_')
    plt.errorbar([p+2.5*barWidth for p in pos], barsH, yerr=errH, fmt='.k',label='_nolegend_')
    plt.errorbar([p+3.5*barWidth for p in pos], barsN, yerr=errN, fmt='.k',label='_nolegend_')
    plt.errorbar([p+4.5*barWidth for p in pos], barsW, yerr=errW, fmt='.k',label='_nolegend_')

    title = 'Comparison of Races by Age, Topic: ' + asn.Topic.unique()
    ax.set_title(title[0])
    ax.set_xlabel('Age')
    ax.set_ylabel('Percent')
    ax.set_xticks([r+2*barWidth for r in range(len(barsA))])
    ax.set_xticklabels(asn['StratificationID1'])

    plt.xlim(min(pos)-barWidth, max(pos)+barWidth*6)
    plt.ylim([0,155])
    plt.xticks(rotation=45, ha='right')

    plt.legend(loc='upper left')
    plt.grid()
    plt.show()
```


```python
compare_race_by_topic(Cdf_R17[Cdf_R17['Topic'] == 'Provide care for a friend or family member in past month'])
```


![png](/projects/images/output_22_0.png)



```python
compare_race_by_topic(Cdf_R17[Cdf_R17['Topic'] == 'Duration of caregiving among older adults'])
```


![png](/projects/images/output_23_0.png)



```python
compare_race_by_topic(Cdf_R17[Cdf_R17['Topic'] == 'Expect to provide care for someone in the next two years'])
```


![png](/projects/images/output_24_0.png)



```python
compare_race_by_topic(Cdf_R17[Cdf_R17['Topic'] == 'Intensity of caregiving among older adults'])
```


![png](/projects/images/output_25_0.png)



```python
compare_race_by_topic(Cdf_R17[Cdf_R17['Topic'] == 'Provide care for someone with cognitive impairment within the past month'])
```


![png](/projects/images/output_26_0.png)


#### Initial Observations/Thoughts

There doesn't appear to be strong patterns between the races across the five topics. The white, non-Hispanic group consistently has the smallest error margin which could indicate that the majority of the data came from this population and more data from other races may be necessary to accurately observe differences.

The range of data by topic seems similar to the gender analysis, however the topic 'intensity of caregiving among older adults' seems to have a much larger range which can largely be attributed to high numbers for Asians 50-54, and Native American/Alaska Native 60-64 and 65 plus.

In this data set, the race and gender categories cannot overlap, so you cannot look at gender divisions within the different races. However, the lower error bars and pattern of the gender data seem to suggest that the data collected under the gender division is largely contributed by the white, non-Hispanic population.
