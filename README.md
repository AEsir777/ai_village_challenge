# time series forecasting
## compare sequence-to-vector RNN / sequence-to-sequence RNN / stateful sequence-to-sequence RNN / LSTM Cell / CNN
|  RNN model          |   sequence-to-vector   |   sequence-to-sequence | stateful | LSTM | LSTM with Conv1D as Preprocessing | WaveNet
| :-----:          | :---:            | :---:                             | :---: | :---:|  :---:|  :---:|
| best learning rate |  $3 \times 10^{-6}$ | $2 \times 10^{-6}$ | $1 \times 10^{-6}$ | $3 \times 10^{-7}$ | $6 \times 10^{-6}$ |$3 \times 10^{-3}$ |
|   MAE            |   7.2522078       |   5.2465625    | 5.9912305                        | 5.4648457 | 6.797924 |  5.760452|
|stopping epoches  |   174      |   455                              |  129 | 113| over 1000 | 169 |
- sequence-to-sequence RNN performes weight better than  the sequence-to-vector RNN for time series
- sequence-to-vector RNN performes very bad when there is a large change in the trend of the time series  
- LSTM performs similar to sequence-to-vector but with the highest efficiency and lowest best learning rate
- LSTM with Con1D as preprocessing layer has the longest epoches to train
- CNN perm similar to LSTM and sequence-to-sequence RNN
   
  

## tested on only 1 layer (linear model) and with 2 extra dense layers
target: a linear time series with trend, seanality and some noise
### change the learning rate acorrding to epochs

