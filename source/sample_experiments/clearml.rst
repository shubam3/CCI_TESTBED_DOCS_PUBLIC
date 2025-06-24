.. _clearml_experiment:

ML based pathloss radio map predictor in ClearML
=================================================

This section provides a step-by-step guide to training a machine learning (ML) model for pathloss radio map prediction in indoor wireless networks using the TensorFlow/Keras framework on the ClearML platform. The process covers uploading data to the ClearML server, training and evaluating an ML model, and logging results—closely aligned with the given code.

Environment Setup and Library Imports
-------------------------------------

The necessary libraries for data handling, visualization, machine learning, and ClearML tracking are imported. Random seeds are set for reproducibility. Warnings are filtered for a cleaner output.

.. code-block:: python

   from numpy.random import seed
   seed(0)
   import tensorflow
   from tensorflow import keras
   tensorflow.random.set_seed(0)

   import math
   import numpy as np
   import pandas as pd
   import seaborn as sns
   import matplotlib.pyplot as plt
   from sklearn import metrics
   from sklearn.model_selection import train_test_split, cross_val_score, KFold
   from sklearn.metrics import accuracy_score, r2_score
   from sklearn import model_selection
   from sklearn.preprocessing import MinMaxScaler 
   from sklearn.decomposition import PCA
   from tensorflow.keras import layers, models, losses
   from tensorflow.keras.layers import Activation, LeakyReLU, PReLU, ELU, ReLU, Dropout, BatchNormalization
   from tensorflow.keras.optimizers import SGD, Adam, RMSprop
   from tensorflow.keras.callbacks import LearningRateScheduler, History, EarlyStopping
   from plot_keras_history import plot_history
..    import warnings
..    warnings.filterwarnings('ignore')
..    warnings.simplefilter(action='ignore', category=FutureWarning)
..    warnings.filterwarnings('ignore', category=DeprecationWarning)


ClearML Task Initialization and Dataset Loading
----------------------------------------------

ClearML `Task.init()` initializes the experiment. The dataset is fetched from the ClearML server using `Dataset.get()`. HuggingFace's `load_dataset` is used to load all CSV files in the dataset directory and convert them to a pandas DataFrame.

.. code-block:: python

   import os
   from pathlib import Path
   from clearml import Dataset, Task
   from datasets import load_dataset

   task = Task.init(project_name="tf_project_1", task_name="baseline_model", output_uri=True)

   local_dataset_path = Path(Dataset.get(
       dataset_project="tf_project_1",
       dataset_name="radio_map_1",
       alias="radio_map_1"
   ).get_local_copy())

   # Filter for CSV files
   csv_files = [csv_path for csv_path in os.listdir(local_dataset_path) if csv_path.endswith(".csv")]

   dataset = load_dataset(
       "csv",
       data_files=[str(local_dataset_path / csv_path) for csv_path in csv_files],
       split="all"
   )

   df = dataset.to_pandas()

.. figure:: ../../_static/clearml_task_init.png
   :alt: ClearML task initialization
   :align: center
   :width: 600px

   Figure 1: ClearML task initialization in Python code.

Dataset Overview and Exploration
-------------------------------

This section provides an overview of the dataset, including displaying basic information and statistics, as well as uploading and previewing the dataset in the ClearML dashboard.

.. figure:: ../../_static/clearml_dataset.png
   :alt: Dataset overview screenshot (ClearML dashboard and logs)
   :align: center
   :width: 600px

   Figure 2: ClearML dashboard showing dataset upload and preview.


Data Preprocessing
------------------

After loading the dataset, preprocessing steps are performed: Rows with a path loss of 250 are removed. Input (X, Y) and output (Path Loss) are separated. Nulls are dropped and data is scaled using MinMaxScaler.


.. code-block:: python

   X_actual = df[['X(m)','Y(m)']]
   y_actual = df[['Path Loss (dB)']]

   df['Path Loss (dB)'] = np.where(df['Path Loss (dB)'] == 250, np.nan, df['Path Loss (dB)'])
   df = df.dropna()
   x = df[['X(m)', 'Y(m)']].values
   y = df[['Path Loss (dB)']].values

   x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.3, random_state=0)
   scaler1 = MinMaxScaler()
   x_train = scaler1.fit_transform(x_train)
   x_test = scaler1.transform(x_test)
   scaler2 = MinMaxScaler()
   y_train = scaler2.fit_transform(y_train)
   y_test = scaler2.transform(y_test)

   X_actual_arr = X_actual.values
   X_actual_norm = scaler1.fit_transform(X_actual_arr)

