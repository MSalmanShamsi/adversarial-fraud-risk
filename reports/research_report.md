# Adversarial Sensitivity of a Behavioral Fraud Model

## Study status
Completed amount-reduction experiment and threshold-defense evaluation.
This is a draft research report, not a publication-ready manuscript.

## Objective
Evaluate whether constrained changes to fraudulent transaction amounts
reduce detection, and assess a validation-selected threshold defense.

## Data and model
The September test set contains 287,873 transactions,
including 2,547 fraudulent transactions.
The model is standardized logistic regression with balanced class
weights, followed by isotonic calibration fitted on validation data.

Model features:
TX_AMOUNT, TX_HOUR, TX_DAY_OF_WEEK_NUM, TX_MONTH, CUSTOMER_TX_COUNT_PREV, CUSTOMER_AMOUNT_DIFF, TERMINAL_TX_COUNT_PREV

## Experimental method
Fraud amounts were independently reduced by 5%, 10%, 15%, and 20%.
Historical features remained fixed. CUSTOMER_AMOUNT_DIFF was recalculated.
Legitimate transactions were unchanged. The 0% control reproduced the
original test probabilities.

## Original-threshold results
At threshold 0.145, clean test precision was
66.98% and recall was
25.09%.

At a 20% amount reduction, recall was
16.57% and precision was
57.26%.
Of the originally detected fraud transactions,
217 evaded detection,
an attack success rate of
33.96%.

## Threshold defense
The defense threshold, 0.140350877193, was selected using
20%-amount-reduced validation data only. Selection maximized F1
subject to an alert budget of twice the original clean validation
alert count and a threshold no higher than 0.145.

At this threshold, clean test recall was
25.48% and precision was
61.34%.

Under the 20% test attack, defense recall was
17.04% and precision was
51.48%.
Recall changed by 0.47 percentage points relative to
the original threshold under the same attack.

The defense produced 409 false alerts,
a change of +94 relative to the original threshold.
The validation alert budget is not a guarantee of test alert volume.

## Validation/test correction
The previously reported 82.77% precision and 23.75% recall were
validation results. Original September test precision and recall
at threshold 0.145 were 66.98% and 25.09%, respectively.

## Interpretation
The experiment measures sensitivity to consistent transaction-amount
changes. The threshold defense trades detection against alert workload.
Attack success rates for the two thresholds use their own clean
detected-fraud denominators; compare recall and false alerts as well.

## Limitations
- Results use simulated data and one model family.
- Changing an amount may reduce an attacker's financial benefit.
- Fraud labels are held fixed as a simulation assumption.
- Attacks are individual counterfactuals, not a sequential replay:
  modified transactions do not update later historical features.
- The defense is selected for the 20% validation attack scenario.
- The twofold alert budget is an experimental assumption.
- No real-world effectiveness, publication acceptance, or immigration
  outcome is established by these results.

## Reproducibility
The output folder includes comparison metrics, charts, model files,
and experiment metadata. The original notebook and source dataset
are also needed to reproduce training and feature generation.

## Additional methodological disclosure
The source notebook compared calibration methods on September test results
before selecting isotonic calibration. September is therefore an exploratory
evaluation set, not a completely untouched model-selection holdout. August
validation data was reused for fitting calibration and selecting thresholds.
Day 9 used a threshold near 0.255814. The amount experiments use the later
Day 10 validation-grid threshold of 0.145 throughout. These operating points
must not be mixed when comparing results. A future confirmatory study should
use a distinct calibration/selection strategy and an unseen evaluation period.

## Dataset and code attribution
The synthetic transaction generator derives from the Fraud Detection Handbook
by Le Borgne, Siblini, Lebichot and Bontempi (2022).
https://github.com/Fraud-Detection-Handbook/fraud-detection-handbook
The upstream notebook code uses GNU GPL v3. The simulation results do not
verify any client loss estimate or demonstrate deployment effectiveness.
