---
layout: lesson
root: ../..
---

# Introduction  


<div>
<p>This example shows how to plot a composition for data that is changing over time when there are only a small number of time periods. Two plots will be created. - <strong>A Stacked Column Chart to show relative and absolute differences.</strong> - <strong>A 100% Stacked Column Chart to show relative differences.</strong></p>
<p>The example uses yearly data on hospital acquired infections. Data from two hospitals is compared over 3 years.</p>
</div>


<div class="in">
<pre>%matplotlib inline
from matplotlib import pylab as plt
import numpy as np
import pandas as pd  
</pre>
</div>


<div>
<p>First we read the data using Pandas.</p>
</div>


<div class="in">
<pre>df = pd.read_csv(&#34;Hospital-Acquired_Infections__Beginning_2008.csv&#34;)</pre>
</div>


<div>
<p>Take a look at the columns to get a feel for the information in the dataset.</p>
</div>


<div class="in">
<pre>df.columns</pre>
</div>

<div class="out">
<pre>Index([u&#39;Facility Id&#39;, u&#39;Hospital Name&#39;, u&#39;Indicator Name&#39;, u&#39;Year&#39;, u&#39;Infections observed&#39;, u&#39;Infections predicted&#39;, u&#39;Denominator&#39;, u&#39;Indicator value&#39;, u&#39;Indicator Lower confidence limit&#39;, u&#39;Indicator Upper confidence limit&#39;, u&#39;Indicator units&#39;, u&#39;Comparison results&#39;, u&#39;Location 1&#39;], dtype=&#39;object&#39;)</pre>
</div>


<div>
<p>It is also useful to look at the head (first 5 rows) of the data in order to inspect how the data is organized.</p>
</div>


<div class="in">
<pre>df.head()</pre>
</div>

<div class="out">
<pre>   Facility Id                   Hospital Name  \
0            0  New York State - All Hospitals   
1            0  New York State - All Hospitals   
2            0  New York State - All Hospitals   
3            0  New York State - All Hospitals   
4            0  New York State - All Hospitals   

                             Indicator Name  Year  Infections observed  \
0  SSI Overall Standardized Infection Ratio  2012                 1618   
1                        CDI Hospital Onset  2012                 9904   
2                 CLABSI Cardiothoracic ICU  2012                   67   
3                       CLABSI Coronary ICU  2012                   60   
4                        CLABSI Medical ICU  2012                  130   

   Infections predicted  Denominator  Indicator value  \
0                   NaN          NaN             1.00   
1                   NaN     11948043             8.29   
2                   NaN        75757             0.88   
3                   NaN        48540             1.24   
4                   NaN       107618             1.21   

   Indicator Lower confidence limit  Indicator Upper confidence limit  \
0                               NaN                               NaN   
1                               NaN                               NaN   
2                               NaN                               NaN   
3                               NaN                               NaN   
4                               NaN                               NaN   

                                     Indicator units Comparison results  \
0                                                NaN                NaN   
1  # hospital onset cases per 10,000 patient days...                NaN   
2                        # CLABSI per 1000 line days                NaN   
3                        # CLABSI per 1000 line days                NaN   
4                        # CLABSI per 1000 line days                NaN   

  Location 1  
0        NaN  
1        NaN  
2        NaN  
3        NaN  
4        NaN  </pre>
</div>


<div>
<p>We have a column called 'Hospital Name'. It is useful to look at the unique values of this column in order to determine how many and which hospitals the data contains.</p>
</div>


<div class="in">
<pre>hospitalNames=pd.Series(df[&#39;Hospital Name&#39;].unique())
print hospitalNames</pre>
</div>

<div class="out">
<pre>0                        New York State - All Hospitals
1                        Albany Medical Center Hospital
2                              Albany Memorial Hospital
3                                    St Peters Hospital
4     Memorial Hosp of Wm F &amp; Gertrude F Jones A/K/A...
5             Our Lady of Lourdes Memorial Hospital Inc
6     United Health Services Hospitals Inc. - Wilson...
7                                Olean General Hospital
8                             Auburn Community Hospital
9                              Brooks Memorial Hospital
10                        Woman&#39;s Christian Association
11               TLC Health Network Lake Shore Hospital
12                           Arnot Ogden Medical Center
13                                  St Josephs Hospital
14                       Chenango Memorial Hospital Inc
...
167               Peninsula Hospital Center- Closed 2012
168                               Queens Hospital Center
169               St. Johns Queens Hospital- Closed 2009
170                 St Johns Episcopal Hospital So Shore
171         New York Hospital Medical Center   of Queens
172                                Forest Hills Hospital
173    Mount Sinai Hospital - Mount Sinai Hospital of...
174              Woodhull Medical &amp; Mental Health Center
175                   Richmond University Medical Center
176                  Staten Island University Hosp-North
177                  North General Hospital- Closed 2010
178          SVCMC Mary Immaculate Hospital- Closed 2009
179    Montefiore Med Center - Jack D Weiler Hosp of ...
180                   Millard Fillmore Suburban Hospital
181      New York Presbyterian Hospital - Allen Hospital
Length: 182, dtype: object
</pre>
</div>


<div>
<p>In this step we assign two hospitals from the list to the variables 'hospital1' and 'hospital2'. It is good practice to use variables for the hospital names. If we want to graph data from different hospitals later, we only have to change the below block.</p>
</div>


<div class="in">
<pre>hospital1=hospitalNames[10]
hospital2=hospitalNames[20]

print hospital1+&#34; : &#34;+hospital2</pre>
</div>

<div class="out">
<pre>Woman&#39;s Christian Association : Northern Dutchess Hospital
</pre>
</div>


<div>
<p>We extract the data for hospital1 and hospital2 and check to verify the type of the subdata. It should still be a dataframe.</p>
</div>


<div class="in">
<pre>hospitalData1=df[(df[&#39;Hospital Name&#39;]==hospital1)]
hospitalData2=df[(df[&#39;Hospital Name&#39;]==hospital2)]

type(hospitalData1)</pre>
</div>

<div class="out">
<pre>pandas.core.frame.DataFrame</pre>
</div>


<div>
<p>Now we take a look at the data again. Data can be messy. There is no guarantee the data from different hospitals will be consistent. So we need to check what the dataframe contains. Specifically we want to know what years data was collected, and what Indicators were observed. As before it is handy to use the unique() function to take a look at the unique values of a column.</p>
</div>


<div class="in">
<pre>print hospitalData1[&#39;Year&#39;].unique()
print hospitalData2[&#39;Year&#39;].unique()
print
print hospitalData2[&#39;Indicator Name&#39;].unique()
print
print hospitalData2[&#39;Indicator Name&#39;].unique()</pre>
</div>

<div class="out">
<pre>[2012 2011 2010 2009 2008]
[2012 2011 2010 2009 2008]

[SSI Overall Standardized Infection Ratio SSI Hysterectomy SSI Hip
 SSI Colon CLABSI Overall Standardized Infection Ratio
 CLABSI Medical Surgical ICU Nonteaching CDI Hospital Onset
 CDI Hospital Associated CDI Community Onset]

[SSI Overall Standardized Infection Ratio SSI Hysterectomy SSI Hip
 SSI Colon CLABSI Overall Standardized Infection Ratio
 CLABSI Medical Surgical ICU Nonteaching CDI Hospital Onset
 CDI Hospital Associated CDI Community Onset]
</pre>
</div>


<div>
<p>In this case we see we have data for years 2008 to 2012 and several infection 'Indicators'. So let's us the 'CDI Hospital Associated' category as our indicator and extract that subset of data.</p>
</div>


<div class="in">
<pre>indicator=&#39;CDI Hospital Associated&#39;

infections1=hospitalData1[hospitalData1[&#39;Indicator Name&#39;]==indicator]
infections2=hospitalData2[hospitalData2[&#39;Indicator Name&#39;]==indicator]</pre>
</div>


<div>
<p>Define colors for the plots.</p>
</div>


<div class="in">
<pre>color1=&#39;#d71f26&#39;
color2=&#39;#874c9d&#39;</pre>
</div>


<div>
<p>Now we can use a stacked collumn chart to Create a stacked collumn chart based on this example (http://matplotlib.org/examples/pylab_examples/bar_stacked.html)</p>
</div>


<div class="in">
<pre>p1=plt.bar([1,2,3], infections1[&#39;Infections observed&#39;], 1, color=color1)
p2=plt.bar([1,2,3], infections2[&#39;Infections observed&#39;], 1, color=color2, bottom=infections1[&#39;Infections observed&#39;])

plt.ylabel(&#39;infections&#39;)
plt.title(indicator+&#39;infections&#39;)
plt.xticks([1.5,2.5,3.5],(&#39;2010&#39;, &#39;2011&#39;, &#39;2012&#39;) )
#plt.yticks(np.arange(0,81,10))
plt.legend( (p1[0], p2[0]), (hospital1, hospital2) )
</pre>
</div>

<div class="out">
<pre>&lt;matplotlib.legend.Legend at 0x5b68ed0&gt;
<img src="../../intermediate/datavis/02-infections_files/intermediate/datavis/02-infections_22_1.png">
</pre>
</div>


<div class="in">
<pre></pre>
</div>


<div>
<p>To visualize relative changes we can normalize the data and plot a stacked 100% area chart. The first step is to convert the panda dataframe column to an array so we can perform math operations on it using numpy.</p>
</div>


<div class="in">
<pre>array1=np.array(infections1[&#39;Infections observed&#39;].tolist())
array2=np.array(infections2[&#39;Infections observed&#39;].tolist())</pre>
</div>


<div>
<p>We want the total height of eached stacked column to be 1 (or 100%). To achieve this we divide each array by the sum of both arrays. Afterwards we print out the arrays so we can confirm the data is correct.</p>
</div>


<div class="in">
<pre>array1_normal=array1/(array1+array2)
array2_normal=array2/(array1+array2)

print array1_normal
print array2_normal</pre>
</div>

<div class="out">
<pre>[ 0.45833333  0.83018868  0.6       ]
[ 0.54166667  0.16981132  0.4       ]
</pre>
</div>


<div>
<p>Now Plot the normalized data in a stacked column chart. This gives us the stacked 100% column chart which can be used to see relative differences</p>
</div>


<div class="in">
<pre>p1=plt.bar([1,2,3], array1_normal, 1, color=color1)
p2=plt.bar([1,2,3], array2_normal, 1, color=color2, bottom=array1_normal)

plt.ylabel(&#39;infections&#39;)
plt.title(&#39;hospital infections&#39;)
plt.xticks([1.5,2.5,3.5],(&#39;2010&#39;, &#39;2011&#39;, &#39;2012&#39;) )
#plt.yticks(np.arange(0,81,10))
plt.legend( (p1[0], p2[0]), (hospital1, hospital2) )</pre>
</div>

<div class="out">
<pre>&lt;matplotlib.legend.Legend at 0x5ba4050&gt;
<img src="../../intermediate/datavis/02-infections_files/intermediate/datavis/02-infections_29_1.png">
</pre>
</div>