# 📊 Photovoltaic Sizing and Payback Simulator

This repository contains the documentation for the formulas and mathematical logic used in the financial and technical simulation model for photovoltaic solar energy projects (Group A and Group B). The model is structured into two main sheets in Excel: **Dimensionamento** (Sizing) and **Payback**.

---

## 🛠️ Sheet: Dimensionamento

This sheet is responsible for the technical sizing of the system components (modules, inverters, strings) and the breakdown of initial capital expenditures (CAPEX).

### 1. Cost Parameters and Tariffs
The table below defines the consumption and injection costs based on the tariff components (TUSD and TE) and the Fio B impact:

| Variable | Description / Formula |
| :--- | :--- |
| `custo_cons_B` | Consumption cost for Group B:<br> $$TUSD_B + TE_B$$ |
| `custo_inj_B` | Injection cost for Group B (with Fio B discount):<br> $$TUSD_B + TE_B \cdot (1 - \text{fioB}\%_{TUSD})$$ |
| `custo_cons_A` | Consumption cost for Group A:<br> $$TUSD_{A\_cheio} + TE_{A\_cheio}$$ |
| `custo_inj_A` | Injection cost for Group A:<br> $$TE_{A\_scee} + TUSD_{A\_scee} \cdot (1 - \text{fioB}\%_{TUSD})$$ |

### 2. Technical Sizing

* **Generation Demand ($Dem_{ger}$):**
  $$Dem_{ger} = \frac{Pinv_{CA}}{1000}$$

* **Theoretical Photovoltaic Power ($Pfv_{teorica}$):**
  $$Pfv_{teorica} = \frac{\text{consumo} \cdot 1000}{30 \cdot \eta_{sis} \cdot GHI}$$

* **Number of Modules ($N_{mod}$):**
  $$N_{mod} = \text{ARRED.P.CIMA}\left(\frac{Pfv_{teorica}}{P_{mod}}\right)$$

* **Modules per MPPT ($N_{mod\_por\_mppt}$):**
  $$N_{mod\_por\_mppt} = \text{ARRED}\left(\frac{N_{mod}}{N_{mppt}}\right)$$

* **Real Photovoltaic Power ($Pfv_{real}$):**
  $$Pfv_{real} = N_{mod} \cdot P_{mod}$$

* **AC Inverter Power ($Pinv_{CA}$):**
  $$Pinv_{CA} = \frac{Pfv_{real}}{FS_{inv}}$$

* **Real Monthly Energy Generation ($Efv_{real}$):**
  $$Efv_{real} = \frac{30 \cdot GHI \cdot \eta_{sis} \cdot Pfv_{real}}{1000}$$

* **Maximum MPPT Voltage ($Vmax_{mppt}$):**
  $$Vmax_{mppt} = Voc \cdot (1 + (T_{min} - 25) \cdot \alpha) \cdot N_{mod\_por\_mppt}$$

* **Ideal Module Tilt (Nested `SE` Logic):**
  $$inclinação = \begin{cases} 10, & \text{se } Lat < 10 \\ Lat, & \text{se } Lat \le 20 \\ Lat + 5, & \text{se } Lat \le 30 \\ Lat + 10, & \text{se } Lat \le 40 \\ Lat + 15, & \text{caso contrário} \end{cases}$$

### 3. Investment Breakdown (CAPEX)

* **Total Item Cost:** `custo_total_item = qtd · custo_unit`
* **Distributed Price:** `preço_distribuído = SOMA(custos_item)`
* **Material Value:** `SE(escolha=1; preço_distribuído; preço_concentrado)`
* **Freight / Transportation (Estimated at 10%):** `transporte = 0,1 · valor_material`
* **Total Investment:** `SOMA(valor_material; transporte; estudos; mão_obra)`
* **Cost per Watt-peak ($custo\_Wp$):** 
  $$custo\_Wp = \frac{\text{investimento}}{Pfv_{real}}$$

---

## 📈 Sheet: Payback

This sheet projects the cash flow over the years ($n$), taking into account module degradation, tariff adjustments, maintenance costs, and inverter replacements.

### 1. Auxiliary Tariff Variables
* **Effective TE:** `TE_ef = SE(grupo='A'; TE_A_cheio; TE_B)`
* **Effective TUSD:** `TUSD_ef = SE(grupo='A'; TUSD_A_cheio; TUSD_B)`

### 2. Annual Projections (Year $n$)

* **Energy Generation ($Energia_{(n)}$):**
  * **Year 1:** $Efv_{real} \cdot 12$
  * **Year 2:** $Efv_{real} \cdot 12 \cdot (1 - perda_1)$
  * **Year $n \ge 3$:** $Efv_{real} \cdot 12 \cdot (1 - perda_1 - (n - 2) \cdot perda_n)$

* **Consumption Cost Evolution ($custo\_cons_{(n)}$):**
  * **Year 1:** $TE_{ef} + TUSD_{ef}$
  * **Year $n > 1$:** $custo\_cons_{(n-1)} \cdot (1 + aum)$

* **Injection Cost Evolution ($custo\_inj_{(n)}$):**
  * **Year 1:** Depends on the selected regulatory scenario:
    * *Scenario 1:* $TE_{scee}$
    * *Scenario 2:* $TE_{scee} + TUSD_{scee} \cdot (1 - fioB\%_{TUSD})$
    * *Scenario 3:* $TE_{scee} + TUSD_{scee}$
  * **Year $n > 1$:** $custo\_inj_{(n-1)} \cdot (1 + aum)$

* **Annual Savings Generated ($Economia_{(n)}$):**
  $$Economia_{(n)} = (custo\_cons \cdot autoc + (1 - autoc) \cdot custo\_inj) \cdot Energia$$

* **Maintenance and O&M Costs ($Manutenção_{(n)}$):**
  Includes inverter replacement forecasting during its lifespan years (`vida_inv`):
  $$Manutenção_{(n)} = \begin{cases} Economia \cdot (m\% + inv\%), & \text{se } n = \text{vida\_inv} \text{ ou } n = 2 \cdot \text{vida\_inv} \\ Economia \cdot m\%, & \text{caso contrário} \end{cases}$$

### 3. Financial Indicators and Cash Flow

* **Net Cash Flow ($Fluxo_{(n)}$):**
  * **Year 0:** $-\text{investimento}$
  * **Year $n \ge 1$:** 
    $$Fluxo_{(n)} = Economia - Manutenção - \text{SE}(grupo='A'; Dem_{ger} \cdot TUSDg \cdot 12; 0)$$

* **Accumulated Cash Flow:** `Acumulado(n) = Acumulado(n-1) + Fluxo`
* **Discounted Cash Flow ($Fluxo\_desc_{(n)}$):**
  $$Fluxo\_desc_{(n)} = \frac{Fluxo}{(1 + i_{desc})^n}$$
* **Accumulated Discounted Cash Flow:** `Fluxo_desc_acum(n) = Fluxo_desc_acum(n-1) + Fluxo_desc`

#### Final Viability Metrics
> 📊 **VPL (Net Present Value - NPV):** `=SOMA(Fluxo_desc)`
> 
> 📈 **TIR (Internal Rate of Return - IRR):** `=TIR(Fluxos)`

---
*This file serves as engineering documentation to validate the business rules implemented within this repository's spreadsheets.*
