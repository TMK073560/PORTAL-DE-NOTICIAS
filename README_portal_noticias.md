# Portal de Noticias — Control de acceso

Este proyecto representa un **portal de noticias desarrollado con Django 5 y SQLite**, pensado con una separación clara de permisos entre visitantes, usuarios comunes y personal autorizado.

## Roles del sistema

### Guest / Visitante

Un visitante que no ha iniciado sesión puede:

- Ver las noticias publicadas.
- Entrar al detalle de una noticia.
- Registrarse.
- Iniciar sesión.

Un visitante **no puede**:

- Crear noticias.
- Editar noticias.
- Eliminar noticias.
- Entrar al panel de administración.

---

### Usuario común

Un usuario registrado y autenticado puede:

- Ver las noticias publicadas.
- Acceder al contenido disponible para usuarios normales.
- Cerrar sesión de forma segura.

Un usuario común **no puede publicar, editar ni eliminar noticias**.

Esto evita que cualquier persona que cree una cuenta tenga acceso a funciones editoriales.

---

### Moderador / Editor

En este proyecto, un moderador o editor se representa mediante un usuario con permiso de **staff**.

Un usuario staff puede:

- Publicar noticias.
- Acceder a funciones editoriales.
- Entrar al panel administrativo si tiene los permisos correspondientes.

Además, para editar o eliminar una noticia desde el portal, el sistema verifica que el usuario sea también el **autor de esa noticia**.

Esto evita que un moderador modifique accidentalmente contenido creado por otro miembro del equipo.

---

### Administrador

Un administrador normalmente corresponde a un **superusuario de Django**.

Puede:

- Administrar usuarios.
- Asignar o quitar permisos.
- Convertir usuarios autorizados en staff.
- Administrar contenido desde `/admin/`.
- Supervisar el portal completo.

El administrador debe ser una cuenta reservada para personal de confianza.

---

## ¿Por qué se separan estos permisos?

En un portal de noticias serio, **registrarse no debe convertir automáticamente a una persona en periodista, editor o moderador**.

Permitir que cualquier usuario registrado publique contenido podría provocar:

- Noticias falsas.
- Spam.
- Eliminación o modificación de publicaciones.
- Suplantación de periodistas.
- Contenido ofensivo o malicioso.
- Daño reputacional al medio.

Por eso se aplica el principio de **mínimo privilegio**: cada usuario recibe únicamente los permisos que necesita.

## Flujo recomendado en un entorno real

1. Una persona visita el portal como guest.
2. Puede leer las noticias públicas.
3. Si crea una cuenta, sigue siendo un usuario común.
4. Un administrador revisa manualmente si esa persona debe formar parte del equipo editorial.
5. Solo entonces se le asigna el rol de staff/moderador.
6. Los administradores mantienen el control de permisos y usuarios.

## Resumen de permisos

| Acción | Guest | Usuario común | Moderador / Staff | Administrador |
|---|---:|---:|---:|---:|
| Ver noticias publicadas | Sí | Sí | Sí | Sí |
| Registrarse | Sí | — | — | — |
| Iniciar sesión | Sí | Sí | Sí | Sí |
| Publicar noticias | No | No | Sí | Sí |
| Editar noticias propias | No | No | Sí | Sí |
| Eliminar noticias propias | No | No | Sí | Sí |
| Administrar usuarios | No | No | No | Sí |
| Asignar permisos | No | No | No | Sí |
| Acceder al panel admin | No | No | Según permisos | Sí |

## Seguridad aplicada

El proyecto utiliza mecanismos propios de Django:

- `LoginRequiredMixin` para exigir sesión.
- `UserPassesTestMixin` para comprobar permisos.
- `request.user` para asignar automáticamente el autor.
- `{% csrf_token %}` en formularios POST.
- `UserCreationForm` para crear usuarios.
- `django.contrib.auth.urls` para login/logout.
- Separación entre usuarios normales y usuarios staff.

En un sistema real también sería recomendable usar HTTPS, contraseñas seguras, permisos específicos por grupo, copias de seguridad, auditoría de cambios y configuración de producción con `DEBUG=False`.

---

## Idea principal

**Leer noticias es una función pública. Publicar noticias es una función editorial. Administrar el sistema es una función administrativa.**

Estas responsabilidades deben mantenerse separadas para evitar que un usuario común pueda modificar el contenido oficial del portal.
