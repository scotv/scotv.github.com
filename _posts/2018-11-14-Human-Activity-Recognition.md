
Human activity recognition is the problem of classifying sequences of accelerometer data recorded by specialized harnesses or smart phones into known well-defined movements.

## Classical Approach

Our first step is extracting features from the raw data using feature engineering. We usually create sensor speicific or domain specific features and it requires an expert to analyze the data and engineer the feature required for the model.

Once the data is ready we can employ traditional machine learning models like SVM, KNN CLassifier, Random Forest, Gradient Bossting etc.

One of the major drawback of this approach was its need for an expert for engieerning the features for the model to fit. Moreover the features extracted will be sensor specific and domain speciific. 

We need to robust approach where the model or the preprocessing is itself able to understand these features and give us robust classification. This led to Neural Netwiork Architecture

## Neural Network
Neural Network are special class of machine learning algorithms that are inspired by biological neurons. 
Recently with the advent to huge amount data generated from accelerometer and gyroscope from  the mobile phone, Neural netoworks have started expanding in the field of Humna Activity Recognition and producing state-of-art results. They learn from raw sesor data and are producing better results from the models derived from classical approach.

We are going to discuss two major Neural Network Arcchitecute used for Human Activity
 Reocgnition-
 1. Convolutionlal Neural Network
 2. Recurrent Neurlal Network
 
 
 ### Convolutional Neural Network
 Although they are mostly used for understanding and analyziing images or video data. We can use 1D Convolutional Kernel to capture the dependecy of the senor data.
 
 *"CNN has two advantages over other models: local dependency and scale invariance. Local dependency means the nearby signals in HAR are likely to be correlated, while scale invariance refers to the scale-invariant for different paces or frequencies.""*

— Deep Learning for Sensor-based Activity Recognition: A Survey, 2018.


Following are few innovative and major papers used for Human Activiity Recognition-
1. [Convolutional Neural Networks for Human Activity Recognition using Mobile Sensors](https://ieeexplore.ieee.org/document/7026300)
2. [Divide and Conquer-Based 1D CNN Human Activity Recognition Using Test Data Sharpening.](http://www.mdpi.com/1424-8220/18/4/1055)
3. [Human Activity Recognition Using Wearable Sensors by Deep Convolutional Neural Networks.](https://dl.acm.org/citation.cfm?id=2806333)
 
 src([Deep Learning Models for Human Activity Recognition by Jason Brownlee](https://machinelearningmastery.com/deep-learning-models-for-human-activity-recognition/))
 
 
 Other than the 3rd paper all of them use Conv1D for analyzing the sequence, so we need to first unnderstand what is Conv1D and how does it work.
 
 
 ### Conv1D
  Convolutional 1D is a 1-dimension convolutinalm kernel that operates in similar fashion as in case ofd 2D convolution. It is usually used in analyzing sequence and capturing short term depedency in sequences. One major advantage it has over LSTM or any recurrent neural network is its mutifold better speed to train and test. Convolution 1D is also heavily used in Inception Network.
  
  Let's now get to coding....
  
  {% highlight python %}
#@title Default title text
import pandas as pd #loading the csv data
import 	numpy as np
import matplotlib.pyplot as plt #visualization
import os
from keras.utils import to_categorical
from keras.models import Sequential
import keras
 ## Importing python Libraries
{% endhighlight %}

We are going to use pandas for reading the csv file
  {% highlight python %}
def read_Txtfile(filename):
  data = pd.read_csv(filename, header = None,delim_whitespace = True)
  return data.values
{% endhighlight %}


We are going to use pandas for reading the csv file
  {% highlight python %}
def read_DataGroup(prefix,group,filenames):
  data = []
  for filename in filenames:
    for file in filename:
      temp = read_Txtfile(prefix+'/'+file)
      data.append(temp)
  
  train_X = np.dstack(data)
  return train_X

def load_Data(prefix,group):
  path = prefix + '/' + group + '/Inertial Signals' 
  filenames = []
  filenames.append(['total_acc_x_'+group+'.txt', 'total_acc_y_'+group+'.txt', 'total_acc_z_'+group+'.txt'])
  
  filenames.append(['body_acc_x_'+group+'.txt', 'body_acc_y_'+group+'.txt', 'body_acc_z_'+group+'.txt'])
  
  filenames.append(['body_gyro_x_'+group+'.txt', 'body_gyro_y_'+group+'.txt', 'body_gyro_z_'+group+'.txt'])
  
  X_train =  read_DataGroup(path,group,filenames)
  y_train = read_Txtfile(prefix+'/'+group+'/'+ 'y_'+group+'.txt')
  
  print (np.array(X_train).shape)
  print (y_train.shape)
  return np.array(X_train), y_train
 

{% endhighlight %}


We need one hot encode the y label

  {% highlight python %}

y_train-=1
y_test-=1
y_train = to_categorical(y_train)
y_test = to_categorical(y_test)

print ("Shape of y_train : ",y_train.shape)

print ("Shape of y_test : ",y_test.shape)

{% endhighlight %}
