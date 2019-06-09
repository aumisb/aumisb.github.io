---
layout: post
title: "Let's Encrypt"
date: 2019-06-09 21:00:00
tags: [namecheap, cloudflare, nginx, pfSense, let's encrypt]
---

This post covers how I get and renew Let's Encrypt certificates for my services. I use a Python virtual environment, in which I install the certbot and other required packages. Using certbot is straightforward for my case as it has a DNS plugin for my DNS provider (Cloudflare) and an installer for my server program (nginx).  

Since I have sever addresses of the form `x.domain.name`, I obtain a wildcard certificate for the domain, `*.domain.name`. This requires using Let's Encrypt's new ACMEv2 server and one of Certbot's DNS plugins (i.e., the Cloudflare DNS plugin).

---

1. Create the virtual environment, activate it, and install required packages. Note that the Python interpreter I use for this virtual environment is not the system one.

    ```bash
    mkdir -p /home/siam/projects/certbot
    cd /home/siam/projects/certbot
    /home/siam/opt/Python-3.7.2/python -m venv venv
    source venv/bin/activate
    pip3 install certbot certbot-dns-cloudflare certbot-nginx
    ```
2. Create a cloudflare.ini file with Cloudflare e-mail and global API. 

    ```
    dns_cloudflare_email = ""
    dns_cloudflare_api_key = ""
    ```

3. Renew

    ```bash
    sudo /home/siam/projects/certbot/venv/bin/certbot -a dns-cloudflare --dns-cloudflare-credentials cloudflare.ini -i nginx -d domain.name,*.domain.name --server https://acme-v02.api.letsencrypt.org/directory
    ```

4. Running this command will generate the correct SSL options for the certificates in `/etc/letsencrypt/options-ssl-nginx.conf` and DH params (not tied to the certificates) in `/etc/letsencrypt/ssl-dhparams.pem` and modify virtual hosts in `/etc/nginx/sites-enabled` as follows. 

    ```bash
    ssl_certificate /etc/letsencrypt/live/domain.name/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/domain.name/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    ```

    - Note that my subdomain virtual hosts did not properly update with the `include /etc/letsencrypt/options-ssl-nginx.conf;` and `ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;` lines. I do not know if they are necessary, but I created a `001-certbot.conf` file with these two lines in `/etc/nginx/conf.d` and added `include /etc/nginx/conf.d/001-certbot.conf` to every subdomain virtual host.