.. figure:: ../../_static/dataframe_info.png
   :alt: DataFrame after cleaning and preprocessing
   :align: center
   :width: 600px

   Figure 3: DataFrame info after cleaning and preprocessing.

Model Definition
----------------

A Sequential Keras model is defined with three hidden layers using ReLU activations. BatchNormalization, Dropout, and other advanced layers can be added as needed.

.. code-block:: python

   def baseline_model():
       model = Sequential()
       model.add(Dense(64, input_dim=x.shape[1], activation='relu', kernel_initializer='random_normal'))
       # model.add(BatchNormalization())
       # model.add(Dropout(0.2))
       model.add(Dense(32, activation='relu', kernel_initializer='random_normal'))
       # model.add(BatchNormalization())
       # model.add(Dropout(0.2))
       model.add(Dense(16, activation='relu', kernel_initializer='random_normal'))
       model.add(Dense(y.shape[1], activation='relu', kernel_initializer='random_normal'))
       model.compile(optimizer=Adam(learning_rate=0.001), loss='mean_squared_error', metrics=['mean_absolute_error'])
       return model


Model Training with Early Stopping
----------------------------------

The model is trained with early stopping based on validation loss. Training history is plotted using `plot_history()` for visualization.

.. code-block:: python

   m = baseline_model()
   early_stopping = keras.callbacks.EarlyStopping(monitor="val_loss", patience=5, verbose=2)

   import time
   start_time = time.time()
   history = m.fit(x_train, y_train, validation_data=(x_test, y_test), callbacks=[early_stopping], batch_size=16, epochs=120)
   end_time = time.time()
   duration = end_time - start_time

   plot_history(history.history)
   task.get_logger().report_matplotlib_figure('Loss curve', "latest model", plt)

.. figure:: ../../_static/clearml_training.png
   :alt: Training and validation loss curve
   :align: center
   :width: 600px

   Figure 5: Training and validation loss curve during model training.

Evaluation, Prediction, and Metrics Logging
-------------------------------------------

Predictions are made on the test set, and performance is evaluated using MSE, RMSE, MAE, and R2 metrics. Model predictions are made on the full dataset, then inverse-scaled back to the original values for interpretation. All key performance metrics and training duration are logged using ClearML's reporting utilities.

.. code-block:: python

   y_pred = m.predict(x_test)
   print("Test Mean Squared error (MSE):", metrics.mean_squared_error(y_test, y_pred))
   print("Test Root mean squared error (RMSE):", np.sqrt(metrics.mean_squared_error(y_test, y_pred)))
   print("Test Mean absolute error (MAE):", metrics.mean_absolute_error(y_test, y_pred))
   y_pred_flat = y_pred.flatten()
   y_test_flat = y_test.flatten()
   print("R2 Score Test:", metrics.r2_score(y_test_flat, y_pred_flat))

   y_pred_all = m.predict(X_actual_norm)
   y_pred_all_inv = scaler2.inverse_transform(y_pred_all)

   task.get_logger().report_single_value("Test Mean Squared error (MSE)", metrics.mean_squared_error(y_test, y_pred))
   task.get_logger().report_single_value("Test Root mean squared error (RMSE)", np.sqrt(metrics.mean_squared_error(y_test, y_pred)))
   task.get_logger().report_single_value("Test Mean absolute error (MAE)", metrics.mean_absolute_error(y_test, y_pred))
   task.get_logger().report_single_value("Training time (seconds)", duration)

.. figure:: ../../_static/clearml_training2.png
   :alt: Evaluation metrics screenshot
   :align: center
   :width: 600px

   Figure 6: Evaluation metrics and logs in ClearML dashboard.

.. .. figure:: ../../_static/clearml_predictions.png
..    :alt: Predictions screenshot
..    :align: center
..    :width: 600px

..    Figure 7: Model predictions and post-processing results.

.. .. figure:: ../../_static/clearml_metrics.png
..    :alt: Metrics reported in ClearML
..    :align: center
..    :width: 600px

..    Figure 8: Metrics reported and logged in ClearML.

ClearML Dashboard: Training Results
----------------------------------

After completing the model training and evaluation, the ClearML dashboard provides a visual summary of the loss and mean absolute error curves for the completed training task.

.. figure:: ../../_static/clearml_evaluation.png
   :alt: ClearML dashboard showing loss and mean absolute error curves
   :align: center
   :width: 600px

   Figure 7: ClearML dashboard showing loss and mean absolute error curves for the completed training task.

Saving the Model
----------------

The trained Keras model is saved locally for reuse.

.. code-block:: python

   m.save('./serving_model.keras')

For architectural details and integration, see the :ref:`ClearML Architecture <clearml_architecture>` in the Software Architecture section.

