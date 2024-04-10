# Tutorial: Setting up a Firewall (UFW) and reverse Proxy with NGINX

This tutorial will guide you through the steps involved for adding a firewall using ufw (Uncomplicated Firewall) and configuring a reverse proxy server with NGINX.

## Prerequisites

Before you start, you should have the following:

- Assignment 3 part 1 setup complete
- backend binary is place on the server, ie `hello-server` in this case
- Service file is created and configured in order for the `backend.service` to be able to run as a service
- NGINX installed on your server

## Step 1: Firewall (UFW) Setup
First, install UFW on the server. This can be done by running the command below:

1. *UFW Installation:*
``` bash
    sudo pacman -Syu
    sudo pacman -S ufw
```
2. *Enable UFW and permit SSH*
```bash
    sudo ufw allow SSH
    sudo ufw enable
```

3. *Allow access to the specified ports (80 for HTTP!):*
Permit the http connection for the web server
 ```bash
    sudo ufw allow 80/tcp
 ```

Turn the firewall on:

``` bash
sudo ufw enable
```

4. *Verify the firewall settings:*
Check the status with:

```bash 
    sudo ufw status verbose
```

## Step 2: Configuring UFW
1. *Modify NGINX Configuration* 

Modify the NGINX server block configured in the previous assignment

```bash
    sudo vim /etc/nginx/sites-enabled/2420-nginx.conf
```

Edit the file, add the changes to the server block as shown below. 

```nginx
   server {
       listen 80;
       server_name your_server_ip;

       location / {
           proxy_pass http://127.0.0.1:8080;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
```
2. *Test the configurations*

Ensure that the configuration is properly syntxed with:

```bash
    sudo nginx -t
```

3.


