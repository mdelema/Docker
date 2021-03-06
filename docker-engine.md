# Docker Application Container Engine


#### Paso 1: Instalar Docker
Primero, actualice su lista de paquetes existente:
```ssh
sudo apt update
```

A continuación, instale algunos paquetes de requisitos previos que permitan a apt usar paquetes a través de HTTPS:
```ssh
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Luego, añada la clave de GPG para el repositorio oficial de Docker en su sistema:
```ssh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
``` 

Agregue el repositorio de Docker a las fuentes de APT:
```ssh
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

A continuación, actualice el paquete de base de datos con los paquetes de Docker del repositorio recién agregado:
```ssh
sudo apt update
```
Asegúrese de estar a punto de realizar la instalación desde el repositorio de Docker en lugar del repositorio predeterminado de Ubuntu:
```ssh
apt-cache policy docker-ce
```
```ssh    
Si bien el número de versión de Docker puede ser distinto, verá un resultado como el siguiente:
  Output of apt-cache policy docker-ce
  docker-ce:
    Installed: (none)
    Candidate: 5:19.03.9~3-0~ubuntu-focal
    Version table:
       5:19.03.9~3-0~ubuntu-focal 500
          500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
Observe que docker-ce no está instalado, pero la opción más viable para la instalación es del repositorio de Docker para Ubuntu 20.04 (focal).
```

Por último, instale Docker:
```ssh
sudo apt install docker-ce
```

Compruebe que funcione:
```ssh
sudo systemctl status docker
```
```ssh
El resultado debe ser similar al siguiente, y mostrar que el servicio está activo y en ejecución:
  Output
  ● docker.service - Docker Application Container Engine
       Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled) 
       Active: active (running) since Tue 2020-05-19 17:00:41 UTC; 17s ago  
  TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 24321 (dockerd)
      Tasks: 8
     Memory: 46.4M
     CGroup: /system.slice/docker.service
             └─24321 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

### Paso 2 : Ejecutar el comando Docker sin sudo (opcional)
Por defecto, el comando docker solo puede ser ejecutado por el usuario root o un usuario del grupo docker, que se crea automáticamente durante el proceso de instalación de Docker. 

Si desea evitar escribir sudo al ejecutar el comando docker, agregue su nombre de usuario al grupo docker:
```ssh
sudo usermod -aG docker ${USER}
```

Para aplicar la nueva membresía de grupo, cierre la sesión del servidor y vuelva a iniciarla o escriba lo siguiente:
```ssh
su - ${USER}
```
Para continuar, se le solicitará ingresar la contraseña de su usuario.

Confirme que ahora su usuario se agregó al grupo docker escribiendo lo siguiente:
```ssh
id -nG
```
```ssh
    Output
    sammy sudo docker
```

Si debe agregar al grupo docker un usuario con el que no inició sesión, declare dicho nombre de usuario de forma explícita usando lo siguiente:
```ssh
sudo usermod -aG docker username
```

### Paso 3: Usar el comando docker
El uso de docker consiste en pasar a este una cadena de opciones y comandos seguida de argumentos. La sintaxis adopta esta forma:
```ssh
docker [option] [command] [arguments]
```
Para ver todos los subcomandos disponibles, escriba lo siguiente:

```ssh
docker
```
```ssh
A partir de Docker 19, en la lista completa de subcomandos disponibles se incluye lo siguiente:

  Output
    attach      Attach local standard input, output, and error streams to a running container
    build       Build an image from a Dockerfile
    commit      Create a new image from a container's changes
    cp          Copy files/folders between a container and the local filesystem
    create      Create a new container
    diff        Inspect changes to files or directories on a container's filesystem
    events      Get real time events from the server
    exec        Run a command in a running container
    export      Export a container's filesystem as a tar archive
    history     Show the history of an image
    images      List images
    import      Import the contents from a tarball to create a filesystem image
    info        Display system-wide information
    inspect     Return low-level information on Docker objects
    kill        Kill one or more running containers
    load        Load an image from a tar archive or STDIN
    login       Log in to a Docker registry
    logout      Log out from a Docker registry
```

Si desea ver las opciones disponibles para un comando específico, escriba lo siguiente:
```ssh
docker docker-subcommand --help
```

Para ver información sobre Docker relacionada con todo el sistema, utilice lo siguiente:
```ssh
docker info
```

### Paso 4: Trabajar con imágenes de Docker

Para verificar si puede acceder a imágenes y descargarlas de Docker Hub, escriba lo siguiente:
```ssh
docker run hello-world
```
```ssh
El resultado indicará que Docker funciona de forma correcta:

    Output
  Unable to find image 'hello-world:latest' locally
  latest: Pulling from library/hello-world
  0e03bdcc26d7: Pull complete
  Digest: sha256:6a65f928fb91fcfbc963f7aa6d57c8eeb426ad9a20c7ee045538ef34847f44f1
  Status: Downloaded newer image for hello-world:latest

  Hello from Docker!
  This message shows that your installation appears to be working correctly.
  ...
