## celery 
celery project is a tool that helps us to handle long processing tasks. to work with celery we should choose a broker.

## choosing a broker
first you have to choose a broker and start it. you can installing it by commandbelow in linux
```bash
sudo apt install rabbitmq-server
```
althogh after you install it on your system rabbitmq is goint to start, there are some commands here to work with rabbitmq 

* see status of your  rabbitmq server
```bash
systemctl status rabbitmq-server

```
* stop rabbitmq
```bash
systemctl stop rabbitmq-server
```

* start rabbitmq
```bash
systemctl start rabbitmq-server
```

## install celery 
Celery is on the Python Package Index (PyPI), so it can be installed with standard Python tools like pip:
```bash
pip install celery

```

## first step
Firstly you need to import the Celery from celery module and pass the name of current module and broker url.
For RabbitMQ you can use `amqp://localhost`, or for Redis you can use `redis://localhost`.
Another parameter for celery instance is `backend`. to store results of your tasks you must fill this parameter. you can use redis or rabbitmq as your backend as well.
example : `backend='rpc://'` or `backend='redis://localhost'`. when you are using backend, you must call get() or forget() after calling your tasks, if you don't call them you may face some problems with resources.

* let's create a file in tasks.py:
```python
from celery import Celery


app = Celery('tasks', broker='pyamqp://guest@localhost//', backend='rpc://')

# create your tasks
@app.task
def add(x,y):
    return x+y
```

* call your task in main.py
```python
result = edd.delay(4, 4)
print(result.get())
```

## run the worker
```bash
celery -A tasks worker --loglevel=INFO
```
In production you’ll want to run the worker in the background as a daemon. To do this you need to use the tools provided by your platform, or something like supervisord (see Daemonization for more information).

  
