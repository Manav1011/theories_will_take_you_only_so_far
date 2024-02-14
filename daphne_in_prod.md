# Using Daphne as a Production Server in Django

Daphne is an ASGI server designed to serve Django applications and support WebSockets. Follow the steps below to set up Daphne as your production server for a Django project.

## 1. Install Daphne

```bash
pip install daphne
```

## 2. Configure ASGI in Django

In your Django project, create an `asgi.py` file in the project folder (alongside `settings.py`):

```python
import os
from django.core.asgi import get_asgi_application
from channels.routing import ProtocolTypeRouter,URLRouter
from channels.security.websocket import AllowedHostsOriginValidator

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'SMARTROLL.settings')

django_asgi_app = get_asgi_application()

from Session.routing import session_urlpatterns

application = ProtocolTypeRouter({
    "http": django_asgi_app,
    "websocket": URLRouter((session_urlpatterns)) 
})
```

Make sure to replace `'your_project.settings'` with the actual path to your Django project's settings.

## 3. Update Django Settings

In your `settings.py` file, make sure you have the following settings configured:

```python
# settings.py
ASGI_APPLICATION = "your_project.asgi.application"
```


# 5. Nginx Configuration

Configure Nginx to proxy requests to Daphne: 

HTTP

```nginx
server {
    listen 80;
    server_name your_domain.com www.your_domain.com;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /path/to/your/project;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/path/to/your/project/run/daphne.sock;
    }

    location /ws/ {
        include proxy_params;
        proxy_pass http://unix:/path/to/your/project/run/daphne.sock;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

Replace placeholders with your actual paths and domain.

## 6. Restart Services

Restart Nginx and Daphne:

```bash
sudo service nginx restart
sudo supervisorctl restart your_project
```

Now, Daphne is serving your Django project in a production environment. Ensure proper security configurations, including SSL, for a robust deployment.


# **It can also work with https**

```
daphne -e ssl:8001:privateKey=/home/manav1011/Documents/SMARTROLL_SSIP_2023/SMARTROLL/localhost.key:certKey=/home/manav1011/Documents/SMARTROLL_SSIP_2023/SMARTROLL/localhost.crt SMARTROLL.asgi:application
```

```
server {
    listen 80;
    server_name smartroll.ssip.in;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name smartroll.ssip.in;

    ssl_certificate /home/manav1011/Documents/SMARTROLL_SSIP_2023/SMARTROLL/localhost.crt;
    ssl_certificate_key /home/manav1011/Documents/SMARTROLL_SSIP_2023/SMARTROLL/localhost.key;

    location / {
        proxy_pass https://127.0.0.10:8001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /ws/ {
        proxy_pass https://127.0.0.10:8001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /static/ {
        alias /home/manav1011/Documents/SMARTROLL_SSIP_2023/SMARTROLL/staticfiles/;
    }
}

```
