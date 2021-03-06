# Atlassian Docker
It's Atlassian docker compose file, to run Atlassian products (Jira, Confluence, Bitbucket) with docker on one single machine.

### How to use:

1. Clone the atlassian:

    ```
    $ git clone git@github.com:ChunAllen/atlassian_docker.git
    ```

2. Set environment variables:


    ```
    $ export DOMAIN=example.com
     ```

3. Run docker compose:


    ```
    $ docker-compose -p atlassian up
    ```

4. Set `DNS` according to the above `DOMAIN` value, on somewhere that you want to connect to host of `docker-compose`:


    ```
    $ vim /etc/hosts
        127.0.0.1 jira.example.com www.jira.example.com
        127.0.0.1 wiki.example.com www.wiki.example.com
        127.0.0.1 bitbucket.example.com www.bitbucket.example.com
    ```
Replace `127.0.0.1` with IP of host that `docker-compose` command run on it.


```
jira.example.com   wiki.example.com   bitbucket.example.com
       +                   +                    +
       |                   |                    |
       +----------------------------------------+
                           |
                           v
                         Nginx
                           +
       +-----------------------------------------+
       |                   |                     |
       v                   v                     v
   Atlassian Jira    Atlassian Confluence   Atlassian Bitbucket
 [host:jira:8080]   [host:confluence:8090]  [host:bitbucket:7990]
       +                   +                     |
       |                   |                     |
       +-----------------------------------------+
                           |
                           v
                        Postgres
                   [host:database:5432]
                           +
                           |
       +------------------------------------------+
       |                   |                      |
       v                   v                      v
   [db:jira]           [db:wiki]          [db:bitbucket]
```


Atlassian supported products:

- Jira `7.0.5`
- Confluence `5.9.4`
- Bitbucket `4.14`

With:
- Postgres `9.4`
- Nginx `latest`

Requirements:

- Docker version 1.13.1+
- docker-compose version 1.10.0+

Docker image source files:

- [cptactionhank/atlassian-jira](https://hub.docker.com/r/cptactionhank/atlassian-jira/)
- [cptactionhank/atlassian-confluence](https://hub.docker.com/r/cptactionhank/atlassian-confluence/)
- [atlassian/bitbucket-server](https://hub.docker.com/r/atlassian/bitbucket-server/)
- [nginx](https://hub.docker.com/_/nginx/)
- [postgres](https://hub.docker.com/_/postgres/)


5. Create Databases:


    ```
    $ docker exec -it atlassian_database_1  psql -U postgres
       postgres=# CREATE DATABASE jira;
       postgres=# CREATE DATABASE wiki;
       postgres=# CREATE DATABASE bitbucket;
       postgres=# \l
       postgres-# \q
    ```

6. Browse Atlassian products:


        ```
        http://jira.example.com
        http://wiki.example.com
        http://bitbucket.example.com
        ```

Notes:

Data persisted on the  named volumes, to see them:


       $ docker volume ls
       local               atlassian_bitbucket-data
       local               atlassian_confluence-data
       local               atlassian_jira-data
       local               atlassian_database-data

### Disclaimer
This is the modified version of https://github.com/omidraha/atlassian.
In this repo, the volumes will be automatically mounted in your local directory.
