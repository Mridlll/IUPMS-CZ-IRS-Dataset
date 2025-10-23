# Data Dictionary: IUPMS-CZ-IRS Merged Panel Dataset

**Dataset**: US Commuting Zone Panel (1980-2023)
**Author**: Mridul Khanna, Heidelberg University
**Version**: 1.0

---

## Variable List (72 variables)

### **Panel Identifiers**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `year` | int | Calendar year | YYYY | - | 1980-2023 |
| `cz` | float | Commuting Zone code (1990 definition) | - | Tolbert & Sizer (1996) | All |

---

### **Geographic Coordinates**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `lon` | float | Longitude of CZ centroid | Decimal degrees | IPUMS USA | All |
| `lat` | float | Latitude of CZ centroid | Decimal degrees | IPUMS USA | All |

---

### **Demographics (by Age and Skill)**

Population counts by age group and skill level:

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `youth_high` | float | Youth (age 0-24) with high skill | Persons | IPUMS USA | 1980-2023 |
| `youth_low` | float | Youth (age 0-24) with low skill | Persons | IPUMS USA | 1980-2023 |
| `young_high` | float | Young adults (age 25-49) with high skill | Persons | IPUMS USA | 1980-2023 |
| `young_low` | float | Young adults (age 25-49) with low skill | Persons | IPUMS USA | 1980-2023 |
| `old_high` | float | Older adults (age 50+) with high skill | Persons | IPUMS USA | 1980-2023 |
| `old_low` | float | Older adults (age 50+) with low skill | Persons | IPUMS USA | 1980-2023 |
| `total_pop` | float | Total population | Persons | IPUMS USA | 1980-2023 |

**Skill Definition**:
- **High skill** = Bachelor's degree or higher
- **Low skill** = Less than bachelor's degree

**Age Groups**:
- **Youth**: 0-24 years
- **Young**: 25-49 years
- **Old**: 50+ years

---

### **Demographic Shares**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `share_old` | float | Share of population age 50+ | 0-1 | Derived | 1980-2023 |
| `share_young` | float | Share of population age 0-49 | 0-1 | Derived | 1980-2023 |
| `share_high_skill` | float | Share of adults (25+) with BA+ | 0-1 | Derived | 1980-2023 |
| `share_low_skill` | float | Share of adults (25+) without BA | 0-1 | Derived | 1980-2023 |

**Formula**:
- `share_old = (old_high + old_low) / total_pop`
- `share_high_skill = (young_high + old_high) / (young_high + young_low + old_high + old_low)`

---

### **Labor Market Outcomes**

#### **Wages**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `annual_wage_high` | float | Mean annual wage for high-skill workers | 2023 USD | QCEW/IPUMS USA | 1980-2023 |
| `annual_wage_low` | float | Mean annual wage for low-skill workers | 2023 USD | QCEW/IPUMS USA | 1980-2023 |
| `hourly_wage_high` | float | Mean hourly wage for high-skill workers | 2023 USD/hour | QCEW/IPUMS USA | 1980-2023 |
| `hourly_wage_low` | float | Mean hourly wage for low-skill workers | 2023 USD/hour | QCEW/IPUMS USA | 1980-2023 |
| `skill_wage_premium` | float | High-skill wage premium | Ratio | Derived | 1980-2023 |

**Wage Adjustments**:
- All wages adjusted to 2023 dollars using CPI-U
- Skill groups based on educational attainment (BA+ vs. less than BA)

**Formula**:
- `skill_wage_premium = hourly_wage_high / hourly_wage_low`

#### **Employment**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `employed_pop_high` | float | Employed population, high-skill | Persons | IPUMS USA | 1980-2023 |
| `employed_pop_low` | float | Employed population, low-skill | Persons | IPUMS USA | 1980-2023 |
| `total_emp` | float | Total employment (all industries) | Persons | CBP | 1980-2023 |

---

### **Housing Market**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `mean_rent` | float | Mean gross rent (contract rent + utilities) | 2023 USD/month | IPUMS USA | 1980-2023 |
| `n_renters` | float | Number of renter-occupied households | Households | IPUMS USA | 1980-2023 |

**Rent Adjustments**:
- Adjusted to 2023 dollars using CPI-U for Housing

---

### **Environmental Quality**

#### **PM2.5 (Fine Particulate Matter)**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `pm25_mean` | float | Annual mean PM2.5 concentration (observed) | μg/m³ | EPA AQS | 2000-2023 |
| `pm25_n_monitors` | float | Number of PM2.5 monitors in CZ | Count | EPA AQS | 2000-2023 |
| `pm25_imputed` | float | Imputed PM2.5 for missing CZ-years | μg/m³ | Derived | 2000-2023 |
| `imputation_method` | str | Method used for imputation | - | Derived | 2000-2023 |
| `pm25_final` | float | PM2.5 (observed or imputed) | μg/m³ | Derived | 2000-2023 |
| `pm25_is_imputed` | bool | Flag: True if PM2.5 is imputed | 0/1 | Derived | 2000-2023 |

