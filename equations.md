# Photovoltaic Model Equations (Sizing + Payback)

Grid-tied photovoltaic system for micro/mini-generators (Groups A and B) under **Law 14.300/2022**, with technical sizing and financial feasibility analysis (NPV, IRR, simple and discounted payback).

## Nomenclature

| Symbol | Description | Unit |
|---|---|---|
| $E_M$ | Average monthly energy consumption | kWh/month |
| $TUSD_B,\ TE_B$ | Consumption tariffs — Group B | R\$/kWh |
| $TUSD_A,\ TE_A$ | Full tariffs (off-peak) — Group A | R\$/kWh |
| $TUSD_A^{scee},\ TE_A^{scee}$ | SCEE tariffs — Group A | R\$/kWh |
| $TUSD_g$ | Generation demand tariff (TUSDg) — Group A | R\$/kW |
| $f_B$ | Fio B percentage over the TUSD | — |
| $GHI$ | Global horizontal solar irradiation | kWh/m²·day |
| $\eta_{sis}$ | Overall system efficiency | — |
| $P_{mod}$ | Module power | W |
| $FS$ | Inverter overload factor ($P_{CC}/P_{CA}$) | — |
| $N_{mppt}$ | Number of inverter MPPTs | — |
| $V_{oc},\ \alpha$ | Open-circuit voltage and module temperature coefficient | V; /°C |
| $T_{min}$ | Minimum temperature | °C |
| $\phi$ | Latitude | ° |
| $a$ | Annual energy tariff increase | /year |
| $t_{ac}$ | Self-consumption rate | — |
| $i$ | Discount rate | /year |
| $V_{inv}$ | Inverter service life | years |
| $m,\ c_{inv}$ | Maintenance and inverter-replacement cost (% of savings) | — |
| $p_1,\ p_n$ | Efficiency loss in 1st year / following years | — |
| $c$ | ANEEL scenario: pessimistic (1), base (2), optimistic (3) | — |
| $I_0$ | Initial investment | R\$ |

---

## 1. Sizing

**Unit energy costs**

$$C_{cons}^{B} = TUSD_B + TE_B$$

$$C_{inj}^{B} = TE_B + TUSD_B\,(1 - f_B)$$

$$C_{cons}^{A} = TUSD_A + TE_A$$

$$C_{inj}^{A} = TE_A^{scee} + TUSD_A^{scee}\,(1 - f_B)$$

**Array sizing**

$$P_{FV}^{teo} = \frac{E_M \cdot 1000}{30 \cdot \eta_{sis} \cdot GHI}$$

$$N_{mod} = \left\lceil \frac{P_{FV}^{teo}}{P_{mod}} \right\rceil
\qquad
N_{mod/mppt} = \text{round}\!\left(\frac{N_{mod}}{N_{mppt}}\right)$$

$$P_{FV} = N_{mod} \cdot P_{mod}
\qquad
P_{inv}^{CA} = \frac{P_{FV}}{FS}$$

$$E_{FV} = \frac{30 \cdot GHI \cdot \eta_{sis} \cdot P_{FV}}{1000}
\qquad
D_{ger} = \frac{P_{inv}^{CA}}{1000}$$

**Voltage check and tilt**

$$V_{mppt}^{max} = V_{oc}\,\bigl(1 + (T_{min} - 25)\,\alpha\bigr)\,N_{mod/mppt}$$

$$\beta =
\begin{cases}
10 & \phi < 10 \\
\phi & 10 \le \phi \le 20 \\
\phi + 5 & 20 < \phi \le 30 \\
\phi + 10 & 30 < \phi \le 40 \\
\phi + 15 & \phi > 40
\end{cases}$$

**Costs and investment**

$$\text{cost}_{item} = q_{item}\cdot c_{item}
\qquad
\text{Distributed price} = \sum_i \text{cost}_{item,i}$$

$$\text{Material} = \text{IF}(\text{option}=1;\ \text{Distributed price};\ \text{Bundled price})$$

$$\text{Transport} = 0{.}10\cdot\text{Material}$$

$$I_0 = \text{Material} + \text{Transport} + \text{Studies} + \text{Labor}$$

$$\text{Cost}_{Wp} = \frac{I_0}{P_{FV}}$$

---

## 2. Financial Feasibility (Payback)

Evaluated over $n = 0,1,\dots,30$ years. Injection base tariffs: Group A uses $TE_A^{scee},\ TUSD_A^{scee}$; Group B uses $TE_B,\ TUSD_B$.

**Annual energy generated** (module degradation)

$$E_g(n) =
\begin{cases}
12\,E_{FV} & n = 1 \\
12\,E_{FV}\,(1 - p_1) & n = 2 \\
12\,E_{FV}\,\bigl(1 - p_1 - (n-2)\,p_n\bigr) & n \ge 3
\end{cases}$$

**Adjusted unit costs**

$$C_{cons}(n) =
\begin{cases}
TE_{ef} + TUSD_{ef} & n = 1 \\
C_{cons}(n-1)\,(1 + a) & n > 1
\end{cases}$$

$$C_{inj}(1) =
\begin{cases}
TE_{base} & c = 1 \\
TE_{base} + TUSD_{base}\,(1 - f_B) & c = 2 \\
TE_{base} + TUSD_{base} & c = 3
\end{cases}
\qquad
C_{inj}(n>1) = C_{inj}(n-1)\,(1+a)$$

**Savings, maintenance and cash flow**

$$S(n) = \bigl(C_{cons}(n)\,t_{ac} + (1 - t_{ac})\,C_{inj}(n)\bigr)\,E_g(n)$$

$$M(n) =
\begin{cases}
S(n)\,(m + c_{inv}) & n = V_{inv}\ \text{or}\ n = 2\,V_{inv} \\
S(n)\,m & \text{otherwise}
\end{cases}$$

$$FC(n) =
\begin{cases}
-\,I_0 & n = 0 \\
S(n) - M(n) - \text{IF}\bigl(\text{Group A};\ 12\,D_{ger}\,TUSD_g;\ 0\bigr) & n \ge 1
\end{cases}$$

**Cumulative values and metrics**

$$FC_{ac}(n) = FC_{ac}(n-1) + FC(n)
\qquad
FD(n) = \frac{FC(n)}{(1 + i)^{\,n}}
\qquad
FD_{ac}(n) = FD_{ac}(n-1) + FD(n)$$

$$\boxed{\ \text{NPV} = \sum_{n=0}^{30} FD(n)\ }
\qquad
\boxed{\ \text{IRR}:\ \sum_{n=0}^{30} \frac{FC(n)}{(1 + \text{IRR})^{\,n}} = 0\ }$$

The simple and discounted **paybacks** are the year in which $FC_{ac}(n)$ and $FD_{ac}(n)$ cross zero, respectively.
