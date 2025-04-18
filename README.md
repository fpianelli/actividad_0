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

`mkdir djangotutorial`

2. ejecutamos el siguiente comando para iniciar un proyecto de django

`django-admin startproject mysite djangotutorial`

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

`py manage.py runserver`

Luego de ejecutar este comando, entre toda la informacion que salga, debemos visitar http://127.0.0.1:8000/ en la web para visitar la página web creada por el proyecto. 

### Creando la aplicación de encuestas
Creamos la aplicacion de encuestas en el directorio djangotutorial. Para eso, ejecutamos el siguiente comando: 

`py manage.py startapp polls`

Este comando crea un directorio llamado polls, el cual almacenará la estructura completa de la aplicación. 

### Escribí tu primera vista
Para escribir una vista en django, debemos ir a polls/views.py.
Creamos algo basico, para eso escribimos lo siguiente: 

```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

Para poder acceder a dicha vista desde una web, necesitamos mapearla a una URL (debemos definir un URLconf). Para eso debemos crear el archivo polls/urls.py y escribir lo siguiente: 

```
from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

El próximo paso es agregar el URLconf de polls/urls.py en el URLconf raiz en el proyecto mysite, en mysite/urls.py
Para hacer esto, debemos importar include desde django.urls e insertar un include() en la lista urlpatterns, de la siguiente manera: 

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("polls/", include("polls.urls")),
    path("admin/", admin.site.urls),
]
```

- path(): espera al menos dos argumentos: ruta y vista
- include(): permite referenciar otros URLconfs. La idea de include() es facilitar el agregar y operar URLs. Es decir, debemos utilizar include() siempre que queramos agregar otro URL pattern. 

Para verificar que funciona, ejecutamos nuevamente el siguiente comando: 

`py manage.py runserver`

### Parte 2: Configuracion de la base de datos
Primero, debemos ir a mysite/settings.py  
Este archivo contiene variables que representan la configuración de django. A continuación realizamos el siguiente cambio: 

`TIME_ZONE = 'America/Argentina/Buenos_Aires'`

Lo que hacemos ahora es crear las tablas en la base de datos integrada en django (SQLlite) con el siguiente comando: 

`py manage.py migrate`

El comando migrate mira al apartado de INSTALLED_APPS en mysite/settings.py y crea las tablas necesarias segun la configuración de la base de datos en mysite/settings.py

### Creando modelos
Creamos dos modelos: Pregunta y respuesta. Pregunta tiene la pregunta en sí y una fecha de publicación. Respuesta tiene el texto de la respuesta y un recuento de votos. Cada respuesta está asociada a una pregunta. Para hacer esto, debemos ir a polls/models.py y escribir lo siguiente: 

```
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

### Activando modelos
Lo que vamos a hacer ahora es decirle a nuestro proyecto que nuestra app de encuestas (polls) está instalada. Para incluir la aplicación en nuestro proyecto, debemos agregar una referencia a su clase de configuración en el apartado INSTALLED_APPS en mysite/settings.py (polls.apps.PollsConfig)
Ahora ejecutamos el siguiente comando: 

`py manage.py makemigrations polls`

Con este comando, le dijimos a django que hicimos modificaciones a nuestros modelos y queres que los cambios se guarden como una migración. 
Si ejecutamos el siguiente comando: 

`py manage.py sqlmigrate polls 0001`

podemos ver el código SQL generado en la migración. Este comando no corre la migración en tu base de datos. 
Para crear las tablas en la base de datos, ejectuamos el siguiente comando: 

`py manage.py migrate`

Este comando permite sincronizar mis modelos con mis tablas en la base de datos. Para resumir, los tres pasos a seguir para agregar nuevos modelos o modificar modelos son: 

1. Cambiar los modelos (en models.py)
2. Correr `py manage.py makemigrations` para crear las migraciones para esos cambios
3. Correr `py manage.py migrate` para aplicar los cambios en la base de datos

### Jugando con la API
Para llamar la shell de Python, utilizamos el siguiente comando: 

`py manage.py shell` 

A continuación hay algunos comandos que podemos utilizar en la shell:

