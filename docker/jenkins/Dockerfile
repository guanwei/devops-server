FROM jenkins
MAINTAINER Edward Guan <285006386@qq.com>

USER root
RUN mkdir -p /var/log/jenkins && chown -R jenkins:jenkins /var/log/jenkins
RUN mkdir -p /var/cache/jenkins && chown -R jenkins:jenkins /var/cache/jenkins
USER jenkins

ENV JENKINS_OPTS="--logfile=/var/log/jenkins/jenkins.log --webroot=/var/cache/jenkins/war --prefix=/jenkins"

#COPY executors.groovy /usr/share/jenkins/ref/init.groovy.d/executors.groovy

#COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
#RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
