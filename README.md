# Kafka Python with Tansu

Tansu is a Kafka compatible broker that stores data in S3 or PostgreSQL.
This example uses [KafkaJS](https://kafka.js.org),
with [MinIO S3](https://min.io) or [PostgreSQL](https://www.postgresql.org)
to send and receive messages with Tansu.

[MinIO](https://min.io), [PostgreSQL](https://www.postgresql.org) and Tansu
run in [Docker](https://docs.docker.com/desktop/) for easy installation and setup.

Copy `example.env` into `.env` so that you have a local working copy:

```shell
cp example.env .env
```

To use S3 with [MinIO](https://min.io), uncomment this line and comment out
other `STORAGE_ENGINE` definitions in your `.env`:

```
STORAGE_ENGINE="s3://tansu/"
```

To use [PostgreSQL](https://www.postgresql.org), uncomment this line and
comment out other `STORAGE_ENGINE` definitions in your `.env`:

```
STORAGE_ENGINE="postgres://postgres:postgres@localhost"
```

Now, start up Minio S3, PostgreSQL and Tansu:

```shell
docker compose up -d
```

Tansu's PostgreSQL schema is set up in [compose.yaml](./compose.yaml),
requiring no extra configuration. If you're using MinIO, there are 3 extra steps.

Wait for MinIO to become ready:

```shell
docker compose exec minio /usr/bin/mc ready local
```

Then, setup some default MinIO credentials:

```shell
docker compose exec minio /usr/bin/mc alias set local http://localhost:9000 minioadmin minioadmin
```

Finally, create a bucket on MinIO for Tansu:

```shell
docker compose exec minio /usr/bin/mc mb local/tansu
```

Using [nodejs](https://nodejs.org/):

```shell
npm install
```

Run the example:

```shell
node producer.js
```

In this example we have used the [KafkaJS](https://kafka.js.org)
client to send and receive messages to Tansu a Kafka compatible broker using
S3 or PostgreSQL for storage.
