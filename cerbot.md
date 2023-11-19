### Let's go through the process of generating an SSL certificate using

Certbot on Linux. I'll provide a general guide, and keep in mind that
specifics may vary depending on your Linux distribution and version.


### Step 1: Install Certbot

Make sure you have Certbot installed on your system. The installation
process can vary depending on your Linux distribution.

```bash
sudo apt update
sudo apt install certbot
```


### Step 2: Obtain an SSL Certificate

Run Certbot to obtain an SSL certificate. Replace `your_domain.com` with your actual domain:

`sudo certbot certonly --standalone -d your_domain.com`


### Step 3: Configure Nginx

Now that you have the certificate, you need to configure Nginx to use
it. Open your Nginx configuration file. This is often located at `/etc/nginx/sites-available/default` or `/etc/nginx/nginx.conf`. Add or update the server block for your domain:


```
server {
    listen 443 ssl http2;
    server_name your_domain.com;

    ssl_certificate /etc/letsencrypt/live/your_domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your_domain.com/privkey.pem;

    # Other SSL configurations...

    location / {
        # Proxy pass to your Django application
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        # Other proxy configurations...
    }
}
```


### Step 4: Reload Nginx

After making changes to the Nginx configuration, reload Nginx to apply the new settings:

<pre><div class="bg-black rounded-md"><div class="flex items-center relative text-gray-200 bg-gray-800 gizmo:dark:bg-token-surface-primary px-4 py-2 text-xs font-sans justify-between rounded-t-md"><span>bash</span></div></div></pre>

<pre><div class="bg-black rounded-md"><div class="p-4 overflow-y-auto"><code style="color:white; "class="!whitespace-pre hljs language-bash">sudo systemctl reload nginx
</code></div></div></pre>

### Step 5: Set Up a Renewal Process

Let's Encrypt certificates are valid for 90 days. It's recommended to set up a renewal process to automatically renew your certificates. You can do this by adding a cron job. Open your crontab configuration:

<pre><div class="bg-black rounded-md"><div class="flex items-center relative text-gray-200 bg-gray-800 gizmo:dark:bg-token-surface-primary px-4 py-2 text-xs font-sans justify-between rounded-t-md"><span>bash</span></div></div></pre>

<pre><div class="bg-black rounded-md"><div class="p-4 overflow-y-auto"><code style="color:white; "class="!whitespace-pre hljs language-bash">sudo crontab -e
</code></div></div></pre>

Add the following line to run the renewal process twice a day:

<pre><div class="bg-black rounded-md"><div class="flex items-center relative text-gray-200 bg-gray-800 gizmo:dark:bg-token-surface-primary px-4 py-2 text-xs font-sans justify-between rounded-t-md"><span>bash</span></div></div></pre>

<pre><div class="bg-black rounded-md"><div class="p-4 overflow-y-auto"><code style="color:white; " class="!whitespace-pre hljs language-bash">0 */12 * * * /usr/bin/certbot renew --quiet
</code></div></div></pre>

This cron job checks for the expiration of certificates and renews them if necessary.

That's it! Your Django application should now be accessible over HTTPS using the SSL certificate obtained from Let's Encrypt. Ensure that your firewall allows traffic on port 443 (HTTPS) if you haven't done so already.
