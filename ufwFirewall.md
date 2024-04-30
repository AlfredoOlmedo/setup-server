# UFW Uncomplicated Firewall

El cual es un Gestor de Firewall simplificado, el cual tiene un bajo nivel de complejidad y se enfoca en que las reglas sean simple

Necesitas un usuario que no sea Root

#### 1. Instala UFW

```
sudo apt install ufw
```

#### 2. IPV6 con UFW (Opcional)

Este tutorial es escrito usando ipv4 en mente, pero puede ser usado tambien con ipv6 si lo habilitas (Opcional)
para habilitarlo necesitas A. dirigirte a la ubicacion

```
/etc/default/ufw
```

B. Localiza el campo IPV6 y asegurate de habilitarlo
IPV6=yes

C. Guarda y Cierra el archivo

#### 3. Configura el Default Policies (Policitas por Default)
Las primeras reglas que se deben de definir son las Default Policies, estas son reglas que no encuadran con ninguna otra regla preestablecida y son las siguientes Entrada y Salida, por defualt Entrada es Denegada mientras que salida es Otorgada

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

el mensaje que saldra una vez puesta las anteriores reglas son:

```
Output
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
```

Estas Reglas son generalemnte para un usuario comun sin embargo para un Servidor son diferentes, las veremois a continuacion

#### Servidor

Como vimos anteriormente, poniendo las reglas de la anterior forma

Denegara todo acceso de entrada al servidor, y una de las funciones del Servidor es que se permita la entrada al servidor para ciertas funciones, tareas, por lo que tenemos que modificiar las reglas anteriores

#### 4. Permitiendo Comunicaciones SSH

```
sudo ufw allow ssh
Output
Rules updated
Rules updated (v6)
```

Nota:   
Esto permitira la entrada del servicio SSH al puerto por default (22), este puerto en el que escucha el Deamon de forma predeterminada. UFW sabe que puerto esta vinculado para permitir ssh ya que aparece en el archivo /etc/services
Sin embargo es cambiar de puerto al que sea determinado de la siguiente forma
```
sudo ufw allow 2222
```

#### 5. Habilitando UFW

```
sudo ufw enable
```

Nota:   
Para Deshabilitar el UFW

```
sudo ufw diable
```

Para ver las reglas que seteaste
```
sudo ufw status verbose
```

#### 6. Permitiendo otras comunicaciones
En este punto ya habras configurado lo esencial, las coneciones que permitas depende de cada caso y sus necesidades

HTTP
```
sudo ufw allow http
sudo ufw allow http 80
```

HTTPS
```
sudo ufw allow https
sudo ufw allow https 443
```

Nota:   
Los puertos por default son http 80 https 443 Asi que cambialos segun las necesidades

#### Especificando Rangos

Puedes espoecificar reangos con UFW, algunas aplicaciones usan multiples puertos en vez de uno solo
por ejemplo para permitir Conexiones X11 puedes usar los puertos 6000-6007
```
sudo ufw allow 6000:6007/tcp
sudo ufw allow 6000:6007/udp
```

Nota:
Para usar TCP / UDP, es necesario especificarlo despues de haber asiganado el rango de los puertos como vimos anteriormente, esto no fue necesario porque no especificar el protocolo, permite automáticamente ambos protocolos, lo cual está bien en la mayoría de los casos.

#### Direcciones IP específicas
Cuando trabaja con UFW, también puede especificar direcciones IP; por ejemplo, si desea permitir conexiones desde una dirección IP específica como una dirección IP del trabajo o del hogar de 203.0.113.4, debe especificar desde y luego la dirección IP.

```
sudo ufw allow from 203.0.113.4
```

También puede especificar un puerto específico al que la dirección IP puede conectarse agregando a cualquier puerto seguido del número de puerto, por ejemplo, si desea permitir que 203.0.113.4 se conecte al puerto 22 (SSH), use este comando

```
sudo ufw allow from 203.0.113..4 to any port 22
```

#### Subredes
Si desea permitir una subred de direcciones IP, puede hacerlo utilizando la notación CIDR para especificar una máscara de red. por ejemplo, si desea permitir todas las direcciones IP que van desde 203.0.113.1 a 203.0.113.24, puede usar este comando
```
sudo ufw allow from 203.0.113.0/24
```

Asimismo, también puedes especificar el puerto de destino al que la subred 203.0.113.0/24 puede volver a conectarse, usando el puerto 22(SSH) como ejemplo.
```
sudo ufw allow from 203.0.113.0/24 to any 
```

