FROM mcr.microsoft.com/mssql/server:2019-latest
USER root
ENV ACCEPT_EULA="Y"

RUN set -ex && \
    wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O /tmp/packages-microsoft-prod.deb && \
    dpkg -i /tmp/packages-microsoft-prod.deb && \
    rm /tmp/packages-microsoft-prod.deb && \
    apt-get update && \
    apt-get install -y apt-transport-https software-properties-common dirmngr gnupg ca-certificates nuget && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    apt-add-repository 'deb https://download.mono-project.com/repo/ubuntu stable-focal main' && \
    apt-get update && \
    apt-get install -y dotnet-sdk-6.0 powershell nano git && \
    apt-get install mono-complete -yq && \
    export PATH="/opt/mssql-tools/bin:$PATH";
USER mssql