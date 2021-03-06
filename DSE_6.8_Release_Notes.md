# Release notes for DataStax Enterprise
DSE 6.8.x is compatible with Apache Cassandra&trade; 3.11 and adds additional production-certified changes, if any. Components that are indicated with an asterisk (*) (if any) are known to be updated since the prior patch version.

# Release notes for DSE 6.8.9
7 January 2021

## Component versions for DSE 6.8.9

   * Apache Solr™ 6.0.1.4.2814
   * Apache Spark™ 2.4.0.16
   * Apache TinkerPop™ 3.4.5-20200107-6cec00d8
   * Apache Tomcat® 8.0.53
   * DSE Java Driver 1.10.0-dse+20200217
   * Netty 4.1.25.7.dse
   * Spark JobServer 0.8.0.49

## DSE 6.8.9 Streaming

* Addresses a severe issue where streaming an older file format to a new node could crash sending and receiving nodes. (DB-4846)


## DSE 6.8.9 Core

* Add support for multiple authorization sources (LDAP + DSE Internal) (DSP-14233)


## DSE 6.8.9 SparkConnector

* Fixed direct join optimization for spark sql. (DSP-21498)


# Release notes for DSE 6.8.8
15 December 2020

:warning: **NOTE**: Due to a serious bug which affects DSE `6.8.7` and DSE `6.8.8`, these releases have been retracted.  We recommend against upgrading to these versions at this time.  If you have already upgraded to these versions, please _EITHER_ set `zerocopy_streaming_enabled=false` in the `cassandra.yaml` and perform a rolling restart AND/OR run `upgradesstables` on all nodes in your cluster before adding new nodes, running repair, or restoring from backups.  This bug has been addressed in DSE `6.8.9`. All features and fixes for 6.8.8 and 6.8.7 are present in 6.8.9.

## Components versions for DSE 6.8.8

   * Apache Solr™ 6.0.1.4.2814*
   * Apache Spark™ 2.4.0.16
   * Apache TinkerPop™ 3.4.5-20200107-6cec00d8
   * Apache Tomcat® 8.0.53
   * DSE Java Driver 1.10.0-dse+20200217
   * Netty 4.1.25.7.dse
   * Spark JobServer 0.8.0.49

## DSE 6.8.8 Backup and Restore

* During Backup Service startup, to avoid stalling during service initialization, check cluster readiness in response to endpoint "on alive" events. (DB-4818)

## DSE 6.8.8 Management API

* Fixed an issue in DSE that prevented the Management API to work with DSE versions 6.8.5 - 6.8.7 (DSP-21607)


## DSE 6.8.8 Indexing

* The issue: When flushing text column indexes, the internal ordering of terms can be processed out of order causing an "*java.lang.AssertionError: Incremental trie requires sorted keys*" error. When this happens, all flushing of indexes involved in this transaction is aborted and the indexes are marked non-queryable. Recovering from this issue involves either rebuilding the indexes or restarting the nodes. (DSP-21580)
* Fixes a performance regression in SAI for versions 6.8.6 and 6.8.7 regarding *MultiRangeReadCommand*. (DSP-21601)


## DSE 6.8.8 Search

* A system property `dse.solr.fuzzy.max.expansion` was added which allows the user to define a custom number of fuzzy query expansions. The maximal possible value is 1024. When unset, the default number of max expansions is 50. (DSP-21605)


## DSE 6.8.8 Spark

* Adjust available framework values for `--framework` parameter. (DSP-21500) 

# Release notes for DSE 6.8.7
23 November 2020

:warning: **NOTE**: Due to a serious bug which affects DSE `6.8.7` and DSE `6.8.8`, these releases have been retracted.  We recommend against upgrading to these versions at this time.  If you have already upgraded to these versions, please _EITHER_ set `zerocopy_streaming_enabled=false` in the `cassandra.yaml` and perform a rolling restart AND/OR run `upgradesstables` on all nodes in your cluster before adding new nodes, running repair, or restoring from backups.  This bug has been addressed in DSE `6.8.9`. All features and fixes for 6.8.8 and 6.8.7 are present in 6.8.9.

