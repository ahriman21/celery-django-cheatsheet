# Using celery in application
### Project layout:
```
src/
    proj/__init__.py
        /mycelery.py
        /tasks.py
```


* proj/mycelery.py
```python

from celery import Celery

app = Celery('proj',
             broker='amqp://',
             backend='rpc://',
             include=['proj.tasks'])

# Optional configuration, see the application user guide.
...

if __name__ == '__main__':
    app.start()
```


* proj/tasks.py
```python

from .mycelery import app


@app.task
def add(x, y):
    return x + y


@app.task
def mul(x, y):
    return x * y


@app.task
def xsum(numbers):
    return sum(numbers)
```


### calling tasks
* delay() method:
```python
from proj.tasks import add

add.delay(2, 2)
```
* apply_async() method:
In the below  example the task will be sent to a queue named lopri and the task will execute, at the earliest, 10 seconds after the message was sent. so in this method you can select the queue and specify a countdown to run your task.
```python

add.apply_async((2, 2), queue='lopri', countdown=10)

```
* signature() or s() method:
this method helps us to use primitives for tasks
```python
add.signature(2,2)
```


### starting the worker
* start a single worker
```bash
celery -A proj worker -l INFO

```
* start multi workers (proper for production)
```bash

mkdir -p /var/run/celery
mkdir -p /var/log/celery
celery multi start w1 -A proj -l INFO --pidfile=/var/run/celery/%n.pid \
                                        --logfile=/var/log/celery/%n%I.log
```
> `proj` is name of the folder that includes celery instace and tasks.  