- Question.objects.all(): nos muestra todos los objetos de tipo Question.
- from django.utils import timezone: para utilizar funcionalidades de timezone.
- q = Question(question_text="What's new?", pub_date=timezone.now()): para crear una pregunta.
- q.save(): para guardar en la bd.
- q.id: nos muestra el id de la pregunta.
- q.question_text: nos muestra la pregunta.
- q.pub_date: nos muestra la fecha de publicación.
-  q.question_text = "what's up?": cambiamos el contenido del texto de la pregunta. Debemos guardarlo con q.save().
- Question.objects.filter(id=1): permite filtrar por id.
- Question.objects.filter(question_text__startswith="what"): otra forma de filtrar.
- Question.objects.get(pub_date__year=current_year): para obtener una pregunta en especifico.
- Question.objects.get(pk=1): busca por primary key.
- q.was_published_recently(): para invocar las operaciones dentro del modelo.
- q.choice_set.all(): accedo a las respuestas vinculadas a mi pregunta
- q.choice_set.create(choice_text="not much", votes=0): para crear una respuesta a una pregunta en especifico.
- c.question: puedo acceder a la pregunta desde la respuesta.
- q.choice_set.count(): cuenta la cantidad de respuestas a una pregunta. 
- Choice.objects.filter(question__pub_date__year=current_year): muestra todas las respuestas cuya pregunta fue publicada este año. 
-  c = q.choice_set.filter(choice_text__startswith="just hacking"): fitlramos una respuesta
- c.delete(): la eliminamos

### Introduciendo a Django Admin - Creando un usuario Admin
Primero, necesitamos crear un usuario que pueda logearse al sitio para el administrador. Para eso, debemos correr el siguiente comando: 

`py manage.py createsuperuser`

Luego podremos correr la aplicación e ingresar al sitio del administrador con las credenciales del super usuario que creamos.

### Hacer que la aplicación de encuestas sea modificable en el sitio administrativo
Debemos decirle al admin que los objetos de tipo Question tienen una interfaz para el sitio del administrador. Para eso, debemos ir al archivo polls/admin.py y editarlo para que esté de la siguiente manera: 

```
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```

Ahora que registramos Question, django sabe que debe ser mostrada en la página del administrador. 

### Parte 3: Escribiendo más vistas
Agregamos más vistas al archivo polls/views.py

```
def detail(request, question_id):
    return HttpResponse("you're looking at question %s." % question_id)

def results(request, question_id):
    response = "you're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

Luego, vamos a conectar esas vistas a sus respectivas urls en polls/urls.py de la siguiente manera:

```
from django.urls import path
from . import views

urlpatterns = [
    <!--ex: /polls/  -->
    path("", views.index, name="index"),
    <!--ex: /polls/5  -->
    path("<int:question_id/>", views.detail, name="detail"),
    <!--ex: /polls/5/results/  -->
    path("<int:question_id>/results/", views.results, name="results"),
    <!--ex: /polls/5/vote  -->
    path("<int:question_id/vote/>", views.vote, name="vote"),
]
```

### Escribir vistas que realmente hagan algo
Utilizamos la API de la base de datos de django, para modificar lo que hace la vista index(). Vamos a mostrar las ultimas 5 preguntas en el sistema, segun la fecha de publicación: 

```
from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by("-pub_date")[:5]
    output = ", ".join([q.question_text for q in latest_question_list])
    return HttpResponse(output)
```

El problema con esto es que el diseño de la página está harcodeado en la vista. Si queremos cambiar esto, podemos utilizar los templates de django para separar el diseño de Python, al crear un template que nuestra vista pueda usar.  
Para eso, vamos a crear un directorio llamado Templates en el directorio polls. Dentro del directorio templates, creamos otro directorio llamado polls, y dentro de este creamos un archivo llamado index.html   
index.html tendrá un codigo html para mostrar el contenido de index. Debemos modificar index() para que utilice el template

```
def index(request):
    latest_question_list = Question.objects.order_by("-pub_date")[:5]
    template = loader.get_template("polls/index.html")
    context = {"latest_question_list": latest_question_list}
    return HttpResponse(template.render(context, request))
```

### un shortcut: Render() 
Django provee un shortcut para no tener que cargar un template, llenar el contexto y enviar la respuesta http con el resultado del template renderizado cada vez que queramos utilizar templates. Si reescribimos el método, resulta:

```
from django.shortcuts import render

def index(request):
    latest_question_list = Question.objects.order_by("-pub_date")[:5]
    context = {"latest_question_list": latest_question_list}
    return render(request, "polls/index.html", context)
```

### Levantando un error 404
En la vista detail, se lanza una excepción del tipo Http404 si la pregunta con el id seleccionado no existe. 

```
from django.http import Http404

