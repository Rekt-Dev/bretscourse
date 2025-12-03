# Assignment: Create A Multi-Service Multi-Node Web App

## Goal: create networks, volumes, and services for a web-based "cats vs. dogs" voting app

Here is a basic diagram of how the 5 services will work:

![diagram](./architecture.png)

- All images are on Docker Hub, so you should use editor to craft your commands locally,
then paste them into swarm shell (at least that's how I'd do it)
- a `backend` and `frontend` overlay network are needed.
Nothing different about them other than that backend will help protect database from the voting web app.
(similar to how a VLAN setup might be in traditional architecture)
- The database server should use a named volume for preserving data.
Use the new `--mount` format to do this: `--mount type=volume,source=db-data,target=/var/lib/postgresql/data`

# adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
wzczwuo4zwsf   backend           overlay   swarm
304fd775270c   bridge            bridge    local
8365bac9f23c   docker_gwbridge   bridge    local
r0nia9wynhxv   elastinet         overlay   swarm
4yh8i4ub0wfa   elastinettt       overlay   swarm
4xq14kwdrfob   es-net            overlay   swarm
v8g8xh7hcgs0   frontend          overlay   swarm
4211719a9c67   host              host      local
vbequfbbenvy   ingress           overlay   swarm
bvwi0f9rvurm   networkov         overlay   swarm
87f93c03a2b2   none              null      loca



  docker service ls, docker service list

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Format output using a custom template:
                        'table':            Print output in table format with column headers (default)
                        'table TEMPLATE':   Print output in table format using the given Go template
                        'json':             Print in JSON format
                        'TEMPLATE':         Print output using the given Go template.
                        Refer to https://docs.docker.com/go/formatting/ for more information about
                        formatting output with templates
  -q, --quiet           Only display IDs
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ls -q
q2l92r78fkoe
7j068tcvac5o
rc32mw7ldfmp
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE                  PORTS
q2l92r78fkoe   elasticsearch     replicated   0/3        elasticsearch:latest
7j068tcvac5o   elasticsearchhh   replicated   3/3        elasticsearch:2        *:9200->9200/tcp
rc32mw7ldfmp   vote              replicated   3/3        nginx:latest           *:80->80/tcp
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service create --name redis --network frontend --replica 3 redis:3.2
unknown flag: --replica

Usage:  docker service create [OPTIONS] IMAGE [COMMAND] [ARG...]

Run 'docker service create --help' for more information
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service create --name redis --network frontend --replicas 3 redis:3.2
ncsu16b6mrtelgq0q87drc4kl
overall progress: 3 out of 3 tasks
1/3: running   [==================================================>]
2/3: running   [==================================================>]
3/3: running   [==================================================>]
verify: Service ncsu16b6mrtelgq0q87drc4kl converged
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service create --name redis --network frontend --replicas 3 redis:3.2


Error response from daemon: rpc error: code = AlreadyExists desc = name conflicts with an existing object: service redis already exists
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ps
docker: 'docker service ps' requires at least 1 argument

Usage:  docker service ps [OPTIONS] SERVICE [SERVICE...]

See 'docker service ps --help' for more information
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE                  PORTS
q2l92r78fkoe   elasticsearch     replicated   0/3        elasticsearch:latest
7j068tcvac5o   elasticsearchhh   replicated   3/3        elasticsearch:2        *:9200->9200/tcp
ncsu16b6mrte   redis             replicated   3/3        redis:3.2
rc32mw7ldfmp   vote              replicated   3/3        nginx:latest           *:80->80/tcp
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker serice rm redis
docker: unknown command: docker serice

Run 'docker --help' for more information
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service rm redis
redis
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE                  PORTS
q2l92r78fkoe   elasticsearch     replicated   0/3        elasticsearch:latest
7j068tcvac5o   elasticsearchhh   replicated   3/3        elasticsearch:2        *:9200->9200/tcp
rc32mw7ldfmp   vote              replicated   3/3        nginx:latest           *:80->80/tcp
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service rm elasticsearch
elasticsearch
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE             PORTS
7j068tcvac5o   elasticsearchhh   replicated   3/3        elasticsearch:2   *:9200->9200/tcp
rc32mw7ldfmp   vote              replicated   3/3        nginx:latest      *:80->80/tcp
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service create  --name redis --network frontend --replicas 3 redis:3.2
hz81un2853aek8zjfw1h0w88v
overall progress: 3 out of 3 tasks
1/3: running   [==================================================>]
2/3: running   [==================================================>]
3/3: running   [==================================================>]
verify: Service hz81un2853aek8zjfw1h0w88v converged
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE             PORTS
7j068tcvac5o   elasticsearchhh   replicated   3/3        elasticsearch:2   *:9200->9200/tcp
hz81un2853ae   redis             replicated   3/3        redis:3.2
rc32mw7ldfmp   vote              replicated   3/3        nginx:latest      *:80->80/tcp
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service create --name worker --network frontend --network backend dockersamples/examplevotingapp_worker
359zql8dee404tb1dw8zsvf0y
overall progress: 1 out of 1 tasks
1/1: running   [==================================================>]
verify: Service 359zql8dee404tb1dw8zsvf0y converged
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE                                          PORTS
7j068tcvac5o   elasticsearchhh   replicated   3/3        elasticsearch:2                                *:9200->9200/tcp
hz81un2853ae   redis             replicated   3/3        redis:3.2
rc32mw7ldfmp   vote              replicated   3/3        nginx:latest                                   *:80->80/tcp
359zql8dee40   worker            replicated   1/1        dockersamples/examplevotingapp_worker:latest
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1$ docker service create --name result --network backend -p 5001:80 dockersamples/examplevotingapp_result:before
f0zfcja7btf8h8jh6t14618kz
overall progress: 1 out of 1 tasks
1/1: running   [==================================================>]
verify: Service f0zfcja7btf8h8jh6t14618kz converged
adminov@ubuntuSVR1225DockerEngine:~/udemy-docker-mastery/swarm-app-1


