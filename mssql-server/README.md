## MSSQL SERVER Configuration

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

