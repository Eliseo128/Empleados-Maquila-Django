# Empleados-Maquila-Django
proyecto
¡Excelente! Claro que sí. A continuación, te guiaré paso a paso para crear el proyecto `sistema_admin_empleado` con la aplicación `informacion_empleado` en Django y Bootstrap.

He reestructurado y traducido el código según tus especificaciones para que sea completamente funcional y se parezca lo más posible a la imagen que proporcionaste.

### Resumen de Cambios y Características:
1.  **Traducción:** Nombres de modelos, campos, URLs y texto en las plantillas están en español.
2.  **CRUD Completo:** Funcionalidad para Crear, Leer, Actualizar y Borrar (CRUD) para Departamentos, Posiciones y Empleados.
3.  **Interfaz con Bootstrap:** El diseño se basa en Bootstrap 5 y se asemeja a tu captura de pantalla, incluyendo la barra lateral, tablas y botones con iconos (usando Font Awesome).
4.  **Estructura Organizada:** Se explica la estructura de carpetas y archivos para una fácil comprensión.

---

### Paso 1: Configuración Inicial del Proyecto

Abre tu terminal o línea de comandos y ejecuta los siguientes comandos:

```bash
# 1. Crear el proyecto Django
django-admin startproject sistema_admin_empleado

# 2. Navegar a la carpeta del proyecto
cd sistema_admin_empleado

# 3. Crear la aplicación
python manage.py startapp informacion_empleado
```

### Paso 2: Estructura de Carpetas

Después de los comandos anteriores, tu proyecto tendrá la siguiente estructura. Crearemos las carpetas `templates` y `static` manualmente.

```
sistema_admin_empleado/
├── manage.py
├── sistema_admin_empleado/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── ...
└── informacion_empleado/
    ├── migrations/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    ├── views.py
    └── urls.py  <-- Crearemos este archivo
└── templates/      <-- Crea esta carpeta
    └── informacion_empleado/ <-- Crea esta subcarpeta
        ├── base.html
        ├── inicio.html
        ├── lista_departamentos.html
        ├── gestionar_departamento.html
        ├── lista_posiciones.html
        ├── gestionar_posicion.html
        ├── lista_empleados.html
        ├── gestionar_empleado.html
        └── ver_empleado.html
```

### Paso 3: Configurar `settings.py`

Edita el archivo `sistema_admin_empleado/settings.py`.

```python
# sistema_admin_empleado/settings.py

import os # Asegúrate de que 'os' está importado al principio del archivo
from pathlib import Path

# ... (resto del archivo)

# Agrega tu aplicación a la lista de aplicaciones instaladas
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Nuestra aplicación
    'informacion_empleado',
]

# ...

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # Agrega la ruta a la carpeta 'templates' global
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# ...

# Configuración de archivos estáticos
STATIC_URL = 'static/'

# Para el login y logout (opcional, pero buena práctica)
LOGIN_URL = 'iniciar_sesion'
LOGIN_REDIRECT_URL = 'pagina-inicio'
LOGOUT_REDIRECT_URL = 'pagina-inicio'
```

### Paso 4: Definir los Modelos (Traducidos)

Edita el archivo `informacion_empleado/models.py`. He traducido los nombres de los campos para que coincidan con tu solicitud.

