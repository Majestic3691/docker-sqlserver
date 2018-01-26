# docker-sqlserver
This is the procedure for creating a new docker machine, load a sql server container and connect/verify the installation.

__NOTE: Powershell ISE may have [issues](https://github.com/docker/for-win/issues/223) connecting to containers__
__*USE non-ISE Powershell session*__

SQL Server 2017 (Linux)- [requirements]
(https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup#system)
Cores - 2 (2 GHz)
Disk - 6 GB min
Memory - 2GB min

#### Create new machine - format
```
$ docker-machine create -d hyperv --hyperv-virtual-switch "Your LAN Virtual Switch" --hyperv-disk-size <mb> --hyperv-memory <mb> --hyperv-cpu-count 2 sqldbVM
```

### Create a docker machine for SQL Server
```
 PS C:\projects\aws\github\sqlserver> docker-machine create -d hyperv --hyperv-virtual-switch "Your LAN Virtual Switch" --hyperv-disk-size 20000 --hyperv-memory 4096 --hyperv-cpu-count 2 sqldbVM
Running pre-create checks...
Creating machine...
(sqldbVM) Copying C:\Users\Michael\.docker\machine\cache\boot2docker.iso to C:\Users\Michael\.docker\machine\machines\sqldbVM\boot2docker.iso...
(sqldbVM) Creating SSH key...
(sqldbVM) Creating VM...
(sqldbVM) Using switch "Your LAN Virtual Switch"
(sqldbVM) Creating VHD
(sqldbVM) Starting VM...
(sqldbVM) Waiting for host to start...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe env sql
dbVM
```

### Remove a docker machine
```
PS C:\projects\aws\github\sqlserver> docker-machine rm sqldbVM -y -f
About to remove sqldbVM
WARNING: This action will delete both local reference and remote instance.
Successfully removed sqldbVM
PS C:\projects\aws\github\sqlserver> docker-machine ls
NAME        ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER        ERRORS
defaultVM   -        hyperv   Running   tcp://192.168.1.188:2376           v17.12.0-ce  
```


## Set environment for sqldbVM
```
PS C:\projects\aws\github\sqlserver> docker-machine env sqldbVM
$Env:DOCKER_TLS_VERIFY = "1"
$Env:DOCKER_HOST = "tcp://192.168.1.195:2376"
$Env:DOCKER_CERT_PATH = "C:\Users\Michael\.docker\machine\machines\sqldbVM"
$Env:DOCKER_MACHINE_NAME = "sqldbVM"
$Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
# Run this command to configure your shell:
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env sqldbVM | Invoke-Expression
```

### Docker inspect container
```
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
        "VSwitch": "Your LAN Virtual Switch",
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
```

### Container -connect to bash shell
```
docker container exec -t -i 7cc69388ef9e /bin/bash
```

### Set environment for the machine
```
C:\projects\aws\github\sqlserver> docker-machine env sqldbVM
$Env:DOCKER_TLS_VERIFY = "1"
$Env:DOCKER_HOST = "tcp://192.168.1.196:2376"
$Env:DOCKER_CERT_PATH = "C:\Users\Michael\.docker\machine\machines\sqldbVM"
$Env:DOCKER_MACHINE_NAME = "sqldbVM"
$Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
# Run this command to configure your shell:
# & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env sqldbVM | Invoke-Expression
```

### Execute environment setting command
```
 PS C:\projects\aws\github\sqlserver> & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env sqldbVM | Invoke-Expression
```

### Install sql server 2017 container
```
C:\projects\aws\github\sqlserver> docker pull microsoft/mssql-server-linux:2017-latest
2017-latest: Pulling from microsoft/mssql-server-linux
f6fa9a861b90: Pulling fs layer
da7318603015: Pulling fs layer
6a8bd10c9278: Pulling fs layer
...
ea7199b7fbb6: Pull complete
Digest: sha256:fa1265cae3447320542cd0d1da160988aeffe5d2ae602a84336ddbd069e2b4ee
Status: Downloaded newer image for microsoft/mssql-server-linux:2017-latest
```

### Start the container image
```
C:\projects\aws\github\sqlserver> docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=pwd" `
   -p 1401:1433 --name sql1 `
   -d microsoft/mssql-server-linux:2017-latest

834402886b5ff926930fa467485acbae131eb1b538b25ff798002b985422a1a4
```

### Verify container execution
```
C:\projects\aws\github\sqlserver> docker ps -a
CONTAINER ID        IMAGE                                      COMMAND                  CREATED              STATUS              PORTS                    NAMES
834402886b5f        microsoft/mssql-server-linux:2017-latest   "/bin/sh -c /opt/mssâ€¦"   About a minute ago   Up About a minute   0.0.0.0:1401->1433/tcp   sql1
```

### Changes the internal name of the container to a custom value
```
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```
[Docker SQLServer on Linux Quickstart](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker)

### Connect to sql server
```
docker exec -it sql1 "bash"
root@834402886b5f:/#

OR

C:\projects\aws\github\sqlserver> docker container exec -t -i 834402886b5f /bin/bash
root@834402886b5f:/#
```

### Connect from outside the container
__*NOTE: Port 1401 is the port mapped from container to the outside - inside container 1403 - outside container 1401*__
```
C:\Users\Michael> sqlcmd -S 192.168.1.196,1401 -U SA -P 'pwd'
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
```


### Change the SA password
```
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U SA -P "<YourStrong!Passw0rd>" `
   -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong!Passw0rd>'"
```

### Create a test db
```
root@834402886b5f:/# /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'pwd'
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
```


##### [INSTALL AND CONFIGURE](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker)

##### [TROUBLESHOOTING](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker#troubleshooting)
