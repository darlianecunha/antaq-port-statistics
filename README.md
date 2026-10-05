# Brazil Port Data: consolidated ANTAQ port statistics, 2010-2026

**Annual cargo, berthings and port times for 247 Brazilian port installations (public ports and private terminals), aggregated from ANTAQ's Estatístico Aquaviário with the same filter as the agency's public panel, so national totals match the official figures.**

Version 1.0 · Darliane Ribeiro Cunha, PhD · [brazilportdata.com](https://brazilportdata.com) · ORCID [0000-0003-2548-1237](https://orcid.org/0000-0003-2548-1237)

## Why this dataset

ANTAQ publishes very detailed open data, but turning it into consistent annual series by installation takes work: choosing the movement filter, joining berthing and cargo tables, harmonising installation codes over 17 years. This dataset is that work, done once and documented, so that researchers, students and analysts can start from comparable series.

It is the data behind [brazilportdata.com](https://brazilportdata.com) and the MCP server [port-emissions-mcp](https://github.com/darlianecunha/port-emissions-mcp), which lets Claude answer questions about Brazilian ports from these tables.

## Files

All tables are in `data/`, UTF-8 CSV with a header row (one JSON file).

**Coverage:** 2010 to February 2026. Rows for 2026 are partial (January and February only) and must not be compared with full years.
**Extraction:** ANTAQ statistical panel (Aquarela), July 2026 (2010-2026 series) and 24 September 2026 (2021-2025 revision). ANTAQ revises recent months, so small differences against later extractions are expected.

| File | Grain | Columns |
|---|---|---|
| `cargo_by_installation_2010_2026.csv` | year × installation | `ano` (year), `complexo` (port complex), `codigo` (ANTAQ installation code, e.g. BRSSZ = Santos), `toneladas` (tonnes) |
| `cargo_by_navigation_2010_2026.csv` | year × installation | `longo_curso_t` (deep sea), `cabotagem_t` (cabotage), `vias_interiores_t` (inland waterway), tonnes |
| `cargo_by_navigation_detail_2010_2026.csv` | year × installation × navigation type | `navegacao` (Longo Curso, Cabotagem, Interior, Apoio Portuário, Apoio Marítimo), `toneladas` |
| `berthings_by_installation_2010_2026.csv` | year × installation | `org` (1 = public/organised port, 0 = private terminal, TUP), `atracacoes` (number of berthings) |
| `berthings_brazil_2010_2026.csv` | year | `tups`, `portos_organizados`, `total` berthings |
| `port_times_by_installation_2010_2026.csv` | year × installation | mean hours: `t_espera_atracacao_h` (anchorage to berth), `t_operacao_h` (operation), `t_atracado_h` (at berth), `t_estadia_h` (total stay) |
| `installations_2023_2025.csv` | installation | `tonnes_2023`, `tonnes_2024`, `tonnes_2025`, `berthings_2025` for the 208 installations with movement in 2023-2025 |
| `top20_installations_2021_2025.csv` | installation × year | tonnes and berthings for the 20 largest installations of 2025 |
| `brazil_totals_2021_2025.json` | year | national totals by cargo nature, direction, installation type, navigation type, destination state, and top-5 partner countries (exports by destination country, imports by origin country) |

Notes
- *Installation* is the individual port or terminal (ANTAQ code). *Complex* groups every installation of a port area (e.g. the Itaqui complex includes the public port of Itaqui, Ponta da Madeira and Alumar).
- Berthings count distinct berthing records with authorised cargo operations, so they are lower than ANTAQ's raw berthing counts, which include non-cargo calls.
- Partner-country tonnage covers deep-sea navigation only. "Brasil" appears as origin or destination for cabotage and inland flows and is excluded from the partner rankings.


## Source and licence

- **Source:** Agência Nacional de Transportes Aquaviários (ANTAQ), *Estatístico Aquaviário*, open data. https://web3.antaq.gov.br/ea/sense/download.html
- **This dataset** is a derived database, released under the **Open Database License (ODbL) 1.0**: you may share, adapt and use it, including commercially, provided you attribute it (citation below), keep derived databases under ODbL, and keep it open. Full text: https://opendatacommons.org/licenses/odbl/1-0/
- ANTAQ is not responsible for this aggregation. Errors in the aggregation are mine; please report them.

## How to cite

> Cunha, D. R. (2026). *Brazil Port Data: consolidated ANTAQ port statistics, 2010-2026* (Version 1.0) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.23158268
## Quick start

```python
import pandas as pd
cargo = pd.read_csv("data/cargo_by_installation_2010_2026.csv")
itaqui = cargo[cargo.codigo == "BRIQI"]          # public port of Itaqui
print(itaqui[itaqui.ano.between(2019, 2025)])  # 2026 is partial
```

With Claude: install [port-emissions-mcp](https://github.com/darlianecunha/port-emissions-mcp) and run `port-emissions-mcp download` to fetch this record.

## Related work

- [maritime-co2](https://doi.org/10.5281/zenodo.20708090): at-berth CO₂ of liquid-bulk vessels, IMO Fourth GHG Study 2020 method
- [port-emissions-mcp](https://github.com/darlianecunha/port-emissions-mcp): MCP server that combines this dataset with maritime-co2
