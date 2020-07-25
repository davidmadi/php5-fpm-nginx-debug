# This docker file will install mysql and our database
FROM mysql:5.7
RUN apt-get update -y
ENV MYSQL_DATABASE horserace
ENV MYSQL_ROOT_PASSWORD mysuperpassword

COPY ./sql-scripts/ /docker-entrypoint-initdb.d/