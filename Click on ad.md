
# Predicting the likelihood of a User clicking on an ad

Research Question

A Kenyan entrepreneur has created an online cryptography course and would want to advertise it on her blog. She currently targets audiences originating from various countries. In the past, she ran ads to advertise a related course on the same blog and collected data in the process. She would now like to employ your services as a Data Science Consultant to create a solution that would allow her to determine whether ads targeted to audiences of certain characteristics i.e. city, male country, ad topic, etc. would click on her ads. Ceate a prediction model that more accurately predicts whether a user will click an Ad.

## Load the Dataset


```R
advertising <- read.csv("advertising.csv",header = T)

head(advertising,10)
```


<table>
<thead><tr><th scope=col>Daily.Time.Spent.on.Site</th><th scope=col>Age</th><th scope=col>Area.Income</th><th scope=col>Daily.Internet.Usage</th><th scope=col>Ad.Topic.Line</th><th scope=col>City</th><th scope=col>Male</th><th scope=col>Country</th><th scope=col>Timestamp</th><th scope=col>Clicked.on.Ad</th></tr></thead>
<tbody>
	<tr><td>68.95                                </td><td>35                                   </td><td>61833.90                             </td><td>256.09                               </td><td>Cloned 5thgeneration orchestration   </td><td>Wrightburgh                          </td><td>0                                    </td><td>Tunisia                              </td><td>2016-03-27 00:53:11                  </td><td>0                                    </td></tr>
	<tr><td>80.23                                </td><td>31                                   </td><td>68441.85                             </td><td>193.77                               </td><td>Monitored national standardization   </td><td>West Jodi                            </td><td>1                                    </td><td>Nauru                                </td><td>2016-04-04 01:39:02                  </td><td>0                                    </td></tr>
	<tr><td>69.47                                </td><td>26                                   </td><td>59785.94                             </td><td>236.50                               </td><td>Organic bottom-line service-desk     </td><td>Davidton                             </td><td>0                                    </td><td>San Marino                           </td><td>2016-03-13 20:35:42                  </td><td>0                                    </td></tr>
	<tr><td>74.15                                </td><td>29                                   </td><td>54806.18                             </td><td>245.89                               </td><td>Triple-buffered reciprocal time-frame</td><td>West Terrifurt                       </td><td>1                                    </td><td>Italy                                </td><td>2016-01-10 02:31:19                  </td><td>0                                    </td></tr>
	<tr><td>68.37                                </td><td>35                                   </td><td>73889.99                             </td><td>225.58                               </td><td>Robust logistical utilization        </td><td>South Manuel                         </td><td>0                                    </td><td>Iceland                              </td><td>2016-06-03 03:36:18                  </td><td>0                                    </td></tr>
	<tr><td>59.99                                </td><td>23                                   </td><td>59761.56                             </td><td>226.74                               </td><td>Sharable client-driven software      </td><td>Jamieberg                            </td><td>1                                    </td><td>Norway                               </td><td>2016-05-19 14:30:17                  </td><td>0                                    </td></tr>
	<tr><td>88.91                                </td><td>33                                   </td><td>53852.85                             </td><td>208.36                               </td><td>Enhanced dedicated support           </td><td>Brandonstad                          </td><td>0                                    </td><td>Myanmar                              </td><td>2016-01-28 20:59:32                  </td><td>0                                    </td></tr>
	<tr><td>66.00                                </td><td>48                                   </td><td>24593.33                             </td><td>131.76                               </td><td>Reactive local challenge             </td><td>Port Jefferybury                     </td><td>1                                    </td><td>Australia                            </td><td>2016-03-07 01:40:15                  </td><td>1                                    </td></tr>
	<tr><td>74.53                                </td><td>30                                   </td><td>68862.00                             </td><td>221.51                               </td><td>Configurable coherent function       </td><td>West Colin                           </td><td>1                                    </td><td>Grenada                              </td><td>2016-04-18 09:33:42                  </td><td>0                                    </td></tr>
	<tr><td>69.88                                </td><td>20                                   </td><td>55642.32                             </td><td>183.82                               </td><td>Mandatory homogeneous architecture   </td><td>Ramirezton                           </td><td>1                                    </td><td>Ghana                                </td><td>2016-07-11 01:42:51                  </td><td>0                                    </td></tr>
