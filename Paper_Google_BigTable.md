### Goal 
* wide applicability, scalability, high performance, and high availability 
* Provide clients with a simple data model that supports dynamic control over data layout and format, and allows clients to reason about the **locality properties** of the data represented in the **underlying storage**.

### Data Model
* A multi-dimentional sorted map, indexed by (row, column, timestamp)
* Feature: 
    * **atomic** change under row key for read/write 
    * **persistency** each cell allows multiple versions of value(contens) through time-stamp
    * **economy** saving space by auto garbage collection of previous self-defined n versions of contents in the table

### Dependencies 
* Google GFS -> Needless to say
* Google SSTable -> internally store BigTable Data 
    * Block index -> locate the contents
    * Feature: single disk-seek for look-up 
* Chubby -> distributed lock service 

### Implementations
1. Library linked to each client, one master server, many tablets servers 
    * Clients directly communicate with table servers for read/write 
    * Light weight master
2. Hierarchy tablet location design 
    1. Chubby -> location of root tablet 
    2. root tablet -> location of METADATA table 
    3. METADATA table -> location of tablets
    (so location of usertable is not stored/transmitted thru master server)
3. Chubby keeps track of tablet servers 
    * In Chubby dir, create and acquires a lock on a file which has the same name as the tablet server
    * (thoughts: similar to the proc implementation to monitor currently running processes in Linux)
4. Compactions
    * When over threshhold, store some SSTable to GFS 
5. Refinement - features 
    * Compression: Two pass compression schemes 
    * Caching for read performance thru Block Cache + Scan Cache
    * Bloom filters: improve read speed by decreasing disk access e.g lookups for non-existent rows don't need to touch disks
    * CommitLog 
        * issue1: a single humongous log file causes redundant/duplicating read during recovery
        * solution: sorting commit entries together using the key <table, row name, log seq num>
        * issue2 : performance bottleneck caused by GFS e.g. network/GFS bugs/crushes
        * solution: two log writing threads; only one active each time; include seq number to avoid duplicated entries 
6. Applications: Google Analytics, Google Earth, Personalized Search 

### Take-home messages/ Lessons Learned  
1. Distributed systems are vulnerable to many types of failures -> be resistent/ be aware
2. Delay adding new features in until it's clear
3. System-level monitorining
4. **Simple designs**

     
