---
layout:     post
title:      Setup Oracle and T-SQL DB on Mac (using Docker)
date:       2018-07-15 01:58:00 - 5:30:00
summary:    Setup Oracle and T-SQL DB on Mac (using Docker)
categories: osx
thumbnail: jekyll
tags:
 - osx
 - database
 - tsql
 - oracle
---

# Setup Oracle and T-SQL DB on Mac (using Docker)

It was quite a hassle to find the _right_ way to install Oracle and TSQL db on a Mac, so I decided to create a post about it to keep it all in one place.

### Install Docker

Install docker from the [Mac Download page](https://store.docker.com/editions/community/docker-ce-desktop-mac?tab=description)

### Oracle DB installation

```
docker pull sath89/oracle-12c
docker run -d -p 8080:8080 -p 1521:1521 sath89/oracle-12c
docker logs -f <whatever hash you get on running>
```

Login using:

```
sqlplus system/oracle@//localhost:1521/xe
```

### TSQL installation

For TSQL, make sure to increase the memory allocated to it to around 4 GB (at least 3.25 GB)

```
docker pull microsoft/mssql-server-linux
docker run -d --name <name> -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<password>' -p 1433:1433 microsoft/mssql-server-linux
```

Make sure to keep a strong password, else the installation will fail.

##### Install sql-cli

```
sudo npm install -g sql-cli
```

##### Connect to the database

```
mssql -u sa -p <password>
```

