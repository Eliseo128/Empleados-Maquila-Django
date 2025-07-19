¡Por supuesto! Con gusto te proporciono el código para las plantillas que faltan, asegurándome de que el diseño y la funcionalidad coincidan con lo que has descrito y mostrado en las imágenes.

El código se integra perfectamente con las vistas (`views.py`) y URLs (`urls.py`) que creamos anteriormente. Simplemente copia y pega este contenido en los archivos correspondientes dentro de la carpeta `templates/informacion_empleado/`.

---

### 1. Código para `templates/informacion_empleado/lista_departamentos.html`

Esta plantilla mostrará la lista de departamentos con las columnas y acciones que solicitaste.

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}Lista de Departamentos{% endblock %}

{% block content %}
<div class="card">
    <div class="card-header card-header-custom">
        <h5 class="mb-0">Lista de Departamentos</h5>
        <a href="{% url 'gestionar-departamento' %}" class="btn btn-purple">
            <i class="fa fa-plus"></i> Agregar Nuevo
        </a>
    </div>
    <div class="card-body">
        <div class="table-responsive">
            <table class="table table-striped table-hover">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">Nombre Departamento</th>
                        <th scope="col">Descripción</th>
                        <th scope="col">Estado</th>
                        <th scope="col">Acción</th>
                    </tr>
                </thead>
                <tbody>
                    {% for depto in departamentos %}
                    <tr>
                        <th scope="row">{{ forloop.counter }}</th>
                        <td>{{ depto.nombre }}</td>
                        <td>{{ depto.descripcion }}</td>
                        <td>
                            {% if depto.estado == 1 %}
                                <span class="badge" style="background-color: #6a1b9a;">Activo</span>
                            {% else %}
                                <span class="badge bg-danger">Inactivo</span>
                            {% endif %}
                        </td>
                        <td>
                            <!-- Botón para Editar -->
                            <a href="{% url 'gestionar-departamento-pk' depto.pk %}" class="btn btn-sm btn-primary" title="Editar">
                                <i class="fa fa-edit"></i>
                            </a>
                            <!-- Botón para Eliminar (dentro de un formulario para seguridad) -->
                            <form action="{% url 'eliminar-departamento' depto.pk %}" method="post" class="d-inline" onsubmit="return confirm('¿Estás seguro de que quieres eliminar este departamento?');">
                                {% csrf_token %}
                                <button type="submit" class="btn btn-sm btn-danger" title="Eliminar">
                                    <i class="fa fa-trash"></i>
                                </button>
                            </form>
                        </td>
                    </tr>
                    {% empty %}
                    <tr>
                        <td colspan="5" class="text-center">No hay departamentos registrados.</td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>
    </div>
</div>
{% endblock %}
```

### 2. Código para `templates/informacion_empleado/gestionar_departamento.html`

Este es el formulario para crear o editar un departamento.

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}{% if departamento %}Editar{% else %}Crear{% endif %} Departamento{% endblock %}

{% block content %}
<div class="card">
    <div class="card-header card-header-custom">
        <h5 class="mb-0">{% if departamento %}Editar Departamento{% else %}Crear Nuevo Departamento{% endif %}</h5>
    </div>
    <div class="card-body">
        <form method="post">
            {% csrf_token %}
            <div class="mb-3">
                <label for="nombre" class="form-label">Nombre del Departamento</label>
                <input type="text" class="form-control" id="nombre" name="nombre" value="{{ departamento.nombre|default:'' }}" required>
            </div>
            <div class="mb-3">
                <label for="descripcion" class="form-label">Descripción</label>
                <textarea class="form-control" id="descripcion" name="descripcion" rows="3" required>{{ departamento.descripcion|default:'' }}</textarea>
            </div>
            <div class="mb-3">
                <label for="estado" class="form-label">Estado</label>
                <select class="form-select" id="estado" name="estado" required>
                    <option value="1" {% if departamento.estado == 1 %}selected{% endif %}>Activo</option>
                    <option value="0" {% if departamento.estado == 0 %}selected{% endif %}>Inactivo</option>
                </select>
            </div>
            <hr>
            <div class="text-end">
                <a href="{% url 'pagina-departamento' %}" class="btn btn-secondary">Cancelar</a>
                <button type="submit" class="btn btn-purple">Guardar</button>
            </div>
        </form>
    </div>
</div>
{% endblock %}
```

