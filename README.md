# Fear & Hunger Item Analysis - DSA210 Project

This project explores the strategic decision-making behind weapon selection and status effect management in the game *Fear & Hunger*. The core aim is to quantify the **usefulness of weapons** and the **danger level of status effects** by building structured datasets, applying data science techniques, and developing predictive models based on in-game factors.

Using a custom-built dataset with numerical features, I will score and analyze each weapon and status effect, visualize trends, and use clustering and regression to extract actionable insights for players.

---

*Fear & Hunger* is a punishing game where player success hinges on understanding obscure mechanics. With no built-in guidance, most decisions about which items to use or which effects to avoid are made through trial and error.

This project gives me the chance to combine:
- My interest in RNG based  survival games like *Fear & Hunger*
- A desire to turn hard-to-access game knowledge into helpful, data-driven resources for other players to use

---


The dataset is being created manually and sourced from:
- **Fear & Hunger Wiki**
- In-game observations from a Youtube walkthrough

### Weapons Dataset (`weapons.csv`)
| Column | Description |
|--------|-------------|
| Name | Weapon name |
| ATK | Attack value |
| #AltWeaponsLowerOrEqual | Number of weapons with lower or equal ATK |
| WeaponNumber | how many copies of this specific weapon can be obtained, calculated by guaranteed and enemy drops|
| AcquisitionType | `Guaranteed`, `RNG`, or `Enemy Drop` (one weapon can have multiple ways of obtaining it) | 
| NumEnemiesDrop | Number of enemies that drop this weapon |
| EnemyPopulationDrop     | Total number of individual enemies that can drop this weapon |
| NumRNGSpots | Number of RNG locations it can be found / totalRNGspots |
| NumGuaranteedSpots | Number of fixed spots it can be obtained from |
| UsefulnessScore | Calculated as: `#AltWeaponsLowerOrEqual + (NumEnemiesDrop × EnemyPopulationDrop) − (NumGuaranteedSpots + (NumRNGSpots/)`|

### Status Effects Dataset (`status_effects.csv`)
| Column | Description |
|--------|-------------|
| Name | Status effect name |
| NumWaysToGet | Total number of sources that apply it |
| NumCures | Number of known cures for the status effect |
| NumGuaranteedSpots | Number of curing items found as guaranteed |
| NumRNGSpots | Number of RNG locations that curing items can be found |
| CureAvailabilityScore | Calculated for each status as: NumGuaranteedSpots + (NumRNGSpots/NumCures)
| Uncurable | 1 if there is no known cure, 0 otherwise |
| DangerLevel | Calculated as: `(NumWaysToGet − CureAvailabilityScore)+(Uncurable * 20)` |

---

## Research Questions & Hypotheses

### RQ1: Which weapons are most useful?
- **H₀**: Weapon acquisition method and ATK value are not correlated with usefulness.
- **Hₐ**: Weapons with higher ATK and easier acquisition paths are significantly more useful.

### RQ2: Which status effects are the most dangerous?
- **H₀**: All status effects are equally dangerous.
- **Hₐ**: Status effects with fewer cures and higher frequency of application are more dangerous.

---

## Data Collection Plan
- Data will be logged manually in Excel using the fields above and more if necessary.
- Acquisition and application types will be categorized based on F&H wiki info and verified in-game if possible.
- Subjective scores will be calculated.

---

## Data Analysis Plan

### Exploratory Data Analysis
- Histograms of attack values and cure counts
- Correlation matrices between availability and usefulness
- Group comparisons (e.g. Guaranteed vs RNG weapons)

### Visualizations
- **Heatmaps** showing relationships between variables
- **Bar plots** ranking danger level and usefulness
- **Cluster plots** showing groupings of weapons/statuses

---

## Tools & Technologies
- **Python** for scripting and analysis
- **Pandas** and **NumPy** for data wrangling
- **Matplotlib** and **Seaborn** for visualizations
- **Excel** for initial data collection

---

## Expected Insights
- A ranked list of weapons by combined attack value and acquisition ease
- A ranked list of status effects by difficulty to cure and frequency
- Simple, visually intuitive guides that players can refer to for better decision-making

---

## Limitations & Future Work
- Some variables (like encounter rate) are subjective or hard to measure because the game has 3 different maps 
- Data sources may be incomplete or inconsistent because data will be collected by walkthrough video
- Future work may include:
  - Ranking the Magic spells
  - Enemy threat level ranking
