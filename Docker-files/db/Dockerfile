FROM mysql:8.0.33
LABEL "Project"="Vprofile"
LABEL "Author"="Imran"

ENV MYSQL_ROOT_PASSWORD="vprodbpass"
# according to docs, when db image will be built MYSQL_DATABASE will be created
ENV MYSQL_DATABASE="accounts"
# put the script in the right path so it will be running when image will be build
ADD db_backup.sql docker-entrypoint-initdb.d/db_backup.sql
# we dont need to expoe port 3306, since its being taken by the image by default.