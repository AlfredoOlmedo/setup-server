# Código automatización
Scripts y Código para automatización SRE

## Descripción
Ejemplos de Scripts y Código para automatizacion


## Instalación

### Python

Para instalar y ejecutar scripts de automatización en Python, puedes seguir los siguientes pasos:

- Instala Python: Si aún no tienes Python instalado en tu sistema, puedes descargarlo e instalarlo desde el sitio web oficial de Python.

- Instala las bibliotecas necesarias: Dependiendo del script de automatización que estés utilizando, es posible que necesites instalar ciertas bibliotecas de Python. Puedes hacerlo utilizando el administrador de paquetes de Python, pip. Por ejemplo, si estás utilizando un script que requiere la biblioteca paquete,

Instalación paquete usando pip:
```
pip install paquete
```

Ejecuta el script: Una vez que hayas instalado todas las bibliotecas necesarias, puedes ejecutar el script de Python utilizando el 
siguiente comando:
```
python nombre_del_script.py
```

Asegúrate de reemplazar nombre_del_script.py con el nombre real de tu script.

### Bash

Para instalar y ejecutar scripts de automatización en Bash, puedes seguir los siguientes pasos:

1. Para crear un script, abre un editor de texto y escribe tus comandos de Bash. Por ejemplo:
```
#!/bin/bash
echo "Hola, mundo!"
```

Guarda este archivo con la extensión .sh, por ejemplo, mi_script.sh.

2. Asigna permisos de ejecución al script: Antes de poder ejecutar tu script, necesitas darle permisos de ejecución. Puedes hacerlo con el siguiente comando:
chmod +x mi_script.sh

3. Ejecuta el script: Ahora puedes ejecutar tu script utilizando el siguiente comando:
```
./mi_script.sh
```

4. Automatiza la ejecución del script: Si deseas que tu script se ejecute automáticamente a intervalos regulares, puedes utilizar la utilidad cron de Linux. Para editar tu crontab y agregar una tarea programada, utiliza el siguiente comando:
```
crontab -e
```
A continuación, agrega una línea para tu script siguiendo la sintaxis de cron. Por ejemplo, para ejecutar mi_script.sh todos los días a las 5 PM, podrías agregar:
```
0 17 * * * /ruta/a/mi_script.sh
```

Puedes personalizarlos tanto como quieras.

## Consideraciones
El repositorio está pensado como referencia para la creación de scripts utilizados para automatización en Administración de sistemas

## Licencia
Bajo Licencia MIT

