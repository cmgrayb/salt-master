ARG DEBIAN_CODENAME=bullseye
FROM debian:${DEBIAN_CODENAME}

ARG DEBIAN_MAJOR_VER=11
ARG SALT_MAJOR_VER=3005

# Safely fail in case of failure in piped command
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install latest available version of curl
RUN apt-get update
RUN apt-get install glibc-source curl bsdmainutils dctrl-tools debconf-utils dmidecode net-tools openssh-client -y

RUN mkdir /etc/apt/keyrings
RUN curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring.gpg "https://repo.saltproject.io/salt/py3/debian/${DEBIAN_MAJOR_VER}/amd64/${SALT_MAJOR_VER}/salt-archive-keyring.gpg"
RUN echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring.gpg arch=amd64] https://repo.saltproject.io/salt/py3/debian/${DEBIAN_MAJOR_VER}/amd64/${SALT_MAJOR_VER} bullseye main" | tee /etc/apt/sources.list.d/salt.list

# Second apt-get update required to fetch from repo.saltproject.io
RUN apt-get update -o Dir::Etc::sourcelist=/etc/apt/sources.list.d/salt.list
RUN apt-get install -t "o=SaltProject,a=stable" salt-master salt-minion salt-ssh salt-syndic salt-cloud salt-api -y
COPY salt-services.sh /usr/bin
RUN chmod +x /usr/bin/salt-services.sh

EXPOSE 8000 4505-4506

CMD [ "/usr/bin/salt-services.sh" ]
