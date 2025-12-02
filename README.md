# Udemy Course Docker Mastery: with Kubernetes+Swarm from a Docker Captain

[![](https://dcbadge.limes.pink/api/server/devops)](https://discord.gg/devops)

> Build, test, and deploy containers with the best mega-course on Docker, Kubernetes, Compose, Swarm and Registry using a cloud native DevOps mindset

This repository is for use in my Udemy Courses "[Docker Mastery](https://www.udemy.com/course/docker-mastery/?referralCode=1410924A733D33635CCB
)" and "[Swarm Mastery](https://www.udemy.com/course/docker-swarm-mastery/?referralCode=4927D9CB156D4AE0228C)"

- Get these courses with my "cheapest on the internet" coupon links: [bretfisher.com/courses](https://www.bretfisher.com/courses)
- Get the [weekly newsletter](https://www.bretfisher.com/newsletter) on all the cloud native DevOps content I'm creating.

## Course Sections

1. Quick Start!
   1. [README](./intro/README.md)
   2. [Commands and Links](./references/S01%20Commands%20and%20Links.md)
2. Course Introduction
3. The Best Way to Setup Docker for Your OS
4. Creating and Using Containers Like a Boss
5. Container Images, Where To Find Them and How To Build Them
6. Container Lifetime & Persistent Data: Volumes, Volumes, Volumes
7. Making It Easier with Docker Compose: The Multi-Container Tool
8. Swarm Intro and Creating a 3-Node Swarm Cluster
9.  Swarm Basic Features and How to Use Them In Your Workflow
10. Swarm App Lifecycle
11. Container Registries: Image Storage and Distribution
12. Docker in Production
13. The What and Why of Kubernetes
14. Kubernetes Architecture and Install
15. Your First Pods
16. Inspecting Kubernetes Resources
17. Exposing Kubernetes Ports
18. Kubernetes Management Techniques
19. Moving to Declarative Kubernetes YAML
20. Your Next Steps and The Future of Kubernetes
21. Automated CI Workflows
22. GitHub Actions Workflow Examples
23. Docker Security Good Defaults and Tools
24. Docker 19.03 Release New Features
25. DevOps and Docker Clips
26. Dockerfiles and Docker Images in 2022
27. Dockerfile and Compose File Reviews
28. Extra's, Common Questions, and Resources



  IPv4 address for enp0s3: 192.168.1.192
  IPv6 address for enp0s3: 2a00:a041:e92d:9e00:a00:27ff:fec5:80e0


Expanded Security Maintenance for Applications is not enabled.

33 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


Last login: Tue Dec  2 06:05:57 2025 from 192.168.1.247
adminov@ubuntuSVR1225DockerEngine:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS             PORTS      NAMES
d220174fdd44   redis:3.2      "docker-entrypoint.s…"   50 minutes ago      Up 50 minutes      6379/tcp   redis.3.13a45cm7fmmhq6scy8s8au7l4
7e46292bbce2   nginx:latest   "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp     vote.2.gfeu6gbyvemfk8asy55n6imeh
adminov@ubuntuSVR1225DockerEngine:~$ docker service ls
ID             NAME      MODE         REPLICAS   IMAGE                                          PORTS
hz81un2853ae   redis     replicated   3/3        redis:3.2
f0zfcja7btf8   result    replicated   1/1        dockersamples/examplevotingapp_result:before   *:5001->80/tcp
rc32mw7ldfmp   vote      replicated   3/3        nginx:latest                                   *:80->80/tcp
359zql8dee40   worker    replicated   1/1        dockersamples/examplevotingapp_worker:latest
adminov@ubuntuSVR1225DockerEngine:~$ docker volume rm db-data
db-data
adminov@ubuntuSVR1225DockerEngine:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS             PORTS      NAMES
d220174fdd44   redis:3.2      "docker-entrypoint.s…"   50 minutes ago      Up 50 minutes      6379/tcp   redis.3.13a45cm7fmmhq6scy8s8au7l4
7e46292bbce2   nginx:latest   "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp     vote.2.gfeu6gbyvemfk8asy55n6imeh
adminov@ubuntuSVR1225DockerEngine:~$ docker service rm db
Error response from daemon: service db not found
adminov@ubuntuSVR1225DockerEngine:~$ docker service create --name db --network backend --mount type=volume,source=db-data,target=/var/lib/postgresql/data -e POSTGRES_PASSWORD=mypassword postgres:latest
623257lrp5dib0amtwcxzixe7
overall progress: 0 out of 1 tasks
1/1: ready     [======================================>            ]
verify: Detected task failure
^COperation continuing in background.
Use `docker service ps 623257lrp5dib0amtwcxzixe7` to check progress.
adminov@ubuntuSVR1225DockerEngine:~$ ^C
adminov@ubuntuSVR1225DockerEngine:~$ docker service rm db
db
adminov@ubuntuSVR1225DockerEngine:~$ docker volume create db-data
db-data
adminov@ubuntuSVR1225DockerEngine:~$ docker service create --name db --network backend --mount type=volume,source=db-datoverall progress: 0 out of 1 tasks
1/1: assigned  [======================>                            ]
verify: Detected task failure
^COperation continuing in background.=======================>      ]
adminov@ubuntuSVR1225DockerEngine:~$ ^C
adminov@ubuntuSVR1225DockerEngine:~$ ^C
adminov@ubuntuSVR1225DockerEngine:~$ docker run --rm -it \
>   -v db-data:/var/lib/postgresql/data \
>   -e POSTGRES_PASSWORD=mypassword \
>   -e POSTGRES_USER=admin \
>   -e POSTGRES_DB=mydb \
>   postgres:16
Unable to find image 'postgres:16' locally
16: Pulling from library/postgres
a9fe86bc7f87: Pull complete
115db6d16b77: Pull complete
9bb5da4cc6c3: Pull complete
ef2bcb52083b: Pull complete
9d6744175495: Pull complete
49c4e00df3b4: Pull complete
2b3e79aa53c6: Pull complete
34fde4663855: Pull complete
65e5c9408eb3: Pull complete
2edb059d627d: Pull complete
5e588012c944: Pull complete
e8ff652dc231: Pull complete
a2e54379c540: Pull complete
b85d0e635948: Download complete
a6f7ddee4bf8: Download complete
Digest: sha256:cf2a05fe40887b721e4b3dbac8fd32673c08292dcc8ba6b62b52b7f640433bd0
Status: Downloaded newer image for postgres:16
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... Etc/UTC
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

initdb: warning: enabling "trust" authentication for local connections
initdb: hint: You can change this by editing pg_hba.conf or using the option -A, or --auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgresql/data -l logfile start

waiting for server to start....2025-12-02 11:42:10.215 UTC [47] LOG:  starting PostgreSQL 16.11 (Debian 16.11-1.pgdg13+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 14.2.0-19) 14.2.0, 64-bit
2025-12-02 11:42:10.219 UTC [47] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2025-12-02 11:42:10.232 UTC [50] LOG:  database system was shut down at 2025-12-02 11:42:07 UTC
2025-12-02 11:42:10.239 UTC [47] LOG:  database system is ready to accept connections
 done
server started
CREATE DATABASE


/usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

waiting for server to shut down...2025-12-02 11:42:10.648 UTC [47] LOG:  received fast shutdown request
.2025-12-02 11:42:10.653 UTC [47] LOG:  aborting any active transactions
2025-12-02 11:42:10.655 UTC [47] LOG:  background worker "logical replication launcher" (PID 53) exited with exit code 1
2025-12-02 11:42:10.657 UTC [48] LOG:  shutting down
2025-12-02 11:42:10.662 UTC [48] LOG:  checkpoint starting: shutdown immediate
2025-12-02 11:42:11.085 UTC [48] LOG:  checkpoint complete: wrote 926 buffers (5.7%); 0 WAL file(s) added, 0 removed, 0 recycled; write=0.166 s, sync=0.240 s, total=0.428 s; sync files=301, longest=0.144 s, average=0.001 s; distance=4273 kB, estimate=4273 kB; lsn=0/191F0D0, redo lsn=0/191F0D0
2025-12-02 11:42:11.092 UTC [47] LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

2025-12-02 11:42:11.206 UTC [1] LOG:  starting PostgreSQL 16.11 (Debian 16.11-1.pgdg13+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 14.2.0-19) 14.2.0, 64-bit
2025-12-02 11:42:11.207 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2025-12-02 11:42:11.207 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2025-12-02 11:42:11.214 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2025-12-02 11:42:11.225 UTC [63] LOG:  database system was shut down at 2025-12-02 11:42:11 UTC
2025-12-02 11:42:11.239 UTC [1] LOG:  database system is ready to accept connections
2025-12-02 11:47:11.277 UTC [61] LOG:  checkpoint starting: time
2025-12-02 11:47:15.689 UTC [61] LOG:  checkpoint complete: wrote 45 buffers (0.3%); 0 WAL file(s) added, 0 removed, 0 recycled; write=4.383 s, sync=0.011 s, total=4.413 s; sync files=12, longest=0.003 s, average=0.001 s; distance=260 kB, estimate=260 kB; lsn=0/19604D0, redo lsn=0/1960498
^C2025-12-02 11:47:27.659 UTC [1] LOG:  received fast shutdown request
2025-12-02 11:47:27.663 UTC [1] LOG:  aborting any active transactions
2025-12-02 11:47:27.665 UTC [1] LOG:  background worker "logical replication launcher" (PID 66) exited with exit code 1
2025-12-02 11:47:27.668 UTC [61] LOG:  shutting down
2025-12-02 11:47:27.672 UTC [61] LOG:  checkpoint starting: shutdown immediate
2025-12-02 11:47:27.684 UTC [61] LOG:  checkpoint complete: wrote 0 buffers (0.0%); 0 WAL file(s) added, 0 removed, 0 recycled; write=0.001 s, sync=0.001 s, total=0.016 s; sync files=0, longest=0.000 s, average=0.000 s; distance=0 kB, estimate=234 kB; lsn=0/1960580, redo lsn=0/1960580
2025-12-02 11:47:27.689 UTC [1] LOG:  database system is shut down
^C^Cadminov@ubuntuSVR1225DockerEngine:~$ ^C
adminov@ubuntuSVR1225DockerEngine:~$ ^C
adminov@ubuntuSVR1225DockerEngine:~$ ^C
adminov@ubuntuSVR1225DockerEngine:~$ docker run -v db-data:var/lib/postgresql/data postgres:16
docker: Error response from daemon: invalid volume specification: 'db-data:var/lib/postgresql/data': invalid mount config for type "volume": invalid mount path: 'var/lib/postgresql/data' mount path must be absolute

Run 'docker run --help' for more information
adminov@ubuntuSVR1225DockerEngine:~$ docker volume create db-data
db-data
adminov@ubuntuSVR1225DockerEngine:~$ docker run -v db-data:var/lib/postgresql/data postgres:16
docker: Error response from daemon: invalid volume specification: 'db-data:var/lib/postgresql/data': invalid mount config for type "volume": invalid mount path: 'var/lib/postgresql/data' mount path must be absolute

Run 'docker run --help' for more information
adminov@ubuntuSVR1225DockerEngine:~$ docker run -v db-data:/var/lib/postgresql/data postgres:16

PostgreSQL Database directory appears to contain a database; Skipping initialization

2025-12-02 11:48:56.481 UTC [1] LOG:  starting PostgreSQL 16.11 (Debian 16.11-1.pgdg13+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 14.2.0-19) 14.2.0, 64-bit
2025-12-02 11:48:56.481 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2025-12-02 11:48:56.481 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2025-12-02 11:48:56.488 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2025-12-02 11:48:56.498 UTC [29] LOG:  database system was shut down at 2025-12-02 11:47:27 UTC
2025-12-02 11:48:56.507 UTC [1] LOG:  database system is ready to accept connections
^C2025-12-02 11:49:01.696 UTC [1] LOG:  received fast shutdown request
2025-12-02 11:49:01.700 UTC [1] LOG:  aborting any active transactions
2025-12-02 11:49:01.702 UTC [1] LOG:  background worker "logical replication launcher" (PID 32) exited with exit code 1
2025-12-02 11:49:01.703 UTC [27] LOG:  shutting down
2025-12-02 11:49:01.708 UTC [27] LOG:  checkpoint starting: shutdown immediate
2025-12-02 11:49:01.741 UTC [27] LOG:  checkpoint complete: wrote 3 buffers (0.0%); 0 WAL file(s) added, 0 removed, 0 recycled; write=0.015 s, sync=0.003 s, total=0.038 s; sync files=2, longest=0.002 s, average=0.002 s; distance=0 kB, estimate=0 kB; lsn=0/19605F8, redo lsn=0/19605F8
2025-12-02 11:49:01.746 UTC [1] LOG:  database system is shut down
adminov@ubuntuSVR1225DockerEngine:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS             PORTS      NAMES
d220174fdd44   redis:3.2      "docker-entrypoint.s…"   About an hour ago   Up About an hour   6379/tcp   redis.3.13a45cm7fmmhq6scy8s8au7l4
7e46292bbce2   nginx:latest   "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp     vote.2.gfeu6gbyvemfk8asy55n6imeh
adminov@ubuntuSVR1225DockerEngine:~$ docker container ls
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS             PORTS      NAMES
d220174fdd44   redis:3.2      "docker-entrypoint.s…"   About an hour ago   Up About an hour   6379/tcp   redis.3.13a45cm7fmmhq6scy8s8au7l4
7e46292bbce2   nginx:latest   "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp     vote.2.gfeu6gbyvemfk8asy55n6imeh
adminov@ubuntuSVR1225DockerEngine:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS             PORTS      NAMES
d220174fdd44   redis:3.2      "docker-entrypoint.s…"   About an hour ago   Up About an hour   6379/tcp   redis.3.13a45cm7fmmhq6scy8s8au7l4
7e46292bbce2   nginx:latest   "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp     vote.2.gfeu6gbyvemfk8asy55n6imeh
adminov@ubuntuSVR1225DockerEngine:~$ docker ls
docker: unknown command: docker ls

Run 'docker --help' for more information
adminov@ubuntuSVR1225DockerEngine:~$ docker container ls
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS             PORTS      NAMES
d220174fdd44   redis:3.2      "docker-entrypoint.s…"   About an hour ago   Up About an hour   6379/tcp   redis.3.13a45cm7fmmhq6scy8s8au7l4
7e46292bbce2   nginx:latest   "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp     vote.2.gfeu6gbyvemfk8asy55n6imeh
adminov@ubuntuSVR1225DockerEngine:~$ docker run -d -v db-data:/var/lib/postgresql/data postgres:16
2ec44f601ddd9e775b748a30bc66e43a04bb4a466c662916e9d211b3ddf4b0be
adminov@ubuntuSVR1225DockerEngine:~$ docker container ls
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS             PORTS      NAMES
2ec44f601ddd   postgres:16    "docker-entrypoint.s…"   3 seconds ago       Up 2 seconds       5432/tcp   clever_mclaren
d220174fdd44   redis:3.2      "docker-entrypoint.s…"   About an hour ago   Up About an hour   6379/tcp   redis.3.13a45cm7fmmhq6scy8s8au7l4
7e46292bbce2   nginx:latest   "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp     vote.2.gfeu6gbyvemfk8asy55n6imeh
adminov@ubuntuSVR1225DockerEngine:~$
adminov@ubuntuSVR1225DockerEngine:~$




Feel free to create issues or PRs if you find a problem with examples in this repository!
