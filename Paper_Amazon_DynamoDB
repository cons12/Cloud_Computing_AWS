###Goal
* A highly available key-value storage system provides "always-on" experience 
* Due to Amazon's company mission, it requires the storage technology to be always available 
* Availability >>>>>> throughput/latency >>>>>> availability 

###System Assumptions and Requirements 
1. Query Model: 
  * Observations: most prev queries only primary key lookup 
  * Improvements: 
      * no operations span multiple data items and there is no need for relationial schema 
      * targets applications store small objects 
2. ACID properties:
  * Availability > ACID (Atomicity, consistency, isolation, durability)
3. Efficiency:
  * High requirement for lationcy and throughput 
4. Security:
  * Dynamo is designed for internal usages, so assuming non-hostile environment
  * Also friendly to scale out
5. Service Level Agreement (SLA)
  * Measured at the 99.9 percentile of the distribution instead of avaerage to accomodate longer searching time for consumers with more search history stored 
  
  