### 3. Código para `templates/informacion_empleado/lista_posiciones.html`

Similar a la de departamentos, esta plantilla lista todas las posiciones.

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}Lista de Posiciones{% endblock %}

{% block content %}
<div class="card">
    <div class="card-header card-header-custom">
        <h5 class="mb-0">Lista de Posiciones</h5>
        <a href="{% url 'gestionar-posicion' %}" class="btn btn-purple">
            <i class="fa fa-plus"></i> Agregar Nueva
        </a>
    </div>
    <div class="card-body">
        <div class="table-responsive">
            <table class="table table-striped table-hover">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">Posición</th>
                        <th scope="col">Descripción</th>
                        <th scope="col">Estado</th>
                        <th scope="col">Acción</th>
                    </tr>
                </thead>
                <tbody>
                    {% for pos in posiciones %}
                    <tr>
                        <th scope="row">{{ forloop.counter }}</th>
                        <td>{{ pos.nombre }}</td>
                        <td>{{ pos.descripcion }}</td>
                        <td>
                            {% if pos.estado == 1 %}
                                <span class="badge" style="background-color: #6a1b9a;">Activo</span>
                            {% else %}
                                <span class="badge bg-danger">Inactivo</span>
                            {% endif %}
                        </td>
                        <td>
                            <!-- Botón para Editar -->
                            <a href="{% url 'gestionar-posicion-pk' pos.pk %}" class="btn btn-sm btn-primary" title="Editar">
                                <i class="fa fa-edit"></i>
                            </a>
                            <!-- Botón para Eliminar -->
                            <form action="{% url 'eliminar-posicion' pos.pk %}" method="post" class="d-inline" onsubmit="return confirm('¿Estás seguro de que quieres eliminar esta posición?');">
                                {% csrf_token %}
                                <button type="submit" class="btn btn-sm btn-danger" title="Eliminar">
                                    <i class="fa fa-trash"></i>
                                </button>
                            </form>
                        </td>
                    </tr>
                    {% empty %}
                    <tr>
                        <td colspan="5" class="text-center">No hay posiciones registradas.</td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>
    </div>
</div>
{% endblock %}
```

### 4. Código para `templates/informacion_empleado/gestionar_posicion.html`

El formulario para crear o editar una posición.

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}{% if posicion %}Editar{% else %}Crear{% endif %} Posición{% endblock %}

{% block content %}
<div class="card">
    <div class="card-header card-header-custom">
        <h5 class="mb-0">{% if posicion %}Editar Posición{% else %}Crear Nueva Posición{% endif %}</h5>
    </div>
    <div class="card-body">
        <form method="post">
            {% csrf_token %}
            <div class="mb-3">
                <label for="nombre" class="form-label">Nombre de la Posición</label>
                <input type="text" class="form-control" id="nombre" name="nombre" value="{{ posicion.nombre|default:'' }}" required>
            </div>
            <div class="mb-3">
                <label for="descripcion" class="form-label">Descripción</label>
                <textarea class="form-control" id="descripcion" name="descripcion" rows="3" required>{{ posicion.descripcion|default:'' }}</textarea>
            </div>
            <div class="mb-3">
                <label for="estado" class="form-label">Estado</label>
                <select class="form-select" id="estado" name="estado" required>
                    <option value="1" {% if posicion.estado == 1 %}selected{% endif %}>Activo</option>
                    <option value="0" {% if posicion.estado == 0 %}selected{% endif %}>Inactivo</option>
                </select>
            </div>
            <hr>
            <div class="text-end">
                <a href="{% url 'pagina-posicion' %}" class="btn btn-secondary">Cancelar</a>
                <button type="submit" class="btn btn-purple">Guardar</button>
            </div>
        </form>
    </div>
</div>
{% endblock %}
```