```

Inicialmente, Docker no pudo encontrar la imagen de hello-world a nivel local. Por ello la descargó de Docker Hub, el repositorio predeterminado. Una vez que se descargó la imagen, Docker creó un contenedor a partir de ella y de la aplicación dentro del contenedor ejecutado, y mostró el mensaje.

Puede buscar imágenes disponibles en Docker Hub usando el comando docker con el subcomando search. Por ejemplo, para buscar la imagen de Ubuntu, escriba lo siguiente:
```ssh
docker search ubuntu
```
La secuencia de comandos rastreará Docker Hub y mostrará una lista de todas las imágenes cuyo nombre coincida con la cadena de búsqueda. En este caso, el resultado será similar a lo siguiente:

```ssh
  Output
  NAME                                                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
  ubuntu                                                    Ubuntu is a Debian-based Linux operating sys…   10908               [OK]
  dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interface …   428                                     [OK]
  rastasheep/ubuntu-sshd                                    Dockerized SSH service, built on top of offi…   244                                     [OK]
  consol/ubuntu-xfce-vnc                                    Ubuntu container with "headless" VNC session…   218                                     [OK]
  ubuntu-upstart                                            Upstart is an event-based replacement for th…   108                 [OK]
  ansible/ubuntu14.04-ansible                               Ubuntu 14.04 LTS with
  ...
```

Ejecute el siguiente comando para descargar la imagen oficial de ubuntu a su ordenador:
```ssh
docker pull ubuntu
```

Verá el siguiente resultado:
```ssh
  Output
  Using default tag: latest
  latest: Pulling from library/ubuntu
  d51af753c3d3: Pull complete
  fc878cd0a91c: Pull complete
  6154df8ff988: Pull complete
  fee5db0ff82f: Pull complete
  Digest: sha256:747d2dbbaaee995098c9792d99bd333c6783ce56150d1b11e333bbceed5c54d7
  Status: Downloaded newer image for ubuntu:latest
  docker.io/library/ubuntu:latest
```

Para ver las imágenes descargadas a su computadora, escriba lo siguiente:
```ssh
docker images
```
El resultado debe tener un aspecto similar al siguiente:
```ssh
  Output
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  ubuntu              latest              1d622ef86b13        3 weeks ago         73.9MB
  hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
```

### Paso 5: Ejecutar un contenedor de Docker

Como ejemplo, ejecutemos un contenedor usando la imagen más reciente de Ubuntu. La combinación de los conmutadores -i y -t le proporcionan un acceso interactivo del shell al contenedor:
```ssh
docker run -it ubuntu
```
Su símbolo del sistema debe cambiar para reflejar el hecho de que ahora trabaja dentro del contenedor y debe adoptar esta forma:
```ssh
Output
    root@d9b100f2f636:/#
```
Tenga en cuenta el ID del contenedor en el símbolo del sistema. En este ejemplo, es d9b100f2f636. Más adelante, necesitará ese ID de contenedor para identificar el contenedor cuando desee eliminarlo.

Ahora puede ejecutar cualquier comando dentro del contenedor. Por ejemplo, actualicemos la base de datos del paquete dentro del contenedor. No es necesario prefijar ningún comando con sudo, ya que realiza operaciones dentro del contenedor como el usuario root:
```ssh
    $ apt update
