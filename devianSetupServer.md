# Configuración inicial del servidor con Debian

Cuando crea por primera vez un nuevo servidor Debian, hay algunos pasos de configuración que debe seguir desde el principio como parte de la configuración básica. Esto aumentará la seguridad y usabilidad de su servidor y le dará una base sólida para acciones posteriores.

#### 1. Iniciar sesión como root 
Para iniciar sesión en su servidor, necesitará conocer la dirección IP pública de su servidor. También necesitará la contraseña o, si instaló una clave SSH para la autenticación, la clave privada de la cuenta del usuario raíz.
Si no está conectado a su servidor, continúe e inicie sesión como usuario root usando el siguiente comando (sustituya la parte resaltada del comando con la dirección pública de su servidor).

```
ssh root@ your_server_ip
```

Acepte la advertencia sobre la autenticidad del host si apareció. Si está utilizando autenticación de contraseña, proporcione su contraseña de root para iniciar sesión. Si está utilizando una clave ssh que está protegida con una frase de contraseña, es posible que se le solicite que ingrese la frase de contraseña la primera vez que use la clave en cada sesión, si esta es la primera vez. Al iniciar sesión en el servidor con una contraseña, es posible que también se le solicite que cambie la contraseña de root.

####  Acerca de Root (sudo su / root)

El usuario root es el usuario administrativo en un entorno Linux que tiene privilegios muy generales. Debido a los mayores privilegios de la cuenta raíz, no se le recomienda usarla de forma regular, esto se debe a que parte del poder inherente a la cuenta raíz es la capacidad de realizar cambios muy destructivos, incluso por accidente.

#### 2. Crear un nuevo usuario
Una vez que haya iniciado sesión como root, estamos preparados para agregar la nueva cuenta de usuario que usaremos para iniciar sesión de ahora en adelante.
Crear un nombre de usuario
```
adduser [username]
```

Nota:   
se le harán algunas preguntas, comenzando con la contraseña de la cuenta. Ingrese una contraseña segura y, si es opcional, agregue más información, luego presione ENTRAR

#### 3 Otorgamiento de privilegios administrativos

Ahora hemos creado una nueva cuenta de usuario con privilegios de cuenta normales. Sin embargo, es posible que en ocasiones necesitemos realizar tareas administrativas con él.
Para evitar tener que cerrar sesión en nuestro usuario normal y volver a iniciar sesión como cuenta root, podemos configurar lo que se conoce como privilegios de superusuario o root para nuestra cuenta normal. Esto permitirá a nuestro usuario normal ejecutar comandos con privilegios administrativos poniendo la palabra sudo antes del comando.
Para agregar estos privilegios a nuestro nuevo usuario, necesitamos agregar el nuevo usuario al grupo sudo. De forma predeterminada, en Debian 11, los usuarios que pertenecen al grupo sudo pueden utilizar el comando sudo.
Como root, ejecute este comando para agregar su nuevo usuario al grupo sudo (sustituya la palabra resaltada con su nuevo usuario):
```
usermod -aG sudo [nombre de usuario]
```

ahora, cuando inicie sesión como usuario habitual, puede escribir sudo antes de los comandos para ejecutar el comando con privilegios de superusuario.

#### 4 Configuración de un firewall básico 
Los servidores Debian pueden usar firewalls para asegurarse de que solo se permitan ciertas conexiones a servicios específicos. En esta guía, instalaremos y usaremos el firewall UFW para ayudar a establecer políticas de firewall y administrar excepciones.
Podemos utilizar el administrador de paquetes apt para instalar UFW. Actualice el índice local para recuperar la información más reciente sobre los paquetes disponibles y luego instale el software de firewall UFW escribiendo:
```
apt update
apt install ufw
```
Nota:   
Recomendamos utilizar solo un firewall a la vez para evitar reglas conflictivas que puedan ser difíciles de depurar.
Los perfiles de firewall permiten a UFW administrar conjuntos de reglas de firewall con nombre para las aplicaciones instaladas. Los perfiles de algunos software comunes se incluyen con UFW de forma predeterminada y los paquetes pueden registrar perfiles adicionales con UFW durante el proceso de instalación. OpenSSH, el servicio que ahora nos permite conectarnos a nuestro servidor, tiene un perfil de firewall que podemos usar.
Enumera todos los perfiles de aplicaciones disponibles escribiendo:

