# lymbID
Tech Test for LymbicAI

How did the methodology influence their data-set?

The researchers utilize EEG datasets of resting brain states (i.e eyes closed and relaxed). We want to use this type of EEG-data here to essentially filter out bio-disturbances in the data before pre-processing. For example in the paper, we note that opening and closing eyes cause a disturbance in the EEG signal. This will be similar for other markers including jaw clenching etc. To ensure that we extrapolate the purest signal, a resting brain-state will ensure that we record signals that are unique to the individual. This is essential for the authentication use-case as we dont want our model to learn the parameters that are similar across individuals, rather the opposite.


Note that in the paper, the researchers pre-process the EEG-data to further the above concept. As delta brain waves were proven to vary the least (the waves associated with a subjects lower awareness), the recorded data from the EEG data-set is passed through a butter-worth band-pass filter. This essentially extrapolates the delta waves from the data-set. Assuming that the DC offset is removed, and the data is normalized, the remaining pre-processed values can now be used to train the RNN.


How would you improve the model approach that was used?


To improve the model approach, I would first want to try and use a bi-directional LSTM layer such that past and future events in the EEG time-series are taken into account. For the identification use-case I believe that this would add more robustness to the system, such that participants are further distinguishable. One issue with the paper is that the test data used is solely from recorded resting state signals. In an ideal system this would not be the case, as emotional fluctuations in the signals would likely affect the accuracy of the system. I propose that bi-directional LSTMs could be the solution to this problem. Similarly, additional basic LSTM layers could be used to further the latter.


I would want to test the system on another dataset, that does not use resting state brain data. Although the latter is great to use for training the RNN model, the resulting accuracy using the same dataset is likely to be high. Hence, a dataset with more fluctuations from real-life encounters should be used to validate the model accuracy and robustness. Further pre-processing steps may need to be used to account for the latter (i.e emotional state) however it is clear from the paper that delta waves are dominant in the case of identification.


Which hyper-parameters would you be looking to change?


In terms of hyper-parameters, I would want to additionally experiment with increasing the number of neurons (units) within each hidden layer of the network. Similarly, changing the initial bias variable of hidden layers, as this will converge at a local optimum (not a global optimum) via gradient descent. Furthermore, finding the optimum activation function for each layer. The batch size and max epocs would also need to be adjust. We want to alter these while taking into account overfitting and underfitting of the model.


What other model approach would work well in your opinion?


Typically RNNs with LSTM layers will always work best for time-series data for deep learning. Similarly we could use a TDNN (time-delay NN) however although I believe it would be suitable for the task, LSTM RNNs generally outperform TDNN.


We could also use a CNN with LSTM layers, this could theoretically outperform the RNN with LSTM layers but more research needs to be done for this comparison. A hybrid of the above could be most useful here, as used in the following paper: https://www.mendeley.com/catalogue/5e9d8cd2-a518-3abb-a099-a77a317985bd/ , this could be essential for the case when using real data with higher fluctuations in emotional states. Given the time to implement and research, I believe a similar architecture would the perform well for this task in the real world, with bi-directional LSTM layers. For example we would add Convolutional layers before the LSTM layers, to identify any features that could not be filtered out during the pre-processing stage.


To avoid adding additional complexity to the model internally as above, an alternative approach in
https://ieeexplore.ieee.org/document/9364129 could be adopted, and is my preferred approach to the task. Essentially, the researchers here append an additional input feature (facial image data) to the EEG-data used within the training of the LSTM-RNN. The outputs of the LSTM-RNN are then features that are used for clustering within SVM that is used to identify an individual.
