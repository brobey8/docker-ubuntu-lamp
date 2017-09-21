# docker-ubuntu-lamp
A Dockerfile that generates a php-7 ubuntu app

Use it like this:

`docker run -itd -e "VIRTUAL_PORT=80" -e "VIRTUAL_HOST=mycontainer.docker" --name mycontainer -v /Your/path/to/local/folder:/var/www/html ratbag/docker-ubuntu-lamp`

Note: I'm using jwilder/nginx-proxy to expose my containers using the environment variables.

The only thing you should need to do is add your vhosts in `/etc/apache2/sites-available` e.g:

```
<VirtualHost *:80>
  ServerName mycontainer.docker
  DocumentRoot "/var/www/html/public"
  <Directory "/var/www/html/public">
    AllowOverride all
  </Directory>
</VirtualHost>
```

And symlink it:

```cd /etc/apache2/sites-enabled
ln -s /etc/apache2/sites-available/mycontainer.conf
```

Setting up the DB:

Login to mysql - `mysql` and create your database.

Once you've done that add another user `CREATE USER 'myapp'@'localhost' IDENTIFIED BY 'password';`
Then give that user access `GRANT ALL PRIVILEGES ON myapp.* TO 'myapp'@'%' WITH GRANT OPTION;`

Restart the container / apache2 and you're good to go.
