# kissbacktest
(Keep it simple and stupid) trading strategy backtesting in Python

## Disclamer

_Quod de futuris non est determinata omnino veritas._

## Download data

Soit le cour d'un actif (ici le BTC/EUR 4h entre juillet 2022 et juillet 2023)

Par exemple disponible ici:

```python
import pandas as pd
df = pd.read_csv('https://raw.githubusercontent.com/brulint/kissbacktest/main/btceur_4h.csv')
```

<p align="center"><img src="img/2023-08-21 20:13:08.179580841 +0200.png"></p>

Dans l'interval $[t_{n-1} \rightarrow t_n]$, le rendement vaut :

$$ r_0(t_n) = r_0([t_{n-1} \rightarrow t_n]) = ln \biggl( { Prix(t_n) \over Prix(t_{n-1}) } \biggr) $$


<p align="center"><img src="img/2023-08-21 20:13:14.203368676 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:19.063197571 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:24.730998091 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:36.390587965 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:41.642403336 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:46.774222986 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:52.218031736 +0200.png"></p>











_To be continued_

_Enjoy!_