### 5. Código para `templates/informacion_empleado/ver_empleado.html`

Esta es la página de "Ver Detalle" para un empleado específico, mostrando su información en modo de solo lectura.

```html
{% extends 'informacion_empleado/base.html' %}
{% block title %}Detalle del Empleado{% endblock %}

{% block content %}
<div class="card">
    <div class="card-header card-header-custom">
        <h5 class="mb-0">Detalles de: {{ empleado.nombre_completo }}</h5>
    </div>
    <div class="card-body">
        <div class="container-fluid">
            <div class="row">
                <div class="col-md-6">
                    <p><strong>Código de Empleado:</strong></p>
                    <p>{{ empleado.codigo }}</p>
                </div>
                <div class="col-md-6">
                    <p><strong>Nombre Completo:</strong></p>
                    <p>{{ empleado.nombre_completo }}</p>
                </div>
            </div>
            <hr>
            <div class="row">
                <div class="col-md-6">
                    <p><strong>Email:</strong></p>
                    <p>{{ empleado.email }}</p>
                </div>
                <div class="col-md-6">
                    <p><strong>Contacto:</strong></p>
                    <p>{{ empleado.contacto }}</p>
                </div>
            </div>
            <hr>
            <div class="row">
                <div class="col-md-6">
                    <p><strong>Departamento:</strong></p>
                    <p>{{ empleado.departamento.nombre }}</p>
                </div>
                <div class="col-md-6">
                    <p><strong>Posición:</strong></p>
                    <p>{{ empleado.posicion.nombre }}</p>
                </div>
            </div>
            <hr>
            <div class="row">
                <div class="col-md-4">
                    <p><strong>Fecha de Contratación:</strong></p>
                    <p>{{ empleado.fecha_contratacion|date:"d M, Y" }}</p>
                </div>
                <div class="col-md-4">
                    <p><strong>Salario:</strong></p>
                    <p>${{ empleado.salario|floatformat:2 }}</p>
                </div>
                <div class="col-md-4">
                    <p><strong>Estado:</strong></p>
                    <p>
                        {% if empleado.estado == 1 %}
                            <span class="badge" style="background-color: #6a1b9a;">Activo</span>
                        {% else %}
                            <span class="badge bg-danger">Inactivo</span>
                        {% endif %}
                    </p>
                </div>
            </div>
        </div>
    </div>
    <div class="card-footer text-end">
        <a href="{% url 'pagina-empleado' %}" class="btn btn-secondary">
            <i class="fa fa-arrow-left"></i> Volver a la lista
        </a>
    </div>
</div>
{% endblock %}
```

### Puntos Clave a Notar:

*   **Consistencia:** Todas las plantillas heredan de `base.html`, manteniendo un diseño uniforme.
*   **URLs Dinámicas:** Se usa `{% url 'nombre-de-la-url' %}` para generar los enlaces, lo cual es la práctica recomendada en Django.
*   **Seguridad en Eliminación:** Los botones de "Eliminar" están dentro de un `<form>` que usa el método `POST`. Esto previene la eliminación accidental de datos a través de un simple clic o por rastreadores web.
*   **Formularios de Edición/Creación:** Los formularios `gestionar_*` funcionan tanto para crear nuevos registros como para editar existentes. La lógica `{% if departamento %}` o `{% if posicion %}` se encarga de cambiar el título y rellenar los campos con los datos existentes.
*   **Iconos:** Se utilizan clases de Font Awesome (`fa fa-plus`, `fa fa-edit`, `fa fa-trash`) para los botones, tal como se ve en las imágenes.

Con estos archivos, tu proyecto ahora tendrá el CRUD completo y funcional para las tres entidades (Departamentos, Posiciones y Empleados).