```
Luego, instale cualquier aplicación en él. Probemos con Node.js:
```ssh
apt install nodejs
```
Con esto, se instala Node.js en el contenedor desde el repositorio oficial de Ubuntu. Cuando finalice la instalación, verifique que Node.js esté instalado:
```ssh
node -v
```
Verá el número de versión en su terminal:
```ssh
Output
v10.19.0
```
Cualquier cambio que realice dentro del contenedor solo se aplica a este.

Para cerrar el contenedor, escriba
```ssh
exit 
```
en la línea de comandos.

### Paso 6: Administrar contenedores de Docker
Después de usar Docker durante un tiempo, tendrá muchos contenedores activos (en ejecución) e inactivos en su computadora. Para ver los activos, utilice lo siguiente:
```ssh
docker ps
```
Verá una salida similar a la siguiente:
```ssh
Output
CONTAINER ID        IMAGE               COMMAND             CREATED             
```

Para ver todos los contenedores, activos e inactivos, ejecute docker ps con el conmutador -a:
```ssh
docker ps -a
```
Visualizará un resultado similar a esto:
```ssh
1c08a7a0d0e4        ubuntu              "/bin/bash"         2 minutes ago       Exited (0) 8 seconds ago                       quizzical_mcnulty
a707221a5f6c        hello-world         "/hello"            6 minutes ago       Exited (0) 6 minutes ago                       youthful_curie
```
Para ver el último contenedor que creó, páselo al conmutador -l:
```ssh
docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
1c08a7a0d0e4        ubuntu              "/bin/bash"         2 minutes ago       Exited (0) 40 seconds ago                       quizzical_mcnulty
```
Para iniciar un contenedor detenido, utilice:
```ssh
docker start
```
seguido del o el nombre ID del contenedor. 

Ej: Vamos a iniciar el contenedor basado en Ubuntu con el ID 1c08a7a0d0e4:
```ssh
docker start 1c08a7a0d0e4
```
El contenedor se iniciará y podrá usar docker ps para ver su estado:
```ssh
Output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
1c08a7a0d0e4        ubuntu              "/bin/bash"         3 minutes ago       Up 5 seconds                            quizzical_mcnulty
```
Para detener un contenedor en funcionamiento utilice:
```ssh
docker stop
```
seguido del ID o nombre del contenedor. 
Ej: Esta vez usaremos el nombre que Docker asignó al contenedor, que es quizzical_mcnulty:
```ssh
docker stop quizzical_mcnulty
```
Para eliminar un contenedor:
```ssh
docker rm
```
y use nuevamente el ID o el nombre del contenedor. 
Ej: Utilice el comando docker ps -a para encontrar el ID o nombre del contenedor asociado con la imagen hello-world y elimínelo.
```ssh
docker rm youthful_curie
```
Puede iniciar un nuevo contenedor y darle un nombre usando el conmutador --name. También podrá usar el conmutador de --rm para crear un contenedor que se elimine de forma automática cuando se detenga. Consulte el comando docker run help para obtener más información sobre estas y otras opciones.

### Paso 7: Confirmar cambios aplicados a una imagen de Docker en un contenedor

Confirme los cambios en una nueva instancia de imagen de Docker utilizando el siguiente comando:
```ssh
docker commit -m "What you did to the image" -a "Author Name" container_id repository/new_image_name
```
El conmutador -m es para el mensaje de confirmación que le permite a usted y a otros a saber qué cambios realizaron, mientras que -a se utiliza para especificar el autor. El container_id es el que observó anteriormente en el tutorial cuando inició la sesión interactiva de Docker.

Por ejemplo, para el usuario sammy, con el ID de contenedor d9b100f26f636, el comando sería el siguiente:
```ssh
docker commit -m "added Node.js" -a "sammy" d9b100f2f636 sammy/ubuntu-nodejs
```
Cuando confirme una imagen, la nueva imagen se guardará a nivel local en su computadora. 

Listar las imágenes de Docker de nuevo mostrará la nueva imagen, así como la anterior de la que se derivó:
```ssh
docker images
```
Verá resultados como este:
```ssh
Output
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
sammy/ubuntu-nodejs   latest              7c1f35226ca6        7 seconds ago       179MB
...
```
En este ejemplo ubuntu-nodejs es la nueva imagen, derivada de la imagen ubuntu existente de Docker Hub. La diferencia de tamaño refleja los cambios realizados. En este ejemplo, el cambio fue la instalación de NodeJS. Por ello, la próxima vez que deba ejecutar un contenedor usando Ubuntu con NodeJS preinstalado, podrá usar simplemente la nueva imagen.

### Paso 8: Introducir imágenes de Docker en un repositorio de Docker

Para introducir su imagen, primero inicie sesión en Docker Hub.
```ssh
docker login -u docker-registry-username
```
Se le solicitará autenticarse usando su contraseña de Docker Hub. Si especificó la contraseña correcta, la autenticación tendrá éxito.

Nota: Si su nombre de usuario de registro de Docker es diferente del nombre de usuario local que usó para crear la imagen, deberá etiquetar su imagen con su nombre de usuario de registro. Para el ejemplo que se muestra en el último paso, deberá escribir lo siguiente:
```ssh
docker tag sammy/ubuntu-nodejs docker-registry-username/ubuntu-nodejs
```
Luego podrá introducir su propia imagen usando lo siguiente:
```ssh
docker push docker-registry-username/docker-image-name
```
Para introducir la imagen ubuntu-nodejs en el repositorio de sammy, el comando sería el siguiente:
```ssh
docker push sammy/ubuntu-nodejs
```
El proceso puede tardar un tiempo en completarse cuando se suben las imágenes, pero una vez que finalice el resultado será el siguiente:
```ssh
Output
The push refers to a repository [docker.io/sammy/ubuntu-nodejs]
e3fbbfb44187: Pushed
5f70bf18a086: Pushed
a3b5c80a4eba: Pushed
7f18b442972b: Pushed
3ce512daaf78: Pushed
7aae4540b42d: Pushed
...
```

Una vez que introduce una imagen en un registro, esta debe aparecer en el panel de su cuenta, como se muestra en la siguiente captura:
Nuevo listado de imágenes de Docker en Docker Hub
Si un intento de introducción produce un error de este tipo, es probable que no haya iniciado sesión:
```ssh
Output
The push refers to a repository [docker.io/sammy/ubuntu-nodejs]
e3fbbfb44187: Preparing
5f70bf18a086: Preparing
a3b5c80a4eba: Preparing
7f18b442972b: Preparing
3ce512daaf78: Preparing
7aae4540b42d: Waiting
unauthorized: authentication required
```
Inicie sesión con docker login y repita el intento de introducción. Luego, compruebe que exista en su página de repositorios de Docker Hub.


###### Referencia
> https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-es