```python
# informacion_empleado/models.py
from django.db import models
from django.utils import timezone

# Modelo para los Departamentos de la empresa
class Departamento(models.Model):
    nombre = models.CharField(max_length=255)
    descripcion = models.TextField()
    # Usaremos 1 para 'Activo' y 0 para 'Inactivo'
    estado = models.IntegerField(default=1) 
    fecha_creacion = models.DateTimeField(default=timezone.now)
    fecha_actualizacion = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.nombre

# Modelo para las Posiciones o cargos
class Posicion(models.Model):
    nombre = models.CharField(max_length=255)
    descripcion = models.TextField()
    # Usaremos 1 para 'Activo' y 0 para 'Inactivo'
    estado = models.IntegerField(default=1)
    fecha_creacion = models.DateTimeField(default=timezone.now)
    fecha_actualizacion = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.nombre

# Modelo principal para los Empleados
class Empleado(models.Model):
    codigo = models.CharField(max_length=100, blank=True)
    nombres = models.TextField()
    segundo_nombre = models.TextField(blank=True, null=True)
    apellidos = models.TextField()
    genero = models.TextField(blank=True, null=True)
    fecha_nacimiento = models.DateField(blank=True, null=True)
    contacto = models.TextField()
    direccion = models.TextField()
    email = models.EmailField()
    departamento = models.ForeignKey(Departamento, on_delete=models.CASCADE, related_name='empleados')
    posicion = models.ForeignKey(Posicion, on_delete=models.CASCADE, related_name='empleados')
    fecha_contratacion = models.DateField()
    salario = models.FloatField(default=0)
    # Usaremos 1 para 'Activo' y 0 para 'Inactivo'
    estado = models.IntegerField(default=1)
    fecha_creacion = models.DateTimeField(default=timezone.now)
    fecha_actualizacion = models.DateTimeField(auto_now=True)

    def __str__(self):
        return f"{self.nombres} {self.apellidos}"
    
    # Propiedad para obtener el nombre completo fácilmente en las plantillas
    @property
    def nombre_completo(self):
        if self.segundo_nombre:
            return f"{self.nombres} {self.segundo_nombre} {self.apellidos}"
        return f"{self.nombres} {self.apellidos}"

```

### Paso 5: Crear las Migraciones y la Base de Datos

Ahora que los modelos están definidos, crea las tablas en la base de datos.

```bash
python manage.py makemigrations
python manage.py migrate

# Opcional: crea un superusuario para acceder al panel de admin
python manage.py createsuperuser
```

### Paso 6: Configurar las URLs

#### Archivo de URLs del Proyecto
Edita `sistema_admin_empleado/urls.py`.

```python
# sistema_admin_empleado/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    # Incluimos las URLs de nuestra aplicación
    path('', include('informacion_empleado.urls')),
]
```

#### Archivo de URLs de la Aplicación
Crea el archivo `informacion_empleado/urls.py` y añade el siguiente código.

```python
# informacion_empleado/urls.py
from . import views
from django.urls import path

urlpatterns = [
    # URLs principales y de autenticación
    path('', views.inicio, name="pagina-inicio"),
    # path('login', auth_views.LoginView.as_view(template_name='informacion_empleado/login.html', redirect_authenticated_user=True), name="iniciar-sesion"),
    # path('logout', views.logoutuser, name="cerrar-sesion"),
    
    # URLs para Departamentos (CRUD)
    path('departamentos/', views.lista_departamentos, name="pagina-departamento"),
    path('departamentos/gestionar/', views.gestionar_departamento, name="gestionar-departamento"),
    path('departamentos/gestionar/<int:pk>/', views.gestionar_departamento, name="gestionar-departamento-pk"),
    path('departamentos/eliminar/<int:pk>/', views.eliminar_departamento, name="eliminar-departamento"),

    # URLs para Posiciones (CRUD)
    path('posiciones/', views.lista_posiciones, name="pagina-posicion"),
    path('posiciones/gestionar/', views.gestionar_posicion, name="gestionar-posicion"),
    path('posiciones/gestionar/<int:pk>/', views.gestionar_posicion, name="gestionar-posicion-pk"),
    path('posiciones/eliminar/<int:pk>/', views.eliminar_posicion, name="eliminar-posicion"),

    # URLs para Empleados (CRUD)
    path('empleados/', views.lista_empleados, name="pagina-empleado"),
    path('empleados/ver/<int:pk>/', views.ver_empleado, name="ver-empleado"),
    path('empleados/gestionar/', views.gestionar_empleado, name="gestionar-empleado"),
    path('empleados/gestionar/<int:pk>/', views.gestionar_empleado, name="gestionar-empleado-pk"),
    path('empleados/eliminar/<int:pk>/', views.eliminar_empleado, name="eliminar-empleado"),
]
```

