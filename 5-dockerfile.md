# Dockerfile
Un Dockerfile es un archivo de texto plano que contiene una serie de instrucciones que Docker utiliza para construir una imagen de contenedor Docker. Este conjunto de instrucciones define cómo se debe configurar y construir una imagen de contenedor, incluyendo qué sistema operativo base usar, qué software instalar, qué archivos copiar en el contenedor y cómo configurar el entorno de ejecución.
Las instrucciones en un Dockerfile son simples y están diseñadas para ser leídas y comprendidas fácilmente. 

### FROM 
Esta instrucción especifica la imagen base que se utilizará como punto de partida para construir la nueva imagen de Docker. Por ejemplo, FROM nginx:alpine significa que la nueva imagen se basará en la imagen oficial de Nginx que está etiquetada como "alpine". Al utilizar una imagen base existente, se heredan todas las configuraciones y software incluidos en esa imagen, lo que facilita la creación de nuevas imágenes sin tener que empezar desde cero.

### RUN
La instrucción RUN ejecuta comandos en el contenedor durante el proceso de construcción de la imagen. Puedes usar esta instrucción para instalar software, configurar el entorno, o realizar cualquier otra tarea necesaria para preparar la imagen. Por ejemplo, RUN apt-get update && apt-get install -y <paquete> instalará un paquete utilizando el administrador de paquetes apt en una imagen basada en Ubuntu.

### COPY
Esta instrucción copia archivos o directorios desde la máquina host al sistema de archivos del contenedor. Se utiliza para agregar archivos, scripts u otros recursos necesarios para la aplicación en la imagen de contenedor. Por ejemplo, COPY ./mi-app /app copiará el directorio mi-app desde la máquina host al directorio /app en el contenedor.

### ADD
Esta instrucción copia archivos o directorios desde el sistema de archivos de la máquina host al sistema de archivos del contenedor. Aunque es similar a la instrucción COPY, ADD tiene algunas características adicionales, como la capacidad de descomprimir automáticamente archivos y de copiar archivos desde una URL remota.

### CMD 
Esta instrucción define el comando predeterminado que se ejecutará cuando se inicie el contenedor. Puede haber solo una instrucción CMD en un Dockerfile. Si se proporciona más de una, solo la última tendrá efecto. Por ejemplo, CMD ["nginx", "-g", "daemon off;"] ejecutará el servidor web Nginx en modo daemon cuando se inicie el contenedor.

### ENTRYPOINT
Esta instrucción se utiliza para configurar el comando o el script que se ejecutará cuando se inicie un contenedor basado en la imagen. A diferencia de la instrucción CMD, que especifica el comando predeterminado que se ejecutará y que puede ser sobrescrito desde la línea de comandos al iniciar el contenedor, ENTRYPOINT establece un comando que no se puede sobrescribir fácilmente al iniciar el contenedor.
Es importante destacar que, si se proporciona un comando en la línea de comandos al iniciar el contenedor (por ejemplo, docker run <imagen> <comando>), este comando se agregará como argumentos al comando especificado en ENTRYPOINT, en lugar de sobrescribirlo. Esto permite que el contenedor sea más versátil y se adapte a diferentes necesidades de uso.

### EXPOSE
Esta instrucción informa a Docker que el contenedor escuchará en un puerto específico en tiempo de ejecución. No abre realmente el puerto, solo documenta que la aplicación dentro del contenedor puede usar dicho puerto. Por ejemplo, EXPOSE 80 expone el puerto 80 en el contenedor, lo que permite que se acceda a la aplicación que se esté ejecutando en ese puerto desde fuera del contenedor.

### ENV
Esta instrucción se utiliza para definir variables de entorno dentro del contenedor. Las variables de entorno definidas con ENV estarán disponibles para cualquier proceso en el contenedor durante su ejecución. Por ejemplo, ENV MYSQL_ROOT_PASSWORD=password define una variable de entorno llamada MYSQL_ROOT_PASSWORD con el valor password.

##  Para construir una imagen de Docker a partir de un Dockerfile
```
docker build -t <nombre imagen>:<version> .
```
La opción -t se utiliza para etiquetar la imagen que se está construyendo con un nombre y una versión. <nombre_imagen> es el nombre que le quieres dar a la imagen, y <version> es la versión. Por ejemplo, myapp:1.0.
*.* Este punto indica al comando docker build que busque el Dockerfile en el directorio actual. Especifica la ubicación del contexto de la construcción, que incluye el Dockerfile y cualquier otro archivo necesario para la construcción de la imagen.


