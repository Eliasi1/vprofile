FROM openjdk:11 AS BUILD_IMAGE 
# we will use APT since openjdk is based on debian
RUN apt update && apt install maven -y
RUN git clone git@github.com:Eliasi1/vprofile.git
RUN cd vprofile-project && git checkout docker && mvn install 

FROM tomcat:9-jrell 
# removing the default artifact
RUN rm -rf /usr/local/tomcat/webapps/*
# copy the artifact created in BUILD_IMAGE
COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
# as instructed in dockerhub image docs
CMD ["catalina.sh","run"]