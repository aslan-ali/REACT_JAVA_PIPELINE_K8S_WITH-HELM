FROM realworldio/django-drf
WORKDIR /app/django
ADD . .
RUN python manage.py makemigrations && python manage.py migrate

CMD python manage.py runserver 0.0.0.0:8000
