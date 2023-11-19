# Memcache

`sudo apt-get install memcached`

`pip install pymemcache`


In settings.py

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.PyMemcacheCache',
        'LOCATION': '127.0.0.1:11211',  # Adjust this to match your Memcached server's address and port
    }
}
```
