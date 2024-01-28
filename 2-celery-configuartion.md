## celery configuration
you can set your configurations in the same module with your Celery instance, but the better way is to create a module named `celeryconfig.py` in the same directory of `tasks` and put your configurations there.

>  verify that your configuration file works properly and doesn’t contain any syntax error `python -m celeryconfig`  

* configuration.py
```python
broker_url = 'pyamqp://'
result_backend = 'rpc://'

task_serializer = 'json'
result_serializer = 'json'
accept_content = ['json']
timezone = 'Europe/Oslo'
enable_utc = True
```

* tasks.py
```python
...
app = Celery('tasks')
app.config_from_object('celeryconfig')
...
```
