# Setup
[Youtube tutorial](https://youtu.be/rd02PnWRAW0?si=Ct_JpNLM05y-Rnf8)
[Cloudflaretutorial](https://www.sambobb.com/posts/cloudflared-in-docker-compose/)


1.  List networks `docker network ls`

2.  Create docker virtual network `docker network create app_network`

3.  Up postgres engine `docker compose -f postgres.yml up -d`

4. Install pgvector into postgres container

    4.1. Open terminal into container
    `docker exec -it postgres_aula sh`
    
    4.2 Execute:
    
    `apt-get update`

    `apt-get install -y git build-essential postgresql-server-dev-15 curl`
    
    `git clone https://github.com/pgvector/pgvector.git /usr/src/pgvector`

    `cd /usr/src/pgvector`

    `make && make install`

    4.3 Activate extension:

    `psql -U postgres`

    `CREATE EXTENSION IF NOT EXISTS vector;`

    4.3 Create databases

    `CREATE DATABASE chatwoot;`

    `CREATE DATABASE evolution;`

    `CREATE DATABASE n8n_fila;`    

5.  Up redis `docker compose -f redis.yml up -d`

6.  Up minio `docker compose -f minio.yml up -d`

    6.1  Go to minio admin panel [http://localhost:9001/login](http://localhost:9001/login) using `admin` and `minha_senha`

    6.2  Create a bucket callet `evolution` for evolution api, go to summary and change access policy to `public`

    6.2  Create a access key called `evolution` for evolution api and copy this value into evolution config on `S3_ACCESS_KEY` and `S3_SECRET_KEY`

    access key: 0KcdtnOusrdQ86V80C9k

    secret key: YJmOjCpNboUC1TorYTbWCktxrV0lVWbHs0Rzb9UD

    6.3 The same steps for `chatwoot`

7.  Open `evolution.yml file` and update `SERVER_URL` with current host local ip.

8.  Up minio `docker compose -f evolution.yml up -d`.

    8.1  Go to evolution api admin panel [http://localhost:8080/manager](http://localhost:8080/manager) using `AUTHENTICATION_API_KEY`.

9.  Open `chatwoot.yml file` and update `SERVER_URL` with current host local ip.

10.  Up chatwoot `docker compose -f chatwoot.yml up -d`.

        10.1 Open terminal into container

        `docker exec -it chatwoot-rails sh`

        10.2 Execute:
    
        `bundle exec rails db:migrate`

        `bundle exec rails db:chatwoot_prepare`    

        10.3  Go to chatwoot admin panel [http://localhost:3000/installation/onboarding](http://localhost:3000/installation/onboarding).

11.  Open `n8n.yml file` and update `SERVER_URL` with current host local ip.

12.  Up chatwoot `docker compose -f n8n.yml up -d`.

        12.1  Go to n8n admin panel [http://localhost:5678/setup](http://localhost:5678/setup).

13.  On cloudflare [https://one.dash.cloudflare.com/](https://one.dash.cloudflare.com/) connector secction generate token.

14.  Up cloudflare local `docker compose -f cloudflare.yml up -d`.