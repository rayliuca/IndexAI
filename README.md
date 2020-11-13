# IndexAI-DNN
Using Deep Neural Networks to predict the SP500 index

---
## Intro
Here I tried MLP using scikit-learn, MLP, LSTM, CNN using TensorFlow/Keras

## Method

To properly train/predict the index, we cannot use the raw values as it increases over time. The raw values even just a few years ago are completely useless to train the models since all the models tested here are decision trees. They don’t perform any mathematical calculations on the data. To solve this, instead of using the raw values, I used the ratio of the value we want to predict and the most recent value we have access to. To put it more plainly:

![\Large r= \frac{ Data_{i+predict\_distance}}{Data_{i}}](https://latex.codecogs.com/gif.latex?r=&space;\frac{&space;Data_{i&plus;predict\_distance}}{Data_{i}})

For example, if we are predicting the index 30 days away:

![\Large r= \frac{ Data_{i+30}}{Data_{i}}](https://latex.codecogs.com/gif.latex?r=&space;\frac{&space;Data_{i&plus;30}}{Data_{i}})

This moving normalization/ ratio formula was chosen so the data would remain in the same ballpark for all the values. In a distanced sense, this is like a derivative, as both are describing the rate of change. Noting the ratio has the index of i+predict_distance, otherwise, we will have a leak.

Besides the normalization, trendlines are also very important for improving the prediction. For the current implementation, only the first and second order polynomial extrapolations are used since the linear (first order) extrapolation provides most of the benefit.


## Results

Sklearn MLP, 30 days ratio prediction:

  <a href="./assets/30days_ratio_prediction_mlp_sk.png"><img src="./assets/30days_ratio_prediction_mlp_sk.png" style="margin-left: 10%" height=400px></a>

  <p style="text-align:center">MAE: 0.667</p>

TensorFlow MLP, 30 days ratio prediction:

  <a href="./assets/30days_ratio_prediction_mlp_tf.png"><img src="./assets/30days_ratio_prediction_mlp_tf.png" style="margin-left: 10%" height=400px></a>
  <p style="text-align:center">MAE score: 0.596</p>



LSTM, 30 days ratio prediction:

<a href="./assets/30days_ratio_prediction_lstm.png"><img src="./assets/30days_ratio_prediction_lstm.png" style="margin-left: 10%" height=400px></a>
<p style="text-align:center">MAE score: 1.015</p>


CNN, 30 days ratio prediction:

<a href="./assets/30days_ratio_prediction_cnn.png"><img src="./assets/30days_ratio_prediction_cnn.png" style="margin-left: 10%" height=400px></a>
<p style="text-align:center">MAE score: 0.437</p>

As we can see, CNN performs the best in this bunch. If we would to compare it to the actual index:

<a href="./assets/30days_prediction_cnn.png"><img src="./assets/30days_prediction_cnn.png" style="margin-left: 10%" height=400px></a>

It predicted the SP500 pretty well. The cross-correlation shows promising results:

  <a href="./assets/30days_prediction_cnn_cc.png"><img src="./assets/30days_prediction_cnn_cc.png" style="margin-left: 10%" height=400px></a>

Lets try the prediction distance of 90 days:

 <a href="./assets/90days_prediction_cnn.png"><img src="./assets/90days_prediction_cnn.png" style="margin-left: 10%" height=400px></a>

cross-correlation:

 <a href="./assets/60days_prediction_XGB_2018-08.png"><img src="./assets/90days_prediction_cnn_cc.png" style="margin-left: 10%" height=400px></a>

The prediction performs surprisingly well, compared to tree-based models I tried before. The MAE is 0.840 for the ratio prediction. Noting the smoothed SP500 dataset is obtained by passing the original data through a Blackman filter with a window size equal to the prediction distance, so the MAE values are not directly comparable to the MAEs of other prediction distances. To test the limit of the model, let's try a prediction distance of 180 days:

 <a href="./assets/180days_prediction_cnn.png"><img src="./assets/180days_prediction_cnn.png" style="margin-left: 10%" height=400px></a>

 The MAE has degraded to 2.430, which means the ratio only deviated 2.430% from the smoothed dataset, on average. However, it still did a pretty good job in terms time delays

   <a href="./assets/180days_prediction_cnn_cc.png"><img src="./assets/180days_prediction_cnn_cc.png" style="margin-left: 10%" height=400px></a>


## TODO

- deploy the trained model and wrap the code so it can send the prediction periodically (i.e. monthly/ bi-weekly) through email

## License
My work is released under the MIT license
