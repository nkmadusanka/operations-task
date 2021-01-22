FROM debian:stable

RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y python-pip

WORKDIR /app
COPY . .
RUN pip install -U gunicorn && pip install -Ur requirements.txt

CMD gunicorn -b :3000 wsgi
