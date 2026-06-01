# Photovoltaic Dimensioning Spreadsheet

**[2026]** · *PV · Excel · PVsyst · Tariffs · Payback · Brazil*

After completing the *Curso Solar* taught by Prof. Dr. Elmer Cari (USP), I built my
own photovoltaic payback spreadsheet, using a model from the course as a starting
point and developing it further with what I learned and my Excel skills. I extended
it to cover Brazilian **Group A** customers as well, not only Group B.

## Procedure

Based on the *Curso Solar* by Prof. Dr. Elmer Pablo Tito Cari (EESC–USP), I used my
Excel skills and my knowledge of Brazil's tariffs and charges to expand the original
spreadsheet, taking it from Group B only to Group A as well, applying the correct
taxes for a more detailed and accurate analysis.

First I dimension the photovoltaic system, then calculate its cost, and use economic
factors to adjust the payback curve over the years. It could be expanded — as the
Group B version already is — to include year-by-year PV generation, and to calculate
payback under a financing/loan scenario (WIP).

### How to Use

Choose the customer group, fill in the inputs, and select your project sizing by
entering the correct values in the green cells.

### Additions and Improvements

- The spreadsheet and its content are original; they are only *based on* the
  professor's model, his teaching, and my own studies.
- Removed ICMS (not applied in 2026; under reform).
- Removed the Energy Availability Cost (not relevant to solar without BESS; minor
  effect, more significant with high self-consumption).
- Added a separation between consumed and injected energy, producing a more realistic
  economic analysis with more accurate cost calculations.
- Changed from Group B only to Group A or B.
- Added Group A energy costs and contracted generation demand, including the
  calculation of the optimal generation demand and all Group A dynamics.
- Added the **Fio B** charge, the current tax on injected energy.
- Added clearer explanations for each cell.
- Added scenarios for the future of energy taxation following ANEEL's resolution —
  3 fixed scenarios and one customizable.
- Restructured poorly formatted cells and text.
- Added a self-consumption factor and estimate for better sizing of energy-cost
  compensation.
- Greatly improved formatting and visuals over the original base, including auxiliary
  cells for easier editing and for generating new graphs.
- Prof. version: 7.3; my version: 4th.

### Credits

The dimensioning methodology is based on the educational material from the
Photovoltaic Systems Course at EESC–USP (School of Engineering of São Carlos,
University of São Paulo), coordinated by Prof. Elmer P. T. Cari. The sizing equations
follow established industry practices and Brazilian technical standards
(ABNT NBR 16690, Law 14.300/2022).

This spreadsheet was developed independently, extending the original into a more
complete model with Group A analysis and more precise modeling, all under
Law 14.300/2022.

### Disclaimer

This tool is intended for educational and preliminary study purposes only. It does
not replace a detailed executive project. Real installations require a project signed
by a licensed engineer (with ART/CREA registration in Brazil, or equivalent
certification elsewhere) and formal approval by the local distribution utility.

Results are estimates based on simplified models and should be validated with
dedicated simulation software (e.g., PVsyst) and current local tariff and regulatory
data.

### References

- **Curso Solar** (EESC–USP) — Prof. Dr. Elmer P. T. Cari, Photovoltaic Systems
  dimensioning course; original spreadsheet model and methodology.
- **Tariff classes — lectures and exercises** — Prof. Dr. José Carlos de Melo.
  Vieira Jr. (EESC–USP).
- ABNT NBR 16690 — *Instalações elétricas de arranjos fotovoltaicos*.
- Law 14.300/2022 — Marco Legal da Geração Distribuída.
- ANEEL — regulations on charges and energy compensation.
- Joi Energês. *[DEMANDA DE GERAÇÃO: O que fazer?](https://www.youtube.com/watch?v=shWBmDaj6Do)* (YouTube), TUSDg.
- EC 132/2023 & LC 214/2025 — tax reform (IBS/CBS), 2026–2033 transition.
- [Instituto Solar — Como Calcular o Valor do Fio B para Energia Solar](https://institutosolar.com/como-calcular-o-valor-do-fio-b-para-energia-solar/) (2023)