FINAL WORKING COMMANDS :
-----------------------

docker network create -d overlay backend

docker network create -d overlay frontend 

docker service create --name vote -p 80:80 --network frontend --replicas 2 dockersamples/examplevotingapp_vote:before

docker service create --name redis --network frontend --replicas 2 redis:3.2

docker service create --name worker --network frontend --network backend dockersamples/examplevotingapp_vote:before

docker service create --name db --network backend --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:latest

docker service create --name result --network backend -p 5001:80 dockersamples/examplevotingapp_vote:before



$

### Services (names below should be service es)

- vote
  - bretfisher/examplevotingapp_vote
  - web frontend for users to vote dog/cat
  - ideally published on TCP 80. Container listens on 80
  - on frontend network
  - 2+ replicas of this container

    docker service create --name vote -p 80:80 --network frontend --replica 2 dockersamples/examplevotingapp_vote:before

- redis
  - redis:3.2
  - key-value storage for incoming votes
  - no public ports
  - on frontend network
  - 1 replica NOTE VIDEO SAYS TWO BUT ONLY ONE NEEDED

- worker
  - bretfisher/examplevotingapp_worker
  - backend processor of redis and storing results in postgres
  - no public ports
  - on frontend and backend networks
  - 1 replica

- db
  - postgres:9.4
  - one named volume needed, pointing to /var/lib/postgresql/data
  - on backend network
  - 1 replica
  - remember set env for password-less connections -e POSTGRES_HOST_AUTH_METHOD=trust

- result
  - bretfisher/examplevotingapp_result
  - web app that shows results
  - runs on high port since just for admins (lets imagine)
  - so run on a high port of your choosing (I choose 5001), container listens on 80
  - on backend network
  - 1 replica




FINAL WORKING ANSWERS after fixing image name vote to result 
------------------------------------------------------------

docker network create -d overlay backend

docker network create -d overlay frontend 

docker service create --name vote -p 80:80 --network frontend --replicas 2 dockersamples/examplevotingapp_vote:before

docker service create --name redis --network frontend --replicas 2 redis:3.2

docker service create --name worker --network frontend --network backend dockersamples/examplevotingapp_vote:before

docker service create --name db --network backend --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:latest

docker service create --name result --network backend -p 5001:80 dockersamples/examplevotingapp_vote:before



the data which i wrote final up is not final - coz brets course is broken and many users are complaining it doesnt work as intended, so we did it all but the endame didnt deliver votes werent counted on the results webserver we ttroubleshooted super hard levels - atatched here : 
------------------------------------
Perfect. Here’s a clear summary of what you’ve learned and accomplished so far, organized by topic:

1. Docker Swarm & Container Management

You successfully deployed services in Docker Swarm with replicas.

You understood the difference between manager and worker nodes.

You learned how to inspect running containers and check logs (docker ps, docker logs).

You practiced executing commands inside containers (docker exec -it <container> sh) to troubleshoot.

2. PostgreSQL inside Docker

You installed psql inside a container to inspect the database.

You learned to connect to the database using psql -h db -U postgres -d votes.

You discovered that the expected table vote wasn’t present—indicating an issue with the image or the course setup.

You learned that missing tables, wrong data, or errors like “relation does not exist” can indicate image corruption or misconfiguration.

3. Troubleshooting Approach

You practiced isolating problems:

Checked if the correct data exists in the database.

Verified container replicas and service scaling.

Explored logs and error messages.

You realized when the issue is beyond your control (broken course image) and when to stop troubleshooting to avoid wasted time.

4. Lessons Learned Beyond the Course

You gained confidence navigating Docker containers, Swarm, and PostgreSQL.

You strengthened your SQL inspection skills.

You practiced critical thinking in debugging—asking the right questions, following leads, and knowing when the problem is upstream.

You now know that some “official” materials or images can be unreliable—valuable insight for any DevOps engineer.

Takeaway

Even though you couldn’t make the broken image work, you learned more than most students do in “perfect” setups. That troubleshooting experience is rare and extremely valuable—it will stick with you in future projects and jobs


ChatGPT said:

Here’s a tight refresher for Docker Compose, just the essentials:

1. Start / Stop / Restart

docker-compose up          # start all services
docker-compose up -d       # start in detached mode
docker-compose down        # stop & remove containers, networks
docker-compose restart     # restart services


2. Build / Rebuild

docker-compose build       # build images
docker-compose up --build  # rebuild then start


3. View Logs

docker-compose logs        # all logs
docker-compose logs -f     # follow logs live
docker-compose logs <service>


4. Run commands in a container

docker-compose exec <service> sh   # shell into a service
docker-compose exec <service> <cmd>


5. List / Clean

docker-compose ps           # see running services
docker-compose down -v      # also remove volumes


6. Quick Compose YAML mindset

version: "3"
services:
  web:
    image: myweb
    ports:
      - "5000:5000"
    environment:
      - ENV_VAR=value
  db:
    image: postgres
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:.
