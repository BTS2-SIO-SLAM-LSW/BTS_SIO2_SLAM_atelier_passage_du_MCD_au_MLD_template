# Base : bureau LXDE + VNC + noVNC
FROM dorowu/ubuntu-desktop-lxde-vnc:latest

USER root

# Empêcher les questions interactives pendant apt (tzdata, locales, etc.)
ENV DEBIAN_FRONTEND=noninteractive

# 1️⃣ Préparation APT
# - activer l'archi i386 (pour wine 32 bits)
# - supprimer le dépôt Chrome qui casse apt-get update dans Codespaces
RUN dpkg --add-architecture i386 && \
    rm -f /etc/apt/sources.list.d/*google-chrome*.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        wine \
        wine32 \
        wine64 \
        winbind \
        git \
        curl \
        nano \
        locales && \
    rm -rf /var/lib/apt/lists/*

# 2️⃣ Locale FR (facultatif mais plus confortable pour BTS SIO)
RUN sed -i 's/# fr_FR.UTF-8 UTF-8/fr_FR.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen fr_FR.UTF-8 && \
    update-locale LANG=fr_FR.UTF-8
ENV LANG=fr_FR.UTF-8 \
    LANGUAGE=fr_FR:fr \
    LC_ALL=fr_FR.UTF-8

# 3️⃣ Installer Looping.exe dans l'image
# ⚠ Tu dois déposer le binaire Windows "Looping.exe" dans .devcontainer/ AVANT le build Codespaces
RUN mkdir -p /opt/looping
COPY Looping.exe /opt/looping/Looping.exe

# 4️⃣ Script lanceur (icône bureau va appeler ça)
RUN echo '#!/bin/bash\nwine /opt/looping/Looping.exe &\n' > /usr/local/bin/run-looping && \
    chmod +x /usr/local/bin/run-looping

# 5️⃣ Icône "Looping (Merise)" sur le bureau LXDE
RUN mkdir -p /root/Desktop && \
    echo "[Desktop Entry]\n\
Type=Application\n\
Name=Looping (Merise)\n\
Comment=Modélisation MCD / MLD / SQL\n\
Exec=/usr/local/bin/run-looping\n\
Icon=utilities-terminal\n\
Terminal=false\n" > /root/Desktop/Looping.desktop && \
    chmod +x /root/Desktop/Looping.desktop

# 6️⃣ Exposer noVNC (bureau graphique via navigateur du Codespace)
EXPOSE 80 5900

# On ne touche pas à l'ENTRYPOINT/CMD :
# l'image de base lance supervisord, le serveur VNC et noVNC.
