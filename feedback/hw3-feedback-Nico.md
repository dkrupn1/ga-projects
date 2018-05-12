### ***Project 3 Feedback***

***Nico Van de Bovenkamp***

***

**Overall**  
This is a great start! You have spent a long time on this dataset and working on your Pandas/Python skills -- it's paying off. As we have discussed, you know more than you think you know!

There is still a bit of work to be done on this for you to build your model, but I know you are on your way! So, below are some useful notes that I have sent some other people.

**Some notes**  

* **Train/test Split earlier** As we discussed in our review session, the first thing that you want to do is perform your train/test split (once you have your data). You don't want any information about your test set to "leak" into what you are learning about your training set. All preprocessing, transformations, handling of missing values, etc. should be done on your train set as the goal is to have it generalize onto your test set. Again, the concept is: "We don't have the test yet, that comes tomorrow." Also, note that your test size is a bit large. I would stick to values closer to 0.1 - 0.2. Otherwise, you are just losing out on data that you could be using to train your model.
* **Scaling your data**  Again, as we discussed in our review session, it's pretty much always a good idea.
* **Precision vs. Recall vs. F1** Which should you maximize? This is a key question in any modeling task. If you want to avoid False Negatives, cases when you predict class 0 but it is actually class 1, then you want to maximize Recall. The reverse is true for Precision (avoid False Positives). I hope I am understanding this correctly problem correctly, but is this about if a person defaults on a loan or not? If it is, then you probably want to be sensitive to saying someone won't default when they actually do. This means you really want to avoid False Negatives because being wrong about about defaults can be super costly (my apologies if I don't properly understand the problem). So, how do we shift our balance of recall and precision:
    - Try different models, with different hyperparameters, features, or totally different all together, and pick the one that gives you the best recall (without losing too many False Positives).
    - With logistic regression, because we have probabilities, you can actually change a threshold for making the classification. Instead of making it a majority vote (probability > 0.5 in binary), you can cut it off at a higher threshold. If you make the cut-off to say it is going to rain (class 1) at probability 0.4, you are being more flexible to saying "Hey, it may rain." This will lower your chance of making a False Negative prediction!
    - The last option is by weighting... You can actually give more weight to a certain class if it has more cost: https://stackoverflow.com/questions/30972029/how-does-the-class-weight-parameter-in-scikit-learn-work
* **Cross Validation!** You always want to cross validate your models! That is how we ensure that we are building a solid model. If you do not cross validate, then there is not much assurance that you didn't overfit your training set. You should always be building your models with an eye on those cross val scores. If this is unclear, let's chat after class!
* **Hyperparameter optimization**  As we discussed in our review session, once you have decided on a solid set of features, you want to do some level of hyper parameter tuning. You currently aren't adding any penalty to your model, which could help with some over-fitting.
        - Logistic regression:
            - C : Amount of penalty you apply to your weights! Remember that in this implementation, it is actually 1/C. Thus, a smaller C means more penalty as you are adding MORE of the weights to the penalty.
            - penalty: 'l1' vs. 'l2'
            - fit_intercept : (this is a parameter, but you likely always use this term)
            - solver : {‘newton-cg’, ‘lbfgs’, ‘liblinear’, ‘sag’, ‘saga’} this is your term to change the optimization procedure! These can be interesting to experiment with. On a smaller dataset, this may not be so important, but when datasets become huge... This is very important.
        - KNN:
            - n_neighbors : number of neighbors.
* **Using different models**  So, why would you want to use LogisticRegression vs. KNN? I hope you have a better idea after our review session, but here are some points:
    - **Model structure**  Logistic regression is modeling the probability of a class occurring, given some data, to make a linear classification boundary. KNN does not have a model form, it is just empirically taking the structure of your training data and hoping that there is some local information given your data that will have once class be closer to another class (distance).
    - **Model output**  Logistic regression output model probabilities. This means that we can rank the outcomes of things (based on probability) and we can see a band of different outcomes! You have more information in there. When you use KNN, we are just applying a label to a new datapoint.
    - **Efficiency and performance**  If you have tons of features, KNN is going to be SLOW. At times, it will even be totally useless as distance becomes meaningless in high dimensions. Logistic Regression are very, very fast.
* **Looping through values**  In general, it is not a good idea to loop through values in a for loop. You should take advantage of some pandas functions (and numpy) to make things speed up! I would recommend that you re-write that loop into a function and the apply it to each row: `df['Grade'] = df['G3'].apply(grade_bucketing)` where `grade_bucketing` is your function that checks a value and returns 1 if in range (0 - 5), etc.!
    - Check it out: https://engineering.upside.com/a-beginners-guide-to-optimizing-pandas-code-for-speed-c09ef2c6a4d6
* **Plotting**  When plotting with lots of features, I recommend that you take a look at just a subset of features. In particular, I would choose features of interest to be plotted by themselves and with your **target** variable. If you plot too much, you end up with an unreadable mess.
* **A note on predict methods**  In Sklearn, there are a few ways that we can "predict" in the api. The methods given are always defined within the documentation, as they are not always the same. For LogisticRegression, we have a `predict()`, `predict_proba()`, `predict_log_proba()`, and `decision_function()`. If you look through the documentation, you will notice that these functions output/mean different things. You have used the `decision_function()`, which outputs a "confidence score" by taking the distance of the given point from the hyper-plane (the plane that is separating our classes). A positive score indicates a high confidence via distance, and a low score would be greatly negative as it is on the "wrong side" of the prediction boundary. You can use this, but be aware of what it truly means! I would recommend using probabilities with Logistic Regression, as it's one of the benefits of having this model in the first place! `predict()` will predict the **class** and 'predict_proba()' will predict a probability.
