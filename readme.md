# TFIDF Related Experiments #

## Summary ##
This folder contains files related to   
1. the __"TFIDF + RidgeCV"__,  
2. the __"TFIDF + Random Forest"__  
experiments of RWP multi-label theme classification  

## Data ##
The experiments use the development set of RWP dataset. It contains 29912 records.  

## Norms and Symbols Commonly Used ##
### Experiment ID's ###
id1: Supervised FastText  
id2: Unsupervised FastText + RidgeCV  
id3: TFIDF + RidgeCV  
id4: Unsupervised FastText + Random Forest  
id5: TFIDF + Random Forest  
  
This doc focuses on id3 and id5.  
  
file suffix:  
"id_" refers to results obtained from 10-fold cross validation (each time training on 90% dev set and test on 10% dev set). Results usually have dimension 29912.  
"id_x" or "id_x_10" refers to results obtained from training on 90% dev set and test on 10% dev set. Results usually have dimension 2991.  

### Abbrevations ###
dev: develoment set  
ft: FastText  
rf: Random Forest  
lbl: label  
mlb: multilabel

## Flow of "TFIDF + Classifier" ##
### Step 0: Define Input and Output ###
Input: Raw doc in training set, and their labels.  
Output: Labels and Proba of each label of each doc in test set, as well as the result for on-demand evaluation metrics.  

### Step 1: Get the TFIDF Embeddings for All Doc in Dev Set ###
This step requires the customized TFIDF module __[link]__.  
After following the steps specified in the customized TFIDF module, the doc and their {word:tfidf} result can be obtained from the self.document variable.  
This result is stored in file __"dict_list.json"__.  

### Step 2: Vectorization of TFIDF dictionary ###
By transforming the __"dict_list.json"__ file using DictVectorizer (sklearn) and vstack (scipy), we can transform the dict file into sparse vectors, which can be directly used for sklearn models.  

### Step 3: Prepare the Training and Testing datasets ###
Related files:  
1. __"id_list.json"__: A list of string, dim 29912 * 1. It records the sequence of document id's in dev set.  
2. __"dev_train_index_90.json"__: A list of integer, dim 26921 * 1. It identifies what are the training record indices. The training set is 90% of dev set, and the indices range is 0 - 29911.  
3. __"dev_test_index_10.json"__: A list of integer, dim 2991 * 1. It identifies what are the test record indices. The test set is 10% of dev set, and the indices range is 0 - 29911.  

### Step 4: Apply ML models ###
Apply RidgeCV or Random Forest Models on prepared data and get predicted data that contain proba.   

### Step 5: Get Evaluations ###
Put what obtained from Step 4 and the truth as arguments of multilabel evaluator (the __MLB_Evaluation__ class or the __evaluation__ method in it), then you can get on-demand evaluation metric results.  

## JSON Result files ##
### NDCG files, each number is NDCG for a record ###
result_ndcg_best_tfidf_RidgeCV_id3x_10  
result_ndcg_best_tfidf_RidgeCV_id3  
  
result_ndcg_best_tfidf_rf_id5x_10  
result_ndcg_best_tfidf_rf_id5  

### Proba files, each list is [lbl,proba] for a record ###
result_lbl_n_prob_best_tfidf_RidgeCV_id3x_10  
result_lbl_n_prob_best_tfidf_RidgeCV_id3x_10_unprocessed  
result_lbl_n_prob_best_tfidf_RidgeCV_id3  
  
result_lbl_n_prob_best_tfidf_rf_id5x_10  
result_lbl_n_prob_best_tfidf_rf_id5x_10_unprocessed  
result_lbl_n_prob_best_tfidf_rf_id5  







