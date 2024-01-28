## the primitives :
primitives are signature objects. so they can be combined in many number of ways.

### groups 
A group calls a list of tasks in parallel
```python
from celery import group
from proj.tasks import add

group(add.s(i, i) for i in range(10))().get()
# result of above code : [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

* partial group
```python
g = group(add.s(i) for i in range(10))
g(10).get()
# result of above codes : [10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
``` 

### chains
it is a method that gets multiple tasks and each task gives its result to the next tast in chain.
```python
from celery import chain
from proj.tasks import add, mul

chain(add.s(4, 4) | mul.s(8))().get()
# result : 64
```

### chords
chord is a group chained to another task
```python

x = chord((add.s(i, i) for i in range(10)), xsum.s())
print(x().get)


```
