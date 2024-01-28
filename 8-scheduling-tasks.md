## configuring timezone
first you need to configure timezone and enable utc
```python
enable_utc = True
timezone = 'Asia/Tehran'
```

> remember the order of setting timezones. FIRST--> enable utc   SECOND-->set your timezone
  

## set a scheduled task
for example we want to write a code to run `send_email` task 10 seconds later.
to schedule a task you need to use apply_async, and pass your parameters to it, then set `eta` parameter to determine the datetime.
 
```python
from pytz import timezone
from .tasks import send_email
from celery.utils.time import datetime, timedelta


tzone = timezone('Asia/Tehran')
send_email.apply_async(email, eta=datetime.now(tzone)+timedelta(seconds=10))
```

> rememeber to use `datetime.utcnow()` or `datetime.now(timezone('your time zone'))`  









