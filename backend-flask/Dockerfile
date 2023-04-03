# this is the previous Docekr file without multistaging 
#FROM python:3.10-slim-buster
FROM 533378368499.dkr.ecr.ca-central-1.amazonaws.com/cruddur-python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
#CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]
RUN chmod +x start_flask.sh
COPY start_flask.sh /backend-flask
RUN chmod +x /backend-flask/start_flask.sh
ENTRYPOINT ["/backend-flask/start_flask.sh"]