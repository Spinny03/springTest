# Utilizziamo un'immagine base con Java 23
FROM eclipse-temurin:23-jdk

# Installazione dei pacchetti necessari per VS Code e lo sviluppo
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends gradle maven curl wget jq git \
    fontconfig libfreetype6 libxrender1 libxtst6 libxi6 sudo \
    procps lsof ca-certificates zip unzip

# Set the JAVA_HOME environment variable
ENV JAVA_HOME /opt/java/openjdk

# Clean up
RUN apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Set up Spring Boot CLI (optional)
RUN curl -s "https://get.sdkman.io" | bash \
    && bash -c "source $HOME/.sdkman/bin/sdkman-init.sh && sdk install springboot"

# Creiamo l'utente vscode con controllo se esiste già il gruppo o l'utente
RUN if getent group 1000 > /dev/null; then \
        groupmod -n vscode $(getent group 1000 | cut -d: -f1); \
    else \
        groupadd --gid 1000 vscode; \
    fi \
    && if id -u 1000 > /dev/null 2>&1; then \
        usermod -l vscode -g 1000 -s /bin/bash -d /home/vscode -m $(id -un 1000); \
    else \
        useradd -s /bin/bash --uid 1000 --gid 1000 -m vscode; \
    fi \
    && echo vscode ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/vscode \
    && chmod 0440 /etc/sudoers.d/vscode
