# kissbacktest
(Keep it simple and stupid) trading strategy backtesting in Python

## Disclamer

_Quod de futuris non est determinata omnino veritas._ [^1]

[^1]: "No truth can be determined with certainty about the future." Letter from Nostradamus to Henri II

## Explication

Soit le cour d'un actif (ici le BTC/EUR 4h entre juillet 2022 et juillet 2023)

<p align="center"><img src="img/2023-08-21 20:13:08.179580841 +0200.png"></p>

## Signaux et position

La stratégie consiste à déterminer selon certains critères établis préalablement, les instants $t_n$ pendant lesquels le marché est favorable à l'achat ($SIG_{achat}(t_n) = 1$) et/ou à la vente (\$SIG_{vente}(t_n) = 1$). Entre le $1^{er}$ signal d'achat et le $1^{er}$ signal de vente suivant, on est en position.

Dans notre exemple, la stratégie consiste à acheter quand le cours recommence à monter après une chute et de vendre quand le cours recommence à descendre. On va utiliser le RSI:

### RSI

<p align="center"><img src="img/2023-08-21 20:13:14.203368676 +0200.png"></p>

### Signal d'achat

$$ SIG_{achat}(t_n) = \biggl[ \  RSI_{14}(t_{n-1}) < 25 \  \biggl] \ \\& \ \biggl[ \  RSI_{14}(t_n) > 25 \  \biggr] $$

<p align="center"><img src="img/2023-08-21 20:13:19.063197571 +0200.png"></p>

### Signal de vente

$$ SIG_{vente}(t_n) = \biggl[ \  RSI_{14}(t_{n-1}) > 75 \  \biggl] \ \\& \ \biggl[ \  RSI_{14}(t_n) < 75 \  \biggr] $$

<p align="center"><img src="img/2023-08-21 20:13:24.730998091 +0200.png"></p>

### Position

$$ SIG_0(t_n) = SIG_{achat}(t_n) - SIG_{vente}(t_n) $$

<p align="center"><img src="img/2023-08-21 20:13:36.390587965 +0200.png"></p>

$$ SIG_1(t_n) = \begin{cases} SIG_0(t_n) & \text{si } SIG_0(t_n) \ne 0 \\\\ SIG_0(t_{n-1}) & \text{sinon} \end{cases} $$

$$ POS(t_n) = \biggl[ \  SIG_1(t_n) > 0 \  \biggr] $$

<p align="center"><img src="img/2023-08-21 20:13:41.642403336 +0200.png"></p>

## Rendement

Dans l'interval $[t_{n-1} \rightarrow t_n]$, le rendement vaut :

$$ r_{hodl}(t_n) = r_{hodl}([t_{n-1} \rightarrow t_n]) = \ln \biggl( { Prix(t_n) \over Prix(t_{n-1}) } \biggr) $$

Interprétation du signal $POS$:

  * $POS(t_{n-1}) = 0 \  \\& \  POS(t_n) = 0 \rightarrow$ Hors position de $t_{n-1}$ à $t_n$ $\rightarrow r_{strat}(t_n) = 0$
  * $POS(t_{n-1}) = 0 \  \\& \  POS(t_n) = 1 \rightarrow$ Hors position de $t_{n-1}$ à $t_n$ et achat à l'instant $t_n$ $\rightarrow r_{strat}(t_n) = 0$
  * $POS(t_{n-1}) = 1 \  \\& \  POS(t_n) = 1 \rightarrow$ En position de $t_{n-1}$ à $t_n$ $\rightarrow r_{strat}(t_n) = r_{hodl}(t_n)$
  * $POS(t_{n-1}) = 1 \  \\& \  POS(t_n) = 0 \rightarrow$ En position de $t_{n-1}$ à $t_n$ et vente à l'instant $t_n$ $\rightarrow r_{strat}(t_n) = r_{hodl}(t_n)$

$r_{strat}(t_n)$ est donc une fonction de $POS(t_{n-1})$:

$$ r_{strat}(t_n) = \begin{cases} r_{hodl}(t_n) & \text{si } POS(t_{n-1}) = 1 \\\\ 0 & \text{sinon} \end{cases}  $$

$$ \Rightarrow r_{strat}(t_n) = r_{hodl}(t_n) \times \biggl[ POS(t_{n-1}) \biggr] $$

Lors de chaque transaction (achat et vente), la plateforme prend un fee équivalent à $fee \%$:

Même raisonnement que plus haut:

$$ r_{fee}(t_n) = \begin{cases} fee & \text{si } POS(t_{n-1}) \neq POS(t_n) \\\\ 0 & \text{sinon} \end{cases} $$

$$ \Rightarrow r_{fee}(t_n) = fee \times \biggl[ \ POS(t_{n-1}) \neq POS(t_n) \ \biggr] $$

<p align="center"><img src="img/2023-08-21 20:13:46.774222986 +0200.png"></p>

$$ R(t_n) = \exp \left( \sum_{i=1}^{t_n} \left( r_{strat}(i) - r_{fee}(i) \right) \right) $$

<p align="center"><img src="img/2025-03-23 11_13_52.png"></p>

Avec:
  * en grisé, le rendement cumulé en HOLD
  * en bleu, le rendement brut cumulé de la stratégie
  * en rouge, le rendement net cumulé de la stratégie 

## Implémentation

### Install TALib

To take full advantage of all features (technical indicators), you have to install TALib.

Go to ta-lib.org/install

### Install Python Libraries

```bash
sudo pip3 install TA-Lib bokeh
```

### Download data

Download the 'btceur_4h.csv' from this repository.

or download [the .cvs file directly from Kraken.com](https://support.kraken.com/hc/en-us/articles/360047124832-Downloadable-historical-OHLCVT-Open-High-Low-Close-Volume-Trades-data) and add "time,open,high,low,close,volume,count" a the first line.

### Backtesting

```python
import numpy as np
import pandas as pd
import talib as ta
from bokeh.plotting import figure,show

df = pd.read_csv('Downloads/btceur_4h.csv')

# Strategy begin
RSI = ta.RSI(df.close, timeperiod=14)
SIG_achat = (RSI.shift() < 25) & (RSI > 25)
SIG_vente = (RSI.shift() > 75) & (RSI < 75)
# Strategy end

POS = (SIG_achat.astype(int) - SIG_vente.astype(int)).
POS = POS.replace(to_replace=0, method='ffill') > 0
r_hodl = np.log(df.close / df.close.shift())
r_strat = r_hodl * (POS.shift() == 1)
r_netto = r_strat - 0.0025 * (POS != POS.shift())

fig = figure(height=300)
fig.line(df.time, np.exp(r_hodl.cumsum()), color='lightgray')
fig.line(df.time, np.exp(r_strat.cumsum()), color='blue')
fig.line(df.time, np.exp(r_netto.cumsum()), color='red')
show(fig)
```

_To be continued_

_Enjoy!_
