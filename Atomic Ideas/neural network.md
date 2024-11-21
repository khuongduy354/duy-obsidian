- input -> hidden -> output layer 
- neuron: 
	- connected with weights, 
	- has 1 bias, 
	- has 1 value = Weighted Sum (of previous nodes) + bias 
- a densed neural network has each neuron connected 
- an [[ml functions#activation function|activation function]]  is used to shift input data 
## backpropagation 
- in training process, after get final out put 
-> calculate loss using loss function, go back 
-> change weights & biases  
with [[ml functions|gradient descent]] 

# convolutionary 
> output a map of features (not true false)  

-> use for analyzing local changes 
not global changes (pixel by pixel)

# Implementations 
```python
INPUT
model = keras.Sequential([
    keras.layers.Flatten(input_shape=(28, 28)),  # input layer (1)  #flatten means turn the 2D dataset (28x28) to 1D 
    keras.layers.Dense(128, activation='relu'),  # hidden layer (2) #ddense means ensely connected
    keras.layers.Dense(10, activation='softmax') # output layer (3) #softmax calc prob dist, 0 1 ,for each neutron 
])

COMPILE
model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])



TRAIN
model.fit(train_images, train_labels, epochs=10)  
```







# neural network
--- 
# Refererences 




2022 09 24 18:03
#cheatsheet [[machine learning]] [[Supervised Learning]]