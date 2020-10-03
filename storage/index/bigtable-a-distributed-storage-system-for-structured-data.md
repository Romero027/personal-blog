---
description: >-
  https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf
---

# Bigtable: A Distributed Storage System for Structured Data

### TL;DR: 

BigTable is a sparse, distributed, persistent, multi-dimensional\[1\], sorted map.

### Summary:

Problem Statement: GFS provides us a reliable, scalable distributed file storage, but it does not provide any facility for structuring the data contained in the files beyond a hierarchical directory structure. Thus, the authors built BigTable and wrote it up.

BigTable is most useful if 1. There is no need to join each data table with another 2. Data need to be updated. 3. Range queries are common.

In the Bigtable data model, data is grouped in tables. Each record in one table must have a string identifier unique in that table. Each table is defined to have a set of “column families”. Columns are grouped into sets called column families, and a qualifier is used to distinguish between two members of a column family. A column is identified by a family:qualifier pair. A record has values, that is the point of having a record. Each of those values is associated with a timestamp when it was created.

BigTable provides an API to application developers that allows the typical operations you might expect; creation and deletion of tables and column families, writing data and deleting columns from a row. Transactions are supported at the row level, but not across several row keys \(just like PNUTS\). However, write batching is implemented to improve throughput.

BigTable is built on top of GFS, Chubby lock service\[2\] and SSTable. Bigtable data is stored in the SSTable file format. An SSTable provides a persistent ordered immutable map from keys to values, where both keys and values are arbitrary byte strings.

BigTables are split into lexicographically ordered\(by row key\) chunks called tablets. BigTable tablet servers each maintain responsibility for a number of tablets. We don't need to explicitly replicate tablet servers because every update to a tablet is reflected in GFS. Tablet servers keep the most recent updates to a tablet in an in-memory representation called a memtable.

Tablets are located via a three-tier hierarchical lookup mechanism. The location of a root tablet, which contains metadata for a BigTable instance, is located in Chubby and can be read from the filesystem there. The root tablet contains the locations of other metadata tablets, which themselves point to the location of data tablets. Together the root and metadata tablets form the METADATA table, which is itself a BigTable table. Each row in the METADATA table maps the pair \(table name, last row\) to a location for a tablet whose last row is as given.

The master is responsible for assigning tablets to tablet servers, detecting the addition and expiration of tablet servers, balancing tablet-server load, and garbage collection of files in GFS.

### Lesson Learned: 

1. Delay adding new features until it is clear how the new features will be used 2. The importance of proper system-level monitoring 3. The value of simple designs…code and design clarity are of immense help in code maintenance and debugging

\[1\] "Multi-dimensional refers to the fact that the index used to lookup data may be comprised of several dimensions. A simple key-&gt;value lookup is one-dimensional. \(row, column\)-&gt;value is two-dimensional. BigTables may be multi-dimensional as the index maps from \(row, column, time, column qualifier\) onto a value. In particular the idea that you can index values by time is novel - so you can retrieve the previous versions of a value by timestamp if you like. This implies that no data are ever deleted from a BigTable, and this is indeed the case. Once a value is written, it’s written for life, uniquely identified by its timestamp. Well - almost. Applications may specify to BigTable that only the most recent n or only versions that were written recently should be kept, and BigTable is free to garbage collect those that are not." 

\[2\] BigTable uses Chubby for a variety of tasks: 1. To ensure there is at most one active master at any time. 2. To store the bootstrap location of BigTable data\(root tablet\). 3. To discover BigTable servers and finalize tablet server death. 4. To store ACLs. Thus, Chubby unavailable = BigTable unavailable.

### Reference: 

[https://www.the-paper-trail.org/post/2008-10-29-bigtable-googles-distributed-data-store/](https://www.the-paper-trail.org/post/2008-10-29-bigtable-googles-distributed-data-store/) [http://ethanp.github.io/blog/2016/04/10/bigtable-paper-summary/](http://ethanp.github.io/blog/2016/04/10/bigtable-paper-summary/)

  






