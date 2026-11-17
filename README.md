# OLS Diagnostics: Robust and Clustered Standard Errors

## Objective

I measured how often nominal 95% confidence intervals for a regression slope actually contain the true value under heteroskedasticity and under errors shared within firms, and checked an AI-written function that builds a table of standard errors against statsmodels.

## Methodology

- Simulated 500 samples with error spread growing with x, and counted how often classical, HC1 and HC3 intervals for the slope contained the true value of 2.0.
- Simulated 300 panels of 40 firms with 25 employees each, with a shock shared within each firm, and counted coverage of the true slope of 1.5 for classical, HC3 and firm-clustered standard errors.
- Fitted one regression (y on x and tenure) on the analyst's file with missing values, using 960 of 1,000 rows, and compared six standard errors: classical, HC0, HC1, HC2, HC3 and clustered by firm.
- Planted three rows (A, B and C) in the panel and computed leverage, studentised residuals and Cook's distance, then counted rows flagged by Cook's distance above 4/n and leverage above 3p/n.
- Wrote my own prompt asking an AI for a function, `se_table`, that returns the 18 standard errors (3 coefficients by 6 types), and checked every number against statsmodels, including a run with every column renamed.

## Key Findings

- **Heteroskedastic errors:** coverage was 92.4% for classical standard errors, 94.2% for HC1 and 94.4% for HC3, against a nominal 95%. Classical's shortfall was about 2.2 Monte Carlo standard errors.
- **Errors shared within firms:** coverage was 49.0% for classical, 50.3% for HC3 and 93.7% when clustered by firm. Robust standard errors fix unequal variances but not dependence.
- **Clustering and the coefficients:** for x, which has a firm-level component, the clustered standard error was 0.08644 against 0.02576 classical. For tenure, which was drawn independently per employee, it was 0.00629 against 0.00643.
- **Influence:** of the three planted rows, only C (leverage 0.0229, studentised residual -12.0929, Cook's distance 1.4982) moved the slope, from 1.4927 to 1.4468. A has high leverage but lies near the line, and B has a large residual but low leverage. Cook's distance above 4/n flagged 44 rows and leverage above 3p/n flagged 28, so most flagged rows were not planted. Flags are a list to inspect, not to delete.
- **AI-written function:** I went 1 round, and all 18 standard errors matched statsmodels, with the renamed-column run also matching 18 of 18.
