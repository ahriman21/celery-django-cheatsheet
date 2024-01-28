## abstract tasks
you can create a class and inherit it from Task so you can add some additional functionalities or set some defualt settings

* decide what happens when you call a task :
```python

from celery import Celery
from celery import Task


app = Celery('app')

class MyTask(Task):
    def __call__(self, *args, **kwargs):
        print(f'task {self.request.id} is running')
        # you should not use `super` keyword in code below
        return self.run(*args, *kwargs)
``` 

* set a default queue for tasks
```python

from celery import Celery
from celery import Task


app = Celery('app')

class MyTask(Task):
    queue = 'some_queue_name'

```

## how to use the custom class
* you can use the following code to apply the changes on your tasks
```python
app.Task = MyTask
```
* or you can use it particulary for each task
```python

@app.task(base=MyTask)
def add(x,y):
    ...
```


