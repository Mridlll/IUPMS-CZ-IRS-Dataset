# IUPMS-CZ-IRS Merged Panel Dataset
**A Comprehensive US Commuting Zone Panel (1980-2023)**

**Author**: Mridul Khanna
**Affiliation**: Heidelberg University
**Last Updated**: October 2025
**Version**: 1.0

---

## Overview

This dataset merges multiple high-quality data sources to create a comprehensive panel of US Commuting Zones spanning over four decades. It combines demographic, economic, environmental, and migration data at the commuting zone level, enabling research on spatial economics, environmental sorting, labor markets, and migration patterns.

### Dataset Characteristics

- **Geographic Coverage**: 741 US Commuting Zones (1990 definition)
- **Time Period**: 1980-2023 (44 years)
- **Observations**: 6,422 CZ-year observations
- **Variables**: 72 variables across multiple domains
- **File Format**: CSV (comma-separated values)
- **File Size**: ~2 MB

---

## Citation

If you use this dataset in your research, please cite:

```
Khanna, Mridul (2025). "IUPMS-CZ-IRS Merged Panel Dataset:
A Comprehensive US Commuting Zone Panel (1980-2023)."
Heidelberg University. https://github.com/Mridlll/Environmental-OGS
```

---

## Data Sources

This dataset integrates the following authoritative data sources:

### 1. **IUPMS (Industry-based Urban Population Mobility System)**
- **Source**: County Business Patterns (CBP), Bureau of Labor Statistics
- **Variables**: Employment by industry sector, establishment counts, payroll
- **Coverage**: 1980-2023
- **Aggregation**: County-level data aggregated to CZ using population weights

### 2. **IRS Migration Data**
- **Source**: Internal Revenue Service, Statistics of Income Division
- **Variables**: Inflows, outflows, net migration (returns, exemptions, AGI)
- **Coverage**: 1990-2023
- **URL**: https://www.irs.gov/statistics/soi-tax-stats-migration-data

### 3. **EPA Air Quality System (AQS)**
- **Source**: Environmental Protection Agency
- **Variables**: PM2.5 concentration (annual mean), Ozone levels, monitor counts
- **Coverage**: 2000-2023 (PM2.5), 1980-2023 (Ozone)
- **URL**: https://aqs.epa.gov/aqsweb/airdata/

### 4. **IPUMS USA (Integrated Public Use Microdata Series)**
- **Source**: IPUMS USA, University of Minnesota
- **Variables**: Demographics, housing costs, population by age/skill
- **Coverage**: 1980-2023
- **Data**: Census and American Community Survey microdata
- **Citation**: Steven Ruggles, Sarah Flood, Matthew Sobek, et al. IPUMS USA: Version 15.0 [dataset]. Minneapolis, MN: IPUMS, 2024. https://doi.org/10.18128/D010.V15.0
- **URL**: https://usa.ipums.org/usa/

### 5. **Quarterly Census of Employment and Wages (QCEW)**
- **Source**: Bureau of Labor Statistics
- **Variables**: Average wages by skill group, hourly wages
- **Coverage**: 1980-2023

---

## Variable Categories

### **Geographic Identifiers**
- `cz`: Commuting Zone code (1990 definition)
- `lon`: Longitude (CZ centroid)
- `lat`: Latitude (CZ centroid)

### **Demographics (by Skill Group)**
- Population by age and skill: `youth_high`, `youth_low`, `young_high`, `young_low`, `old_high`, `old_low`
- Total population: `total_pop`
- Population shares: `share_old`, `share_young`, `share_high_skill`, `share_low_skill`

### **Labor Market**
- Wages: `annual_wage_high`, `annual_wage_low`, `hourly_wage_high`, `hourly_wage_low`
- Employment: `employed_pop_high`, `employed_pop_low`, `total_emp`
- Skill premium: `skill_wage_premium`

### **Housing**
- `mean_rent`: Average rent (nominal dollars)
- `n_renters`: Number of renter households

### **Environmental Quality**
- `pm25_mean`: Annual mean PM2.5 concentration (μg/m³)
- `pm25_n_monitors`: Number of EPA monitors
- `pm25_final`: PM2.5 with imputation for missing values
- `pm25_is_imputed`: Flag for imputed values
- `ozone_mean`: Annual mean ozone concentration (ppm)

### **Migration (IRS Data)**
- Inflows: `in_returns`, `in_exemptions`, `in_agi`
- Outflows: `out_returns`, `out_exemptions`, `out_agi`
- Net flows: `net_returns`, `net_exemptions`, `net_agi`

### **Industry Composition (IUPMS)**
- Aggregated: `total_emp`, `total_est`, `total_payroll`
- By sector: `accommodation_food`, `administrative`, `agriculture`, `arts_entertainment`,
  `construction`, `education`, `finance`, `health_care`, `information`, `management`,
  `manufacturing`, `mining`, `other_services`, `professional_services`, `real_estate`,
  `retail`, `transportation`, `utilities`, `wholesale`
- Industry shares: `high_skill_share`, `pollution_share`

### **Derived Variables (Log-transformed)**
- `log_total_pop`, `log_pm25_mean`, `log_ozone_mean`, `log_mean_rent`,
  `log_hourly_wage_high`, `log_hourly_wage_low`, `log_total_emp`

---

## Data Quality Notes

### **Missing Data Patterns**

1. **PM2.5 (1980-1999)**: No EPA monitoring data available
   - PM2.5 monitoring began in 1999 with limited coverage
   - Full coverage achieved by mid-2000s
   - Imputed values available in `pm25_final` (see `pm25_is_imputed` flag)

2. **IRS Migration (pre-1990)**: Limited availability
   - IRS SOI migration data begins in 1990
   - Earlier years may have NaN values

