# Data Analytics Project

This project is one of the parts in the Motion detection project in **ICT710: software Design for Embedded Systems**, which is a responsibility by **Isada Sukprapa (sadanalog)**.

## Introduction

The Data analytics task is used to analyze the collected data from Edge-device sensors that include on Gyroscope and accelerometer sensors, for predicting that device is moving whether or not. Based on pattern recognition, the project is considered as a classification problem, to classify the incoming data according to labeled data.

[Back to TOC](#table-of-contents)

## Table of Contents

- [Introduction](#introduction)
- [Features Description](#features-description)
- [Process Pipeline Design](#process-pipeline-design)
- [Data Exploration](#data-exploration)
- [Data Preprocessing](#data-preprocessing)
- [Model Selection](#model-selection)
- [Fine-tune Model](#fine-tune-model)
- [See Also](#see-also)

[Back to TOC](#table-of-contents)

## Features Description

In the dataset, there are six features following that: roll, pitch, and yaw come from gyroscope sensors, and the acc_x, acc_y, and acc_z,  from the accelerometer sensor. The question is how these sensors are measured?

### Gyroscrope

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/bsGFAi1WknY/0.jpg)](https://youtu.be/bsGFAi1WknY)

Sensor data: roll, pitch, yaw

A gyroscope is a device that uses Earth's gravity to help determine orientation. Its design consists of a freely-rotating disk called a rotor, mounted onto a spinning axis in the center of a larger and more stable wheel. As the axis turns, the rotor remains stationary to indicate the central gravitational pull, and thus which way is "down.". 

### Accelerometer

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/i2U49usFo10/0.jpg)](https://youtu.be/i2U49usFo10)

Sensor Data: acc_x, acc_y, acc_z

An accelerometer is a compact device designed to measure non-gravitational acceleration. When the object it's integrated into goes from a standstill to any velocity, the accelerometer is designed to respond to the vibrations associated with such movement. It uses microscopic crystals that go under stress when vibrations occur, and from that stress, a voltage is generated to create reading on any acceleration. Accelerometers are important components to devices that track fitness and other measurements in the quantified self-movement.



[Back to TOC](#table-of-contents)

## Process Pipeline Design

This is the design of the whole analytics process. In order to work on analysis data, the plan and draft throw us an idea of how the process can be done. Moreover, it makes us verify the necessary step in the system.

For the system pipeline, it is consist of files following:

- `2dataframe.ipynb`
- `exploration.ipynb`
- `preprocessing.ipynb`
- `model.ipynb`

### Converting dataset to DF type

This file is used for converting the text file dataset into the DataFrame. Using the function `load_data` and `convert2df`.

### Virtualization

This is a step of exploration data, to figure out the insight and understanding truly of data behavior.

- `display_description`
    - Print the data description 

- `plot_bar`
    - Plot the quantity of class label 

- `plot_boxplot` 
    - Plot the feature boxplot to see the length of class 1 and class 0

- `plot_corr`
    - Plot the correlation matrix to see how to relate to each feature, in order to plot to scatter next

- `plot_scatter`
    - After see relevant, now we would like to see its distribution

- `plot_line`
    - This one is optional, what happens if we consider the data as the time-domain signal processing. in this state, I would like to if apply the Fourier Transform

### Data Cleansing

[<center><img src="https://static.wixstatic.com/media/625cd8_6c48afd2fa01487a9f2a04f46550e0d5~mv2.jpg/v1/fill/w_630,h_391,al_c,q_80,usm_0.66_1.00_0.01/625cd8_6c48afd2fa01487a9f2a04f46550e0d5~mv2.webp" width="300"></center>](outliers)

This one to ensure the data is in the state of read-to-train. For its procedure, it is a list of:

- **Balancing the gold standard.** For the unbalancing dataset, it affects a lot of performance in the machine learning model especially the probability-based model, the prediction will bias to the more dataset class.

- **Eliminate the unused features.** Sometimes data is very messy and too much considered irrelevant.

- **Eliminate the outlier.** When plotting the boxplot, we will see the outlier. The outlier will make the Machine Learning model consider noise as a real occurring feature. Therefore, the noise can mislead the threshold of the model.

- **Scaling down.** After peeking the dataset, I see some values are not the same scale. scaling down the value will make some machine learning models are easy to cut-off (in contrast Decision tree Model doesn't improve-performance affect the scaling).


### Train ML Models and Evaluation

In this stage, I will start to train the machine learning model using cross-validation (k-fold) to measure the f1-score and average conditional probability in evaluation. For selecting the model it depends on the behavior of dataset and values. The selected model will be dump into the server-side for use in the prediction of the incoming data.

[Back to TOC](#table-of-contents)

## Data Exploration

Check the [notebook](https://github.com/ICT710-g4/data-analytics-project/blob/master/exploration.ipynb)

During the exploration, the insight data that we discover are:

- The labeled data is imbalanced
    - The imbalanced data can cause a Machine Learning model is biased.
- The data has much noise (outlinear)
    - The good feature data should have the same characteristic, the outliner makes data is fuzzier.
- The correlation matrix shows that every pair of features are correlated except a pair (acc_x, acc_z)
    - When a feature is an increase, the other will also increase. Or if it is still, others will move. This makes the model will easily classify the features.

- The distribution of data is distinguished
    - According to the correlation, we want to see it more clearly how is correlated. and we found that the data distribution can be separated by some kernel.
- We found some behaviors
    - At the end of the last record, both data have something the same. I guess that it occurs when the collector tries to unplug the edge-device.
- Even the data is signal, but it is not periodic.
    - Using the FFT algorithm, we found that the signal contains much frequency. 

[Back to TOC](#table-of-contents)

## Data Preprocessing

Check the [notebook](https://github.com/ICT710-g4/data-analytics-project/blob/master/preprocessing.ipynb)

For data processing, the things that we have done are

1. Removing the outliners
2. Balancing the label data

For feature elimination, we see that every feature is still important in case of keeping the data characteristic.

For scaling down, it is no needed since every feature is on the same scale, from 1000 to 10000.

[Back to TOC](#table-of-contents)

## Model Selection

Check the [notebook](https://github.com/ICT710-g4/data-analytics-project/blob/master/model.ipynb)

Now, we train ML models from the dataset and measure every model using F1. We found the most of them perform very well in cleaned data (f-score = 1). So, we point out the messy data which have f-score lower. In this, the stage we decide to use Gradient Boosting Classifier.

[<center><img src="https://miro.medium.com/max/1400/1*5_ZAlFhlCk8llhnYWD5PXw.png" width="200"></center>](f1-socre)

Since, in my opinion, Gradient Boosting Classifier is noise-tolerant more than other machine learning algorithms. Gradient boosting is a machine learning technique for regression and classification problems, which produces a prediction model in the form of an ensemble of weak prediction models, typically decision trees.


[Back to TOC](#table-of-contents)

## Fine-tune Model

Check the [notebook](https://github.com/ICT710-g4/data-analytics-project/blob/master/model.ipynb)

In this process, we need to find the best values for Gradient Boosting Classifier parameters. The parameters that we can fine-tune the following:
- learning_rate
- N_estimators
- max_depth
- min_samples_split
- min_samples_leaf
- max_features

[Back to TOC](#table-of-contents)

## See Also

- [Data Mining in Brief](https://towardsdatascience.com/data-mining-in-brief-26483437f178)
- [Accelerometer vs Gyroscope](https://www.livescience.com/40103-accelerometer-vs-gyroscope.html)
- [Gradient Boosting from scratch](https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d)
- [In depth parameter ttuning for Gradient Boosting](https://medium.com/all-things-ai/in-depth-parameter-tuning-for-gradient-boosting-3363992e9bae)

[Back to TOC](#table-of-contents)