def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, "polls/detail.html", {"question": question})
```

Para que esto funcione, debemos crear un template para la vista detail() (detail.html) y agregar 

`{{ question }}`

### Otro shortcut: get_object_or_404()
Es muy comun usar get() y luego levantar una excepción Http404 si no existe un objeto. Podemos utilizar, por consiguiente, el siguiente atajo:

```
from django.shortcuts import get_object_or_404, render

def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, "polls/detail.html", {"question": question})
```

### Utilizar el sistema de templates 
Modificamos el archivo detail.html

### Removiendo URLs harcodeados en los templates
Cuando escribimos el archivo /polls/index.html el link para acceder a los detalles de la pregunta estaba parcialmente harcodeado:

`<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>`

El problema de esto es que se vuelve dificil cambiar URLs en proyectos con muchos templates. Sin embargo, como nosotros previamente definimos un argumento name en path() en polls.urls podemos reducir la dependencia en un path URL especifico y reemplazarlo por este tag: {% url %}

`<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>`

### Namespacing nombres de URL
En proyectos reales de Django, pueden haber muchas aplicaciones. ¿Cómo hace Django para diferenciar el nombre de un URL entre todas estas aplicaciones? Por ejemplo, la app de encuestas tienen una vista "detail", pero la misma vista puede estar en otra aplicación, dentro del mismo proyecto. 
La respuesta está en agregar un Namespace en el URLconf. En polls/urls.py agregamos la siguiente linea de codigo:

`app_name = "polls"`

Ahora si cambiamos index.html y ponemos en el link que tratamos hace un rato:

`<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>`

Esto permitirá distinguir entre diferentes aplicaciones. 

### Parte 4: Escribir un minimal form
En esta parte, vamos a hacer un form en polls/detail.html  
Una vez hecho el html necesario, lo que haremos es crear una vista para tomar los datos del formulario y hacer algo con esos datos.  
Agregamos el siguiente codigo en polls/views.py en vote()

```
from django.db.models import F
from django.http import HttpResponse, HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse

from .models import Choice, Question


# ...
def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST["choice"])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(
            request,
            "polls/detail.html",
            {
                "question": question,
                "error_message": "You didn't select a choice.",
            },
        )
    else:
        selected_choice.votes = F("votes") + 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse("polls:results", args=(question.id,)))
```

- request.POST: es un objeto tipo diccionario que te deja acceder a la información del form mediante un key name. 
- F("votes") + 1: le ordena a la base de datos incrementar la cantidad de votos en 1.
- HttpResponseRedirect: toma como argumento el URL al cual el usuario será redireccionado. 
- reverse(): nos evita tener que harcodear el URL en la función de la vista. 

Luego de que alguien vote, la función vote() nos redirije a la página de resultados para la pregunta. Creamos entonces un template results.html para mostrar esa informacion. 

```
def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, "polls/results.html", {"question": question})
```
### Usar vistas genericas: Menos codigo es mejor
Las vistas detail() y results() son muy cortas, y además, redundantes. Lo mismo pasa con la vista index()  
Estas vistas representan un caso básico de desarrollo web, que es obtener datos de una base de datos según un parametro pasado en la URL, cargar un template y retornar un template renderizado.  
Esto es muy comun en django, por eso se utiliza un shortcut llamado sistema de "Vistas Genéricas".  
Para comenzar a usar las vistas genéricas, tenemos que cubrir los siguientes pasos:

1. Convertir el URLconf.
2. Eliminar algunas de las viejas e innecesarias vistas.
3. Introducir nuevas vistas genéricas basadas en Django.

### Modificamos URLconf
En polls/urls.py modificamos los URLconf de la siguiente manera:

```
from django.urls import path

from . import views

app_name = "polls"
urlpatterns = [
    path("", views.IndexView.as_view(), name="index"),
    path("<int:pk>/", views.DetailView.as_view(), name="detail"),
    path("<int:pk>/results/", views.ResultsView.as_view(), name="results"),
    path("<int:question_id>/vote/", views.vote, name="vote"),
]
```

### Modificamos vistas
Eliminamos las vistas viejas index, detail y results y utilizamos las vistas genéricas de Django en su lugar.  
Para eso, vamos a polls/views.py y cambiamos el codigo de la siguiente manera:

```
class IndexView(generic.ListView):
    template_name = "polls/index.html"
    context_object_name = "latest_question_list"

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by("-pub_date")[:5]


class DetailView(generic.DetailView):
    model = Question
    template_name = "polls/detail.html"


class ResultsView(generic.DetailView):
    model = Question
    template_name = "polls/results.html"
```

