</tbody>
</table>



## Data Cleaning and Exploration

### Are there missing values in the dataset?


```R
colSums(is.na(advertising))
```


<dl class=dl-horizontal>
	<dt>Daily.Time.Spent.on.Site</dt>
		<dd>0</dd>
	<dt>Age</dt>
		<dd>0</dd>
	<dt>Area.Income</dt>
		<dd>0</dd>
	<dt>Daily.Internet.Usage</dt>
		<dd>0</dd>
	<dt>Ad.Topic.Line</dt>
		<dd>0</dd>
	<dt>City</dt>
		<dd>0</dd>
	<dt>Male</dt>
		<dd>0</dd>
	<dt>Country</dt>
		<dd>0</dd>
	<dt>Timestamp</dt>
		<dd>0</dd>
	<dt>Clicked.on.Ad</dt>
		<dd>0</dd>
</dl>



No missing values in the advertising dataset

### Are there duplicate rows in the advertising dataset?


```R
duplicated_rows = advertising[duplicated(advertising),]

duplicated_rows
```


<table>
<thead><tr><th scope=col>Daily.Time.Spent.on.Site</th><th scope=col>Age</th><th scope=col>Area.Income</th><th scope=col>Daily.Internet.Usage</th><th scope=col>Ad.Topic.Line</th><th scope=col>City</th><th scope=col>Male</th><th scope=col>Country</th><th scope=col>Timestamp</th><th scope=col>Clicked.on.Ad</th></tr></thead>
<tbody>
</tbody>
</table>



No duplicated values in the advertising dataset

### Check for Outliers


```R
#Screen for Outliers using a boxplot

boxplot(advertising)
```


![png](output_13_0.png)


The boxplot indicates the 3rd column contains outliers beyond the threshold. The Area.Income column.


```R
#List the outliers in the Area Income column

boxplot.stats(advertising$Area.Income)$out
```


<ol class=list-inline>
	<li>17709.98</li>
	<li>18819.34</li>
	<li>15598.29</li>
	<li>15879.1</li>
	<li>14548.06</li>
	<li>13996.5</li>
	<li>14775.5</li>
	<li>18368.57</li>
</ol>



Upon further exploration of the dataset, the outliers are genuine entries. We maintain the outliers.

## Univariate Exploratory Data Analysis

### Measures of Central Tendency

What are the numerical columns in the dataset?


```R
#Check the data type of each column 

sapply(advertising, class)
```


<dl class=dl-horizontal>
	<dt>Daily.Time.Spent.on.Site</dt>
		<dd>'numeric'</dd>
	<dt>Age</dt>
		<dd>'integer'</dd>
	<dt>Area.Income</dt>
		<dd>'numeric'</dd>
	<dt>Daily.Internet.Usage</dt>
		<dd>'numeric'</dd>
	<dt>Ad.Topic.Line</dt>
		<dd>'factor'</dd>
	<dt>City</dt>
		<dd>'factor'</dd>
	<dt>Male</dt>
		<dd>'integer'</dd>
	<dt>Country</dt>
		<dd>'factor'</dd>
	<dt>Timestamp</dt>
		<dd>'factor'</dd>
	<dt>Clicked.on.Ad</dt>
		<dd>'integer'</dd>
</dl>



Check for mean, mode and median of each numerical column

### Mean


