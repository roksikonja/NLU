## Natural Language Understanding
## Project 2



### SCT absolute classification ```bert_sct.py/ipynb``` 

#### Data Format

Data format in ```./data_pp/```:
* ```sct.train.tsv```
* ```sct.validation.tsv```
* ```sct.test.tsv```

unique_id |	label |	text_a |	text_b |	vs_sent1 |	vs_sent2 |	vs_sent3 |	vs_sent4 |	vs_sent5 | cs_dist
--- | ---- | ---- |---- |---- |---- |---- |---- |---- | ---
c9e0ad...5 |	0 |	 The football team ... scored. |	The crowd ... loudly. |	[-0.1027  0.167   0.833   0.    ] |	[0.765 0.    0.577 0.423] |	[0. 0. 1. 0.] |	[0. 0. 1. 0.] |	[0. 0. 1. 0.] | [1. 1. 1. 1.]

Preprocessed data is obtained by running ```preprocess_data.py``` with initial data files stored in ```./data/```.

#### RUN Mode

Using tensorflow-hub module.
```
python bert_sct.py 
--data_dir=./data_pp/ 
--output_dir="C:\Users\roksi\output" 
--do_train=True 
--do_eval=True 
--do_predict=True 
--bert_model_hub="https://tfhub.dev/google/bert_uncased_L-12_H-768_A-12/1"
```

Using checkpoint, downloaded from: https://storage.googleapis.com/bert_models/2018_10_18/cased_L-12_H-768_A-12.zip.
```
python bert_sct.py 
--data_dir=./data_pp/ 
--output_dir="C:\Users\roksi\output" 
--do_train=True 
--do_eval=True 
--do_predict=True 
--init_checkpoint=./bert/bert_model.ckpt 
--bert_dir=./bert/
```

Using checkpoint, pretrained on training set with masked language model and next sentence prediction loss.
```
python bert_sct.py 
--data_dir=./data_pp/ 
--output_dir="C:\Users\roksi\output" 
--do_train=True 
--do_eval=True 
--do_predict=True 
--init_checkpoint=./bert_masked_lm_pp/model.ckpt 
--bert_dir=./bert/ 
--num_train_epochs=0.01
```

### SCT relative classification ```bert_sct.py/ipynb``` 

#### Data Format

Data format in ```./data_pp/```:
* ```sct_v2.train.tsv```
* ```sct_v2.validation.tsv```
* ```sct_v2.test.tsv```

unique_id |	label |	text_a |	text_b _pos | text_b _neg |	vs_sent1 |	vs_sent2 |	vs_sent3 |	vs_sent4 |	vs_sent5_pos | vs_sent5_neg | cs_dist_pos | cs_dist_neg
--- | ---- | ---- |---- |---- |---- |---- |---- |---- | --- | --- | --- | ---
c...5 |	1 |	 The ... |	The ... | The ... |	[-0.1027  0.167   0.833   0.    ] |	[0.765 0.    0.577 0.423] |	[0. 0. 1. 0.] |	[0. 0. 1. 0.] |	[0. 0. 1. 0.] |	[0. 0. 1. 0.] | [1. 1. 1. 1.] | [-1. -1. -1. -1.]

Preprocessed data is obtained by running ```preprocess_data.py``` with initial data files stored in ```./data/```.

#### RUN Mode

Using tensorflow-hub module.
```
python bert_sct.py 
--data_dir=./data_pp/ 
--output_dir="C:\Users\roksi\output" 
--do_train=True 
--do_eval=True 
--do_predict=True 
--bert_model_hub="https://tfhub.dev/google/bert_uncased_L-12_H-768_A-12/1"
```

Using checkpoints, change accordingly as before.


Local directory hierarchy.

![Local directory hierarchy.](./docs/dir.PNG?raw=false "Local directory hierarchy.")


### Experiments

#### Experiment A (FINAL)

* Parameters
    * BERT initialization: ```https://tfhub.dev/google/bert_uncased_L-12_H-768_A-12/1```
    * Data: ```data_pp_test/``` 
  
    ``` df_train_test = pd.concat([df_train_tmp.iloc[0:data_train_val.shape[0]], df_train.iloc[-data_train_val.shape[0]:]], axis=0)``` 
    
    * Classification: Relative
    * ```num_epochs = 3.0, batch_size = 8, max_seq_length = 400, learning_rate = 0.5, warmup_proportion = 0.1```
    * Sentiment: NO
    * Common sense: NO
    * Main: ```bert_sct_v2.py```

