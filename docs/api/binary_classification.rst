Classification – Binary
=========================

Using the GUI
-------------------------------
Training an ``XClassifier`` model with the embedded xplainable GUI is easy. Run
the following lines of code, and you can configure and optimise your model
within the GUI to minimise the amount of code you need to write.

Example – GUI
~~~~~~~~~~~~~~~~~~~~~~~
::
   
      import xplainable as xp
      import pandas as pd
      import os
      
      # Initialise your session
      xp.initialise(api_key=os.environ['XP_API_KEY'])

      # Load your data
      data = pd.read_csv('data.csv')

      # Train your model (this will open an embedded gui)
      model = xp.classifier(data)

Using the Python API
------------------------
You can also train an xplainable classification model programmatically. This
works in a very similar way to other popular machine learning libraries.

You can import the ``XClassifier`` class and train a model as follows:

Example – XClassifier()
~~~~~~~~~~~~~~~~~~~~~~~~~~
::
      
      from xplainable.core.models import XClassifier
      from sklearn.model_selection import train_test_split
      import pandas as pd

      # Load your data
      data = pd.read_csv('data.csv')
      x, y = data.drop('target', axis=1), data['target']
      x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2)

      # Train your model
      model = XClassifier()
      model.fit(x_train, y_train)

      # Predict on the test set
      y_pred = model.predict(x_test)

Example – PartitionedClassifier()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::
      
      from xplainable.core.models import PartitionedClassifier
      from xpainable.core.models import XClassifier
      import pandas as pd
      from sklearn.model_selection import train_test_split
      
      # Load your data
      data = pd.read_csv('data.csv')
      train, test = train_test_split(data, test_size=0.2)

      # Instantiate the partitioned model
      partitioned_model = PartitionedClassifier(partition_on='partition_column')

      # Train the base model
      base_model = XClassifier()
      base_model.fit(
            train.drop(columns=['target', 'partition_column']),
            train['target']
            )

      # Add the base model to the partitioned model (call this '__dataset__')
      partitioned_model.add_partition(base_model, '__dataset__')

      # Iterate over the unique values in the partition column
      for partition in train['partition_column'].unique():
            # Get the data for the partition
            part = train[train['partition_column'] == partition]
            x_train, y_train = part.drop('target', axis=1), part['target']
            
            # Fit the embedded model
            model = XClassifier()
            model.fit(x, y)

            # Add the model to the partitioned model
            partitioned_model.add_partition(model, partition)
      
      # Prepare the test data
      x_test, y_test = test.drop('target', axis=1), test['target']

      # Predict on the partitioned model
      y_pred = partitioned_model.predict(x_test)

Classifier Classes
~~~~~~~~~~~~~~~~~~~~~~~
.. automodule:: xplainable.core.ml.classification
    :members:
    :inherited-members:
    :undoc-members:
    :show-inheritance:


Hyperparameter Optimisation
-------------------------------
You can optimise ``XClassifier`` models automatically using the embedded GUI or
programmatically using the Python API. The speed of hyperparameter optimisation
with xplainable is much faster than traditional methods due to the concept
of **rapid refits** first introduced by xplainable. You can find documentation
on rapid refits in the advanced_concepts/rapid_refitting section.

The hyperparameter optimisation process uses a class called ``XParamOptimiser``
which is based on Bayesian optimisation using the `Hyperopt
<https://github.com/hyperopt/hyperopt>`_ library. Xplainable's wrapper has
pre-configured optimisation objectives and an easy way to set the search space
for each parameter. You can find more details in the XParamOptimiser docs.

Example
~~~~~~~~~~~~~~~~~~~~~~~
::

      from xplainable.core.models import XClassifier
      from xplainable.core.optimisation.bayesian import XParamOptimiser
      from sklearn.model_selection import train_test_split
      import pandas as pd

      # Load your data
      data = pd.read_csv('data.csv')
      x, y = data.drop('target', axis=1), data['target']
      x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2)
      
      # Find optimised params
      optimiser = XParamOptimiser(n_trials=200, n_folds=5, early_stopping=40)
      params = optimiser.optimise(x_train, y_train)

      # Train your optimised model
      model = XClassifier(**params)
      model.fit(x_train, y_train)


Optimiser Class
~~~~~~~~~~~~~~~~~~~~~~~
.. automodule:: xplainable.core.optimisation.bayesian
    :members:
    :inherited-members:
    :undoc-members:
    :show-inheritance:
