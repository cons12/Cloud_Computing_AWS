##### **The Most Exciting Paper I read so far!!My Favorite Paper so far!!**
##### **Shout out to Sanjay Ghemawat, Howard Gobioff, and Shun-Tak Leung** 

### Goals and moltivation 
1. Constant monitoring, error detection, fault tolerance, and automatic recovery must be integral to the system.
   * observations: component failures are the norm rather than the exception.
2. Design assumptions and parameters such as I/O operation and block sizes have to be revisited.
   * observations: files are huge by traditional standards. Multi-GB files are common.
3. Appending becomes the focus of performance optimization and atomicity guarantees
   * caching data blocks in the client loses its appeal.
   * observations: most files are mutated by appending new data rather than overwriting existing data.
4. Co-designing the applications and the file system API benefits the overall system by increasing our flexibility.

### Design
1. Architecture - a cluster 
    * a single master: maintain all file system metadata 
    * multiple chunk-servers: files are divided into fixed-sized chunks(64MB)
    * accessed by multiple clients: more interactions with chunk-servers to fetch data, only interact with master to fetch metadata; request send to usually the closet chunkservers
2. Metadata - in-memory 
    * namespace, mapping from files to chunks, and the locations of each chunk's replicas 
    * **periodic scanning to garbage-coolec, re-replication in presence of chunkserver failures, and chunk migration for load balancin**
3. Operation log
    * The only persistent record of metadata (where recovery happens)
4. Consistency Model
    * File namespace mutations are atomic.  (Defined + consistent region)
    * Mutilple replicas and re-creation of replicas to gurantee files in case of chunkserver failure
    * Spread the chunks to other racks in case of cluster failure 
    * Data integrity is guranteed thru checksum comparison in the chunkserver
    * Snapshot (copy-on-write)
  
    
    