![image](https://user-images.githubusercontent.com/77596290/210108084-b23a3eb0-97ff-4bbc-a4cc-c03f307a372c.png)
*learning rate vs. loss for linear model*

![image](https://user-images.githubusercontent.com/77596290/210108104-4faebdd9-0b9d-40a5-85dc-51b085de28b0.png)
*learning rate vs. loss for more complex model*

- more complex model approches minumum faster than the linear model
- but they both behave equally well around learning rate = $10^{-4}$

### apply early stopping
|  target          |   linear model   |   model with 2 extra dense layers |
| :-----:          | :---:            | :---:                             |
|   mae            |   4.935261       |   5.4686675                           |
|stopping epoches  |   160   |   62                             |
- unsurprisingly, linear model performances bettern since the time series is meant to be linear
- more complex model stops earlier than linear model also because complex model is more likely to lead to overfitting

# NLP
## compare RNN and dense models for sentiment analysis
- both used subwords embedding  
- embeding dimention = 16  
- the first layer is embedding layer
- there is only dense layer which is the last layer

|  model          |  embedding model    |   CNN | GRU | LSTM | multilayer LSTM |
| :-----:          | :---:            | :---: | :---: | :---: | :---: |
|   specific layer   |   GlobalAveragePooling1D       |   Conv1D and GlobalAveragePooling1D | GRU(32) | LSTM(16) | double LSTM(16) |
|   learning rate   |   0.0001       |   0.0001                | 0.00005 | 0.00005 | 0.00005 | 
| validation accuracy (max) |   0.77   |   0.75     | 0.76 | 0.78 | 0.77 | 0.76 |
| mse on test dataset |   0.37773073   |   0.40608215    | 0.40457442 | 0.38270417 |0.45821545|
- since the test set only contains simple sentences that does not depend on words that are far from the key words, plain embedding model performs the best while multilayer LSTM is penalitied for overfitting  
- the validation set contains context that are harder to analyze, so LSTM performs the best

## explore tensorboard
model using embedding and textVectorization layer  
- vocabulary_size = 1000
- sequence_length = 100
- use flatten layer after embedding layer
![image](https://user-images.githubusercontent.com/77596290/217428943-7c110b06-b6ef-41ee-9f5a-e579a423de10.png)

- vocabulary_size = 500
- sequence_length = 50
- use globalAveragePool1D layer after embedding layer
![image](https://user-images.githubusercontent.com/77596290/217437590-7c9854ff-7480-4205-8bdb-9154e7c33c21.png)

- using subwords and TensorBoardPlugin
![image](https://user-images.githubusercontent.com/77596290/217669877-45707be3-2c7f-4916-b4b4-81a1efd87105.png)

# Keras parameter study
## conclusion (tested on **plain vanilla** and **CNN** model)
CNN have two extra convolution and pooling layers and others remain the same.

![image](https://user-images.githubusercontent.com/77596290/202949015-05f37562-48f4-467d-bbdc-5fca0fd1fab6.png)
*change epoch and normalization for plain vanilla model*

![image](https://user-images.githubusercontent.com/77596290/202955224-38fbf2a6-4f02-4012-b9af-6c0cbd5a1cba.png)
*change epoch and normalization for CNN model*

- reducing the epoches will decrease the accuracy
- not normalizing the activation for the first layer input will also decrease the accuracy: normalization seems to have a bigger impact for the plain vanilla model than the CNN
- CNN is more accurate when dealing with the fashion MNIST dataset

![image](https://user-images.githubusercontent.com/77596290/202863296-31bb1a40-268f-409e-ab98-ffad224c5299.png)
*change the number of neurons in first dense layer for plain vanilla model with 3 layers*

![image](https://user-images.githubusercontent.com/77596290/202863307-4a4567ec-adf4-4e82-887f-0c9b9a46bd0f.png)
*change the number of neurons in second dense layer for plain vanilla model with 4 layers*


![image](https://user-images.githubusercontent.com/77596290/202951821-9c2facab-bd79-4e2b-9212-4341cd01bb38.png)
*change the number of neurons in first dense layer for CNN*

![image](https://user-images.githubusercontent.com/77596290/202953633-18fae1dd-a351-4ce6-bd52-3791c264d1a8.png)
*change the number of neurons in second dense layer for CNN*


compare two tables where one table (left) represents a model with only three layers and controlled the number of neurons in the second layer and the other table (right) represens a model with 4 layers and controlled the number of neurons in the forth layer
- the model with more layers is slightly more accurate than the model with less layers
- as number of neurons increases, the accuracy tends to be higher; however, when the number of neurons is larger enough, there do not seem be that much difference and obvious trend for accuracy.
- overfitting with more neurons may lead to lower accuracy

# ctf
contains kaggle for ai_village_challenge uses numpy, pandas, scikit-learn for classification, clustering, dimention reduction.  

# transfer learning
## compare mobilenet_v2, inception_v3 transfer learning for flower dataset
condition: epoches = 6, only add a Dense layer, 7/3 train/validation
![image](https://user-images.githubusercontent.com/77596290/204699775-b58f1d9d-00d2-4d9c-8852-2f5264b8fe07.png)
*mobilenet_v2 accuracy(epoches 6) = 0.9494(train)/0.9083(validation)*

![image](https://user-images.githubusercontent.com/77596290/204699889-01de2099-e617-4297-9cdf-24fe20f98cec.png)
*inception_v3 accuracy(epoches 6) = 0.9354(train)/0.9010(validation)*

- **mobilenet_v2** fits the flower dataset bettern than **inception_v3**
reason for val accuracy exceeds training accuracy at the first epoch:
- training accuracy is measured during the epoch, however validation accuracy is only measured at the end of the epoch
- the model is **pre-trained** on flower images 
- there might be **image augmentation** layers for training dataset but not for validation dataset so the training data is harder to classify
- 
# image-recognition
## conclusion
implement **image augmentation** and **dropout** <br />
For flower dataset: apply <br />
randomWidth/Height = 0.15, horizontal RandomFlip, 45&deg; RandomRotation, 50% RandomZoom, Dropout=0.2 
![image](https://user-images.githubusercontent.com/77596290/204006200-1a292407-a8c4-456f-81a8-cb3b36293f77.png)
- the training loss begin to increase after 40 epochs, so overfitting did have effect on the accuracy for the validation set <br />
<br />
<br />
For dog and cat dataset: apply <br />
Horizontal and vertical RandomFlip, 36&deg; RandomRotation, 20% RandomZoom, 0.5 Dropout

![image](https://user-images.githubusercontent.com/77596290/203888888-f261bdd2-d2b9-4f95-a142-505a113ee918.png)
- For relatively large epoches (1-100), the traning and validation accuracy is still increasing, so these two methods effectively avoid **overfitting**

# R
some basic R usage
