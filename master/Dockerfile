FROM jenkins/jenkins:2.228

USER root
#Group Id can remove
#RUN groupadd -g 993 docker
RUN groupadd docker
RUN apt-get update && \
    apt-get -y install --no-install-recommends apt-transport-https \
      ca-certificates \
      curl \
      gnupg2 \
      software-properties-common && \
      rm -rf /var/lib/apt/lists/* 

RUN curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
    add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
      $(lsb_release -cs) \
      stable"

RUN apt-get update && \
    apt-get -y install --no-install-recommends docker-ce && \
    rm -rf /var/lib/apt/lists/*

RUN usermod -a -G docker jenkins 
USER jenkins
COPY plugins_list.txt /usr/share/jenkins/ref/plugins_list.txt
ENV JENKINS_HOME /var/jenkins_home
ARG JAVA_OPTS
ENV JAVA_OPTS "-Djenkins.install.runSetupWizard=false ${JAVA_OPTS:-}"
RUN xargs /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins_list.txt

