<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

## SIOC Machine Learning Prioritized Inventory Expansion Program Overview
Ship in Own Container (SIOC) is a shipping type that allows products to
be shipped without any additional Amazon packaging. SIOC shipments
reduce shipping supply costs and transportation costs through decreased
shipped volume. Increasing SIOC shipments is the one of primary goal of
Amazon Japan.

In support of the goal of increasing JP SIOC shipments, I intend to
certify true SIOC ASINs (Amazon Standard Identification Number, meaning
unique catalog entry) as SIOC. Our team performs the testing program to
check whether ASINs are SIOC capable. The SIOC testing program finds
ASINs capable of shipping without an Amazon box by checking items in FCs
(Fulfillment Centers) and certifying ASINs that meet SIOC standards.
SIOC certification requires primary packaging inspections in FCs to
ensure that existing packaging meets standards designed to prevent
damage during fulfillment and delivery. The below picture shows the
example packaging meeting SIOC criteria described above.

![SIOC image](/assets/img/SIOC.png)

With limited resources for performing the SIOC teting program, my team
needs to find the best way to identify which ASINs to test our of more
than 100 million inventory. I developed a supervised machine learning
(ML) model trained on historic SIOC ASINs to predict SIOC likelihood. In
this way, I could increase the chances that inspected ASINs are SIOC and
can increase the future SIOC shipments from each certified SIOC ASIN vs
randomly selecting ASINs for inspection.

The ML prioritization methodology is estimated to 537M JPY cost savings
anually.

## ASIN Prioritization Process -- Supervised Learning Model Development

I developed a SIOC Prediction Model that learns which qualities
(dimensions, manufacturer information, category information, and textual
information like item title and descriptions) from a set of SIOC and
non-SIOC ASINs are associated with SIOC ASINs. This trained model can
then be used to make predictions about unseen ASINs, whose SIOC status
is unknown.

The training module is developed to run in an AWS SageMaker Notebook and
captures data from and saves data to S3 buckets.

This model uses two popular word similarity libraries, LDA and Word2Vec
to identify similar words between ASINs. I used XGBClassifier, a popular
gradient boosting machine learning algorithm, to train the model.

## Training Module

The module consists of 3 main components:

**1. PIP Installs, Python Library Imports, and Function Declarations**

**2. Training Data Creation**

1.  Training Data Set Up

    a.  Import training set, manipulate data (remove duplicates and
        drop/format/rename columns), save training set data to file.

    b.  Create bag of words: create a string of the words used in all of
        the text fields across each item.\
        Text fields include: item name, gl_product_group_description,
        category description, subcategory description, brand name,
        manufacturer name, product_type, binding, product_description,
        and bullet point fields.

    c.  Encode the categorical columns into category codes in the
        training set and save the category translations as an encoder
        list and save it to file.

2.  Word Similarity Capture -- TF-IDF/LDA

    a.  Create a dictionary from the bag of words (encoding each unique
        word to a number) and save the dictionary to file.

    b.  TF-IDF - Term frequency-inverse document frequency is a
        statistic intended to reflect how important a word is to the
        corpus. The higher the number, the more important the word is.
        TF-IDF provides scores for each word associated with each ASIN.

    c.  LDA - LDA (latent Dirichlet allocation) is a model for computing
        text similarity. Its inputs include the tfidf model and the
        dictionary.

    d.  Topic list - create and save a list of specific 'topics' in the
        LDA model defined above. Each topic is associated with a record
        in the training data.

3.  Word Similarity Capture -- Word2Vec\
    Word2vec is another model that can be used to detect sentence
    similarity. Use the bag of word data associated with each ASIN
    created above to create a word2vec model and save it.

4.  Combine models (LDA and Word2Vec) into a single input training set

**3. Define Prediction Model for Training**

1.  Define XGBClassifier classifier (set learning rate, XGBClassifier max depth, gamma, etc)

2.  Create model with bag of words, numerical columns, categorical columns, binary columns, LDA topics scores, and word2vec vector.

## Prediction Module

The prediction module is developed to run in an AWS SageMaker Notebook
and captures data from S3 buckets. The module consists of 4 main
components:

1.  PIP Installs, Python library imports, function declarations, and
    encoder list import (initialization)