```R
#Find the mean of the different columns in the dataset

DailyTime.mean = mean(advertising$Daily.Time.Spent.on.Site)
Age.mean = mean(advertising$Age)
AreaIncome.mean = mean(advertising$Area.Income)
DailyInternet.mean = mean(advertising$Daily.Internet.Usage)

print("The mean of the Daily Time Spent on the site:",quote=FALSE)
DailyTime.mean
print("The mean of the Age:",quote=FALSE)
Age.mean
print("The mean of the Area Income:",quote=FALSE)
AreaIncome.mean
print("The mean of the Daily Internet Usage:",quote=FALSE)
DailyInternet.mean
```

    [1] The mean of the Daily Time Spent on the site:
    


65.0002


    [1] The mean of the Age:
    


36.009


    [1] The mean of the Area Income:
    


55000.00008


    [1] The mean of the Daily Internet Usage:
    


180.0001


### Median


```R
#Find the median of the different columns in the dataset

DailyTime.median = median(advertising$Daily.Time.Spent.on.Site)
Age.median = median(advertising$Age)
AreaIncome.median = median(advertising$Area.Income)
DailyInternet.median = median(advertising$Daily.Internet.Usage)

print("The median of the Daily Time spent on the site:",quote=FALSE)
DailyTime.median
print("The median of the Age:",quote=FALSE)
Age.median
print("The median of the Area Income:",quote=FALSE)
AreaIncome.median
print("The median of the Daily Internet Usage:",quote=FALSE)
DailyInternet.median
```

    [1] The median of the Daily Time spent on the site:
    


68.215


    [1] The median of the Age:
    


35


    [1] The median of the Area Income:
    


57012.3


    [1] The median of the Daily Internet Usage:
    


183.13


### Mode


```R
#Find the mode of the different columns in the dataset

#Set the function to get the mode

getmode <- function(v) {
   uniqv <- unique(v)
   uniqv[which.max(tabulate(match(v, uniqv)))]
}

DailyTime.mode = getmode(advertising$Daily.Time.Spent.on.Site)
Age.mode = getmode(advertising$Age)
AreaIncome.mode = getmode(advertising$Area.Income)
DailyInternet.mode = getmode(advertising$Daily.Internet.Usage)

print("The mode of the Daily Time Spent on the Site:",quote=FALSE)
DailyTime.mode
print("The mode of the Age:",quote=FALSE)
Age.mode
print("The mode of the Area Income:",quote=FALSE)
AreaIncome.mode
print("The mode of the Daily Internet Usage:",quote=FALSE)
DailyInternet.mode
```

    [1] The mode of the Daily Time Spent on the Site:
    


62.26


    [1] The mode of the Age:
    


31


    [1] The mode of the Area Income:
    


61833.9


    [1] The mode of the Daily Internet Usage:
    


167.22


### Measures of Central Dispersion


```R
#Find the minimum value of the different numerical columns in the dataset

DailyTime.min = min(advertising$Daily.Time.Spent.on.Site)
Age.min = min(advertising$Age)
AreaIncome.min = min(advertising$Area.Income)
DailyInternet.min = min(advertising$Daily.Internet.Usage)

print("The minimum value of the Daily Time Spent on the Site:",quote=FALSE)
DailyTime.min
print("The minimum value of the Age:",quote=FALSE)
Age.min
print("The minimum value of the Area Income",quote=FALSE)
AreaIncome.min
print("The minimum value of the Daily Internet Usage:",quote=FALSE)
DailyInternet.min
```

    [1] The minimum value of the Daily Time Spent on the Site:
    


32.6


    [1] The minimum value of the Age:
    


19


    [1] The minimum value of the Area Income
    


13996.5


    [1] The minimum value of the Daily Internet Usage:
    


104.78



