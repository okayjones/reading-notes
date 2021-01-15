# Data Analysis

## Jupyter Lab

[Source: What is Jupyter Lab?](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html)

`Juptyr Notebook` - document combining live runnable code with narrative text

- Text editor
- Terminal
- Images
- PDFs, html, LaTex

## NumPy

[Source:NumPy Tutorial: Data Analysis with Python](https://www.dataquest.io/blog/numpy-tutorial-python/)

- `NumPy` - commonly used data analysis package
- Work with multidimensional arrays
  
### Create an Array

1. import `numpy`
2. Pass list of lists into `numpy.array()` to convert to numpy array
 -- exclude any header ray (splice list)
 -- use keyword arg `dtype` to specify type

```python
import numpy as np
import csv

with open("winequality-red.csv", 'r') as f:
    wines = list(csv.reader(f, delimiter=";"))

    #print(wines[:3])
    # -> [['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates', 'alcohol', 'quality'], ['7.4', '0.7', '0', '1.9', '0.076', '11', '34', '0.9978', '3.51', '0.56', '9.4', '5'], ['7.8', '0.88', '0', '2.6', '0.098', '25', '67', '0.9968', '3.2', '0.68', '9.8', '5']]

wines = np.array(wines[1:], dtype=np.float)
```

#### Create 3x4 of 0's

```python
empty_array = np.zeros((3,4)) empty_array
```

#### Create 3x4 of randoms

```python
np.random.rand(3,4)
```

### Read Files

- Automatically assigns proper data type
- Can ignore header

```python
wines = np.genfromtxt("winequality-red.csv", delimiter=";", skip_header=1)
```

### Working w/ 2-D NumPy Arrays

#### Indexing

- Index using row, column

```python
wines[2,3]
```

#### Slicing

- Use colon to select more than one

```python
# array([ 1.9, 2.6, 2.3])
wines[0:3,3] or wines[:3,3]

# entire 4th column array([ 1.9, 2.6, 2.3, ..., 2.3, 2. , 3.6])
wines[:,3]

# entire 4th row
wines[3,:]

# all
wines[:,:]
```

#### Value Assignment

```python
wines[1,5] = 10

#replace entire column
wines[:,10] = 50
```

### Other Types

- 1-D Arrays (vectors) also work

```python
third_wine = wines[3,:]
11.200
0.280
0.560
1.900
0.075
17.000
60.000
0.998
3.160
0.580
9.800
6.000

third_wine[1]
0.28000000000000003

```

- N-Dimensional (not as common)

### Data Types

- Mostly maps to python data types
  - float — numeric floating point data.
  - int — integer data.
  - string — character data.
  - object — Python objects.

### Converting data types

- Create copy of array with specific type

```python
wines.astype(int)
```

- Check type

```python
int_wines = wines.astype(int)
int_wines.dtype.name

'int64'
```

### Array Operations

- Apply function to all items

```python
# creates new array
wines[:,11] + 10 #all in column

# multiply in place
wines[:,11] += 10
```