#### Conexiones a una interfaz de red específica
Si desea crear una regla de firewall que solo se aplique a una interfaz de red específica, puede hacerlo especificando "permitir entrada en", seguido del nombre de la interfaz de red. Será útil buscar sus interfaces de red antes de continuar haciéndolo, use este comando

```
ip addr

output
. . .
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state
. . .
3: eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default
. . .
```

El resultado resaltado indica los nombres de las interfaces de red. Por lo general, tienen nombres como eth0 o enp3s2.

Si su servidor tiene una interfaz de red pública llamada eth0-, puede permitir el tráfico http con este comando.

```
sudo ufw allow in on eth0 to any port 80
```

Hacerlo permitiría a su servidor recibir solicitudes HTTP de la Internet pública.
O si desea que un servidor de base de datos MySQL (puerto 3306) escuche las conexiones en las interfaces de red privada eth1, puede usar este comando
```
sudo ufw allow in on eth1 to any port 3306
```
esto permitiría que otros servidores de su red privada se conecten a su base de datos MySQL 

#### Denying Connections

If you haven't changed the default policy for incoming connections, ufw is configured to deny all incoming connections, Generally, this simplifies the process of creating a secure firewall policy by requiring you to create rules that explicitly allow specific ports and IP address though

Sometimes you will want to deny specific connections based on the sources ip address or subnet because you know that it can be attacked, also if you want to change your default incoming policy to allow (which is not recommended) you would need to create deny rules for any services or IP addresses that you don't want to allow connection for.

To write deny rules, you can use the commands described above replacing allow with deny

for example
```
sudo ufw deny from 203.0.113.4
```
or if you want to deny all connections from 203.0.113.4 you could use this command
```
sudo ufw deny from 203.0.113.4
```

#### Negar conexiones
Si no ha cambiado la política predeterminada para las conexiones entrantes, ufw está configurado para denegar todas las conexiones entrantes. Generalmente, esto simplifica el proceso de creación de una política de firewall segura al requerirle que cree reglas que permitan explícitamente puertos y direcciones IP específicos.

A veces querrás denegar conexiones específicas según la dirección IP o la subred de las fuentes porque sabes que pueden ser atacadas. Además, si deseas cambiar tu política entrante predeterminada para permitir (lo cual no se recomienda), necesitarás crear "denegar". ` reglas para cualquier servicio o dirección IP para los que no desea permitir la conexión.

Para escribir reglas de denegación, puede utilizar los comandos descritos anteriormente reemplazando permitir con denegar

Por ejemplo
```
sudo ufw deny from 203.0.113.4
```
o si desea denegar todas las conexiones desde 203.0.113.4, puede usar este comando
```
sudo ufw deny from 203.0.113.4
```

#### Eliminar reglas
Saber cómo eliminar reglas de firewall es tan importante como saber cómo crearlas. Hay dos formas de especificar qué reglas eliminar. -A. por número de regla -B. por regla misma

A. Por número de regla

Lo primero es obtener una lista de las reglas de su firewall, con el comando status tiene la opción number, que muestra números al lado de cada regla.
```
sudo ufw status numbered

Output
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22                         ALLOW IN    15.15.15.0/24
[ 2] 80                         ALLOW IN    Anywhere
```

Eliminar por número
```
sudo ufw delete 2
```

#### B. Por regla misma
La alternativa a los números de regla es especificar la regla real que se va a eliminar. por ejemplo, si desea eliminar la regla "permitir http", puede escribirla así
```
sudo ufw delete allow http
```

También puede especificar la regla con "permitir 80" en lugar del nombre del servicio.
```
sudo ufw delete allow 80 
```
#### 9 Verificar el estado y las reglas de UFW
en cualquier momento puedes comprobar el estado de ufw con este comando
```
sudo ufw status verbose
```

Si ufw está deshabilitado, que es el valor predeterminado, la salida será
```
output

status: inactive
```

Si ufw está activo, lo cual debería estarlo si siguió el paso 3, el resultado dirá que está activo y enumerará las reglas que haya establecido. Por ejemplo, si el firewall está configurado para permitir conexiones SSH (puerto 22) desde cualquier lugar, el resultado podría incluir algo como

```
Output
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere 
```

#### 10 Habilitar, deshabilitar, Estado o Restablecer UFW (opcional)

Permitir
```
sudo ufw enable
```

Denegar
```
sudo ufw disable
```

Estado
```
sudo ufw status
```

Restablecer
```
sudo ufw reset
```

lista todos los perfiles de aplicaciones que ha creado en el firewall
```
sudo ufw app list
```

Referencia:     
https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands