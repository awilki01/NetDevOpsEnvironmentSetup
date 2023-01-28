FROM ubuntu:22.04

# Make all package installs completely noninteractive - had issues with ansible install without doing this
ENV DEBIAN_FRONTEND=noninteractive

# Create a privilege separation directory for SSHD services
RUN mkdir -p /var/run/sshd

# Update packages
RUN apt -y update

# Add Ansible repository - software-properties-common needed for 'apt-add-repository' command
RUN apt install -y software-properties-common
RUN apt-add-repository -y ppa:ansible/ansible

# Update and upgrade packages - needed again after updating repo above
RUN apt -y update --fix-missing && apt -y upgrade

# Install packages
RUN apt install -y openjdk-11-jdk
# Not sure I need following line. Ran into issues and this fixed it I believe.
RUN apt -y update --fix-missing && apt -y upgrade
RUN apt install -y openssh-server
RUN apt install -y python-is-python3
RUN apt install -y python3-pip
RUN apt install -y ansible
RUN apt install -y git

# Generate keys
RUN ssh-keygen -A

# Add ‘sshd_config’ to ‘/etc/ssh/sshd_config’
# ADD ./sshd_config /etc/ssh/sshd_config

# Change root password
RUN echo root:password123 | chpasswd

# Allow root login
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Allow remote SSH logins
# RUN echo "ALL" >> /etc/hosts.allow

# Start SSH services
CMD ["/usr/sbin/sshd", "-D"]

# Create new jenkins user and update password
RUN useradd -ms /bin/bash jenkins     
RUN echo jenkins:password123 | chpasswd

#USER jenkins

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
#RUN pip install --no-input pyflakes
#RUN pip install --no-input pycodestyle
#RUN pip install --no-input pydocstyle
#RUN pip install --no-input bandit
#RUN pip install --no-input pylint
#RUN pip install --no-input pyyaml
#RUN pip install --no-input mypy
#RUN pip install --no-input mccabe
#RUN pip install --no-input radon
#RUN pip install --no-input black
#RUN pip install --no-input isort
#RUN pip install --no-input yamllint
#RUN pip install --no-input bandit
#RUN pip install --no-input netmiko
#RUN pip install --no-input napalm
#RUN pip install --no-input nornir

USER root




