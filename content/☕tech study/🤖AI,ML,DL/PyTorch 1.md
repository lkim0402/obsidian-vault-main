# Tensors 
- A specialized data structure similar to arrays and matrices
- In PyTorch, tensors are used to encode the inputs and outputs of a model, and the model's parameters
- Similar to NumPy's `ndarrays`, except tensors can run on GPUs or other hardware accelerators
	- tensors and NumPy arrays often share the same underlying memory, which eliminates the need to copy data
- [Tensor basics (PyTorch Docs)](https://docs.pytorch.org/tutorials/beginner/basics/tensorqs_tutorial.html)
## Initializing
```python
import torch
import numpy as np

data = [[1,2], [3,4]]

# directly from data
x_data = torch.tensor(data)

# from a numpy array
np_array = np.array(data)
x_np = torch.from_numpy(np_array)

# from another tensor
x_ones = torch.ones_like(x_data) # retains properties of x_data
```

Initializing with  with random/constant values
```python
shape = (2,3,) # shape is a tuple of tensor dimensions
rand_tensor = torch.rand(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)

# outputs
Random Tensor:
 tensor([[0.8337, 0.3246, 0.3503],
        [0.4280, 0.2519, 0.1214]])

Ones Tensor:
 tensor([[1., 1., 1.],
        [1., 1., 1.]])

Zeros Tensor:
 tensor([[0., 0., 0.],
        [0., 0., 0.]])
```

## Attributes
- Tensor attributes describe their **shape**, **datatype**, and the **device** on which they are stored
```python
tensor = torch.rand(3,4)

print(f"Shape of tensor: {tensor.shape}") # torch.Size([3, 4])
print(f"Datatype of tensor: {tensor.dtype}") # torch.float32
print(f"Device tensor is stored on: {tensor.device}") # cpu
```
## Operations
- Over 1200 tensor operations, including arithmetic, linear algebra, matrix manipulation (transposing, indexing, slicing), sampling and more are comprehensively described [here](https://pytorch.org/docs/stable/torch.html)

Standard NumPy-like indexing and slicing
- [[List (python)#Slicing|List (python) - Slicing]]
```python
tensor = torch.ones(4, 4)
print(f"First row: {tensor[0]}")
print(f"First column: {tensor[:, 0]}")
print(f"Last column: {tensor[..., -1]}")
tensor[:,1] = 0
print(tensor)

# output
First row: tensor([1., 1., 1., 1.])
First column: tensor([1., 1., 1., 1.])
Last column: tensor([1., 1., 1., 1.])
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
```

### Arithmetic
```python
# This computes the matrix multiplication between two tensors. y1, y2, y3 will have the same value
# tensor.T returns the transpose of a tensor
y1 = tensor @ tensor.T

y2 = tensor.matmul(tensor.T)

y3 = torch.rand_like(y1)
torch.matmul(tensor, tensor.T, out=y3)

# This computes the element-wise product. z1, z2, z3 will have the same value
z1 = tensor * tensor
z2 = tensor.mul(tensor)

z3 = torch.rand_like(tensor)
torch.mul(tensor, tensor, out=z3)

# output
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
```

---
# Loading Data
- PyTorch has two primitives to work with data (data loading utility)
	- `torch.utils.data.Dataset`
		- stores samples and their corresponding labels
		- Ex) `torchvision.datasets`  module contains `Dataset` objects for real world vision data like CIFAR and COCO and FasionMNIST
	- `torch.utils.data.DataLoader`
		- wraps an iterable around the `Dataset`, supports automatic batching, sampling, shuffling, and multiprocess data loading
		- After getting the `Dataset`, we pass it to `DataLoader` as an argument

```python
import torch
from torch import nn
from torch.utils.data import DataLoader
from torchvision import datasets
from torchvision.transforms import ToTensor

# Download training data from open datasets.
training_data = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor(),
)

# Download test data from open datasets.
test_data = datasets.FashionMNIST(
    root="data",
    train=False,
    download=True,
    transform=ToTensor(),
)

batch_size = 64

# Create data loaders
train_dataloader = DataLoader(training_data, batch_size=batch_size)
test_dataloader = DataLoader(test_data, batch_size=batch_size)

for X, y in test_dataloader:
    print(f"Shape of X [N, C, H, W]: {X.shape}")
    print(f"Shape of y: {y.shape} {y.dtype}")
    break

# Out:
# Shape of X [N, C, H, W]: torch.Size([64, 1, 28, 28])
# Shape of y: torch.Size([64]) torch.int64
```


# Creating Models
- To create a neural network in PyTorch, write a class that inherits from `nn.Module`
	- define the layers of the network in the `__init__` function and specify how data will pass through the network in the `forward` function
```python
device = torch.accelerator.current_accelerator().type if torch.accelerator.is_available() else "cpu"
print(f"Using {device} device")

# Define model
class NeuralNetwork(nn.Module):
    def __init__(self):
        super().__init__()
        self.flatten = nn.Flatten()
        self.linear_relu_stack = nn.Sequential(
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 512),
            nn.ReLU(),
            nn.Linear(512, 10)
        )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.linear_relu_stack(x)
        return logits

model = NeuralNetwork().to(device)
print(model)
```

# Optimizing Model parameters
- To train a model, we need a loss function and an optimizer
	- loss function - a mathematical process that quantifies the error margin between a model's prediction and the actual target value
	- optimizer - an algorithm used to change a model's internal parameters, like weights, to minimize the difference between its predictions and the actual data

```python
loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=1e-3)

# training loop
def train(dataloader, model, loss_fn, optimizer):
    size = len(dataloader.dataset)
    model.train()
    for batch, (X, y) in enumerate(dataloader):
        X, y = X.to(device), y.to(device)

        # Compute prediction error
        pred = model(X)
        loss = loss_fn(pred, y)

        # Backpropagation
        loss.backward()
        optimizer.step()
        optimizer.zero_grad()

        if batch % 100 == 0:
            loss, current = loss.item(), (batch + 1) * len(X)
            print(f"loss: {loss:>7f}  [{current:>5d}/{size:>5d}]")

# testing loop  
def test(dataloader, model, loss_fn):
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    model.eval()
    test_loss, correct = 0, 0
    with torch.no_grad():
        for X, y in dataloader:
            X, y = X.to(device), y.to(device)
            pred = model(X)
            test_loss += loss_fn(pred, y).item()
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()
    test_loss /= num_batches
    correct /= size
    print(f"Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>8f} \n")

# training + testing conducted over epochs
epochs = 5
for t in range(epochs):
    print(f"Epoch {t+1}\n-------------------------------")
    train(train_dataloader, model, loss_fn, optimizer)
    test(test_dataloader, model, loss_fn)
print("Done!")
```
- Training 
	- In a single training loop, the model makes predictions on the training dataset (fed to it in batches), and backpropagates the prediction error to adjust the model’s parameters.
- Testing
	- We also check the model’s performance against the test dataset to ensure it is learning.

# Saving and loading models
```python
# save
torch.save(model.state_dict(), "model.pth")
print("Saved PyTorch Model State to model.pth")

# loading
model = NeuralNetwork().to(device)
model.load_state_dict(torch.load("model.pth", weights_only=True))

# Example usage of the loaded model
classes = [
    "T-shirt/top",
    "Trouser",
    "Pullover",
    "Dress",
    "Coat",
    "Sandal",
    "Shirt",
    "Sneaker",
    "Bag",
    "Ankle boot",
]
model.eval()
x, y = test_data[0][0], test_data[0][1]
with torch.no_grad():
    x = x.to(device)
    pred = model(x)
    predicted, actual = classes[pred[0].argmax(0)], classes[y]
    print(f'Predicted: "{predicted}", Actual: "{actual}"')
```
- Saving
	- A common way to save a model is to serialize the internal state dictionary (containing the model parameters).
- Loading
	- The process for loading a model includes re-creating the model structure and loading the state dictionary into it.