--- 
 
# required metadata 
title: "Machine Learning Text Transform" 
description: "Text transforms that can be performed on data before training" 
keywords: "transform, featurizer, text" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

## ``featurize_text``: Convert text columns into numerical features


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.featurize_text(cols: [<class ‘str’>, <class ‘dict’>, <class ‘list’>], language: [‘AutoDetect’, ‘English’, ‘French’, ‘German’, ‘Dutch’, ‘Italian’, ‘Spanish’, ‘Japanese’] = ‘English’, stopwords_remover=None, case: [‘Lower’, ‘Upper’, ‘None’] = ‘Lower’, keep_diacritics: bool = False, keep_punctuations: bool = True, keep_numbers: bool = True, dictionary: dict = None, word_feature_extractor={‘name’: ‘NGram’, ‘settings’: {‘weighting’: ‘Tf’, ‘allLengths’: True, ‘ngramLength’: 1, ‘skipLength’: 0, ‘maxNumTerms’: [10000000]}}, char_feature_extractor=None, vector_normalizer: [‘None’, ‘L1’, ‘L2’, ‘LInf’] = ‘L2’, **kargs)
```




### Description

Text transforms that can be performed on data before training
a model.


### Details

The ``featurize_text`` transform produces a bag of counts of
sequences of consecutive words, called n-grams, from a given corpus of text.
There are two ways it can do this:

* build a dictionary of n-grams and use the id in the dictionary as the index in the bag; 

* hash each n-gram and use the hash value as the index in the bag. 

The purpose of hashing is to convert variable-length text documents into
equal-length numeric feature vectors, to support dimensionality reduction
and to make the lookup of feature weights faster.

The text transform is applied to text input columns. It offers language
detection, tokenization, stopwords removing, text normalization and feature
generation. It supports the following languages by default: English, French,
German, Dutch, Italian, Spanish and Japanese.

The n-grams are represented as count vectors, with vector slots
corresponding either to n-grams (created using ``n_gram``) or to
their hashes (created using ``n_gram_hash``). Embedding ngrams in a vector
space allows their contents to be compared in an efficient manner.
The slot values in the vector can be weighted by the following factors:

* term frequency - The number of occurrences of the slot in the text 

* inverse document frequency - A ratio (the logarithm of inverse relative slot frequency) that measures the information a slot provides by determining how common or rare it is across the entire text. 

* term frequency-inverse document frequency - the product term frequency and the inverse document frequency. 


### Arguments


##### cols

A character string or list of variable names to transform. If
``dict``, the keys represent the names of new variables to be created.


##### language

Secifies the language used in the data set. The following
values are supported:

* ``"AutoDetect"``: for automatic language detection. 

* ``"English"``. 

* ``"French"``. 

* ``"German"``. 

* ``"Dutch"``. 

* ``"Italian"``. 

* ``"Spanish"``. 

* ``"Japanese"``. 


##### stopwords_remover

Specifies the stopwords remover to use. There are
three options supported:

* *None* No stopwords remover is used. 

* ``predefined``: A precompiled language-specific lists of stop words is used that includes the most common words from Microsoft Office. 

* ``custom``: A user-defined list of stopwords. It accepts the following option: ``stopword``. 

The default value is *None*.


##### case

Text casing using the rules of the invariant culture. Takes the
following values:

* ``"Lower"``. 

* ``"Upper"``. 

* ``"None"``. 

The default value is ``"Lower"``.


##### keep_diacritics

``False`` to remove diacritical marks; ``True`` to
retain diacritical marks. The default value is ``False``.


##### keep_punctuations

``False`` to remove punctuation; ``True`` to
retain punctuation. The default value is ``True``.


##### keep_numbers

``False`` to remove numbers; ``True`` to retain
numbers. The default value is ``True``.


##### dictionary

A dictionary of whitelisted terms which accepts
the following options:

* ``term``: An optional character vector of terms or categories. 

* ``dropUnknowns``, and 

* ``sort``: Specifies how to order items when vectorized. Two orderings are

      supported: ``"occurrence"``: items appear in the order encountered.
      ``"value"``: items are sorted according to their default comparison.
      For example, text sorting will be case sensitive (e.g., ‘A’ then ‘Z’
      then ‘a’).

The default value is *None*.
Note that the stopwords list takes precedence over the dictionary whitelist
as the stopwords are removed before the dictionary terms are whitelisted.


##### word_feature_extractor

Specifies the word feature extraction arguments. There
are two different feature extraction mechanisms:

* ``n_gram()``: Count-based feature extraction (equivalent to WordBag). It accepts the following options: ``max_num_terms`` and ``weighting``. 

* ``n_gram_hash()``: Hashing-based feature extraction (equivalent to WordHashBag). It accepts the following options: ``hash_bits``, ``seed``, ``ordered`` and ``invert_hash``. 

The default value is ``n_gram``.


##### char_feature_extractor

Specifies the char feature extraction arguments. There
are two different feature extraction mechanisms:

* ``n_gram()``: Count-based feature extraction (equivalent to WordBag). It accepts the following options: ``max_num_terms`` and ``weighting``. 

* ``n_gram_hash()``: Hashing-based feature extraction (equivalent to WordHashBag). It accepts the following options: ``hash_bits``, ``seed``, ``ordered`` and ``invert_hash``. 

The default value is *None*.


##### vector_normalizer

Normalize vectors (rows) individually by rescaling
them to unit norm. Takes one of the following values:

* ``"None"``. 

* ``"L2"``. 

* ``"L1"``. 

* ``"LInf"``. 

The default value is ``"L2"``.


##### kargs

Additional arguments sent to compute engine.


### Returns

An object defining the transform.


### Example



```
'''
Example with featurize_text and rx_logistic_regression.
'''
import numpy
import pandas
from microsoftml import rx_logistic_regression, featurize_text, rx_predict
from microsoftml.entrypoints._stopwordsremover_predefined import predefined


