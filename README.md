# kissbacktest
(Keep it simple and stupid) trading strategy backtesting in Python

## Disclamer

_Quod de futuris non est determinata omnino veritas._

## Download data

```python
import pandas as pd
df = pd.read_csv('https://raw.githubusercontent.com/brulint/kissbacktest/main/btceur_4h.csv')
```

```python
              time     open     high      low    close      volume         count
0     1.657253e+09  21264.0  22050.0  21214.8  21769.9  208.974269  4.540874e+06
1     1.657267e+09  21770.0  21919.1  21400.1  21555.3  342.107795  7.376402e+06
2     1.657282e+09  21552.7  21582.4  21075.0  21239.8  712.399541  1.517022e+07
3     1.657296e+09  21249.2  21718.2  20890.6  21567.3  972.427518  2.054594e+07
4     1.657310e+09  21567.3  21567.4  21192.7  21417.7  204.926552  4.382840e+06
...            ...      ...      ...      ...      ...         ...           ...
2185  1.688717e+09  27689.5  27808.0  27610.4  27751.7   54.904653  1.520114e+06
2186  1.688731e+09  27579.4  27748.5  27570.8  27723.3  102.634849  2.839516e+06
2187  1.688746e+09  27680.9  27862.6  27588.8  27850.1   92.766245  2.573481e+06
2188  1.688760e+09  27768.1  27788.9  27545.9  27597.1   55.765203  1.543954e+06
2189  1.688774e+09  27584.8  27687.2  27571.7  27648.9   21.474707  5.931365e+05

[2190 rows x 7 columns]
```

_To be continued_
_Enjoy!_
