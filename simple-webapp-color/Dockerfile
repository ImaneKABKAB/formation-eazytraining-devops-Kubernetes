FROM python:2.7-stretch
LABEL maintainer.name(imane) maintainer.email(ikanbbhj@jhh)
RUN apt-get update -y
COPY . .
RUN pip install -r ./requirements.txt
EXPOSE 8080
CMD [ "python", "./app.py" ]

