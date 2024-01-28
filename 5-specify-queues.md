## declare queues using celery command
```bash
celery A- proj worker -Q q1,q2 -l info
```

## specify queue for each task or functions in celeryconfig.py :
```python

task_routes = {
    'proj.tasks.add' : {'queue':'q1'},
    'proj.tasks.devide' : {'queue':'q2'}
}

task_default_queue = 'q1'
``

> `proj` is name of the folder that contains celery instance and tasks.  

* you can specify the queue when you are calling tasks:
```python
devide.apply_async([[2,1]], queue='q1')
``` 
