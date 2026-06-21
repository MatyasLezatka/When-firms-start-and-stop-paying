# Feature Variable Documentation

**Dividend Initiation and Omission Prediction — Compustat Constructions**
>References behind the variable selection and Compustat-based feature construction.

---

## 1. Life-Cycle & Maturity

| Variable | Compustat Formula | Source |
|---|---|---|
| `RE_TE` | `re / ceq` | DeAngelo et al. (2006) |
| `RE_TA` | `re / at` | DeAngelo et al. (2006); Kale et al. (2012) |
| `TE_TA` | `ceq / at` | DeAngelo et al. (2006) |
| `Size` | `log(at)` | DeAngelo et al. (2006); Huang & Paul (2024); Fama & French (2001) |
| `Log_MarketCap` | `log(prcc_f × csho)` | Huang & Paul (2024) |
| `Sales_Growth` | `(sale − lagged_sale) / lagged_sale` | DeAngelo et al. (2006) |
| `Asset_Growth` | `(at − lagged_at) / lagged_at` | DeAngelo et al. (2006); Fama & French (2001) |
| `Listing_Age` | `current_fyear − first_fyear` | Herron & Izhakian (2025) |

---

## 2. Growth Options & Investment Opportunities

| Variable | Compustat Formula | Source |
|---|---|---|
| `Market_to_Book` | `(at − ceq + prcc_f × csho) / at` | Fama & French (2001) |
| `CapEx_to_Assets` | `capx / at` | Kale et al. (2012); Grullon et al. (2002) |
| `RD_assets` | `xrd / at` (missing → 0) | Kale et al. (2012) |
| `Price_to_Sales` | `(prcc_f × csho) / sale` | Ivascu (2024) |

---

## 3. Signaling & Profitability

| Variable | Compustat Formula | Source |
|---|---|---|
| `ROA` | `ni / at` (or `ib / at`) | Smith & Pennathur (2019); DeAngelo et al. (2006); Huang & Paul (2024) |
| `ROE` | `ni / ceq` | Ivascu (2024) |
| `OpCF_to_Assets` | `oancf / at` | Anderson et al. (2022) |
| `ORA` | `oibdp / at` | Kale et al. (2012) |
| `Profit_Margin` | `ib / sale` | Smith & Pennathur (2019) |
| `Accruals` | `(ib − oancf) / at` | Smith & Pennathur (2019) |

---

## 4. Free Cash Flow & Agency

| Variable | Compustat Formula | Source |
|---|---|---|
| `Cash_to_Assets` | `che / at` | DeAngelo et al. (2006); Smith & Pennathur (2019); Huang & Paul (2024) |
| `FCF_to_Assets` | `(oancf − capx) / at` | Jensen (1986); Kumar et al. (2022) |
| `Repurchase_to_Assets` | `prstkc / at` | Carl et al. (2025) |
| `Goodwill_to_Assets` | `gdwl / at` | Jensen (1986) |

---

## 5. Conservatism & Risk

| Variable | Compustat Formula | Source |
|---|---|---|
| `Leverage` | `(dltt + dlc) / at` | Fama & French (2001); Huang & Paul (2024) |
| `LTDA` | `dltt / at` | Kale et al. (2012) |
| `Current_Ratio` | `act / lct` | Ivascu (2024) |
| `Earnings_Volatility` | Rolling 3-year SD of `ni / at` | Michaely & Moin (2022) |
| `Interest_Coverage` | `ebit / xint` | Amini et al. (2021) |
| `FAT` | `(dlc + 0.5 × dltt) / at` | Tian et al. (2015) |
| `LCTAT` | `lct / at` | Tian et al. (2015) |
| `Labor_Intensity` | `emp / ppent` | Carl et al. (2025) |

---

## 6. Corporate Taxation

| Variable | Compustat Formula | Source |
|---|---|---|
| `GAAP_ETR` | `txt / pi` | Anderson et al. (2022) |
| `Cash_ETR` | `txpd / (pi − spi)` | Anderson et al. (2022) |

---

## 7. Transaction Cost Theory

| Variable | Compustat Formula | Source |
|---|---|---|
| `Share_Turnover` | `cshtr_f / csho` | Kale et al. (2012) |

---

## References

Amini, Shahram, Ryan Elmore, Özde Öztekin, and Jack Strauss. 2021. "Can Machines Learn Capital Structure Dynamics?" *Journal of Corporate Finance* 70: 102073.

Anderson, Mark, Muhammad Kabir, Harun Rashid, and Hussein Warsame. 2022. "Corporate Dividend Policy and Tax Avoidance." *Canadian Tax Journal* 70: 747.

Carl, Casimir, Peter Limbach, and Florencio Lopez-de Silanes. 2025. Corporate Income Taxes and Payout Policy: Evidence from US State-Level Tax Changes. SSRN Working Paper 5106131.

DeAngelo, Harry, Linda DeAngelo, and René M. Stulz. 2006. "Dividend Policy and the Earned/Contributed Capital Mix: A Test of the Life-Cycle Theory." *Journal of Financial Economics* 81(2): 227–254.

Fama, Eugene F., and Kenneth R. French. 2001. "Disappearing Dividends: Changing Firm Characteristics or Lower Propensity to Pay?" *Journal of Financial Economics* 60(1): 3–43.

Grullon, Gustavo, Roni Michaely, and Bhaskaran Swaminathan. 2002. "Are Dividend Changes a Sign of Firm Maturity?" *Journal of Business* 75(3): 387–424.

Herron, Richard, and Yehuda Izhakian. 2025. "Ambiguity, Risk, and Payout Policy." SSRN Working Paper 2980600.

Huang, Wei, and Donna L. Paul. 2024. "Industry Effects in the Dividend Initiation Decision." *Journal of Accounting and Finance* 24(4): 61–77.

Ivascu, Codrut-Florin. 2024. "Understanding Dividend Puzzle Using Machine Learning." *Computational Economics* 64(1): 161–179.

Jensen, Michael C. 1986. "Agency Costs of Free Cash Flow, Corporate Finance, and Takeovers." *American Economic Review* 76(2): 323–329.

Kale, Jayant R., Omesh Kini, and Jan D. Payne. 2012. "The Dividend Initiation Decision of Newly Public Firms: Some Evidence on Signaling with Dividends." *Journal of Financial and Quantitative Analysis* 47(2): 365–396.

Kumar, Alok & Lei, Zicheng & Zhang, Chendi, 2022. "Dividend sentiment, catering incentives, and return predictability," *Journal of Corporate Finance, Elsevier*, vol. 72(C).

Michaely, Roni, and Amani Moin. 2022. "Disappearing and Reappearing Dividends." *Journal of Financial Economics* 143(1): 207–226.

Smith, D. D., and Anand K. Pennathur. 2019. "Signaling Versus Free Cash Flow Theory: What Does Earnings Management Reveal About Dividend Initiation?" *Journal of Accounting, Auditing & Finance* 34(2): 284–308.

Tian, Shaonan, Yan Yu, and Hui Guo. 2015. "Variable Selection and Corporate Bankruptcy Forecasts." *Journal of Banking & Finance* 52: 89–100.