### Colocar las siguientes instrucciones en un Dockerfile
1. Instalar el sistema operativo centos 7
2. Actualizar el sistema operativo
3. Instalar apache
4. Copiar los archivos desde ./web a /var/www/html
5. Exponer el puerto 80
6. Ejecutar apache

![mapeo](imagenes/dockerfile.PNG)

- apachectl: Es el script de control para el servidor web Apache. Se utiliza para iniciar, detener y controlar el servidor web.
- -D FOREGROUND: Esta opción le dice a Apache que se ejecute en primer plano. Por defecto, Apache se ejecuta como un servicio en segundo plano. Sin embargo, en un contenedor Docker, es preferible que el proceso principal (en este caso, Apache) se ejecute en primer plano para que Docker pueda monitorear el estado del proceso. Si Apache se ejecutara en segundo plano, Docker no podría saber si el servidor web está funcionando correctamente o no.

 
### Ejecutar el archivo Dockerfile y construir una imagen en la versión 1.0
```
# Base de la imagen
FROM centos:7

# Actualiza el sistema operativo
RUN yum update -y

# Instala Apache
RUN yum install httpd -y

# Copia los archivos de la carpeta ./web a /var/www/html
COPY ./web /var/www/html

# Expone el puerto 80
EXPOSE 80

# Inicia Apache en primer plano
CMD ["apachectl", "-D", "FOREGROUND"]

```
1. Descargar la Imagen de CentOS 7
Primero, necesitas descargar la imagen base de CentOS 7.
```
docker pull centos:7
```
Este comando descarga la imagen de CentOS 7 desde el repositorio de Docker Hub.

2. Crear y Ejecutar un Contenedor con CentOS 7
Crea y ejecuta un contenedor interactivo de CentOS 7:
```
docker run -it --name mi-contenedor-centos centos:7 /bin/bash
```
Esto inicia un contenedor en modo interactivo (-it) y abre una sesión de Bash dentro del contenedor.

3. Actualizar el Sistema Operativo dentro del Contenedor
Una vez dentro del contenedor, ejecuta el siguiente comando para actualizar el sistema operativo:
```
yum update -y
```
4. Instalar Apache
Luego, instala Apache ejecutando:
```
yum install httpd -y
```
**¿Cuántos pasos se han ejecutado?**

6

![mapeo](imagenes/dockerfile1.PNG)

**Modificar el archivo index.html para incluir su nombre**
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>Johanna Huaraca</h1>
  </body>
</html>
**¿Cuántos pasos se han ejecutado? ¿Observa algo diferente en la creación de la imagen**

## Mecanismo de caché
Docker usa un mecanismo de caché cuando crea imágenes para acelerar el proceso de construcción y evitar la repetición de pasos que no han cambiado. Cada instrucción en un Dockerfile crea una capa en la imagen final. Docker intenta reutilizar las capas de una construcción anterior si no han cambiado, lo que reduce significativamente el tiempo de construcción.

- Instrucción FROM: Si la imagen base ya está en el sistema, Docker la reutiliza.
- Instrucciones de configuración (ENV, RUN, COPY, etc.): Docker verifica si alguna instrucción ha cambiado. Si no, reutiliza la capa correspondiente de la caché.
- Instrucción COPY y ADD: Si los archivos copiados no han cambiado, Docker reutiliza la capa de caché correspondiente.
![mapeo](imagenes/dockerfile-cache.PNG)

### Crear un contenedor a partir de las imagen creada, mapear todos los puertos
```

```

### ¿Con que puerto host se está realizando el mapeo?
# COMPLETAR CON LA RESPUESTA

**¿Qué es una imagen huérfana?**
Una imagen huérfana en Docker es una imagen que no tiene ninguna etiqueta asociada (un tag). Estas imágenes pueden ocupar espacio en disco innecesariamente y suelen generarse cuando se actualizan o reemplazan imágenes y se pierden las referencias a las versiones anteriores.

En Docker, las imágenes huérfanas se suelen llamar imágenes colgantes o dangling images. Estas imágenes se identifican por tener el identificador <none> tanto para el repositorio como para la etiqueta (tag).

### Identificar imágenes huérfanas
```
docker images -f "dangling=true"
```

### Listar los IDS de las imágenes huérfanas
```
docker images -q -f "dangling=true"
```

### Eliminar imágenes huérfanas
```
docker image prune -f
```

### Ejecutar un archivo Dockerfile que tiene otro nombre
```
docker build -t <nombre imagen>:<version> -f <ruta y nommbre del Dockerfile> .
```

