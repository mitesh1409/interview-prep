# Databases Interview Questions

## Index

1. [Write a query](./Q-1.md)

---

Backlog items  

## Database Scaling

Database Scaling can be **Vertical** (upgrading hardware) or **Horizontal** (adding more machines).  

Horizontal scaling is achieved through **sharding** (splitting data) and **replication** (copying data).  

Sharding  
This means distributing different portions/shards of the data set across multiple servers.  
This means you split the data into smaller chunks and distribute it across multiple servers.  

Some of the Sharding strategies are:  

* Range-based Sharding - Based on the range of a given key.
* Directory-based Sharding - Lookup service to direct traffic to the database.
* Geographical Sharding - Based on geographic location.

Replication  
This means keeping copies of data on multiple servers for high availability.  

Some of the Replication strategies are:  

* Master-Slave Replication - where you have one Master database (read + write) and several Slave databases (read only).
* Master-Master Replication - where both Master databases have read and write capabilities.

---

## Database Performance

There are different performance techniques that can help to access your data faster.  

Caching  
Caching is not just for web servers, database caching can be done through in-memory databases  
like Redis.  
You can use it to cache frequent queries and boost your performance.  

Indexing  
Indexing is another way to boost the performance of your database.
Creating an index for frequently accessed column will significantly speed up retrieval times.  

Query Optimization  
Consider optimizing queries for fast data access,  
this includes minimizing joins and using tools like  
SQL query analyzer or explain plan to understand your  
query's performance.  
