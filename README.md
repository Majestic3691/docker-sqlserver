# docker-sqlserver

__NOTE: Powershell ISE cannot connect to containers use non-ISE Powershell session__

## Set environment for sqldbVM
 PS C:\projects\aws\github\sqlserver> docker-machine env sqldbVM
$Env:DOCKER_TLS_VERIFY = "1"
$Env:DOCKER_HOST = "tcp://192.168.1.195:2376"
$Env:DOCKER_CERT_PATH = "C:\Users\Michael\.docker\machine\machines\sqldbVM"
$Env:DOCKER_MACHINE_NAME = "sqldbVM"
$Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
# Run this command to configure your shell:
# & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env sqldbVM | Invoke-Expression
 

 PS C:\projects\aws\github\sqlserver> docker-machine inspect sqldbVM
{
    "ConfigVersion": 3,
    "Driver": {
        "IPAddress": "192.168.1.196",
        "MachineName": "sqldbVM",
        "SSHUser": "docker",
        "SSHPort": 22,
        "SSHKeyPath": "C:\\Users\\Michael\\.docker\\machine\\machines\\sqldbVM\\id_rsa",
        "StorePath": "C:\\Users\\Michael\\.docker\\machine",
        "SwarmMaster": false,
        "SwarmHost": "tcp://0.0.0.0:3376",
        "SwarmDiscovery": "",
        "Boot2DockerURL": "",
        "VSwitch": "Majestic LAN Virtual Switch",
        "DiskSize": 20,
        "MemSize": 4096,
        "CPU": 2,
        "MacAddr": "",
        "VLanID": 0
    },
    "DriverName": "hyperv",
    "HostOptions": {
        "Driver": "",
        "Memory": 0,
        "Disk": 0,
        "EngineOptions": {
            "ArbitraryFlags": [],
            "Dns": null,
            "GraphDir": "",
            "Env": [],
            "Ipv6": false,
            "InsecureRegistry": [],
            "Labels": [],
            "LogLevel": "",
            "StorageDriver": "",
            "SelinuxEnabled": false,
            "TlsVerify": true,
            "RegistryMirror": [],
            "InstallURL": "https://get.docker.com";
        },
        "SwarmOptions": {
            "IsSwarm": false,
            "Address": "",
            "Discovery": "",
            "Agent": false,
            "Master": false,
            "Host": "tcp://0.0.0.0:3376",
            "Image": "swarm:latest",
            "Strategy": "spread",
            "Heartbeat": 0,
            "Overcommit": 0,
            "ArbitraryFlags": [],
            "ArbitraryJoinFlags": [],
            "Env": null,
            "IsExperimental": false
        },
        "AuthOptions": {
            "CertDir": "C:\\Users\\Michael\\.docker\\machine\\certs",
            "CaCertPath": "C:\\Users\\Michael\\.docker\\machine\\certs\\ca.pem",
            "CaPrivateKeyPath": "C:\\Users\\Michael\\.docker\\machine\\certs\\ca-key.pem",
            "CaCertRemotePath": "",
            "ServerCertPath": "C:\\Users\\Michael\\.docker\\machine\\machines\\sqldbVM\\server.pem",
            "ServerKeyPath": "C:\\Users\\Michael\\.docker\\machine\\machines\\sqldbVM\\server-key.pem",
            "ClientKeyPath": "C:\\Users\\Michael\\.docker\\machine\\certs\\key.pem",
            "ServerCertRemotePath": "",
            "ServerKeyRemotePath": "",
            "ClientCertPath": "C:\\Users\\Michael\\.docker\\machine\\certs\\cert.pem",
            "ServerCertSANs": [],
            "StorePath": "C:\\Users\\Michael\\.docker\\machine\\machines\\sqldbVM"
        }
    },
    "Name": "sqldbVM"
}
 
 
Container -connect to bash shell
PS docker container exec -t -i 7cc69388ef9e /bin/bash


Set environment for the machine
PS C:\projects\aws\github\sqlserver> docker-machine env sqldbVM
$Env:DOCKER_TLS_VERIFY = "1"
$Env:DOCKER_HOST = "tcp://192.168.1.196:2376"
$Env:DOCKER_CERT_PATH = "C:\Users\Michael\.docker\machine\machines\sqldbVM"
$Env:DOCKER_MACHINE_NAME = "sqldbVM"
$Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
# Run this command to configure your shell:
# & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env sqldbVM | Invoke-Expression

 PS C:\projects\aws\github\sqlserver> & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env sqldbVM | Invoke-Expression

