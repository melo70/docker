## Indice

1. [dotnet-sdk](/dotnet-sdk/README.md)
3. [mssql-server](/mssql-server/READEME.md)
4. [Ottimizzazione della Rappresentazione](#3-ottimizzazione-della-rappresentazione)
5. [Funzioni di Utilità](#4-funzioni-di-utilità)
   - 4.1 [PS_len: Lunghezza in Tempo Costante](#41-ps_len-lunghezza-in-tempo-costante)
   - 4.2 [PS_free: Gestione della Memoria](#42-ps_free-gestione-della-memoria)
6. [Problemi di Gestione della Memoria](#5-problemi-di-gestione-della-memoria)
7. [Reference Counting](#6-reference-counting)
8. [Verso le Strutture](#7-verso-le-strutture)


Per creare un container eseguire i seguenti comandi di esempio:

```bash
docker build -t dotnet-dev .
```
Avendo preparati diversi dockerfile per ognuna delle versioni di dotnet si potrebbe procedere in questo modo:

```bash
docker build -f Dockerfile.dotnet.sdk.6.0 -t dotnet-dev6 .
docker build -f Dockerfile.dotnet.sdk.7.0 -t dotnet-dev7 .
docker build -f Dockerfile.dotnet.sdk.8.0 -t dotnet-dev8 .
docker build -f Dockerfile.dotnet.sdk.8.0 -t dotnet-dev8 .
```

Per avviare il container usare il seguente comando:

```bash
docker run -it --rm -v ${PWD}:/app -p 5000:5000 -p 5001:5001 dotnet-dev
```
Per installare il repostitory sotto /etc/apt usare i seguenti comandi:

```bash
wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb

dpkg -i packages-microsoft-prod.deb 

apt update

apt search dotnet-sdk
```

ed installare la versione di dotnet interessata

```bash
apt search dotnet-sdk
```
Ora supponendo di aver tutte le versioni disponibili di dotnet-sdk all'interno del container, per creare applicazioni che si basano su framework differenti si dovrebbero usare i seguenti comandi:


# COMANDI PER GESTIRE MULTIPLE VERSIONI .NET

1. Verificare tutte le versioni installate
```
dotnet --list-sdks
```
2. Creare applicazioni con versioni specifiche:

# Creare app con .NET 6.0
```
dotnet new console -n MiaApp60 -f net6.0
```

# Creare app con .NET 7.0  
```
dotnet new console -n MiaApp70 -f net7.0
```
# Creare app con .NET 8.0
```
dotnet new console -n MiaApp80 -f net8.0
```

3. Usare global.json per forzare una versione specifica in una directory
```
echo '{
  "sdk": {
    "version": "6.0.425"
  }
}' > global.json
```
4. Verificare quale versione verrà usata
```
dotnet --version
```
# COMANDI DOCKER BASE

# Costruire l'immagine Docker
```
docker build -t dotnet-dev-debian .
```
# Eseguire il container in modalità interattiva
```
docker run -it --rm -p 5000:5000 -p 5001:5001 dotnet-dev
```
# Eseguire il container con un volume persistente per i progetti
```
docker run -it --rm -v C:\percorso\ai\tuoi\progetti:/app -p 5000:5000 -p 5001:5001 dotnet-dev
```
# Eseguire il container in background
```
docker run -d --name dotnet-container -v ${PWD}:/app -p 5000:5000 -p 5001:5001 dotnet-dev
```
# Entrare nel container se è in esecuzione in background
```
docker exec -it dotnet-container bash
```
# Fermare e rimuovere il container
```
docker stop dotnet-container
docker rm dotnet-container
```


# 2. MSSQL SERVER
Official images for Microsoft SQL Server based on Ubuntu

Featured tags
2025-latest (SQL Server 2025 Preview) docker pull mcr.microsoft.com/mssql/server:2025-latest

2022-latest docker pull mcr.microsoft.com/mssql/server:2022-latest

2019-latest docker pull mcr.microsoft.com/mssql/server:2019-latest

2017-latest docker pull mcr.microsoft.com/mssql/server:2017-latest

Note: Starting with SQL Server 2022 (16.x) CU 14 and SQL Server 2019 (15.x) CU 28, container images include the mssql-tools18 package. The previous directory /opt/mssql-tools/bin is being phased out. The directory for Microsoft ODBC 18 tools is /opt/mssql-tools18/bin, aligning with the latest tools offering. For more information about changes and security enhancements, see ODBC Driver 18.0 for SQL Server Released - Microsoft Community Hub . ODBC driver version 18 is designed with an encryption-first approach, ensuring that utilities like sqlcmd and bcp, that use the Microsoft ODBC driver, operate under the secure by default principle. Users who wish to disable encryption must do so explicitly.

For example, when you try to connect using the sqlcmd tool, the -N option is available with [s|m|o] parameters, where s stands for strict, m for mandatory, and o for optional. The default setting is mandatory. For more information, see sqlcmd utility . To connect without encryption, the sample command is:

sqlcmd -S <ip address,port> -U <login_name> -P <password> -No
Related repositories
The program for SQL Server Windows Container was suspended in 2021. For more information, see Update on Beta program for SQL Server on Windows Container . SQL Server Windows Container is not supported for production workloads. For development purposes, if you still need references for a custom SQL Server container image on Windows, provided as-is and not supported, see SQL Server Windows Containers sample .

About this image
Official container images for Microsoft SQL Server on Linux for Docker Engine.

How to use this image
You can get started with SQL Server 2025 Preview on Ubuntu 22.04 which is currently in preview. To deploy a container with SQL Server 2025 Preview based on Ubuntu 22.04, use the following command:

docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<password>" -e "MSSQL_PID=Evaluation" -p 1433:1433  --name sqlpreview --hostname sqlpreview -d mcr.microsoft.com/mssql/server:2025-latest
Start a mssql-server instance for SQL Server 2022 (16.x). These images are based on Ubuntu 22.04, and are fully supported for production workloads.

docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<password>" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
Start a mssql-server instance using a CU tag. In this example, we use CU 16 for SQL Server 2022.

Important: If you use PowerShell on Windows to run these commands, use double quotes instead of single quotes.

docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<password>" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-CU16-ubuntu-22.04
Start a mssql-server instance using the latest update for SQL Server 2022:

docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<password>" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
Start a mssql-server instance running as SQL Server Express edition:

docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<password>" -e "MSSQL_PID=Express" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest 
Connect to Microsoft SQL Server

You can connect to the SQL Server instance using the sqlcmd tool inside of the container, with the following command on the host:

docker exec -it <container_id|container_name> /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P <password>
You can also use the tools in an entrypoint.sh script to do things like create databases or logins, attach databases, import data, or other setup tasks. See this example of using an entrypoint.sh and setup.sql scripts to create a database .

You can connect to the SQL Server instance in the container from outside the container by using various command line and GUI tools on the host or remote computers. See Install the sqlcmd and bcp SQL Server command-line tools on Linux in the SQL Server on Linux documentation.

Configuration
Requirements
This image requires Docker Engine 1.8+ in any of their supported platforms .

At least 2GB of RAM (3.25 GB before SQL Server 2017 CU2). Make sure to assign enough memory to the Docker container if you're running on Docker for macOS or Windows .

Requires the following environment flags

"ACCEPT_EULA=Y"
"MSSQL_SA_PASSWORD=<password>"
"MSSQL_PID=<your_product_id | edition_name> (default: Developer)"
A strong system administrator (SA) password, which must be at least eight (8) characters in length, including uppercase, lowercase letters, base-10 digits and/or non-alphanumeric symbols.

Environment variables
You can use environment variables to configure SQL Server on Linux containers.

ACCEPT_EULA confirms your acceptance of the End-User Licensing Agreement .

MSSQL_SA_PASSWORD is the database system administrator (userid = 'sa') password used to connect to SQL Server once the container is running. Important note: This password needs to include at least 8 characters of at least three of these four categories: uppercase letters, lowercase letters, numbers and non-alphanumeric symbols.

MSSQL_PID is the Product ID (PID) or Edition that the container runs with. Acceptable values for SQL Server 2025 Preview:

Evaluation (free, no production use rights, 180-day limit)
Enterprise Developer (free, no production use rights)
Standard Developer (free, no production use rights)
Express (free)
Web (PAID)
Standard (PAID)
Enterprise (PAID) - CPU core utilization restricted to 20 physical/40 hyperthreaded
Enterprise Core (PAID) - CPU core utilization up to Operating System Maximum
I bought a license through a retail sales channel and have a product key to enter
Standard (Billed through Azure) - Use pay-as-you-go billing through Azure
Enterprise Core (Billed through Azure) - Use pay-as-you-go billing through Azure
<valid product id>: This option runs the container with the edition that is associated with the PID

For a complete list of environment variables that can be used, and for earlier versions of SQL Server, refer to the documentation .