### Paso 7: Crear las Vistas (`views.py`)

Este es el cerebro de la aplicación. Edita `informacion_empleado/views.py`.

```python
# informacion_empleado/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Departamento, Posicion, Empleado
from django.contrib import messages

# Vista de Inicio
def inicio(request):
    return render(request, 'informacion_empleado/inicio.html')

# --- Vistas para Departamentos ---
def lista_departamentos(request):
    departamentos = Departamento.objects.all()
    return render(request, 'informacion_empleado/lista_departamentos.html', {'departamentos': departamentos})

def gestionar_departamento(request, pk=None):
    if pk:
        departamento = get_object_or_404(Departamento, pk=pk)
    else:
        departamento = None
    
    if request.method == 'POST':
        nombre = request.POST.get('nombre')
        descripcion = request.POST.get('descripcion')
        estado = request.POST.get('estado')

        if departamento:
            departamento.nombre = nombre
            departamento.descripcion = descripcion
            departamento.estado = estado
            departamento.save()
            messages.success(request, 'Departamento actualizado con éxito.')
        else:
            Departamento.objects.create(nombre=nombre, descripcion=descripcion, estado=estado)
            messages.success(request, 'Departamento creado con éxito.')
        return redirect('pagina-departamento')

    return render(request, 'informacion_empleado/gestionar_departamento.html', {'departamento': departamento})

def eliminar_departamento(request, pk):
    departamento = get_object_or_404(Departamento, pk=pk)
    if request.method == 'POST':
        departamento.delete()
        messages.success(request, 'Departamento eliminado con éxito.')
        return redirect('pagina-departamento')
    # Por seguridad, es mejor usar POST para eliminar, pero un GET simple podría funcionar
    # para un ejemplo. Aquí forzamos POST.
    return redirect('pagina-departamento')

# --- Vistas para Posiciones ---
def lista_posiciones(request):
    posiciones = Posicion.objects.all()
    return render(request, 'informacion_empleado/lista_posiciones.html', {'posiciones': posiciones})

def gestionar_posicion(request, pk=None):
    posicion = get_object_or_404(Posicion, pk=pk) if pk else None
    if request.method == 'POST':
        if posicion:
            posicion.nombre = request.POST.get('nombre')
            posicion.descripcion = request.POST.get('descripcion')
            posicion.estado = request.POST.get('estado')
            posicion.save()
            messages.success(request, 'Posición actualizada con éxito.')
        else:
            Posicion.objects.create(nombre=request.POST.get('nombre'), descripcion=request.POST.get('descripcion'), estado=request.POST.get('estado'))
            messages.success(request, 'Posición creada con éxito.')
        return redirect('pagina-posicion')
    return render(request, 'informacion_empleado/gestionar_posicion.html', {'posicion': posicion})

def eliminar_posicion(request, pk):
    posicion = get_object_or_404(Posicion, pk=pk)
    posicion.delete()
    messages.success(request, 'Posición eliminada con éxito.')
    return redirect('pagina-posicion')

# --- Vistas para Empleados ---
def lista_empleados(request):
    empleados = Empleado.objects.select_related('departamento', 'posicion').all()
    return render(request, 'informacion_empleado/lista_empleados.html', {'empleados': empleados})

def ver_empleado(request, pk):
    empleado = get_object_or_404(Empleado, pk=pk)
    return render(request, 'informacion_empleado/ver_empleado.html', {'empleado': empleado})

def gestionar_empleado(request, pk=None):
    empleado = get_object_or_404(Empleado, pk=pk) if pk else None
    departamentos = Departamento.objects.filter(estado=1)
    posiciones = Posicion.objects.filter(estado=1)

    if request.method == 'POST':
        # Lógica para guardar el empleado (crear o actualizar)
        data = request.POST
        if empleado:
            empleado.codigo = data.get('codigo')
            empleado.nombres = data.get('nombres')
            empleado.apellidos = data.get('apellidos')
            empleado.email = data.get('email')
            empleado.contacto = data.get('contacto')
            empleado.departamento_id = data.get('departamento')
            empleado.posicion_id = data.get('posicion')
            empleado.fecha_contratacion = data.get('fecha_contratacion')
            empleado.salario = data.get('salario')
            empleado.estado = data.get('estado')
            empleado.save()
            messages.success(request, 'Empleado actualizado con éxito.')
        else:
            Empleado.objects.create(
                codigo=data.get('codigo'),
                nombres=data.get('nombres'),
                apellidos=data.get('apellidos'),
                email=data.get('email'),
                contacto=data.get('contacto'),
                departamento_id=data.get('departamento'),
                posicion_id=data.get('posicion'),
                fecha_contratacion=data.get('fecha_contratacion'),
                salario=data.get('salario'),
                estado=data.get('estado')
            )
            messages.success(request, 'Empleado creado con éxito.')
        return redirect('pagina-empleado')
    
    context = {
        'empleado': empleado,
        'departamentos': departamentos,
        'posiciones': posiciones
    }
    return render(request, 'informacion_empleado/gestionar_empleado.html', context)

def eliminar_empleado(request, pk):
    empleado = get_object_or_404(Empleado, pk=pk)
    empleado.delete()
    messages.success(request, 'Empleado eliminado con éxito.')
    return redirect('pagina-empleado')
```