2.  ASIN import and manipulations (gather the data for prediction)

3.  Trained model import (load the prediction model)

4.  ASIN prediction and export (perform the desired action and export)

## The model performance {#the-model-performance .CPEX-Section}

The dataset extracted consists of 587k data points (SIOC: 187k,
non-SIOC: 400k), split into 80% training data, 10% validation data, and
10.% test data. The breakdown is as follows. The validation data was
used for verification during training, and the test data was held out
purely for evaluating the prediction results.

The transition of Logloss and Precision-Recall-AUC scores for the
validation data is shown in the graph below. The graph on the right
displays the trend of the AUC score for both training and validation
datasets. The AUC score increases and stabilizes, with the training AUC
reaching almost perfect performance (close to 1.0) and the validation
AUC also showing high performance, indicating that the model performs
well in distinguishing between SIOC and non-SIOC.

![SIOC training accuracy graph](/assets/img/SIOC_training_accuracy_graph.png)

The below confusion matrix for the test data highlights that the number
of false positives and false negatives is relatively low compared to the
number of true positives and true negatives, indicating the model\'s
robustness.

| Prediction result | Accuracy   | Formula                                | Description                                                |
|-------------------|------------|----------------------------------------|------------------------------------------------------------|
| Accuracy          | **96.2%**  | $$\frac{TP + TN}{TP + TN + FP + FN}$$  | How often a classification ML model is correct overall     |
| Precision         | **93.5%**  | $$\frac{\text{TP}}{TP + FP}$$          | How often an ML model is correct when predicting the target class |
| Recall            | **94.7%**  | $$\frac{\text{TP}}{TP + FN}$$          | Whether an ML model can find all objects of the target class |

The Precision-Recall Cunfusion Matrix is shown as follows. The model
shows high accuracy, precision, and recall, indicating strong overall
performance.

## Appendix:
### SIOC Blind Test Details

Our team performed a random sample test to capture foundational
information about SIOC candidate ASINs.

**Sample Size Estimation**

Our team first identified the number of ASINs to capture via testing.
The standard formula for calculating error is:

$$
\frac{e}{\text{sample\ error}} = \ t\sqrt{\frac{N - n}{N - 1} \times \frac{p\left( 1 - p \right)}{n}}
$$

Where:
- *N*: population size
- *p*: SIOC rate
- *n*: sample size
- *t*: error coefficient


The population of ASINs we plan to sample was calculated at 695,697
ASINs.

The SIOC rate is the quantity we intended to estimate using this
process. We made some assumptions using data available to us and set the
SIOC rate *p* to 8.2%.

When the sample error *e* is 0.01 at 95% confidence, it means that we
are 95% confident that the population would have a true SIOC rate within
the 8.2 plus/minus 1% range. Given our trust in our existing SIOC data, we
were fairly confident that the true SIOC rate was between 7.2% and 9.2%.

Finally, we calculated the number of samples we should collect to
represent the population, according to our assumptions:

$$n = \frac{N}{\left( \frac{e}{1.96} \right)^{2}\frac{N - 1}{p\left( 1 - p \right)} + 1} = \frac{695,697}{\left( \frac{0.01}{1.96} \right)^{2}\frac{695,697 - 1}{0.082\left( 1 - 0.082 \right)} + 1} \approx 2,880$$

According to our calculations, by checking at least 2,880 randomly
selected ASINs, following the assumptions that 8.2 plus/minus 1% of the
sampled ASINs are SIOC, we would have 95% confidence that the
population's SIOC rate is also in the 8.2 plus/minus 1% range.

**Performing the Test**

Our team members visited one FC and manually inspected randomly sampled
ASINs, recording their results to paper. Team members participated for
multiple days, inspecting as many ASINs as they could find within the
allotted time for inspection. The goal was to inspect at least 2,880
ASINs, as mentioned above.

**Results**

After performing the sample testing, the team cataloged the results and
found that 2,915 inspections had been performed. Of those, 227 ASINs
(7.7%) were found to meet the SIOC criteria (which is within
expectations). Our team uses this 7.7% SIOC rate for calculating
entitlements. Separately, these results were used to evaluate various
machine learning models prior to implementation.