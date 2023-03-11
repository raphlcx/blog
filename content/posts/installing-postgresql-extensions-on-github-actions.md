---
title: "Installing PostgreSQL extensions on GitHub Actions"
date: 2023-03-11T16:05:01+08:00
---
On GitHub Actions, you can easily start a PostgreSQL service using its service container.

For instance:

```
jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
```

The job starts a PostgreSQL service using the `postgres` Docker image.

If you wish to install additional PostgreSQL extensions, you could build a custom PostgreSQL Docker image with all the extensions installed.

But, there is also another way to install the extensions without going through all the hassles.

## The slumbering PostgreSQL in GitHub hosted runners

At the time of writing, the latest version of GitHub Linux hosted runner ([Ubuntu 22.04](https://github.com/actions/runner-images/blob/2e2fb133890aeb7f2999b36a37ad221d50f15ece/images/linux/Ubuntu2204-Readme.md)) has a PostgreSQL installed in it, but it is disabled by default:

```
PostgreSQL 14.7

User: postgres
PostgreSQL service is disabled by default.
Use the following command as a part of your job to start the service: 'sudo systemctl start postgresql.service'
```

All you have to do is start it according to the instruction.

## Install PostgreSQL extensions

You can install PostgreSQL extensions by using apt. For instance, if you are looking to install `pg_cron` on PostgreSQL 14, run `sudo apt install postgres-14-cron`.

Once installed, you may have to edit the main PostgreSQL configuration file. For PostgreSQL 14, this file is located at `/etc/postgresql/14/main/postgresql.conf`.

Append your custom configuration into the file using the `tee` command:

```
echo "shared_preload_libraries = 'pg_cron'" | sudo -u postgres tee -a "/etc/postgresql/14/main/postgresql.conf" > /dev/null
```

Note that our `tee` command is preceded with `sudo -u postgres`. The PostgreSQL configuration file is owned by the `postgres` user, so we will have to impersonate as the user to edit the file.

After editing the configuration file, restart the PostgreSQL service.

And that's it, you can now use the installed extension in your PostgreSQL database server!

## Addon: Executing SQL scripts stored in your repository in GitHub Actions workflow

If your repository contains SQL scripts, you can execute them by using the `psql` command while impersonating as the `postgres` user:

```
sudo -u postgres psql -f your_script.sql
```

But, you will run into an error:

```
could not change directory to "/home/runner/work/your-repo/your-repo": Permission denied
psql: error: your_script.sql: No such file or directory
```

Since `psql` is executed with the `postgres` user, it lacks the permission to change its working directory to the repository cloned by the `runner` user, which is the user that runs our workflow.

Since it could not change its working directory, naturally it could not see your SQL scripts.

I'm not sure why it could not change its working directory to `/home/runner/work/x/x`. The destination directory already has an execute permission bit set on the "others" permission group".

Regardless, to resolve the issue, we will have to run `psql` as the current host user, instead of the `postgres` user.

We create a new database user together with its database in PostgreSQL. The new database user must match the host user:

```
sudo -u postgres createuser -e -s "$USER" # which should create the "runner" user
createdb
```

Now, you can execute your SQL scripts as the `runner` user:

```
psql -f your_script.sql
```

Since `pg_hba.conf` specifies that all local connections are authenticated using `peer` method, as long as our host user is the same as the database user, we can connect to the database server.

For reference, this is the content of `pg_hba.conf` (after omitting all comments) for the PostgreSQL installed on GitHub hosted runners :

```
local   all             postgres                                peer
local   all             all                                     peer
host    all             all             127.0.0.1/32            scram-sha-256
host    all             all             ::1/128                 scram-sha-256
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            scram-sha-256
host    replication     all             ::1/128                 scram-sha-256
```

## The complete workflow configuration

```
name: My Job

on:
  workflow_dispatch:

jobs:
  my-job:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup PostgreSQL with extensions
        run: |
          sudo apt update
          sudo apt install -y postgresql-14-cron
          sudo -u postgres tee -a "/etc/postgresql/14/main/postgresql.conf" > /dev/null << EOF
          shared_preload_libraries = 'pg_cron'
          cron.database_name = 'dbname'
          EOF
          sudo systemctl start postgresql.service

      - name: Prepare database user
        run: |
          sudo -u postgres createuser -e -s "$USER"
          createdb

      - name: Run script
        run: |
          psql -f your_script.sql
```
