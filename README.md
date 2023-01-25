# cft2022
Kaggle competition notebook for CFT 2022

https://www.kaggle.com/competitions/uplift-shift-23

# Uplift? Or Forecasting Randomized Data

This notebook is my solution and review the SHIFT CFT 2022 Kaggle problem.

In the data we have information about one retail's chain clients - who received a text (SMS) and who didn't. This is called a treatment flag. This treatment as we understand is a completely random occurrence.

We also have purchased / not purchased flag, thus we can track clients who were treated and purchased something and vice-versa.


**Target** now the target is formulated as follows:
   - 1 if:
        - (treated==1) and (purchased==1) or
        - (treated==0) and (purchased==0)
   - 0 otherwise

They also ask that the target is projecting who needs to recieve a text for presuasion to purchase, i.e. uplift.

But we can see that this is not the case and we are actually asked to predict a randomized data.

$$ 
Target = P(treatment) * P(purchase) + P(notreatment)*P(nopurchase)
$$


And so the target actually depends on a randomized variable - treatment. As we believe the treatment sample to make the experiment was randomized by the design, which is a sensible thought in A-B testing. 


Nevertheless, the data has some information about the treatment and purchase/no purchase, which allows us to predict the outcome even though with very limited accuracy (top submissions on Kaggle are around .05 in Gini criterion).

**Uplift** - is calculated as difference in target('purchased') mean between treated and control groups by the design of this competition. This makes sense when we have (as in our case) a large amount of data and relatively balanced treatment and control groups.

We will do a **two-model approach**:
* Data is split by two - treatment group and control group
* Two independent classifiers are built
* Each one makes a predictions on the full dataset
* Substracting probabilities from one to another we get an estimated **uplift** effect

**What do we do?**

1. Analyse the data and see if there are any insights
2. Construct some helpful features
3. Two-model approach to estimate the effect
4. Predict the ranking for Gini / ROC-AUC criterion by taking differences in probabilities in two-model approach.


**Summary of results:**

* Two-model predictions gives a better understanding which of the clients are much likely to purchase under the treatment
* Two-model approach helped us to calculate the uplift effect on the train data which we estimated around 3.16%, which is quite close to the difference of the means approach which gives 3.35%
* The best solution was able to predict about 3% above the baseline (random predictor). Which also kind of points to the issue of the target definition.
