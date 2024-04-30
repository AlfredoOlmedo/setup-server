# Setup Ubuntu Server

Después de instalar Ubuntu Server, próximos pasos para configurarlo

1. Actualizar y mejorar
Comience actualizando la lista de paquetes y actualizando los paquetes instalados a sus últimas versiones:
```
sudo apt update && sudo apt upgrade
```

2. SSH seguro
Configure SSH cambiando el puerto SSH predeterminado, deshabilite el inicio de sesión de root y use autenticación basada en claves en lugar de contraseñas.

3. Configuración del cortafuegos
Ubuntu Server viene con una herramienta de configuración de Firewall llamada "UFW - Uncomplicated Firewall". Configúrelo para permitir las conexiones entrantes y salientes necesarias mientras bloquea otras.
```
sudo ufw <port>/<protocol>
sudo ufw enable
```
4. Instale el software necesario
Dependiendo del propósito de su servidor, instale los paquetes de software requeridos. Por ejemplo, si es un servidor web, instale Apache o NginX, MySQL o MariaDB y su pila favorita (MERN), Mongo, Express React y Node, por ejemplo.

5. Configurar servicios
Configura los servicios según tus necesidades,

6. Configurar el nombre de dominio
Si su servidor alojará sitios web, configure su nombre de dominio y configure la configuración DNS para que apunte a la dirección IP de su servidor.

7. Actualizaciones de seguridad
Verifique periódicamente las actualizaciones de seguridad y aplíquelas usando apt
```
sudo apt update && sudo apt upgrade
```

8. Copias de seguridad
Configure una estrategia de respaldo para asegurarse de poder recuperar datos en caso de falla. Utilice herramientas como rsync tar o soluciones cloud baes

9. Monitoreo y registro
Instale herramientas de monitoreo como netdata o nagios para realizar un seguimiento del rendimiento del servidor y configurar la rotación de registros para administrar los archivos de registro de manera eficiente.

10. Gestión de usuarios
Cree cuentas de usuario con los permisos adecuados para administrar su servidor. evite usar la cuenta raíz para tareas regulares

11. Prueba
Después de configurar todo, pruebe minuciosamente su servidor para asegurarse de que todo funcione como se esperaba.
Recuerde mantener y actualizar periódicamente su servidor para mantenerlo seguro y funcionando sin problemas.

Referencia:
```
Uncomplicated Firewall UFW:
https://help.ubuntu.com/community/UFW

Nagios:
https://www.nagios.org/

Netdata:
https://www.netdata.cloud/

MariaDB:
https://mariadb.org/

MySQL:
https://www.mysql.com/

MongoDB:
https://www.mongodb.com/

Node:
https://nodejs.org/en

Apache Server:
https://httpd.apache.org/

Python:
https://www.python.org/

ReactJS:
https://react.dev/

Docker:
https://www.docker.com/

Kubernetes:
https://kubernetes.io/
```