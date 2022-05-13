FROM mcr.microsoft.com/windows/servercore:ltsc2019

ENV ssei "https://go.microsoft.com/fwlink/?linkid=866662"

ENV DOTNET_ROOT='C:/Program Files/dotnet'

ENV sa_password="_" \
    attach_dbs="[]" \
    ACCEPT_EULA="_" \
    sa_password_path="C:\ProgramData\Docker\secrets\sa-password"

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]


#COPY SQL2019-SSEI-Dev.exe C:/
RUN Invoke-WebRequest -Uri $env:ssei -OutFile SQL2019-SSEI-Dev.exe

RUN C:/SQL2019-SSEI-Dev.exe /ACTION=Download /MEDIAPATH=C:/ /MEDIATYPE=CAB /QUIET

# make install files accessible
COPY start.ps1 /
#COPY SQLServer2019-DEV-x64-ENU.box /
#COPY SQLServer2019-DEV-x64-ENU.exe /
#COPY SQLServer2019-DEV-x64-ENU /

WORKDIR /

RUN Start-Process -Wait -FilePath .\SQLServer2019-DEV-x64-ENU.exe -ArgumentList /qs, /x:setup ; \
        .\setup\setup.exe /q /ACTION=Install /INSTANCENAME=MSSQLSERVER /FEATURES=SQLEngine /UPDATEENABLED=0 /SQLSVCACCOUNT='NT AUTHORITY\NETWORK SERVICE' /SQLSYSADMINACCOUNTS='BUILTIN\ADMINISTRATORS' /TCPENABLED=1 /NPENABLED=0 /IACCEPTSQLSERVERLICENSETERMS ; \
        Remove-Item -Recurse -Force SQLServer2019-DEV-x64-ENU.exe, SQLServer2019-DEV-x64-ENU.box, SQL2019-SSEI-Dev.exe, setup

RUN stop-service MSSQLSERVER ; \
        set-itemproperty -path 'HKLM:\software\microsoft\microsoft sql server\mssql15.MSSQLSERVER\mssqlserver\supersocketnetlib\tcp\ipall' -name tcpdynamicports -value '' ; \
        set-itemproperty -path 'HKLM:\software\microsoft\microsoft sql server\mssql15.MSSQLSERVER\mssqlserver\supersocketnetlib\tcp\ipall' -name tcpport -value 1433 ; \
        set-itemproperty -path 'HKLM:\software\microsoft\microsoft sql server\mssql15.MSSQLSERVER\mssqlserver\' -name LoginMode -value 2 ;

RUN Invoke-WebRequest 'https://dot.net/v1/dotnet-install.ps1' -o ./dotnet-install.ps1 ; \
        ./dotnet-install.ps1 -InstallDir $env:DOTNET_ROOT ; \
        setx path '%path%;C:/Program Files/dotnet'

HEALTHCHECK CMD [ "sqlcmd", "-Q", "select 1" ]

CMD .\start -sa_password $env:sa_password -ACCEPT_EULA $env:ACCEPT_EULA -attach_dbs \"$env:attach_dbs\" -Verbose


