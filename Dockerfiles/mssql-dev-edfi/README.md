# mssql-dev-edfi

SQL Server 2019 on Ubuntu 20.04 with Ed-Fi pre reqs installed:

* dotnet-sdk-6
* powershell
* mono
* nano *(not required but useful)*
* git *(not required but useful)*

**NOTE:** This container will run as USER mssql. If you need to cli inside of it and need root access, be sure to run as USER root.

``` powershell
docker exec -it -u root CONTAINER_NAME "bash"
```
