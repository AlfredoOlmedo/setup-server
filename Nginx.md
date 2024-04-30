# Install and configure Nginx

1. Instale Nginx
```
sudo apt update
sudo apt install nginx
```
2. Crear un sitio web de introducción
Ubicación
```
/var/www/html
```

Create a simple html page 
```
/var/www//webpage or folder touch index.html | mkdir folder > touch index.html

cd /var/www
sudo mkdir tutorial
cd tutorial
sudo "${EDITOR:-vim}" index.html
```
3. En index.html crea un contenido predeterminado.
```
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello, Nginx!</title>
</head>
<body>
    <h1>Hello, Nginx!</h1>
    <p>We have just configured our Nginx web server on Ubuntu Server!</p>
</body>
</html>
```

4.Configurar un servidor virtual
Ubicación
```
/etc/nginx/sites-enabled
```

5. Por razones de seguridad cambie el puerto, el puerto predeterminado es el 80 así que cámbielo a 81
```
/etc/nginmx/sitex-enabled
sudo "${EDITOR:-vim}" index.html
server {
	listen 81;
	listen [::]:81;

	server_name example.ubuntu.com;

	root /var/www/tutorial;
	index index.html;

	location / {
		try_files $uri $uri/ =404;
	}
}
```

6. Activación del host virtual y resultados de las pruebas
```
sudo service nginx restart
```

Referencia:  
https://ubuntu.com/tutorials/install-and-configure-nginx#1-overview

### Cómo instalar Nginx en Debian 10

Referencia:

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-10

