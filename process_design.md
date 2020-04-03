# Process Pipeline

Written by `sadanalog`

## Outline

This is the design of the whole process. In order to work on analysis data, the plan and draft throw us an idea of how the process can be done. Moreover, it makes us verify the necessary step in the system.

For the system pipeline, it is consist of files following:

- `2dataframe`
- `exploration.ipynb`
- `preprocessing.ipynb`
- `model.ipynb`

## Converting dataset to DF type

This file used for converting the text file dataset into the DataFrame. Using the function `load_data` and `convert2df`.

## Virtualization

This is a step of exploration data, to figure out the insight and understanding truly of data behavior.

- `display_description`
    - print the data description 

- `plot_bar`
    - plot the quantity of class label 

- `plot_boxplot` 
    - plot the feature boxplot to see the length of class 1 and class 0

- `plot_corr`
    - plot the correlation matrix to see how to relate to each feature, in order to plot to scatter next

- `plot_scatter`
    - after see relevant, now we would like to see its distribution

- `plot_line`
    - This one is optional, what happens if we consider the data as the time-domain signal processing. in this state, I would like to if apply the Fourier Transform

## Data Cleansing

This one to ensure the data is in the state of read-to-train. For its procedure, it is a list of:

- **Balancing the gold standard.** for the unbalancing dataset, it affects a lot of performance in the machine learning model especially the probability-based model, the prediction will bias to the more dataset class.

- **Eliminate the unused features.** Sometimes data is very messy and too much considered irrelevant.

- **Eliminate the outlier.** When plotting the boxplot, we will see the outlier. The outlier will make the Machine Learning model consider noise as a real occurring feature. Therefore, the noise can mislead the threshold of the model.

- **Scaling down.** After peeking the dataset, I see some values are not the same scale. scaling down the value will make some machine learning models are easy to cut-off (in contrast Decision tree Model doesn't improve-performance affect the scaling).


## Train ML Models and Evaluation

In this stage, I will start to train the machine learning model using cross-validation (k-fold) to measure the f1-score and average conditional probability in evaluation. For selecting the model it depends on the behavior of dataset and values. The selected model will be dump into the server-side for use in the prediction of the incoming data.

---