# Ocean State Waves Pitching End-Of-Season Statistics

## Overview
- Using TrackMan data, this project finds important pitching metrics to assist the coaching staff with end-of-season evaluations.
- This project was built for the Ocean State Waves of the NECBL.
- Summer 2026 NECBL Season.

## Key Results
- R^2 of 0.25, signifying a modest predictive performance for the custom xwOBA model.
- Used difference of wOBA and xwOBA to find out how players' expected stats differed from their actual stats.
- Sent to the director of analytics who shared it with the coaching staff and media team.

## Data
- Source: NECBL database with every pitch tracked and tagged by TrackMan.
- Metrics tracked: whiff rate, walk rate, strikeout rate, maximum velocity, maximum spin rate, wOBA, custom xwOBA.
- *Note: Due to rules with proprietary TrackMan data in the NECBL, the original dataset was used to find all these metrics initially, before being sent to our director of analytics. This project includes my code and qualitative findings.*

| Column | Description |
|---|---|
| `Pitcher` | Pitcher's name |
| `Pitcher_Id` | Unique ID associated with each pitcher |
| `PitcherTeam` | Pitcher's team |
| `PitchCall` | Tracks if a pitch was a called strike, swinging strike, ball, foul ball or put in play |
| `KorBB` | This column tracks if the pitch result ended in a strikeout or walk with values K or BB |
| `PlayResult` | This column records the results of balls-in-play |
| `RelSpeed` | Pitch velocity |
| `SpinRate` | Pitch spin-rate |
| `ExitSpeed` | Batted ball exit velocity |
| `Angle` | Batted ball launch angle |

## Pitching Metrics
- wOBA - every play result was given a certain weight based on MLB's published wOBA weights from last season. I averaged the weighted plate appearance outcomes for each pitcher, rather than using the MLB's specific formula.
- Maximum Velocity - pitches were grouped by pitcher and then aggregated for maximum velocity.
- Maximum Spin Rate - pitches were grouped by pitcher and then aggregated for maximum spin rate.
- Strikeout Rate - total strikeouts were counted by grouping the results dataframe by pitcher. This was then divided by total plate appearances against.
- Walk Rate - total walks were counted by grouping the results dataframe by pitcher. This was then divided by total plate appearances against.
- Whiff Rate - total swings were counted by grouping the results dataframe and then finding results that were either in play, swinging strikes, or foul balls. I then found the total amount of whiffs per pitcher which was divided by the total swings value.
- Custom xwOBA - this is my custom expected outcome model using exit velocity and launch angle.

## Model
- Features: exit velocity ('ExitSpeed') and launch angle ('Angle')
- Random Forest Regression was used because many baseball relationships are nonlinear. The random forest assigns xwOBA predictions to every batted ball and these predictive values are then averaged to find a pitcher's custom xwOBA. I used a five-fold cross-validation, repeating the model so every fold was able to be the test group once.
- The model predicts at the batted ball level and then my final custom expected production metric is an average at the pitcher-level.
- I validated my model by finding the cross-validated R^2 value of 0.25, which was a mean of the five-fold cross-validation scores. This signified a modest predictive performance for my model.
- I used the difference in wOBA and xwOBA to see how players' expected and actual stats differed. This helps gauge the quality of contact a pitcher gives up rather than strictly looking at their actual results.

## Limitations
- The sample size was small as only Waves games were used across a short summer-league season. This makes it hard to understand how well this model predicts new data.
- I only used launch angle and exit velocity, which means the model omits other possibly influential factors.
- The Waves play in a ballpark with a very short right field wall that allowed for a lot of extremely short home runs. This could cause a lot of variance in the model when it attempted to predict home runs versus flyouts.

## Next Steps
- Add additional features such as sprint speed, spray angle, ballpark factor or defense quality.
- I would try an XGBoost model as these models attempt to correct the errors made by previous trees. This allows for the possibility of more nuance in deciding if a certain batted ball was a bloop single or a catchable popup to a middle infielder. It would be a solid next step in finding out which regression performs better depending on data or evaluation method. 

## Repository Files
| File | Description |
|---|---|
| `README.md` | Project documentation |
| `OSW_Stats.ipynb` | The code used for this project |
| `requirements.txt` | Dependencies |

## How to Run
- Install dependencies: `pip install -r requirements.txt`
- Open the notebook: `jupyter notebook OSW_Stats.ipynb`

*Note: the model will not run end-to-end due to the omission of proprietary data.*