```R
#Find the maximum value of the different numerical columns in the dataset

DailyTime.max = max(advertising$Daily.Time.Spent.on.Site)
Age.max = max(advertising$Age)
AreaIncome.max = max(advertising$Area.Income)
DailyInternet.max = max(advertising$Daily.Internet.Usage)

print("The maximum value of the Daily Time Spent on the site:",quote=FALSE)
DailyTime.max
print("The maximum value of the Age:",quote=FALSE)
Age.max
print("The maximum value of the Area Income:",quote=FALSE)
AreaIncome.max
print("The maximum value of the Daily Internet Usage:",quote=FALSE)
DailyInternet.max
```

    [1] The maximum value of the Daily Time Spent on the site:
    


91.43


    [1] The maximum value of the Age:
    


61


    [1] The maximum value of the Area Income:
    


79484.8


    [1] The maximum value of the Daily Internet Usage:
    


269.96



```R
#Find the range in the numerical columns in the dataset

DailyTime.range = range(advertising$Daily.Time.Spent.on.Site)
Age.range = range(advertising$Age)
AreaIncome.range = range(advertising$Area.Income)
DailyInternet.range = range(advertising$Daily.Internet.Usage)

print("The range of the Daily Time spent on the site:",quote=FALSE)
DailyTime.range
print("The range of the Age:",quote=FALSE)
Age.range
print("The range of the Area Income:",quote=FALSE)
AreaIncome.range
print("The range of the Daily Internet Usage:",quote=FALSE)
DailyInternet.range
```

    [1] The range of the Daily Time spent on the site:
    


<ol class=list-inline>
	<li>32.6</li>
	<li>91.43</li>
</ol>



    [1] The range of the Age:
    


<ol class=list-inline>
	<li>19</li>
	<li>61</li>
</ol>



    [1] The range of the Area Income:
    


<ol class=list-inline>
	<li>13996.5</li>
	<li>79484.8</li>
</ol>



    [1] The range of the Daily Internet Usage:
    


<ol class=list-inline>
	<li>104.78</li>
	<li>269.96</li>
</ol>




```R
#Find the quantile in the numerical columns in the dataset

DailyTime.quantile = quantile(advertising$Daily.Time.Spent.on.Site)
Age.quantile = quantile(advertising$Age)
AreaIncome.quantile = quantile(advertising$Area.Income)
DailyInternet.quantile = quantile(advertising$Daily.Internet.Usage)

print("The quantiles of the Daily Time Spent on the site:",quote=FALSE)
DailyTime.quantile
print("The quantiles of the Age:",quote=FALSE)
Age.quantile
print("The quantiles of the Area Income:",quote=FALSE)
AreaIncome.quantile
print("The quantiles of the Daily Internet Usage:",quote=FALSE)
DailyInternet.quantile
```

    [1] The quantiles of the Daily Time Spent on the site:
    


<dl class=dl-horizontal>
	<dt>0%</dt>
		<dd>32.6</dd>
	<dt>25%</dt>
		<dd>51.36</dd>
	<dt>50%</dt>
		<dd>68.215</dd>
	<dt>75%</dt>
		<dd>78.5475</dd>
	<dt>100%</dt>
		<dd>91.43</dd>
</dl>



    [1] The quantiles of the Age:
    


<dl class=dl-horizontal>
	<dt>0%</dt>
		<dd>19</dd>
	<dt>25%</dt>
		<dd>29</dd>
	<dt>50%</dt>
		<dd>35</dd>
	<dt>75%</dt>
		<dd>42</dd>
	<dt>100%</dt>
		<dd>61</dd>
</dl>



    [1] The quantiles of the Area Income:
    


<dl class=dl-horizontal>
	<dt>0%</dt>
		<dd>13996.5</dd>
	<dt>25%</dt>
		<dd>47031.8025</dd>
	<dt>50%</dt>
		<dd>57012.3</dd>
	<dt>75%</dt>
		<dd>65470.635</dd>
	<dt>100%</dt>
		<dd>79484.8</dd>
</dl>



    [1] The quantiles of the Daily Internet Usage:
    


