# Terms 
- shape: number of elements in each dimension
-> elements of array outside in. 
[2,1] shape: `[[1],[1]] 
[2,2] shape: `[[1,2],[1,2]] 
![[Pasted image 20220924224548.png]]
- rank: deepest level of nested arrays 
rank 0: 0 (scalar)
rank 1: `[1]`
rank 2: `[[2],[2]]`


batch size: number of samples process between weight update
epoch: 1 epoch = 1 all dataset forward and backpropagation (1 cycle o training)
# Process 
### Load 
```python
# Create a dataset.
dataset = keras.preprocessing.image_dataset_from_directory(
  'path/to/main_directory', batch_size=64, image_size=(200, 200))

# For demonstration, iterate over the batches yielded by the dataset.
for data, labels in dataset:
   print(data.shape)  # (64, 200, 200, 3)
   print(data.dtype)  # float32
   print(labels.shape)  # (64,)
   print(labels.dtype)  # int32
```
### Preprocessing 
```
TextVectorization
Normalization 
Image Augmentation
```
### Building Models 
> [The Functional API](https://keras.io/guides/functional_api/)
```python

img_inputs = keras.Input(shape=(32, 32, 3)) #input layer

dense = layers.Dense(64, activation="relu") #dense layer
x = dense(inputs)                         # connect dense to input

x = layers.Dense(64, activation="relu")(x) #another dense layer
outputs = layers.Dense(10)(x) #connect dense layers 

model = keras.Model(inputs=inputs, outputs=outputs, name="mnist_model")
# final model, can be view as a layer
model.summary()
keras.utils.plot_model(model, "my_first_model.png") 

model.save("path_to_my_model")
del model
# Recreate the exact same model purely from the file:
model = keras.models.load_model("path_to_my_model")

data = np.random.randint(0, 256, size=(64, 200, 200, 3)).astype("float32")
processed_data = model(data)



```
### Train model 
- compile first (activation, loss funcs...) 
- call fit() to train
```python
model.compile(optimizer='rmsprop', loss='categorical_crossentropy')


model.fit(numpy_array_of_samples, numpy_array_of_labels,
          batch_size=32, epochs=10)
```
### metrics 
- callbacks: fn run at diffrent time of training  
- tensorboard 
### Evalutions 
# Concepts 
### CPU & GPU training 
 [Site Unreachable](https://towardsdatascience.com/overcoming-data-preprocessing-bottlenecks-with-tensorflow-data-service-nvidia-dali-and-other-d6321917f851)
# tensorflow
--- 
# Refererences 
[Training & evaluation with the built-in methods](https://keras.io/guides/training_with_built_in_methods/)
[The Functional API](https://keras.io/guides/functional_api/)
[The Sequential model](https://keras.io/guides/sequential_model/)

2022 09 24 22:45
#cheatsheet [[machine learning]]