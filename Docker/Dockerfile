FROM debian:bullseye

RUN apt update
RUN apt install glibc-source curl systemctl -y

RUN mkdir /etc/apt/keyrings
RUN curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring.gpg https://repo.saltproject.io/salt/py3/debian/11/amd64/latest/salt-archive-keyring.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring.gpg arch=amd64] https://repo.saltproject.io/salt/py3/debian/11/amd64/latest bullseye main" | tee /etc/apt/sources.list.d/salt.list

RUN apt-get update
RUN apt-get install salt-master salt-minion salt-ssh salt-syndic salt-cloud salt-api -y
COPY salt-services.sh /usr/bin
RUN chmod +x /usr/bin/salt-services.sh

EXPOSE 8000 4505-4506

CMD [ "/usr/bin/salt-services.sh" ]
