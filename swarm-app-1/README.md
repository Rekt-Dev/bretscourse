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