**PM2.5 Notes**:
- EPA monitoring began 1999, limited coverage until mid-2000s
- `pm25_mean`: Directly from EPA monitors (NaN if no monitors in CZ)
- `pm25_final`: Combines observed + imputed values for complete coverage
- Imputation uses inverse distance weighting from nearby CZs

**Health Context**:
- EPA standard: 12 μg/m³ annual mean (as of 2012)
- WHO guideline: 5 μg/m³ annual mean (as of 2021)

#### **Ozone**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `ozone_mean` | float | Annual mean ozone concentration | ppm (parts per million) | EPA AQS | 1980-2023 |
| `ozone_n_obs` | float | Number of ozone observations | Count | EPA AQS | 1980-2023 |

**Ozone Notes**:
- Longer monitoring history than PM2.5
- Typically measured as 8-hour daily maximum

---

### **Migration Flows (IRS Data)**

#### **Inflows**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `in_returns` | float | Number of tax returns moving into CZ | Returns | IRS SOI | 1990-2023 |
| `in_exemptions` | float | Number of exemptions moving into CZ | Exemptions | IRS SOI | 1990-2023 |
| `in_agi` | float | Total adjusted gross income moving in | 2023 USD | IRS SOI | 1990-2023 |

#### **Outflows**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `out_returns` | float | Number of tax returns moving out of CZ | Returns | IRS SOI | 1990-2023 |
| `out_exemptions` | float | Number of exemptions moving out of CZ | Exemptions | IRS SOI | 1990-2023 |
| `out_agi` | float | Total adjusted gross income moving out | 2023 USD | IRS SOI | 1990-2023 |

#### **Net Flows**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `net_returns` | float | Net inflow of tax returns (in - out) | Returns | Derived | 1990-2023 |
| `net_exemptions` | float | Net inflow of exemptions (in - out) | Exemptions | Derived | 1990-2023 |
| `net_agi` | float | Net inflow of AGI (in - out) | 2023 USD | Derived | 1990-2023 |

**IRS Migration Notes**:
- Based on year-to-year changes in tax filing location
- **Exemptions** ≈ proxy for number of migrants (includes dependents)
- **Returns** = number of households/tax units
- AGI adjusted to 2023 dollars using CPI-U
- Data begins in 1990 (earlier years = NaN)

---

### **Industry Composition (County Business Patterns)**

#### **Aggregated Employment**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `total_emp` | float | Total employment across all industries | Persons | CBP | 1980-2023 |
| `total_est` | float | Total number of establishments | Count | CBP | 1980-2023 |
| `total_payroll` | float | Total annual payroll | 2023 USD | CBP | 1980-2023 |

#### **Employment by Industry (NAICS 2-digit sectors)**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `accommodation_food` | float | Accommodation and Food Services (NAICS 72) | Persons | CBP | 1980-2023 |
| `administrative` | float | Administrative and Support Services (NAICS 56) | Persons | CBP | 1980-2023 |
| `agriculture` | float | Agriculture, Forestry, Fishing (NAICS 11) | Persons | CBP | 1980-2023 |
| `arts_entertainment` | float | Arts, Entertainment, Recreation (NAICS 71) | Persons | CBP | 1980-2023 |
| `construction` | float | Construction (NAICS 23) | Persons | CBP | 1980-2023 |
| `education` | float | Educational Services (NAICS 61) | Persons | CBP | 1980-2023 |
| `finance` | float | Finance and Insurance (NAICS 52) | Persons | CBP | 1980-2023 |
| `health_care` | float | Health Care and Social Assistance (NAICS 62) | Persons | CBP | 1980-2023 |
| `information` | float | Information (NAICS 51) | Persons | CBP | 1980-2023 |
| `management` | float | Management of Companies (NAICS 55) | Persons | CBP | 1980-2023 |
| `manufacturing` | float | Manufacturing (NAICS 31-33) | Persons | CBP | 1980-2023 |
| `mining` | float | Mining, Quarrying, Oil/Gas Extraction (NAICS 21) | Persons | CBP | 1980-2023 |
| `other_services` | float | Other Services (NAICS 81) | Persons | CBP | 1980-2023 |
| `professional_services` | float | Professional, Scientific, Technical (NAICS 54) | Persons | CBP | 1980-2023 |
| `real_estate` | float | Real Estate, Rental, Leasing (NAICS 53) | Persons | CBP | 1980-2023 |
| `retail` | float | Retail Trade (NAICS 44-45) | Persons | CBP | 1980-2023 |
| `transportation` | float | Transportation and Warehousing (NAICS 48-49) | Persons | CBP | 1980-2023 |
| `utilities` | float | Utilities (NAICS 22) | Persons | CBP | 1980-2023 |
| `wholesale` | float | Wholesale Trade (NAICS 42) | Persons | CBP | 1980-2023 |

**Industry Notes**:
- Employment counts from County Business Patterns
- Aggregated from county to CZ using spatial crosswalks
- Some values may be suppressed in small CZs (disclosure avoidance)
- NAICS classification system (post-1997); earlier years use SIC codes converted to NAICS

#### **Industry Shares**

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `high_skill_share` | float | Employment share in high-skill industries | 0-1 | Derived | 1980-2023 |
| `pollution_share` | float | Employment share in pollution-intensive industries | 0-1 | Derived | 1980-2023 |

