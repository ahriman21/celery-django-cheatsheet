# how to use celery in django

1- install a broker on your server  
> such as redis and rabitmq

2- install celery and appropreate broker in your django project (your virtual environment) 
> pip install celery  
> pip install redis

3- add your celery config to your settings:
```
CELERY_BROKER_URL = ''
CELERY_RESULT_BACKEND = CELERY_BROKER_URL
CELERY_ACCEPT_CONTENT = ['application/json']
CELERY_RESULT_SERIALIZER = 'json'
CELERY_TASK_SERIALIZER = 'json'
CELERY_TIMEZONE = TIME_ZONE
CELERY_TASK_DEFAULT_QUEUE = 'default'
```

4- create a celery.py module in your core application of your project

5- implement celery in your created module:
```
from celery import Celery
import os

# create an instance of Celery and set the name to name of your module.
app = Celery('core') 

# import celery configs here:
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "django_celery.settings")
app.config_from_object('django.conf:settings', namespace='CELERY')

```

6- to use celery in your functions you should import the created instance from your celery.py module and use it as decorator on top of your function like that . it is better to create a task.py module in each app that you want to use celery and in task.py you implement your functions
then import the instance that we talked about there.:
```
>> blog/task.py/

from core.celery import app

@app.task
def send_email(msg,to):
  ...

```

7- after writing your function , import it and call the function like this :
```
>> blog/views.py/

# use .delay() after calling your function:
from .tasks import send_email

...
send_email.delay(msg,user_email)
...

```

8- start your workers in terminal:
```
# celery -A [name of the app you create celery.py] worker -l info
> celery -A core workder -l info
```
