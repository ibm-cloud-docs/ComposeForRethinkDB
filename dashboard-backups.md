---

copyright:
  years: 2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Backups
{: #backups}

You can create and download backups from the *Manage* page of your service dashboard. Both scheduled and manual backups are available.

## Viewing existing backups

Daily backups of your database are automatically scheduled. To view your existing backups, navigate to the *Manage* page of your service dashboard. 

![Backups](./images/rethink-backups-show.png "A list of backups in the service dashboard")

## Creating a backup on demand

As well as scheduled backups you can create a backup manually. To create a manual backup, navigate to the *Manage* page of your service dashboard and click *Backup now*.

## Downloading a backup

To download a backup, navigate to the *Manage* page of your service dashboard and click *Download* in the corresponding row for the backup you wish to download.

## Backup contents

RethinkDB backups use the `dump` command from the RethinkDB command-line utility on your running database cluster to backup your entire deployment. It saves database and table contents as well as metadata. `dump` does use some cluster resources, but will not lock-out your clients and can be run on a live cluster. Compose provides backups for RethinkDB deployments that are in a format that `rethinkdb restore` can use directly.

## Using a backup with a local database

Since your RethinkDB backups are available for you to download, you can get a local instance of your deployment up and running.

1. Install [rethink](https://www.rethinkdb.com/docs/install/)
2. Install the [python driver](https://www.rethinkdb.com/docs/install-drivers/python/) in your path.
3. Download your compressed backup file. You do not need to unpack the backup archive file, the RethinkDB tools know how to handle it.
4. To spin up RethinkDB, run the `rethinkdb` command in one terminal window, and in a separate terminal window navigate to the location of your downloaded backup and run `rethinkdb restore backup.tar.gz`.

Open a browser window and navigate to `locahost:8080` to see the RethinkDB UI and your data.

## Bringing a Local Backup into your service

If you have a backup file locally that you would like to restore to {{site.data.keyword.composeForRethinkDB}} you can do this using `rethinkdb restore`.

1. Install [rethink](https://www.rethinkdb.com/docs/install/)
2. Install the [python driver](https://www.rethinkdb.com/docs/install-drivers/python/) in your path.
3. Download the certificate from the *Overview* page of your service and save it locally as compose.cert.
4. Restore from the backup using the following command:

  ```
  rethinkdb restore -c <host>:<port> --tls-cert compose.cert -p backup.tar.gz
  ```

The host and port values can be found in your connection string, which you'll find on the *Overview* page of your service. The `-p` in the command will prompt for the _Authentication Credential_.

**Note:** If you are restoring into an existing deployment you might have to use `--force` to overwrite existing tables.