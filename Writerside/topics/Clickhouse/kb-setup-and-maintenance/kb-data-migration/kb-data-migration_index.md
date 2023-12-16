#Data Migration
linkTitle: "Data Migration"
description: >
    Data Migration
---
## Export & Import into common data formats

Pros:
* Data can be inserted into any DBMS.

Cons:
* Decoding & encoding of common data formats may be slower / require more CPU
* The data size is usually bigger than ClickHouse formats.
* Some of the common data formats have limitations.


The best approach to do that is using clickhouse-client, in that case, encoding/decoding of format happens client-side, while client and server speak clickhouse Native format (columnar & compressed).

In contrast: when you use HTTP protocol, the server do encoding/decoding and more data is passed between client and server.


## remote/remoteSecure or cluster/Distributed table

Pros:
* Simple to run.
* It’s possible to change the schema and distribution of data between shards.
* It’s possible to copy only some subset of data.
* Needs only access to ClickHouse TCP port.

Cons:
* Uses CPU / RAM (mostly on the receiver side)

See details of both approaches in:

[remote-table-function.md]({{<ref "remote-table-function.md" >}})

[distributed-table-cluster.md]({{<ref "distributed-table-cluster.md" >}})

## clickhouse-copier

Pros:
* Possible to do **some** changes in schema.
* Needs only access to ClickHouse TCP port.
* It’s possible to change the distribution of data between shards.
* Suitable for large clusters: many clickhouse-copier can execute the same task together.

Cons:
* May create an inconsistent result if source cluster data is changing during the process.
* Hard to setup.
* Requires zookeeper.
* Uses CPU / RAM (mostly on the clickhouse-copier and receiver side)


Internally it works like smart `INSERT INTO cluster(…) SELECT * FROM ...` with some consistency checks.



Run clickhouse copier on the same nodes as receiver clickhouse, to avoid doubling the network load.


See details in:

[kb-clickhouse-copier]({{<ref "kb-clickhouse-copier" >}})

## Manual parts moving: freeze / rsync / attach

Pros:
* Low CPU / RAM usage.

Cons:
* Table schema should be the same.
* A lot of manual operations/scripting.


With some additional care and scripting, it’s possible to do cheap re-sharding on parts level.


See details in:

[rsync.md]({{<ref "rsync.md" >}})

## clickhouse-backup

Pros:
* Low CPU / RAM usage.
* Suitable to recover both schema & data for all tables at once.

Cons:
* Table schema should be the same.

Just create the backup on server 1, upload it to server 2, and restore the backup.

See [https://github.com/AlexAkulov/clickhouse-backup](https://github.com/AlexAkulov/clickhouse-backup)

[introduction-to-clickhouse-backups-and-clickhouse-backup](introduction-to-clickhouse-backups-and-clickhouse-backup.md)

## Fetch from zookeeper path

Pros:
* Low CPU / RAM usage.

Cons:
* Table schema should be the same.
* Works only when the source and the destination clickhouse servers share the same zookeeper (without chroot)
* Needs to access zookeeper and ClickHouse replication ports: (`interserver_http_port` or `interserver_https_port`)

```
ALTER TABLE table_name FETCH PARTITION partition_expr FROM 'path-in-zookeeper'
```
[alter table fetch detail]({{<ref "fetch_alter_table" >}})

## Replication protocol

Just make one more replica in another place.

Pros:
* Simple to setup
* Data is consistent all the time automatically.
* Low CPU and network usage.

Cons:
* Needs to reach both zookeeper client (2181) and ClickHouse replication ports: (`interserver_http_port` or `interserver_https_port`)
* In case of cluster migration, zookeeper need’s to be migrated too.
* Replication works both ways.

[kb-zookeeper-cluster-migration.md](kb-zookeeper-cluster-migration.md)

## See also

### Github issues

[https://github.com/ClickHouse/ClickHouse/issues/10943](https://github.com/ClickHouse/ClickHouse/issues/10943)
[https://github.com/ClickHouse/ClickHouse/issues/20219](https://github.com/ClickHouse/ClickHouse/issues/20219)
[https://github.com/ClickHouse/ClickHouse/pull/17871](https://github.com/ClickHouse/ClickHouse/pull/17871)

### Other links

[https://habr.com/ru/company/avito/blog/500678/](https://habr.com/ru/company/avito/blog/500678/)
