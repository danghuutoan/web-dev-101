# Install nginx

```bash
	sudo apt update
	sudo apt upgrade
	sudo apt install nginx
```


```bash
	sudo systemctl enable nginx
	sudo systemctl start nginx
```

Check nginx status

```bash
	sudo systemctl status nginx
```

Check which process is using the port

OS X
```bash
	sudo lsof -i tcp:3000
```
ubuntu

```bash
	netstat -vanp tcp | grep 3000
```

# References:

[1](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04) install LEMP stack ubuntu
