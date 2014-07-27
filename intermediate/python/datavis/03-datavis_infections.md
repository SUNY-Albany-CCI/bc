---
layout: lesson
root: ../..
---

# Introduction  


<div>
<p>This example shows how to make charts for composition data that is changing over time when there are only a small number of time periods. (TODO: explain when one would use &quot;comparison&quot; charts as opposed to &quot;composition&quot; charts)</p>
<p>The example uses yearly data on hospital acquired infections. Data from two hospitals is compared over 3 years.</p>
</div>


<div class="in">
<pre>%matplotlib inline
from matplotlib import pylab as plt
import numpy as np
import pandas as pd
df = pd.read_csv(&#34;Hospital-Acquired_Infections__Beginning_2008.csv&#34;)</pre>
</div>


<div class="in">
<pre>df.columns</pre>
</div>

<div class="out">
<pre>Index([u&#39;Facility Id&#39;, u&#39;Hospital Name&#39;, u&#39;Indicator Name&#39;, u&#39;Year&#39;, u&#39;Infections observed&#39;, u&#39;Infections predicted&#39;, u&#39;Denominator&#39;, u&#39;Indicator value&#39;, u&#39;Indicator Lower confidence limit&#39;, u&#39;Indicator Upper confidence limit&#39;, u&#39;Indicator units&#39;, u&#39;Comparison results&#39;, u&#39;Location 1&#39;], dtype=&#39;object&#39;)</pre>
</div>


<div>
<p>It is useful to look at the head (first 5 rows) of the data in order to inspect how the data is organized.</p>
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
<p>We want to extract data for Brooklyn Hospital and Albany Medical Center. TODO: I know the IDs for each hospital because I inspected the data (https://health.data.ny.gov/Health/Hospital-Acquired-Infections-Beginning-2008/utrt-zdsi?). Need to come up with a way to do it in ipython. For example extracting unique hospital names and corresponding IDs)</p>
</div>


<div class="in">
<pre>df_bhcdc=df[(df[&#39;Facility Id&#39;]==1288)]
</pre>
</div>


<div class="in">
<pre>df_amc=df[(df[&#39;Facility Id&#39;]==1)]</pre>
</div>


<div>
<p>Below we just take a look at the data again. We want to see what was measured. Specifically what years data was collected, and what Indicators were observed.</p>
</div>


<div class="in">
<pre>df_amc</pre>
</div>

<div class="out">
<pre>     Facility Id                   Hospital Name  \
89             1  Albany Medical Center Hospital   
90             1  Albany Medical Center Hospital   
91             1  Albany Medical Center Hospital   
92             1  Albany Medical Center Hospital   
93             1  Albany Medical Center Hospital   
94             1  Albany Medical Center Hospital   
95             1  Albany Medical Center Hospital   
96             1  Albany Medical Center Hospital   
97             1  Albany Medical Center Hospital   
98             1  Albany Medical Center Hospital   
99             1  Albany Medical Center Hospital   
100            1  Albany Medical Center Hospital   
101            1  Albany Medical Center Hospital   
102            1  Albany Medical Center Hospital   
103            1  Albany Medical Center Hospital   
104            1  Albany Medical Center Hospital   
105            1  Albany Medical Center Hospital   
106            1  Albany Medical Center Hospital   
107            1  Albany Medical Center Hospital   
108            1  Albany Medical Center Hospital   
109            1  Albany Medical Center Hospital   
110            1  Albany Medical Center Hospital   
111            1  Albany Medical Center Hospital   
112            1  Albany Medical Center Hospital   
113            1  Albany Medical Center Hospital   
114            1  Albany Medical Center Hospital   
115            1  Albany Medical Center Hospital   
116            1  Albany Medical Center Hospital   
117            1  Albany Medical Center Hospital   
118            1  Albany Medical Center Hospital   
..           ...                             ...   
134            1  Albany Medical Center Hospital   
135            1  Albany Medical Center Hospital   
136            1  Albany Medical Center Hospital   
137            1  Albany Medical Center Hospital   
138            1  Albany Medical Center Hospital   
139            1  Albany Medical Center Hospital   
140            1  Albany Medical Center Hospital   
141            1  Albany Medical Center Hospital   
142            1  Albany Medical Center Hospital   
143            1  Albany Medical Center Hospital   
144            1  Albany Medical Center Hospital   
145            1  Albany Medical Center Hospital   
146            1  Albany Medical Center Hospital   
147            1  Albany Medical Center Hospital   
148            1  Albany Medical Center Hospital   
149            1  Albany Medical Center Hospital   
150            1  Albany Medical Center Hospital   
151            1  Albany Medical Center Hospital   
152            1  Albany Medical Center Hospital   
153            1  Albany Medical Center Hospital   
154            1  Albany Medical Center Hospital   
155            1  Albany Medical Center Hospital   
156            1  Albany Medical Center Hospital   
157            1  Albany Medical Center Hospital   
158            1  Albany Medical Center Hospital   
159            1  Albany Medical Center Hospital   
160            1  Albany Medical Center Hospital   
161            1  Albany Medical Center Hospital   
162            1  Albany Medical Center Hospital   
163            1  Albany Medical Center Hospital   

                                    Indicator Name  Year  Infections observed  \
89                                SSI Hysterectomy  2012                    2   
90        SSI Overall Standardized Infection Ratio  2012                   27   
91                                         SSI Hip  2012                    3   
92                                       SSI Colon  2012                   14   
93                             SSI CABG donor site  2012                    1   
94                             SSI CABG chest site  2012                    7   
95                             CLABSI Surgical ICU  2012                    3   
96                            CLABSI Pediatric ICU  2012                    1   
97     CLABSI Overall Standardized Infection Ratio  2012                   12   
98                        CLABSI Neurosurgical ICU  2012                    0   
99   CLABSI Neonatal ICU Regional Perinatal Center  2012                    0   
100                             CLABSI Medical ICU  2012                    2   
101                            CLABSI Coronary ICU  2012                    2   
102                      CLABSI Cardiothoracic ICU  2012                    4   
103                             CDI Hospital Onset  2012                  183   
104                        CDI Hospital Associated  2012                  237   
105                            CDI Community Onset  2012                  113   
106                            CLABSI Surgical ICU  2011                    5   
107                            CDI Community Onset  2011                   81   
108                        CDI Hospital Associated  2011                  185   
109                             CDI Hospital Onset  2011                  147   
110                      CLABSI Cardiothoracic ICU  2011                    1   
111                            CLABSI Coronary ICU  2011                    2   
112                             CLABSI Medical ICU  2011                    4   
113  CLABSI Neonatal ICU Regional Perinatal Center  2011                    3   
114                       CLABSI Neurosurgical ICU  2011                    1   
115    CLABSI Overall Standardized Infection Ratio  2011                   18   
116                           CLABSI Pediatric ICU  2011                    2   
117                            SSI CABG chest site  2011                    3   
118                            SSI CABG donor site  2011                    0   
..                                             ...   ...                  ...   
134                      CLABSI Cardiothoracic ICU  2010                    0   
135                             CDI Hospital Onset  2010                   91   
136                        CDI Hospital Associated  2010                  115   
137                            CDI Community Onset  2010                   37   
138                             CLABSI Medical ICU  2009                    5   
139                       CLABSI Neurosurgical ICU  2009                    0   
140  CLABSI Neonatal ICU Regional Perinatal Center  2009                    1   
141                            CLABSI Coronary ICU  2009                    1   
142                      CLABSI Cardiothoracic ICU  2009                    4   
143    CLABSI Overall Standardized Infection Ratio  2009                   16   
144                           CLABSI Pediatric ICU  2009                    0   
145                            CLABSI Surgical ICU  2009                    5   
146                            SSI CABG chest site  2009                    7   
147                            SSI CABG donor site  2009                    1   
148                                      SSI Colon  2009                   20   
149                                        SSI Hip  2009                    1   
150       SSI Overall Standardized Infection Ratio  2009                   29   
151       SSI Overall Standardized Infection Ratio  2008                   27   
152                                        SSI Hip  2008                    0   
153                                      SSI Colon  2008                   22   
154                            SSI CABG donor site  2008                    0   
155                            SSI CABG chest site  2008                    5   
156                            CLABSI Surgical ICU  2008                   11   
157                       CLABSI Neurosurgical ICU  2008                    0   
158  CLABSI Neonatal ICU Regional Perinatal Center  2008                    5   
159                             CLABSI Medical ICU  2008                    5   
160                            CLABSI Coronary ICU  2008                    1   
161                      CLABSI Cardiothoracic ICU  2008                    7   
162                           CLABSI Pediatric ICU  2008                   14   
163    CLABSI Overall Standardized Infection Ratio  2008                   43   

     Infections predicted  Denominator  Indicator value  \
89                   3.96          239             0.80   
90                  32.73          NaN             0.83   
91                   4.12          397             0.80   
92                  16.20          360             3.90   
93                   1.77          282             0.30   
94                   6.67          312             2.10   
95                   6.04         5307             0.60   
96                   4.19         2173             0.50   
97                  26.93          NaN             0.45   
98                   1.67         1195             0.00   
99                   4.91         4041             0.00   
100                  4.00         3309             0.60   
101                  3.31         2674             0.70   
102                  2.81         3175             1.30   
103                   NaN       183307             9.98   
104                   NaN       183307            12.93   
105                   NaN        32315             0.35   
106                  7.56         5429             0.90   
107                   NaN        31536             0.26   
108                   NaN       173406            10.67   
109                   NaN       173406             8.48   
110                  3.02         3311             0.30   
111                  3.22         2278             0.90   
112                  4.07         2720             1.50   
113                  7.92         4440             0.68   
114                  1.59         1211             0.80   
115                 32.11          NaN             0.56   
116                  4.73         2202             0.90   
117                  6.80          354             0.80   
118                  2.09          315             0.00   
..                    ...          ...              ...   
134                  2.97         2994             0.00   
135                   NaN       168962             5.39   
136                   NaN       168962             6.81   
137                   NaN        29917             0.12   
138                  6.50         2821             1.80   
139                  2.44         1148             0.00   
140                 11.58         6155             0.17   
141                  4.01         2163             0.50   
142                  3.55         2868             1.40   
143                 41.45          NaN             0.39   
144                  4.58         2039             0.00   
145                  8.79         4111             1.20   
146                  8.70          367             1.80   
147                  2.94          340             0.30   
148                 17.89          379             5.40   
149                  3.19          270             0.40   
150                 32.72          NaN             0.89   
151                 30.27          NaN             0.89   
152                  2.31          209             0.00   
153                 15.56          360             6.30   
154                  3.60          350             0.00   
155                  8.80          398             1.20   
156                 12.36         4448             2.50   
157                  1.20          528             0.00   
158                 13.62         5120             1.05   
159                  8.97         3321             1.50   
160                  4.75         2197             0.50   
161                  4.11         2882             2.40   
162                  6.57         1972             7.10   
163                 51.58          NaN             0.83   

     Indicator Lower confidence limit  Indicator Upper confidence limit  \
89                               0.10                              2.97   
90                               0.54                              1.20   
91                               0.16                              2.22   
92                               2.15                              6.59   
93                               0.01                              1.73   
94                               0.85                              4.37   
95                               0.12                              1.65   
96                               0.01                              2.56   
97                               0.23                              0.78   
98                               0.00                              2.51   
99                               0.00                              0.75   
100                              0.07                              2.18   
101                              0.09                              2.70   
102                              0.34                              3.23   
103                              8.59                             11.54   
104                             11.34                             14.68   
105                              0.29                              0.42   
106                              0.30                              2.15   
107                              0.20                              0.32   
108                              9.19                             12.32   
109                              7.16                              9.96   
110                              0.01                              1.68   
111                              0.11                              3.17   
112                              0.40                              3.77   
113                              0.14                              1.98   
114                              0.02                              4.60   
115                              0.33                              0.89   
116                              0.11                              3.28   
117                              0.17                              2.47   
118                              0.00                              0.91   
..                                ...                               ...   
134                              0.00                              1.00   
135                              4.34                              6.61   
136                              5.62                              8.17   
137                              0.09                              0.17   
138                              0.58                              4.14   
139                              0.00                              2.61   
140                              0.00                              0.97   
141                              0.01                              2.58   
142                              0.38                              3.57   
143                              0.22                              0.63   
144                              0.00                              1.47   
145                              0.39                              2.84   
146                              0.73                              3.75   
147                              0.01                              1.67   
148                              3.32                              8.39   
149                              0.01                              1.99   
150                              0.59                              1.27   
151                              0.59                              1.30   
152                              0.00                              1.45   
153                              3.93                              9.49   
154                              0.00                              0.82   
155                              0.40                              2.86   
156                              1.23                              4.42   
157                              0.00                              5.67   
158                              0.34                              2.46   
159                              0.49                              3.51   
160                              0.01                              2.54   
161                              0.98                              5.00   
162                              3.88                             11.91   
163                              0.60                              1.12   

                                       Indicator units  \
89             # SSI per 100 procedures, risk-adjusted   
90                                                 NaN   
91             # SSI per 100 procedures, risk-adjusted   
92             # SSI per 100 procedures, risk-adjusted   
93             # SSI per 100 procedures, risk-adjusted   
94             # SSI per 100 procedures, risk-adjusted   
95                         # CLABSI per 1000 line days   
96                         # CLABSI per 1000 line days   
97                                                 NaN   
98                         # CLABSI per 1000 line days   
99                         # CLABSI per 1000 line days   
100                        # CLABSI per 1000 line days   
101                        # CLABSI per 1000 line days   
102                        # CLABSI per 1000 line days   
103  # hospital onset cases per 10,000 patient days...   
104  # hospital associated per 10,000 patient days,...   
105  # comm. onset (not my hospital) per 100 admiss...   
106                        # CLABSI per 1000 line days   
107  # comm. onset (not my hospital) per 100 admiss...   
108  # hospital associated per 10,000 patient days,...   
109  # hospital onset cases per 10,000 patient days...   
110                        # CLABSI per 1000 line days   
111                        # CLABSI per 1000 line days   
112                        # CLABSI per 1000 line days   
113                        # CLABSI per 1000 line days   
114                        # CLABSI per 1000 line days   
115                                                NaN   
116                        # CLABSI per 1000 line days   
117            # SSI per 100 procedures, risk-adjusted   
118            # SSI per 100 procedures, risk-adjusted   
..                                                 ...   
134                        # CLABSI per 1000 line days   
135  # hospital onset cases per 10,000 patient days...   
136  # hospital associated per 10,000 patient days,...   
137  # comm. onset (not my hospital) per 100 admiss...   
138                        # CLABSI per 1000 line days   
139                        # CLABSI per 1000 line days   
140                        # CLABSI per 1000 line days   
141                        # CLABSI per 1000 line days   
142                        # CLABSI per 1000 line days   
143                                                NaN   
144                        # CLABSI per 1000 line days   
145                        # CLABSI per 1000 line days   
146            # SSI per 100 procedures, risk-adjusted   
147            # SSI per 100 procedures, risk-adjusted   
148            # SSI per 100 procedures, risk-adjusted   
149            # SSI per 100 procedures, risk-adjusted   
150                                                NaN   
151                                                NaN   
152            # SSI per 100 procedures, risk-adjusted   
153            # SSI per 100 procedures, risk-adjusted   
154            # SSI per 100 procedures, risk-adjusted   
155            # SSI per 100 procedures, risk-adjusted   
156                        # CLABSI per 1000 line days   
157                        # CLABSI per 1000 line days   
158                        # CLABSI per 1000 line days   
159                        # CLABSI per 1000 line days   
160                        # CLABSI per 1000 line days   
161                        # CLABSI per 1000 line days   
162                        # CLABSI per 1000 line days   
163                                                NaN   

                                    Comparison results  \
89   Not significantly different than NYS 2012 average   
90   Not significantly different than NYS 2012 average   
91   Not significantly different than NYS 2012 average   
92   Not significantly different than NYS 2012 average   
93   Not significantly different than NYS 2012 average   
94   Not significantly different than NYS 2012 average   
95   Not significantly different than NYS 2012 average   
96   Not significantly different than NYS 2012 average   
97           Significantly lower than NYS 2012 average   
98   Not significantly different than NYS 2012 average   
99           Significantly lower than NYS 2012 average   
100  Not significantly different than NYS 2012 average   
101  Not significantly different than NYS 2012 average   
102  Not significantly different than NYS 2012 average   
103  Not significantly different than Hospital 2011...   
104                                       Not compared   
105                                       Not compared   
106  Not significantly different than NYS 2011 average   
107                                       Not compared   
108                                       Not compared   
109      Not compared due to change in lab test method   
110  Not significantly different than NYS 2011 average   
111  Not significantly different than NYS 2011 average   
112  Not significantly different than NYS 2011 average   
113  Not significantly different than NYS 2011 average   
114  Not significantly different than NYS 2011 average   
115          Significantly lower than NYS 2011 average   
116  Not significantly different than NYS 2011 average   
117  Not significantly different than NYS 2011 average   
118  Not significantly different than NYS 2011 average   
..                                                 ...   
134  Not significantly different than NYS 2010 average   
135                                       Not compared   
136                                       Not compared   
137                                       Not compared   
138  Not significantly different than NYS 2009 average   
139  Not significantly different than NYS 2009 average   
140          Significantly lower than NYS 2009 average   
141  Not significantly different than NYS 2009 average   
142  Not significantly different than NYS 2009 average   
143          Significantly lower than NYS 2009 average   
144          Significantly lower than NYS 2009 average   
145  Not significantly different than NYS 2009 average   
146  Not significantly different than NYS 2009 average   
147  Not significantly different than NYS 2009 average   
148  Not significantly different than NYS 2009 average   
149  Not significantly different than NYS 2009 average   
150  Not significantly different than NYS 2009 average   
151  Not significantly different than NYS 2008 average   
152  Not significantly different than NYS 2008 average   
153  Not significantly different than NYS 2008 average   
154          Significantly lower than NYS 2008 average   
155  Not significantly different than NYS 2008 average   
156  Not significantly different than NYS 2008 average   
157  Not significantly different than NYS 2008 average   
158          Significantly lower than NYS 2008 average   
159  Not significantly different than NYS 2008 average   
160  Not significantly different than NYS 2008 average   
161  Not significantly different than NYS 2008 average   
162         Significantly higher than NYS 2008 average   
163  Not significantly different than NYS 2008 average   

                  Location 1  
89   (42.653238, -73.773969)  
90   (42.653238, -73.773969)  
91   (42.653238, -73.773969)  
92   (42.653238, -73.773969)  
93   (42.653238, -73.773969)  
94   (42.653238, -73.773969)  
95   (42.653238, -73.773969)  
96   (42.653238, -73.773969)  
97   (42.653238, -73.773969)  
98   (42.653238, -73.773969)  
99   (42.653238, -73.773969)  
100  (42.653238, -73.773969)  
101  (42.653238, -73.773969)  
102  (42.653238, -73.773969)  
103  (42.653238, -73.773969)  
104  (42.653238, -73.773969)  
105  (42.653238, -73.773969)  
106  (42.653238, -73.773969)  
107  (42.653238, -73.773969)  
108  (42.653238, -73.773969)  
109  (42.653238, -73.773969)  
110  (42.653238, -73.773969)  
111  (42.653238, -73.773969)  
112  (42.653238, -73.773969)  
113  (42.653238, -73.773969)  
114  (42.653238, -73.773969)  
115  (42.653238, -73.773969)  
116  (42.653238, -73.773969)  
117  (42.653238, -73.773969)  
118  (42.653238, -73.773969)  
..                       ...  
134  (42.653238, -73.773969)  
135  (42.653238, -73.773969)  
136  (42.653238, -73.773969)  
137  (42.653238, -73.773969)  
138  (42.653238, -73.773969)  
139  (42.653238, -73.773969)  
140  (42.653238, -73.773969)  
141  (42.653238, -73.773969)  
142  (42.653238, -73.773969)  
143  (42.653238, -73.773969)  
144  (42.653238, -73.773969)  
145  (42.653238, -73.773969)  
146  (42.653238, -73.773969)  
147  (42.653238, -73.773969)  
148  (42.653238, -73.773969)  
149  (42.653238, -73.773969)  
150  (42.653238, -73.773969)  
151  (42.653238, -73.773969)  
152  (42.653238, -73.773969)  
153  (42.653238, -73.773969)  
154  (42.653238, -73.773969)  
155  (42.653238, -73.773969)  
156  (42.653238, -73.773969)  
157  (42.653238, -73.773969)  
158  (42.653238, -73.773969)  
159  (42.653238, -73.773969)  
160  (42.653238, -73.773969)  
161  (42.653238, -73.773969)  
162  (42.653238, -73.773969)  
163  (42.653238, -73.773969)  

[75 rows x 13 columns]</pre>
</div>


<div>
<p>take a look at the 'CDI Hospital Associated' category</p>
</div>


<div class="in">
<pre>df_amc_infections=df_amc[df_amc[&#39;Indicator Name&#39;]==&#39;CDI Hospital Associated&#39;]</pre>
</div>


<div class="in">
<pre>df_bhcdc_infections=df_bhcdc[df_bhcdc[&#39;Indicator Name&#39;]==&#39;CDI Hospital Associated&#39;]</pre>
</div>


<div>
<p>Create a stacked collumn chart based on this example (http://matplotlib.org/examples/pylab_examples/bar_stacked.html)</p>
</div>


<div class="in">
<pre>p1=plt.bar([1,2,3], df_amc_infections[&#39;Infections observed&#39;], 1, color=&#39;r&#39;)
p2=plt.bar([1,2,3], df_bhcdc_infections[&#39;Infections observed&#39;], 1, color=&#39;b&#39;, bottom=df_amc_infections[&#39;Infections observed&#39;])

plt.ylabel(&#39;infections&#39;)
plt.title(&#39;hospital infections&#39;)
plt.xticks([1.5,2.5,3.5],(&#39;2010&#39;, &#39;2011&#39;, &#39;2012&#39;) )
#plt.yticks(np.arange(0,81,10))
plt.legend( (p1[0], p2[0]), (&#39;Albany&#39;, &#39;Brooklyn&#39;) )
</pre>
</div>

<div class="out">
<pre>&lt;matplotlib.legend.Legend at 0x5096a90&gt;
<img src="../../intermediate/python/datavis/03-datavis_infections_files/intermediate/python/datavis/03-datavis_infections_15_1.png">
</pre>
</div>


<div>
<p>convert panda dataframe column to an array so we can normalize the data</p>
</div>


<div class="in">
<pre>array1=np.array(df_amc_infections[&#39;Infections observed&#39;].tolist())
array2=np.array(df_bhcdc_infections[&#39;Infections observed&#39;].tolist())</pre>
</div>


<div class="in">
<pre>array1_normal=array1/(array1+array2)
array2_normal=array2/(array1+array2)
</pre>
</div>


<div>
<p>Plot the normalized data in a stacked column chart. This gives us a stacked 100% column chart which can be used to easily see relative differences</p>
</div>


<div class="in">
<pre>p1=plt.bar([1,2,3], array1_normal, 1, color=&#39;r&#39;)
p2=plt.bar([1,2,3], array2_normal, 1, color=&#39;b&#39;, bottom=array1_normal)

plt.ylabel(&#39;infections&#39;)
plt.title(&#39;hospital infections&#39;)
plt.xticks([1.5,2.5,3.5],(&#39;2010&#39;, &#39;2011&#39;, &#39;2012&#39;) )
#plt.yticks(np.arange(0,81,10))
plt.legend( (p1[0], p2[0]), (&#39;Albany&#39;, &#39;Brooklyn&#39;) )</pre>
</div>

<div class="out">
<pre>&lt;matplotlib.legend.Legend at 0x47b0ad0&gt;
<img src="../../intermediate/python/datavis/03-datavis_infections_files/intermediate/python/datavis/03-datavis_infections_20_1.png">
</pre>
</div>


<div class="in">
<pre></pre>
</div>