* Performance
    * eval_accuracy = 0.8787554
    * eval_loss = 0.39823273
    * global_step = 1403
    * loss = 0.39823273
    * precision = 1.0
    * recall = 0.8787554

* Output
    * Eval: ```09_08-53eval_results.txt```
    * Test: ```09_08-53test_results.tsv```
    
#### Experiment B (FINAL)

* Parameters
    * BERT initialization: ```https://tfhub.dev/google/bert_uncased_L-12_H-768_A-12/1```
    * Data: ```data_pp/``` 
  
    ``` df_train = df_train_val``` 
    
    * Classification: Relative
    * ```num_epochs = 3.0, batch_size = 8, max_seq_length = 400, learning_rate = 0.5, warmup_proportion = 0.1```
    * Sentiment: NO
    * Common sense: NO
    * Main: ```bert_sct_v2.py```

* Performance
    * eval_accuracy = 0.86212444
    * eval_loss = 0.51910084
    * global_step = 701
    * loss = 0.51910084
    * precision = 1.0
    * recall = 0.86212444

* Output
    * Eval: ```09_09-39eval_results.txt```
    * Test: ```09_09-39test_results.tsv```

#### Experiment C (FINAL)

* Parameters
    * BERT initialization: ```https://tfhub.dev/google/bert_uncased_L-12_H-768_A-12/1```
    * Data: ```data_pp_test/```     
    * Classification: Absolute
    * ```num_epochs = 3.0, batch_size = 8, max_seq_length = 400, learning_rate = 0.5, warmup_proportion = 0.1```
    * Sentiment: NO
    * Common sense: NO
    * Main: ```bert_sct.py```

* Performance
    * eval_accuracy = 0.8581371
    * eval_loss = 0.99690187
    * f1 = 1.0
    * global_step = 1403
    * loss = 0.99690187

* Output
    * Eval: ```09_10-20eval_results.txt```
    * Test: ```09_10-20test_results.tsv```
    
#### Experiment D (FINAL)

* Parameters
    * BERT initialization: ```./bert_masked_lm_pp```
    * Data: ```data_pp_test/```     
    * Classification: Absolute
    * ```num_epochs = 3.0, batch_size = 8, max_seq_length = 400, learning_rate = 0.5, warmup_proportion = 0.1```
    * Sentiment: NO
    * Common sense: NO
    * Main: ```bert_sct.py```

* Performance
    * eval_accuracy = 0.8506424
    * eval_loss = 1.0681794
    * f1 = 1.0
    * global_step = 1403
    * loss = 1.0681794

* Output
    * Eval: ```09_11-18eval_results.txt```
    * Test: ```09_11-18test_results.tsv```
    
#### Experiment E (FINAL)

* Parameters
    * BERT initialization: ```https://tfhub.dev/google/bert_uncased_L-12_H-768_A-12/1```
    * Data: ```data_pp/``` TS1 Full     
    * Classification: Absolute
    * ```num_epochs = 1.0, batch_size = 8, max_seq_length = 400, learning_rate = 0.5, warmup_proportion = 0.1```
    * Sentiment: NO
    * Common sense: NO
    * Main: ```bert_sct_v2.py```

* Performance
    * eval_accuracy = 0.7988197
    * eval_loss = 0.51480865
    * global_step = 11721
    * loss = 0.51480865
    * precision = 1.0
    * recall = 0.7988197

* Output
    * Eval: ```09_22-09eval_results.txt```
    * Test: ```09_22-09test_results.tsv```
    

#### Experiment F (FINAL)

* Parameters
    * BERT initialization: ```https://tfhub.dev/google/bert_uncased_L-12_H-768_A-12/1```
    * Data: ```data_pp/``` TS1 Training only     
    * Classification: Absolute
    * ```num_epochs = 1.0, batch_size = 8, max_seq_length = 400, learning_rate = 0.5, warmup_proportion = 0.1```
    * Sentiment: NO
    * Common sense: NO
    * Main: ```bert_sct_v2.py```

* Performance
    * eval_accuracy = 0.5767167
    * eval_loss = 0.6829421
    * global_step = 0
    * loss = 0.6829421
    * precision = 1.0
    * recall = 0.5767167

* Output
    * Eval: ```10_04-38eval_results.txt```
    * Test: ```10_04-38test_results.tsv``` 

