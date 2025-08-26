Guía rápida: Django en Windows con VS Code (lista para GitHub) 🧑‍💻⚡

Objetivo: dejar un proyecto Django funcionando con una app de ejercicios básicos (hola) y rutas de práctica. Sin drama.

✅ Requisitos

Windows

Python 3.x instalado (python --version)

VS Code con extensión Python

Terminal: PowerShell (recomendado)

1) Crear y activar entorno virtual
# Dentro de la carpeta de tu proyecto
python -m venv .venv
.\.venv\Scripts\Activate


Verás (.venv) al inicio de la línea.

2) Instalar Django
python -m pip install --upgrade pip
pip install django
python -m django --version   # para verificar

3) Crear el proyecto
python -m django startproject miweb .


Con . se crea en la carpeta actual (sin subcarpeta extra).

4) Ejecutar el servidor de desarrollo
python manage.py runserver


Navega a http://127.0.0.1:8000/

5) Crear la app hola
python manage.py startapp hola


Agrega la app en miweb/settings.py:

INSTALLED_APPS = [
    # apps de Django...
    'hola',  # 👈 nuestra app
]


La configuración de templates por defecto (APP_DIRS: True) es suficiente.

6) Vistas de práctica (mini-lab)

hola/views.py

from django.http import HttpResponse, JsonResponse
from django.shortcuts import render

def saludo(request):
    return HttpResponse("¡Hola Mundo desde Django! 😎")

def ej1_lista(request):
    frutas = ["manzana", "pera", "uva"]
    print("DEBUG:", frutas)  # se ve en la consola del server
    return HttpResponse(f"Lista: {frutas}")

def ej2_bucle(request):
    nums = [1, 2, 3, 4, 5]
    cuadrados = [n*n for n in nums]
    return HttpResponse("Cuadrados: " + ", ".join(map(str, cuadrados)))

def ej3_diccionario(request):
    persona = {"nombre": "Ana", "edad": 30, "skills": ["C#", "SQL", "Django"]}
    return JsonResponse(persona)  # JSON bonito

def ej4_template(request):
    datos = {"titulo": "Hola Template", "items": ["A", "B", "C"]}
    return render(request, "hola/ej4.html", datos)

def ej5_query_params(request):
    # Ejemplo: /ej5?x=7&y=3
    x = int(request.GET.get("x", 0))
    y = int(request.GET.get("y", 0))
    return HttpResponse(f"{x} + {y} = {x+y}")

7) URLs de la app

Crea hola/urls.py:

from django.urls import path
from . import views

urlpatterns = [
    path("", views.saludo),           # raíz de la app
    path("ej1/", views.ej1_lista),
    path("ej2/", views.ej2_bucle),
    path("ej3/", views.ej3_diccionario),
    path("ej4/", views.ej4_template),
    path("ej5/", views.ej5_query_params),
]


Conecta las URLs de la app en el urls.py del proyecto:

miweb/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("hola.urls")),  # monta la app en la raíz
]

8) Template para ej4

Crea la estructura de templates por app:

<raíz del proyecto>/
├─ hola/
│  ├─ views.py
│  ├─ urls.py
│  └─ templates/
│     └─ hola/
│        └─ ej4.html
└─ miweb/
   └─ settings.py


Contenido de hola/templates/hola/ej4.html:

<!doctype html>
<html>
  <head><meta charset="utf-8"><title>{{ titulo }}</title></head>
  <body>
    <h1>{{ titulo }}</h1>
    <ul>
      {% for i in items %}
        <li>{{ i }}</li>
      {% endfor %}
    </ul>
  </body>
</html>

9) Probar rutas

Servidor:

python manage.py runserver


Rutas:

http://127.0.0.1:8000/ → saludo

http://127.0.0.1:8000/ej1/

http://127.0.0.1:8000/ej2/

http://127.0.0.1:8000/ej3/

http://127.0.0.1:8000/ej4/

http://127.0.0.1:8000/ej5?x=7&y=3

10) Estructura final (referencial)
.
├─ .venv/
├─ hola/
│  ├─ __init__.py
│  ├─ admin.py
│  ├─ apps.py
│  ├─ migrations/
│  ├─ models.py
│  ├─ tests.py
│  ├─ urls.py
│  ├─ views.py
│  └─ templates/
│     └─ hola/
│        └─ ej4.html
├─ miweb/
│  ├─ __init__.py
│  ├─ asgi.py
│  ├─ settings.py
│  ├─ urls.py
│  └─ wsgi.py
├─ db.sqlite3
├─ manage.py
└─ (opcional) main.py  # según versión

11) Comandos útiles
# Activar venv
.\.venv\Scripts\Activate
