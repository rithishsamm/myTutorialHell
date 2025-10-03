# Dockerfile:
```
FROM python:3.6
MAINTAINER "sudhams reddy duba" dubareddy.383@gamil.com
RUN apt update && \
    apt install -y memcached
RUN pip install pymemcache
COPY get_data.py /root/
WORKDIR /root
CMD ["python3.6", "get_data.py"]
```

## get_data.py file code:
```
from pymemcache.client import base
# Don't forget to run `memcached' before running this next line:
client = base.Client(('localhost', 11211))
# Once the client is instantiated, you can access the cache:
client.set('docker', 'visualpath')
# Retrieve previously set data again:
print(client.get('docker'))
```