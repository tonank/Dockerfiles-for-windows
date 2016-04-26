# Dockerfiles-for-windows

* sqlexpress: SQL Server 2014 Express
* dotnet-aspnet46-webapp: ASP.NET 4.6 Web UI app ("bcw" Bike Commuter Weather app), running under IIS - built in the Dockerfile using msbuild
  * There is a docker-compose.yml file in dotnet-aspnet46-webapp, to run both ui and service locally - however, docker-compose network mode functionality is limited as of TP4: The app will be accessible from outside only if run via docker run, not docker-compose
* python-rest-service: Python REST web service consumed by the "bcw" UI above (may instead be run under Linux, via Linux Dockerfile at [Dockerfiles for Linux](https://github.com/brogersyh/Dockerfiles-for-Linux))
* swarm-windows: Docker Swarm executable for Windows
* postgresql: PostgreSQL 9.5
* ruby-sinatra-helloworld: simple "hello world" ruby sinatra web app
* jdk8: Java JDK 8

Each of the above is entirely self-contained - all you need is what's in the dockerfile folder:
* docker build -t sqlexpress ./sqlexpress
  * docker run -d -p 1433:1433 sqlexpress
  * The sa password (specified in the Dockerfile) is: thepassword2#
* docker build -t bcwui ./dotnet-aspnet46-webapp
  * docker run -d -e WeatherServiceUrl=http://**pythonRestServiceHost**.eastus.cloudapp.azure.com:5000 -p 80:80 bcwui
* docker build -t bcwservice ./python-rest-service
  * docker run -d -e WUNDERGROUND_API_KEY=YourWundergroundApiKey -p 5000:5000 bcwservice
* docker build -t swarm ./swarm-windows
  * docker run --rm swarm create
* docker build -t postgresql ./postgresql
  * docker run -d -p 5432:5432 postgresql
  * test using a client such as pgAdmin III, psql or the "DB Navigator" plugin for Jetbrains IDEs IntelliJ IDEA and others - login as user 'postgres', empty password
* docker build -t ruby-sinatra-helloworld ./ruby-sinatra-helloworld
  * docker run -d -p 4567:4567 ruby-sinatra-helloworld
* docker build -t jdk8 ./jdk8
  * (intended as a base image for app requiring jdk to build)

Please see my blog post [Dockerfile to create SQL Server Express windows container image](http://26thcentury.com/2016/01/03/dockerfile-to-create-sql-server-express-windows-container-image/) for a detailed description of the SQL Server dockerfile

# Credits
* [Stefan Scherer: Windows Dockerfiles](https://github.com/StefanScherer/dockerfiles-windows) - includes Compose, Consul, Golang, Jenkins-swarm-slave, node.js, Swarm <br />
* [Stefan Scherer: How to run a Windows Docker Engine in Azure]
(https://stefanscherer.github.io/how-to-run-windows-docker-engine-in-azure/) - defines an Azure template which improves on the 
[Msft Azure TP4 quickstart-templates](https://github.com/Azure/azure-quickstart-templates), including enabling public TCP listening<br />
* [Stefan Scherer: Build Docker Swarm binary for Windows the "Docker way"]
(https://stefanscherer.github.io/build-docker-swarm-for-windows-the-docker-way/)- dockerized Swarm image for Windows<br />
* [Stefan Sherer: Docker-themed blog](https://stefanscherer.github.io/) - includes using Chocolatey to get set up to run Docker Linux containers on Windows, and posts describing Docker on Raspberry Pi<br />
