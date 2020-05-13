# dockerfile-cockroachoss
![Build CockroachDB core features (BSL)](https://github.com/kota2and3kan/dockerfile-cockroachoss/workflows/Build%20CockroachDB%20core%20features%20(BSL)/badge.svg)  
Dockerfile for local test of CockroachDB core features (Business Source License).  
This Dcokerfile builds a core features (Business Source License) of CockroachDB (Excluding enterprise functionality).


## Note
For test local or development environment.  
Not for production.


## Build
1. Clone this repository.
```
$ git clone https://github.com/kota2and3kan/dockerfile-cockroachoss.git
$ cd dockerfile-cockroachoss
```

2. Run `docker build`.
```
$ sudo docker build -t <image-name>:<tag-name> ./cockroachoss
```


## Usage

### Start CockroachDB
After build image, you can start CockroachDB (APL2) container by the following commands.


You can use this image almost the same way as described on this [Docs](https://www.cockroachlabs.com/docs/stable/start-a-local-cluster-in-docker.html), and you can see the more details (command options etc...) from the Docs.


1. Create docker network.
```
$ sudo docker network create -d bridge roachnet
```

2. Run first node.
```
$ sudo docker run -d \
--name=roach1 \
--hostname=roach1 \
--net=roachnet \
-p 26257:26257 -p 8080:8080 \
-v "${PWD}/cockroach-data/roach1:/cockroach/cockroach-data" \
cockroachoss:latest start --insecure
```

3. Add second node to the cluster.
```
$ sudo docker run -d \
--name=roach2 \
--hostname=roach2 \
--net=roachnet \
-v "${PWD}/cockroach-data/roach2:/cockroach/cockroach-data" \
cockroachoss:latest start --insecure --join=roach1
```

4. Add third node to the cluster.
```
$ sudo docker run -d \
--name=roach3 \
--hostname=roach3 \
--net=roachnet \
-v "${PWD}/cockroach-data/roach3:/cockroach/cockroach-data" \
cockroachoss:latest start --insecure --join=roach1
```

### Connect to CockroachDB
After start the CockroachDB, you can connect to the CockroachDB with using built-in SQL shell by the following command.
```
$ sudo docker run -it --rm --net roachnet cockroachoss:latest sql --insecure --host roach1
```

And, you can exit the SQL shell by the "\q" command.
```
root@roach1:26257/defaultdb> \q
```

Example
```
$ sudo docker run -it --rm --net roachnet cockroachoss:latest sql --insecure --host roach1
#
# Welcome to the CockroachDB SQL shell.
# All statements must be terminated by a semicolon.
# To exit, type: \q.
#
# Server version: CockroachDB OSS v19.2.2 (x86_64-linux-gnu, built 2019/12/20 06:27:02, go1.12.12) (same version as client)
# Cluster ID: 6934cb24-cb6f-4f51-ac53-ffae795c21f2
#
# Enter \? for a brief introduction.
#
root@roach1:26257/defaultdb>
root@roach1:26257/defaultdb> show databases;
  database_name  
+---------------+
  defaultdb      
  postgres       
  system         
(3 rows)

Time: 5.119444ms

root@roach1:26257/defaultdb>
root@roach1:26257/defaultdb> \q
```


### Access to Admin UI
After start the CockroachDB, you can access to the Admin UI by the following URL.

```
http://<IP>:8080/
```


## License
Please refer the [LICENSE](https://github.com/kota2and3kan/dockerfile-cockroachoss/blob/master/LICENSE) for the license of the files in this repository.
