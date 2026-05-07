# Expected Rush Yards

Predicting the number of yards a running back will gain after a handoff using NFL player tracking data and XGBoost.

**Authors:** Lucca Ferraz, Savan Patel, Ryan Coombs, Peyton Stevenson — Rice University

---

## Contents

| File | Description |
|------|-------------|
| `Expected_Rush_Yards.pdf` | Full slide deck with methodology and results |
| `expected_rush_yards.qmd` | Quarto source file with all code and analysis |

---

## Overview

Using NFL player tracking data from the 2018 season, we built a model to estimate expected rushing yards at the moment of the handoff. The data captures the position, speed, direction, and orientation of every player on the field for each rushing play.

---

## Methodology

### Preprocessing
- Flipped coordinates so all plays move rightward
- Adjusted player directions and orientations to be consistent
- Identified the rusher in each play
- Labeled offensive and defensive players
- Computed yards from own goal line

### Feature Engineering
**Rusher features:** position, speed, acceleration, orientation, direction, distance from line of scrimmage

**Defender features:** distances to rusher, angles, number of defenders ahead in forward cone

**Blocker features:** count, distances to rusher, line width/depth, average orientation

**Contextual features:** down, distance, quarter, yard line, score difference, formation, personnel, defenders in the box

**Convex hull features:** area of offensive/defensive player formations, hull area ratio

**Cone features:** offensive and defensive hull area in rusher's forward cone, cone area ratio, number of defenders in cone, closest and farthest cone defender

### Modeling
XGBoost was chosen for its ability to capture non-linear player interactions, handle high-dimensional tracking data, and remain robust to missing values and mixed feature types.

- Target variable: yards gained, converted to 199 classes
- Trained a multi-class softprob model to produce cumulative probability distributions
- Categorical features handled with one-hot encoding
- Hyperparameters tuned via cross-validation
- Final model trained on all plays; predictions evaluated on held-out test set

---

## Results

The model achieved a **CRPS of 0.01322** on 15% of the data.

Top predictors by variable importance: rusher yardline distance, rusher acceleration, rusher direction, rusher speed, average blocker orientation, and defensive hull area.

---

## Takeaways

- Feature selection requires empirical testing — many intuitive features underperformed and vice versa
- XGBoost performance scales substantially with more training data
- Hyperparameter tuning had a large impact on final model performance
- Training and cross-validation are computationally expensive at this data scale

---

## Data Source

NFL player tracking data from the 2018 season via Kaggle's NFL Big Data Bowl competition
