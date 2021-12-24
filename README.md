# psql-nasty-surprise
the repo to illustrate medium article

1. put `migration` file into `changelog` folder for future mapping
2. create empty Postgres database container
```
docker run -d --rm -p 5432:5432 --name postgrestest -v ./changelog:\changelog -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password postgres:13.1-alpine
```
3. apply migrations using psql
```
docker exec -i postgrestest psql -h localhost -U user -d postgres -f \changelog/migrations.sql
```
5. visit pulblic schema - you will see migrations A and C in `public.__EFMigrationsHistory` table
6. stop container `postgrestest`
7. create empty Postgres database container one more time
```
docker run -d --rm -p 5432:5432 --name postgrestest -v ./changelog:\changelog -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password postgres:13.1-alpine
```
8. apply migrations using `Liquibase`
```
docker run --rm -v ./changelog:\liquibase\changelog liquibase/liquibase --driver=org.postgresql.Driver --url=jdbc:postgresql://host.docker.internal:5432/postgres --changeLogFile=migrations.sql --username=platform --password=platform update
```
9. visit pulblic schema - you will see only migrations A in `public.__EFMigrationsHistory` table


