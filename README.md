# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3: Web APIs & NLP

### Background

The COVID-19 pandemic has taken a toll on most people and it has resulted in a huge increase in mental health awareness.

### Problem statement

**What we plan to do**

We are a group of data scientists representing a yoga studio. We note that there are notable differences between yoga (Physical and mental exercise) and mediation (Pursuit of mental stability). 

**Is the scope of these project appropriate? Who are our important stakeholders and why is this important to investigate?**

Given the increased awareness of the importance of mental health, we see an opportunity to increase and broaden our customer base. Hence, there is a need to tailor our marketing campaigns to promote yoga as a coping strategy / self-care tool. 

We would also like to maximise the effectiveness of our campaigns (Maximising true positives; Accurately classifying r/yoga posts and converting them to members by implementing marketing campaigns focusing on top predictor words).

**What type of models will we be exploring?**

Our approach is detailed below:
* Grouping posts from reddit r\yoga and r\Meditation (which are similar in nature)
* Discover 'trending* words for both subreddits
* Exploring the similarities, but also focusing on exploiting differences between both subgroups
* Build an effective *classification* model to better target yoga enthusiasts (aka. r/yoga users; Maximise marketing spend)
* We can also exploit similarities between both subgroups to convert people interested in meditation to try out yoga

**How will success be evaluated?**

We will focus on optimising F1 scores (Ensuring a good trade-off between precision and recall). Our aim is to create a model that significantly outperforms the baseline heuristic which we've identified earlier (50-50).


### Structure of our workflow

We have split our code into 3 separate notebooks

- ```project 3_data_collection```
- ```project 3_cleaning and EDA```
- ```project 3_modelling```

---

### Data collection

We will utilise Pushshift's API to extract posts from r/yoga and r/meditation subreddits. Given the limitations of the Pushshift API, we have created custom function to loop our 'calls'. We will pull out 1500 posts from each subreddit.

We will also need to introduce time breaks in the code to prevent the server from being overload, as well as standardising our retrieval times to ensure our dataset remains fixed.

---

### Data dictionary

This is a dataset of our ```merged``` dataset (with our columns of interest), which will provide a quick overview of our features / variables / columns, alongside data types and descriptions.

|Feature|Type|Description|
|:-|:-:|:-|
|subreddit|object|Whether the posts belong to r/yoga or r/meditation|
|title|object|Title of subreddit post|
|selftext|object|Message body|
|is_self|bool|1 - If this is a text-only post|
|num_comments|int64|Number of comments for a particular subreddit post / thread|
|created_utc|int64|Unix timestamp for a particular post|
|created|object|Date and time-stamp for a particular post|
|removed by category|object|Whether a post was removed|
|author|object|Author's reddit username|
|permalink|object|URL of reddit posts|
|score|int64|Number of upvotes - Number of downvotes|
|upvote_ratio|float64|Net user approvals|

---

### Data cleaning and EDA

Our merged dataset has a total of 12 columns. After carrying out some preliminary EDA, we have decided to implement the following steps:

* Filter out posts that contain text, or have already been removed by a specfic subgroup
* We will then utilise the redditcleaner package to tidy our title and selftext columns. This python module removes characters temming from (Reddit-specific) Markdown formatting and returns the cleaned text. Common punctuation, numbers, parentheses, quotation marks, emojis are deliberately not removed

Source: https://github.com/LoLei/redditcleaner

* We then carry out some sentiment analysis on subbredit posts' titles and selftext. Concatenation of titles and selftext will be carried out to create a more representative text corpus.
* Lemmatization is done soon after (instead of stemming). Lemmatization looks beyond word reduction and considers a language's full vocabulary to apply a morphological analysis to words/ It is hence more informative than simple stemming and that is one of the main reasons why we've decided to employ this text normalization technique in our project.
* Stop-words are then removed via the gensimpackage

---

### Pre-processing

Our approach is summarised below:
* Splitting / sampling data for validation and training purposes (Ensuring samples are stratified)
* Convert text data to a matrix


### Modelling

* We will test / evaluate a variety of models to identify a production algorithm with CountVectorizer or TFIDFVectorizer overlays
    * Logistic Regression
    * Naive Bayes
    * Random Forests
* Explaining how our chosen models work and highlight a particular model's pros/cons
* Comparing models to the baseline score and explain the evaluation metrics we have chosen to focus on
* Diving deeper into our final recommendations and addressing our original problem statement

We will build our models using a pipeline and utilise GridSearchCV to determine the best parameters. We will also be logging the total time needed to iterate our models.

### Model evaluation

![My model results](model_results.png)

**Precision**

*Precision* gives us information about the model's performance with respect to false positives (How many did we catch?). Tying this back to our problem statement, this would mean the measure of posts we correctly classify as falling under r/yoga out of all r/yoga posts.

**Recall**

On the other hand, recall gives us information about a classifier's performance with respect to false negatives (How many did we miss?). Out of all r/yoga posts, this tells us how many we correctly identify as falling under r/yoga.

**F1 score**

The F1 score is the harmonic mean (or, simply **balance**) of both ***precision and recall***. This forces us to make a trade-off between Precision and Recall. 

**Overall**: 

The Naive Bayes model with the CountVectorizer overlay has the best F1 score here. As our dataset is not significantly unbalanced, we do not to tweak our false positive thresholds, hence we do not need to place a lot of emphasis on the ROC AUC.

---

### Conclusion and recommendation

Our best model appears to be a Naive Bayes + Count Vectorizer mix. We are optimising for our F1 score.

![My top predictors](top_predictors.png)

There are considerable overlaps in the top predictor words for r/yoga and r/meditation classifications. However, certain words stand out for r/yoga (i.e. pose, mat, class, week, teacher, studio, video).

These terms tend to be found in posts - that contain concerns and challenges faced by yoga enthusiasts (i.e. incorrect postures, beginner challenges, bodyaches after yoga, questions around various poses).


**Recommendations**
* Our text-based classifier model is able to accurately pin-point posts that fall under r/yoga - This means that we will be able to improve efficacy of our marketing approach
* Our identification of top-words can be used to optimise the messaging of marketing content or product description
* There appears to be a demand for yoga instructional videos / online classes on a variety of poses
* Mats also appear to feature in a lot of r/yoga subreddit discussions; We could improve studio sign-up rates by throwing in a complimentary yoga mat
* There are also numerous mentions of studios / classes - Marketing materials should clearly list locations of studios and provide detailed time-tables of yoga classes

**Future Enhancements**
* We can look and identify more subreddits (i.e. Hot and top sections) to determine what is trending
* Exploring a variety of models (i.e. SVM, Gradient descent and deep learning models)
* We can run sentiment analysis on subreddit post comments
* Stop-words can be further customised to optimise our models