## Components versions for DSE 6.8.7

   * Apache Solr™ 6.0.1.4.2794
   * Apache Spark™ 2.4.0.16
   * Apache TinkerPop™ 3.4.5-20200107-6cec00d8
   * Apache Tomcat® 8.0.53
   * DSE Java Driver 1.10.0-dse+20200217
   * Netty 4.1.25.7.dse
   * Spark JobServer 0.8.0.49

## DSE 6.8.7 Cassandra

* Fixed a bug where the slow query log would fill with queries that do not meet the slow query threshold (DSP-21417)


## DSE 6.8.7 Core

* Fixed a bug where a single partition read might fail if the following conditions were true:
    1) several sstables had the same partition level deletion info 
    2) some of the sstables had wide rows whereas others had not 
    3) the sstables in question contained range tombstone markers (DSP-21346)
* Fixed a bug in `cassandra.repair.mutation_repair_rows_per_batch` setting that caused sending all repair mutations at once (DSP-21429)


## DSE 6.8.7 Indexing

* On the CQLSH `CREATE CUSTOM INDEX ... WITH OPTIONS` statement, SAI adds support for an ascii option. If set to `true`, converts alphabetic, numeric, and symbolic characters that are not in the Basic Latin Unicode block (first 127 ASCII characters) to their ASCII equivalent, if one exists. For example, the filter changes à to a. The default is `false`. (DSP-21409)
* Make the SAI read path synchronous. (DSP-21451)
* Fixed "java.lang.ArithmeticException: integer overflow" printing in `system.log` when retrieving the SAI index `segmentRowID` (DSP-21522)


## DSE 6.8.7 Graph

* A meaningful error message is logged when two properties with the same name but different types are used in a single core graph. Classic graph was not affected. (DSP-21490)


## DSE 6.8.7 Security

* Optimized retrieval when `memberof_search` used the wrong attribute to retrieve groups of the user. (DSP-21537)


## DSE 6.8.7 Backup and Restore

* Multi-datacenter backup and restore, new `CompositeStore` type of backup store. (DB-4489)
* Adds the possibility to restore a backup marked as `INCOMPLETE` by using the new `FORCE RESTORE` statement.


## DSE 6.8.7 CommitLog

* Addressed a bug where a "CommitLogReplayException" is caused by a bad header but correct CRC after restart (DB-3996)
* Fixed a bug where some part of the commit log might not be replayed after injecting a foreign sstable to a node or, on 6.8, after zero-copy streaming of an sstable (DB-4629)


## DSE 6.8.7 Streaming

* Fixed an issue where zero copy streaming could cause file descriptor leakage (DB-4594)


## DSE 6.8.7 Tools

* SStableloader now uses `native_transport_port_ssl` over `native_transport_port` when passed a config file with the property set (DB-4632)

## DSE 6.8.7 TPC

* Fixed memory leak in Netty resulting in OOM. (DB-4664)
* Fixed a problem in the scheduling and counting of active materialized view updates that could cause too many to be executed concurrently, overwhelming the node. (DB-4782)



# Release notes for DataStax Enterprise version 6.8.6
12 November 2020

## Components versions for DSE 6.8.6

   * Apache Solr™ 6.0.1.4.2794
   * Apache Spark™ 2.4.0.16
   * Apache TinkerPop™ 3.4.5-20200107-6cec00d8
   * Apache Tomcat® 8.0.53
   * DSE Java Driver 1.10.0-dse+20200217
   * Netty 4.1.25.7.dse
   * Spark JobServer 0.8.0.49

## DSE 6.8.6 Bootstrap

* A node may be stuck in repair while joining the cluster if broadcast_address is set differently than local_address (DB-4786)


# Release notes for DataStax Enterprise version 6.8.5
20 October 2020

   * Apache Solr™ 6.0.1.4.2794
   * Apache Spark™ 2.4.0.16
   * Apache TinkerPop™ 3.4.5-20200107-6cec00d8
   * Apache Tomcat® 8.0.53
   * DSE Java Driver 1.10.0-dse+20200217
   * Netty 4.1.25.7.dse
   * Spark JobServer 0.8.0.49

