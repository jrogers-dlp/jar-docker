# jar-docker
A collection of useful Dockerfiles and scripts I use often.

## Example use: Ed-Fi initdev on SQL Server
This is an example of how to build a linux SQL Server container, start it, and then run Ed-Fi initdev inside of it.

* Build the image and run the container

```powershell
cd ./Dockerfiles/mssql-dev-edfi
docker build -t mssql-dev-edfi .
docker run --name "sql-edfi-test" -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=yourStrong(!)Password" -p 1433:1433 -d mssql-dev-edfi
```

* Change to your Ed-Fi base directory and copy the ODS and Implementation directories to the container

```powershell
cd "C:\git-home\Ed-Fi-Alliance-OSS"
docker cp .\Ed-Fi-ODS-Implementation\ sql-edfi-test:/mnt/Ed-Fi-ODS-Implementation/
docker cp .\Ed-Fi-ODS\ sql-edfi-test:/mnt/Ed-Fi-ODS/
```

* Connect to the container shell as root and run initdev in powershell

```powershell
docker exec -it -u root sql-edfi-test "bash"
pwsh
cd /mnt/Ed-Fi-ODS-Implementation
./InitializePowershellForDevelopment.ps1
initdev
```