3. **Industry Detail**: Varies by year and CZ size
   - Small CZs may have suppressed values for privacy (County Business Patterns disclosure rules)
   - Industry composition more reliable for larger CZs

### **Imputation Methods**

For PM2.5, missing values are imputed using:
- Spatial interpolation (inverse distance weighting from nearby CZs)
- Temporal interpolation (linear interpolation across years)
- See `imputation_method` variable for details

### **Commuting Zone Definition**

This dataset uses the **1990 Commuting Zone definition** from Tolbert and Sizer (1996):
- 741 CZs covering the entire United States
- Based on commuting patterns from 1990 Census
- Includes Alaska and Hawaii
- Reference: Tolbert, C.M., and Sizer, M. (1996). "U.S. Commuting Zones and Labor Market Areas."

---

## Usage Examples

### Load the Dataset (Python)

```python
import pandas as pd

# Load full dataset
df = pd.read_csv('data/processed/cz_panel.csv')

# Filter to recent years with complete PM2.5 coverage
df_recent = df[df['year'] >= 2000].copy()

# Create analysis sample (non-missing PM2.5, population > 0)
df_analysis = df_recent[
    (df_recent['pm25_final'].notna()) &
    (df_recent['total_pop'] > 0)
].copy()

print(f"Analysis sample: {len(df_analysis)} CZ-year observations")
```

### Load the Dataset (R)

```r
library(tidyverse)

# Load full dataset
df <- read_csv('data/processed/cz_panel.csv')

# Filter to recent years
df_recent <- df %>%
  filter(year >= 2000)

# Summary statistics
summary(df_recent %>% select(pm25_final, mean_rent, hourly_wage_high))
```

### Common Research Applications

**1. Environmental Sorting Analysis**
```python
# Estimate relationship between pollution and skill composition
import statsmodels.formula.api as smf

model = smf.ols('share_high_skill ~ log_pm25_mean + log_total_pop + C(year)',
                data=df_analysis)
results = model.fit(cov_type='cluster', cov_kwds={'groups': df_analysis['cz']})
print(results.summary())
```

**2. Migration Response to Pollution**
```python
# Analyze net migration flows and environmental quality
migration_model = smf.ols('net_returns ~ pm25_final + mean_rent + C(year)',
                          data=df_analysis[df_analysis['net_returns'].notna()])
results = migration_model.fit(cov_type='cluster',
                             cov_kwds={'groups': df_analysis['cz']})
```

**3. Industry Composition and Pollution**
```python
# Examine relationship between manufacturing share and PM2.5
industry_model = smf.ols('log_pm25_mean ~ manufacturing + C(year) + C(cz)',
                         data=df_analysis)
results = industry_model.fit()
```

---

## Data Structure

```
data/processed/
└── cz_panel.csv        # Main dataset file (6,422 rows × 72 columns)
```

### Panel Structure

The dataset is in **long format** (CZ-year):
- Each row represents one Commuting Zone in one year
- Panel is **unbalanced** due to missing data for some CZ-year combinations
- Use `year` and `cz` as panel identifiers

---

## License

This dataset is made available for **academic research and educational purposes**.

### Terms of Use

1. **Attribution Required**: Please cite this dataset as specified above
2. **Academic Use**: Free for research and educational purposes
3. **Commercial Use**: Contact author for permission
4. **Redistribution**: Allowed with attribution, but must reference this source
5. **No Warranty**: Data provided "as is" without warranty of any kind

### Source Data Licenses

This dataset integrates public domain and freely available data sources:
- **IPUMS USA**: Free for research use, requires citation (see above)
- **IRS Migration Data**: Public domain (US government work)
- **EPA Air Quality Data**: Public domain (US government work)
- **County Business Patterns**: Public domain (US government work)
- **QCEW**: Public domain (US government work)

---

## Contact

**Author**: Mridul Khanna
**Email**: Available upon request
**Institution**: Heidelberg University
**GitHub**: https://github.com/Mridlll/Environmental-OGS

For questions, bug reports, or data requests:
- Open an issue on GitHub: https://github.com/Mridlll/Environmental-OGS/issues
- Or contact via university email

---

## Version History

### Version 1.0 (October 2025)
- Initial public release
- 741 CZs, 1980-2023
- 72 variables across demographics, labor, environment, migration, and industry
- PM2.5 imputation for missing values
- Complete documentation and data dictionary

---

## Acknowledgments

This dataset was created as part of masters research at Heidelberg University on environmental sorting and spatial inequality. Special thanks to:

- **Data Providers**: IPUMS USA (University of Minnesota), US Census Bureau, EPA, IRS, Bureau of Labor Statistics
- **CZ Definitions**: Tolbert and Sizer (1996), David Dorn's crosswalk files
- **Research Support**: Heidelberg University Department of Economics

**IPUMS Citation**: Steven Ruggles, Sarah Flood, Matthew Sobek, Danika Brockman, Grace Cooper, Stephanie Richards, and Megan Schouweiler. IPUMS USA: Version 15.0 [dataset]. Minneapolis, MN: IPUMS, 2024. https://doi.org/10.18128/D010.V15.0

---

## Related Resources

- **IPUMS USA**: https://usa.ipums.org/usa/
- **Commuting Zone Crosswalks**: http://www.ddorn.net/data.htm
- **EPA Air Quality Data**: https://aqs.epa.gov/aqsweb/airdata/
- **IRS Migration Data**: https://www.irs.gov/statistics/soi-tax-stats-migration-data
- **County Business Patterns**: https://www.census.gov/programs-surveys/cbp.html

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/Mridlll/Environmental-OGS.git
cd Environmental-OGS

# Load the dataset
python
>>> import pandas as pd
>>> df = pd.read_csv('data/processed/cz_panel.csv')
>>> df.info()
```

**Happy researching!**
