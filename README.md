# Deep Text Eval (DTE)

## Problem
A differentiable function that measures text complexity/readability will be beneficial as part of the loss function of neural network text simplification system. However, to our best knowledge, there are not such a differentiable
function.

## Basic Idea
Train an RNN to measure the ext complexity/readability. RNN is differentiable by
its nature. This RNN will be part of a future building block for the loss of a
neural network text simplification system.

## Learning Task
Leveraging a classification of articles into a real number readability score.

## Data

### Structure
Each corpus has its own HD5 file with the key `text_df` that contains the
articles, with two these columns:

1. `text` - The text itself.
2. `y` - The level label (integer, stating from `0`), as described below.

### Train-Test Split
80%-20% with shuffle

### Usage in Python (Example with the Weebit Corpus)
```python
import pandas as pd

with pd.HDFStore('data/weebit/weebit.h5') as weebit_store:
	text_df = weebit_store['text_df']
	train_df = weebit_store['train_df']
	test_df = weebit_store['test_df']
```

### Corpora

Crude word count:

1. Weebit - 458,497 words
2. GEO - 1,188,247 words in total
3. DW - 934,734 words in total


#### Weebit
```
Sowmya Vajjala and Detmar Meurers: "On Improving the Accuracy of
Readability Classification using Insights from Second Language
Acquisition". Proceedings of the 7th Workshop on Innovative Use of NLP
for Building Educational Applications (BEA7), Association for
Computational Linguistics. 2012.
```

##### Levels
```
0 - WRLevel2 (Age 7-8)
1 - WRLevel3 (Age 8-9)
2 - WRLevel4 (Age 9-10)
3 - BitKS3 (Age 11-14)
4 - BitGCSE (Age 14-16)
```
##### Train-Test Datasets
The `BitGCSE` class has ~x5 texts than the other classes, therefore it was
downsampled to 800 texts after all the cleaning and preprocessing.

```
level  #text  %text  #train  %train  #test  %test
0      607    16.69  486     16.71   121    16.62
1      788    21.67  630     21.66   158    21.70
2      798    21.95  638     21.94   160    21.98
3      643    17.68  514     17.68   129    17.72
4      800    22.00  640     22.01   160    21.98
```

#### Newsla

##### Levels
###### Continuous y
```
300 - 1670
```

###### Discrete y_cat
```
2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0, 12.0
```

##### Removed Data by small y_cat
```
10.0    23
11.0     2
13.0     1
```


##### Train-Test Datasets

###### Categorial `y`
```
      #text  %text  #train  %train  #test  %test
2.0     921   4.25     737    4.25    184   4.25
3.0    2681  12.37    2145   12.37    536  12.37
4.0    2582  11.92    2065   11.91    517  11.93
5.0    3449  15.92    2759   15.92    690  15.92
6.0    2291  10.57    1833   10.57    458  10.57
7.0    2570  11.86    2056   11.86    514  11.86
8.0    1650   7.61    1320    7.62    330   7.61
9.0    1804   8.33    1443    8.32    361   8.33
12.0   3720  17.17    2976   17.17    744  17.17
```

###### Continuous `y_lexile`
```
    name      #  min   max        mean         std
0   text  21668  300  1670  939.476186  268.819219
1  train  17334  300  1670  939.390793  268.471205
2   test   4334  310  1660  939.817720  270.237459
train-test Kolmogorov-Smirnov p-value 0.9999167905571691
```


#### GEO
##### Levels
```
0 - GEOlino
1 - GEO
```


#### Deutsche Welle
[Deutsche Welle - Deutsche Lernen](http://www.dw.com/de/deutsch-lernen/s-2055)
news articles leveled by
[CEFR](https://en.wikipedia.org/wiki/Common_European_Framework_of_Reference_for_Languages).

##### Levels
```
0 - B1
1 - B2, C1
```
