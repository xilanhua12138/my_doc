# Model Description
&emsp;&emsp;We modeled the three questions (MCQ, TF, NUM) separately in the competition. These models are all fine-tuned models based on FiD's T5 v1.1. The difference is that the structure of the output part has been adjusted.
1. The MCQ model uses the original FiD seq2seq structure. We concatenated the answers and options as the model's label. For example, the answer "A" and the options "Under 7, Between 7 and 9, More than 9" are concatenated into "A. Under 7". After training, the model uses a cross-encoder to measure the semantic similarity between the output and each option during the testing stage. It then outputs the result after softmax. This is because we noticed that the options are often related, for example, in the above example, when the true answer is A, choosing B is closer to the true answer than choosing C. So this is more reasonable. We also verified our idea in the Hugging Face demo.
2. We adjusted the structure of FiD for the TF model. A fully connected layer was added after the last hidden layer, and then the probability of choosing yes or no was obtained using softmax. Here, the details during training are: soft labels were used instead of strictly setting the labels to [0, 1] or [1, 0]. Label smoothing with epsilon = 0.2 was used to make the labels label_smooth [epsilon, 1-epsilon] or [1-epsilon, epsilon]. In addition, considering that human experts cannot confidently predict whether something will happen, we incorporated human predictions into the labels. If the human prediction is 0.2, predicts_human = [0.2, 0.8]. If the prediction is 0.9, predicts_human = [0.1, 0.9]. Finally, the label is label = (label_smooth + predicts_human) / 2.
3. For the NUM model, a fully connected layer was also added after the last hidden layer. Here, the sigmoid activation function was used to obtain a value between 0 and 1.
Here is a schematic diagram:
![[Pasted image 20230426122957.png]]
![[Pasted image 20230426122925.png]]
![[Pasted image 20230426123006.png]]

# Faithful Prediction
