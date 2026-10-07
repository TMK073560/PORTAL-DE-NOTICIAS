PORTAL DE NOTICIAS - DJANGO 5

Este proyecto usa SOLO Django y SQLite.

Después de descomprimir:

python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver

PERMISOS:
- Visitante: puede ver noticias publicadas.
- Usuario registrado normal: puede iniciar sesión y ver noticias.
- Usuario staff: puede publicar noticias.
- Editar y borrar: solo si además de ser staff, es autor de la noticia.

El autor se asigna automáticamente con request.user.
No se incluye autor ni creada en el formulario.
La URL de la imagen debe comenzar con https://.
