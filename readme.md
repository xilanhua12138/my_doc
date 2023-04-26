# Model Description
&emsp;&emsp;We modeled the three questions (MCQ, TF, NUM) separately in the competition. These models are all fine-tuned models based on FiD's T5 v1.1. The difference is that the structure of the output part has been adjusted.
1. The MCQ model uses the original FiD seq2seq structure. We concatenated the answers and options as the model's label. For example, the answer "A" and the options "Under 7, Between 7 and 9, More than 9" are concatenated into "A. Under 7". After training, the model uses a cross-encoder to measure the semantic similarity between the output and each option during the testing stage. It then outputs the result after softmax. This is because we noticed that the options are often related, for example, in the above example, when the true answer is A, choosing B is closer to the true answer than choosing C. So this is more reasonable. We also verified our idea in the Hugging Face demo.
2. We adjusted the structure of FiD for the TF model. A fully connected layer was added after the last hidden layer, and then the probability of choosing yes or no was obtained using softmax. Here, the details during training are: soft labels were used instead of strictly setting the labels to [0, 1] or [1, 0]. Label smoothing with epsilon = 0.2 was used to make the labels label_smooth [epsilon, 1-epsilon] or [1-epsilon, epsilon]. In addition, considering that human experts cannot confidently predict whether something will happen, we incorporated human predictions into the labels. If the human prediction is 0.2, predicts_human = [0.2, 0.8]. If the prediction is 0.9, predicts_human = [0.1, 0.9]. Finally, the label is label = (label_smooth + predicts_human) / 2.
3. For the NUM model, a fully connected layer was also added after the last hidden layer. Here, the sigmoid activation function was used to obtain a value between 0 and 1.
Here is a schematic diagram:
![[Pasted image 20230426122957.png]]
![[Pasted image 20230426122925.png]]
![[Pasted image 20230426123006.png]]

# Passage Retrival
&emsp;&emsp;For the article retrieval method, we utilize semantic retrieval. First, we utilize the all-MiniLM-L6-v2 model for semantic encoding, encoding it into vector space. Similarly, we encode one million articles. By comparing the inquiry encoding and article encodings' proximity, we can achieve searching for articles corresponding to the inquiry. To expedite search speed, referencing the Faiss project, we construct a large vector space for the article database corresponding index, rapidly searching via Faiss algorithms for k articles most proximate to the inquiry. Here, we set k=20. We then utilize the ms-marco-MiniLM-L-6-v2 model, a cross encoder, to further analyses the relevance of inquiries and articles, re-ranking. Finally, we opt for the top 10 articles as input for the FiD model.

# Faithful Prediction
&emsp;&emsp;To make faithful predictions, first, we used the C4 article retrieval dataset, which was released in 2020, to ensure that the test questions would not be exposed to future information. And the model we used was also approved in the competition. Secondly, during the testing phase, we did not use any predictions from humans. We only used articles retrieved based on the C4 dataset. Here, it should be noted that during the training stage, we did not specifically ensure that future information leakage related to the retrieved articles was prevented. We only ensured that it was not leaked during the testing stage. In addition, for questions without answers during the training stage, we selected the last crowd forecast before the close time as the answer.
# How to Use
1.To convert the question into a format that FiD can accept
```python
python ./builddataset/datasetbuild_test.py
```
**Remember to change your data set path in py file.

2.Retrieving relevant articles for your question
```python
python ./builddataset/build_autocast_dataset_with_c4.py
```
**Remember to change your input path and output path in py file, here wo don't need to split the dataset because now is the test stage.
It is advisable to utilize two conda environments here, as incompatible version issues require you to create separate environments for the FiD model and the current sentence retrieval system (Sentence Transformer) respectively.

3.To carry out prediction
```python
python ./test/valid_uni.py
```
**Remember to change your input path and output path in py file, here wo don't need to split the dataset because now is the test stage.

&emsp;&emsp;The relevant pre-trained models and resources required for article retrieval can be downloaded from this link: 

&emsp;&emsp;It should be noted that num_without_predict corresponds to the model for predicting NUM, mc_without_predict corresponds to the model for predicting MCQ, and tf_without_predict corresponds to the model for predicting TF. index_c4_realnewslike_more_1M.faiss constitutes the constructed Faiss index vector space. passages_with_time_stamp_c4_realnewslike_1M.json contains one million C4 news articles. autocast_questions_k_10_test_without_predict.json is the test set with articles that have already been retrieved.

