---
layout: post
title: Strategies for scaling your Database
lead: 7 different approaches to handling larger amounts of data
---

Most applications start out with a single Postgres or MySQL docker image acting as the database. This enables developers to quickly
create a POC, and test the idea in the market. A large number of applications created will never reach the threshold where the number of
requests will overwhelm the database. For the few which end up facing this positive problem, different scaling techniques will become a
requirement before users start turning away due to slow response times or even database crashes.

Below I have created a list of 7 different techniques to keep in mind when scaling is required. For most of the applications I create I
start off with a Postgres DB. For this reason, we will often reference PostgreSQL and their documentation below.

## Indexes

The most straight forward way to improve response times for evergrowing tables is to introduce indexes.

Example of creating a B-Tree index:
```sql
CREATE INDEX name ON table USING BTREE (column);
```
[Ref: Create Index](https://www.postgresql.org/docs/current/sql-createindex.html)

Relational databases today offer different [types of indexes](https://www.postgresql.org/docs/17/indexes-types.html). Postgres for example provides the `B-Tree`, `Hash`,
`GiST`, `SP-GiST`, `GIN` and `BRIN`. The index type to use depends on the nature of the data stored in the particular column. Having said that, the most common index type
(and the default in postgres) is the B-Tree. The B-Tree keeps data sorted making it easier to find the record you're looking for. It is particularly effective in range
queries. The worst (and average) time for a B-Tree search is `O(log n)`. Without and index the worst case would be a full table scan, a very time consuming operation with
ever growing tables.

![BTree image from https://en.wikipedia.org/wiki/File:Btree_index.PNG](/assets/posts/png/bree_index.png "B-Tree Example")

### Things to keep in mind

 - On every write operation the index has to be updated. This means that write operations are slowed down with every new index created. Ideally only create indexes on fields used particularly often to filter by.

## Denormalization

Without any prior knowledge of the queries which will be mostly used, it is ideal to normalize the database. However sometimes you will realise that
 some queries are being executed very often. When such queries contain expensive joins, you can either:
  - Add an index to the specific columns you will be joining upon (since indexes help the database quickly locate matching rows)
  - Store redundant data in the table in order to reduce the complexity of queries.

### When to store redundant data

If you have large tables where a large percentage of rows match the join condition, the database might decide a full table scan is more efficient.

Consdier the example below:
![BTree image from https://en.wikipedia.org/wiki/File:Btree_index.PNG](/assets/posts/png/denormalization.png "Denormalization")

Lets' assume thousands of car rides happen every day and our drivers want to load a list of car rides for analytical purposes. To avoid joining tables, we can simply add the driver and customer details which are used for this query and our new query will be a simple filter by an indexed column.

### Things to keep in mind

- Update data carefully. In the example above, if the drivers' phone number is updated, we need to carefully update both the Driver table and Car Ride table.
- Storage requirements can start growing heavily. For this reason this method should only be used when the join is required very often.

## Caching

On other occassions, the same data is accessed very frequently. Therefore to avoid database load, especially during heavy usage, we can introduce a
caching layer, for example by using Redis, or in-application cache. The cached data will have a very low response time, whilst the reduction in load on the database will speed
up response time of other queries as well.

### Things to keep in mind

- Cache invalidation need to be planned and taken care of (either by introducing an event-driven invalidation, or a time-based invalidation). This will
ensure that the user will always see fresh data.


## Partitioning

Partitioning refers to splitting the data inside what is logically one table into separate (smaller) physical pieces.
A table can be partitioned into 2 different ways:
 - Vertical Partitioning: divide the table by column;
 - Horizontal Partitioning: divide the table by row using a specific field (called the partition key);

### Veritcal Partitioning
Vertical Partitioning is not supported out of the box in Postgres, however the behaviour can be mimicked by
normalizing a table further. Sometimes a specific group of fields are retrieved way more often then other
fields. Hence seperating the frequently queried fields into a seperate table can enhance performance.

### Horizontal Partitioning

Postgres offers built-in horizontal [partitioning](https://www.postgresql.org/docs/current/ddl-partitioning.html) methods, including Range,
List and Hash partitioning. Splitting the table by range, for example means that every partition will contain a
range of non-overlapping values of the partition key. For example users can be partitioned by age.

[Example of creating a table partitioned by range.](https://www.postgresql.org/docs/current/ddl-partitioning.html#DDL-PARTITIONING-DECLARATIVE-EXAMPLE):
```
CREATE TABLE user (
    user_id         int not null,
    created         date not null,
    age             int,
    name            varchar(255)
) PARTITION BY RANGE (age);
```

## Sharding
Sharding is a form of horizontal scaling where a server is responsible for one partition of a table. Just like
partitions are split by a specific column (the partition key), sharding normally involves splitting the data by
the shard key. In PostgreSQL sharding is not natively implemented, but extensions like
[Citus](https://www.citusdata.com/getting-started/) make it possible.

## Vertical Scaling

Vertical scaling is the simplest way to scale a database and the most immediate results. It is basically about adding more resources (CPU, Storage, Memory)
to the server so that it is able to handle more load.

- Relatively straightforward to implement since it does not require any changes to the database architecture.

### Side note

- There is often a hard limit to how much this can be applied (either due to financial or physical issues)
- Scaling vertically means there is no redundancy and still 1 single point of failure.

## Other options
### Materialized Views
Sometimes you need to transform your data with complex queries, before you're able to use it to extract reports etc. It may be ideal to precompute
these complex queries and store them in new tables (possibly external to the database itself) for faster access. When using this method, keep in
mind that data has to be refreshed, and when it is, it can be a heavy operation on the database.
