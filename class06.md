# Game of Green

## Random module

[Source: How to use the Random Module in Python](https://www.pythonforbeginners.com/random/how-to-use-the-random-module-in-python)

[Python Random](https://docs.python.org/3/library/random.html)

- `random.randint` generate random int

```python
import random
print random.randint(0, 5)

# returns 1, 2, 3, 4, 5
```

- `random.random()` multiple for larger numbers

```python
import random
random.random() * 100

# between 0 - 100
```

- `random.choice()` - Random from a sequence

```python
import random
myList = [2, 109, False, 10, "Lorem", 482, "Ipsum"]
random.choice(myList)
```

- `random.shuffle()` - Shuffles list

```python
from random import shuffle
x = [[i] for i in range(10)]
shuffle(x)

# print x  gives  [[9], [2], [7], [0], [4], [5], [3], [1], [8], [6]]
# of course your results will vary
```

- `random.randrange(start, stop[, step])` - Random element in range

```python
import random
for i in range(3):
    print random.randrange(0, 101, 5)
```

## Risk Analysis in Testing

[Source: What is Risk Analysis in Software Testing and how to perform it?](https://www.edureka.co/blog/risk-analysis-in-software-testing/)

Identifying risks in application and prioritizing them to test

Examples:

1. Use of new hardware
2. Use of new technology
3. Use of new automation tool
4. The sequence of code
5. Availability of test resources for the application

### Avoidable Risks

1. The time that you allocated for testing
2. A defect leakage due to the complexity or size of the application
3. Urgency from the clients to deliver the project
4. Incomplete requirements

### Risk Avoidance

- Conduct Risk Assessment review meeting
- Use maximum resources to work on high-risk areas
- Create a Risk Assessment database for future use
- Identify and notice the risk magnitude indicators: high, medium, low.

### Risk Magnitude

- `High` means the effect of the risk would be very high and non-tolerable. The company might face loss.

- `Medium` it is tolerable but not desirable. The company may suffer financially but there is a limited risk.

- `Low` it is tolerable. There lies little or no external exposure or no financial loss.

### Risk Identification

- `Business Risks` Most common. Risk from company or your customer, not from your project.

- `Testing Risks` Should understand the platform and testinf needed.

- `Premature Release Risk` risk associated with releasing unsatisfactory or untested software is required

- `Software Risks` risks associated with the software development process.

### Risk Assessment

- `Effect` – To assess risk by Effect. In case you identify a condition, event or action and try to determine its impact.

- `Cause` – To assess risk by Cause is opposite of by Effect. Initialize scanning the problem and reach to the point that could be the most probable reason behind that.

- `Likelihood` – To assess risk by Likelihood is to say that there is a probability that a requirement won’t be satisfied.

### Risk Analysis

- Searching the risk
- Analyzing the impact of each individual risk
- Measures for the risk identified