train_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Do not like it", "Really like it",
        "I hate it", "I like it a lot", "I kind of hate it", "I do like it",
        "I really hate it", "It is very good", "I hate it a bunch", "I love it a bunch",
        "I hate it", "I like it very much", "I hate it very much.",
        "I really do love it", "I really do hate it", "Love it!", "Hate it!",
        "I love it", "I hate it", "I love it", "I hate it", "I love it"],
    like=[True, False, True, False, True, False, True, False, True, False,
        True, False, True, False, True, False, True, False, True, False, True,
        False, True, False, True]))
        
test_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Really like it", "I hate it",
        "I like it a lot", "I love it", "I do like it", "I really hate it", "I love it"]))

out_model = rx_logistic_regression("like ~ review_tran",
                    data=train_reviews,
                    ml_transforms=[
                        featurize_text(cols=dict(review_tran="review"),
                            stopwords_remover=predefined(),
                            keep_punctuations=False)])
                            
# Use the model to score.
score_df = rx_predict(out_model, data=test_reviews, extra_vars_to_write=["review"])
print(score_df.head())
```



### See also

[``n_gram``](n_gram.md),
[``n_gram_hash``](n_gram_hash.md),
[``n_gram``](custom.md),
[``n_gram_hash``](predefined.md),
[``get_sentiment``](get_sentiment.md).


### Example



```
'''
Train and test reviews.
'''
import pandas

train_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Do not like it", "Really like it",
        "I hate it", "I like it a lot", "I kind of hate it", "I do like it",
        "I really hate it", "It is very good", "I hate it a bunch", "I love it a bunch",
        "I hate it", "I like it very much", "I hate it very much.",
        "I really do love it", "I really do hate it", "Love it!", "Hate it!",
        "I love it", "I hate it", "I love it", "I hate it", "I love it"],
    like=[True, False, True, False, True, False, True, False, True, False,
        True, False, True, False, True, False, True, False, True, False, True,
        False, True, False, True]))
        
test_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Really like it", "I hate it",
        "I like it a lot", "I love it", "I do like it", "I really hate it", "I love it"]))
```



### Example



```
'''
Example with featurize_text and rx_logistic_regression.
'''
import numpy
import pandas
from microsoftml import rx_logistic_regression, featurize_text, rx_predict
from microsoftml.entrypoints._stopwordsremover_predefined import predefined


train_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Do not like it", "Really like it",
        "I hate it", "I like it a lot", "I kind of hate it", "I do like it",
        "I really hate it", "It is very good", "I hate it a bunch", "I love it a bunch",
        "I hate it", "I like it very much", "I hate it very much.",
        "I really do love it", "I really do hate it", "Love it!", "Hate it!",
        "I love it", "I hate it", "I love it", "I hate it", "I love it"],
    like=[True, False, True, False, True, False, True, False, True, False,
        True, False, True, False, True, False, True, False, True, False, True,
        False, True, False, True]))
        
test_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Really like it", "I hate it",
        "I like it a lot", "I love it", "I do like it", "I really hate it", "I love it"]))

out_model = rx_logistic_regression("like ~ review_tran",
                    data=train_reviews,
                    ml_transforms=[
                        featurize_text(cols=dict(review_tran="review"),
                            stopwords_remover=predefined(),
                            keep_punctuations=False)])
                            
# Use the model to score.
score_df = rx_predict(out_model, data=test_reviews, extra_vars_to_write=["review"])
print(score_df.head())
```
