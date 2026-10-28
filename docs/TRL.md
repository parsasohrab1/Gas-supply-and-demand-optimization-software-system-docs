# TRL assessment (evidence-based)

**Level reached: TRL 4 for the individual components, on SYNTHETIC data only.** TRL 5 is not claimed.

`pytest` (CI on every push) runs 6 tests:

| Component | Module | Validated against |
|---|---|---|
| Balance model as a linear programme (production with turnaround losses, storage, import/export, four sectors, liquid fuel in power plants) | `optimize.solve_lp` | conservation, capacity limits, unserved non-power demand under 5 % (SRS objective 1) |
| Scenarios (demand scale, export ceiling, extra turnaround) | `problem.scenario` | monotonic effects (more export room never worsens the optimum, more turnaround never lowers liquid-fuel use) |
| Genetic algorithm on the same objective (SRS FR-12) | `optimize.solve_ga` | never better than the exact LP, within 10 % (+50) of it on the test instance |
| Seasonal demand forecaster (Fourier + heating-degree days; baseline) | `forecast.py` | hold-out year MAPE under 10 % (SRS: accuracy above 90 %) |

## Not done (and why)
- All magnitudes are **illustrative synthetic numbers**, not the Iranian network. Results show the code is correct, not that it balances the real grid.
- **No network topology or pressure constraints** (FR-09 mentions line capacity, pressure, quality): the model is a monthly mass balance only.
- The **LSTM** forecaster (FR-13) is not implemented; a seasonal regression is the baseline. Uncertainty modelling (FR-10) and sensitivity analysis (FR-14) are not implemented beyond what-if scenarios.
- The hybrid GA/PSO/ant-colony algorithm named in the differentiation table is **not implemented** (single GA only), and runtime for the national annual plan (NFR-02) is not measured.

## Next steps toward TRL 5
1. National balance data (production, storage, sector demand, turnaround plan, export/import) for 2-3 past years under a data-sharing agreement.
2. Backtest: reproduce the realised annual balance and compare liquid-fuel use with the Excel-based plan.
3. Add network constraints and the daily forecast with weather data.
