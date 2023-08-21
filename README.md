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

## Signaux et position

La stratégie consiste à déterminer selon certains critères établis préalablement, les instants $t_n$ pendant lesquels le marché est favorable à l'achat ($SIG_{achat}(t_n) = 1$) et/ou à la vente (\$SIG_{vente}(t_n) = 1$). Entre le $1^{er}$ signal d'achat et le $1^{er}$ signal de vente suivant, on est en position.

Dans notre exemple, la stratégie consiste à acheter quand le cours recommence à monter après une chute et de vendre quand le cours recommence à descendre. On va utiliser le RSI:

<p align="center"><img src="img/2023-08-21 20:13:14.203368676 +0200.png"></p>

$$ SIG_{achat} = \biggl[ \  RSI_{14}(t_{n-1}) < 25 \  \biggl] \ \\& \ \biggl[ \  RSI_{14}(t_n) > 25 \  \biggr] $$

<p align="center"><img src="img/2023-08-21 20:13:19.063197571 +0200.png"></p>

$$ SIG_{vente} = \biggl[ \  RSI_{14}(t_{n-1}) > 75 \  \biggl] \ \\& \ \biggl[ \  RSI_{14}(t_n) < 75 \  \biggr] $$

<p align="center"><img src="img/2023-08-21 20:13:24.730998091 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:36.390587965 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:41.642403336 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:46.774222986 +0200.png"></p>
<p align="center"><img src="img/2023-08-21 20:13:52.218031736 +0200.png"></p>











_To be continued_

_Enjoy!_
