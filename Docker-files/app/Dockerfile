FROM openjdk:11 AS BUILD_IMAGE 
# we will use APT since openjdk is based on debian
RUN apt update && apt install maven -y
# Install Git and SSH client
RUN apt-get update && apt-get install -y git openssh-client

# Copy your SSH private key to the container
COPY id_rsa /root/.ssh/id_rsa

# Set the permissions of the private key
RUN chmod 600 /root/.ssh/id_rsa

# Start the SSH agent and add the private key
RUN eval $(ssh-agent -s) && ssh-add /root/.ssh/id_rsa && ssh-keyscan github.com >> /root/.ssh/known_hosts
# finally clone the git
RUN git clone git@github.com:Eliasi1/vprofile.git

RUN cd vprofile && git checkout docker && mvn install 

FROM tomcat:9.0-jre11
# removing the default artifact
RUN rm -rf /usr/local/tomcat/webapps/*
# copy the artifact created in BUILD_IMAGE
COPY --from=BUILD_IMAGE vprofile/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
# as instructed in dockerhub image docs
CMD ["catalina.sh","run"]