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

# Autenticar de forma automática

En git, para subir un repositorio o actualizarlo se requiere de que ingreses tus credenciales para autorizar los, sin embargo esto es un problema cuando requieres de que esto se haga de forma automática. Git tiene la posibilidad de que puedas actualizar el repositorio sin necesidad de ingesar las credenciales pertinetes para conectarte al host remoto, para esto vamos a generar una llave que autentique que estamos autorizados para subir contenido, utilizaremos la herramienta ssh-keygen par esto.

#### ssh-keygen

Escribiremos en el bash lo siguente:
~~~~~
ssh-keygen -t rsa -b 4096 -C "tu_correo@tu_proveedor.com"
~~~~~
El comando `ssh-keygen` crea unas llaves de nuestro equipo(una privada y una publica), la opcion `-t` esta espesificando el tipo de encriptacion que usará en nuestro caso hace uso del tipo "`rsa`", la opción `-b` espesifica la longitud de la llave, que entre más larga mejor encriptación tiene pero tambien realentiza la conexión inicial, se maneja en bits por defecto a 2048 bits en nuestro caso la duplicamos, y la opción `-C` especifica la información del campo de comentario en el archivo de la llave, si no especifica un comentario cuando crea una clave, se crea un comentario predeterminado que incluye el tipo, el creador, la fecha y la hora de la clave, en nuestro caso pusimos el correo que es como nos identificamos en github.

En pantalla nos pedirá un nombre para nuestra llave podemos poner lo que nosotros querramos de preferencia se recomienda un nombre referente al equipo a conectarse con la llave, si no es espesificada por default pondrá de nombre `id_rsa`.

~~~~~
Enter file in which to save the key (/home/tu_usuario/.ssh/id_rsa): mykey
~~~~~
 tambien pedirá una (`passphrase`) que dará una mejor llave pero nos pedirá a cada rato esa clave, por lo tanto lo omitiremos dejandolo vacío, y tambien pedirá la confimacion de este mismo.
~~~~~
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
~~~~~
Esto te generará dos llaves la privada y publica, esta última se identificará con la terminación `.pub` se guardarán en el directorio `/home/tu_usuario/.ssh/`.
~~~~~
Your identification has been saved in /home/tu_usuario/.ssh/id_rsa.
Your public key has been saved in /home/tu_usuario/.ssh/id_rsa.pub.
~~~~~
#### A gregar la llave a github y Configurar

Para que **github** identifique la llave se tiene que agregar para esto necesitamos de entrar a nuestra cuenta e ir a settings.
![settings][1]

luego Seleccionar en "*SSH and GPG keys*"
![ssh][2]

seleccionamos "*new SSH key*"
![new][3]

Colocamos un nombre a nuestra llave en title y agregamos el contenido de nuestra llave publica.
![add][4]

ya por último configuraremos una opcion de nuestro git, colocaremos esto:
~~~~~~
git config remote.origin.url git@github.com:tu_cuenta_github/tu_repositorio.git
~~~~~~
en donde solo estamos remplazando la ubicación del repositorio hacia tu cuenta de *github*, y listo con eso tu podras actualizar tu repositorio sin necesidad de identificarse.
