# Uso básico de Git desde consola

## Configuración inicial de git

#### Instalación de Git

Dependiendo de si tenemos instalado `yum` o `apt` utilizaremos uno de los siguientes comandos con privilegios de administrador

~~~~
$ yum install git
$ apt install git
~~~~

#### Configurando datos personales

Es importante configurar los datos personales, ya que al hacer confirmaciones queda un registro de quién modificó qué y podría ser útil para futuras aclaraciones.

~~~
$ git config --global user.name "Carlos Perales"
$ git config --global user.email jc.perales.dj@gmail.com
~~~

#### Agregando un repositorio a un directorio local

Existen dos formas de hacer ésto, una de ellas es clonar un repositorio existente 

~~~
$ git clone https://github.com/JCPerales/guiabasicagit.git nombrecarpeta
~~~

El comando anterior nos creará la carpeta `nombrecarpeta` dentro de nuestro directorio actual con todo el contenido del repositorio, el nombre por default de dicho directorio es origin.

Otra forma de hacer lo anterior es, sobre la carpeta que estamos actualmente, tecleamos:

~~~~
$ git init
$ git remote add nombrerepositorio https://github.com/JCPerales/guiabasicagit.git
$ git push nombrerepositorio master
~~~~

De una u otra manera, ya tenemos un repositorio de manera local.

## Modificaciones locales y remotas.

Una vez que tenemos el repositorio de manera local y nuestros datos personales configurados, podemos modificar archivos, la idea general de trabajo será la siguiente:

	1. Actualizamos nuestro repositorio local (pull).
	2. Hacemos las modificaciones pertinentes.
	3. Añadimos los archivos modificados a preparados para confirmar (add).
	4. Confirmamos las modificaciones (commit).
	5. Actualizamos el repositorio remoto con las modificaciones hechas (push).

Esté será el ciclo que llevaremos a cabo cada vez que comencemos a hacer modificaciones a los archivos del repositorio

Si clonamos el repositorio los camandos serán los siguientes

~~~
$ git pull
$ git add archivomodificados
$ git commit -m "Descripcion de modificaciones"
$ git push
~~~

Entre `pull` y `add` está la parte donde realizamos las modificaciones.

En caso de que hayamos usado `git remote add`, en los comandos `pull` y `push` hay que especificar el nombre del repositorio y la rama (branch) a la cual queremos realizar dicha acción.

~~~
$ git pull nombrerepositorio master
$ git add archivomodificados
$ git commit -m "Descripcion de modificaciones"
$ git push nombrerepositorio master
~~~

Cabe mencionar que si en el comando `commit` no se da la opcion -m, se abrirá el editor predeterminado para que se describan las notificaciones. En caso de que no se hayan configurado los datos personales, recibiremos un mensaje indicando que es necesario proporcionarlos o que los tomará del sistema.

Después de ejecutar el comando `push` habrá que poner nuestras credenciales de github para que se vean reflejados los cambios en el repositorio remoto, si existe algún conflicto entre versiones, habrá que solucionarlo primero para poder aplicar los cambios al repositorio remoto (lo que implica un `add`, `commit` y `push` de nuevo).