<dl class=dl-horizontal>
	<dt>0%</dt>
		<dd>104.78</dd>
	<dt>25%</dt>
		<dd>138.83</dd>
	<dt>50%</dt>
		<dd>183.13</dd>
	<dt>75%</dt>
		<dd>218.7925</dd>
	<dt>100%</dt>
		<dd>269.96</dd>
</dl>




```R
#Find the variance in the numerical columns in the dataset

DailyTime.variance = var(advertising$Daily.Time.Spent.on.Site)
Age.variance = var(advertising$Age)
AreaIncome.variance = var(advertising$Area.Income)
DailyInternet.variance = var(advertising$Daily.Internet.Usage)

print("The variance of the Daily Time Spent on the Site:",quote=FALSE)
DailyTime.variance
print("The variance of the Age:",quote=FALSE)
Age.variance
print("The variance of the Area Income:",quote=FALSE)
AreaIncome.variance
print("The variance of the Daily Internet Usage:",quote=FALSE)
DailyInternet.variance
```

    [1] The variance of the Daily Time Spent on the Site:
    


251.337094854855


    [1] The variance of the Age:
    


77.1861051051051


    [1] The variance of the Area Income:
    


179952405.951775


    [1] The variance of the Daily Internet Usage:
    


1927.41539618619



```R
#Find the standard deviation in the numerical columns in the dataset

DailyTime.std = sd(advertising$Daily.Time.Spent.on.Site)
Age.std = sd(advertising$Age)
AreaIncome.std = sd(advertising$Area.Income)
DailyInternet.std = sd(advertising$Daily.Internet.Usage)

print("The standard deviation of the Daily Time Usage:",quote=FALSE) 
DailyTime.std
print("The standard deviation of the Age:",quote=FALSE)
Age.std
print("The standard deviation of the Area Income:",quote=FALSE)
AreaIncome.std
print("The standard deviation of the Daily Internet Usage:",quote=FALSE)
DailyInternet.std
```

    [1] The standard deviation of the Daily Time Usage:
    


15.8536145675002


    [1] The standard deviation of the Age:
    


8.78556231012592


    [1] The standard deviation of the Area Income:
    


13414.6340222824


    [1] The standard deviation of the Daily Internet Usage:
    


43.9023393019801


Alternatively, you can use the summary report to explore your dataset


```R
summary(advertising)
```


     Daily.Time.Spent.on.Site      Age         Area.Income    Daily.Internet.Usage
     Min.   :32.60            Min.   :19.00   Min.   :13996   Min.   :104.8       
     1st Qu.:51.36            1st Qu.:29.00   1st Qu.:47032   1st Qu.:138.8       
     Median :68.22            Median :35.00   Median :57012   Median :183.1       
     Mean   :65.00            Mean   :36.01   Mean   :55000   Mean   :180.0       
     3rd Qu.:78.55            3rd Qu.:42.00   3rd Qu.:65471   3rd Qu.:218.8       
     Max.   :91.43            Max.   :61.00   Max.   :79485   Max.   :270.0       
                                                                                  
                                     Ad.Topic.Line              City    
     Adaptive 24hour Graphic Interface      :  1   Lisamouth      :  3  
     Adaptive asynchronous attitude         :  1   Williamsport   :  3  
     Adaptive context-sensitive application :  1   Benjaminchester:  2  
     Adaptive contextually-based methodology:  1   East John      :  2  
     Adaptive demand-driven knowledgebase   :  1   East Timothy   :  2  
     Adaptive uniform capability            :  1   Johnstad       :  2  
     (Other)                                :994   (Other)        :986  
          Male                 Country                  Timestamp   Clicked.on.Ad
     Min.   :0.000   Czech Republic:  9   2016-01-01 02:52:10:  1   Min.   :0.0  
     1st Qu.:0.000   France        :  9   2016-01-01 03:35:35:  1   1st Qu.:0.0  
     Median :0.000   Afghanistan   :  8   2016-01-01 05:31:22:  1   Median :0.5  
     Mean   :0.481   Australia     :  8   2016-01-01 08:27:06:  1   Mean   :0.5  
     3rd Qu.:1.000   Cyprus        :  8   2016-01-01 15:14:24:  1   3rd Qu.:1.0  
     Max.   :1.000   Greece        :  8   2016-01-01 20:17:49:  1   Max.   :1.0  
                     (Other)       :950   (Other)            :994                


