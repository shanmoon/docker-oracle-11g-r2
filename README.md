docker-oracle-xe-11g-r2
============================

Oracle Express Edition 11g Release 2 on Ubuntu 16.04 LTS


### Setup / Installation
```

docker build TOODO build arguments

docker pull shanmoon/oracle-xe-11g-r2
```

Run with 22 and 1521 ports opened:
```
docker run -d -p 49160:22 -p 49161:1521 shanmoon/oracle-xe-11g-r2
```

Run this, if you want the database to be connected remotely:
```
docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true shanmoon/oracle-xe-11g
```

By default, the password verification is disable(password never expired). If you want it back, run this:
```
docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_PASSWORD_VERIFY=true shanmoon/oracle-xe-11g-r2
```

For performance concern, you may want to disable the disk asynch IO:
```
docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_DISABLE_ASYNCH_IO=true shanmoon/oracle-xe-11g-r2
```

For XDB user, run this:
```
docker run -d -p 49160:22 -p 49161:1521 -p 8080:8080 -e ORACLE_ENABLE_XDB=true shanmoon/oracle-xe-11g-r2
```

Verify localhost:8080 is functioning
```
curl -XGET "http://localhost:8080"
```
You should see
```
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<HTML><HEAD>
<TITLE>401 Unauthorized</TITLE>
</HEAD><BODY><H1>Unauthorized</H1>
</BODY></HTML>
```

```
# Login http://localhost:8080 with these credentials:
username: XDB
password: xdb
```

Connect to the database with these settings:
```
hostname: localhost
port: 49161
sid: xe
username: system
password: oracle
```

Password for SYS & SYSTEM
```
oracle
```

Login by SSH
```
ssh root@localhost -p 49160
password: admin
```

Support custom DB Initialization
```
# Dockerfile
FROM shanmoon/oracle-xe-11g-r2

ADD init.sql /docker-entrypoint-initdb.d/
```