**Definitions**:
- **High-skill industries**: Professional services, finance, information, management, education
- **Pollution-intensive industries**: Manufacturing, mining, utilities, agriculture, construction

---

### **Log-Transformed Variables**

For econometric analysis, key variables are provided in natural logarithm form:

| Variable | Type | Description | Units | Source | Years |
|----------|------|-------------|-------|--------|-------|
| `log_total_pop` | float | ln(total population) | - | Derived | 1980-2023 |
| `log_pm25_mean` | float | ln(PM2.5 concentration) | - | Derived | 2000-2023 |
| `log_ozone_mean` | float | ln(Ozone concentration) | - | Derived | 1980-2023 |
| `log_mean_rent` | float | ln(mean gross rent) | - | Derived | 1980-2023 |
| `log_hourly_wage_high` | float | ln(high-skill hourly wage) | - | Derived | 1980-2023 |
| `log_hourly_wage_low` | float | ln(low-skill hourly wage) | - | Derived | 1980-2023 |
| `log_total_emp` | float | ln(total employment) | - | Derived | 1980-2023 |

**Notes**:
- Logs are natural logarithms (base e)
- Missing or zero values result in NaN for log variables
- Useful for elasticity interpretation and reducing skewness

---

## Data Types Summary

| Type | Count | Examples |
|------|-------|----------|
| **Identifiers** | 2 | year, cz |
| **Geographic** | 2 | lat, lon |
| **Demographics** | 11 | Population by age/skill, shares |
| **Labor Market** | 7 | Wages, employment, skill premium |
| **Housing** | 2 | Rent, renters |
| **Environment** | 8 | PM2.5, ozone, monitors |
| **Migration** | 9 | IRS inflows, outflows, net flows |
| **Industry** | 22 | Total + 19 sectors + shares |
| **Derived (logs)** | 7 | Log-transformed variables |
| **Flags** | 2 | pm25_is_imputed, imputation_method |
| **Total** | 72 | - |

---

## Missing Data Codes

- **NaN / NULL**: Missing or not available
- **0**: True zero (e.g., zero employment in a sector)

**Common reasons for missing data**:
1. **Pre-monitoring period**: PM2.5 before 2000, IRS migration before 1990
2. **Disclosure suppression**: Small CZs with few establishments (CBP privacy rules)
3. **Data quality**: Insufficient monitors, unreliable estimates
4. **CZ coverage**: Some variables not available for Alaska/Hawaii in certain years

---

## Recommended Filters for Analysis

### **Baseline Analysis Sample** (Post-2000, Complete PM2.5)

```python
df_analysis = df[
    (df['year'] >= 2000) &
    (df['pm25_final'].notna()) &
    (df['total_pop'] > 0) &
    (df['mean_rent'] > 0)
].copy()
```

### **Migration Analysis** (IRS Data Available)

```python
df_migration = df[
    (df['year'] >= 1990) &
    (df['net_returns'].notna())
].copy()
```

### **Long-Run Analysis** (Full Time Series)

```python
df_longrun = df[
    (df['total_pop'] > 0) &
    (df['total_emp'] > 0)
].copy()
```

---

## Variable Relationships

### **Identities**

```
total_pop = youth_high + youth_low + young_high + young_low + old_high + old_low
share_high_skill + share_low_skill = 1
share_old + share_young = 1
net_returns = in_returns - out_returns
skill_wage_premium = hourly_wage_high / hourly_wage_low
pm25_final = pm25_mean (if observed) OR pm25_imputed (if missing)
```

### **Derived Shares**

```
high_skill_share = (professional_services + finance + information + management + education) / total_emp
pollution_share = (manufacturing + mining + utilities + agriculture + construction) / total_emp
```

---

## Quality Checks

### **Recommended Data Quality Checks**

1. **Population Consistency**:
   ```python
   # Check population adds up from components
   df['pop_check'] = (df['youth_high'] + df['youth_low'] +
                      df['young_high'] + df['young_low'] +
                      df['old_high'] + df['old_low'])
   assert (df['pop_check'] - df['total_pop']).abs().max() < 1
   ```

2. **Share Bounds**:
   ```python
   # All shares should be in [0, 1]
   share_vars = ['share_old', 'share_young', 'share_high_skill', 'share_low_skill']
   assert df[share_vars].min().min() >= 0
   assert df[share_vars].max().max() <= 1
   ```

3. **Wage Premium**:
   ```python
   # Skill premium should generally be > 1
   assert (df['skill_wage_premium'] >= 1).mean() > 0.95
   ```

---

## Updates and Corrections

If you discover data issues or have suggestions for improvements:
- **Open an issue**: https://github.com/Mridlll/Environmental-OGS/issues
- **Email author**: Contact via Heidelberg University

---

## Version History

**Version 1.0** (October 2025):
- Initial release with 72 variables
- Coverage: 741 CZs, 1980-2023
- 6,422 CZ-year observations

---

**Last Updated**: October 24, 2025
**Author**: Mridul Khanna, Heidelberg University
