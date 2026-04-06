# Credit Risk Model 

# Credit Risk Modelling and Threshold Analysis
In this project, I worked with the German Credit dataset to build a credit risk model and understand how model decisions actually impact lending outcomes.
The goal here was not just to classify borrowers as good or bad, but to see what those decisions mean once you bring in financial impact.

## What I did
I started by loading and preparing the data.
I encoded the target variable, split the dataset into features and labels, then split it again into training and testing sets. I also standardized the data before training the model.
From there, I built a logistic regression model as a baseline and generated prediction probabilities.
Instead of using the default cutoff, I set a classification threshold and used that to generate predictions. I then evaluated the model using the confusion matrix, accuracy, and classification report.

# Moving from accuracy to financial impact
Before building the cost framework, I made a few simple assumptions:
average loan = 1000
loss = 20 percent (200) for bad loans that are approved
no loss for correct decisions
no loss for good loans that are rejected
From there, I translated the confusion matrix into a cost matrix by applying these values.

# Step 1: Model results
Confusion Matrix + Cost Matrix
The confusion matrix looks like this:

<img width="688" height="554" alt="image" src="https://github.com/user-attachments/assets/b23ffe1f-13cb-4d01-bfb7-26be473064f9" />

So:
134 good loans were correctly approved
20 bad loans were correctly rejected
39 bad loans were approved → this is where the loss comes from
7 good loans were rejected


When I apply the financial assumptions, the cost matrix shows:

<img width="676" height="545" alt="image" src="https://github.com/user-attachments/assets/8a795942-e275-400a-a38d-2391161f02f5" />


total loss = -7800
everything else = 0

So the main takeaway here is:
the entire financial impact is coming from those 39 bad loans that were approved

# Step 2: Contingency reserve
Contingency Reserve vs Threshold

<img width="923" height="818" alt="image" src="https://github.com/user-attachments/assets/48c48279-4ccc-4eed-ac5b-2ccba7eb5d21" />

Next, I calculated the total financial impact across a range of thresholds.
For each threshold, I:
  - generated predictions
  - built the confusion matrix
  - applied the value matrix
  - calculated total cost
  
This allowed me to find the threshold that minimizes contingency reserve.

From the plot, the optimal threshold is around 0.95.
At this point:
  - very few risky borrowers are approved
  - total financial impact is minimized

# Step 3: Profit perspective
Profit vs Threshold

<img width="814" height="831" alt="image" src="https://github.com/user-attachments/assets/ffcbe3e5-426b-410f-9418-975e8a936518" />

I also looked at the problem from a profit perspective instead of just minimizing loss.
Here, the optimal threshold shifts to around 0.58, where profit is highest.
This creates a clear trade-off:
0.95 → minimizes risk
0.58 → maximizes profit
What stands out
There isn’t one “best” answer.
If the goal is risk control  =  higher threshold
If the goal is growth / profit = lower threshold
So the decision really depends on what you’re optimizing for.



# Model comparison
To extend the analysis, I also tested:
Random Forest
XGBoost
Neural Network
One thing I noticed is that some models perform well only in a narrow range of thresholds, while others are more stable.
In practice, I would choose the model that performs consistently, not just the one with the highest peak performance.

# Final takeaway
The main takeaway for me is that accuracy alone doesn’t mean much.
Once you introduce financial impact:
the threshold becomes just as important as the model
model decisions change depending on what you optimize for
the “best” model depends on the business context
If I had to choose in practice, I would go with something balanced depending on the risk appetite.

