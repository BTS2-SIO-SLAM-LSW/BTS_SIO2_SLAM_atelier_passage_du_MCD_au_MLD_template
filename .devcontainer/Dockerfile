FROM dorowu/ubuntu-desktop-lxde-vnc:latest
USER root
RUN dpkg --add-architecture i386 && apt-get update && apt-get install -y --no-install-recommends wine wine32 wine64 winbind git curl nano locales && rm -rf /var/lib/apt/lists/*
RUN sed -i 's/# fr_FR.UTF-8 UTF-8/fr_FR.UTF-8 UTF-8/' /etc/locale.gen && locale-gen fr_FR.UTF-8 && update-locale LANG=fr_FR.UTF-8
ENV LANG=fr_FR.UTF-8 LANGUAGE=fr_FR:fr LC_ALL=fr_FR.UTF-8
RUN mkdir -p /opt/looping
COPY Looping.exe /opt/looping/Looping.exe
RUN echo '#!/bin/bash\nwine /opt/looping/Looping.exe &\n' > /usr/local/bin/run-looping && chmod +x /usr/local/bin/run-looping
RUN mkdir -p /root/Desktop && echo "[Desktop Entry]\nType=Application\nName=Looping (Merise)\nComment=Modélisation MCD / MLD / SQL\nExec=/usr/local/bin/run-looping\nIcon=utilities-terminal\nTerminal=false\n" > /root/Desktop/Looping.desktop && chmod +x /root/Desktop/Looping.desktop
EXPOSE 6080
