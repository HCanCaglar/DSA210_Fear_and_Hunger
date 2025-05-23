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
- In-game observations from a Youtube walkthrough: https://www.youtube.com/watch?v=Nx5cuJA_lIs

### Weapons Dataset (`weapons.csv`)
| Column | Description |
|--------|-------------|
| Name | Weapon name |
| ATK | Attack value |
| #AltWeaponsLowerOrEqual | Number of weapons with lower or equal ATK |
| WeaponNumber | how many copies of this specific weapon can be obtained, calculated by RNG, guaranteed and enemy drops|
| AcquisitionType | `Guaranteed`, `RNG`, or `Enemy Drop` (one weapon can have multiple ways of obtaining it) | 
| NumEnemiesDrop | Number of enemies that drop this weapon |
| EnemyPopulationDrop     | Total number of individual enemies that can drop this weapon |
| NumRNGSpots | Number of RNG locations it can be found / totalRNGspots |
| NumGuaranteedSpots | Number of fixed spots it can be obtained from |
| UsefulnessScore | Calculated as: `#AltWeaponsLowerOrEqual + (NumEnemiesDrop) + (NumGuaranteedSpots + (NumRNGSpots)`| 

### Status Effects Dataset (`status_effects.csv`)
| Column | Description |
|--------|-------------|
| Name | Status effect name |
| NumEnemiesToGet | Total number of enemies that apply it |
| NumCures | Number of known cures for the status effect |
| TotalCureCount | Calculated for each status as (total RNG spots/ item count) + guaranteed spots |
| Curability | 1 if there is no known cure, 0 otherwise |
| DangerLevel | Calculated as: `(NumWaysToGet − CureAvailabilityScore)+(Uncurable * 20)` |

---

## Research Questions & Hypotheses

### RQ1: Which weapons are most useful?
- **H₀**: Weapon acquisition method and ATK value are not correlated with usefulness.
- **Hₐ**: Weapons with higher ATK and easier acquisition paths are significantly more useful.

### RQ2: Which status effects are the most dangerous?
- **H₀**: All status effects are equally dangerous.
- **Hₐ**: Status effects with fewer cures and higher frequency of application are more dangerous.

### RQ1: Is ATK related to a weapons usefulness?
- **H₀**: Weapon ATK value is not correlated with usefulness.
- **Hₐ**: Weapons with higher ATK are significantly more useful.

### RQ2: Does a weapon's frequency affects its usefulness?
- **H₀**: Weapon acquisition method is not correlated with usefulness.
- **Hₐ**: Weapons with more acquisition paths are significantly more useful.

### RQ3: Do status effects get more dangerous with their frequency?
- **H₀**: Frequancy of a status effect does not make it more dangerous.
- **Hₐ**: Status effects with higher frequency of application are more dangerous.

### RQ4: Do status effects get more dangerous if they have no cure?
- **H₀**: Curability of a status effect does not make it more dangerous.
- **Hₐ**: Status effects with fewer cures are more dangerous.


---

## Data Collection Plan
- Data will be logged manually in Excel using the fields above and more if necessary.
- Acquisition and application types will be categorized based on F&H wiki info and verified in-game if possible.
- Subjective scores will be calculated.

---

## Machine Learning Integration

To move beyond descriptive statistics and validate the underlying relationships in the dataset, supervised machine learning models of Linear Regression and K-nearest neşghbours were applied and compared with each other, to both weapon and status effect data.

### Weapon Usefulness Prediction
A **multiple linear regression model** was trained to predict the `UsefulnessScore` of each weapon based on key numeric and categorical features, including `ATK`, acquisition frequency, and drop type. Categorical acquisition methods (`RNG`, `Guaranteed`, `Enemy Drop`) were encoded using multi-label binarization. The model achieved an R² of ~0.999, confirming that `UsefulnessScore` is strongly and linearly correlated with measurable attributes.

Alternative model **K-Nearest Neighbors** were also evaluated. However, their performance was weaker due to the small dataset size and the how the `UsefulnessScore` formula is calculated. Linear regression was both the most accurate and most interpretable model for this task.

### Status Effect DangerLevel Prediction
A similar regression was applied to the `status_effects.csv` dataset to model `DangerLevel`, a derived score based on the frequency of application, number of cures, and curability.Both linear regression and KNN models were trained.

Linear regression again proved highly effective, with R² scores near 0.999. This suggests that `DangerLevel`, like `UsefulnessScore`, behaves in a predictable and explainable linear manner based on underlying mechanics. However by the unnaturality of randomly chosen status effects for the ML sometimes KNN model predicted very close to what MLR model and sometimes much much worse negative R² values, so it is concluded that MLR is a more consistant model

### ML Workflow Summary
The machine learning workflow included:
- Model training and evaluation using:
  - R² score (variance explained)
  - RMSE (prediction error)
- Visualization of predicted vs actual scores, annotated with weapon/item names

This ML component validates that game design decisions (weapon availability, status severity) can be modeled with high accuracy.

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

## Interesting Findings
- Collected data and conclusions made by the hypothesis tests are not always accurate to the game knowledge
- sometimes some status effects are more dangerous than the others despite having the same numerical danger lever value
- for calculations we had to assume many things about RNG spawing or being able to use everything we have for curing status effects and nothing else
- all these assumtions make ome status effects look less dangerous than they are as well
- same thing for weapons, some weapons have similar usefulness score but anyone who plays the game will see that some weapons with high usefulnessscore are very useless

---
## Limitations & Future Work
- Some variables (like encounter rate) are subjective or hard to measure because the game has 3 different maps 
- Data sources may be incomplete or inconsistent because data will be collected by walkthrough video and wiki page
- Many other data such as traps and craftable items has to be ignored for this project
- We have to assume that rng spots only drop these status curing items for data.
- Future work may include:
  - Ranking the Magic spells
  - Enemy threat level ranking
  - very long time spent on gameplay to get more consistent data
