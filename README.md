# Actividad 0 - tutorial de django 
### Felipe Pianelli

## como empezar

ir a este link (https://docs.djangoproject.com/en/5.2/intro/tutorial01/)

## requisitos previos

tener instalado
- python
- django (para verificar que lo tenes instalado usar  py -m django --version)
- git

## crear el repositorio git

1. git init para crear el repositorio local
2. git add (nombre_archivo) para agregar el archivo a la zona de preparacion
3. git commit -m "first commit" para hacer el commit en el repo local
4. git branch -M main para forzar el nombre de la rama actual a main
5. git remote add origin (url del repo en github) (traemos el repo remoto desde github)
6. git push -u origin main para subir los cambios del repo local al remoto

## Tutorial django -  aplicación de encuestas
### Creando un proyecto
1. creamos un nuevo directorio

mkdir djangotutorial

2. ejecutamos el siguiente comando para iniciar un proyecto de django

django-admin startproject mysite djangotutorial

3. el proyecto crea los siguientes archivos:

- manage.py: nos permite interactuar con el proyecto de django de diversas maneras
- mysite/: directorio que es el actual paquete de python para tu proyecto. Podremos luego importar los archivos que contenga dentro.
- mysite/--init--.py: es un archivo vacio que le dice a python que este directorio debe ser considerado un paquete de python.
- mysite/asgi.py: para deployar nuestro proyecto utilizando servidores web compatibles con ASGI.
- mysite/settings.py: configuracion para el proyecto de django.
- mysite/urls.py: una "tabla de contenidos" de mi sitio que utiliza django.
- mysite/wsgi.py: para deployar nuestro proyecto utilizando servidores web compatibles con WSGI.

### El servidor de desarrollo 
para verificar que el proyecto funciona, ejecutamos el siguiente comando:

py manage.py runserver

al ejecutar este comando, entre toda la informacion que salga, debemos visitar http://127.0.0.1:8000/ en la web, y veremos una pagina web. Ahi podremos ver una el servidor de desarrollo escrito completamente en python. 

### Creando la aplicación de encuestas
Creamos la aplicacion de encuestas en el directorio djangotutorial. Para eso, ejecutamos el siguiente comando: 

py manage.py startapp polls

Este comando crea un directorio polls, el cual almacenará la estructura completa de la aplicación. 





