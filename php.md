# Install *php-fpm* :

```bash
	sudo apt-get install php-fpm php-mysql
```

## Configure the PHP Processor:

Open the main php-fpm configuration file with root privileges:

```bash
sudo vi /etc/php/7.0/fpm/php.ini
```

What we are looking for in this file is the parameter that sets cgi.fix_pathinfo. This will be commented out with a semi-colon (;) and set to "1" by default.
Uncommenting the line and setting it to "0" like this:

```bash
cgi.fix_pathinfo=0
```

Restart php processor

```bash
sudo systemctl restart php7.0-fpm
```

# Configure Nginx to Use the PHP Processor:

Now, we have all of the required components installed. The only configuration change we still need is to tell Nginx to use our PHP processor for dynamic content.

We do this on the server block level (server blocks are similar to Apache's virtual hosts). Open the default Nginx server block configuration file by typing:

```bash
sudo nano /etc/nginx/sites-available/default
```

Currently, with the comments removed, the Nginx default server block file looks like this:

```bash
/etc/nginx/sites-available/default
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

We need to make some changes to this file for our site.

First, we need to add *index.php* as the first value of our index directive so that files named index.php are served, if available, when a directory is requested.
We can modify the server_name directive to point to our server's domain name or public IP address.
For the actual PHP processing, we just need to uncomment a segment of the file that handles PHP requests by removing the pound symbols (#) from in front of each line. This will be the location *~\.php$* location block, the included *fastcgi-php.conf* snippet, and the socket associated with *php-fpm*.
We will also uncomment the location block dealing with *.htaccess* files using the same method. Nginx doesn't process these files. If any of these files happen to find their way into the document root, they should not be served to visitors.
The changes that you need to make are in red in the text below:

```
/etc/nginx/sites-available/default
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name server_domain_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

When you've made the above changes, you can save and close the file.

Test your configuration file for syntax errors by typing:

```bash
sudo nginx -t
```

If any errors are reported, go back and recheck your file before continuing.

When you are ready, reload Nginx to make the necessary changes:

```bash
sudo systemctl reload nginx
```


# Testing 

add a file named **index.php** to **/var/www/html/**

with the content below:

```php
<html>
	<head>
		<title>PHP Test</title>
	</head>
	<body>
		<?php echo '<p>Hello World</p>'; ?> 
	</body>
</html>
```

Use your browser to access the url http://localhost the page will show nothing but the **"helloworld"** string

# References:

[1](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04) install LEMP stack ubuntu