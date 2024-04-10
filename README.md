# Setting Up a Server with NGINX and UFW on Arch Linux

*This guide will walk you through the process of setting up a server with NGINX and UFW on Arch Linux.*

## Step 1: Setting up necessary file/folders:

*First, you'll need to log into your server. You can do this with the following command, replacing`your_ssh_name`, `user_name`, and `droplet_ip_address` with your specific details:*

```bash
    ssh -i .ssh/your_ssh_name user_name@droplet_ip_address
```

## 2. Install NGINX
*Nginx is the application that will deploy the server for your project. You can install it using the following command:* 

```bash
    sudo pacman -S nginx
```
## 3. Install Vim 
*Vim is your code editor for this tutorial.*

```bash
    sudo pacman -S vim
```

## 4. Create Required Directory in the Home Folder

```bash
    cd ~
    sudo mkdir -p /web/html/nginx-2420
```

### The cd ~ command navigates your current working directory to the user specific home directory. The sudo mkdir -p command creates the directory that you need and all the subfolders in one go in the root folder, which is also why you need sudo since normal users don’t have permissions to make changes to the root folder.*

# Step 2 - Configuring NGINX Files

To properly configure the web server, follow these steps:

 ## 1. Create a Configuration File in the Required Folder
*Navigate to the required folder where the configuration file needs to be created:*
```bash
    cd /etc/nginx/
```
## 2.
*Create a folder named `sites-available`:*

```bash
    sudo mkdir -p sites-available
```

## 3.
*Create a folder named `sites-enabled`:*

```bash
    sudo mkdir -p sites-enabled
```

## 4.
*Create and open the new configuration file in the `sites-available` directory. This file is necessary for the server to run. Opening the file with root permission is required since a regular user typically doesn't have access to modify it*

```bash
    sudo vim sites-available/nginx-2420.conf
```

## 5. 
*After creating the necessary folder structure, it's time to insert the configuration details into the newly created file. Follow these steps:*
```bash
    sudo vim sites-available/nginx-2420.conf
```

## 6.
*Paste the provide configuration code into the opened file. This configuration specifies the settings that nginx will utilize. <NOTE> Ensure to replace placeholders such as "your_droplet_ip" with the actual name and IP address of your DigitalOcean droplet:*

```bash
    server {
        listen 80;
        server_name your_droplet_ip;

        root /web/html/nginx-2420;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
 ```
 
### Save and close the file. To do this, press `Esc` to exit insert mode, then type `:wq` (without the quotes) followed by pressing `Enter`. This command saves the changes and exits the editor.*

## Your configuration file is now updated and saved with the necessary details

## Step 3. Activate NGINX

To apply the changes made to the NGINX configuration and restart the server, follow these steps:

```bash
    sudo systemctl restart nginx
```

*Verify if the NGINX server has restarted properly by executing:*

```bash
    sudo systemctl status nginx
```

*For a more detailed log, you can use the following command:*

```bash
    journalctl -u nginx
```

*After confirming that the service has restarted successfully, navigate back to your home directory:*

```bash
    cd ~
```

## Step 4 Creating and Opening HTML File

To create and open the HTML file for your web server, follow these steps:

## 1.
*Use the following command to create and open the file in the specified directory:*

```bash
    sudo vim /web/html/nginx-2420/index.html
```

## 2.
*Copy and paste the provided HTML code below into the opened file. This code will determine the appearance of your web server's display:*

```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>2420</title>
        <style>
          * {
            color: #db4b4b;
            background: #16161e;
          }
          body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
          }
          h1 {
            text-align: center;
            font-family: sans-serif;
          }
        </style>
      </head>
      <body>
        <h1>All your base are belong to us</h1>
      </body>
    </html>
```

*Make any necessary changes to the HTML code to customize the display according to your preferences.*

*Save and close the file by pressing `Esc`, then typing `:wq` (without the quotes) followed by pressing `Enter`. This command saves the changes and exits the editor.*

### Your HTML file is now created and saved

# Step 5. Creating Symbolic Link and Checking for Syntax Errors

To enable the server block and ensure the NGINX configuration is error-free, follow these steps:

## 1.
*Create a symbolic link to enable the server block by executing the following command:*

```bash
    sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/nginx-2420
```

*The symbolic link must be located in the `sites-enabled` directory within the NGINX configuration directory (`/etc/nginx/sites-enabled`).*

## 2. 
*Next, check for any syntax errors in the NGINX configuration file by running:*

```bash
    sudo nginx -t
```

*If the output indicates success for the configuration file and syntax, you are good to proceed.*

## 3.
*Reload NGINX to apply the changes:*

```bash
    sudo systemctl reload nginx
```
## These steps ensure that the server block is enabled and the NGINX configuration is error-free. You can now proceed with the next steps

# Step 6. Opening the Original NGINX Configuration File

To open the original NGINX configuration file and add a code snippet to it, follow these steps:

## 1.
*Execute the following command to open the NGINX configuration file:*

```bash
    sudo vim /etc/nginx/nginx.conf
```

*Once the file is opened, navigate to the `https` function block.*

## 2.
*Copy and paste the following code snippet into the `https` function block:*

```bash
    include /etc/nginx/sites-enabled/*;
```

*This code snippet includes all configuration files from the `sites-enabled` directory, enabling the server blocks defined in those files.*

## 3.
*Save and close the file by pressing `Esc`, then typing `:wq` (without the quotes) followed by pressing `Enter`. This command saves the changes and exits the editor.*

## The original NGINX configuration file is now updated with the necessary code snippet.

# Step 7 Accessing Your Droplet's IP Address in the Browser
Once you have completed the setup and configuration of your web server, you can access it by entering your droplet's IP address into your web browser's address bar.

1. Open your web browser of choice. 
2. Enter the IP address of your droplet in the address bar
3. Press Enter to navigate to the specified address.

You should now see the expected display in your browser:
## EXAMPLE:
![alt text](image.png)

*If everything is configured correctly, you should be able to view the content served by your NGINX web server. Enjoy your very own web server!*
