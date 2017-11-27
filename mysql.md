# Install mysql:

```bash
	sudo apt-get install mysql-server
```

# Test mysql:
## Login to mysql:

```bash
	mysql -u root -h localhost -p
```

## List all the available databases:

```bash
	show databases;
```

## Choose the database which you want to work with (for example the database name is *mysql* ):

```bash
	use mysql
```

## List all the tables which are included in the database:

```bash
	show tables
```

# References:

[1](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04) install LEMP stack ubuntu