# docker-slurm
Slurm docker containers

#Getting started instructions:

1) run
  docker-compose build

2) Generate a munge key and add it as a docker secret named munge_key

3) run
  docker stack deploy -f docker-cloud.yml slurm

4) Logon to the db server and run from a mysql prompt:
  grant all on slurm_acct_db.* to 'slurm'@'%'
  drop database slurm_acct_db;

  (The drop database command may hang if this is the case then stop the slurmdbd
container)

5) In the login container run the following to register the cluster into the
accounting database:
  sacctmgr add -i cluster cluster

6) The login node should now be capable of submitting jobs to the slurm queue
