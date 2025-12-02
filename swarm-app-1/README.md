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
87f93c03a2b2   none              null      local
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
