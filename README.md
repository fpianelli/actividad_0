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

### Escribí tu primera vista
Para escribir una vista en django, debemos ir a polls/views.py.
Creamos algo basico, para eso escribimos lo siguiente: 

from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")

Para poder acceder a dicha vista desde una web, necesitamos mapearla a una URL (debemos definir un URLconf). Para eso debemos crear polls/urls.py y escribir lo siguiente: 

from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"),
]

El próximo paso es agregar el URLconf de polls/urls.py en el URLconf raiz en el proyecto mysite, en mysite/urls.py
Para hacer esto, debemos importar include desde django.urls e insertar un include() en la lista urlpatterns, de la siguiente manera: 

from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("polls/", include("polls.urls")),
    path("admin/", admin.site.urls),
]

- path(): espera dos argumentos: ruta y vista
- include(): permite referenciar otros URLconfs. La idea de include() es facilitar el agregar y operar URLs. Es decir, debemos utilizar include() siempre que queramos agregar otro URL pattern. 

Para verificar que funciona, ejecutamos nuevamente el siguiente comando: 

py manage.py runserver 

### Parte 2: Configuracion de la base de datos
Primero, debemos ir a mysite/settings.py
Este archivo contiene variables que representan la configuración de django. A continuación realizamos el siguiente cambio: 

TIME_ZONE = 'America/Argentina/Buenos_Aires'

Lo que hacemos ahora es crear las tablas en la base de datos integrada en django (SQLlite) con el siguiente comando: 

py manage.py migrate

El comando migrate mira al apartado de INSTALLED_APPS en mysite/settings.py y crea las tablas necesarias segun la configuración de la base de datos en mysite/settings.py

### Creando modelos
Creamos dos modelos: Pregunta y respuesta. La pregunta tiene la pregunta en sí y una fecha de publicación. Respuesta tiene el texto de la respuesta y un recuento de votos. Cada respuesta está asociada a una pregunta. Estos conceptos se representan mediante clases en python. Para hacer esto, debemos ir a polls/models.py y escribir lo siguiente: 

from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)