## DSE 6.8.5 Backup and Restore

* Server side backup and restore now supports Microsoft Azure cloud storage as a backup target. (DB-3894)
* snapshot `schema.cql` files will now contain `IF NOT EXISTS` clause for `CREATE TYPE` statements (DB-4685)


## DSE 6.8.5 Compaction

* Fixes a problem where races in notifying compaction strategies of added and removed sstables can cause compaction to try to use non-existing sstables and repeatedly fail to make progress. (DB-4711)


## DSE 6.8.5 Core

* Fixed node restart issue after dropping a PointType column. (DSP-21326)
* Fixed extreme local pauses on all nodes in the cluster on a node restart. (DB-4657)


## DSE 6.8.5 Local Write-Read Paths

* Improves performance of estimation of partition counts for subranges. (DB-3679)


## DSE 6.8.5 Virtual Tables

* Fsync nodes metadata to prevent FSReadError issues on startup. (DB-4672)

## DSE 6.8.5 Cassandra

* Fixes LDAP user permissions problem following LDAP server restart. (DSP-21284)


## DSE 6.8.5 Security

* Fixes LDAP user permissions problem following LDAP server restart. (DSP-21284)


## DSE 6.8.5 Graph

* No text in Doc Notes field. (DSP-21450)

## DSE 6.8.5 Spark

* Fix: Spark Application contacting Nodes in Non Local DC  (DSP-19961)

## TinkerPop changes for DSE 6.8.5

DataStax Enterprise (DSE) 6.8.4 includes all changes from previous DSE versions. See TinkerPop [upgrade documentation](http://tinkerpop.apache.org/docs/3.4.5/upgrade/#_upgrading_for_users) for all changes.



# Release notes for DataStax Enterprise version 6.8.4
17 September 2020

## Component versions for DSE 6.8.4

   * Apache Solr™ 6.0.1.4.2794
   * Apache Spark™ 2.4.0.16
   * Apache TinkerPop™ 3.4.5-20200107-6cec00d8
   * Apache Tomcat® 8.0.53
   * DSE Java Driver 1.10.0-dse+20200217
   * Netty 4.1.25.7.dse
   * Spark JobServer 0.8.0.49

## DSE 6.8.4 Compaction

* Fixes compaction getting stuck on acquiring references for non-existing sstables. (DB-4290)


## DSE 6.8.4 CQL

* Distributes Netty connections more uniformly across TPC cores (DB-4683)


## DSE 6.8.4 MessagingService

* Distributes Netty connections more uniformly across TPC cores (DB-4683)


## DSE 6.8.4 Core

* Adds TTL and TimeWindowCompactionStrategy (TWCS) to `system_distributed.repair_history` and `system_distributed.parent_repair_history` tables.  (DB-2009)
* DNS Service Discovery is now a part of the DSE/LDAP integration. (DSP-11450)
* New system property to cap the maximum amount of memory used by bloom filters: `-Dcassandra.max_bf_memory_mb}`. By default, this is _unlimited_. (DSP-21344)


## DSE 6.8.4 Security

* DNS Service Discovery is now a part of the DSE/LDAP integration. (DSP-11450)


## DSE 6.8.4 DSEFS

*  DSEFS waits for a schema agreement before starting and issuing the first CQL query. (DSP-20743)


## DSE 6.8.4 Indexing

* Storage-Attached Indexing (SAI) adds support for creating multiple SAI indexes on the same collection map column. 
See [SAI collection map examples with keys, values, and entries](https://docs.datastax.com/en/storage-attached-index/6.8/sai/saiUsing.html#saiUsing__saiUsingCollectionsExamples). (DSP-21306)


## TinkerPop changes for DSE 6.8.4

DataStax Enterprise (DSE) 6.8.4 includes all changes from previous DSE versions. See TinkerPop [upgrade documentation](http://tinkerpop.apache.org/docs/3.4.5/upgrade/#_upgrading_for_users) for all changes.


## Release notes for previous versions
Release notes for previous DSE patch releases can be found here:
https://docs.datastax.com/en/dse/6.8/dse-admin/datastax_enterprise/releaseNotes/RNdse.html#RNdse
