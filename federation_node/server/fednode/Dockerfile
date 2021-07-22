FROM ubuntu:latest as runner
ENV RSK_VERSION 2.1.0.0-PAPYRUS
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    openjdk-8-jdk \
    supervisor \
    systemd \
    software-properties-common \
    openssh-server
RUN mkdir /usr/share/rsk /etc/rsk /var/lib/rsk /var/log/rsk /var/run/sshd
RUN adduser mngmt
RUN mkdir /home/mngmt/.ssh
RUN echo 'ssh-rsa YOUR-SSH-KEY-HERE' > /home/mngmt/.ssh/authorized_keys
RUN chmod 700 /home/mngmt/.ssh -R \
    && chown mngmt. /home/mngmt/.ssh -R \
    && chmod 600 /home/mngmt/.ssh/authorized_keys
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config
RUN sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PermitEmptyPasswords no/PermitEmptyPasswords no/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
VOLUME /var/log/rsk
VOLUME /var/lib/rsk
VOLUME /etc/rsk
COPY federate-node-$RSK_VERSION-all.jar /usr/share/rsk/rsk.jar
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord"]