### Univariate Graphical Exploratory Data Analysis

### Plotting Boxplots


```R
#Daily Time Spent on Site Boxplot

boxplot(advertising$Daily.Time.Spent.on.Site)
```


![png](output_39_0.png)



```R
#Age Boxplot

boxplot(advertising$Age)
```


![png](output_40_0.png)



```R
#Area Income Boxplot

boxplot(advertising$Area.Income)
```


![png](output_41_0.png)



```R
#Daily Internet Usage Boxplot

boxplot(advertising$Daily.Internet.Usage)
```


![png](output_42_0.png)


### Bar Graphs


```R
#Bar Graph of the Daily Time Spent on the Site

DailyTime = advertising$Daily.Time.Spent.on.Site

DailyTime_frequency = table(DailyTime)

barplot(DailyTime_frequency)
```


![png](output_44_0.png)



```R
#Bar Graph for the Age column

age = advertising$Age

age_frequency = table(age)

barplot(age_frequency)
```


![png](output_45_0.png)



```R
#Bar Graph for the Area Income column

AreaIncome = advertising$Area.Income

AreaIncome_frequency = table(AreaIncome)

barplot(AreaIncome_frequency)
```


![png](output_46_0.png)



```R
#Bar Grpah for the Daily Internet Usage column

DailyInternet = advertising$Daily.Internet.Usage

DailyInternet_frequency = table(DailyInternet)

barplot(DailyInternet_frequency)

```


![png](output_47_0.png)


### Histogram


```R
#Histogram for the Daily Time Spent on the Site column

hist(advertising$Daily.Time.Spent.on.Site)
```


![png](output_49_0.png)



```R
#Histogram for the Age column

hist(advertising$Age)
```


![png](output_50_0.png)



```R
#Histogram for the Area Income column

hist(advertising$Area.Income)
```


![png](output_51_0.png)



```R
#Histogram for the Daily Internet Usage column

hist(advertising$Daily.Internet.Usage)
```


![png](output_52_0.png)


### Bivariate and Multivariate Graphical Data Analysis


```R
#Find the covariance for the Daily Time Spent on Site and the Daily Internet Usage

DailyTime = advertising$Daily.Time.Spent.on.Site

DailyInternet = advertising$Daily.Internet.Usage

print("The Covariance of the Daily Time Spent on Site and Daily Internet Usage:",quote=FALSE)
cov(DailyTime, DailyInternet)

```

    [1] The Covariance of the Daily Time Spent on Site and Daily Internet Usage:
    


360.991882662663



```R
#Find the correlation coefficient between the Daily Time Spent on Site and the Daily Internet Usage

DailyTime = advertising$Daily.Time.Spent.on.Site

DailyInternet = advertising$Daily.Internet.Usage

print("The correlation coefficient of the Daily Time Spent on Site and Daily Internet Usage:",quote=FALSE)
cor(DailyTime, DailyInternet)

```

    [1] The correlation coefficient of the Daily Time Spent on Site and Daily Internet Usage:
    


0.518658475337186



```R
#Plot a scatterplot between the Daily Time and Daily Internet Usage

DailyTime = advertising$Daily.Time.Spent.on.Site

DailyInternet = advertising$Daily.Internet.Usage
 
plot(DailyTime, DailyInternet, xlab="Daily Time Spent on Site", ylab="Daily Internet Usage")

```


![png](output_56_0.png)





```R

```


```R

```


```R

```


```R

```


```R

```


```R

```


```R

```


```R

```


```R

```