```
ufw app list

output
Available applications:
...
openSSH
...
```

Necesitamos asegurarnos de que el firewall permita conexiones SSH para que podamos volver a iniciar sesión la próxima vez. Podemos permitir estas conexiones escribiendo:

```
ufw allow OpenSSH 
```
Después, podemos habilitar el firewall escribiendo:
ufw enable 
Escriba y y presione ENTER para continuar. Puedes ver que las conexiones SSH todavía están permitidas escribiendo:
ufw status

```
Output
Status: active

To             Action   From
--             ------   ----
OpenSSH          ALLOW    Anywhere
OpenSSH (v6)     ALLOW    Anywhere (v6)
```

Como el firewall actualmente bloquea todas las conexiones excepto SSH, si instala y configura servicios adicionales, deberá ajustar la configuración del firewall para permitir la entrada de tráfico aceptable.

Referencia a: ufw Firewall

#### 5 Habilitación del acceso externo para su usuario habitual
Ahora que tenemos un usuario habitual para uso diario, debemos asegurarnos de poder acceder mediante SSH directamente a la cuenta.

Nota: Hasta verificar que puede iniciar sesión y usar sudo con su nuevo usuario, le recomendamos permanecer conectado como root. De esta manera, si tienes problemas, puedes solucionarlos y realizar los cambios necesarios como root.

El proceso para configurar el acceso SSH para su nuevo usuario depende de si la cuenta raíz de su servidor utiliza una contraseña o claves SSH para la autenticación.

Si la cuenta raíz utiliza autenticación de contraseña

Si inició sesión en su cuenta raíz usando una contraseña, entonces la autenticación de contraseña está habilitada para SSH. Puede acceder mediante SSH a su nueva cuenta de usuario abriendo una nueva sesión de terminal y usando SSH con su nuevo nombre de usuario:
```
ssh username @ your_server_ip
```

Después de ingresar su contraseña de usuario habitual, iniciará sesión. Recuerde, si necesita ejecutar un comando con privilegios administrativos, escriba sudo antes de esta manera:
```
sudo command_to_run
```

Se le solicitará su contraseña de usuario habitual cuando utilice Sudo por primera vez en cada sesión (y periódicamente después).

Para mejorar la seguridad de su servidor, recomendamos encarecidamente configurar claves SSH en lugar de utilizar la autenticación con contraseña. Siga nuestra guía sobre cómo configurar claves SSH en Debian 11 para aprender cómo configurar la autenticación basada en claves.

Si la cuenta raíz utiliza autenticación de clave SSH
Si inició sesión en su cuenta raíz usando claves SSH, la autenticación de contraseña está deshabilitada para SSH. Deberá agregar una copia de su clave pública local al archivo ~/.ssh/authorized_keys del nuevo usuario para iniciar sesión correctamente.

Dado que su clave pública ya está en el archivo ~/.ssh/authorized_keys de la cuenta raíz en el servidor, podemos copiar ese archivo y estructura de directorio a nuestra nueva cuenta de usuario en nuestra sesión existente con el comando cp. Luego, podemos ajustar la propiedad de los archivos usando el comando chown.

Asegúrese de cambiar las partes resaltadas del siguiente comando para que coincidan con el nombre de su usuario habitual:
```
cp -r ~/.ssh/home/username
chwon -R username:username /home/username/.ssh
```

El comando cp -r copia todo el directorio al directorio de inicio del nuevo usuario, y el comando chown -R cambia el propietario de ese directorio (y todo lo que contiene) al nombre de usuario especificado: nombre de grupo (Debian crea un grupo con el mismo nombre que su nombre de usuario por defecto).

Ahora, abre una nueva sesión de terminal e inicia sesión vía SSH con tu nuevo nombre de usuario:
```
ssh username @ your_server_ip
```

Debería iniciar sesión en la nueva cuenta de usuario sin utilizar una contraseña. Recuerde, si necesita ejecutar un comando con privilegios administrativos, escriba sudo antes de esta manera:
```
sudo command_to_run
```
Se le solicitará su contraseña de usuario habitual cuando utilice Sudo por primera vez en cada sesión (y periódicamente después).

Referencia:
```
https://www.digitalocean.com/community/tutorials/initial-server-setup-with-debian-11
```