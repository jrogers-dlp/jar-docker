FROM mcr.microsoft.com/mssql/server:2019-latest
USER root
ENV ACCEPT_EULA="Y"

RUN set -ex \
    && wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && apt-get update \
    && apt-get install -yq \
        software-properties-common dotnet-sdk-6.0 powershell \
    && rm packages-microsoft-prod.deb \
    && export PATH="/opt/mssql-tools/bin:$PATH";
USER mssql