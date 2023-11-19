If you don't have a domain and you just want to configure HTTPS for your local Django application, you can use a self-signed SSL certificate. Here are the steps:

### Step 1: Generate a Self-Signed SSL Certificate

You can use OpenSSL to generate a self-signed SSL certificate. Open a terminal and run the following commands:

```bash
# Create a private key
openssl genpkey -algorithm RSA -out localhost.key

# Create a Certificate Signing Request (CSR)
openssl req -new -key localhost.key -out localhost.csr

# Create a self-signed certificate
openssl x509 -req -days 365 -in localhost.csr -signkey localhost.key -out localhost.crt
```

### Step 2: Configure Django to Use HTTPS

Update your Django development server to use the generated SSL certificate. In your Django project settings, modify the `runserver` command:

```python
# settings.py

import os

# Define the paths to the SSL certificate and key
SSL_CERTIFICATE_PATH = os.path.join(BASE_DIR, "localhost.crt")
SSL_KEY_PATH = os.path.join(BASE_DIR, "localhost.key")

# Enable HTTPS in development
if DEBUG:
    INSTALLED_APPS += ['sslserver']
    # Use the 'runsslserver' command instead of 'runserver'
    INSTALLED_APPS += ['sslserver']

    # Specify the paths to the SSL certificate and key
    SSL_CERTIFICATE = SSL_CERTIFICATE_PATH
    SSL_KEY = SSL_KEY_PATH
```

### Step 3: Run the Development Server with HTTPS

Now, you can run the development server with HTTPS:

```bash
python manage.py runsslserver
```

Visit `https://localhost:8000` in your web browser, and you should see your Django application served over HTTPS.

Keep in mind that self-signed certificates are not trusted by default in browsers, and you may encounter security warnings. These certificates are suitable for development purposes but not for production. In a production environment, you should obtain a valid SSL/TLS certificate from a trusted Certificate Authority (CA).

# Add a domain to IP mapping in hosts file


# Update the nginx server block

```bash
server {
    listen 443 ssl http2;
    server_name djangoapp.live;

    ssl_certificate /home/manav1011/Documents/dynamic_led_display_prod/dynamic_led_display_prod/localhost.crt;
    ssl_certificate_key /home/manav1011/Documents/dynamic_led_display_prod/dynamic_led_display_prod/localhost.key;
  

    location / {
        proxy_pass https://192.168.29.18:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /ws/ {
        proxy_pass https://192.168.29.18:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}

```

Check for errors and reload nginx

```
sudo nginx -t
sudo systemctl restart nginx
```
