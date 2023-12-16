---
title: "InfluxDB v1 Cheat Sheet"
date: 2023-12-16T21:29:00Z
draft: false
image: cover.jpg
---

## Introduction

Sometimes we may want a handy cheat sheet for influxdb v1 on hand rather than searching the document for a long time.

## The Cheat Sheet
Tested in influxDB v1.8

### Connect to database
```sh
influx -host 'host name' -username 'username' -password 'password'
```

### Queries
```sql
### Use a database
USE <DATABASES_NAME>

### Show all measurements
SHOW MEASUREMENTS

### Show all tag values of measurements with key "entity_id"
SHOW TAG VALUES WITH KEY = "entity_id"

### Show tag keys
SHOW TAG KEYS

### Show tag keys of a measurement with name "state"
SHOW TAG KEYS WHERE "name" = "state"

### Show measurements where "entity_id" tag is 'last_boot'
SHOW MEASUREMENTS WHERE "entity_id" = 'last_boot'

### Show measurements that start with 'M'
SHOW MEASUREMENTS WITH MEASUREMENT =~ /M.*/

### Show estimated cardinality of measurement set on the current database
SHOW MEASUREMENT CARDINALITY

### Show exact cardinality of measurement set on a specified database
SHOW MEASUREMENT EXACT CARDINALITY ON <DATABASES_NAME>

### Show all running queries
SHOW QUERIES

### Show all retention policies on a database
SHOW RETENTION POLICIES ON "<DATABASES_NAME>"

### Show all users in InfluxDB
SHOW USERS

### Show all series
SHOW SERIES

### Show series from a measurement with a specific tag
SHOW SERIES FROM "state" WHERE entity_id = 'last_boot'

### Show estimated cardinality of the series on current database
SHOW SERIES CARDINALITY

### Show estimated cardinality of the series on specified database
SHOW SERIES CARDINALITY ON <DATABASES_NAME>

### Show all tag keys
SHOW TAG KEYS

### Show all tag keys from the `state` measurement
SHOW TAG KEYS FROM "state"

### Show all tag keys from the `state` measurement where the `entity_id` key is with value `last_boot`
SHOW TAG KEYS FROM "state" WHERE entity_id = 'last_boot'

### Show all tag keys where the `entity_id` key is with value `last_boot`
SHOW TAG KEYS WHERE entity_id = 'last_boot'

### Show tag values across all measurements for all tag keys that do not include the letter c
SHOW TAG VALUES WITH KEY !~ /.*c.*/

### Show tag values from the `state` measurement for `domain` and `entity_id` tag keys where the value of `entity_id` is `last_boot`
SHOW TAG VALUES FROM "state" WITH KEY IN ("domain", "entity_id") WHERE "entity_id" = 'last_boot'

### Explain the plan behind the query
EXPLAIN SELECT * FROM "state"

### Explain the query performance and storage during runtime, visualized as a tree
EXPLAIN ANALYZE SELECT * FROM "state"
```

## References

- https://docs.influxdata.com/influxdb/v1/query_language/spec/#query-engine-internals
- https://docs.influxdata.com/influxdb/v1/query_language/explore-schema/#show-tag-values
- https://songrgg.github.io/operation/influxdb-command-cheatsheet/

## Credits

Cover photo by [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/open-book-lot-Oaqk7qqNh_c?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

  