### Paso 8: Crear las Plantillas HTML

Aquí crearemos la interfaz.

#### `templates/informacion_empleado/base.html`
Esta es la plantilla principal que las demás heredarán.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Sistema de Empleados{% endblock %}</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome para iconos -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
    <style>
        body {
            display: flex;
            min-height: 100vh;
            background-color: #f8f9fa;
        }
        .sidebar {
            width: 250px;
            background-color: #4a148c; /* Morado oscuro */
            color: white;
            padding-top: 1rem;
        }
        .sidebar .nav-link {
            color: #e0e0e0;
            padding: 0.75rem 1.5rem;
            display: flex;
            align-items: center;
        }
        .sidebar .nav-link:hover, .sidebar .nav-link.active {
            background-color: #6a1b9a;
            color: white;
            border-left: 4px solid #ab47bc;
        }
        .sidebar .nav-link .fa-fw {
            margin-right: 0.75rem;
        }
        .main-content {
            flex-grow: 1;
            padding: 2rem;
            overflow-y: auto;
        }
        .card-header-custom {
            background-color: #f1f1f1;
            border-bottom: 1px solid #dee2e6;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .btn-purple {
            background-color: #6a1b9a;
            color: white;
            border: none;
        }
        .btn-purple:hover {
            background-color: #4a148c;
            color: white;
        }
    </style>
</head>
<body>
    <!-- Barra Lateral de Navegación -->
    <div class="sidebar d-flex flex-column flex-shrink-0">
        <h4 class="text-center mb-4">AdminSys</h4>
        <ul class="nav nav-pills flex-column mb-auto">
            <li class="nav-item">
                <a href="{% url 'pagina-inicio' %}" class="nav-link {% if request.path == '/' %}active{% endif %}">
                    <i class="fa fa-home fa-fw"></i> Inicio
                </a>
            </li>
            <li>
                <a href="{% url 'pagina-departamento' %}" class="nav-link {% if 'departamento' in request.path %}active{% endif %}">
                    <i class="fa fa-building fa-fw"></i> Listar Departamentos
                </a>
            </li>
            <li>
                <a href="{% url 'pagina-posicion' %}" class="nav-link {% if 'posicion' in request.path %}active{% endif %}">
                    <i class="fa fa-list-alt fa-fw"></i> Listar Posición
                </a>
            </li>
            <li>
                <a href="{% url 'pagina-empleado' %}" class="nav-link {% if 'empleado' in request.path %}active{% endif %}">
                    <i class="fa fa-users fa-fw"></i> Listar Empleados
                </a>
            </li>
        </ul>
    </div>

    <!-- Contenido Principal -->
    <main class="main-content">
        {% if messages %}
            {% for message in messages %}
                <div class="alert alert-{{ message.tags }} alert-dismissible fade show" role="alert">
                    {{ message }}
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                </div>
            {% endfor %}
        {% endif %}
        {% block content %}
        <!-- El contenido de cada página irá aquí -->
        {% endblock %}
    </main>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

