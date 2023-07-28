FROM jenkins/jenkins:2.401.3-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli

# Install necessary packages
RUN apt-get update -y && apt-get install -y \
    unzip  \
    wget \
    gnupg2 \
    software-properties-common \
    unzip  \
    vim


# # Install apt
# RUN \
# # Update
# apt-get update -y && \
# # Install Unzip
# apt-get install unzip -y && \
# # need wget
# apt-get install wget -y && \
# # vim
# apt-get install vim -y

################################
# Install Terraform
################################

# Download terraform for linux
RUN wget https://releases.hashicorp.com/terraform/1.5.4/terraform_1.5.4_linux_arm64.zip

# Unzip
RUN unzip terraform_1.5.4_linux_arm64.zip

# Move to local bin
RUN mv terraform /usr/local/bin/
# Check that it's installed
RUN terraform --version 

################################
# Install python
################################

RUN apt-get install -y python3-pip
#RUN ln -s /usr/bin/python3 python
RUN pip3 install --upgrade pip
RUN python3 -V
RUN pip --version

################################
# Install AWS CLI
################################
RUN pip install awscli --upgrade --user

# add aws cli location to path
ENV PATH=~/.local/bin:$PATH

# Adds local templates directory and contents in /usr/local/terrafrom-templates
# ADD templates /usr/local/bin/templates

RUN mkdir ~/.aws && touch ~/.aws/credentials

################################
# Install Ansible
################################

RUN apt-get install -y ansible


USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"

USER root
# Clean up
RUN rm -rf terraform_*.zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