Install sql server 2017 container
 PS C:\projects\aws\github\sqlserver> docker pull microsoft/mssql-server-linux:2017-latest
2017-latest: Pulling from microsoft/mssql-server-linux
f6fa9a861b90: Pulling fs layer
da7318603015: Pulling fs layer
6a8bd10c9278: Pulling fs layer
d5a40291440f: Pulling fs layer
bbdd8a83c0f1: Pulling fs layer
3a52205d40a6: Pulling fs layer
6192691706e8: Pulling fs layer
1a658a9035fb: Pulling fs layer
827975ce0c02: Pulling fs layer
ea7199b7fbb6: Pulling fs layer
d5a40291440f: Waiting
bbdd8a83c0f1: Waiting
3a52205d40a6: Waiting
6192691706e8: Waiting
ea7199b7fbb6: Waiting
1a658a9035fb: Waiting
827975ce0c02: Waiting
da7318603015: Verifying Checksum
da7318603015: Download complete
6a8bd10c9278: Verifying Checksum
6a8bd10c9278: Download complete
bbdd8a83c0f1: Verifying Checksum
bbdd8a83c0f1: Download complete
d5a40291440f: Download complete
3a52205d40a6: Verifying Checksum
3a52205d40a6: Download complete
1a658a9035fb: Verifying Checksum
1a658a9035fb: Download complete
6192691706e8: Verifying Checksum
6192691706e8: Download complete
f6fa9a861b90: Download complete
f6fa9a861b90: Pull complete
da7318603015: Pull complete
6a8bd10c9278: Pull complete
d5a40291440f: Pull complete
bbdd8a83c0f1: Pull complete
3a52205d40a6: Pull complete
6192691706e8: Pull complete
1a658a9035fb: Pull complete
ea7199b7fbb6: Verifying Checksum
ea7199b7fbb6: Download complete
827975ce0c02: Verifying Checksum
827975ce0c02: Download complete
827975ce0c02: Pull complete
ea7199b7fbb6: Pull complete
Digest: sha256:fa1265cae3447320542cd0d1da160988aeffe5d2ae602a84336ddbd069e2b4ee
Status: Downloaded newer image for microsoft/mssql-server-linux:2017-latest

Start the container image
 PS C:\projects\aws\github\sqlserver> docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=hpy6DX6B" `
   -p 1401:1433 --name sql1 `
   -d microsoft/mssql-server-linux:2017-latest

834402886b5ff926930fa467485acbae131eb1b538b25ff798002b985422a1a4

Verify container execution
 PS C:\projects\aws\github\sqlserver> docker ps -a
CONTAINER ID        IMAGE                                      COMMAND                  CREATED              STATUS              PORTS                    NAMES
834402886b5f        microsoft/mssql-server-linux:2017-latest   "/bin/sh -c /opt/mssâ€¦"   About a minute ago   Up About a minute   0.0.0.0:1401->1433/tcp   sql1
 

Changes the internal name of the container to a custom value
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')

https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker

Connect to sql server
PS docker exec -it sql1 "bash"
root@834402886b5f:/#

PS C:\projects\aws\github\sqlserver> docker container exec -t -i 834402886b5f /bin/bash
root@834402886b5f:/#



Connect from outside the container
## Port 1401 is the port mapped from container to the outside - inside container 1403 - outside container 1401
PS C:\Users\Michael> sqlcmd -S 192.168.1.196,1401 -U SA -P 'hpy6DX6B'
1> select name from sys.Databases
2> go
name                                                                                                                    
--------------------------------------------------------------------------------------------------------------------------------
master                                                                                                                  
tempdb                                                                                                                  
model                                                                                                                   
msdb                                                                                                                    
TestDB                                                                                                                  
(5 rows affected)



Change the SA password
PS docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U SA -P "<YourStrong!Passw0rd>" `
   -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong!Passw0rd>'"

Create a test db
root@834402886b5f:/# /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'hpy6DX6B'
1> Create Database TestDB
2> select name from sys.Databases
3> go
name                                                                                                                    
--------------------------------------------------------------------------------------------------------------------------------
master                                                                                                                  
tempdb                                                                                                                  
model                                                                                                                   
msdb                                                                                                                    
TestDB                                                                                                                  
(5 rows affected)



INSTALL AND CONFIGURE - https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker

TROUBLESHOOTING - https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker#troubleshooting




