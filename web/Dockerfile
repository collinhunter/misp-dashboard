FROM ubuntu:xenial

# Install core components
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get dist-upgrade -y && apt-get autoremove -y && apt-get clean
RUN apt-get install -y sudo git python3 python3-virtualenv apt-utils wget redis-server
RUN wget https://bootstrap.pypa.io/get-pip.py; python3 get-pip.py

RUN pip3 install flask flask_login redis
RUN adduser www-data sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

#ARG MM_LIC_KEY
#MM_LIC_KEY=${MM_LIC_KEY}
# Setup www dir
WORKDIR /var/www
RUN chown www-data:www-data /var/www
USER www-data

#Start installing dashboard
RUN git clone --branch master https://github.com/collinhunter/misp-dashboard.git
#RUN git clone --branch master https://github.com/MISP/misp-dashboard.git
WORKDIR /var/www/misp-dashboard
RUN cd /var/www/misp-dashboard
RUN sudo -H pip3 install -r requirements.txt
RUN VIRTUAL_ENV="TUTU"; sudo -H ./install_dependencies.sh ${MM_LIC_KEY}


#Run misp-dashboard
ADD run.sh /run.sh
RUN sudo chmod +x /run.sh

VOLUME /var/www/misp-dashboard
ENTRYPOINT ["/run.sh"]