#### `templates/informacion_empleado/inicio.html`

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}Inicio{% endblock %}

{% block content %}
<div class="container-fluid">
    <h1 class="mt-4">Bienvenido al Sistema de Administración de Empleados</h1>
    <p>Utiliza la barra de navegación de la izquierda para gestionar los departamentos, posiciones y empleados.</p>
</div>
{% endblock %}
```

#### `templates/informacion_empleado/lista_empleados.html`
Este es el archivo clave que coincide con tu imagen.

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}Lista de Empleados{% endblock %}

{% block content %}
<div class="card">
    <div class="card-header card-header-custom">
        <h5 class="mb-0">Lista de Empleados</h5>
        <a href="{% url 'gestionar-empleado' %}" class="btn btn-purple">
            <i class="fa fa-plus"></i> Agregar Nuevo
        </a>
    </div>
    <div class="card-body">
        <table class="table table-striped table-hover">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Código</th>
                    <th scope="col">Nombre</th>
                    <th scope="col">Departamento</th>
                    <th scope="col">Posición</th>
                    <th scope="col">Estado</th>
                    <th scope="col">Acción</th>
                </tr>
            </thead>
            <tbody>
                {% for empleado in empleados %}
                <tr>
                    <th scope="row">{{ forloop.counter }}</th>
                    <td>{{ empleado.codigo }}</td>
                    <td>{{ empleado.nombre_completo }}</td>
                    <td>{{ empleado.departamento.nombre }}</td>
                    <td>{{ empleado.posicion.nombre }}</td>
                    <td>
                        {% if empleado.estado == 1 %}
                            <span class="badge bg-primary">Activo</span>
                        {% else %}
                            <span class="badge bg-danger">Inactivo</span>
                        {% endif %}
                    </td>
                    <td>
                        <a href="{% url 'ver-empleado' empleado.pk %}" class="btn btn-sm btn-purple" title="Ver">
                            <i class="fa fa-eye"></i>
                        </a>
                        <a href="{% url 'gestionar-empleado-pk' empleado.pk %}" class="btn btn-sm btn-primary" title="Editar">
                            <i class="fa fa-edit"></i>
                        </a>
                        <!-- Usamos un formulario para la eliminación segura con POST -->
                        <form action="{% url 'eliminar-empleado' empleado.pk %}" method="post" class="d-inline" onsubmit="return confirm('¿Estás seguro de que quieres eliminar a este empleado?');">
                            {% csrf_token %}
                            <button type="submit" class="btn btn-sm btn-danger" title="Eliminar">
                                <i class="fa fa-trash"></i>
                            </button>
                        </form>
                    </td>
                </tr>
                {% empty %}
                <tr>
                    <td colspan="7" class="text-center">No hay empleados registrados.</td>
                </tr>
                {% endfor %}
            </tbody>
        </table>
    </div>
</div>
{% endblock %}
```

#### `templates/informacion_empleado/gestionar_empleado.html`
Este es el formulario para crear o editar un empleado.

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}{% if empleado %}Editar{% else %}Crear{% endif %} Empleado{% endblock %}

