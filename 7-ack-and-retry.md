## acknowledge in celery
by defaut when a task moves from queue to a worker and starts to run, the worker removes the task from queue. so if task fails worker won't process it again.


## acks early VS acks late
Tasks are only removed from a queue when they are acknowledged ("acked") by the worker that received them. The `acks_late` setting determines when a worker will ack a task. When set to true, tasks are acked after the worker finishes executing them. When set to false, they are acked right before the worker starts executing them.

The default of `acks_late` is false, however if your tasks are idempotent it's strongly recommended that you set `acks_late` to true. This has two major benefits.

## idempotent
Idempotence is a mathematical property that describes a function that can be called multiple times without changing the result.. Practically it means that a function can be repeated many times without unintended effects, but not necessarily side-effect free in the pure sense



## retry
In Celery, "retry" refers to the ability to automatically attempt to execute a failed task again. When a task encounters an exception or fails for some reason, Celery can be configured to retry the task after a certain period, giving it another chance to succeed.remember this option is used when you know you will have a exception error that if you retry it, the exception error will be gone.
To use retry in Celery tasks, you can take advantage of the retry decorator or the retry task option.


* retry decorator
```python
@app.task(bind=True, max_retries=3)
def my_task(self, *args, **kwargs):
    try:
        # Task logic here
        result = do_something(*args, **kwargs)
    except Exception as exc:
        # Log the exception or handle it as needed
        print(f"Task failed: {exc}")
        # Retry the task after 5 seconds
        raise self.retry(countdown=5)
```
In this example, the `max_retries` parameter specifies the maximum number of retry attempts, and `countdown` is the time delay before the next retry.

* retry task option
```python
@app.task
def my_task(*args, **kwargs):
    try:
        # Task logic here
        result = do_something(*args, **kwargs)
    except Exception as exc:
        # Log the exception or handle it as needed
        print(f"Task failed: {exc}")
        # Retry the task after 5 seconds
        raise my_task.retry(countdown=5, max_retries=3)
```

> as an option you can use `retry_backoff=true` to reduce presure on your network  

