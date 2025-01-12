# CS412 Project

## Classification

My first approach was to use a random forest classifier. After vectorizing the biographies and extracting/encoding various categorical features from the profiles (such as business categories and verification status), I used a CV grid search to find the best set of parameters. This gave me a testing accuracy of around 40%, which represented significant room for improvement.

On subsequent rounds, I changed the random forest classifier to a support vector machine, and later a *linear* SVM since other kernel types only slowed computation without any accuracy increase. Although a feature scaler was initially used, it did not provide any test accuracy increase either.

The feature extraction step was also changed to only include vectorized corpuses made from biographies and post captions, previously ignored. In addition to the already provided preprocessing, the feature extraction step was modified first to include emojis, then to lemmatize the cleaned text. A limited, lookup-based lemmatizer was used to avoid the infeasible processing times of the full model. Finally, the vectorizer was changed to include bigrams and trigrams on top of individual words.

A one-hot encoding of business categories and category enums was also initially used, but was removed after discovering the testing data did not include this information.

The final validation accuracy was around 65-70%.

## Regression

My first approach for regression was similar to classification, with a random forest regressor. I extracted comment counts, media types and vectorized captions from individual posts, then once again used a CV grid search to find the optimal parameters for the model.

In the next rounds, I instead opted for another SVM approach, this time with an RBF kernel. However, this time I did not use a single model, but instead trained individual support vector regressors for each user's posts. The output of this model was clamped to positive numbers, since it's not possible for a post to have negative likes.

The features I extracted for each user in this round were all numeric: comment counts and post times. I used comment counts directly, but post times were processed further. I first extracted the day of the week (0 ≤ n < 7) and the time of day in seconds (0 ≤ n < 86400). Then, these two values were each mapped to two dimensions, forming two circles. This ensured that subsets of the form "from time X to time Y" would always be linearly separable, even if that time period crossed midnight.

In all, these changes brought validation log MSE down to around 0.56. 