{% block content %}
<div class="card">
    <div class="card-header card-header-custom">
        <h5 class="mb-0">{% if empleado %}Editar Empleado{% else %}Crear Nuevo Empleado{% endif %}</h5>
    </div>
    <div class="card-body">
        <form method="post">
            {% csrf_token %}
            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="codigo" class="form-label">Código de Empleado</label>
                    <input type="text" class="form-control" id="codigo" name="codigo" value="{{ empleado.codigo|default:'' }}" required>
                </div>
                <div class="col-md-6 mb-3">
                    <label for="nombres" class="form-label">Nombres</label>
                    <input type="text" class="form-control" id="nombres" name="nombres" value="{{ empleado.nombres|default:'' }}" required>
                </div>
                <div class="col-md-6 mb-3">
                    <label for="apellidos" class="form-label">Apellidos</label>
                    <input type="text" class="form-control" id="apellidos" name="apellidos" value="{{ empleado.apellidos|default:'' }}" required>
                </div>
                 <div class="col-md-6 mb-3">
                    <label for="email" class="form-label">Email</label>
                    <input type="email" class="form-control" id="email" name="email" value="{{ empleado.email|default:'' }}" required>
                </div>
                <div class="col-md-6 mb-3">
                    <label for="departamento" class="form-label">Departamento</label>
                    <select class="form-select" id="departamento" name="departamento" required>
                        <option value="">Seleccione un departamento...</option>
                        {% for dep in departamentos %}
                        <option value="{{ dep.pk }}" {% if empleado.departamento.pk == dep.pk %}selected{% endif %}>{{ dep.nombre }}</option>
                        {% endfor %}
                    </select>
                </div>
                <div class="col-md-6 mb-3">
                    <label for="posicion" class="form-label">Posición</label>
                    <select class="form-select" id="posicion" name="posicion" required>
                        <option value="">Seleccione una posición...</option>
                        {% for pos in posiciones %}
                        <option value="{{ pos.pk }}" {% if empleado.posicion.pk == pos.pk %}selected{% endif %}>{{ pos.nombre }}</option>
                        {% endfor %}
                    </select>
                </div>
                <div class="col-md-4 mb-3">
                    <label for="fecha_contratacion" class="form-label">Fecha de Contratación</label>
                    <input type="date" class="form-control" id="fecha_contratacion" name="fecha_contratacion" value="{{ empleado.fecha_contratacion|date:'Y-m-d'|default:'' }}" required>
                </div>
                 <div class="col-md-4 mb-3">
                    <label for="salario" class="form-label">Salario</label>
                    <input type="number" step="0.01" class="form-control" id="salario" name="salario" value="{{ empleado.salario|default:'' }}" required>
                </div>
                <div class="col-md-4 mb-3">
                    <label for="estado" class="form-label">Estado</label>
                    <select class="form-select" id="estado" name="estado" required>
                        <option value="1" {% if empleado.estado == 1 %}selected{% endif %}>Activo</option>
                        <option value="0" {% if empleado.estado == 0 %}selected{% endif %}>Inactivo</option>
                    </select>
                </div>
                 <div class="col-md-12 mb-3">
                    <label for="contacto" class="form-label">Contacto</label>
                    <input type="text" class="form-control" id="contacto" name="contacto" value="{{ empleado.contacto|default:'' }}">
                </div>
            </div>
            <hr>
            <div class="text-end">
                <a href="{% url 'pagina-empleado' %}" class="btn btn-secondary">Cancelar</a>
                <button type="submit" class="btn btn-purple">Guardar</button>
            </div>
        </form>
    </div>
</div>
{% endblock %}
```

**Nota:** Por brevedad, no he incluido los archivos HTML para `lista_departamentos`, `gestionar_departamento`, `lista_posiciones`, `gestionar_posicion` y `ver_empleado`. Son muy similares en estructura a los de empleados. Puedes crearlos fácilmente adaptando el código anterior.

### Paso 9: Registrar Modelos en el Admin (Opcional)

Para poder ver y gestionar tus modelos en el panel de administración de Django (`/admin`), edita `informacion_empleado/admin.py`:

```python
# informacion_empleado/admin.py
from django.contrib import admin
from .models import Departamento, Posicion, Empleado

# Register your models here.
admin.site.register(Departamento)
admin.site.register(Posicion)
admin.site.register(Empleado)
```

### Paso 10: ¡Ejecutar el Servidor!

¡Todo está listo! Vuelve a tu terminal y ejecuta el servidor de desarrollo.

```bash
python manage.py runserver
```

Ahora, abre tu navegador y ve a `http://127.0.0.1:8000/`. Deberías ver la página de inicio de tu sistema. Navega a `http://127.0.0.1:8000/empleados/` para ver la lista de empleados que coincide con tu imagen.
