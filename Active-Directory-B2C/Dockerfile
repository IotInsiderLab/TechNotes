FROM python:2

WORKDIR /usr/src

RUN apt-get update
RUN apt-get install nano

RUN pip install Flask
RUN pip install flask-oauthlib
RUN pip install python-jose

RUN git clone https://github.com/Azure-Samples/active-directory-python-flask-web-b2c.git app

WORKDIR app

EXPOSE 5000
ENV FLASK_APP=b2cflaskapp.py

CMD [ "flask", "run", "--host=0.0.0.0" ]
